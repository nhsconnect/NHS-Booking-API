---
title: Introduction to FHIR NHS Booking API
keywords: homepage
sidebar: overview_sidebar
permalink: index.html
toc: false
summary: A brief introduction to getting started with the FHIR NHS Booking API.
---

{% include important.html content="As of 25 March 2022, this API is [deprecated](https://digital.nhs.uk/developer/guides-and-documentation/reference-guide#api-status), and will be retired in 12 t0 18 months. It is still available for use by in-flight development but not by new integrations. For details contact the [Booking & Referral Team.](mailto:bookingandreferrals@nhs.net)" %}

# Introduction #
This API was previously called "FHIR NHS Scheduling API", it has been renamed to make it more closely reflect the purpose.

## In Scope ##
The initial scope is to book from NHS111 services into UTC services, however an attempt has been made to avoid constraints which limit the applicability of this specification. The specifics of this use case are <a href='https://developer.nhs.uk/apis/uec-appointments/'>detailed more fully here</a>.

## Out of Scope ##
This specification does not cover the transfer of the supporting information (currently a CDA) which describes the Patient's journey prior to the appointment.

## Quick Links ##

FHIR Profiles: <a href="resources_overview.html">NHS Booking FHIR Profiles</a><br/>
How to Search for Slots: <a href="search_slots.html">Search Slots</a><br/>
How to Book an Appointment: <a href="book_an_appointment.html">Book an Appointment</a>

## Glossary ##

A full list of the Glossary terms used by NHS Projects can be found at the <a href="https://developer.nhs.uk/library/glossary/" target="_blank">NHS Developer Network - Glossary</a>.
