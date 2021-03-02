---
title: Schedule | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: schedule.html
summary: A Schedule which conceptually groups a set of Slots.
---

{% include important.html content="This site is under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is optionally returned linked to one or more <a href='slot.html'>Slot resources</a>. For more clarification see <a href='resources_overview.html#urgent--emergency-care-appointments-apis'>the diagram on the FHIR Resources overview page</a>.

Schedule will be returned with Slots following a <a href='search_free_slots.html'>search for free Slots</a>, it is used simply to provide the links between other optional resources, as can be seen from <a href='resources_overview.html#urgent--emergency-care-appointments-apis'>the diagram on the FHIR Resources overview page</a>.

{% include custom/fhir.reference.html resource="Schedule" page="CareConnect-Schedule-1" fhirname="Schedule" fhirlink="schedule.html" content="-" userlink="" %}

## Key FHIR Elements ##

The following FHIR elements are key to this implementation :

**Note: Some elements have been constrained further from the Care Connect profile.** 

| Element | Cardinality | Description | Example(s) |
| --- | --- | --- | --- |
| id | [1..1] | A unique id which identifies a schedule. The id can be a globally unique (e.g. UUID).| 12456 |
| actor | [1..3] | Resources linked to this Schedule, can be any of the below | ... |
| actor (HealthcareService) | [0..1] | The HealthcareService that this Schedule is part of. | `{ "reference": "HealthcareService/1231231234" }` |
|actor(Location)|[0..1]|The Location that this Schedule is part of.|`{ "reference": "Location/1231231234" }`|
| actor (Practitioner) | [0..1] | Optionally identifies the [Practitioner](#practitioner-and-practitionerrole) where one is assigned to this Schedule. | `{ "reference": "Practitioner/1231231234" }` |
| actor (PractitionerRole) | [0..1] | Optionally identifies the [PractitionerRole](#practitioner-and-practitionerrole) where a role is assigned to this Schedule. | `{ "reference": "PractitionerRole/767676767" }` |

Note: there *MUST* be a reference to either HealthCare service OR Location OR Both.

### Practitioner and PractitionerRole ###

The Practitioner and the PractitionerRole resources refer to the Care Connect profiles developed by HL7 UK. 
The Practitioner profile accepts a *SDS User ID* or *SDS role profile Id* as an identifier. The PractitionerRole profile only 
accepts the *SDS role profile Id*.
The profiles can be found at:

- [Practitioner](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1)

- [PractitionerRole](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-PractitionerRole-1)
