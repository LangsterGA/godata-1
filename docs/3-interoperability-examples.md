---
layout: page
title: Interoperability Examples
nav_order: 4
permalink: /interoperability-examples/
---
# Interoperability Reference Implementations
The following examples were configured to demonstrate common interoperability use cases & potential solutions. 

## 1. Go.Data <> HMIS/Surveillance system 2-way information exchange
In most country-level implementations, Go.Data needs to interoperate with an existing HMIS or Surveillance system to ensure no duplication of efforts. These systems might be configured on an application like DHIS2 or are custom developed using other software or databases. In this solution we explore the following use cases by designing for a Go.Data integration with a theoretical SQL-based HMIS for 2-way exchange of case information. 

![implementation-1](../assets/godata-example1.png)

---
**Use Cases:**

**#1.**  _As a MOH employee, I would like to integrate contact data from my country’s surveillance system, so that I can use Go.Data for contact tracing & follow-up on reported cases in a way that does not duplicate efforts._

**#2.** _As a Go.Data user, I would like to automatically apply the FHIR data standard to any information collected via Go.Data before sharing with the HMIS to ensure data interoperability and to avoid any manual data cleaning/reformatting steps during information exchange._

---
![diagram-1-2](../assets/io-use-case-1-2.png)


### 1. HMIS to Go.Data
To automate data integration from the HMIS to Go.Data, implementers may consider 2 common integration approaches: 
1. Data forwarding...
2. Direct data integration via APIs where you can (1) send a HTTP request to fetch the relevant data from the source HMIS, and then (2) upsert* the data in Go.Data, matching HMIS `caseId` with Go.Data `externalId` to ensure no duplicate records are created. 


* _**"Upsert"** operations are a data import pattern where you first check if a record exists using an external identifier, and then either **update** or **insert** a new record dependng on whether an existing record is found. See the [section on Unique Identifiers](...) for additional considerations regarding upserts, `externalId` and other unique identifiers._ 

### 2. Go.Data to HMIS & Applying FHIR 
To automate data integration from Go.Data to the HMIS, we...
1. Leverage the Go.Data API to automatically extract cases via an HTTP request to `GET /cases`. 
2. Apply transformation rules determined from [FHIR HL7](...) to clean, re-format, & map the Go.Data information to match the international standard
3. We then upsert the transformed data in the HMIS system, matching HMIS `caseId` with the Go.Data `externalId` to ensure no duplicates are uploaded

### Considerations for two-way syncing
1. Unique identifiers are critical to ensuring no duplicate records or efforts and developing a **shared record reference**. 
2. Determine the system of record...
3. Consider implementing a date/time `cursor` so that every time you extract data from a source system, you will only extract the _most recent_ data. This minimizes the data load to be exchanged between systems, which is good for efficiency and security. 
4. Consider what initiates the data `sync`...

### Demo Solution & Implementation Resources
1. [See this video](...) of the demo solution configured to demonstrate these use cases #1 and #2.  
2. See [example integration scripts](...) ... 
3. See the solution [design documentation](...) ... 

![implementation-2](../assets/godata-example2.png)

## 3. Importing Lab Sample Data to Go.Data
For many Go.Data implementers, lab results data will be the starting reference point when implementing a new Go.Data instance. This sample data will need to be first be transformed to align with the Go.Data data model, where `Case` and `Contact` records must first be created to track individual beneficiary details. Once these individual records are created, then related `Lab Results` data can be imported. 

---
**Use Case:**
**#3.** _As a Go.Data user, I would like to directly integrate lab datasets so that I can more quickly and securely register Contact and Case records to ensure data quality and save time on data entry._

---

### Standard Import Process
Typically a lab will provide export of `Sample`/ `Results` data like [this example](...). In this data source, there might be multiple `Sample` results for 1 individual. To import this data to the Go.Data system, the user will need to...

1. Analyze the dataset to construct unique `Case` and `Contact` records. Consider...

* 1.a. Is there an available patient unique id (e.g., national id)? 
* 1.b. If not, can you construct a new unique id? 
 
2.Import the individual records to Go.Data using the standard `import` wizard or the API. When uploading, match with existing resources to prevent duplicates. To do this, you will need to consider the appropriate unique identfier to check for existing records. Consider...

* 2.a. Will you import a lab-provided custom Id or standard identifier (e.g., `nationalId`) from the lab dataset? (and mask the Go.Data Case Id as `*`...)
* 2.b. Will you generate a new custom Id (e.g., `caseId: 'CASE-00001'`) based on some standard naming convention and autonumber? 
 
3. Organize the data to link every `Sample` record to an individual person record. [See this Case - Lab Results](...) import template. 

4. Export the Ids...

### Implementation Resources
1. See [example import templates](...) ... 
3. See the section on [Unique Identifiers](...) to support with resource matching during data imports. 


## 4. Mobile to Go.Data
Mobile apps and survey tools are widely used by Go.Data implementers and other health initiatives for on-the-ground data collection, response, basic monitoring. Many of these tools are open-source and developed on similar frameworks (e.g., ODK, Kobo Toolbox, ONA, CommCare), so the available integration options are very similar. 

---
**Use Case:**
**#4.** _As a MOH employee, I would like to integrate case data collected via my mobile survey tool so that I can monitor new case registrations in real-time and quickly follow up with new contacts._

---

![dataflow-4](../assets/io-use-case-4.png)

## Integration Steps
1. ...

### Implementation Resources
1. [See this video](...) of the demo solution configured to demonstrate this use case #4.  
2. To click around & explore the solution in-depth, go to OpenFn.org...
3. See [example integration scripts](...) ... 
4. See the solution [design documentation](...) ... 

## 5. Health Facility/ Locations Import to Go.Data
Importing administrative locations (e.g., `Admin-Level-2` province data) and different `Location` reference data are typically the first steps in setting up a new `Outbreak` in Go.Data. Importing location lists from shared reference sources are important to strengthening interoperability and ensuring information can be easily synced and mapped in other systems. [See p. 33 of the Implementation Guide](...) for more information and step-by-step guidance for data imports. Here we demonstrate how `Location` data may be synced via APIs.  

---
**Use Case:**
**#5.** _As a Go.Data analyst, I would like to import facility and location lists from standard registries and external data sources so that I can more easily exchange information with the MOH and other partners._

---
![dataflow-5](../assets/io-use-case-5.png)
### Integration Steps
1. Identify the external data source and available APIs - see [Health Sites example](). 
2. Export the data from the source - [see here]() for an example script where we send a `GET` HTTP request to `/api/facilities` to fetch a health facility list.
3. Analyze the response to #2 to determine the appropriate unique identifiers to matching with Go.Data `locations` and to use as `externalId` for future duplicate prevention. 
4. Upsert the facilities as `locations` in Go.Data using `name` as an external identifier to match existing locations ([see script here](...). You might also consider matching on other unique identifiers such as available `uuid` or `geodata` codes. 

### Implementation Resources
1. [See this video](...) of the demo solution configured to demonstrate this use case #5.  
2. To click around & explore the solution in-depth, go to OpenFn.org...
3. See [example integration scripts](...) ... 
4. See the solution [design documentation](...) ... 

### External Data Sources
1. `HDX` – a clearinghouse of humanitarian open-source data. Included for many countries is the administrative unit boundaries which typically has a unique ID (Pcode) and in some cases this will also include additional data such as “population” that can be used in post analysis.
https://data.humdata.org/
2.`WHO` – Global coverage of administrative unit boundaries, has centroid and Pcode for unique ID – so could be joined back to GIS data afterwards but only goes to Adm2 and in some countries only ADM 1.
https://polioboundaries-who.hub.arcgis.com/
3. `Geonames` – has ID, lat/lon and names – may or may not be able to match up to any GIS outside of Go.Data.
http://download.geonames.org/export/dump/
4. `HealthSites.io` - open source repository of health facility data built in partnership with Open Street Map http://healthsites.io/

## 6. Aggregate reporting to DHIS2 
Reporting...

---
**Use Case:**
**#6.** _As a MOH employee using DHIS2, I would like to receive a weekly summary of Go.Data data cases to monitor performance across key COVID-19 indicators (e.g., # confirmed cases, # hospitalized cases, transmission classifications)._

---
![dataflow-5](../assets/io-use-case-6.png)

## Integration Steps 
1. 

### Implementation Resources
1. [See this video](...) of the demo solution configured to demonstrate this use case #6.  
2. To click around & explore the solution in-depth, go to OpenFn.org...
3. See [example integration scripts](...) ... 
4. See the solution [design documentation](...) ... 
