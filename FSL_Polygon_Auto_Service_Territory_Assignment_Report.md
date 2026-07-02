# FSL Polygon Auto Service Territory Assignment - Development Report

## Objective

Automatically populate the Service Territory on a newly created Map
Polygon based on the Service Territory Members located within the
polygon.

## Business Requirement

When a Map Polygon is created, the \*\*FSL\_\_Service_Territory\_\_c\*\*
field is blank. The system should automatically determine the correct
Service Territory using the coordinates of Service Territory Members.

## Functional Flow

1.  Create a new Map Polygon.
2.  Fire an After Insert trigger.
3.  Read polygon KML from \*\*FSL\_\_KML\_\_c\*\*.
4.  Parse the polygon coordinates.
5.  Query Service Territory Members with valid Latitude and Longitude.
6.  Check whether each STM lies inside the polygon.
7.  Count matching STMs by Service Territory.
8.  Select the Service Territory with the highest count.
9.  Update \*\*FSL\_\_Service_Territory\_\_c\*\* on the polygon.

## Technical Components

### Trigger

-   Object: FSL\_\_Polygon\_\_c
-   Event: After Insert

### Apex Class

**PolygonServiceTerritoryHandler** - Read KML - Parse coordinates -
Perform point-in-polygon validation - Determine Service Territory -
Update polygon

## Business Rules

-   Ignore polygons with blank KML.
-   Ignore STMs with missing Latitude or Longitude.
-   Assign the Service Territory with the highest STM count.
-   Leave the field blank if no matching STM is found.

## Reused Components

The implementation reuses the following methods from the existing
**MapPolygon** class: - parseKML() - isInsidePolygon()

## Benefits

-   Automatic Service Territory assignment
-   Eliminates manual updates
-   Reuses existing polygon logic
-   Bulkified and scalable
-   Easy to maintain

## Process Flow

``` text
Create Polygon
      │
      ▼
After Insert Trigger
      │
      ▼
Read KML
      │
      ▼
Query Service Territory Members
      │
      ▼
Point-in-Polygon Validation
      │
      ▼
Count Matching Territories
      │
      ▼
Select Highest Count
      │
      ▼
Update Polygon Service Territory
```
