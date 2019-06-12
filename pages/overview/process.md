---
title: Overall Process
keywords: homepage
sidebar: overview_sidebar
permalink: process.html
toc: false
summary: An overview of the end to end booking process
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

# Sequence Diagram #

The end to end process includes some existing processes, as well as new processes. The sequence of events is shown in the below diagram.

<img src="images/UEC-Appointments/ApptBookingOverview.png">

The interactions are detailed further here:

### GetServices ###
This is existing process, using a CheckCapacitySummary query to the DoS, this is detailed in <a href='https://developer.nhs.uk/apis/dos-api/'>a separate specification</a>.

### Select ###
This is the process of the Consuming system user and the Patient agreeing on the most suitable service, and is outside the scope of this specification.

### Find Endpoint ###
This is the process used in order to find the techncal endpoint (or URL) of the FHIR server to be used in order to book appointments for the selected Service.
This uses two LDAP queries to SDS, using the ASID from the selected DoS Service.

NB: The examples provided here are described against SDS in OpenTest.

The first query is to retrieve the nhsAS Object and takes the form:
```ldap
ldapsearch -x -H ldap://192.168.128.11 -b "ou=services, o=nhs" "(uniqueIdentifier=[ASID-Value])"
```

The response will be something like this:
```
# extended LDIF
#
# LDAPv3
# base <ou=services, o=nhs> with scope subtree
# filter: (uniqueIdentifier=918999199203)
# requesting: ALL
#

# 918999199203, Services, nhs
dn: uniqueIdentifier=918999199203,ou=Services,o=nhs
nhsApproverURP: uniqueIdentifier=283886066515,uniqueIdentifier=747394079517,uid=828249614516,ou=people,o=nhs
nhsAsSvcIA: urn:nhs:names:services:psisquery:QUPC_IN160101UK05

~~~ Lots of rows removed here ~~~
nhsAsSvcIA: urn:nhs:names:services:careconnect:fhir:rest:create:appointment
nhsAsSvcIA: urn:nhs:names:services:careconnect:fhir:rest:read:slot
~~~ Lots of rows removed here ~~~

nhsAsSvcIA: urn:nhs:names:services:careconnect:fhir:rest:read:appointment
nhsDateApproved: 20151106145512
nhsDateRequested: 20151106145347
nhsIDCode: A91342
uniqueIdentifier: 918999199203
nhsMHSPartyKey: A91342-9199203
nhsRequestorURP: uniqueIdentifier=331622135514,uniqueIdentifier=747394079517, uid=828249614516,ou=people,o=nhs
nhsAsACF: CNST
nhsAsACF: DOLR
nhsAsACF: RBAC
nhsAsClient: A91342
nhsProductKey: 7374
objectClass: top
objectClass: nhsAs

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

The important part of the above is the **nhsMHSPartyKey** value.

The second query is to retrieve the nhsMHS object, and takes the form:
```ldap
ldapsearch -x -H ldap://192.168.128.11 -b "ou=services, o=nhs" "(&(nhsMhsPartyKey=[nhsMHSPartyKey goes here])(nhsMhsSvcIA=urn:nhs:names:services:careconnect:fhir:rest:read:slot))"
```

The response will be something like this:
```
# extended LDIF
#
# LDAPv3
# base <ou=services, o=nhs> with scope subtree
# filter: (&(nhsMhsPartyKey=A91342-9199203)(nhsMhsSvcIA=urn:nhs:names:services:careconnect:fhir:rest:read:slot))
# requesting: ALL
#

# S918999494042, Services, nhs
dn: uniqueIdentifier=S918999494042,ou=Services,o=nhs
nhsDateRequested: 20140718000000
nhsMHsSN: urn:nhs:names:services:careconnect
nhsMHSIsAuthenticated: none
nhsDateApproved: 20180201115957
nhsApproverURP: uniqueidentifier=555050304105,uniqueidentifier=555008548101,uid=555008545108,ou=people, o=nhs
nhsDateDNSApproved: 20180201115957
nhsMHSAckRequested: never
nhsDNSApprover: uniqueidentifier=555050304105,uniqueidentifier=555008548101,uid=555008545108,ou=people, o=nhs
nhsContractPropertyTemplateKey: 14
nhsProductKey: 11080
nhsMhsFQDN: vpn-client-1292.opentest.hscic.gov.uk
objectClass: nhsMhs
objectClass: top
uniqueIdentifier: S918999494042
nhsEPInteractionType: FHIR
nhsRequestorURP: SYSTEM
nhsMHSEndPoint: https://vpn-client-1292.opentest.hscic.gov.uk/
nhsMhsCPAId: S918999494042
nhsMHSPartyKey: A91342-9199203
nhsMHsIN: fhir:rest:create:appointment
nhsIDCode: A91342
nhsMhsSvcIA: urn:nhs:names:services:careconnect:fhir:rest:read:slot
nhsMHSDuplicateElimination: never
nhsMHSSyncReplyMode: None
nhsMHSServiceDescription: OpenTest vpn-client-1292.opentest.hscic.gov.uk

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```
The important part of the above is the **nhsMHSEndPoint** value.

### Get Token ###
This is the process used by an assured system to get an access token which can be used by the Provider system to verify the identity and assurance status of the Consuming system.

To retrieve this JWT token, the following fields are POSTed to the **token endpoint**. This is **TBC**

```
POST  HTTP/1.1
Host: {{tokenEndpoint}}
Content-Type: application/x-www-form-urlencoded
Accept: */*
Host: {TBC}
content-length: 219
Connection: keep-alive

client_id=[see below]
&client_secret=[see below]
&grant_type=client_credentials
&scope=[see below]
```
- `client_id` will be a static value, provided to the Consumer system by NHS Digital following assurance.
- `client_secret` will be a static (but will be rotated on a periodic basis) value, provided to the Consumer system by NHS Digital following assurance.
- `grant_type` will be a fixed value of `client_credentials`
- `scope` will be the nhsMHSEndPoint value suffixed with `/.default`

The response to this will be something like:
```
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "ext_expires_in": 3600,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IkhCeGw5bUFlNmd4YXZDa2NvT1UyVEhzRE5hMCIsImtpZCI6IkhCeGw5bUFlNmd4YXZDa2NvT1UyVEhzRE5hMCJ9.eyJhdWQiOiJodHRwOi8vYXBwb2ludG1lbnRzLmRpcmVjdG9yeW9mc2VydmljZXMubmhzLnVrOjQ0My9wb2MiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9lNTIxMTFjNy00MDQ4LTRmMzQtYWVhOS02MzI2YWZhNDRhOGQvIiwiaWF0IjoxNTU3NDA4ODA0LCJuYmYiOjE1NTc0MDg4MDQsImV4cCI6MTU1NzQxMjcwNCwiYWlvIjoiNDJaZ1lGaVpYUEYwYWF2NG1qTnRPckpHakVmZEFBPT0iLCJhcHBpZCI6IjkyZDg1ZjlkLTA2NjYtNDliYy1hMzFjLTEyYjQ1YjA0YTdkZSIsImFwcGlkYWNyIjoiMSIsImdyb3VwcyI6WyJhYjQxMmZlOS0zZjY4LTQzNjgtOTgxMC05ZGMyNGQxNjU5YjEiLCJkYWNiODJjNS1hZWE4LTQ1MDktODg3Zi0yODEzMjQwNjJkZmQiLCI1NWRhMWM5YS0yY2ViLTQxYTctOWI3Yy0xYzcwMTVlZDFmZGUiXSwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTUyMTExYzctNDA0OC00ZjM0LWFlYTktNjMyNmFmYTQ0YThkLyIsIm9pZCI6IjU4YjI1Zjk5LWQ0MDQtNDQ4My1hNjBhLTQwNmYxY2MzYzg4NyIsInN1YiI6IjU4YjI1Zjk5LWQ0MDQtNDQ4My1hNjBhLTQwNmYxY2MzYzg4NyIsInRpZCI6ImU1MjExMWM3LTQwNDgtNGYzNC1hZWE5LTYzMjZhZmE0NGE4ZCIsInV0aSI6IjJnVG5HSzBUcEVLQThVOHh3Y19RQUEiLCJ2ZXIiOiIxLjAifQ.S6j6S1SHUM6HkefibMh2nAErVAREAAKLx2iafPVRXFzFOEKQ2Z-QXJK2DdpCbnx1k_rvtM2kNfZPfC6y7OjxesuuW0DF04ufEQTnGhT-iwwZ5BWicEzW7B8mB9-YBeYmhU641YglG9AwjK_OikluFoVRR4UY68nRrjxkKa_dgpSysoWuUYrveHyV37Osi74wO4rZ5qiFkd2j8lLdKwxqb0fb-p5RozxSVBaLraWFJeBAIN40yNk4lQ4f4NGuhbdb-ny_9o7R0hmW2dMfuSsUTVGTk7llxpHpFvWX4I7cFTPwu3c3Z0dy1rojx_iZFERjml4xUIv9QQHQOqJWU2ToLA"
}
```

Requests to Provider systems **MUST** include a http header as below (using the above token as an example) with a Key of Authorization and a value of `Bearer ` + the value in `access_token` from the above response:

| Key | Value |
| --- | --- |
| Authorization | Bearer eyJ0eXAi... shortened for display ...QHQOqJWU2ToLA |

NB: This is in addition to <a href='developing.html#use-of-the-ssp-and-associated-http-headers'>http request headers required by the SSP</a>.

### Get Slots ###
This is the process to find availability at the selected service, it is <a href='search_free_slots.html'>described in detail here</a>.

### Select ###
This is the process of the Consuming system user and the Patient agreeing on the most suitable Slot, and is outside the scope of this specification.

### Book ###
This is the process of sending a booked appointment, including details of the Patient and an associated (CDA) document from the Consuming system to the Provider system, and is <a href='book_an_appointment.html'>described in detail here</a>.

## Registering the Appointment ##
As described booked Appointments subsequently need to be<a href='registry.html'>registered in the Registry</a>.

## Quick Links ##

FHIR Profiles: <a href="resources_overview.html">NHS Scheduling FHIR Profiles</a><br/>
How to Search for a free slot: <a href="search_free_slots.html">Search free slots</a><br/>
How to Book an Appointment: <a href="book_an_appointment.html">Book an Appointment</a>

## Glossary ##

A full list of Glossary terms used by NHS Projects can be found at the <a href="https://developer.nhs.uk/library/glossary/" target="_blank">NHS Developer Network - Glossary</a>.