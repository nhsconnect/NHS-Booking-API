---
title: Practitioner | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_entity_practitioner.html
summary: A person who is directly or indirectly involved in the provisioning of healthcare.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="Practitioner" page="CareConnect-Practitioner-1" fhirname="Practitioner" fhirlink="Practitioner.html" content="-" userlink="" %}

## 1. Key FHIR Elements ##

<table>
<tr>
<th>Element</th>
<th>Cardinality</th>
<th>Description</th>
<th>Example value(s)</th>
</tr>
<tr>
<td>Practitioner.identifier</td><td>0..*</td><td>A identifier for the person as this agent</td><td>1234567890</td>
</tr>
<tr>
<td>Practitioner.name</td><td>0..*</td><td>The name(s) associated with the practitioner</td><td>Dr John Smith</td>
</tr>
</table>