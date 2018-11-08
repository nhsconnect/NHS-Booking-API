---
title: Resources Overview
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: explore.html
summary: "This page provides an overview of the FHIR STU3 Resources that are required to build the required API messaging. Each link will take you to the resource page detail with a link to the StructureDefinitions of each resource."
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Urgent & Emergency Care Appointments API's ###

The “disposition” from the patient interaction (NHS111 call etc) determines the urgency (giving the time bounds). The DOS (Directory of Services) will  have been interrogated and an appropriate service for the Patient identified.

- The time bounds and a specific service identifier are passed to the API
- The provider system searches for free slots within the time period given, for the identified service
- The Patient (or the 111 call handler) will review the free slots & pick one
- The provider system will then be sent the slot details & patient detail and will book an appointment into the slot

<img src="images/UEC-Appointments/UEC_Appointments_Flow.png">

<table style="min-width:100%;width:100%">
<tr id="clinical">
<th style="width:33%;">Requirement</th>
<th style="width:33%;">FHIR Resource(s)</th>
<th style="width:33%;">Key FHIR Elements(s)</th>
<th style="width:33%;">Status</th>
</tr>
<tr>
<td>Patient</td>
<td><a href="api_uec_appointments_entity_patient.html">Care Connect Patient</a></td>
<td>identifier(NHSnumber)<br/>
Name<br/>
birthDate<br/>
Gender</td>
<td>Existing</td>
</tr>
<tr>
<td>Practitioner</td>
<td><a href="api_uec_appointments_entity_practitioner.html">Care Connect Practitioner</a></td>
<td>SDSUserid<br/>
Name</td>
<td>Existing</td>
</tr>
<tr>
<td>PractitionerRole</td>
<td><a href="api_uec_appointments_entity_practitioner_role.html">Care Connect PractitionerRole</a></td>
<td>SDSJobRoleName</td>
<td>Existing</td>
</tr>
<tr>
<td>Organization</td>
<td><a href="api_uec_appointments_organisation.html">Care Connect Organization</a></td>
<td>type<br/>
name<br/>
telecom<br/>
address<br/>
contact
</td>
<td>Existing</td>
</tr>
<tr>
<td>Location</td>
<td><a href="api_uec_appointments_location.html">Care Connect Location</a></td>
<td>status<br/>
name<br/>
alias<br/>
description<br/>
telecom<br/>
address<br/>
managing org<br/>
part of
</td>
<td>Existing</td>
</tr>
<tr>
<td>HealthcareService</td>
<td><a href="api_uec_appointments_healthcare_service.html">HealthcareService</a></td>
<td>provided by<br/>
location<br/>
name<br/>
comment<br/>
telecom
</td>
<td>New</td>
</tr>
<tr>
<td>Slot</td>
<td><a href="api_uec_appointments_slot.html">Slot</a></td>
<td>schedule<br/>
status<br/>
start<br/>
end<br/>
service category<br/>
appointment type
</td>
<td>New</td>
</tr>
<tr>
<td>Schedule</td>
<td><a href="api_uec_appointments_schedule.html">Schedule</a></td>
<td>actor.reference</td>
<td>New</td>
</tr>
<tr>
<td>Appointment</td>
<td><a href="api_uec_appointments_appointment.html">Appointment</a></td>
<td>status<br/>
participant.actor<br/>
reason<br/>
slot
</td>
<td>New</td>
</tr>
</table>