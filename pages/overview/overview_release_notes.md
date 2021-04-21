---
title: Release Notes
keywords: development, versioning
sidebar: overview_sidebar
permalink: overview_release_notes.html
---

{% include important.html content="Site under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## 2.0.3 Released: 23-04-21 ##

- Corrected the CapabilityStatement example to add the additional 'start' search parameter.


## 2.0.2-beta Released: 02-03-21 ##

- Resources Overview page has been updated to reflect a generic use case.
- Guidance around Location resource has been updated.
- Additional guidance added regarding _include_ and _revinclude_ parameters
- Element _Id_ has been replaced with _identifier_ for HealthcareService resource.
- Updated the description for _id_ for the Schedule resource.
- Practitioner and PractitionerRole pages have been removed.
- Appointment example has been fixed to reflect the correct format for references.
- Appointment.created element datatype has been changed from Instant to dateTime to reflect the profile.
- References to the registry function has been removed.
- Additional guidance added to the Appointment.created element.
- Narrative added to the diagrams on the profile overview page.
- New page added for the the Bundle resource.
- Addition of a new FHIR SearchParameter required for searching slots.
- Guidance added for outputting data in different formats.
- Additional guidance added for the patient.telecom element.
- CapabilityStatement page added

## 2.0.1-beta Released: 19-10-20 ##

- Updated description for 'supportingInfo' element for the Appointment resource.
- Updated description for 'particpant' element for the Appointment resource.
- Added guidance for using 'incomingReferral' element for the Appointment resource.
- Updated contained resource guidance for the Appointment resource.
- Updated search parameters guidance when searching for free slots  

## 2.0.0-beta Released: 19-08-20 ##

- Renamed from NHS FHIR Scheduling API to NHS FHIR Booking API.
- Updated Appointment guidance to include mandatory Appointment.end to align with the FHIR standard.
- Updated Appointment.created guidance to align with the FHIR standard.
- Updated Appointment.contained to include Slot as a contained resource.
- Updated Appointment examples.
- Updated Patient identifier guidance in line with <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>CareConnect-Patient-1</a> profile.
- Updated 'success' behaviour for cancelling an Appointment.
- Updated Slot elements.
- Updated 'Search for free slots' example.
- Updated 'Search for appointments' example.
- Updated 'Get an appointment' examples.
- Updated development guidance on validating resources.


## 1.0.6-alpha Released: 09-04-2020 ##

- Updated guidance on the use of ASID.
- Updated appointment examples.

## 1.0.5-alpha Released: 13-01-2020 ##

- Updates to the patient identifier system.
- Updated JWT guidance.
- Updated appointment description field.

## 1.0.4-alpha Released: 24-06-2019 ##

- Added missing resources.
- Added overall sequence diagram and more detailed process steps.
- Added concept of the registry.
- Cleaned out some redundant files.
- Added validated example resources.
- Added links to demonstrator, validator and JWT utilities.
- Added references to versioning of Appointment resources.

## 1.0.3-alpha Released: 05-12-2018 ##

- Clarified Search for free slots text and added RESTful parameter query
- Added DocumentReference resource for attaching CDA documents to Appointments
- DocumentReference resource added to the example in Book an Appointment


## 1.0.2-alpha Released: 01-11-2018 ##

Changes made in response to the wider team review including business analysts/technical architects.

- Overview diagram updated with cardinality changes
- Search for free slots diagram UI mock up refined
- Search for free slots information updated
- Book an appointment information updated
- Tags removed sitewide
- Profile pages re-formatted with broken links/numbering fixed

## 1.0.1-experimental Released: 25-09-2018 ##

Changes made in response to the internal team review.

- Flow diagram changed to represent data flow accurately, cardinality updated
- Email feedback removed
- Top navigation links removed
- Profile pages fixed to point to correct FHIR profiles
- Sidebar menu "FHIR" wording removed
- Overview FHIR Profiles page, added clearer definition of the process
- FHIR profile pages, added FHIR elements detailed descriptions
- Warning banner text updated
- FAQ section edited

## 1.0.0-experimental ##

Early development phase, to define the requirements against FHIR profiles.