---
title: Location | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: location.html
summary: A Location at which an Organisation delivers one or more Healthcare Services.
---

{% include important.html content="This is a live release of the NHS Booking API specification. However, prior to commencing development, please contact the [Booking & Referral Team.](mailto:bookingandreferrals@nhs.net)" %}

## Introduction ##
This resource is optionally returned linked to a <a href='healthcare_service.html'>Healthcare Service resource</a>. For more clarification see <a href='resources_overview.html#urgent--emergency-care-appointments-apis'>the diagram on the FHIR Resources overview page</a>.

Location may optionally be returned with Slots following a search for free Slots, however it is not considered important as normally the Location of the service would already have been identified and selected during the service discovery process.

{% include custom/fhir.reference.html resource="Location" page="CareConnect-Location-1" fhirname="HealthcareService" fhirlink="location.html" content="-" userlink="" %}

## Example resource ##
```json
{
    "resourceType": "Location",
    "id": "1",
    "meta": {
        "profile": [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Location-1"
        ]
    },
    "identifier": [
        {
            "use": "usual",
            "system": "https://fhir.nhs.uk/Id/ods-site-code",
            "value": "RTG08"
        }
    ],
    "name": "Long Eaton Clinic",
    "type": {
        "coding": [
            {
                "system": "http://hl7.org/fhir/v3/RoleCode",
                "code": "CSC",
                "display": "community service center"
            }
        ]
    },
    "telecom": [
        {
            "system": "phone",
            "value": "0115 855 4034",
            "use": "work"
        },
        {
            "system": "fax",
            "value": "0532 123 4567",
            "use": "work"
        }
    ],
    "address": {
        "use": "home",
        "type": "postal",
        "line": [
            "Midland Street",
            "Long Eaton"
        ],
        "city": "Nottingham",
        "district": "Derbyshire",
        "postalCode": "NG10 1RY"
    },
    "physicalType": {
        "coding": [
            {
                "system": "http://hl7.org/fhir/location-physical-type",
                "code": "bu",
                "display": "Building"
            }
        ]
    },
    "managingOrganization": {
        "reference": "Organization/2",
        "display": "Derby Teaching Hospitals NHS Foundation Trust"
    }
}
```