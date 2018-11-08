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

This API is still in development, it has been designed around an Urgent & Emergency Care scenario.

## In Scope ##

The initial requirement is to find appointment Slots and create an Appointment in a free slot. The interactions in scope are: search Slot (a RESTful search) and create Appointment.

## Out of Scope ##

The implementation does not cover Read, Update or Delete for an Appointment, however this functionality is expected in future iterations of the API

## Quick Links ##

FHIR Profiles: <a href="explore.html">NHS Scheduling FHIR Profiles</a><br/>
How to Search for a free slot: <a href="search_free_slots.html">Search free slots</a><br/>
How to Book an Appointment: <a href="book_an_appointment.html">Book an Appointment</a>