---
title: Entities | Location
keywords: usecase, location, order
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_location.html
summary: Details and position information for a physical place where services are provided and resources and participants may be stored, found, contained or accommodated.
---
{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="Location" page="CareConnect-Location-1" fhirname="Location" fhirlink="location.html" content="-" userlink="" %}

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
<td>Location.identifier</td><td>0..*</td><td>Unique code or number identifying the location to its users</td><td></td>
</tr>
</table>