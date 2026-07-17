# Salesforce Field Service - Appointment Assistant

## Objective
Learn why Appointment Assistant is used in Salesforce Field Service.

## Purpose
Appointment Assistant recommends the best appointment slots using Salesforce scheduling logic.

## Why Use It?
- Faster scheduling
- Automatic slot recommendations
- Better technician utilization
- Reduced travel time
- Improved customer experience

## Prerequisites
- Service Appointment
- Service Territory
- Service Resource
- Service Territory Member
- Operating Hours
- Scheduling Policy

## Scheduling Factors
- Service Territory
- Skills
- Availability
- Travel Time
- Operating Hours
- Scheduling Policy
- Duration

## Process
Customer Request -> Work Order -> Service Appointment -> Appointment Assistant -> Suggested Slots -> CSR Selects Slot -> Appointment Scheduled -> Assigned Resource

## Objects Used
- Work Order
- Service Appointment
- Service Territory
- Service Resource
- Service Territory Member
- Assigned Resource

## Appointment Assistant vs Dispatcher Console
Appointment Assistant: Suggests best appointment slots.
Dispatcher Console: Manages and optimizes technician schedules.

## Benefits
- Faster booking
- Better resource utilization
- Automated scheduling
- Improved customer satisfaction

## Custom Polygon Integration
Use custom polygon logic to determine the correct Service Territory and eligible Service Resources. Appointment Assistant can then recommend the best appointment slots for those eligible resources.
