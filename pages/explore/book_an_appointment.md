---
title: Book an Appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: book_an_appointment.html
summary: "Details the Book an Appointment functionality required"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

{% include important.html content="The Appointment Management capability pack is aimed at administration of a patient’s appointments. Initially the capability has been restricted to booking appointments." %}

## Prerequisites ##

 1. The consumer SHALL have previously traced the patient’s NHS Number using the Personal Demographics Service or an equivalent service.
 2. Identify the HealthcareService Id.
 3. Identify the FHIR endpoint of the HealthcareService.
- i.e. the consumer SHALL have previously resolved the organisation's FHIR&reg; endpoint base URL through the [Spine Directory Service](https://nhsconnect.github.io/gpconnect/integration_spine_directory_service.html)
 
## Process/flow ##

  1. Create an `Appointment` for the chosen `Slot` and `Patient` resources.

## Security ##

- utilises TLS Mutual Authentication for system level authorization.
- utilises a JSON Web Token (JWT) to transmit clinical Consumer system identity and authorisation details.

### Request operation ###

#### FHIR&reg; relative request ####

```http
POST /Appointment
```

#### Payload request body ####

Consumer systems:
- SHALL send an `Appointment` resource that conforms to the [CareConnect-Appointment-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1) profile.

The following data elements are mandatory (that is, data MUST be present):
 
- a participant with an actor of type patient, providing details of the patient.
- the status identifying the appointment as “booked”.
- the slot reference(s) as supplied by the provider system of one or more slots to be booked.


#### Error handling ####

Provider systems:

- SHALL return a http status "422" with an error message "DUPLICATE_REJECTED" when an appointment can not be booked because the referenced slots within the appointment resource no longer have the status `free`, if for example the slot has already been booked since it was returned as free from a “search for free slots”.
- SHALL return an `Operation Outcome` resource that provides additional detail when one or more request fields are corrupt or a specific business rule/constraint is breached.

For example:

- one or more of the requested `Slot` resources does not exist or already has a `status` of busy (the selected Slot may have been booked since the Slot was last returned).

### Request response ###

#### Response headers ####

Provider systems are not expected to add any specific headers beyond that described in the HTTP and FHIR&reg; standards.

#### Payload response body ####

Provider systems:

- SHALL return a `201` **Created** HTTP status code on successful execution of the operation.
- SHALL return an `Appointment` resource that conform to the [CareConnect-Appointment-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1) profile.

...

```json
{
  "resourceType": "Appointment",
  "id": "123123",
  "meta": {
    "profile": [
      "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
    ]
  },
  "status": "booked",
  "description": "Free text description.",
  "slot": [
    {
      "reference": "Slot/1",
      "display": "Slot 1"
    }
  ],
  "created": "2017-10-09T13:48:41+01:00",
  "comment": "Free text comment.",
  "participant": [
    {
      "actor": {
        "reference": "Patient/1",
        "display": "Mr. Mike Smith"
      },
      "status": "booked"
    }
  ]
}
```