# Salesforce FSL Map Polygon Functionality Gap Analysis Report

## Objective

Document the limitation of standard Salesforce Field Service (FSL) Map
Polygon functionality and the requirement for custom development to
achieve resource-level polygon assignment.

Salesforce FSL Map Polygons are designed to define geographic boundaries
for Service Territories. They can be created using map drawing or
imported using KML and linked with Service Territories.

## Current Salesforce FSL Standard Capability

Salesforce FSL supports:

-   Creating geographic polygons.
-   Importing KML-based polygon boundaries.
-   Linking polygons with Service Territories.
-   Assigning Service Appointments to territories based on polygon
    location.

Standard flow:

    Service Appointment Location
              |
              |
    Geolocation Match
              |
              |
    Map Polygon
              |
              |
    Service Territory

## Current Limitation

### Technician/Resource Level Polygon Mapping Not Available

Standard FSL Map Polygon functionality works at the Service Territory
level, not directly at the individual Service Resource level.

Supported model:

    Map Polygon
          |
          |
    Service Territory
          |
          |
    Service Territory Members
          |
          |
    Service Resources

Required business model:

    Map Polygon A
          |
          |
    Resource A


    Map Polygon B
          |
          |
    Resource B

The direct relationship between a polygon boundary and a specific
technician/resource is not available out of the box.

## Business Requirement

When a Service Appointment is scheduled:

1.  Read Service Appointment Latitude and Longitude.
2.  Identify the matching Map Polygon using KML coordinates.
3.  Validate whether appointment coordinates exist inside the polygon
    boundary.
4.  Identify the related Service Territory.
5.  Find Service Territory Members.
6.  Identify the Service Resource.
7.  Assign the resource to the Service Appointment.

## Custom Solution Required

Custom Apex logic is required for:

-   Reading Map Polygon KML coordinates.
-   Performing point-in-polygon validation.
-   Finding matching geographic areas.
-   Selecting eligible Service Resources.
-   Creating Assigned Resource records.

Flow:

    Service Appointment
            |
            |
    Latitude / Longitude
            |
            |
    Map Polygon KML
            |
            |
    Point-In-Polygon Logic
            |
            |
    Matched Polygon
            |
            |
    Service Territory
            |
            |
    Service Territory Member
            |
            |
    Service Resource
            |
            |
    Assigned Resource

## Expected Outcome

The custom functionality provides:

-   Technician-level polygon assignment.
-   Automatic resource selection based on geographic location.
-   Polygon-based candidate filtering.
-   Improved scheduling accuracy.
-   Reduced manual dispatcher intervention.

## Conclusion

Salesforce FSL Map Polygon functionality supports geographic territory
assignment but does not provide native technician-level polygon mapping.
Custom Apex development is required to extend standard FSL capability
and assign resources based on individual polygon coverage areas.
