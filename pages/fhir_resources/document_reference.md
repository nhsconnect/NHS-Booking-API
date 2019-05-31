---
title: DocumentReference | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: document_reference.html
summary: A DocumentReference which is used to point to a Document that contains information supporting an Appointment.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is contained within the <a href='appointment.html#contained-resources'>Appointment</a>, it is also described in the description of the <a href='appointment.html#documentreference'>Appointment</a> resource.

{% include custom/fhir.reference.html resource="DocumentReference" page="#" fhirname="DocumentReference" fhirlink="document_reference.html" content="-" userlink="" %}

A <a href='http://hl7.org/fhir/stu3/references.html#contained'>contained</a> DocumentReference resource which conforms to <b>TBC</b> profile.
This resource is referenced in the appointment's supportingInformation element, it describes the type and identifier(s) of any supporting information, for example a CDA document which is transferred separately.

The DocumentReference resource **MUST** include the following data items:

## Key FHIR Elements ##

| Name | Value | Description |
|---|---|---|
| id | Any | Any identifier, used to reference the resource from the `supportingInformation` element in the containing <a href='appointment.html'>Appointment</a> resource |
| identifier | see below | Identifies the supporting information (i.e. CDA document) |
| identifier.system | `https://tools.ietf.org/html/rfc4122` | Indicates that the associated value is a UUID. |
| identifier.value | [UUID] | The UUID of the associated CDA (XPath: `/ClinicalDocument/id/@root`) |
| status | "current" | Indicates that the associated document is current. No other value is expected. |
| type | A value from `urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17` | Indicates the type of document |
| content | see below | Describes the actual document |
| content.attachment | Describes the actual document |
| content.attachment.contentType | A valid mime type | Indicates the mime type of the document |
| content.attachment.language | `en` | States that the document is in English |

