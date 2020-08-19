---
title: Practitioner | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: practitioner.html
summary: A practitioner optionally linked to a Slot in Appointment booking.
---

{% include important.html content="This site is under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is optionally returned linked to a <a href='slot.html'>Slot resource</a>.

{% include custom/fhir.reference.html resource="Practitioner" page="CareConnect-Practitioner-1" fhirname="Practitioner" fhirlink="practitioner.html" content="-" userlink="" %}

## Example resource ##
```json
{
    "resourceType": "Practitioner",
    "id": "1",
    "meta": {
        "profile": [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1"
        ]
    },
    "identifier": [
        {
            "system": "https://fhir.nhs.uk/Id/sds-user-id",
            "value": "G8133438"
        }
    ],
    "name": [
        {
            "family": "Bhatia",
            "given": [
                "AA"
            ],
            "prefix": [
                "Dr."
            ]
        }
    ],
    "telecom": [
        {
            "system": "email",
            "value": "abhatia@nhs.skynet",
            "use": "work"
        },
        {
            "system": "phone",
            "value": "0115 9737320",
            "use": "work"
        }
    ],
    "address": [
        {
            "line": [
                "Regent Street",
                "Long Eaton"
            ],
            "city": "Nottingham",
            "district": "Derbyshire",
            "postalCode": "NG10 1QQ"
        }
    ],
    "gender": "male"
}
```