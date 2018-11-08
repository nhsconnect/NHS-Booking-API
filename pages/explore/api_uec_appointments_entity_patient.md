---
title: Patient | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: api_uec_appointments_entity_patient.html
summary: Demographics and other administrative information about an individual receiving care or other health-related services.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

{% include custom/fhir.reference.html resource="Patient" page="CareConnect-Patient-1" fhirname="Patient" fhirlink="patient.html" content="-" userlink="" %}

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
<td>Patient.identifier(nhsNumber)</td><td>1..1</td><td>The patient's NHS number</td><td>1234567890</td>
</tr>
<tr>
<td>Patient.name</td><td>1..1</td><td>A name associated with the patient</td><td>John Smith</td>
</tr>
<tr>
<td>Patient.gender</td><td>0..1</td><td>male | female | other | unknown</td><td><a href="https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-AdministrativeGender-1" target="_blank">https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-AdministrativeGender-1</a></td>
</tr>
<tr>
<td>Patient.birthDate</td><td>0..1</td><td>The date of birth for the individual</td><td><a href="http://hl7.org/fhir/extension-patient-birthtime.html" target="_blank">http://hl7.org/fhir/extension-patient-birthtime.html</a></td>
</tr>
<tr>
<td>Patient.address</td><td>0..*</td><td>Addresses for the individual</td><td>123, Leeds Road, Leeds, LS1 1AA</td>
</tr>
</table>