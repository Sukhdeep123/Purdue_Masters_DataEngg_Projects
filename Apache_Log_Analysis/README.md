# Apache Server Log Analysis — PySpark & Hadoop

**Program:** PG Data Engineering — Purdue University (via Simplilearn)  
**Dataset:** NASA HTTP Server Access Logs  
**Environment:** Hadoop Distributed File System (HDFS) + PySpark on multi-node cluster

---

## Overview

This project simulates a real-world big data engineering scenario: a fast-growing analytics startup has ingested NASA's public HTTP server access logs into their Hadoop data lake and needs actionable insights derived at scale. Using **PySpark on HDFS**, the project performs distributed log parsing, transformation, and aggregation across a large-scale unstructured dataset.

The core objective was to demonstrate the end-to-end data engineering workflow: ingest raw logs from HDFS, parse semi-structured text with regex, apply RDD and DataFrame transformations, and surface operational KPIs for the platform team.

---

## Dataset

**Source:** NASA Kennedy Space Center HTTP access logs  
**Format:** Apache Common Log Format (CLF) — unstructured text  
**Scale:** Millions of HTTP request records

Sample log entry:
```
199.72.81.55 - - [01/Jul/1995:00:00:01 -0400] "GET /history/apollo/ HTTP/1.0" 200 6245
```

Fields parsed: `host`, `timestamp`, `HTTP method`, `endpoint URL`, `HTTP status code`, `response bytes`

---

## Engineering Tasks

### Task 1 — HTTP Status Code Distribution
Parsed all log records and computed the frequency distribution of HTTP response codes (200, 301, 302, 304, 404, 500, etc.). Identified the proportion of successful vs failed vs redirected requests across the full dataset.

### Task 2 — Sort Requests by Count (Descending)
Aggregated total request counts per endpoint and sorted results in descending order to identify the most frequently accessed resources on the server.

### Task 3 — Top 10 IP Visitors
Extracted client host/IP fields and computed a ranked list of the top 10 most active visitors by total request volume. Exposed high-traffic sources including bot traffic patterns.

### Task 4 — 404 URL Detection & Ranking
Filtered all log entries where HTTP status = 404 (Not Found). Aggregated 404 counts per URL and ranked them to identify broken or missing endpoints — critical for site reliability and SEO health.

### Task 5 — Daily Traffic Trends
Parsed timestamp fields to extract date components and computed per-day request volumes. Produced time-series aggregations to reveal traffic patterns, peak load days, and anomalies.

### Task 6 — Bandwidth Usage Analysis
Summed response byte values per endpoint and per day to compute total data served. Identified bandwidth-heavy endpoints and time windows — key inputs for capacity planning.

---

## Technical Approach

### Parsing Strategy
Used Python **regex** to parse the unstructured CLF log format into structured fields. Applied `sc.textFile()` to load raw logs from HDFS, then mapped each line through the regex parser to extract structured tuples.

### RDD vs DataFrame
- Initial transformations performed using **RDD operations** (`map`, `filter`, `reduceByKey`, `sortBy`) for fine-grained control
- Selected aggregations migrated to **PySpark DataFrames** + `spark.sql` for cleaner syntax and Catalyst optimizer benefits
- Combined approach demonstrates understanding of both the lower-level and higher-level PySpark APIs

### Distributed Execution
- All operations run in distributed mode across the Hadoop cluster
- HDFS used for both input data storage and intermediate output
- Spark executors parallelized transformations across partitions

---

## Key Results

| Analysis | Finding |
|---|---|
| Most common status | HTTP 200 (success) dominates, ~95%+ of traffic |
| Top 404 URLs | `/pub/winvn/readme.txt`, several `/shuttle/` paths |
| Peak traffic day | July 4th, 1995 (NASA shuttle launch coverage) |
| Top visitors | Mix of research crawlers and direct browser IPs |
| Highest bandwidth endpoint | `/shuttle/countdown/` video/image resources |

---

## Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3.x |
| Distributed Processing | Apache Spark (PySpark) |
| Storage | Apache Hadoop HDFS |
| Data Model | Spark RDDs + DataFrames |
| Parsing | Python Regex (`re` module) |
| Execution | YARN cluster manager |

---

## Files

```
Apache_Log_Analysis/
├── Apache_Log_Analysis_PySpark.ipynb    # Full PySpark analysis notebook
└── README.md
```

---

## Academic Context

**Course:** Big Data Engineering with Hadoop & Spark  
**Program:** Post Graduate Program in Data Engineering  
**Institution:** Purdue University (delivered via Simplilearn)  
**Domain:** Distributed Systems, Log Analytics, Big Data Processing
