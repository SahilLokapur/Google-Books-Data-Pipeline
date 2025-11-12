# Google-Books-Data-Pipeline

This project focuses on the design and implementation of a robust, near-real-time data ingestion and warehousing pipeline for book metadata sourced from the Google Books API. The pipeline is fully automated and orchestrated to ensure data freshness, quality, and reliability within the data warehouse.

Architecture and Technologies:

Data Source & Ingestion: Data is programmatically captured in real-time from the Google Books API and immediately stored in a designated AWS S3 Landing Bucket.

Orchestration (Apache Airflow): The entire workflow is managed by an Apache Airflow Directed Acyclic Graph (DAG), which is scheduled to run the data collection and processing steps every 10 minutes.

ETL Processing (Spark): The Airflow DAG triggers Spark ETL jobs for data processing. This multi-stage process includes:

S3 Movement: Copying data from the Landing Zone to a Working Zone for processing.

Transformation: Reading data from the Working Zone, applying necessary transformations (e.g., cleaning, normalization), and repartitioning the dataset for optimal storage.

Processed Zone: Storing the final, transformed data back into S3 (Processed Zone).

Data Warehousing (AWS Redshift): A dedicated Warehouse Module stages the data from the S3 Processed Zone into AWS Redshift staging tables. A critical UPSERT (Update or Insert) operation is then performed to efficiently merge the new/updated data into the final Data Warehouse tables.

Data Quality & Analytics: The Airflow DAG incorporates essential quality control and analytical steps:

Data Quality Checks (DQC): DQCs are executed immediately after the Redshift update on all Warehouse tables to ensure integrity.

Analytics Module: A Custom Designed Airflow Operator runs pre-configured Analytics Queries against the updated warehouse. A final DQC is run on the resulting Analytics Tables.
