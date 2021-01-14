---
title: Get a specific Appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: get_an_appointment.html
summary: "Details the Get a specific appointment"
---

{% include important.html content="Site under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A system requests a specific appointment from a Provider system.

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Is routed through the SSP (Spine secure Proxy)
- Utilises TLS Mutual Authentication for system level authentication.

## RESTful Query ##

The request body is sent using an http `GET` method.

The following query demonstrates a request:

<table>
<tr>
<td>
http://[FHIR base URL]/Appointment/eef4f241-e276-4c9e-8d1f-6d12dcdc0086
</td>
</tr>
</table>

The following query demonstrates a <a href='http://hl7.org/fhir/stu3/http.html#vread'>version specific request</a>:

<table>
<tr>
<td>
http://[FHIR base URL]/Appointment/eef4f241-e276-4c9e-8d1f-6d12dcdc0086/_history/2
</td>
</tr>
</table>

## Response ##

### Success ###
A Provider system:

- WILL return a `200` **OK** HTTP status code on successful retrieval of the Appointment.
- WILL include the specified `Appointment` resource.

### Failure ###
- If the request fails because the resource does not exist on the server, the response will be a `404` **Not found**.
This SHOULD be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response WILL include a status of `403` **Forbidden**.
This SHOULD be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the query string parameters were invalid or unsupported, the response WILL include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response WILL include a status of `500` **Internal Server Error**.

Failure responses with a `4xx` status SHOULD NOT be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status MAY be retried.

## Response body structure ##
The successful response body WILL be a FHIR `Appointment` resource, meeting <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the appropriate profile</a>.

### From a provider system ###
Where the request is made against a provider system, the resource will contain the details as defined in <a href='book_an_appointment.html'>Book an Appointment</a>, specifically:

| Name | Value | Description |
|---|---|---|
| id | `[id]` | Identifier of the resource |
| versionId | `[id]` | Version specific identifier of the resource |
| status | `booked` \| `cancelled` \| `entered in error` | Indicates the state of the Appointment. |
| start | instant | A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| end | instant |  A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| supportingInformation | reference | A reference to a contained resource (see below) which describes an associated document. |
| description | Call reason | Text describing the need for the appointment, to be shown for example in an appointment list |
| slot | reference | A reference to a contained resource (see below) which describes the Slot for this Appointment |
| created | dateTime | The date and time the appointment was initially created in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| participant | reference | A reference to a contained resource (see below) which describes the Patient for whom this Appointment is being booked |

### Contained resources ###

The appointment resource retrieved from a Provider System WILL have two <a href='http://hl7.org/fhir/STU3/references.html#contained'>contained</a> resources. Note that contained resources are given an identifier which is only required to be unique within the scope of the containing resource, and are referenced using that identifier prefixed with a Hash `#` character.

#### Patient ####
A contained Patient resource which conforms to the <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>Care Connect Patient profile</a>.
This resource is referenced in the Appointment's participant element, and is used to convey the details of the Patient for whom the Appointment was booked.
The Patient resource **MUST** include the following data items:

| Name | Value | Description |
|---|---|---|
| id | Any | Any identifier, used to reference the resource from the `Appointment.Participant` element |
| identifier | NHS Number | The Patient's NHS Number* as defined in the <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>Care Connect Patient profile</a> |
| name | Patient's name | Name as retrieved from PDS, including Prefix, Given and Family components |
| telecom | Contact number | The number the Patient can be called back on |
| gender | `male` \| `female` \| `other` \| `unknown` | The gender as retrieved from PDS |
| birthdate | yyyy-mm-dd | Patient's DOB |
| address | Address | Patient's full address as retrieved from PDS |

*If you **DO NOT** have an NHS Number for the Patient, then you **MUST NOT** provide an identifier. **NB** If the Consumer does not provide a patient identifier, then the Provider system **MUST** populate this with their local identifier when accepting the booking, therefore making their identifier available in any subsequent requests for the appointment (e.g. <a href='search_patient_appointments.html'>search for Appointments for a Patient</a>, <a href='cancel_an_appointment.html'>cancel an Appointment</a> etc.)

#### DocumentReference ####
A contained DocumentReference resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-DocumentReference-1'>CareConnect-DocumentReference-1</a> profile.
This resource is referenced in the appointment's supportingInformation element, it describes the type and identifier(s) of any supporting information, for example a CDA document which may be transferred separately.
The DocumentReference resource **MUST** include the following data items:

| Name | Value | Description |
|---|---|---|
| id | Any | Any identifier, used to reference the resource from the Appointment.supportingInformation element |
| identifier | see below | Identifies the supporting information (i.e. CDA document) |
| identifier.system | `https://tools.ietf.org/html/rfc4122` | Indicates that the associated value is a UUID. |
| identifier.value | [UUID] | The UUID of the associated CDA (XPath: `/ClinicalDocument/id/@root`) |
| status | "current" | Indicates that the associated document is current. No other value is expected. |
| type | A value from `urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17` | Indicates the type of document |
| content | see below | Describes the actual document |
| content.attachment | Describes the actual document |
| content.attachment.contentType | A valid mime type | Indicates the mime type of the document |
| content.attachment.language | `en` | States that the document is in English |

#### Slot ####
A contained Slot resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Slot-1'>CareConnect-Slot-1</a> profile.
This resource is referenced in the appointment's slot element. The Slot resource must be contained to ensure that when booking an Appointment the correct Slot and Schedule combination are marked as 'busy'. 
The Slot resource **MUST** include the following data items:

| Name | Value | Description |
|---|---|---|
| identifier | See below | An identifier that identifies this Slot |
| identifier.system | `https://tools.ietf.org/html/rfc4122` | Indicates that the associated value is a UUID. |
| identifier.value | [UUID] | The UUID of the Slot |
| status | One of `busy` \| `free` | The current status of the Slot |
| start | `2019-01-17T15:00:00.000Z` |  The start time of this Slot in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| end | `2019-01-17T15:00:00.000Z` |  The end time of this Slot in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| schedule | Reference(Schedule) |  Identifies the Schedule, which links the Slot to a HealthcareService, and optionally to a <a href='practitioner.html'>Practitioner</a> and <a href='practitioner_role.html'>PractitionerRole</a> |


## Sample response from a Provider system ##

```json
{
    "resourceType": "Appointment",
    "id": "cfd9eba2-cc66-4195-a70c-10112ab1c838",
    "meta": {
        "versionId": "2",
        "profile":  [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
        ]
    },
    "language": "en",
    "text": {
        "status": "generated",
        "div": "Appointment"
    },
    "contained":  [
        {
            "resourceType": "DocumentReference",
            "id": "123",
            "meta": {
                "profile":  [
                    "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-DocumentReference-1"
                ]
            },
            "identifier":  [
                {
                    "system": "https://tools.ietf.org/html/rfc4122",
                    "value": "6b9c59dd-675b-4026-98db-f608ef501e6e"
                }
            ],
            "status": "current",
            "type": {
                "coding":  [
                    {
                        "system": "urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17",
                        "code": "POCD_MT200001GB02",
                        "display": "Integrated Urgent Care Report"
                    }
                ]
            },
            "indexed": "2018-12-20T09:43:41+11:00",
            "content":  [
                {
                    "attachment": {
                        "contentType": "application/hl7-v3+xml",
                        "language": "en"
                    }
                }
            ]
        },
        {
            "resourceType": "Patient",
            "id": "P1",
            "meta": {
                "profile":  [
                    "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1"
                ]
            },
            "identifier":  [
                {
                    "extension":  [
                        {
                            "url": "https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSNumberVerificationStatus-1",
                            "valueCodeableConcept": {
                                "coding":  [
                                    {
                                        "system": "https://fhir.hl7.org.uk/STU3/CodeSystem/CareConnect-NHSNumberVerificationStatus-1",
                                        "code": "01",
                                        "display": "Number present and verified"
                                    }
                                ]
                            }
                        }
                    ],
                    "use": "official",
                    "system": "https://fhir.nhs.uk/Id/nhs-number",
                    "value": "1234554321"
                }
            ],
            "name":  [
                {
                    "use": "official",
                    "family": "Chalmers",
                    "given":  [
                        "Peter"
                    ],
                    "prefix":  [
                        "Mr"
                    ]
                }
            ],
            "telecom":  [
                {
                    "system": "phone",
                    "value": "01234 567 890",
                    "use": "home",
                    "rank": 1
                }
            ],
            "gender": "male",
            "birthDate": "1974-12-25",
            "address":  [
                {
                    "use": "home",
                    "text": "123 High Street, Leeds LS1 4HR",
                    "line":  [
                        "123 High Street",
                        "Leeds"
                    ],
                    "city": "Leeds",
                    "postalCode": "LS1 4HR"
                }
            ]
        },
        {
            "resourceType": "Slot",
            "id": "slot002",
            "identifier":  [
                {
                    "system": "https://tools.ietf.org/html/rfc4122",
                    "value": "1db79eb3-72f8-4569-a8dc-af8759797e0f"
                }
            ],
            "schedule": {
                "reference": "Schedule/sched1111"
            },
            "status": "busy",
            "start": "2019-01-17T15:00:00.000Z",
            "end": "2019-01-17T15:30:00.000Z"
        }
    ],
    "status": "booked",
    "description": "Reason for calling",
    "supportingInformation":  [
        {
            "reference": "#123"
        }
    ],
    "start": "2019-01-17T15:00:00.000Z",
    "end": "2019-01-17T15:10:00.000Z",
    "slot":  [
        {
            "reference": "#slot002"
        }
    ],
    "created": "2019-01-17T14:32:22.579+00:00",
    "participant":  [
        {
            "actor": {
                "reference": "#P1",
                "identifier": {
                    "use": "official",
                    "system": "https://fhir.nhs.uk/Id/nhs-number",
                    "value": "1234554321"
                },
                "display": "Peter James Chalmers"
            },
            "status": "accepted"
        }
    ]
}
```