---
title: Entities | Organization
keywords: usecase, Organization
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_organisation.html
summary: A formally or informally recognized grouping of people or organizations formed for the purpose of achieving some form of collective action. Includes companies, institutions, corporations, departments, community groups, healthcare practice groups, etc.
---
{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="Organization" page="CareConnect-Organization-1" fhirname="Organization" fhirlink="organization.html" content="-" userlink="-" %}

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
<td>Organization.identifier</td><td>0..*</td><td>Identifies this organization across multiple systems</td><td></td>
</tr>
</table>