---
title: Introduction to FHIR NHS Scheduling API
keywords: homepage
sidebar: overview_sidebar
permalink: index.html
toc: false
summary: A brief introduction to getting started with the FHIR NHS Scheduling API.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

# Introduction #
This API was previously called "FHIR-A2SI-APPOINTMENTS-API", it has been renamed to make it more closely reflect the purpose.

## In Scope ##
The initial scope is to book from NHS111 services into UTC services, however an attempt has been made to avoid constraints which limit the applicability of this specification. The specifics of this use case are <a href='https://developer.nhs.uk/apis/uec-appointments/'>detailed more fully here</a>.

## Out of Scope ##
This specification does not cover the transfer of the supporting information (currently a CDA) which describes the Patient's journey prior to the appointment.

## Quick Links ##

FHIR Profiles: <a href="resources_overview.html">NHS Scheduling FHIR Profiles</a><br/>
How to Search for a free slot: <a href="search_free_slots.html">Search free slots</a><br/>
How to Book an Appointment: <a href="book_an_appointment.html">Book an Appointment</a>

## Glossary ##

A full list of Glossary terms used by NHS Projects can be found at the <a href="https://developer.nhs.uk/library/glossary/" target="_blank">NHS Developer Network - Glossary</a>.