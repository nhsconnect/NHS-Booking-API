---
title: Get a specific Appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: get_an_appointment.html
summary: "Details the Get a specific appointment"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A system requests a specific appointment from either the registry or a Provider system.
**NB The registry has not yet been delivered.**

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
The Registry:

- WILL return a `200` **OK** HTTP status code on successful retrieval of the Appointment.
- WILL include the specified `Appointment` resource.

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
The succesful response body WILL be a FHIR `Appointment` resource, meeting <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the appropriate profile</a>.

### From the registry ###
Where the request is made against the registry, the returned resource will ONLY contain limited details as defined in <a href='register_an_appointment.html'>Register an Appointment</a>, specifically:

| Name | Value | Description |
|---|---|---|
| id | `[id]/_history/[version]` | Version specific identifier of the resource |
| status | `booked` \| `cancelled` \| `entered in error` | Indicates the state of the Appointment. |
| start | instant | A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) of when the Appointment starts |
| created | instant | When the resource was last updated <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). |
| participant | reference | A <a href='https://nhsconnect.github.io/fhir-policy/national-services.html#FHIR-NAT-01'>national service reference</a> to the Patient for whom this Appointment was booked, for example: `https://demographics.spineservices.nhs.uk|1234567890` where the Patient's NHS Number is 1234567890 |

### From a provider system ###
Where the request is made against a provider system, the resource will contain the details as defined in <a href='book_an_appointment.html'>Book an Appointment</a>, specifically:

| Name | Value | Description |
|---|---|---|
| id | `[id]/_history/[version]` | Version specific identifier of the resource |
| status | `booked` \| `cancelled` \| `entered in error` | Indicates the state of the Appointment. |
| start | instant | A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| end | instant |  A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| supportingInformation | reference | A reference to a contained resource (see below) which describes an associated document. |
| description | Call reason | Text describing the need for the appointment, to be shown for example in an appointment list |
| slot | reference | A reference to a Slot. |
| created | instant | When the appointment was last updated <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
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
| identifier | NHS Number | The Patient's NHS Number as defined in the <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>Care Connect Patient profile</a> |
| name | Patient's name | Name as retrieved from PDS, including Prefix, Given and Family components |
| telecom | Contact number | The number the Patient can be called back on |
| gender | `male` \| `female` \| `other` \| `unknown` | The gender as retrieved from PDS |
| birthdate | yyyy-mm-dd | Patient's DOB |
| address | Address | Patient's full address as retrieved from PDS |

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
| type | A value from `https://fhir.hl7.org.uk/STU3/ValueSet/CareConnect-DocumentType-1` | Indicates the type of document |
| content | see below | Describes the actual document |
| content.attachment | Describes the actual document |
| content.attachment.contentType | A valid mime type | Indicates the mime type of the document |
| content.attachment.language | `en` | States that the document is in English |

## Sample response from the registry ##

```xml
<Appointment xmlns="http://hl7.org/fhir">
    <id value="8f9312e1-ec99-4369-a511-d8f9882d4388/_history/1"></id>
    <meta>
        <profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"></profile>
    </meta>
    <identifier>
        <system value="urn:ietf:rfc:3986"></system>
        <value value="http://test.nhs.uk/test3"></value>
    </identifier>
    <status value="booked"></status>
    <start value="2019-02-01T10:51:23.620+00:00"></start>
    <created value="2019-02-06T10:43:22+00:00"></created>
    <participant>
        <actor>
            <identifier>
                <use value="official"></use>
                <system value="https://fhir.nhs.uk/Id/nhs-number"></system>
                <value value="1234554321"></value>
            </identifier>
        </actor>
    </participant>
</Appointment>
```

## Sample response from a Provider system ##

```json
{
    "resourceType": "Appointment",
    "meta": {
        "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
    },
    "id": "cfd9eba2-cc66-4195-a70c-10112ab1c838/_history/2",
    "language": "en",
    "text": "<div>Appointment</div>",
    "contained": [
        {

            "resourceType": "DocumentReference",
            "id": "123",
            "meta": {
                "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-DocumentReference-1"
            },
            "identifier": {
                "system": "https://tools.ietf.org/html/rfc4122",
                "value": "A709A442-3CF4-476E-8377-376500E829C9"
            },
            "status": "current",
            "type": {
                "coding": [
                    {
                        "system": "urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17",
                        "code": "POCD_MT200001GB02",
                        "display": "Integrated Urgent Care Report"
                    }
                ]
            },
            "indexed": "2018-12-20T09:43:41+11:00",
            "content": [
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
            "meta": {
                "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1"
            },
            "id": "P1",
            "identifier": [
            {
                "extension": [
                {
                    "url": "https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSNumberVerificationStatus-1",
                    "valueCodeableConcept": {
                        "coding": [
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
            "value": "9476719931"
            }
            ],
            "name": [
            {
                "use": "official",
                "prefix": "Mr",
                "given": "John",
                "family": "Smith"
            }
            ],
            "telecom": [
            {
                "system": "phone",
                "value": "01234 567 890",
                "use": "home",
                "rank": 1
            }
            ],
            "gender": "male",
            "birthDate": "1974-12-25",
            "address": [
            {
                "use": "home",
                "text": "123 High Street, Leeds LS1 4HR",
                "line": [
                    "123 High Street",
                    "Leeds"
                ],
                "city": "Leeds",
                "postalCode": "LS1 4HR"
            }
            ]
        }
        ],
        "status": "booked",
        "start": "2019-01-17T15:00:00.000Z",
        "end": "2019-01-17T15:10:00.000Z",
        "supportingInformation": [
        {
            "reference": "#123"
        }
        ],
        "description": "Reason for calling",
        "slot": [
        {
            "reference": "Slot/slot002"
        }
        ],
        "created": "2019-01-18T14:32:22.579+00:00",

    "participant": [
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