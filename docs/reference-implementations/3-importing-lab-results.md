---
layout: default
title: 3. Importing Lab Sample Data to Go.Data
parent: Interoperability Examples //Reference Implementations
nav_order: 3
permalink: /importing-lab-results/
---
# Importing Lab Sample Data to Go.Data
For many Go.Data implementers, lab results data will be the starting reference point when implementing a new Go.Data instance. This sample data will need to be first be transformed to align with the Go.Data data model, where `Case` and `Contact` records must first be created to track individual beneficiary details. Once these individual records are created, then related `Lab Results` data can be imported. 

![use-case-3](../assets/use-case-3.png)

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
2. See the section on [Unique Identifiers](...) to support with resource matching during data imports. 