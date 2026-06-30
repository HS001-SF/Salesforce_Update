# Salesforce FSL Polygon Based Resource Assignment Functionality Report

## Objective

Implement custom Salesforce Field Service (FSL) functionality to
identify the correct Map Polygon based on Service Appointment
coordinates and assign the appropriate Service Resource.

## Business Scenario

When a Service Appointment status changes to **Scheduled**, the system
should:

1.  Read Service Appointment Latitude and Longitude.
2.  Identify the matching FSL Map Polygon using the polygon coordinates
    stored in `FSL__KML__c`.
3.  Update the Service Territory reference on the matched Map Polygon
    record.
4.  Use the identified Service Territory to find related Service
    Territory Members.
5.  Identify the Service Resource associated with the Service Territory
    Member.
6.  Create an Assigned Resource record to assign the Service Resource to
    the Service Appointment.

## Functional Flow

    Service Appointment
            |
            | Status = Scheduled
            |
            v
    Get Appointment Latitude / Longitude
            |
            v
    Read Map Polygon Records
            |
            v
    Parse FSL__KML__c Coordinates
            |
            v
    Point-In-Polygon Validation
            |
            v
    Matched Map Polygon
            |
            v
    Update Map Polygon Service Territory
            |
            v
    Find Service Territory Members
            |
            v
    Get Service Resource
            |
            v
    Create Assigned Resource
            |
            v
    Service Appointment Assigned

## Technical Implementation

### Trigger

A trigger runs on Service Appointment update.

Condition:

-   Previous Status != Scheduled
-   New Status = Scheduled

Purpose:

-   Initiate polygon identification and resource assignment process.

## Polygon Identification Logic

The system:

-   Retrieves Map Polygon records.
-   Reads the `FSL__KML__c` field.
-   Extracts latitude and longitude points from KML coordinates.
-   Uses point-in-polygon algorithm to verify whether the Service
    Appointment location falls inside the polygon boundary.

## Resource Assignment Logic

After identifying the polygon:

-   Find related Service Territory.
-   Query Service Territory Members.
-   Get Service Resource lookup.
-   Create Assigned Resource junction record.

Relationship:

    Service Appointment
            |
            |
    Assigned Resource
            |
            |
    Service Resource

## Key Objects Used

  Object                   Purpose
  ------------------------ ----------------------------------------
  ServiceAppointment       Stores appointment details and status
  MapPolygon               Stores polygon boundaries and KML data
  ServiceTerritory         Defines service area
  ServiceTerritoryMember   Maps resources to territories
  ServiceResource          Technician/resource information
  AssignedResource         Assigns resource to appointment

## Error Handling Considerations

-   Ignore polygons where `FSL__KML__c` is empty.
-   Skip appointments without Latitude/Longitude.
-   Handle cases where no polygon matches.
-   Handle cases where no Service Territory Member exists.

## Expected Result

Example:

Service Appointment:

    Latitude  : 30.70232
    Longitude : 76.70441
    Status    : Scheduled

System Result:

    Matched Map Polygon
            |
            v
    Service Territory
            |
            v
    Service Territory Member
            |
            v
    Service Resource Assigned

The Service Appointment is automatically assigned to the correct
technician based on geographic location.
