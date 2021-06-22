---
title: DocumentReference | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: document_reference.html
summary: A DocumentReference which is used to point to a Document that contains information supporting an Appointment.
---

{% include important.html content="This is a live release of the NHS Booking API specification. However, prior to commencing development, please contact the [Booking & Referral Team.](mailto:bookingandreferrals@nhs.net)" %}

## Introduction ##
This resource is contained within the <a href='appointment.html#contained-resources'>Appointment</a>, it is also described in the description of the <a href='appointment.html#documentreference'>Appointment</a> resource.

{% include custom/fhir.reference.html resource="DocumentReference" page="CareConnect-DocumentReference-1" fhirname="DocumentReference" fhirlink="document_reference.html" content="-" userlink="" %}

A <a href='http://hl7.org/fhir/stu3/references.html#contained'>contained</a> DocumentReference resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-DocumentReference-1'>CareConnect-DocumentReference-1</a> profile.
This resource is referenced in the appointment's supportingInformation element, it describes the type and identifier(s) of any supporting information, for example a CDA document which is transferred separately.

The DocumentReference resource **MUST** include the following data items:

## Key FHIR Elements ##

| Name | Value | Description |
|---|---|---|
| id | Any | An id which is unique within the containing Appointment resource. |
| identifier | see below | Identifies the supporting information (e.g. CDA document). |
| identifier.system | `https://tools.ietf.org/html/rfc4122` | Indicates that the associated value is a UUID. |
| identifier.value | string | The UUID of the associated document. |
| status | "current" | Indicates that the associated document is current. No other value is expected. |
| type | A value from `urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17` | Indicates the type of document. |
| content | see below | Describes the actual document. |
| content.attachment | Describes the actual document. |
| content.attachment.contentType | A valid mime type | Indicates the mime type of the document. |
| content.attachment.language | `en` | States that the document is in English. |

