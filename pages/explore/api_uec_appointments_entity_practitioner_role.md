---
title: Practitioner Role | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_entity_practitioner_role.html
summary: A specific set of Roles/Locations/specialties/services that a practitioner may perform at an organization for a period of time.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="PractitionerRole" page="CareConnect-PractitionerRole-1" fhirname="PractitionerRole" fhirlink="practitionerrole.html" content="-" userlink="" %}

## 1. Key FHIR Elements ##

<table>
<tr>
<th>Element</th>
<th>Cardinality</th>
<th>Description</th>
<th>Example value(s)</th>
</tr>
<tr>
<td>PractitionerRole.practitioner</td><td>0..1</td><td>Practitioner that is able to provide the defined services for the organisation</td><td>Reference <a href="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1" target="_blank">https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1</a></td>
</tr>
<tr>
<td>PractitionerRole.code</td><td>0..*</td><td>Roles which this practitioner may perform</td><td><a href="http://hl7.org/fhir/stu3/valueset-practitioner-role.html" target="_blank">http://hl7.org/fhir/stu3/valueset-practitioner-role.html</a> e.g. doctor</td>
</tr>
</table>