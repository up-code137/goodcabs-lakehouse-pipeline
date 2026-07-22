# 🚕 GoodCabs Regional Analytics — LakeFlow Spark Declarative Pipeline

> An End-to-End Data Engineering project built on Databricks Free Edition using LakeFlow Spark Declarative Pipelines, Medallion Architecture, and AWS S3.

---

## 📌 Project Background

**GoodCabs** is a fast-growing cab services company operating across multiple cities in India. As the business scaled, a serious problem emerged:

- 🔴 Regional managers were **not receiving data on time**
- 🔴 Dashboards were **generic and hard to use**
- 🔴 Teams were **manually reworking data** for every region
- 🔴 Existing pipelines were **tightly coupled and slow to adapt**

This was no longer a local issue — it was a **systematic data platform problem.**

The solution? Migrate from traditional manual PySpark pipelines to **LakeFlow Spark Declarative Pipelines** on Databricks — enabling automatic orchestration, incremental processing, and region-specific analytics at scale.

---

## 🎯 Business Questions Answered

- 📊 What is the **average customer rating per city**?
- 🚗 What is the **total number of rides per region**?
- 📍 Which **regions are performing best** and which need attention?
- 📅 How does **trip volume change over time** across cities?

---

## 🏗️ Architecture

```
Application Server
       ↓
Relational Database
       ↓
  AWS S3 Bucket
       ↓
  Databricks
  ┌────────────────────────────────┐
  │  BRONZE LAYER                  │
  │  Raw data ingestion from S3    │
  │  • city.py                     │
  │  • trips.py                    │
  └────────────┬───────────────────┘
               ↓
  ┌────────────────────────────────┐
  │  SILVER LAYER                  │
  │  Cleaned & transformed data    │
  │  • silver_city.py              │
  │  • silver_trips.py             │
  │  • calendar.py                 │
  └────────────┬───────────────────┘
               ↓
  ┌────────────────────────────────┐
  │  GOLD LAYER                    │
  │  Business-ready fact tables    │
  │  • City-wise fact tables       │
  │  • Regional analytics          │
  └────────────────────────────────┘
               ↓
        Dashboard / Reports
```

---

## ⚡ What Makes This Project Special — LakeFlow Declarative Pipelines

### Traditional Way (Imperative) ❌
```python
# You write every step manually
df = spark.read.csv("s3://bucket/trips.csv")
df_cleaned = df.filter(df.age > 0)
df_cleaned = df_cleaned.dropDuplicates()
df_cleaned.write.saveAsTable("silver.trips")
# More code, more errors, manual orchestration
```

### LakeFlow Way (Declarative) ✅
```python
# You just define WHAT you want
@dlt.table
def silver_trips():
    return (
        dlt.read("bronze_trips")
        .filter("trip_distance > 0")
        .dropDuplicates()
    )
# Databricks handles HOW automatically
```

### Benefits of Declarative Pipelines
| Feature | Benefit |
|---|---|
| Automatic Orchestration | Databricks manages execution order automatically |
| Incremental Processing | Only new data gets processed — faster and efficient |
| Error Handling | Built-in error tracking and recovery |
| Less Code | Shorter, cleaner, less error-prone code |
| Auto Dependencies | System resolves table dependencies automatically |

---

## 🔄 Incremental Loading — How It Works

Unlike time-based triggers, this pipeline uses **file-based incremental loading:**

> Whenever a new file is dropped into AWS S3 → Databricks automatically detects it → Pipeline runs → Data updated

No manual intervention needed. No scheduled jobs. Fully automated.

---

## 📁 Project Structure

```
goodcabs-lakehouse-pipeline/
│
├── data/
│   ├── city_data/          # City dimension data
│   ├── trips_data1/        # Trip fact data (part 1)
│   └── trips_data2/        # Trip fact data (part 2)
│
├── pipeline/
│   ├── bronze/             # Raw ingestion layer
│   ├── silver/             # Cleaning & transformation layer
│   └── gold/               # Business-ready fact tables
│
├── screenshots/            # Project screenshots
├── resources/              # Additional project files
└── README.md
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Databricks Free Edition** | Pipeline development and execution |
| **LakeFlow Spark Declarative Pipelines** | ETL pipeline orchestration |
| **AWS S3** | Raw data storage and ingestion |
| **PySpark** | Data transformation and processing |
| **Python** | Pipeline code and logic |
| **SQL** | Data querying and analytics |
| **Delta Lake** | ACID transactions and incremental loading |
| **Medallion Architecture** | Bronze/Silver/Gold data layers |

---

## 🔑 Key Concepts Learned

### LSDP (LakeFlow Spark Declarative Pipelines) Components

1. **Flow** — Define functions and materialized views
2. **Flow Types:**
   - Materialized View — precomputed query results
   - Auto Streaming Table — continuous data ingestion
3. **Sink** — Streaming pipeline supporting Delta tables
4. **Pipeline** — Complete end-to-end declarative workflow
5. **Incremental Loading** — Process only new/changed data

---

## 📊 Results

- ✅ City-wise fact tables created for all 10 cities
- ✅ Regional managers can now access their specific data directly
- ✅ No more manual data reworking
- ✅ Automatic pipeline execution on new data arrival
- ✅ Role-based access control implemented per city

---

## 🚀 How To Run This Project

### Prerequisites
- Databricks Free Edition account — [Sign up here](https://www.databricks.com/try-databricks)
- AWS account with S3 access
- Python 3.8+

### Steps
1. Clone this repository
```bash
git clone https://github.com/up-code137/goodcabs-lakehouse-pipeline.git
```
2. Upload data files to your AWS S3 bucket
3. Connect Databricks to AWS S3 using external connection
4. Create catalog and schemas in Databricks
5. Import pipeline code files into Databricks workspace
6. Create ETL pipeline using LakeFlow in Databricks
7. Run pipeline and monitor execution

---

## 📸 Screenshots

Check the `screenshots/` folder for step-by-step project screenshots including:
- Databricks setup
- AWS S3 connection
- Pipeline execution
- Gold layer fact tables
- Access management

---

## 👨‍💻 About The Developer

**Sameer Sinha**
Final Year B.Tech IT Student | Maharaja Surajmal Institute of Technology, Delhi

Aspiring Data Engineer specializing in Databricks Lakehouse Architecture

🔗 [LinkedIn](https://linkedin.com/in/sameersinha-de) | [GitHub](https://github.com/up-code137) | 📧 sameersinha137@gmail.com

---

## 🙏 Credits

Project inspired by [Codebasics](https://www.youtube.com/@codebasics) Data Engineering course.

---

⭐ If you found this project useful, please give it a star!
