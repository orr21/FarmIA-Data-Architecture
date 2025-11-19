# Data Architecture Pipeline

This project implements a comprehensive data engineering pipeline designed to handle both real-time and batch data processing. The architecture leverages Azure cloud services and Databricks to ingest, process, store, and serve data from various sources.

## Architecture Overview

The pipeline follows a modern Lakehouse architecture, utilizing Delta Lake for reliable data storage and management. It supports streaming data from IoT devices and web applications, as well as batch data from operational systems.

![Architecture Diagram](images/architecture_diagram.png)

## Architectural Decisions & Data Flow

The architecture is designed to support the following data flow, corresponding to the numbered steps in the diagram:

### 1. Data Sources
The system ingests data from diverse sources to support various business needs:
*   **IoT Devices**: Sensors and smart devices generating real-time telemetry.
*   **E-commerce/Web**: Online shopping platforms and user activity logs.
*   **Users**: Customer data and demographic information.
*   **Logistics**: Supply chain and delivery tracking data.

### 2. Real-time Ingestion
High-velocity data is captured immediately:
*   **Azure IoT Hub**: Acts as the central message hub for bi-directional communication between the IoT application and the devices.
*   **Azure Event Hub**: A big data streaming platform and event ingestion service, capable of receiving and processing millions of events per second from web and user sources.

### 3. Stream Processing
*   **Databricks**: Processes the streaming data in real-time. This allows for immediate insights and actions.
*   **Direct Path to Serving**: In some critical low-latency scenarios, processed stream data can be pushed directly to the serving layer (Power BI) for real-time monitoring.

### 4. Stream to Lakehouse
*   Processed streaming data is also persisted into the **Bronze Lakehouse**. This ensures that all raw real-time data is captured for historical analysis and further refinement.

### 5. Batch Ingestion
*   **Azure Data Factory (ADF)**: Orchestrates and automates data movement and data transformation. It is used to ingest batch data from sources like Logistics and E-commerce databases into the Lakehouse.

### 6. Medallion Architecture (Batch Processing)
Data is refined through a multi-hop architecture within Databricks using Delta Lake:
*   **Bronze Lakehouse**: Stores raw data ingested from all sources (Stream and Batch). The data is kept in its original format.
*   **Silver Lakehouse**: Data is cleaned, filtered, and augmented. This layer represents a validated, enriched version of the data that can be trusted for downstream analysis.
*   **Gold Lakehouse**: Data is aggregated and organized into business-level tables (e.g., star schemas) ready for consumption. This is the "business truth" layer.

### 7. Data Serving
*   **Power BI**: Connects to the **Gold Lakehouse** (and optionally the streaming output) to provide interactive visualizations, dashboards, and business intelligence reports. This enables stakeholders to make data-driven decisions based on the most up-to-date information.
