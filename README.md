# Large-Scale TV Viewing Analytics with PySpark

This repository contains a two-part Big Data project developed using **Apache Spark and PySpark**.

The project analyzes large-scale television viewing, demographic, household, and geographic data. It combines distributed data processing, complex business-rule evaluation, large-scale joins and aggregations, demographic analysis, geographic segmentation, and optimized storage using Parquet.

---

## Project Overview

The project works with several large datasets describing:

- Television programs and program metadata
- Viewing events
- Household demographic information
- Device-to-household mappings
- Geographic market areas (DMA)

The analysis is divided into two main parts.

---

## Part 1 – Rule-Based Detection on Large-Scale Viewing Data

The first part builds a distributed detection pipeline for identifying potentially malicious television programs.

The workflow combines program-level information with household-level demographic characteristics and viewing behavior.

### Data Preparation

The datasets are cleaned before analysis by:

- Removing invalid or null identifiers
- Removing unnecessary duplicates
- Selecting only relevant columns
- Standardizing data types
- Preparing datasets for efficient joins

The project processes datasets containing millions of records, including more than:

- 13 million program records
- 9 million viewing events
- 700,000 device reference records
- 350,000 household demographic records

### Detection Pipeline

Seven different conditions are evaluated using both program metadata and household characteristics.

Examples include:

- Program duration relative to the global average
- Household vehicle information
- Household composition and age differences
- Programs aired around Friday the 13th
- Household device count and income level
- Specific program genres
- Keyword patterns appearing in program titles

Each condition is represented as a flag and later aggregated at the program level.

Household-based conditions are propagated to programs through several distributed joins:

```text
Device → Household → Demographic Data → Viewed Program
```

The different condition scores are then combined into a final maliciousness score.

### Program-Level Aggregation

For every program code, the pipeline determines whether demographic conditions were satisfied by at least one associated household.

Program-level and demographic-level conditions are then combined into a single score.

Programs satisfying at least four of the seven conditions are marked as malicious.

Finally, titles are aggregated and their malicious-record ratio is calculated. Titles for which more than 40% of records are classified as malicious are retained and ranked.

---

## Part 2 – Audience and Geographic Analytics

The second part focuses on extracting business-oriented insights from television viewing behavior.

### Popular Genre Analysis

Viewing events are connected to households and household sizes in order to estimate audience exposure.

The analysis:

- Maps devices to households
- Maps programs to genres
- Removes duplicate household exposure
- Aggregates viewers by genre
- Identifies the most popular television genres

### Geographic Market Analysis

The project analyzes **Designated Market Areas (DMA)** using device and household information.

It identifies:

- The most represented DMAs
- The estimated population associated with these markets
- Household distribution across geographic areas

### Program Popularity Among Families

Households containing children are isolated and linked to their viewing activity.

The pipeline determines the most popular programs among these households and estimates the total number of people exposed to the selected programs.

### DMA Wealth Score

A custom wealth score is computed for each DMA using household:

- Income
- Net worth

Categorical income values are transformed into numerical values before aggregation.

The resulting statistics are used to identify the highest-scoring geographic markets.

### Genre Distribution Across Wealthy DMAs

For the highest-ranked DMAs, the project analyzes genre popularity.

Viewing data is:

- Joined with geographic information
- Aggregated by DMA and genre
- Written to **Parquet files partitioned by DMA**
- Read back selectively for DMA-specific analysis

Popular genres are then assigned to each selected DMA while avoiding repeated genre assignments across markets.

---

## Data Engineering Techniques

The project makes extensive use of distributed Spark operations, including:

- DataFrame transformations
- Explicit schemas
- Large-scale joins
- GroupBy aggregations
- Duplicate elimination
- Null handling
- Conditional columns
- Regular-expression filtering
- Date and timestamp transformations
- Distributed aggregation
- RDD operations
- Parquet storage
- Partitioned Parquet datasets

---

## Technologies

- Python
- PySpark
- Apache Spark
- Spark SQL
- Databricks
- Parquet
- Spark DataFrames
- Spark RDDs

---

## Repository Structure

The repository contains two main notebooks:

```text
├── Part 1
│   └── Large-scale rule-based program detection
│
├── Part 2
│   └── Audience, demographic and geographic analytics
│
└── README.md
```

---

## Topics Covered

- Big Data
- Data Engineering
- Apache Spark
- PySpark
- Distributed Computing
- Large-Scale Data Processing
- ETL
- Data Cleaning
- Data Integration
- Spark SQL
- DataFrame Operations
- Distributed Joins
- Aggregations
- Demographic Analytics
- Audience Analytics
- Geographic Segmentation
- Rule-Based Detection
- Parquet
- Data Partitioning

---

## Key Takeaways

This project demonstrates how Apache Spark can be used to process and combine multiple large-scale datasets in order to build both analytical and rule-based data pipelines.

It covers the full workflow from raw distributed data ingestion and cleaning to complex joins, feature construction, program classification, geographic analysis, audience segmentation, and partitioned storage.
