---
title: Bundle | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: bundle.html
summary: A container of resources.
---

{% include important.html content="This site is under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is used to 'bundle' a set of resources returned that meet some criteria as part of a search.

{% include custom/fhir.reference.nonecc.html fhirname="Bundle" fhirlink="http://hl7.org/fhir/stu3/bundle.html" content="-" userlink="" %}

## Key FHIR Elements ##

The following FHIR elements are key to this implementation :


| Element | Cardinality | Description | Example(s) |
| --- | --- | --- | --- |
| id | [0..1] | A unique id which identifies a bundle. The id can be a globally unique identifier (e.g. UUID).| a56382a1-dce2-483a-8426-65b727b66809 |
| type | [0..1] | Purpose of the bundle | `searchset` |
| total  | [0..1] | The total number of matches. | 23 |
| link | [0..*] | Links related to this Bundle | |
| link.relation | [1..1] ||`self`|
| link.url| [1..1] |Reference details for the link.||
| entry | [0..*] | Entry in the bundle. This will be a resource. | |
| entry.fullUrl | [0..1] | Links related to this entry. ||
| entry.resource |[0..1] | Absolute URL for resource (server address, or UUID/OID).||


### Examples ###

This specification includes examples using the Bundle resource as an output for search results. They can be accessed at:

- [Search for Slots](search_slots.html#sample-response)

- [Search Patient Appointments](search_patient_appointments.html#sample-response)