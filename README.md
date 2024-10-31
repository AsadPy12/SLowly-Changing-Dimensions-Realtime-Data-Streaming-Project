# SLowly-Changing-Dimensions-Realtime-Data-Streaming-Project

This project demonstrates a real-time data pipeline for continuous data ingestion and transformation into a Snowflake data warehouse. It showcases the implementation of Change Data Capture <b>(CDC)</b> and Slowly Changing Dimensions <b>(SCD)</b> for historical data management using various cloud technologies.

## Architecture
![arc diagram](images\scd-architecture.drawio.png)

The system architecture is depicted in the provided diagram, which consists of:

- **EC2 Instance:** Hosts a Python script that generates fake customer data using the Faker library and stores it in a designated folder.
- **Docker:** Contains Apache NiFi and Jupyter Notebook for data movement and manipulation.
- **Apache NiFi:** Monitors the folder on the EC2 instance for newly created data files. It then uploads these files to an S3 bucket.
- **Amazon S3:** Stores the CSV files received from NiFi.
- **Snowpipe:** Automatically ingests data from the S3 bucket into a staging table in Snowflake.
- **Snowflake:**
    - **Staging Table:** A temporary table in Snowflake that receives data from Snowpipe.
    - **Target Table:** Stores the processed and updated customer data, reflecting the latest information.
    - **Historical Table:** Contains the historical records of customer data with SCD implemented.
    - **Stream:** Captures changes to the target table over time, facilitating the creation of historical records.
    - **Task:** Triggers a stored procedure periodically to perform CDC operations and maintain data integrity.
    - **Stored Procedure:** Contains the logic for merging data from the staging table into the target table and truncating the staging table.

## Data Flow
**1- Data Generation (EC2):** Python scripts generate fake customer data and store it in a CSV format within a designated folder.

**2- Data Movement (NiFi):** NiFi monitors the folder and uploads newly created files to the S3 bucket.

**3- Data Ingestion (Snowpipe):** Snowpipe automatically ingests data from the S3 bucket into a staging table in Snowflake.

**4- Data Transformation (Snowflake Stored Procedure):** A scheduled task triggers the stored procedure every minute:

- **MERGE Command (CDC):** Updates the target table based on the staging table. Inserts new records, updates existing ones, and deletes records as necessary.
    
- **Truncate Staging Table:** Prepares the staging table for the next batch of data.

**5- Historical Data Management (Snowflake Stream):** A Snowflake Stream captures changes to the target table over time, populating the historical table with SCD techniques.

## Key Takeaways
- Understanding the basics of SCD and its different types (Type 0, Type 1, Type 2, and Type 3).
- Visualizing the complete architecture of the system.
- Understanding the project and how to use AWS EC2 Instance and security groups.
- Introduction to Docker and its usage.
- Creation of Access key and S3 bucket.
- Test Data preparation.
- Understanding basics of NiFi and its integration with S3.
- Implementing NiFi flow setup.
- Introduction to different Snowflake components (Warehouse, Database, Schema, Table, View, Stored procedure, Snowpipe, Stream, and Task).
- Implementation of different Snowflake components.
- Implementation of SCD Type-1 and Type-2.

## Benefits
- **Real-time Data Pipeline:** Enables near real-time data updates in Snowflake.
- **Change Data Capture (CDC):** Ensures the target table reflects the latest data through inserts, updates, and deletes.
- **Slowly Changing Dimensions (SCD):** Maintains data integrity in the historical table through SCD techniques.
- **Scalability and Manageability:** Cloud-based technologies offer scalability for handling large data volumes.