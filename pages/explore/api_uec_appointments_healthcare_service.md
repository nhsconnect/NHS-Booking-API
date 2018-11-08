---
title: Entities | Healthcare Service
keywords: usecase, location, order
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_healthcare_service.html
summary: The details of a healthcare service available at a location.
---
{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="HealthcareService" page="CareConnect-HealthcareService-1" fhirname="HealthcareService" fhirlink="healthcareservice.html" content="-" userlink="" %}

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
<td>HealthcareService.providedBy</td><td>1..1</td><td>Organization that provides this service</td><td>Reference (Organization) <a href="https://www.hl7.org/fhir/organization.html" target="_blank">https://www.hl7.org/fhir/organization.html</a></td>
</tr>
<tr>
<td>HealthcareService.location</td><td>0..1</td><td>Location(s) where service may be provided</td><td>Reference (Location) <a href="https://www.hl7.org/fhir/location.html" target="_blank">https://www.hl7.org/fhir/location.html</a></td>
</tr>
<tr>
<td>HealthcareService.name</td><td>1..1</td><td>Description of service as presented to a consumer while searching</td><td>Cambridge Urgent Treatment Centre</td>
</tr>
<tr>
<td>HealthcareService.comment</td><td>0..1</td><td>Additional description and/or any specific issues not covered elsewhere</td><td>Used to be known as Cambridge Minor Injuries Unit</td>
</tr>
<tr>
<td>HealthcareService.telecom</td><td>0..1</td><td>Contacts related to the healthcare service</td><td>phone: 01132333444</td>
</tr>
</table>