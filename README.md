--Description--
This project demonstrates how to generate synthetic medication records from multiple, independent EHR medication tables (such as openEHR, externalEHR, and others), merge them into a unified dataset, and export the results in the OMOP drug_exposure format.
The script applies configurable rules (such as source priority) to resolve overlaps and deduplicate records for each patient and medication combination.
This is ideal for testing ETL processes, data harmonization, and interoperability scenarios where medication data comes from several distinct EHR systems or tables.

--Features--
Simulates Multiple Medication Tables:
Generates synthetic medication records for several EHR systems, each as a separate table (DataFrame).

-Customizable Merge & Deduplication:
Merges all medication tables, applying rules (e.g., source priority) to determine which record to keep per patient-drug pair.

-OMOP Export:
Outputs a deduplicated, OMOP-compliant drug_exposure CSV file for downstream analytics or loading into a CDM.
