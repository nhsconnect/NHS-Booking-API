---
title: Overview
keywords: homepage
sidebar: overview_sidebar
permalink: developing.html
toc: false
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Versioning of Appointment resources ##
In order to prevent any loss of data, Appointment resources are protected against concurrency problems, as <a href='http://hl7.org/fhir/stu3/http.html#concurrency'>described in the FHIR standard</a>. The use of a version identifier for each resource prevents that resource being a newer version of that resource being unwittingly overwritten.

## Use of the SSP and associated HTTP Headers ##
All calls to Provider systems are made through the Spine Secure Proxy (SSP). This gives a number of benefits:
- It allows Provider systems to only allow inbound connections from a known address (i.e. the SSP), and also means that Consumer systems only need to make connections to a single network address.
- It simplifies the setup of TLS/MA (Transport Level Security with Mutual Authentication) so each system (Consumer and Provider) only needs to be authenticated to the SSP.

It does however require some minor additional overhead:
- The URL of the SSP **MUST** be concatenated with the Provider system URL.
- Custom HTTP headers **MUST** be included in all resuests to Provider systems.

### Concatenating the URLs ###
The actual URL of the SSP will depend on the environment in use, therefore an artificial URL will be used here for demonstration purposes.

Example SSP URL:
```
https://testspineproxy.nhs.uk/
```

Example Provider URL being called:
```
https://provider.nhs.uk/STU3/Appointment/
```
The concatenated URL would be:
```
https://testspineproxy.nhs.uk/https://provider.nhs.uk/STU3/Appointment/
```

Consumers **MUST** take care to ensure that one and only one **'/'** character is included between the SSP URL and the Provider URL.


### HTTP Headers ###
The following http headers are required by the SSP. **NB These are in addition to** the JWT required in an Authorization header <a href='/process.html#get-token'>as described elsewhere</a>.


| Key | Value | Description
| --- | --- | --- |
| Ssp-TraceID | A UUID | A traceable Identifier which can later be used at both Consumer and Provider to trace back to this request. This UUID **SHOULD** be unique to either the individual http request being made, but at a a minimum **MUST** be unique to the Patient call being handled. |
| Ssp-From | An ASID | The ASID value which has been assigned by NHS Digital to the Consumer system. |
| Ssp-To | An ASID | The ASID value of the Provider system being called. |
| Ssp-InteractionID | One of the Interaction IDs shown below. | The Interaction ID retrieved from SDS and described below. |

## Interaction IDs ##

| Value | Description and usage |
| --- | --- |
| `urn:nhs:names:services:careconnect:fhir:rest:read:metadata` | Used when sending a request through the SSP to a Provider system to retrieve the <a href='https://www.hl7.org/fhir/stu3/capabilitystatement.html'>CapabilityStatement resource, which describes the server's capabilities</a>. |
| `urn:nhs:names:services:careconnect:fhir:rest:search:slot` | Used when <a href='search_free_slots.html'>requesting available Slots</a> from a Provider system. |
| `urn:nhs:names:services:careconnect:fhir:rest:create:appointment` | Used when POSTing a new Appointment resource to a Provider in order to <a href='book_an_appointment.html'>book an appointment</a>. |
| `urn:nhs:names:services:careconnect:fhir:rest:search:appointment` | Used when <a href='search_patient_appointments.html'>searching the Registry</a> for Appointments for a specific Patient. |
| `urn:nhs:names:services:careconnect:fhir:rest:read:appointment` | Used when <a href='get_an_appointment.html'>reading an existing Appointment</a>. |
| `urn:nhs:names:services:careconnect:fhir:rest:update:appointment` | Used when updating an existing appointment, for example when <a href='cancel_an_appointment.html'>setting it to Cancelled</a>. |


## Provider Demonstrator ##
To support development, a <a href='http://appointments.directoryofservices.nhs.uk:443/poc/index'>synthetic Provider system</a> has been created. This allows development to proceed without being tightly coupled to the timescales of another system supplier.

## Validating resources ##
FHIR resources used in this specification can be valiated against their profiles <a href='https://data.developer.nhs.uk/ccri/term/validate'>using this site</a>, alternatively the resources can be POSTed to: https://data.developer.nhs.uk/ccri-fhir/STU3/[ResourceType]/$validate (making sure to set [ResourceType] to the appropriate type of Resource) using a REST client (such as <a href='https://www.getpostman.com/'>POSTman</a>).

## JWT utilities ##
Once a JWT has been created, there are a couple of useful public resources for decoding them, <a href='https://jwt.io/'>jwt.io</a> is useful, however <a href='http://jwt.ms/'>Jwt.ms</a> is slightly more user friendly.
