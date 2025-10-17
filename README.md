
📌 Overview

This project demonstrates a hybrid real-time and batch data pipeline for processing, analyzing, and visualizing NYC Taxi trip data (Yellow & Green cabs).
The pipeline was built to:

Stream and process live taxi trip data every 2 minutes.
Perform transformations and aggregations in real-time.
Store data for both real-time analytics (Cosmos DB) and batch analytics (Snowflake).
Visualize insights such as trip patterns, vendor performance, and congestion trends using Power BI.



🛠️ Tech Stack

Azure Function → Acts as a producer, fetching 100 Yellow & 100 Green taxi trip rows every 2 minutes from Blob Storage and sending them to Event Hub.
Azure Event Hub → Message broker for streaming data between producer and consumer.
Azure Stream Analytics → Real-time data processing & transformations; sends data to Cosmos DB and Blob Storage.
Azure Blob Storage → Holds both raw and transformed data; includes:

Snowflake container (for transformed data)
Dropped container (for failed records)
Event Grid + Storage Queue + Snowpipe → Automates loading transformed files into Snowflake staging area.
Snowflake → Stores cleaned and merged data for batch analytics using scheduled merge tasks.
Cosmos DB → Stores real-time transformed data for immediate dashboard updates.
Power BI → Visualizes both real-time (Cosmos DB) and batch-time (Snowflake) analytics.



🔄 Data Pipeline

1️⃣ Data Ingestion

Azure Function retrieves files from Blob Storage every 2 minutes.
Sends mixed Yellow & Green taxi data (100 rows each) to Event Hub.

2️⃣ Real-Time Stream Processing

Azure Stream Analytics consumes messages from Event Hub.
Performs transformations (cleaning, aggregation, schema standardization).
Outputs results to:

Cosmos DB → For real-time dashboards.
Transformed Blob Container → For further batch processing.

3️⃣ Batch Loading & Transformation

Event Grid triggers Snowpipe when new files land in the transformed container.
Snowpipe loads data into a staging area inside Snowflake.
A merge task runs every minute to update the fact table from staging.

4️⃣ Analytics Layer

Batch Analytics (Snowflake → Power BI):

Total Tip Amount per Vendor.
Weekly Revenue Trends per Vendor.
Payment Type Distribution.
Avg Revenue with vs without Congestion Fee.
Top 3 Pickup Zones.
Trip Count by Borough.

Real-Time Analytics (Cosmos DB → Power BI):

Total Trips by Hour.
Avg Trip Duration per Hour.
Most Congested Pickup Areas in the Last Hour.
Highest Revenue Hour in the Last Day.



📊 Example Dashboards

Batch Analysis Dashboard (Snowflake):

Vendor revenue trends
Congestion fee impact on fares
Top pickup & drop-off zones

Real-Time Dashboard (Cosmos DB):

Live trip count by hour
Dynamic map of top pickup zones (last 60 minutes)
Live revenue updates per vendor



🚀 How to Run

Upload Yellow & Green Taxi CSV files into Azure Blob Storage.
Deploy the Azure Function to fetch and stream data into Event Hub.
Configure Azure Stream Analytics to process the stream and write to Cosmos DB & Blob Storage.
Set up Snowpipe + Event Grid for automatic loading into Snowflake.
Connect Power BI to both Cosmos DB (for real-time) and Snowflake (for batch).
Build dashboards to visualize insights.



💡 Future Improvements

Implement Data Quality Monitoring using Azure Monitor.
Introduce incremental refresh and auto-scaling for large datasets.
Add geospatial heatmaps to Power BI for pickup/drop-off visualization.



🧩 Architecture Diagram

(Insert your architecture image here)
