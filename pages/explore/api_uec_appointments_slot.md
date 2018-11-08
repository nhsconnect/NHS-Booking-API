---
title: Slot | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_slot.html
summary: A slot of time on a schedule that may be available for booking appointments.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="Slot" page="CareConnect-Slot-1" fhirname="Slot" fhirlink="slot.html" content="-" userlink="" %}

## 1. Key FHIR Elements ##

The following FHIR elements have been identified as key to the implementation:

<table>
<tr>
<th>Element</th>
<th>Cardinality</th>
<th>Description</th>
<th>Example value(s)</th>
</tr>
<tr>
<td>Slot.serviceCategory</td><td>1..1</td><td>A broad categorisation of the service that is to be performed during this appointment</td><td><a href="https://www.hl7.org/fhir/valueset-service-category.html" target="_blank">https://www.hl7.org/fhir/valueset-service-category.html</a></td>
</tr>
<tr>
<td>Slot.appointmentType</td><td>0..1</td><td>Reason this appointment is scheduled</td><td><a href="https://www.hl7.org/fhir/v2/0276/index.html" target="_blank">https://www.hl7.org/fhir/v2/0276/index.html</a></td>
</tr>
<tr>
<td>Slot.schedule</td><td>1..1</td><td>A reference to a notional Schedule that this Slot belongs to</td><td>Reference (Schedule) <a href="https://www.hl7.org/fhir/schedule.html" target="_blank">https://www.hl7.org/fhir/schedule.html</a></td>
</tr>
<tr>
<td>Slot.status</td><td>1..*</td><td>busy | free | busy-unavailable | busy-tentative | entered-in-error</td><td><a href="https://www.hl7.org/fhir/valueset-slotstatus.html" target="_blank">https://www.hl7.org/fhir/valueset-slotstatus.html</a></td>
</tr>
<tr>
<td>Slot.start</td><td>1..1</td><td>Date/Time that the slot is to begin</td><td>2018-01-02T09:15:30Z</td>
</tr>
<tr>
<td>Slot.end</td><td>0..1</td><td>Date/Time that the slot is to conclude</td><td>2018-01-02T09:30:30Z</td>
</tr>
</table>

The Schedule will have a grouping of slots with different start and end times.