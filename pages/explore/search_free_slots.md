---
title: Search for free slots
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: search_free_slots.html
summary: "Details the Search for free slots functionality required"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A consuming system requests slots from a provider system matching the selected date/time range and for a specific service.

## Security ##

- utilises TLS Mutual Authentication for system level authorization.
- utilises a JSON Web Token (JWT) to transmit clinical Consumer system identity and authorisation details.

## Search parameters ##

Provider systems SHALL support the following search parameters that be passed to the API:

| Name | Type | Description | Paths |
|---|---|---|---|
| `status` | `token` | The free/busy status of the slots | `Slot.status` |
| `start` | `date` | Slot start date/time. | `Slot.start` |
| `end` | `date` | Slot end date/time. | `Slot.end` |
| `HealthcareService id` | `Id` | The id of the HealthcareService related to the Schedule/Slot | `HealthcareService.id` |

## _include parameters ##

Provider systems SHALL support the following include parameters:

| Name | Description | Paths |
|---|---|---|
| `_include=Slot:schedule` | Include Schedule Resources referenced within the returned Slot Resources | `Slot.schedule` |
| `_include:recurse= Schedule:actor:Practitioner` | Include Practitioner Resources referenced within the returned Schedule Resources | `Schedule.actor:Practitioner` |
| `_include:recurse= Schedule:actor:PractitionerRole` | Include Practitioner Role Resources referenced within the returned Schedule Resources | `Schedule.actor:PractitionerRole` |
| `_include:recurse= Schedule:actor:HealthcareService` | Include HealthcareService Resources referenced within the returned Schedule Resources | `Schedule.actor:HealthcareService` |
| `_include:recurse= HealthcareService:providedBy:Organization` | Include Organization Resources referenced within the returned HealthcareService Resources | `HealthcareService.providedBy:Organization` |
| `_include:recurse= HealthcareService:location:Location` | Include Location Resources referenced within the returned HealthcareService Resources | `HealthcareService.location:Location` |

The following parameters SHALL be included in the request:

- The `start` parameter SHALL be included once in the request.
- The `start` parameter SHALL be supplied with the `le` search prefix. For example, 'start=le2018-01-02T09:15:30:00Z', which indicates that the consumer would like slots where the slot start date/time is at or before "2018-01-02T09:15:30Z".
- Slots that have already started and end after the start parameter should be included in the response.

<img src="images/UEC-Appointments/Slot-timings.png">

## Prerequisites ##

## API usage ##

#### Error handling ####

The provider system SHALL return an error if:

- the `status` parameter is absent or is present with a value other than `free`

SHALL return an `Operation Outcome` resource that provides additional detail when one or more parameters are corrupt or a specific business rule/constraint is breached.

#### Payload response body ####

Provider systems:

- SHALL return a `200` **OK** HTTP status code on successful retrieval of "free" slot details.
- SHALL include the free `Slot` details for the organisation which have a `status` of "free" and meet the requested date/time criteria.
 
  The response `Bundle` SHALL only contain `Schedule`, `HealthcareService`, `Organization`, `Location`, `Practitioner` and `PractitionerRole` resources related to the returned free `Slot` resources. If no free slots are returned for the requested time period then no resources should be returned within the response `Bundle`.

```json
{
  "resourceType": "Bundle",
  "type": "searchset",
  "contained": [
    {
      	"resourceType": "HealthcareService",
	"id": "example",
	"identifier": [
		{
		"system": "http://example.org/shared-ids",
		"value": "HS-12"
		}
	],
	"active": true,
	"providedBy": {
		"reference": "Organization/f001",
		"display": "LGI - Heart Centre Jubilee"
	},
	"location": [
		{
		"reference": "Location/1"
		}
	],
	"name": "Leeds General Infirmary Heart Centre"
	}
	]
  "entry": [
    {
      "fullUrl": "Schedule/14",
      "resource": {
        "resourceType": "Schedule",
        "id": "14",
        "meta": {
          "versionId": "1469444400000",
          "profile": [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Schedule-1"
          ]
        },        
        "actor": [
          {		  
          {
            "reference": "HealthcareService/79"
          },
          {
            "reference": "Practitioner/2"
          },
          {
            "reference": "PractitionerRole/1"
          }
        ],
        "comment": "Schedule 1 for general appointments"
      }
    },
    {
      "fullUrl": "Practitioner/2",
      "resource": {
        "resourceType": "Practitioner",
        "id": "2",
        "meta": {
          "versionId": "636064088099800115",
          "profile": [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1"
          ]
        },
        "identifier": [
          {
            "system": "https://fhir.nhs.uk/Id/sds-user-id",
            "value": "S001"
          }
        ],
        "name": {
          "family": [ "Black" ],
          "given": [ "Sarah" ],
          "prefix": [ "Mrs" ]
        },
        "gender": "female"
      }
    },
    {
      "fullUrl": "Slot/1584",
      "resource": {
        "resourceType": "Slot",
        "id": "1584",
        "meta": {
          "versionId": "1471219260000",
          "profile": [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/NHS-Slot-1"
          ]
        },        
        "schedule": {
          "reference": "Schedule/14"
        },
        "status": "free",
        "start": "2016-08-15T11:30:00.000+01:00",
        "end": "2016-08-15T11:59:59.000+01:00"
      }
    },
    {
      "fullUrl": "Slot/1644",
      "resource": {
        "resourceType": "Slot",
        "id": "1644",
        "meta": {
          "versionId": "1471219260112",
          "profile": [
            "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Slot-1"
          ]
        },        
        "schedule": {
          "reference": "Schedule/14"
        },
        "status": "free",
        "start": "2016-08-15T12:00:00.000+01:00",
        "end": "2016-08-15T12:29:59.000+01:00"
      }
    }
  ]
}
```