---
title: Appointment | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: appointment.html
summary: A booking of a healthcare event among patient(s), practitioner(s), related person(s) and/or device(s) for a specific date/time.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}






## Introduction ##
This resource is used in two closely linked but very different scenarios.






### Booking ###
It is sent as a POST request body from the Consumer to Provider systems in order to book an appointment.






### Registering ###
It is sent as a POST request body from the Consumer to Registry in order to register an appointment.

{% include custom/fhir.reference.html resource="Appointment" page="CareConnect-Appointment-1" fhirname="Appointment" fhirlink="appointment.html" content="-" userlink="" %}






## Key FHIR Elements for Booking ##

The following FHIR elements are key to this implementation when <a href='book_an_appointment.html'>Booking an appointment</a>:

See <a href='#booking-example-resource'>example resource</a>.

The Appointment resource **MUST NOT** include the following data items:

| Name | Description |
|---|---|
| id | The identity of the appointment will be assigned by the Providing system at the point of booking, and **MUST NOT** be included in the request body. |

The Appointment resource **MUST** include the following data items:

| Element | Cardinality | Description | Example(s) |
| --- | --- | --- | --- |
| status | [1..1] | Status of this Appointment | **MUST** be one of: `booked` \| `cancelled` \| `entered in error` |
| contained | [1..1] | Contained resources |  |
| contained[0] | [1..1] | A Contained DocumentReference resource conforming to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-DocumentReference-1'>CareConnect-DocumentReference-1</a> profile. | **See example resource below** |
| contained[1] | [1..1] | A Contained Patient resource conforming to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>CareConnect-Patient-1</a> profile. | **See example resource below** |
| start | [1..1] | The time the Appointment starts in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) | `2019-01-17T15:00:00.000Z` |
| end | [1..1] | The time the Appointment ends in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) | `2019-01-17T15:00:00.000Z` |
| created | [1..1] | When the appointment is booked <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). | `2019-01-17T15:00:00.000Z` |
| description | [1..1] | Text describing the need for the appointment, to be shown for example in an appointment list. Note that developers should follow guidance for their use case as to appropriate content in this field. | 111 Referral |
| slot | [1..1] | The Slot that this appointment is booked into | [ { "reference": "Slot/slot002" } ] |
| supportingInformation | [1..1] | Reference to a contained resource (see below) which describes an associated document. | [ { "reference": "#123" } ] |
| participant | [1..1] | A reference to a contained resource (see below) which describes the Patient for whom this Appointment is being booked | [ { "actor": { "reference": "#P1", "identifier": { "use": "official", "system": "https://fhir.nhs.uk/Id/nhs-number", "value": "1234554321" }, "display": "Peter James Chalmers" }, "status": "accepted" } ] |



### Contained resources ###

The appointment resource **MUST** have two <a href='http://hl7.org/fhir/STU3/references.html#contained'>contained</a> resources. Note that contained resources are given an identifier which is only required to be unique within the scope of the containing resource, and are referenced using that identifier prefixed with a Hash `#` character.



#### Patient ####
A contained Patient resource which conforms to the <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>Care Connect Patient profile</a>.
This resource is referenced in the Appointment's participant element, and is used to convey the details of the Patient for whom the Appointment is being booked.
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
| type | A value from `urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17` | Indicates the type of document |
| content | see below | Describes the actual document |
| content.attachment | Describes the actual document |
| content.attachment.contentType | A valid mime type | Indicates the mime type of the document |
| content.attachment.language | `en` | States that the document is in English |






## Key FHIR Elements for Registering ##

The following FHIR elements are key to this implementation when <a href='register_an_appointment.html'>Registering an appointment</a>:

See <a href='#registering-example-resource'>example resource</a>.

When registering, the Appointment resource **MUST NOT** include the following data items:

| Name | Description |
|---|---|
| id | The identity of the appointment will be assigned by the Providing system at the point of booking, and **MUST NOT** be included in the request body. |
| contained | No additional resources should be contained in the appointment |
| end | The appointment end time is not required. |
| supportingInformation | No supporting Information should be included in the registry. |
| description | No description should be included in the registry. |
| slot | The details of the slot are irrelevant for the purposes of the registry. |

When registering, the Appointment resource **MUST** include the following data items:

| Element | Cardinality | Description | Example(s) |
| --- | --- | --- | --- |
| status | [1..1] | Status of this Appointment | **MUST** be one of: `booked` \| `cancelled` \| `entered in error` |
| start | [1..1] | The time the Appointment starts in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) | 2019-01-17T15:00:00.000Z |
| created | [1..1] | When the appointment is being registered <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). | `2019-01-17T15:00:00.000Z` |
| identifier | [1..1] | The details of the appointment which is being registered | ... |
| identifier.system | [1..1] | Defines that the value is a URL | Fixed value: `urn:ietf:rfc:3986` |
| identifier.value | [1..1] | The URL of the appointment that is being registered | `https://ProviderBaseURL/Appointment/1234567890` |
| participant | [1..1] | A reference to a contained resource (see below) which describes the <a href='patient.html'>Patient</a> for whom this Appointment is being booked | [ { "actor": { "reference": "#P1", "identifier": { "use": "official", "system": "https://fhir.nhs.uk/Id/nhs-number", "value": "1234554321" }, "display": "Peter James Chalmers" }, "status": "accepted" } ] |







## Key FHIR Elements for Cancelling ##

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>. **NB The appointment resource MUST be <a href='get_an_appointment.html'>retrieved from the Provider system</a> in order to ensure that no data is lost.**.

See <a href='#cancelling-example-resource'>example resource</a>.

The following data items in the <a href='get_an_appointment.html'>retrieved Appointment</a> resource **MUST** be changed as defined:

| Name | Value | Description |
|---|---|---|
| status | `cancelled` | Indicates that the Appointment is being changed to a `cancelled` state. |
| created | instant | When the appointment is being cancelled in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |

**No other elements of the Appointment resource may be changed**







## Key FHIR Elements for Cancelling a registered Appointment ##

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>. **NB The appointment resource MUST be <a href='get_an_appointment.html'>retrieved from the Registry</a> in order to ensure that no data is lost.**

See <a href='#cancelling-registered-appointment-example-resource'>example resource</a>.

The following data items in the <a href='get_an_appointment.html'>retrieved Appointment</a> resource **MUST** be changed as defined:

| Name | Value | Description |
|---|---|---|
| status | `cancelled` | Indicates that the Appointment is being changed to a `cancelled` state. |
| created | instant | When the appointment is being cancelled in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |

**No other elements of the Appointment resource may be changed**







## Booking example resource ##
```json
{
    "resourceType": "Appointment",
    "meta": {
        "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
    },
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







## Registering example resource ##
```xml
<Appointment xmlns="http://hl7.org/fhir">
    <meta>
        <profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"></profile>
    </meta>
    <identifier>
        <system value="urn:ietf:rfc:3986"></system>
        <value value="http://test.nhs.uk/test3"></value>
    </identifier>
    <status value="booked"></status>
    <start value="2019-02-01T10:51:23.620+00:00"></start>
    <created value="2019-02-01T10:51:23+00:00"></created>
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






## Cancelling example resource ##
```json
{
    "resourceType": "Appointment",
    "meta": {
        "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
    },
    "id": "efea8f22-0c33-4000-b4e7-a28569d65e91",
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
    "status": "cancelled",
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






## Cancelling registered appointment example resource ##

```xml
<Appointment xmlns="http://hl7.org/fhir">
    <meta>
        <profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"></profile>
    </meta>
    <id value="b7e99463-00a1-45fc-98aa-02301c103aba"></id>
    <identifier>
        <system value="urn:ietf:rfc:3986"></system>
        <value value="http://test.nhs.uk/test3"></value>
    </identifier>
    <status value="cancelled"></status>
    <start value="2019-02-01T10:51:23.620+00:00"></start>
    <created value="2019-02-01T10:51:23+00:00"></created>
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
