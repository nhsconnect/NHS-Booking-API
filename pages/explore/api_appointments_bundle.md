---
title: Other | Bundle | End of Life
keywords: usecase, bundle
tags: [rest, fhir, workflow,api]
sidebar: foundations_sidebar
permalink: api_eol_bundle.html
summary: Other Bundle
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.STU3.reference.html resource="Bundle" page="EOL-Bundle-1" fhirname="Bundle" fhirlink="bundle.html" content="User Stories" userlink="engage_endoflife.html" %}

## Bundle ##

The bundle resource brings together all the entries to make an EOL record. 

...

	<Bundle xmlns="http://hl7.org/fhir">
	<id value="bundle-transaction"/> 
	<!--    this example bundle is a transaction     -->
	<meta> 
		<lastUpdated value="2014-08-18T01:43:30Z"/> 
	<!--    when the transaction was constructed    -->
	</meta> 
	<type value="transaction"/> 
	<entry> 
		<fullUrl value="http://example.org/fhir/Patient/123a"/> 
		<resource> 
		<Patient> 
			<id value="123a"/> 
			<text> 
			<status value="generated"/> 
			<div xmlns="http://www.w3.org/1999/xhtml">Some narrative</div> 
			</text> 
			<active value="true"/> 
			<name> 
			<use value="official"/> 
			<family value="Chalmers"/> 
			<given value="Peter"/> 
			<given value="James"/> 
			</name> 
			<gender value="male"/> 
			<birthDate value="1974-12-25"/> 
		</Patient> 
		</resource> 
		<request> 
		<method value="PUT"/> 
		<url value="Patient/123a"/> 
		<!--   this will only succeed if the source version is correct:   -->
		<ifMatch value="W/&quot;2&quot;"/> 
		</request> 
		</entry> 
		<entry>  
	</entry> 
	</Bundle> 