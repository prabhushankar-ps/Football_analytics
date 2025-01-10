# Wikipedia Data Pipeline

## Overview

This project implements an Apache Airflow pipeline to extract, transform, and load data about football stadiums from a Wikipedia page. The extracted data is stored in an Azure Data Lake for further analysis.

## Architecture

The project consists of the following components:

1. **Airflow DAG:**
   - Defines the sequence of tasks: extraction, transformation, and storage.
2. **Python Scripts:**
   - `wikipedia_pipeline.py`: Implements ETL logic for data extraction, transformation, and loading.
   - `wikipedia_flow.py`: Airflow DAG configuration.
3. **Docker Setup:**
   - `docker-compose.yml`: Configures Airflow components and PostgreSQL database.
   - `entrypoint.sh`: Sets up Airflow and initializes the environment.
4. **Dependencies:**
   - Specified in `requirements.txt` for reproducibility.

## Features

1. **Extraction:** Fetches HTML content from Wikipedia using Python's `requests` library.
2. **Transformation:** Cleans and processes the extracted data using Pandas and Geopy.
3. **Loading:** Stores the cleaned data in Azure Data Lake using the `adlfs` library.
4. **Orchestration:** Utilizes Apache Airflow to automate the workflow.

---

## Prerequisites

- Docker and Docker Compose
- Azure Storage Account

## Installation

1. Clone the repository:

   ```bash
   git clone <repository_url>
   cd <repository_folder>
   ```

2. Set up your Azure Storage Account credentials:

   - Update the account key and connection details in `wikipedia_pipeline.py`:

     ```python
     storage_options={
         'account_key': 'YOUR_ACCOUNT_KEY'
     }
     ```

3. Build and start the Docker containers:

   ```bash
   docker-compose up --build
   ```

4. Access the Airflow web interface:

   - URL: `http://localhost:8080`
   - Default credentials:
     - Username: `admin`
     - Password: `admin`

---

## Azure Integration

### Configuration

- The cleaned data is stored in Azure Data Lake.
- The required Python libraries (`adlfs`, `azure-storage-blob`, etc.) are listed in `requirements.txt`.

### Data Storage Path

- The output file is saved in the Azure Data Lake container:
  `abfs://footballdataeng@footballdataeng.dfs.core.windows.net/data/`

---

## Airflow DAG

### DAG Tasks

1. **Extract:** Downloads HTML from Wikipedia.
2. **Transform:** Parses and processes the HTML data.
3. **Load:** Uploads the cleaned data to Azure Data Lake.

### DAG Configuration

- DAG ID: `wikipedia_flow`
- Start Date: `2023-10-01`
- Schedule Interval: `None`

---

## Files Description

| File                 | Description                                                  |
|----------------------|--------------------------------------------------------------|
| `wikipedia_flow.py`  | Defines the Airflow DAG and tasks.                           |
| `wikipedia_pipeline.py` | Contains ETL functions for extracting, transforming, and loading data. |
| `docker-compose.yml` | Configures Docker services for Airflow and PostgreSQL.       |
| `requirements.txt`   | Lists Python dependencies.                                   |
| `entrypoint.sh`      | Initializes the Airflow environment.                         |

---

## How to Run

1. Start the pipeline by enabling the DAG in the Airflow web interface.
2. Monitor the task execution via the Airflow interface.

---

## Troubleshooting

1. **Airflow webserver not starting:**
   - Check logs: `docker logs <container_name>`

2. **Azure upload issues:**
   - Verify Azure credentials and storage configuration.

3. **Dependencies missing:**
   - Ensure all libraries are installed by checking `requirements.txt`.

---

## Future Enhancements

- Add automated email notifications for pipeline success/failure.
- Implement unit tests for ETL functions.
- Add scalability with Kubernetes deployment.
