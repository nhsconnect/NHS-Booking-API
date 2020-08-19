---
title: Organisation | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: organisation.html
summary: An organisation optionally linked to a Healthcare Service in Appointment booking.
---

{% include important.html content="This site is under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is optionally returned linked to a <a href='healthcare_service.html'>Healthcare Service resource</a> which is delivering a specific Schedule of Slots. For more clarification see <a href='resources_overview.html#urgent--emergency-care-appointments-apis'>the diagram on the FHIR Resources overview page</a>.

{% include custom/fhir.reference.html resource="Organization" page="CareConnect-Organization-1" fhirname="Organization" fhirlink="organisation.html" content="-" userlink="" %}


## Example resource ##
```json
{
    "resourceType": "Organization",
    "id": "2",
    "meta": {
        "profile": [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1"
        ]
    },
    "identifier": [
        {
            "system": "https://fhir.nhs.uk/Id/ods-organization-code",
            "value": "RTG"
        }
    ],
    "type": [
        {
            "coding": [
                {
                    "system": "http://hl7.org/fhir/organization-type",
                    "code": "prov",
                    "display": "Healthcare Provider"
                }
            ]
        }
    ],
    "name": "Derby Teaching Hospitals NHS Foundation Trust",
    "telecom": [
        {
            "system": "fax",
            "value": "01332 340131",
            "use": "work"
        }
    ],
    "address": [
        {
            "use": "work",
            "type": "both",
            "line": [
                "Uttoxeter Road"
            ],
            "city": "Derby",
            "postalCode": "DE22 3NE"
        }
    ]
}
```