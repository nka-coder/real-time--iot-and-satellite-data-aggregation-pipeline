# Technical Architecture: Real-Time IoT & Satellite Telemetry Pipeline

## 1. High-Throughput Ingestion
* **Source:** IoT Sensors (Telemetry) and Satellite Imagery (Spectral Bands).
* **Mechanism:** Decoupled messaging backbone via **Google Cloud Pub/Sub**.
* **Technical Detail:** Ingests millions of events per day using a push/pull subscription model, providing a resilient buffer that absorbs spikes in satellite data uploads and high-frequency IoT bursts without downstream backpressure.

## 2. Stream Processing & Enrichment
* **Runtime:** **Apache Beam** executed on **Google Cloud Dataflow**.
* **Process Flow:**
    * **Windowing:** Implements sliding and tumbling windows to aggregate telemetry data into discrete time intervals.
    * **Deduplication:** Utilizes Beam's stateful processing to filter out redundant sensor packets caused by network retries.
    * **Enrichment:** Joins raw spectral data with geographic metadata to calculate real-time **NDVI (Normalized Difference Vegetation Index)** indices for environmental health monitoring.

## 3. Storage & Analytical Layer
* **Warehouse:** BigQuery).
* **Data Strategy:** Real-time streaming inserts into partitioned and clustered tables.
* **Technical Detail:** Normalizes IoT telemetry and satellite indices into a unified schema, allowing sub-second analytical queries across petabytes of historical asset data for trend analysis.

## 4. Automated Governance & Monitoring
* **Mechanism:** Event-driven **Cloud Functions**.
* **Logic Layer:**
    * **Anomaly Detection:** Triggers on BigQuery "Insert Errors" or data quality threshold breaches (e.g., out-of-range NDVI values).
    * **Alerting:** Integration with Cloud Monitoring to notify stakeholders of sensor malfunctions or environmental anomalies.
* **Downstream Integration:** Serves as the high-quality feature store for predictive models targeting asset maintenance and crop yield forecasting.
