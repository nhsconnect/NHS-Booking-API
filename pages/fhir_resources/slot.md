---
title: Slot | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: slot.html
summary: A Slot into which an Appointment can be booked.
---

{% include important.html content="This site is under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is fundamental to the Appointment Booking process, it represents time allocations determined by a Provider organisation which are made available to Consumers. For more clarification see <a href='resources_overview.html#urgent--emergency-care-appointments-apis'>the diagram on the FHIR Resources overview page</a>.

Slots are returned following a <a href='search_free_slots.html'>search for free Slots</a>. The slot is linked to other resources through a referenced Schedule resource.

{% include custom/fhir.reference.html resource="Slot" page="CareConnect-Slot-1" fhirname="Slot" fhirlink="slot.html" content="-" userlink="" %}

## Key FHIR Elements ##

The following FHIR elements are key to this implementation :

| Element | Cardinality | Description | Example(s) |
| --- | --- | --- | --- |
| identifier | [1..1] | An identifier that identifies this Slot. | `"identifier": {"system": "https://tools.ietf.org/html/rfc4122","value": "1db79eb3-72f8-4569-a8dc-af8759797e0f"},` |
| status | [1..1] | The current status of the Slot. | One of `busy` \| `free` |
| start | [1..1] | The start time of this Slot in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). | `2019-01-17T15:00:00.000Z` |
| end | [1..1] | The end time of this Slot in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). | `2019-01-17T15:30:00.000Z` |
| schedule | [1..1] | Identifies the Schedule, which links the Slot to a HealthcareService, and optionally to a <a href='practitioner.html'>Practitioner</a> and <a href='practitioner_role.html'>PractitionerRole</a>. | `{ "reference": "Schedule/0dbff4a3-fa40-4f9f-93fe-412c2a6c967e" }` |