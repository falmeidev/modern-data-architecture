# Data Flow Overview

This document outlines the detailed data flow through the stages of the **Marketing Analytics Dataset** pipeline, utilizing the **Medallion Architecture**. The pipeline consists of four layers: **RAW**, **BRONZE**, **SILVER**, and **GOLD**, with each layer serving specific purposes in the transformation process.

## 1. RAW Layer (External Location)

### Objective
The **RAW Layer** stores the raw, unmodified data as it is received, in its original format. This stage serves as the source of truth for all data processing.

### Steps
1. **External Storage**: The original **CSV file** (Marketing Analytics Dataset) is stored in an **external location** (AWS S3 bucket).
2. **Data Ingestion**: The CSV file is ingested into a **volume** pointing to the external location. No transformations occur in this step; the data is maintained as-is.
3. **Schema Definition**: The data schema is defined for proper handling during ingestion, ensuring consistent column names and types.
4. **Storage**: Data remains in its raw CSV format, enabling future reprocessing if necessary.

This RAW layer ensures we retain a reliable and unaltered version of the original data for later use.

---

## 2. BRONZE Layer (Delta Format)

### Objective
The **BRONZE Layer** serves as a **copy** of the RAW data, stored in the **Delta format**. No transformations are performed in this layer; the data is stored in a more efficient and optimized format for later processing.

### Steps
1. **Copying from RAW**: Data from the **external RAW location** is copied into a Delta table in the BRONZE layer.
2. **Delta Format**: The data is converted and stored in the **Delta format**, providing support for ACID transactions, version control, and optimized queries.
3. **Storage**: The data remains unchanged, preserving its original form in the Delta format.

The BRONZE layer ensures that the data is available in an optimized, immutable format while preserving its original structure for subsequent processing.

---

## 3. SILVER Layer (Enriched Layer)

### Objective
The **SILVER Layer** is where the main transformations occur, enriching the data by adding new calculated features and performing more advanced data cleaning.

### Steps
1. **Feature Engineering**: New features are created based on the existing data to enhance its value:
   - **Age Group**: Classify customers into age brackets (e.g., "18-25", "26-35").
   - **Income Bracket**: Group customers based on income range (e.g., "Low", "Medium", "High").
   - **Channel Standardization**: Ensure consistency in how marketing channels are named and categorized.
2. **Data Validation**: Ensure integrity of relationships and data quality across all records:
   - Validate that `Customer ID` is consistent and linked to the correct campaign information.
   - Ensure that each campaign has valid attributes (e.g., `Campaign ID`, `Channel`).
3. **Standardization and Cleansing**: Clean and format text fields (e.g., remove excess whitespace, standardize case for categorical fields).
4. **Storage**: The enriched data is stored back in the **Delta format** with the new features and cleaned attributes.

The SILVER layer significantly enhances the dataset, making it more suitable for deeper analysis and modeling.

---

## 4. GOLD Layer (Aggregated Layer)

### Objective
The **GOLD Layer** is the final stage where the data is aggregated, organized into a dimensional schema, and optimized for reporting and analytical queries.

### Steps
1. **Dimensional Modeling**: Data is structured into **fact** and **dimension** tables to support analytical queries:
   - **Fact Table (`FactMarketing`)**: Includes metrics like total purchases, responses, and other key performance indicators (KPIs) tied to campaigns.
   - **Dimension Tables**:
     - **DimCustomer**: Stores customer attributes such as age group, income bracket, etc.
     - **DimCampaign**: Contains details about the campaigns, like campaign type and channel.
2. **Data Aggregation**: Key metrics are calculated, such as:
   - Total purchase amount per campaign.
   - Average response rate by customer segment.
   - Campaign performance by channel.
3. **Storage**: The data is stored in the **Delta format**, which supports quick, optimized analytical queries.

The GOLD layer prepares the data for final reporting and business intelligence, providing structured, easily queryable data in a dimensional format.

