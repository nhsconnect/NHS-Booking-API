---
title: Registry Overview
keywords: homepage
sidebar: overview_sidebar
permalink: registry_overview.html
toc: false
summary: An overview of the Appointment Registry
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

# Background #
In order to support the ability for any service to be able to find and subsequently update or cancel an existing Appointment which is booked for a Patient, Appointments will be indexed in a registry. This registry will then support <a href='search_patient_appointments.html'>searching for Appointments for a given Patient</a>, returning an Appointment resource, which has an Identifier that acts as a Pointer to the original Appointment resource. The details of this Appointment resource holding a pointer can be seen on the <a href='appointment.html#key-fhir-elements-for-registering'>descripton of the Appointment resource</a> for registering an Appointment.

## Sequence Diagram ##

The process for registering an Appointment in the Registry is shown below.

<img src="images/UEC-Appointments/Registering.png">


### Get Token ###
This is the process used by an assured system to get an access token which can be used by the Registry to verify the identity and assurance status of the calling system.

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
- `client_id` will be a static value, provided by NHS Digital following assurance.
- `client_secret` will be a static (but will be rotated on a periodic basis) value, provided by NHS Digital following assurance.
- `grant_type` will be a fixed value of `client_credentials`
- `scope` will be the Registry value suffixed with `/.default`

The response to this will be something like:
```json
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "ext_expires_in": 3600,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IkhCeGw5bUFlNmd4YXZDa2NvT1UyVEhzRE5hMCIsImtpZCI6IkhCeGw5bUFlNmd4YXZDa2NvT1UyVEhzRE5hMCJ9.eyJhdWQiOiJodHRwOi8vYXBwb2ludG1lbnRzLmRpcmVjdG9yeW9mc2VydmljZXMubmhzLnVrOjQ0My9wb2MiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9lNTIxMTFjNy00MDQ4LTRmMzQtYWVhOS02MzI2YWZhNDRhOGQvIiwiaWF0IjoxNTU3NDA4ODA0LCJuYmYiOjE1NTc0MDg4MDQsImV4cCI6MTU1NzQxMjcwNCwiYWlvIjoiNDJaZ1lGaVpYUEYwYWF2NG1qTnRPckpHakVmZEFBPT0iLCJhcHBpZCI6IjkyZDg1ZjlkLTA2NjYtNDliYy1hMzFjLTEyYjQ1YjA0YTdkZSIsImFwcGlkYWNyIjoiMSIsImdyb3VwcyI6WyJhYjQxMmZlOS0zZjY4LTQzNjgtOTgxMC05ZGMyNGQxNjU5YjEiLCJkYWNiODJjNS1hZWE4LTQ1MDktODg3Zi0yODEzMjQwNjJkZmQiLCI1NWRhMWM5YS0yY2ViLTQxYTctOWI3Yy0xYzcwMTVlZDFmZGUiXSwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTUyMTExYzctNDA0OC00ZjM0LWFlYTktNjMyNmFmYTQ0YThkLyIsIm9pZCI6IjU4YjI1Zjk5LWQ0MDQtNDQ4My1hNjBhLTQwNmYxY2MzYzg4NyIsInN1YiI6IjU4YjI1Zjk5LWQ0MDQtNDQ4My1hNjBhLTQwNmYxY2MzYzg4NyIsInRpZCI6ImU1MjExMWM3LTQwNDgtNGYzNC1hZWE5LTYzMjZhZmE0NGE4ZCIsInV0aSI6IjJnVG5HSzBUcEVLQThVOHh3Y19RQUEiLCJ2ZXIiOiIxLjAifQ.S6j6S1SHUM6HkefibMh2nAErVAREAAKLx2iafPVRXFzFOEKQ2Z-QXJK2DdpCbnx1k_rvtM2kNfZPfC6y7OjxesuuW0DF04ufEQTnGhT-iwwZ5BWicEzW7B8mB9-YBeYmhU641YglG9AwjK_OikluFoVRR4UY68nRrjxkKa_dgpSysoWuUYrveHyV37Osi74wO4rZ5qiFkd2j8lLdKwxqb0fb-p5RozxSVBaLraWFJeBAIN40yNk4lQ4f4NGuhbdb-ny_9o7R0hmW2dMfuSsUTVGTk7llxpHpFvWX4I7cFTPwu3c3Z0dy1rojx_iZFERjml4xUIv9QQHQOqJWU2ToLA"
}
```

Requests to The Registry **MUST** include a http header as below (using the above token as an example) with a Key of Authorization and a value of `Bearer` + the value in `access_token` from the above response:

| Key | Value |
| --- | --- |
| Authorization | Bearer eyJ0eXAi... shortened for display ...QHQOqJWU2ToLA |

### Register ###
This is the process of recording in a central registry the details of the appointment that was booked. This is to support subsequent location of that appointment in case it needs to be cancelled It is <a href='register_an_appointment.html'>described in detail here</a>.

## Quick Links ##

FHIR Profiles: <a href="resources_overview.html">NHS Scheduling FHIR Profiles</a><br/>
How to Search for a free slot: <a href="search_free_slots.html">Search free slots</a><br/>
How to Book an Appointment: <a href="book_an_appointment.html">Book an Appointment</a>

## Glossary ##

A full list of Glossary terms used by NHS Projects can be found at the <a href="https://developer.nhs.uk/library/glossary/" target="_blank">NHS Developer Network - Glossary</a>.