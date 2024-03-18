# GoodReads Data Pipeline Project

## Overview
This project encapsulates a comprehensive data pipeline that captures data in real-time from the GoodReads API, processes this data through various ETL jobs, and ultimately stores it in a Redshift data warehouse for analytical purposes. The pipeline utilizes a variety of modules including a GoodReads Python Wrapper, ETL jobs, a Redshift Warehouse Module, and an Analytics Module.

### Key Features
- **Real-time Data Capture:** Utilizes the Goodreads Python wrapper to fetch data from the Goodreads API, storing it locally before moving it to AWS S3.
- **ETL Jobs:** Scheduled via Airflow to run every 10 minutes, transforming data from S3 to a working zone, then processing and moving it to a processed zone.
- **Data Warehousing:** Leveraged Redshift to stage data from the processed zone before updating the data warehouse using UPSERT operations.
- **Analytics:** Post-ETL, Airflow DAG runs analytics queries and performs data quality checks on warehouse tables.

## ETL Flow
1. API data is stored in S3 landing zones.
2. S3 modules transfer data to working zones; Spark jobs then transform this data.
3. Data is staged into Redshift and updated in warehouse tables.
4. Airflow DAGs run data quality checks and analytics queries, finalizing with a data quality assessment.

## Environment Setup

### Hardware
- **EMR:** Utilized a 3-node cluster (m5.xlarge instances) with 64 GiB EBS storage.
- **Redshift:** A 2-node dc2.large cluster.

### Airflow Setup
- Install `apache-airflow[sshtunnel]` via pip for SSH tunneling.
- Configure Airflow connections for EMR and Redshift integration.

### EMR Configuration
- Install necessary packages like `psycopg2` for Redshift connections and `boto3` for S3 interactions.
- Switch EMR to use Python3 by setting environment variables for PySpark.

### Redshift Setup
Utilize the provided `Redshift_Cluster_IaC.py` script for cluster setup.

## Running the Pipeline
Ensure Airflow's webserver and scheduler are active. Access the Airflow UI via the EC2 instance IP and configured port. The GoodReads Pipeline DAG provides comprehensive views including DAG, Tree, and Gantt charts, facilitating monitoring and management.

## Testing the Limits
- Utilized the `goodreadsfaker` module to generate significant data volumes, testing the pipeline under heavy load conditions.
- Performance and scalability tested with up to 1.6 TB/day data processing.

## Scalability and Daily Operations
- Adjustments for data volume increases or read/write priority shifts include Redshift optimization and EMR cluster resizing.
- Daily runs scheduled for 7 AM, with data quality checks and failure notifications ensuring reliability.
- Support for 100+ users managed through Redshift concurrency limits and cluster scaling.

This project demonstrates a robust implementation of a data pipeline from ingestion to analytics, leveraging cloud resources for scalability and efficiency.
