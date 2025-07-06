The notebook provides a complete pipeline for transforming synthetic health data into the OMOP Common Data Model (CDM) and loading it into a PostgreSQL database. Here’s how it works, with emphasis on the use of two different EHR data sources for medications:

1. Data Loading and Preparation
Synthetic patient, condition, medication, and encounter data are loaded from Synthea-generated CSV files using pandas.
Each patient is assigned a unique person_id, and gender is mapped to OMOP concept IDs (8507 for male, 8532 for female).

2. Simulating Two EHR Sources for Medications
The original Synthea medication data is labeled as coming from the 'syntheaEHR' source.
To simulate a second EHR system, a random 30% sample of the Synthea medications is copied, their start/stop dates are slightly altered, and these records are labeled as 'externalEHR'.
Both datasets are combined into a single medications dataframe, with a SOURCE column to distinguish between them.

3. Deduplication and Drug Exposure Table Creation
The combined medications data is deduplicated: for each person and drug, only the record with the longest duration is kept, preferring Synthea data if durations are equal. The deduplicated data is used to build the OMOP drug_exposure table, which includes unique IDs, mapped person IDs, drug codes, dates, and type concept IDs.

4. Mapping and Building Other OMOP Tables
Condition Occurrence: Patient and condition data are merged, and condition codes (ICD-10/ICD-9) are mapped to OMOP standard concept IDs using the official OMOP vocabulary (CONCEPT.csv).
Visit Occurrence: Encounter data is transformed into the OMOP visit_occurrence table, with each visit assigned a unique ID and standard OMOP visit concept.
Person Table: Patient demographic data is formatted to OMOP specifications.

5. Database Operations
Existing OMOP tables in PostgreSQL are dropped and recreated to ensure a clean environment.
Data is uploaded to the database using SQLAlchemy, and row counts are checked for all tables to confirm successful upload.

6. Vocabulary Mapping
ICD codes are mapped to OMOP standard concept IDs, ensuring interoperability and standardization for downstream analytics.

Summary Table: EHR Data Sources for Medications
Source	Description
syntheaEHR	Original synthetic medication data from Synthea
externalEHR	Simulated external EHR: 30% random sample of Synthea data, with date tweaks

Purpose:
This workflow enables researchers to simulate integration of medication data from two different EHR systems, deduplicate overlapping records, and load the harmonized data into an OMOP-compliant database for standardized analytics and research
