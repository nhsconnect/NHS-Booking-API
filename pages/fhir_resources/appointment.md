---
title: Appointment | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: appointment.html
summary: A booking of a healthcare event among patient(s), related person(s) and/or device(s) for a specific date/time.
---

{% include important.html content="This site is under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}






## Introduction ##
This resource is used in two closely linked but very different scenarios.






### Booking ###
It is sent as a POST request body from the Consumer to Provider systems in order to book an appointment.




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
| contained[2] | [1..1] | A Contained Slot resource conforming to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Slot-1'>CareConnect-Slot-1</a> profile. | **See example resource below** |
| start | [1..1] | The time the Appointment starts in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) | `2019-01-17T15:00:00.000Z` |
| end | [1..1] | The time the Appointment ends in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) | `2019-01-17T15:10:00.000Z` |
| created | [1..1] | The date and time the appointment was initially created in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). | `2019-01-17T14:00:00.000Z` |
| description | [1..1] | Text describing the need for the appointment, to be shown for example in an appointment list. Note that developers should follow guidance for their use case as to appropriate content in this field. | 111 Referral |
| slot | [1..1] | The Slot that this appointment is booked into | [ { "reference": "#slot002" } ] |
| supportingInformation | [1..1] | Reference to a contained resource (see below) which describes an associated document, that SHOULD be present if available. | [ { "reference": "#123" } ] |
| incomingReferral| [0..1] | Optionally, the ReferralRequest provided as information to allocate to the Encounter. This may be ignored if not supported. Additional documentation can be added to the `supportingInformation` element.| [ { “reference”: “Referral123” } ]. |
| participant | [1..1] | A reference to a contained resource (see below) which describes the Patient for whom this Appointment is being booked, that SHOULD be present if available. Further participant types may be present. | "participant":  [ {            "actor": {        "reference": "#P1"            }        }    ]|



### Contained resources ###

If supported, the appointment resource **SHOULD** have three <a href='http://hl7.org/fhir/STU3/references.html#contained'>contained</a> resources. Note that contained resources are given an identifier which is only required to be unique within the scope of the containing resource, and are referenced using that identifier prefixed with a Hash '#' character. Some use cases may not support all three contained resources. This should be documented in system guidance.




## Key FHIR Elements for Cancelling ##

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>. **NB The appointment resource MUST be <a href='get_an_appointment.html'>retrieved from the Provider system</a> in order to ensure that no data is lost**.

See <a href='#cancelling-example-resource'>example resource</a>.

The following data items in the <a href='get_an_appointment.html'>retrieved Appointment</a> resource **MUST** be changed as defined:

| Name | Value | Description |
|---|---|---|
| status | `cancelled` | Indicates that the Appointment is being changed to a `cancelled` state. |

**No other elements of the Appointment resource may be changed**





## Booking example resource ##
```json
{
    "resourceType": "Appointment",
    "meta": {
        "profile":  [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
        ]
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
                "reference": "#P1"
            }
        }
    ]
}
```







## Registering example resource ##
```xml
<Appointment xmlns="http://hl7.org/fhir">
	<meta>
		<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
	</meta>
	<identifier>
		<system value="urn:ietf:rfc:3986"/>
		<value value="http://test.nhs.uk/test3"/>
	</identifier>
	<status value="booked"/>
	<start value="2019-01-17T15:00:00.000Z"/>
	<end value="2019-01-17T15:10:00.000Z"/>
	<created value="2019-01-17T14:00:00.000Z"/>
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
```






## Cancelling example resource ##
```json
{
    "resourceType": "Appointment",
    "meta": {
        "profile":  [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
        ]
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
    "status": "cancelled",
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
                "reference": "#P1"
            }
        }
    ]
}
```






## Cancelling registered appointment example resource ##

```xml
<Appointment xmlns="http://hl7.org/fhir">
	<id value="b7e99463-00a1-45fc-98aa-02301c103aba"/>
	<meta>
		<profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"/>
	</meta>
	<identifier>
		<system value="urn:ietf:rfc:3986"/>
		<value value="http://test.nhs.uk/test3"/>
	</identifier>
	<status value="cancelled"/>
	<start value="2019-02-01T10:51:23.620+00:00"/>
	<end value="2019-01-17T15:10:00.000Z"/>
	<created value="2019-01-17T14:00:00.000Z"/>
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
```
