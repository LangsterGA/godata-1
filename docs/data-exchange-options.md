---
layout: page
title: Data Interoperability Options
nav_order: 3
permalink: /interoperability-options/
---
# Data Interoperability Options & Use Cases
## 1. Data Input
...

## 2. Data Sharing & APIs
...

## 3. Integration Scripts & ETL Tools
...

## 4. Connectors with Common ICT4D Tools
...

# Real-World Interoperability Scenarios
To develop this toolkit, we engaged several Go.Data community members and HIS stakeholders to collect common interoperability requirements for Go.Data implementations. Key requirements for data interoperability were summarized into two main categories:

## Transactional information sharing  (individual-level data exchange)
For transactional data exchange and building shared health records for enhanced collaboration. 1- and 2-way information exchange flows, list examples…

[diagram]

## Aggregate information sharing (summary-level data exchange)
For aggregate data exchange primarily used for external reporting & monitoring. Typically a 1-way information flow, list examples…

[diagram]

## Common Use Cases
[See this table] for interoperability use cases identified by the Go.Data partners through "Interoperability" `Ask The Expert` sessions and conversations with Go.Data impleementers and team in December 2020. 

**Table Below:** 6 use cases from this list have been implemented as [Interoperability Reference Implementations](...) to showcase common interoperability scenarios and priority requirements. 
In the table below, we have highlighted the common use cases we will address via solutions developed as reference implementations. 


| # |                 Type                |                                                                                                                                      Use Case                                                                                                                                      |                                                                                                                                Solutions                                                                                                                               |                                                                                                                      Comments                                                                                                                      |
|:-:|:-----------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| 1 | 2-way contact record exchange       | As a MOH employee, I would like to integrate contact data from my country’s surveillance system, so that I can use Go.Data for contact tracing & follow-up in a way that does not duplicate efforts.                                                                               | Users may leverage available web APIs to configure 2-way data integration and share individual records, matching on unique identifiers.  Other options: Excel upload tools, integration scripts                                                                        | See [section X below] for more guidance on building shared records, unique identifiers, & duplicate prevention.                                                                                                                                    |
| 2 | Applying data standards             | As a Go.Data user, I would like to automatically apply the FHIR data standard to any contact information collected via Go.Data before sharing with the HMIS to ensure data interoperability and to avoid any manual data cleaning/reformatting steps during information exchange.  | Users may extract Go.Data data via APIs and develop a script to automate the applying of standards & integration with HMIS.   Other options: Excel upload tools, integration scripts                                                                                   | Applying data standards later in the ETL process is common for scenarios where it’s unrealistic to apply these standards right at data collection.  [FHIR-HL7 & other common standards links…]                                                     |
| 3 | Importing lab results import        | As a Go.Data analyst, I would like to directly integrate lab datasets so that I can more quickly and securely register Contact and Case records to ensure data quality and save time on data entry.                                                                                | Users may leverage available Excel templates for building & importing new Contact and Case records. Other options: direct integration via APIs                                                                                                                         | Lab datasets are the primary data source for many Go.Data implementers, but many struggle to build Contact and Case records from lab sample data.                                                                                                  |
| 4 | Integrating mobile surveys          | As a MOH employee, I would like to integrate case data collected via my mobile survey tool so that I can monitor new case registrations in real-time and quickly follow up with new contacts.                                                                                      | Users may leverage available web APIs to configure data integration and register survey submissions as new cases and contacts.  Other options: Excel upload tools, integration scripts                                                                                 | Many tools for mobile data collection (incl., Kobo Toolbox, ODK, CommCare, ONA, SurveyCTO) have a “data forwarding” feature for quicker integration setup. See [X section] to learn more.                                                          |
| 5 | Importing locations/ facility lists | As a Go.Data analyst, I would like to import health facility lists and geographical/admin units from the HMIS so that I can more easily exchange information with the MOH and other partners.                                                                                      | Users may leverage the web APIs or Excel import features to integrate geographic/admin units and health facility lists from DHIS2 and other HMIS to enable easy reporting and information exchange.   Other options: Excel upload tools, UPC DHIS2 integration scripts | See Go.Data repository for UPC scripts for “Org Unit converter” to import from DHIS2.  What is the Facility List resource Lucas mentioned? … can we demo a live sync with a HeRAMS system?                                                         |
| 6 | Aggregate reporting for KPIs        | As a MOH employee using DHIS2, I would like to receive a weekly summary of Go.Data data cases to monitor performance across key COVID-19 indicators (e.g., # confirmed cases, # hospitalized cases, transmission classifications).                                                 | Users may leverage the Go.Data web API to extract relevant source data, develop an integration script to summarize & re-format  this data, and then map and upload to DHIS2 on s scheduled basis.  Other options: Excel upload & reporting tools, Python and R scripts | DHIS2 provides a COVID-19 Surveillance Package for aggregate data capture, but countries may have their own custom modules configured for aggregate data capture & surveillance. See the Go.Data Github for links to sample Python and R scripts.  |

