
**Data-Engineer-Roadmap**

**Current Date:** Jan 4(Sunday), 2026.
**Target Finish:** April 2026 (Leaving May/June for applications & final exams).
**Constraint:** 3.5 Hours (Mon-Fri) | 6 Hours (Sat-Sun).

Here is our execution order.

---

### **Month 1: The Syntax Foundation (Python & SQL)**
**Goal:** Be able to manipulate data fluently without looking up syntax every 2 minutes.

*   **Week 1: Python Data Structures & Logic**
    *   *Focus:* Dictionaries, Lists, Sets, String Manipulation, Error Handling (`try/except`).
    *   *Mon-Fri:* Learn syntax + solve 3 LeetCode Easy problems (Array/String).
    *   *Weekend:* Build a "File Organizer" script (Scans our Downloads folder and moves files into images/docs/videos folders).
*   **Week 2: Python for Data (Pandas/Requests)**
    *   *Focus:* Reading CSV/JSON, `requests` library (API calls), Pandas DataFrames (Filtering, Cleaning `NaN`).
    *   *Mon-Fri:* Practice cleaning messy datasets (e.g., Titanic dataset).
    *   *Weekend:* Project: "Crypto-Tracker." Fetch Bitcoin price from a public API and save it to a CSV with a timestamp.
*   **Week 3: SQL Fundamentals**
    *   *Focus:* `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `JOINS` (Inner vs Left).
    *   *Mon-Fri:* HackerRank SQL (Basic Select -> Aggregation).
    *   *Weekend:* Setup PostgreSQL locally. Import a CSV. Query it.
*   **Week 4: SQL "The Interview Killers"**
    *   *Focus:* **Window Functions** (`RANK`, `ROW_NUMBER`, `LEAD/LAG`), Common Table Expressions (CTEs), Date manipulation.
    *   *Mon-Fri:* HackerRank SQL (Intermediate/Advanced).
    *   *Weekend:* Solve "Department Top Three Salaries" (LeetCode Hard/Medium SQL) locally.

**🔴 Month 1 Checkpoint (Must Deliver):**
A GitHub repo called `basics-etl`. Inside, a Python script that reads a CSV, cleans it using Pandas, and loads it into a local Postgres Table using SQLalchemy.

---

### **Month 2: Orchestration & Modeling (The "Engineer" Phase)**
**Goal:** Automate the scripts so they run without us touching them.

*   **Week 5: Linux, Git & Docker Basics**
    *   *Focus:* CLI commands (`grep`, `cron`, `ssh`), Git (`commit`, `branch`, `push`), Basic Docker (`Dockerfile`, `docker-compose`).
    *   *Mon-Fri:* Learn to navigate our OS via terminal only.
    *   *Weekend:* Containerize our Month 1 script (Make it run inside a Docker container).
*   **Week 6: Apache Airflow (Concepts)**
    *   *Focus:* DAGs, Operators (BashOperator, PythonOperator), Scheduling, XComs (passing data between tasks).
    *   *Mon-Fri:* Install Airflow via Docker. Run the tutorial DAGs.
    *   *Weekend:* Write a "Hello World" DAG that prints the date and time every minute.
*   **Week 7: Airflow (Implementation)**
    *   *Focus:* Hooks (connecting to DBs), Sensors (waiting for files).
    *   *Mon-Fri:* Connect Airflow to our local Postgres.
    *   *Weekend:* **Project: "Automated News Fetcher."** A DAG that pulls news headlines via API -> Cleans text -> Saves to DB. Runs daily.
*   **Week 8: Data Warehousing Theory**
    *   *Focus:* OLTP vs OLAP, Star Schema, Snowflake Schema, Fact vs Dimension tables.
    *   *Mon-Fri:* Read "The Data Warehouse Toolkit" (Summaries). Understand *why* we structure data this way.
    *   *Weekend:* Draw a Star Schema for a theoretical E-commerce store (Orders, Customers, Products).

**🔴 Month 2 Checkpoint (Must Deliver):**
A screenshot of our Airflow UI with all green lights for our "News Fetcher" DAG, and the code pushed to Git.

---

### **Month 3: The Cloud & Big Data (AWS & Spark)**
**Goal:** Move from "Localhost" to "Cloud" and handle larger files.

*   **Week 9: AWS Essentials (The Free Tier)**
    *   *Focus:* S3 (Buckets, partitions), IAM (Roles/Policies), EC2 (Basic VM).
    *   *Mon-Fri:* Create an AWS account. Manually upload files to S3.
    *   *Weekend:* Write a Python script (using `boto3` library) to upload/download files to S3 programmatically.
*   **Week 10: Serverless ETL (Lambda)**
    *   *Focus:* AWS Lambda functions, Triggers (S3 Event Notifications).
    *   *Mon-Fri:* Create a Lambda that triggers when a file lands in S3.
    *   *Weekend:* Project: "Thumbnail Generator." Upload image to S3 -> Lambda triggers -> Resizes image -> Saves to new bucket.
*   **Week 11: PySpark Basics**
    *   *Focus:* Spark Architecture (Driver/Worker), Reading/Writing Parquet, Basic Transformations.
    *   *Mon-Fri:* Setup PySpark on Google Colab (easiest way to start).
    *   *Weekend:* Re-write our Month 1 Pandas logic using PySpark. Compare the syntax.
*   **Week 12: PySpark Intermediate**
    *   *Focus:* `partitionBy`, Handling Nulls in Spark, Spark SQL.
    *   *Mon-Fri:* Practice Spark SQL syntax.
    *   *Weekend:* Process a large public dataset (e.g., NYC Taxi Data) to find "Average trip distance per hour."

**🔴 Month 3 Checkpoint (Must Deliver):**
A Colab Notebook link showing analysis of a large dataset using PySpark, and an AWS S3 bucket structure that we created via code.

---

### **Month 4: The Final Polish (Interview Ready)**
**Goal:** Resume, Portfolio, and Mock Interviews.

*   **Week 13: System Design (Data Focused)**
    *   *Focus:* Batch vs Streaming, Idempotency, Backfilling data.
    *   *Mon-Fri:* Watch "System Design Primer" videos focused on data (Youtube: Gaurav Sen / ByteByteGo).
    *   *Weekend:* Design a pipeline on a whiteboard: "How would you design a dashboard for Swiggy delivery times?"
*   **Week 14: The Capstone Project**
    *   *Focus:* Combining AWS + Airflow + Spark/Python + SQL.
    *   *Task:* Build **"The Job Market Analyzer."**
        1.  Scrape data (Python).
        2.  Store raw in S3 (Boto3).
        3.  Airflow triggers Spark job.
        4.  Spark cleans data.
        5.  Load into Warehouse (Snowflake Trial or Postgres).
*   **Week 15: LeetCode & SQL Grind**
    *   *Focus:* Speed.
    *   *Mon-Fri:* 3 SQL Mediums + 2 Python Mediums (Arrays/HashMaps) per day.
    *   *Weekend:* Mock Interview (Ask a friend or use Pramp/Interviewing.io).
*   **Week 16: Resume & Application Strategy**
    *   *Focus:* ATS Keywords.
    *   *Task:* Rewrite resume. "Built a pipeline..." -> "Designed an automated ETL pipeline using Airflow and AWS S3 reducing manual data entry by 100%."

**🔴 Month 4 Checkpoint (Must Deliver):**
Our **Capstone Project** documented in a `README.md` with an architecture diagram. This is our ticket to the job.

---

### **Our Daily Operating Routine**

**Monday - Friday (3.5 Hours)**
*   **0 - 30 Mins:** **Warm-up.** 1 SQL Query (Medium) + 1 Python (Easy). *Do not skip.*
*   **30 Mins - 2.5 Hrs:** **Learning.** Watch tutorials, read docs related to the Weekly Topic.
*   **2.5 Hrs - 3.5 Hrs:** **Implementation.** Write code. If we watched a video on S3, write a script to list S3 buckets.

**Saturday & Sunday (6 Hours)**
*   **Hour 1-2:** **Review.** Fix bugs from the week.
*   **Hour 2-6:** **Deep Work (The Weekend Project).** This is where we actually learn. We will get stuck. We will debug errors. This is the job.

**Strategic Advice:**
Startups in India (Bangalore/Pune/NCR) hire based on **GitHub activity**. If our GitHub shows green squares consistently for 4 months with these specific projects (Airflow/AWS/Spark), we will get calls from Tier 2 companies even without a fancy college tag.

**Execute Week 1 starting Monday(5th January).** Good luck.