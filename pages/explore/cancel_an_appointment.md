---
title: Cancel an appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: cancel_an_appointment.html
summary: "Details the Cancel an Appointment interaction"
---

{% include important.html content="Site under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A consuming system wishes to cancel an Appointment which was previously <a href='book_an_appointment.html'>booked</a>.

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Is routed through the SSP (Spine secure Proxy)
- Utilises TLS Mutual Authentication for system level authentication.

## Request ##

The request body is sent using an http `PUT` method.

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>. **NB The appointment resource **MUST** be <a href='get_an_appointment.html'>retrieved from the Provider system</a> in order to ensure that no data is lost.**, for full details of the payload body, see the <a href='appointment.html'>Appointment resource details</a>.

The update is protected using <a href='http://hl7.org/fhir/stu3/http.html#concurrency'>the approach described in the FHIR standard</a> therefore an update may be rejected to prevent the loss of data.

- Provider systems **MUST** store previous versions of the resource to defend against any such loss of data.

## Response ##

### Success ###
Where the request succeeded, the response **MUST** include a status of `200` **OK**.
The response **MUST** include a Location header giving the absolute URL of the created Appointment. This URL **MUST** remain stable, and the resource **SHOULD** support RESTful updates using a PUT request to this URL.
The response body **MUST** include the updated Appointment, this resource **MUST** include the newly assigned versionId of the resource.

### Failure ###
- If the request fails because of a business rule (for example if differences are detected between the existing and updated Appointment), the response **MUST** include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This **SHOULD** be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because the request body failed validation against the relevant profiles, the response **MUST** include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This **SHOULD** be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response **MUST** include a status of `403` **Forbidden**.
This **SHOULD** be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because a version conflict was detected, the response **MUST** include a status of `409` **Conflict**.
This **SHOULD** be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because an update is attempted which does not include an `If-Match` header, the response **MUST** include a status of `412` **Pre-condition failed**.
This **SHOULD** be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the request body was simply invalid, the response **MUST** include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response **MUST** include a status of `500` **Internal Server Error**.

Failure responses with a `4xx` status **SHOULD NOT** be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status **MAY** be retried.

