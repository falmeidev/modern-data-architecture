# Medallion Architecture: A Layered Approach to Data Quality

## Overview of Medallion Architecture

The **Medallion Architecture** is a layered framework designed to improve data quality and organize data processing workflows. This architecture divides data storage and processing into distinct layers, named **RAW (Bronze)**, **Cleansed (Silver)**, and **Aggregated (Gold)**. Each layer enhances data quality, transforming it from raw states to refined, structured formats ready for analysis. Below is an illustration of the architecture:

<img src="..\resources\imgs\medallion_architecture.png" alt="Medallion Architecture" width="600"/>

Key principles of the Medallion Architecture include:
1. **Data Quality at Every Layer**: Data undergoes validation, cleaning, enrichment, and aggregation at each stage.
2. **Modularity**: Each layer has distinct tasks, allowing flexibility in managing data workflows.
3. **Separation of Concerns**: Each layer serves a dedicated purpose, improving maintainability and scalability.

## Implementation for the Marketing Analytics Dataset

This project applies the Medallion Architecture to a **Marketing Analytics Dataset** to transform raw customer and campaign data into structured insights for marketing performance analysis. Each layer has specific tasks and transformations:

### 1. RAW Layer (Bronze)

The **RAW Layer** is the initial stage where data is ingested directly from source files in its original format:
   - **Data Source**: The original CSV file containing customer and campaign data.
   - **Objective**: Preserve unaltered data as the single source of truth.
   - **Storage**: Data is stored as Delta tables in a dedicated RAW folder, ensuring any processing or transformations do not alter the original data.

This layer provides quick access to the raw data, allowing debugging and validation of data integrity.

### 2. Cleansed Layer (Silver)

The **Cleansed Layer** performs data cleaning and initial enrichment, preparing the data for analysis:
   - **Data Cleaning**: Removes duplicates, fills null values, standardizes data types, and normalizes text fields (e.g., `Company`, `Campaign_Type`).
   - **Enrichment**: Adds calculated fields like `Engagement Rate`, `Cost Per Click`, and categorizes `Target_Audience` into age groups.
   - **Purpose**: The SILVER layer provides structured and enriched data that is consistent and ready for deeper analytical transformations.

The SILVER layer ensures that the data is clean, structured, and contains added dimensions for analytical flexibility.

### 3. Aggregated Layer (Gold)

The **Aggregated Layer** presents the data in a dimensional model, optimized for analytics and reporting:
   - **Data Modeling**: Data is structured in fact and dimension tables:
     - **Fact Table (fact_campaign_performance)**: Aggregates metrics such as Clicks, Impressions, and Conversion Rate, providing quantitative insights.
     - **Dimension Tables** (`dim_company`, `dim_campaign`, `dim_channel`, `dim_location`): Include descriptive information, supporting analytics by segmenting data by various attributes.
   - **Purpose**: The GOLD layer supports fast queries and reporting, enabling efficient exploration of campaign performance and customer insights.

The GOLD layer provides a highly organized dataset ready for dashboarding, reporting, or machine learning applications.

## Benefits of the Medallion Architecture for This Project

1. **Data Quality**: Ensures high-quality data for analytics and decision-making at each layer.
2. **Scalability**: New data sources and transformations can be added without disrupting existing workflows.
3. **Performance**: Dimensional modeling in the GOLD layer enables optimized query performance for fast, reliable analytics.


