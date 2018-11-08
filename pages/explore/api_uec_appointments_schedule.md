---
title: Entities | Schedule
keywords: usecase, location, order
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_schedule.html
summary: Details and position information for a physical place where services are provided and resources and participants may be stored, found, contained or accommodated.
---
{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="Schedule" page="CareConnect-Schedule-1" fhirname="Schedule" fhirlink="schedule.html" content="-" userlink="" %}

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
<td>Schedule.actor(HealthcareService)</td><td>1..1</td><td>The HealthcareService resource this Schedule resource is providing availability information for.</td><td>Reference (HealthcareService) <a href="https://www.hl7.org/fhir/healthcareservice.html" target="_blank">https://www.hl7.org/fhir/healthcareservice.html</a></td>
</tr>
</table>