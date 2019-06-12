---
title: Patient | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: patient.html
summary: A Patient as used in Appointment booking.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is contained within the <a href='appointment.html'>Appointment resource</a>.

{% include custom/fhir.reference.html resource="Patient" page="CareConnect-Patient-1" fhirname="Patient" fhirlink="patient.html" content="-" userlink="" %}

## Key FHIR Elements ##

The following FHIR elements are key to this implementation :

See <a href='#example-resource'>example resource</a>.

The Patient resource **MUST** include the following data items:

| Element | Cardinality | Description | Example(s) |
| --- | --- | --- | --- |
| id | [1..1] | An id which is unique within the containing <a href='appointment.html'>Appointment</a> resource. | 123 |
| identifier | [1..1] | The Patient's NHS Number | ... |
| identifier.extension | [1..1] | A <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSNumberVerificationStatus-1'>Care Connect extension</a> that fixes this identifier to be an NHS Number | <a href='#example-resource'>see example</a> |
| identifier.use | [1..1] | Shows that this identifier is a nationally recognised one. | Fixed value: `official` |
| identifier.system | [1..1] | Shows that the value is an NHS Number | Fixed value: `https://fhir.nhs.uk/Id/nhs-number` |
| identifier.value | [1..1] | The Patient's NHS Number | `11122233333` |
| name | [1..1] | The Patient's name, as retrieved from PDS | <a href='#example-resource'>see example</a> |
| telecom | [1..1] | The number the patient can be contacted on | <a href='#example-resource'>see example</a> |
| gender | [1..1] | The patient's gender as retrieved from PDS | `male` \| `female` \| `other` \| `unknown` | |
| birthdate | [1..1 | The patient's gender as retrieved from PDS | 1974-12-31 |
| address | [1..1] | Patient's full address as retrieved from PDS | <a href='#example-resource'>see example</a> |

## Example resource ##
```json
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
```