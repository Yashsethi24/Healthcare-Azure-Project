**Data Sources & Storage Strategy (Medallion Architecture)**

In this project, we will be integrating multiple healthcare-related datasets into a structured, tiered data architecture that follows the Medallion pattern (Bronze, Silver, and Gold layers). Each data source leverages specific Azure services for ingestion, storage, and transformation.

**Data Sources:**

1. **EMR Data (Azure SQL DB):**  
   - **Datasets:** Patients, Providers, Departments, Transactions, Encounters  
   - **Storage & Access:** The Electronic Medical Records (EMR) data will reside in an Azure SQL Database. Data will be periodically extracted and landed into the data lake in Parquet format to form the initial Bronze layer.

2. **Claims Data (Flat Files):**  
   - **Datasets:** Insurance claims  
   - **Storage & Access:** Claims data, provided monthly as flat files, will initially be stored in a **Landing** folder within Azure Data Lake Storage. From there, the data will be transformed and converted into Parquet files for the Bronze layer.

3. **NPI Data (Public API):**  
   - **Datasets:** National Provider Identifier (NPI) details (e.g., unique identifiers for healthcare providers)  
   - **Storage & Access:** NPI data will be retrieved via a public API and ingested into the data lake as raw JSON or CSV files, then standardized into Parquet for the Bronze layer.

4. **ICD Data (Public API):**  
   - **Datasets:** ICD codes, which map diagnoses to standardized codes and descriptions  
   - **Storage & Access:** ICD data will be sourced from a public API and stored similarly to NPI data, initially landing in raw form, then structured into Parquet for Bronze.

**Medallion Architecture Layers:**

- **Landing (Raw Data):**  
  This layer stores incoming data as-is—whether from SQL DB extractions, flat files, or API responses—without any transformations.

- **Bronze Layer (Raw, Parquet Format):**  
  Data in the Landing area is converted into Parquet format, providing a structured, columnar foundation. This layer represents the single source of truth for raw data, enabling efficient reads and schema evolution.

- **Silver Layer (Cleaned & Enriched, Delta Tables):**  
  From the Bronze layer, data undergoes cleaning, deduplication, and enrichment. It may also be standardized into a Common Data Model (CDM), and Slowly Changing Dimensions (SCD2) techniques will be applied where necessary. The Silver layer is stored in Delta tables for versioning and easy incremental updates.

- **Gold Layer (Curated & Business-Ready, Delta Tables):**  
  The Gold layer contains fully curated, analytics-ready datasets that support reporting, advanced analytics, and downstream applications. Delta tables at this layer provide optimized, trusted data ready for BI tools and predictive modeling.

**Azure Services & Data Flows:**

- **Azure SQL Database (Source for EMR Data)**  
- **Azure Data Lake Storage (ADLS) for Landing, Bronze, Silver, and Gold Layers**  
  - Landing: Raw flat files, API outputs  
  - Bronze: Parquet files (raw structure)  
  - Silver: Delta tables (cleaned, standardized)  
  - Gold: Delta tables (highly curated, ready for analytics)
- **Azure Data Factory / Databricks:** Used to orchestrate data ingestion, transformations, and enrichment steps across the Medallion layers.
- **Azure Synapse / Databricks:** Tools for advanced analytics, BI reporting, and machine learning models on the Gold datasets.

This approach ensures a modular, scalable, and governed data ecosystem, enabling better data quality, improved insights, and streamlined decision-making in the healthcare revenue cycle management process.
