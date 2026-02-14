<div align="center">

# 🚀 Data Engineer Bootcamp

### *From Zero to Production-Ready in 4 Months*

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![SQL](https://img.shields.io/badge/SQL-PostgreSQL-336791.svg)](https://www.postgresql.org/)
[![Apache Airflow](https://img.shields.io/badge/Apache-Airflow-017CEE.svg)](https://airflow.apache.org/)
[![AWS](https://img.shields.io/badge/AWS-Cloud-FF9900.svg)](https://aws.amazon.com/)
[![PySpark](https://img.shields.io/badge/Apache-Spark-E25A1C.svg)](https://spark.apache.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**A comprehensive, project-based learning path to become a job-ready Data Engineer**

[Roadmap](#-roadmap-overview) • [Getting Started](#-getting-started) • [Projects](#-key-projects) • [Resources](#-resources)

</div>

---

## 📋 Table of Contents

- [About This Bootcamp](#-about-this-bootcamp)
- [Roadmap Overview](#-roadmap-overview)
- [Month-by-Month Breakdown](#-month-by-month-breakdown)
  - [Month 1: The Syntax Foundation](#month-1-the-syntax-foundation-python--sql)
  - [Month 2: Orchestration & Modeling](#month-2-orchestration--modeling-the-engineer-phase)
  - [Month 3: The Cloud & Big Data](#month-3-the-cloud--big-data-aws--spark)
  - [Month 4: The Final Polish](#month-4-the-final-polish-interview-ready)
- [Daily Operating Routine](#-daily-operating-routine)
- [Key Projects](#-key-projects)
- [Tech Stack](#-tech-stack)
- [Strategic Advice](#-strategic-advice)
- [Getting Started](#-getting-started)
- [Resources](#-resources)

---

## 🎯 About This Bootcamp

This is an **intensive, project-driven Data Engineering bootcamp** designed to take you from beginner to job-ready in **4 months**. Unlike traditional courses, this program focuses on **building real-world projects** that demonstrate production-level skills.

### 📊 Program Details

| **Aspect** | **Details** |
|------------|-------------|
| **Duration** | 4 Months (Jan 2026 - April 2026) |
| **Time Commitment** | 3.5 hrs/day (Mon-Fri) + 6 hrs/day (Sat-Sun) |
| **Target Audience** | Aspiring Data Engineers, Software Engineers transitioning to DE |
| **Prerequisites** | Basic programming knowledge (helpful but not required) |
| **Outcome** | Production-ready portfolio + Interview skills |

### 🎓 What You'll Learn

- ✅ **Python & SQL Mastery** - Data manipulation, API integration, database operations
- ✅ **Workflow Orchestration** - Apache Airflow, DAGs, scheduling, automation
- ✅ **Cloud Engineering** - AWS (S3, Lambda, EC2), Infrastructure as Code
- ✅ **Big Data Processing** - PySpark, distributed computing, large-scale data handling
- ✅ **Data Warehousing** - Star/Snowflake schemas, OLTP vs OLAP, dimensional modeling
- ✅ **DevOps Fundamentals** - Docker, Git, Linux, CI/CD basics
- ✅ **System Design** - Scalable data pipelines, batch vs streaming, idempotency

---

## 🗺️ Roadmap Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     4-MONTH DATA ENGINEERING JOURNEY                     │
└─────────────────────────────────────────────────────────────────────────┘

Month 1            Month 2            Month 3            Month 4
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🐍 Python         🔄 Airflow         ☁️  AWS            🎯 Interview
📊 SQL            🐳 Docker          ⚡ Spark           📝 Resume
📁 Pandas         🏗️  Modeling       🪣 S3/Lambda       🏆 Capstone
                                                        
Deliverable:      Deliverable:       Deliverable:       Deliverable:
basics-etl        News Fetcher       PySpark            Job Market
repo              DAG                Analysis           Analyzer
```

### 🎯 Monthly Checkpoints

Each month has a **mandatory deliverable** that proves mastery:

| Month | Checkpoint | Skills Validated |
|-------|-----------|------------------|
| **1** | `basics-etl` GitHub repo | Python, Pandas, SQL, SQLAlchemy |
| **2** | Airflow News Fetcher DAG | Orchestration, Automation, Git |
| **3** | PySpark Analysis + AWS S3 | Cloud, Big Data, Distributed Computing |
| **4** | Capstone: Job Market Analyzer | End-to-end pipeline, System Design |

---

## 📅 Month-by-Month Breakdown

### Month 1: The Syntax Foundation (Python & SQL)

**🎯 Goal:** Be able to manipulate data fluently without looking up syntax every 2 minutes.

<details>
<summary><b>📖 Week 1: Python Data Structures & Logic</b></summary>

**Focus:** Dictionaries, Lists, Sets, String Manipulation, Error Handling (`try/except`)

**Daily Tasks (Mon-Fri):**
- Learn syntax fundamentals
- Solve 3 LeetCode Easy problems (Array/String)

**Weekend Project:**
- 🛠️ **File Organizer Script** - Scans Downloads folder and organizes files into images/docs/videos folders

</details>

<details>
<summary><b>📖 Week 2: Python for Data (Pandas/Requests)</b></summary>

**Focus:** Reading CSV/JSON, `requests` library (API calls), Pandas DataFrames (Filtering, Cleaning `NaN`)

**Daily Tasks (Mon-Fri):**
- Practice cleaning messy datasets (e.g., Titanic dataset)

**Weekend Project:**
- 🛠️ **Crypto-Tracker** - Fetch Bitcoin price from public API and save to CSV with timestamps

</details>

<details>
<summary><b>📖 Week 3: SQL Fundamentals</b></summary>

**Focus:** `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `JOINS` (Inner vs Left)

**Daily Tasks (Mon-Fri):**
- HackerRank SQL (Basic Select → Aggregation)

**Weekend Project:**
- 🛠️ Setup PostgreSQL locally, import CSV, practice queries

</details>

<details>
<summary><b>📖 Week 4: SQL "The Interview Killers"</b></summary>

**Focus:** **Window Functions** (`RANK`, `ROW_NUMBER`, `LEAD/LAG`), CTEs, Date manipulation

**Daily Tasks (Mon-Fri):**
- HackerRank SQL (Intermediate/Advanced)

**Weekend Project:**
- 🛠️ Solve "Department Top Three Salaries" (LeetCode Hard/Medium SQL) locally

</details>

#### 🔴 Month 1 Checkpoint (Must Deliver)
A GitHub repo called **`basics-etl`** containing:
- Python script that reads CSV
- Cleans data using Pandas
- Loads into local Postgres using SQLAlchemy

---

### Month 2: Orchestration & Modeling (The "Engineer" Phase)

**🎯 Goal:** Automate scripts so they run without manual intervention.

<details>
<summary><b>📖 Week 5: Linux, Git & Docker Basics</b></summary>

**Focus:** CLI commands (`grep`, `cron`, `ssh`), Git (`commit`, `branch`, `push`), Docker (`Dockerfile`, `docker-compose`)

**Daily Tasks (Mon-Fri):**
- Navigate OS via terminal only

**Weekend Project:**
- 🛠️ Containerize Month 1 script (run inside Docker container)

</details>

<details>
<summary><b>📖 Week 6: Apache Airflow (Concepts)</b></summary>

**Focus:** DAGs, Operators (BashOperator, PythonOperator), Scheduling, XComs

**Daily Tasks (Mon-Fri):**
- Install Airflow via Docker
- Run tutorial DAGs

**Weekend Project:**
- 🛠️ Write "Hello World" DAG that prints date/time every minute

</details>

<details>
<summary><b>📖 Week 7: Airflow (Implementation)</b></summary>

**Focus:** Hooks (DB connections), Sensors (waiting for files)

**Daily Tasks (Mon-Fri):**
- Connect Airflow to local Postgres

**Weekend Project:**
- 🛠️ **Automated News Fetcher** - DAG that pulls news headlines via API → Cleans text → Saves to DB (runs daily)

</details>

<details>
<summary><b>📖 Week 8: Data Warehousing Theory</b></summary>

**Focus:** OLTP vs OLAP, Star Schema, Snowflake Schema, Fact vs Dimension tables

**Daily Tasks (Mon-Fri):**
- Read "The Data Warehouse Toolkit" summaries
- Understand *why* we structure data this way

**Weekend Project:**
- 🛠️ Draw Star Schema for theoretical E-commerce store (Orders, Customers, Products)

</details>

#### 🔴 Month 2 Checkpoint (Must Deliver)
Screenshot of Airflow UI with **all green lights** for "News Fetcher" DAG + code pushed to Git

---

### Month 3: The Cloud & Big Data (AWS & Spark)

**🎯 Goal:** Move from "Localhost" to "Cloud" and handle larger files.

<details>
<summary><b>📖 Week 9: AWS Essentials (The Free Tier)</b></summary>

**Focus:** S3 (Buckets, partitions), IAM (Roles/Policies), EC2 (Basic VM)

**Daily Tasks (Mon-Fri):**
- Create AWS account
- Manually upload files to S3

**Weekend Project:**
- 🛠️ Python script using `boto3` to upload/download files to S3 programmatically

</details>

<details>
<summary><b>📖 Week 10: Serverless ETL (Lambda)</b></summary>

**Focus:** AWS Lambda functions, Triggers (S3 Event Notifications)

**Daily Tasks (Mon-Fri):**
- Create Lambda that triggers on S3 file upload

**Weekend Project:**
- 🛠️ **Thumbnail Generator** - Upload image to S3 → Lambda triggers → Resizes image → Saves to new bucket

</details>

<details>
<summary><b>📖 Week 11: PySpark Basics</b></summary>

**Focus:** Spark Architecture (Driver/Worker), Reading/Writing Parquet, Basic Transformations

**Daily Tasks (Mon-Fri):**
- Setup PySpark on Google Colab

**Weekend Project:**
- 🛠️ Re-write Month 1 Pandas logic using PySpark, compare syntax

</details>

<details>
<summary><b>📖 Week 12: PySpark Intermediate</b></summary>

**Focus:** `partitionBy`, Handling Nulls in Spark, Spark SQL

**Daily Tasks (Mon-Fri):**
- Practice Spark SQL syntax

**Weekend Project:**
- 🛠️ Process large public dataset (NYC Taxi Data) to find "Average trip distance per hour"

</details>

#### 🔴 Month 3 Checkpoint (Must Deliver)
- Colab Notebook link showing PySpark analysis of large dataset
- AWS S3 bucket structure created via code

---

### Month 4: The Final Polish (Interview Ready)

**🎯 Goal:** Resume, Portfolio, and Mock Interviews.

<details>
<summary><b>📖 Week 13: System Design (Data Focused)</b></summary>

**Focus:** Batch vs Streaming, Idempotency, Backfilling data

**Daily Tasks (Mon-Fri):**
- Watch "System Design Primer" videos (Gaurav Sen / ByteByteGo)

**Weekend Project:**
- 🛠️ Design pipeline on whiteboard: "How would you design a dashboard for Swiggy delivery times?"

</details>

<details>
<summary><b>📖 Week 14: The Capstone Project</b></summary>

**Focus:** Combining AWS + Airflow + Spark/Python + SQL

**🏆 Task: Build "The Job Market Analyzer"**
1. Scrape job data (Python)
2. Store raw data in S3 (Boto3)
3. Airflow triggers Spark job
4. Spark cleans data
5. Load into Warehouse (Snowflake Trial or Postgres)

</details>

<details>
<summary><b>📖 Week 15: LeetCode & SQL Grind</b></summary>

**Focus:** Speed

**Daily Tasks (Mon-Fri):**
- 3 SQL Mediums + 2 Python Mediums (Arrays/HashMaps) per day

**Weekend Project:**
- 🛠️ Mock Interview (Pramp/Interviewing.io)

</details>

<details>
<summary><b>📖 Week 16: Resume & Application Strategy</b></summary>

**Focus:** ATS Keywords

**Task:**
- Rewrite resume with impact metrics
- Example: "Built a pipeline..." → "Designed an automated ETL pipeline using Airflow and AWS S3 reducing manual data entry by 100%"

</details>

#### 🔴 Month 4 Checkpoint (Must Deliver)
**Capstone Project** documented in `README.md` with architecture diagram. **This is your ticket to the job.**

---

## ⏰ Daily Operating Routine

### 📅 Monday - Friday (3.5 Hours)

| Time | Activity | Description |
|------|----------|-------------|
| **0 - 30 mins** | 🔥 **Warm-up** | 1 SQL Query (Medium) + 1 Python (Easy). *Do not skip.* |
| **30 mins - 2.5 hrs** | 📚 **Learning** | Watch tutorials, read docs related to Weekly Topic |
| **2.5 hrs - 3.5 hrs** | 💻 **Implementation** | Write code. If you watched a video on S3, write a script to list S3 buckets |

### 📅 Saturday & Sunday (6 Hours)

| Time | Activity | Description |
|------|----------|-------------|
| **Hour 1-2** | 🔍 **Review** | Fix bugs from the week |
| **Hour 2-6** | 🚀 **Deep Work** | Weekend Project - This is where you actually learn. You will get stuck. You will debug errors. **This is the job.** |

---

## 🏆 Key Projects

### 1. 📁 File Organizer (Week 1)
**Tech:** Python, OS module, File I/O  
**Impact:** Automates file management

### 2. 💰 Crypto-Tracker (Week 2)
**Tech:** Python, Requests, Pandas, CSV  
**Impact:** Real-time data collection from APIs

### 3. 🗄️ basics-etl (Month 1 Checkpoint)
**Tech:** Python, Pandas, PostgreSQL, SQLAlchemy  
**Impact:** End-to-end ETL pipeline

### 4. 📰 Automated News Fetcher (Week 7)
**Tech:** Apache Airflow, Python, PostgreSQL, APIs  
**Impact:** Production-grade workflow orchestration

### 5. 🖼️ Thumbnail Generator (Week 10)
**Tech:** AWS Lambda, S3, Python (PIL/Pillow), Event-driven architecture  
**Impact:** Serverless, scalable image processing

### 6. 🚕 NYC Taxi Analysis (Week 12)
**Tech:** PySpark, Parquet, Distributed Computing  
**Impact:** Big data processing at scale

### 7. 💼 Job Market Analyzer (Week 14) - **CAPSTONE**
**Tech:** Python, AWS S3, Apache Airflow, PySpark, PostgreSQL/Snowflake  
**Impact:** Full-stack data engineering pipeline demonstrating all learned skills

---

## 🛠️ Tech Stack

### **Languages**
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)

### **Data Processing**
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache_Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)

### **Orchestration**
![Apache Airflow](https://img.shields.io/badge/Apache_Airflow-017CEE?style=for-the-badge&logo=apache-airflow&logoColor=white)

### **Cloud & Infrastructure**
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)

### **Tools & Platforms**
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

---

## 💡 Strategic Advice

> **"Startups in India (Bangalore/Pune/NCR) hire based on GitHub activity."**

### 🎯 Key Success Factors

1. **📊 GitHub Consistency**  
   Green squares for 4 months with Airflow/AWS/Spark projects = calls from Tier 2 companies (even without fancy college tag)

2. **🏗️ Project Over Theory**  
   Employers care more about "Can you build it?" than "Do you know the theory?"

3. **📝 Documentation Matters**  
   Every project needs a clear README with:
   - Architecture diagram
   - Setup instructions
   - Technologies used
   - Challenges faced & solutions

4. **🔄 Consistency > Intensity**  
   3.5 hours daily beats 12 hours on weekends

5. **🐛 Embrace Debugging**  
   Weekend projects will break. That's the point. Debugging IS learning.

### 🚀 Career Trajectory

```
Month 1-2: Building Foundation
    ↓
Month 3: Cloud & Scale
    ↓
Month 4: Interview Prep + Portfolio Polish
    ↓
May-June: Applications + Final Exams
    ↓
🎯 GOAL: Junior Data Engineer Role
```

---

## 🚀 Getting Started

### Prerequisites

- Computer with 8GB+ RAM
- Stable internet connection
- GitHub account
- AWS Free Tier account (Month 3)
- Willingness to debug errors for hours 😅

### Installation & Setup

```bash
# Clone this repository
git clone https://github.com/Aashutosh-357/Data-Engineer-Bootcamp.git
cd Data-Engineer-Bootcamp

# Follow weekly setup instructions in respective folders
# Each week has its own README with specific setup steps
```

### Weekly Structure

```
Data-Engineer-Bootcamp/
├── Month-1-Foundation/
│   ├── Week-01-Python-Basics/
│   ├── Week-02-Pandas-Requests/
│   ├── Week-03-SQL-Fundamentals/
│   └── Week-04-SQL-Advanced/
├── Month-2-Orchestration/
│   ├── Week-05-Linux-Git-Docker/
│   ├── Week-06-Airflow-Concepts/
│   ├── Week-07-Airflow-Implementation/
│   └── Week-08-Data-Warehousing/
├── Month-3-Cloud-BigData/
│   ├── Week-09-AWS-Essentials/
│   ├── Week-10-Lambda-Serverless/
│   ├── Week-11-PySpark-Basics/
│   └── Week-12-PySpark-Intermediate/
└── Month-4-Interview-Prep/
    ├── Week-13-System-Design/
    ├── Week-14-Capstone-Project/
    ├── Week-15-LeetCode-Grind/
    └── Week-16-Resume-Applications/
```

---

## 📚 Resources

### 📖 Learning Platforms

- **LeetCode** - SQL & Python practice
- **HackerRank** - SQL fundamentals
- **Pramp/Interviewing.io** - Mock interviews
- **Google Colab** - Free PySpark environment
- **YouTube** - Gaurav Sen, ByteByteGo (System Design)

### 📘 Books & Documentation

- "The Data Warehouse Toolkit" by Ralph Kimball
- Apache Airflow Official Docs
- AWS Documentation
- PySpark Documentation

### 🔗 Useful Links

- [Apache Airflow Docs](https://airflow.apache.org/docs/)
- [AWS Free Tier](https://aws.amazon.com/free/)
- [PySpark Tutorial](https://spark.apache.org/docs/latest/api/python/)
- [PostgreSQL Tutorial](https://www.postgresql.org/docs/)

---

## 📊 Progress Tracking

Track your progress using this checklist:

- [ ] **Month 1 Complete** - `basics-etl` repo created
- [ ] **Month 2 Complete** - Airflow News Fetcher running
- [ ] **Month 3 Complete** - PySpark analysis + AWS S3 setup
- [ ] **Month 4 Complete** - Capstone project deployed
- [ ] **Resume Updated** - ATS-optimized with project metrics
- [ ] **GitHub Profile** - 4 months of green squares
- [ ] **Portfolio Ready** - All projects documented

---

## 🤝 Contributing

This is a personal learning journey, but if you're following along and find improvements:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -m 'Add some improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🎯 Final Words

> **"Execute Week 1 starting Monday (5th January). Good luck."**

Remember:
- **Consistency beats intensity**
- **Projects beat theory**
- **Debugging is learning**
- **Your GitHub is your resume**

### 🔥 Let's Build Something Amazing!

<div align="center">

**Start Date:** January 5, 2026  
**Target Completion:** April 2026  
**Next Step:** Applications & Final Exams (May-June 2026)

---

Made with ❤️ and ☕ by aspiring Data Engineers

**⭐ Star this repo if you're following along!**

</div>