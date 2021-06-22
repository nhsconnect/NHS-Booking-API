---
title: Search for Appointments for a Patient
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: search_patient_appointments.html
summary: "Details the Search for appointments for a given Patient"
---

{% include important.html content="This is a live release of the NHS Booking API specification. However, prior to commencing development, please contact the [Booking & Referral Team.](mailto:bookingandreferrals@nhs.net)" %}

## Use case ##

A system requests appointments which have been registered for a specific patient from a Provider system.

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Utilises TLS Mutual Authentication for system level authentication.

## Search parameters ##

The provider systems support the following search parameters that SHOULD be passed to the API:

| Name | Type | Description | Paths |
|---|---|---|---|
| `Patient` | reference | The national service identifier of the Patient for whom Slots are being requested | `Appointment.participant.actor` |


## _format ##

The request can be formatted to the following MIME types:

|--|--|
|JSON|`_format=xml`|
|XML|`_format=json`|


## RESTful Query ##

The request body is sent using an http `GET` method.

The following query demonstrates a full request for information:

<table>
<tr>
<td>
http://[FHIR base URL]/Appointment<br>
?Appointment.participant.actor:Patient.identifier=https://fhir.nhs.uk/Id/nhs-number|1234567890
</td>
</tr>
</table>

## Response ##

### Success ###

Provider systems:

- MUST return a `200` **OK** HTTP status code on successful retrieval of Appointments.
- MUST include the (Zero to Many) `Appointment` resources which meet the requested criteria.
- MUST NOT implement <a href='http://hl7.org/fhir/STU3/http.html#paging'>paging as described here</a> to limit the number of resources returned.
- SHOULD implement a limit such that Appointments in the past will not be returned.

### Failure ###

Provider systems:

- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response MUST include a status of `403` **Forbidden**.
This SHOULD be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the query string parameters were invalid or unsupported, the response SHOULD include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response SHOULD include a status of `500` **Internal Server Error**.


Failure responses with a `4xx` status SHOULD NOT be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status MAY be retried.

## Response body structure ##
The response body WILL be a FHIR `Bundle` resource containing zero to many Appointment resources, each meeting <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>. The resources will contain limited details, specifically those defined in <a href='register_an_appointment.html'>Register an appointment</a>:

| Name | Value | Description |
|---|---|---|
| fullURL | [base]/Appointment/[id]/_history/[version] | The version specific (indicating the current version) logical identifier of the resource. |
| status | `booked` \| `cancelled` \| `entered in error` | Indicates the status of the Appointment. |
| start | instant | A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) of when the Appointment starts |
| end | instant | A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) of when the Appointment ends |
| created | dateTime | The date and time the appointment was initially created in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). |
| participant | reference | A <a href='https://nhsconnect.github.io/fhir-policy/national-services.html#FHIR-NAT-01'>national service reference</a> to the Patient for whom this Appointment was booked, for example: `https://demographics.spineservices.nhs.uk/1234567890` where the Patient's NHS Number is 1234567890 |

## Sample response ##

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Bundle xmlns="http://hl7.org/fhir">
	<id value="aaf4ec55-8a7f-4ff4-ab2a-0336ccb9708f"/>
	<meta>
		<lastUpdated value="2019-05-09T13:08:25.180+00:00"/>
	</meta>
	<type value="searchset"/>
	<total value="6"/>
	<link>
		<relation value="self"/>
		<url value="http://FHIRBaseURL/Appointment?Appointment.participant.actor=https%3A%2F%2Ffhir.nhs.uk%2FId%2Fnhs-number%7C1234554321"/>
	</link>
	<entry>
		<fullUrl value="http://FHIRBaseURL/Appointment/8f9312e1-ec99-4369-a511-d8f9882d4388"/>
		<resource>
			<Appointment xmlns="http://hl7.org/fhir">
				<id value="8f9312e1-ec99-4369-a511-d8f9882d4388"/>
				<meta>
					<versionId value="1"/>
					<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
				</meta>
				<identifier>
					<system value="urn:ietf:rfc:3986"/>
					<value value="http://test.nhs.uk/test3"/>
				</identifier>
				<status value="booked"/>
				<start value="2019-02-01T10:51:23.620+00:00"/>
				<end value="2019-02-01T11:01:23.620+00:00"/>
				<created value="2019-01-06T10:43:22+00:00"/>
				<participant>
					<actor>
						<identifier>
							<use value="official"/>
							<system value="https://fhir.nhs.uk/Id/nhs-number"/>
							<value value="1234554321"/>
						</identifier>
					</actor>
					<status value="accepted"/>
				</participant>
			</Appointment>
		</resource>
	</entry>
	<entry>
		<fullUrl value="http://FHIRBaseURL/Appointment/d57e81ec-9886-42d8-8504-ee1e54ed63f1"/>
		<resource>
			<Appointment xmlns="http://hl7.org/fhir">
				<id value="d57e81ec-9886-42d8-8504-ee1e54ed63f1"/>
				<meta>
					<versionId value="1"/>
					<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
				</meta>
				<identifier>
					<system value="urn:ietf:rfc:3986"/>
					<value value="http://test.nhs.uk/test1"/>
				</identifier>
				<status value="booked"/>
				<start value="2019-02-01T10:51:23.620+00:00"/>
				<end value="2019-02-01T11:01:23.620+00:00"/>
				<created value="2019-01-06T09:40:12+00:00"/>
				<participant>
					<actor>
						<identifier>
							<use value="official"/>
							<system value="https://fhir.nhs.uk/Id/nhs-number"/>
							<value value="1234554321"/>
						</identifier>
					</actor>
					<status value="accepted"/>
				</participant>
			</Appointment>
		</resource>
	</entry>
	<entry>
		<fullUrl value="http://FHIRBaseURL/Appointment/bd908180-fcdc-4afe-baf2-ef9533fbe0fd"/>
		<resource>
			<Appointment xmlns="http://hl7.org/fhir">
				<id value="bd908180-fcdc-4afe-baf2-ef9533fbe0fd"/>
				<meta>
					<versionId value="1"/>
					<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
				</meta>
				<identifier>
					<system value="urn:ietf:rfc:3986"/>
					<value value="http://test.nhs.uk/test4"/>
				</identifier>
				<status value="booked"/>
				<start value="2019-02-01T10:51:23.620+00:00"/>
				<end value="2019-02-01T11:01:23.620+00:00"/>
				<created value="2019-01-06T12:57:10+00:00"/>
				<participant>
					<actor>
						<identifier>
							<use value="official"/>
							<system value="https://fhir.nhs.uk/Id/nhs-number"/>
							<value value="1234554321"/>
						</identifier>
					</actor>
					<status value="accepted"/>
				</participant>
			</Appointment>
		</resource>
	</entry>
	<entry>
		<fullUrl value="http://FHIRBaseURL/Appointment/a925cc65-e6e5-4dd7-b634-b81901e68f2e"/>
		<resource>
			<Appointment xmlns="http://hl7.org/fhir">
				<id value="a925cc65-e6e5-4dd7-b634-b81901e68f2e"/>
				<meta>
					<versionId value="1"/>
					<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
				</meta>
				<identifier>
					<system value="urn:ietf:rfc:3986"/>
					<value value="http://test.nhs.uk/test5"/>
				</identifier>
				<status value="booked"/>
				<start value="2019-02-01T10:51:23.620+00:00"/>
				<end value="2019-02-01T11:01:23.620+00:00"/>
				<created value="2019-01-06T12:57:24+00:00"/>
				<participant>
					<actor>
						<identifier>
							<use value="official"/>
							<system value="https://fhir.nhs.uk/Id/nhs-number"/>
							<value value="1234554321"/>
						</identifier>
					</actor>
					<status value="accepted"/>
				</participant>
			</Appointment>
		</resource>
	</entry>
	<entry>
		<fullUrl value="http://FHIRBaseURL/Appointment/99729e6f-2651-4444-b1c0-3633177f742e"/>
		<resource>
			<Appointment xmlns="http://hl7.org/fhir">
				<id value="99729e6f-2651-4444-b1c0-3633177f742e"/>
				<meta>
					<versionId value="1"/>
					<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
				</meta>
				<identifier>
					<system value="urn:ietf:rfc:3986"/>
					<value value="http://test.nhs.uk/test"/>
				</identifier>
				<status value="booked"/>
				<start value="2019-02-01T10:51:23.620+00:00"/>
				<end value="2019-02-01T11:01:23.620+00:00"/>
				<created value="2019-01-04T09:39:49+00:00"/>
				<participant>
					<actor>
						<identifier>
							<use value="official"/>
							<system value="https://fhir.nhs.uk/Id/nhs-number"/>
							<value value="1234554321"/>
						</identifier>
					</actor>
					<status value="accepted"/>
				</participant>
			</Appointment>
		</resource>
	</entry>
	<entry>
		<fullUrl value="http://FHIRBaseURL/Appointment/2f5accb1-23fe-477f-b90a-2c0cef4ab6c3"/>
		<resource>
			<Appointment xmlns="http://hl7.org/fhir">
				<id value="2f5accb1-23fe-477f-b90a-2c0cef4ab6c3"/>
				<meta>
					<versionId value="1"/>
					<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
				</meta>
				<identifier>
					<system value="urn:ietf:rfc:3986"/>
					<value value="http://test.nhs.uk/test8"/>
				</identifier>
				<status value="booked"/>
				<start value="2019-02-01T10:51:23.620+00:00"/>
				<end value="2019-02-01T11:01:23.620+00:00"/>
				<created value="2019-01-07T09:41:47+00:00"/>
				<participant>
					<actor>
						<identifier>
							<use value="official"/>
							<system value="https://fhir.nhs.uk/Id/nhs-number"/>
							<value value="1234554321"/>
						</identifier>
					</actor>
					<status value="accepted"/>
				</participant>
			</Appointment>
		</resource>
	</entry>
</Bundle>
```