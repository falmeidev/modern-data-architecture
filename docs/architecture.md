# Medallion Architecture: A Layered Approach to Data Quality

## Overview of Medallion Architecture

The **Medallion Architecture** is a layered framework designed to improve data quality and facilitate data processing in a scalable, organized manner. This architecture divides data storage and processing into distinct layers, typically named **RAW (Bronze)**, **Cleansed (Silver)**, and **Aggregated (Gold)**. Each layer progressively enhances the data quality, moving it from raw, unprocessed states to refined, structured formats ready for analysis. These layers can be seen on the image below.

 <img src="../resources/imgs/medallion-architecture.png" alt="Medallion Architecture" width="600"/>

The Medallion Architecture follows these principles:
1. **Data Quality at Every Layer**: As data flows through each layer, it undergoes validation, cleansing, enrichment, and aggregation steps to improve quality.
2. **Modularity**: Each layer can be processed separately, providing flexibility in managing data workflows.
3. **Separation of Concerns**: By segmenting data processing stages, each layer has a dedicated purpose, which improves maintainability and scalability.

## Implementation for Marketing Analytics Dataset

This project applies the Medallion Architecture to the **Marketing Analytics Dataset** to transform raw customer and campaign data into insights for marketing performance and customer behavior analysis. Each layer has specific tasks and transformations to enhance data quality and prepare it for analytical use. The stages are as follows:

### 1. RAW Layer (Bronze)

The **RAW Layer** is the initial stage where data is ingested directly from source files in its original format. For this project:
   - **Data Source**: We load the original CSV file containing customer and campaign data.
   - **Objective**: Preserve the unaltered data to serve as a single source of truth.
   - **Storage**: Stored as Delta tables in a dedicated RAW folder, ensuring that any processing or transformations do not affect the original data.
   
   This layer allows quick access to the raw data and simplifies debugging, as the unprocessed data is always available.

### 2. Cleansed Layer (Silver)

The **Cleansed Layer** performs data cleaning and initial transformations to prepare data for analysis. For this project:
   - **Data Cleaning**: Remove null values, standardize data types (e.g., customer IDs), and apply any necessary formatting.
   - **Enrichment**: Calculate additional features, such as age groups based on customer age, or other derived attributes that aid in segmentation and customer insights.
   - **Purpose**: This layer produces clean, consistent data that is enriched for initial analysis and ready to support further transformation.

   By preparing data in the SILVER layer, we ensure it is structured and enriched, providing a foundation for aggregation and dimensional modeling in the GOLD layer.

### 3. Aggregated Layer (Gold)

The **Aggregated Layer** provides the data in an analytical format, optimized for reporting and complex analytics. In this project:
   - **Data Modeling**: We create a dimensional model that includes fact and dimension tables. Key tables include:
     - **Fact Table (FactMarketing)**: Contains transaction details, such as purchase amounts and campaign responses, providing quantitative insights.
     - **Dimension Tables (DimCustomer, DimCampaign)**: Include detailed customer and campaign information, allowing analysts to slice data by age group, income, campaign type, etc.
   - **Purpose**: This structure supports fast querying and reporting, enabling marketing analysts to explore campaign performance, customer segmentation, and other insights efficiently.
   
   The GOLD layer provides a highly organized dataset ready for dashboarding, reporting, or machine learning applications, ensuring maximum data usability.

## Benefits of Medallion Architecture for This Project

1. **Data Quality**: The Medallion Architecture ensures high-quality data at every step, making it suitable for analytics and decision-making.
2. **Scalability**: New data sources or transformations can be easily added without disrupting existing workflows.
3. **Performance**: By structuring the data in a dimensional model in the GOLD layer, the architecture supports optimized query performance for fast analytics.

