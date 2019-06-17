---
title: Register an appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: register_an_appointment.html
summary: "Details the register an Appointment interaction"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A system wishes to register a <a href='book_an_appointment.html'>previously booked Appointment</a> in the Registry.
**NB The registry has not yet been delivered.**

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Utilises TLS Mutual Authentication for system level authentication.

## Request ##

The request body is sent using an http `POST` method.

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>, for full details of the payload body, see the <a href='appointment.html'>Appointment resource details</a>.

## Response ##

### Success ###
Where the request succeeded, the response MUST include a status of 201 Created. The response MUST include a Location header giving the absolute URL of the created Appointment including the version as per <a href='http://hl7.org/fhir/stu3/http.html#create'>the FHIR standard</a> i.e: Location: [base]/Appointment/[id]/_history/[vid] where [id] and [vid] are the newly created id and version id for the resource version. Servers SHOULD return an ETag header with the versionId and a Last-Modified header. This URL WILL remain stable, and the resource WILL support RESTful updates using a PUT request to this URL (a version aware update as per http://hl7.org/fhir/stu3/http.html#concurrency ). The response body WILL include the created Appointment including the newly created id (with version id) value, The response body MUST include the created Appointment, this response resource WILL include the newly assigned id and version id of the resource.

### Failure ###
- If the request fails because of a business rule, the response **WILL** include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This **WILL** be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because the request body failed validation against the relevant profiles, the response **WILL** include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This **WILL** be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response **WILL** include a status of `403` **Forbidden**.
This **WILL** be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the request body was simply invalid, the response **WILL** include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response **WILL** include a status of `500` **Internal Server Error**.

Failure responses with a `4xx` status **SHOULD NOT** be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status **MAY** be retried.
