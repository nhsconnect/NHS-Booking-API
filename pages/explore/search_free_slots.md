---
title: Search for free slots
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: search_free_slots.html
summary: "Details the Search for free slots interaction"
---

{% include important.html content="Site under development by NHS Digital, it is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A consuming system requests available slots from a provider system matching the selected date/time range and for a specific service.

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Is routed through the SSP (Spine secure Proxy)
- Utilises TLS Mutual Authentication for system level authentication.

## Search parameters ##

Servers **MUST** support the following search parameters that MAY be passed to the API. In some use cases clients may not be able to support the complete parameter list, so may omit unsupported parameters (so Servers must cater for parameters not being present).

| Name | Type | Description | Paths |
|---|---|---|---|
| `service` | `token` | The appropriate service id of the service for which Slots are being requested | `schedule.actor:healthcareservice` |
| `status` | `token` | The free/busy status of the slots | `status` |
| `start` | `dateTime` | Slot start date/time. A `dateTime` or `Instant` in the format `yyyy-mm-ddThh:mm:ss+hh:mm`. | `start` |
| `start` | `dateTime` | Slot start date/time. A `dateTime` or `Instant` in the format `yyyy-mm-ddThh:mm:ss+hh:mm`. | `start` |

**NB start is included twice to allow for the definition of the start and end of 'The time window within which Slots must start'.**
**Additional query parameters MAY be used. However, if these are not supported by the Server, they SHOULD be ignored.**

## _include parameters ##

Provider systems **MUST** support the following include parameters. However, if a resource is not supported, then only supported resources **MUST** be returned, rather than an error: 

### Parameter format ###

Parameter values for both _include and _revinclude have three parts, separated by a : character:

- The name of the source resource from which the join comes
- The name of the search parameter which must be of type reference
- (Optional) A specific of type of target resource (for when the search parameter refers to multiple possible target types)



| Name | Description | Paths |
|---|---|---|
| `&_include=Slot:schedule` | Include Schedule Resources referenced within the returned Slot Resources | `Slot:schedule` |
| `&_include:iterate=Schedule:actor:Practitioner` | Include Practitioner Resources referenced within the returned Schedule Resources | `Schedule:actor:Practitioner` |
| `&_include:iterate=Schedule:actor:PractitionerRole` | Include Practitioner Role Resources referenced within the returned Schedule Resources | `Schedule:actor:PractitionerRole` |
| `&_include:iterate=Schedule:actor:HealthcareService` | Include HealthcareService Resources referenced within the returned Schedule Resources | `Schedule:actor:HealthcareService` |
| `&_include:iterate=HealthcareService:Organization` | Include Organization Resources referenced within the returned HealthcareService Resources | `HealthcareService:Organization` |
| `&_include:iterate=HealthcareService:Location` | Include Location Resources referenced within the returned HealthcareService Resources | `HealthcareService:Location` |

In the event that an include is formed part of a slot request, but the providing system does not hold that the data then the providing system should return an empty element for that data item.  Available slots must still be returned to the consumer and the booking of these slots supported.  


## _format ##

The request can be formatted to the following MIME types:

|--|--|
|JSON|`_format=xml`|
|XML|`_format=json`|

## RESTful Query ##

The request body is sent using an http `GET` method.

The following query demonstrates a full request for information:

<table>
<tr>
<td>
http://[FHIR base URL]/Slot<br />
?schedule.actor:healthcareservice=918999198999<br />
&start=ge2019-05-09T10:00:00+00:00<br />
&start=le2019-05-09T10:30:00+00:00<br />
&status=free<br />
&_include=Slot:schedule<br />
&_include:iterate=Schedule:actor:Practitioner<br />
&_include:iterate=Schedule:actor:PractitionerRole<br />
&_include:iterate=Schedule:actor:HealthcareService<br />
&_include:iterate=HealthcareService.location<br />
&_format=json
</td>
</tr>
</table>

## Response ##

### Success ###
Provider systems:

- **MUST** return a `200` **OK** HTTP status code on successful retrieval of "free" slot details.
- **MUST** include the  `Slot` resources which meet the requested criteria.
- **MAY** implement <a href='http://hl7.org/fhir/STU3/http.html#paging'>paging as described here</a> to limit the number of resources returned.
- **MAY** implement an upper limit on returned Slots that excludes Slots which would fall into the requested time window.

The response `Bundle` **MUST** only contain resources related to the returned `Slot` resources. If no free slots are returned then no resources **SHOULD** be returned within the response `Bundle`. Related resources **SHOULD NOT** be duplicated (where for example they are related to multiple Slots).

### Failure ###
Provider systems:

- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response **MUST** include a status of `403` **Forbidden**.
This **MUST** be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the query string parameters were invalid or unsupported, the response **MUST** include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response **MUST** include a status of `500` **Internal Server Error**.

Failure responses with a `4xx` status **SHOULD NOT** be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status **MAY** be retried.

## Sample response ##

```json
{
    "resourceType": "Bundle",
    "id": "a56382a1-dce2-483a-8426-65b727b66809",
    "meta": {
        "lastUpdated": "2019-05-09T09:26:12.010+00:00"
    },
    "type": "searchset",
    "total": 8,
    "link":  [
        {
            "relation": "self",
            "url": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/Slot?_format=json&_include=Slot%3Aschedule&_include=Schedule%3Aactor%3APractitioner&_include=Schedule%3Aactor%3APractitionerRole&_include=Schedule%3Aactor%3AHealthcareService&_include=HealthcareService.location&schedule.actor%3Ahealthcareservice=918999198999&start=ge2019-05-09T10%3A00%3A00%2000%3A00&start=le2019-05-09T10%3A30%3A00%2000%3A00&status=free"
        }
    ],
    "entry":  [
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/Slot/slot005",
            "resource": {
                "resourceType": "Slot",
                "id": "slot005",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Slot-1"
                    ]
                },
                "identifier":  [
                    {
                        "system": "https://tools.ietf.org/html/rfc4122",
                        "value": "5a889965-3db4-4c29-8003-676c247a763e"
                    }
                ],
                "schedule": {
                    "reference": "Schedule/sched1111"
                },
                "status": "free",
                "start": "2019-05-09T10:00:00.000+00:00",
                "end": "2019-05-09T10:15:00.000+00:00"
            }
        },
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/Slot/slot006",
            "resource": {
                "resourceType": "Slot",
                "id": "slot006",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Slot-1"
                    ]
                },
                "identifier":  [
                    {
                        "system": "https://tools.ietf.org/html/rfc4122",
                        "value": "d939f0e0-d434-4962-a327-7ed52fde2b2e"
                    }
                ],
                "schedule": {
                    "reference": "Schedule/sched1111"
                },
                "status": "free",
                "start": "2019-05-09T10:15:00.000+00:00",
                "end": "2019-05-09T10:30:00.000+00:00"
            }
        },
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/Slot/slot007",
            "resource": {
                "resourceType": "Slot",
                "id": "slot007",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Slot-1"
                    ]
                },
                "identifier":  [
                    {
                        "system": "https://tools.ietf.org/html/rfc4122",
                        "value": "50360c34-c1e0-42dd-b965-8e4dd79ed902"
                    }
                ],
                "schedule": {
                    "reference": "Schedule/sched1111"
                },
                "status": "free",
                "start": "2019-05-09T10:30:00.000+00:00",
                "end": "2019-05-09T10:45:00.000+00:00"
            }
        },
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/Schedule/sched1111",
            "resource": {
                "resourceType": "Schedule",
                "id": "sched1111",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Schedule-1"
                    ]
                },
                "identifier":  [
                    {
                        "system": "https://system.supplier.co.uk/MyDiary/Numbering",
                        "value": "1015432"
                    }
                ],
                "actor":  [
                    {
                        "reference": "HealthcareService/918999198999"
                    },
                    {
                        "reference": "Practitioner/ABCD123456"
                    }
                ]
            }
        },
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/HealthcareService/918999198999",
            "resource": {
                "resourceType": "HealthcareService",
                "id": "918999198999",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-HealthcareService-1"
                    ]
                },
                "identifier":  [
                    {
                        "system": "https://system.supplier.co.uk/My/Services",
                        "value": "357"
                    }
                ],
                "providedBy": {
                    "reference": "https://directory.spineservices.nhs.uk/STU3/Organization/RR8"
                },
                "location":  [
                    {
                        "reference": "Location/loc2222"
                    }
                ],
                "name": "Service One"
            }
        },
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/Practitioner/ABCD123456",
            "resource": {
                "resourceType": "Practitioner",
                "id": "ABCD123456",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1"
                    ]
                },
                "identifier":  [
                    {
                        "system": "https://fhir.nhs.uk/Id/sds-user-id",
                        "value": "ABCD123456"
                    },
                    {
                        "system": "https://fhir.nhs.uk/Id/sds-role-profile-id",
                        "value": "R0260"
                    }
                ],
                "name":  [
                    {
                        "family": "Webber",
                        "given":  [
                            "Libbie"
                        ],
                        "prefix":  [
                            "Dr"
                        ]
                    }
                ]
            }
        },
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/PractitionerRole/R0260",
            "resource": {
                "resourceType": "PractitionerRole",
                "id": "R0260",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-PractitionerRole-1"
                    ]
                },
                "code":  [
                    {
                        "coding":  [
                            {
                                "system": "https://fhir.hl7.org.uk/STU3/CodeSystem/CareConnect-SDSJobRoleName-1",
                                "code": "R0260",
                                "display": "General Medical Practitioner"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "fullUrl": "http://a2sibookingprovidertest.directoryofservices.nhs.uk:443/poc/Location/loc1111",
            "resource": {
                "resourceType": "Location",
                "id": "loc1111",
                "meta": {
                    "profile":  [
                        "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Location-1"
                    ]
                },
                "name": "Location One"
            }
        }
    ]
}
```
