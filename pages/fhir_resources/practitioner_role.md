---
title: Practitioner Role | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: practitioner_role_old.html
summary: A practitioner role optionally linked to a Schedule in Appointment booking.
---

{% include important.html content="This is a live release of the NHS Booking API specification. However, prior to commencing development, please contact the [Booking & Referral Team.](mailto:bookingandreferrals@nhs.net)" %}

## Introduction ##
This resource is optionally returned linked to a <a href='schedule.html'>Schedule resource</a>. It is used to describe the Professional Role of the Practitioner delivering that Schedule. It represents a specific set of Roles/Locations/specialties/services that a practitioner may perform at an organization for a period of time.

{% include custom/fhir.reference.html resource="PractitionerRole" page="CareConnect-PractitionerRole-1" fhirname="PractitionerRole" fhirlink="practitionerrole.html" content="-" userlink="" %}

## Example resource ##
```json
{
    "resourceType": "PractitionerRole",
    "id": "1",
    "practitioner": {
        "reference": "Practitioner/1",
        "display": "Dr. AA Bhatia"
    },
    "organization": {
        "reference": "Organization/1",
        "display": "The Moir Medical Centre"
    },
    "code": [
        {
            "coding": [
                {
                    "system": "https://fhir.hl7.org.uk/STU3/CodeSystem/CareConnect-SDSJobRoleName-1",
                    "code": "R0260",
                    "display": "General Medical Practitioner"
                }
            ]
        }
    ]
}
```
