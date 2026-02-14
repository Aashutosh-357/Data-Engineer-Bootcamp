# 🐍 Python Database Connectivity: sqlite3 & psycopg2

## 🎯 **Learning Objectives**
Master Python database connectivity using sqlite3 for lightweight databases and psycopg2 for PostgreSQL enterprise solutions.

> 💡 **Why This Matters:** Database connectivity is fundamental to data engineering - from ETL pipelines to data analysis workflows.

---

# 📊 **Database Connectivity Overview**

## 🔄 **Python Database Workflow**
```
1. Import Module     →  2. Connect to DB    →  3. Create Cursor
        ↓                       ↓                      ↓
4. Execute Query     →  5. Fetch Results    →  6. Close Connection
```

### 📋 **Module Comparison**
| Aspect | sqlite3 | psycopg2 |
|--------|---------|----------|
| **Database** | SQLite (file-based) | PostgreSQL (server-based) |
| **Installation** | Built-in Python | `pip install psycopg2` |
| **Use Case** | Development, prototyping | Production, enterprise |
| **Concurrency** | Limited | High |
| **Features** | Basic SQL | Advanced SQL, JSON, arrays |

---

# 🗃️ **Part 1: SQLite3 Module**

## 📖 **What is SQLite?**
SQLite is a **file-based database** - the entire database is stored in a single file on disk. Perfect for:
- **Development & Testing**
- **Small to medium applications**
- **Data analysis scripts**
- **Embedded applications**

### ✅ **Advantages**
- No server setup required
- Zero configuration
- Built into Python
- Cross-platform compatibility
- ACID compliant

### ⚠️ **Limitations**
- Single writer at a time
- No user management
- Limited concurrent access
- No network access

---

## 🔧 **SQLite3 Implementation**

### 🚀 **Basic Connection & Setup**
```python
import sqlite3
from typing import List, Tuple, Optional
import os

class SQLiteManager:
    """
    Professional SQLite database manager
    """
    
    def __init__(self, db_path: str):
        self.db_path = db_path
        self.connection = None
        self.cursor = None
    
    def connect(self) -> None:
        """Establish database connection"""
        try:
            self.connection = sqlite3.connect(self.db_path)
            self.cursor = self.connection.cursor()
            print(f"✅ Connected to SQLite database: {self.db_path}")
        except sqlite3.Error as e:
            print(f"❌ Connection error: {e}")
    
    def disconnect(self) -> None:
        """Close database connection"""
        if self.cursor:
            self.cursor.close()
        if self.connection:
            self.connection.close()
        print("🔒 Database connection closed")
    
    def execute_query(self, query: str, params: Optional[Tuple] = None) -> List[Tuple]:
        """Execute SELECT query and return results"""
        try:
            if params:
                self.cursor.execute(query, params)
            else:
                self.cursor.execute(query)
            
            results = self.cursor.fetchall()
            print(f"📊 Query executed successfully. Rows returned: {len(results)}")
            return results
        
        except sqlite3.Error as e:
            print(f"❌ Query error: {e}")
            return []
    
    def execute_non_query(self, query: str, params: Optional[Tuple] = None) -> int:
        """Execute INSERT, UPDATE, DELETE queries"""
        try:
            if params:
                self.cursor.execute(query, params)
            else:
                self.cursor.execute(query)
            
            self.connection.commit()
            rows_affected = self.cursor.rowcount
            print(f"✅ Query executed. Rows affected: {rows_affected}")
            return rows_affected
        
        except sqlite3.Error as e:
            print(f"❌ Execution error: {e}")
            self.connection.rollback()
            return 0
```

### 📊 **Complete SQLite Example**
```python
def sqlite_demo():
    """Complete SQLite demonstration"""
    
    # Initialize database manager
    db = SQLiteManager("company.db")
    db.connect()
    
    try:
        # 1. Create tables
        create_tables_sql = """
        CREATE TABLE IF NOT EXISTS departments (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE,
            budget REAL DEFAULT 0
        );
        
        CREATE TABLE IF NOT EXISTS employees (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            department_id INTEGER,
            salary REAL,
            hire_date DATE,
            FOREIGN KEY (department_id) REFERENCES departments (id)
        );
        """
        
        db.cursor.executescript(create_tables_sql)
        print("🏗️ Tables created successfully")
        
        # 2. Insert sample data
        departments = [
            ("Engineering", 500000),
            ("Marketing", 200000),
            ("Sales", 300000)
        ]
        
        db.cursor.executemany(
            "INSERT OR IGNORE INTO departments (name, budget) VALUES (?, ?)",
            departments
        )
        
        employees = [
            ("Alice Johnson", 1, 75000, "2023-01-15"),
            ("Bob Smith", 1, 82000, "2023-02-01"),
            ("Carol Davis", 2, 65000, "2023-01-20"),
            ("David Wilson", 3, 70000, "2023-03-01")
        ]
        
        db.cursor.executemany(
            "INSERT OR IGNORE INTO employees (name, department_id, salary, hire_date) VALUES (?, ?, ?, ?)",
            employees
        )
        
        db.connection.commit()
        print("📥 Sample data inserted")
        
        # 3. Query examples
        print("\n📊 QUERY RESULTS")
        print("=" * 40)
        
        # Simple SELECT
        results = db.execute_query("SELECT * FROM departments")
        print("\n🏢 Departments:")
        for row in results:
            print(f"  ID: {row[0]}, Name: {row[1]}, Budget: ${row[2]:,}")
        
        # JOIN query
        join_query = """
        SELECT e.name, d.name as department, e.salary
        FROM employees e
        JOIN departments d ON e.department_id = d.id
        ORDER BY e.salary DESC
        """
        
        results = db.execute_query(join_query)
        print("\n👥 Employees with Departments:")
        for row in results:
            print(f"  {row[0]} - {row[1]} - ${row[2]:,}")
        
        # Parameterized query
        high_salary_query = "SELECT name, salary FROM employees WHERE salary > ?"
        results = db.execute_query(high_salary_query, (70000,))
        print("\n💰 High Salary Employees (>$70K):")
        for row in results:
            print(f"  {row[0]}: ${row[1]:,}")
        
        # Aggregation query
        agg_query = """
        SELECT d.name, COUNT(e.id) as employee_count, AVG(e.salary) as avg_salary
        FROM departments d
        LEFT JOIN employees e ON d.id = e.department_id
        GROUP BY d.id, d.name
        """
        
        results = db.execute_query(agg_query)
        print("\n📈 Department Statistics:")
        for row in results:
            avg_salary = row[2] if row[2] else 0
            print(f"  {row[0]}: {row[1]} employees, Avg Salary: ${avg_salary:,.2f}")
    
    finally:
        db.disconnect()

# Run the demo
if __name__ == "__main__":
    sqlite_demo()
```

---

# 🐘 **Part 2: psycopg2 Module**

## 📖 **What is psycopg2?**
psycopg2 is the **most popular PostgreSQL adapter** for Python. It provides:
- **Full PostgreSQL feature support**
- **High performance**
- **Thread safety**
- **Connection pooling**
- **Advanced data types**

### 🚀 **Installation**
```bash
# Standard installation
pip install psycopg2

# Binary version (easier installation)
pip install psycopg2-binary

# For production, use the standard version
pip install psycopg2
```

### ✅ **Advantages**
- Enterprise-grade database
- Excellent concurrency
- Advanced SQL features
- JSON support
- Full ACID compliance
- User management & security

---

## 🔧 **psycopg2 Implementation**

### 🚀 **Professional PostgreSQL Manager**
```python
import psycopg2
from psycopg2 import sql, Error
from psycopg2.extras import RealDictCursor
from typing import List, Dict, Optional, Any
from contextlib import contextmanager

class PostgreSQLManager:
    """
    Professional PostgreSQL database manager with connection pooling
    """
    
    def __init__(self, host: str, database: str, user: str, password: str, port: int = 5432):
        self.connection_params = {
            'host': host,
            'database': database,
            'user': user,
            'password': password,
            'port': port
        }
        self.connection = None
    
    def connect(self) -> bool:
        """Establish PostgreSQL connection"""
        try:
            self.connection = psycopg2.connect(**self.connection_params)
            self.connection.autocommit = False  # Explicit transaction control
            print(f"✅ Connected to PostgreSQL: {self.connection_params['database']}")
            return True
        except Error as e:
            print(f"❌ Connection error: {e}")
            return False
    
    def disconnect(self) -> None:
        """Close PostgreSQL connection"""
        if self.connection:
            self.connection.close()
            print("🔒 PostgreSQL connection closed")
    
    @contextmanager
    def get_cursor(self, dict_cursor: bool = False):
        """Context manager for cursor handling"""
        cursor = None
        try:
            if dict_cursor:
                cursor = self.connection.cursor(cursor_factory=RealDictCursor)
            else:
                cursor = self.connection.cursor()
            yield cursor
        except Error as e:
            print(f"❌ Cursor error: {e}")
            self.connection.rollback()
            raise
        finally:
            if cursor:
                cursor.close()
    
    def execute_query(self, query: str, params: Optional[tuple] = None, 
                     fetch_all: bool = True, dict_result: bool = False) -> List[Any]:
        """Execute SELECT query with optional parameters"""
        try:
            with self.get_cursor(dict_cursor=dict_result) as cursor:
                cursor.execute(query, params)
                
                if fetch_all:
                    results = cursor.fetchall()
                else:
                    results = cursor.fetchone()
                
                print(f"📊 Query executed. Rows returned: {len(results) if fetch_all else 1}")
                return results
        
        except Error as e:
            print(f"❌ Query error: {e}")
            return []
    
    def execute_non_query(self, query: str, params: Optional[tuple] = None) -> int:
        """Execute INSERT, UPDATE, DELETE queries"""
        try:
            with self.get_cursor() as cursor:
                cursor.execute(query, params)
                self.connection.commit()
                
                rows_affected = cursor.rowcount
                print(f"✅ Query executed. Rows affected: {rows_affected}")
                return rows_affected
        
        except Error as e:
            print(f"❌ Execution error: {e}")
            self.connection.rollback()
            return 0
    
    def execute_batch(self, query: str, data_list: List[tuple]) -> int:
        """Execute batch operations for better performance"""
        try:
            with self.get_cursor() as cursor:
                cursor.executemany(query, data_list)
                self.connection.commit()
                
                rows_affected = cursor.rowcount
                print(f"✅ Batch executed. Rows affected: {rows_affected}")
                return rows_affected
        
        except Error as e:
            print(f"❌ Batch error: {e}")
            self.connection.rollback()
            return 0
```

### 📊 **Complete PostgreSQL Example**
```python
def postgresql_demo():
    """Complete PostgreSQL demonstration"""
    
    # Initialize database manager
    db = PostgreSQLManager(
        host="localhost",
        database="engineering_lab",
        user="postgres",
        password="your_password"
    )
    
    if not db.connect():
        return
    
    try:
        # 1. Create tables with advanced features
        create_tables_sql = """
        CREATE TABLE IF NOT EXISTS departments (
            id SERIAL PRIMARY KEY,
            name VARCHAR(100) NOT NULL UNIQUE,
            budget DECIMAL(12, 2) DEFAULT 0,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
            metadata JSONB
        );
        
        CREATE TABLE IF NOT EXISTS employees (
            id SERIAL PRIMARY KEY,
            name VARCHAR(200) NOT NULL,
            email VARCHAR(255) UNIQUE,
            department_id INTEGER REFERENCES departments(id) ON DELETE SET NULL,
            salary DECIMAL(10, 2),
            hire_date DATE DEFAULT CURRENT_DATE,
            skills TEXT[],
            is_active BOOLEAN DEFAULT TRUE,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        );
        
        CREATE INDEX IF NOT EXISTS idx_employees_department ON employees(department_id);
        CREATE INDEX IF NOT EXISTS idx_employees_salary ON employees(salary);
        """
        
        db.execute_non_query(create_tables_sql)
        print("🏗️ Advanced tables created")
        
        # 2. Insert departments with JSON metadata
        departments_data = [
            ("Engineering", 500000, '{"location": "Building A", "manager": "John Doe"}'),
            ("Data Science", 400000, '{"location": "Building B", "manager": "Jane Smith"}'),
            ("Marketing", 200000, '{"location": "Building C", "manager": "Bob Johnson"}')
        ]
        
        insert_dept_query = """
        INSERT INTO departments (name, budget, metadata) 
        VALUES (%s, %s, %s) 
        ON CONFLICT (name) DO NOTHING
        """
        
        db.execute_batch(insert_dept_query, departments_data)
        
        # 3. Insert employees with arrays and advanced features
        employees_data = [
            ("Alice Johnson", "alice@company.com", 1, 85000, "2023-01-15", ["Python", "SQL", "Docker"]),
            ("Bob Smith", "bob@company.com", 1, 92000, "2023-02-01", ["Java", "Kubernetes", "AWS"]),
            ("Carol Davis", "carol@company.com", 2, 95000, "2023-01-20", ["Python", "Machine Learning", "TensorFlow"]),
            ("David Wilson", "david@company.com", 3, 70000, "2023-03-01", ["Marketing", "Analytics", "SEO"])
        ]
        
        insert_emp_query = """
        INSERT INTO employees (name, email, department_id, salary, hire_date, skills) 
        VALUES (%s, %s, %s, %s, %s, %s)
        ON CONFLICT (email) DO NOTHING
        """
        
        db.execute_batch(insert_emp_query, employees_data)
        
        # 4. Advanced query examples
        print("\n📊 ADVANCED QUERY RESULTS")
        print("=" * 50)
        
        # JSON query
        json_query = """
        SELECT name, budget, metadata->>'location' as location, metadata->>'manager' as manager
        FROM departments
        ORDER BY budget DESC
        """
        
        results = db.execute_query(json_query, dict_result=True)
        print("\n🏢 Departments with JSON data:")
        for row in results:
            print(f"  {row['name']}: ${row['budget']:,} - {row['location']} (Manager: {row['manager']})")
        
        # Array query
        array_query = """
        SELECT name, skills, array_length(skills, 1) as skill_count
        FROM employees
        WHERE 'Python' = ANY(skills)
        ORDER BY skill_count DESC
        """
        
        results = db.execute_query(array_query)
        print("\n🐍 Python Developers:")
        for row in results:
            skills_str = ", ".join(row[1])
            print(f"  {row[0]}: {skills_str} ({row[2]} skills)")
        
        # Window function query
        window_query = """
        SELECT 
            e.name,
            d.name as department,
            e.salary,
            AVG(e.salary) OVER (PARTITION BY d.name) as dept_avg_salary,
            RANK() OVER (PARTITION BY d.name ORDER BY e.salary DESC) as dept_rank
        FROM employees e
        JOIN departments d ON e.department_id = d.id
        ORDER BY d.name, e.salary DESC
        """
        
        results = db.execute_query(window_query)
        print("\n🏆 Employee Rankings by Department:")
        for row in results:
            print(f"  {row[1]}: {row[0]} - ${row[2]:,} (Avg: ${row[3]:,.2f}, Rank: {row[4]})")
        
        # Aggregation with HAVING
        agg_query = """
        SELECT 
            d.name,
            COUNT(e.id) as employee_count,
            AVG(e.salary) as avg_salary,
            SUM(e.salary) as total_payroll
        FROM departments d
        LEFT JOIN employees e ON d.id = e.department_id AND e.is_active = TRUE
        GROUP BY d.id, d.name
        HAVING COUNT(e.id) > 0
        ORDER BY avg_salary DESC
        """
        
        results = db.execute_query(agg_query)
        print("\n📈 Department Payroll Analysis:")
        for row in results:
            print(f"  {row[0]}: {row[1]} employees, Avg: ${row[2]:,.2f}, Total: ${row[3]:,}")
    
    finally:
        db.disconnect()

# Run the demo
if __name__ == "__main__":
    postgresql_demo()
```

---

# 🔄 **Database Connection Patterns**

## 🎯 **Connection Management Best Practices**

### 🔒 **Context Manager Pattern**
```python
from contextlib import contextmanager

@contextmanager
def database_connection(db_type: str, **kwargs):
    """Universal database connection context manager"""
    connection = None
    try:
        if db_type == "sqlite":
            connection = sqlite3.connect(kwargs['database'])
        elif db_type == "postgresql":
            connection = psycopg2.connect(**kwargs)
        
        yield connection
    
    except Exception as e:
        if connection:
            connection.rollback()
        raise e
    
    finally:
        if connection:
            connection.close()

# Usage examples
def context_manager_examples():
    """Demonstrate context manager usage"""
    
    # SQLite example
    with database_connection("sqlite", database="company.db") as conn:
        cursor = conn.cursor()
        cursor.execute("SELECT COUNT(*) FROM employees")
        count = cursor.fetchone()[0]
        print(f"SQLite employee count: {count}")
    
    # PostgreSQL example
    pg_params = {
        "host": "localhost",
        "database": "engineering_lab",
        "user": "postgres",
        "password": "your_password"
    }
    
    with database_connection("postgresql", **pg_params) as conn:
        cursor = conn.cursor()
        cursor.execute("SELECT COUNT(*) FROM employees")
        count = cursor.fetchone()[0]
        print(f"PostgreSQL employee count: {count}")
```

### 🏊 **Connection Pooling**
```python
from psycopg2 import pool
from threading import Lock

class ConnectionPool:
    """Thread-safe PostgreSQL connection pool"""
    
    def __init__(self, minconn: int = 1, maxconn: int = 10, **kwargs):
        self.pool = psycopg2.pool.ThreadedConnectionPool(
            minconn, maxconn, **kwargs
        )
        self.lock = Lock()
    
    @contextmanager
    def get_connection(self):
        """Get connection from pool"""
        connection = None
        try:
            with self.lock:
                connection = self.pool.getconn()
            yield connection
        finally:
            if connection:
                with self.lock:
                    self.pool.putconn(connection)
    
    def close_all(self):
        """Close all connections in pool"""
        self.pool.closeall()

# Usage
pool_manager = ConnectionPool(
    minconn=2, maxconn=10,
    host="localhost", database="engineering_lab",
    user="postgres", password="your_password"
)

with pool_manager.get_connection() as conn:
   # 🐍 Database Connectivity for Junior Data Engineers

## 🎯 **Career-Focused Learning Path**
Master Python database connectivity for **Junior Data Engineer** roles at **semi-product companies** - where you'll build data pipelines, ETL processes, and analytics infrastructure.

> 💡 **Industry Reality:** Semi-product companies need versatile data engineers who can handle both rapid prototyping (SQLite) and production systems (PostgreSQL).

---

# 🏢 **Semi-Product Company Context**

## 💼 **What You'll Actually Do**

### 🔄 **Daily Responsibilities**
| Task | SQLite Usage | PostgreSQL Usage |
|------|-------------|------------------|
| **Data Exploration** | Quick analysis scripts | Complex analytical queries |
| **ETL Prototyping** | Local pipeline testing | Production data processing |
| **A/B Testing** | Experiment data storage | User behavior analytics |
| **Reporting** | Ad-hoc reports | Automated dashboards |
| **Data Quality** | Validation scripts | Production monitoring |

### 🎯 **Career Progression Path**
```
Junior Data Engineer (6-12 months)
        ↓
Data Engineer (1-2 years)
        ↓
Senior Data Engineer (2-4 years)
        ↓
Lead Data Engineer / Data Architect
```

---

# 🚀 **Production-Ready Code Templates**

## 📊 **ETL Pipeline Foundation**

### 🔧 **Data Pipeline Manager**
```python
import sqlite3
import psycopg2
import pandas as pd
from typing import Dict, List, Any, Optional
from datetime import datetime
import logging
from contextlib import contextmanager

class DataPipelineManager:
    """
    Production-ready database manager for data engineering workflows
    Used in semi-product companies for ETL, analytics, and reporting
    """
    
    def __init__(self, config: Dict[str, Any]):
        self.config = config
        self.logger = self._setup_logging()
        
    def _setup_logging(self) -> logging.Logger:
        """Setup logging for production monitoring"""
        logger = logging.getLogger('data_pipeline')
        logger.setLevel(logging.INFO)
        
        if not logger.handlers:
            handler = logging.StreamHandler()
            formatter = logging.Formatter(
                '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
            )
            handler.setFormatter(formatter)
            logger.addHandler(handler)
        
        return logger
    
    @contextmanager
    def get_connection(self, db_type: str = 'postgresql'):
        """Get database connection with proper error handling"""
        connection = None
        try:
            if db_type == 'sqlite':
                connection = sqlite3.connect(self.config['sqlite_path'])
                connection.row_factory = sqlite3.Row  # Dict-like access
            
            elif db_type == 'postgresql':
                connection = psycopg2.connect(
                    host=self.config['pg_host'],
                    database=self.config['pg_database'],
                    user=self.config['pg_user'],
                    password=self.config['pg_password'],
                    port=self.config.get('pg_port', 5432)
                )
            
            self.logger.info(f"Connected to {db_type} database")
            yield connection
            
        except Exception as e:
            self.logger.error(f"Database connection error: {e}")
            if connection:
                connection.rollback()
            raise
        
        finally:
            if connection:
                connection.close()
                self.logger.info(f"Closed {db_type} connection")
    
    def extract_data(self, query: str, params: Optional[tuple] = None, 
                    db_type: str = 'postgresql') -> pd.DataFrame:
        """Extract data for ETL pipeline"""
        try:
            with self.get_connection(db_type) as conn:
                df = pd.read_sql_query(query, conn, params=params)
                self.logger.info(f"Extracted {len(df)} rows from {db_type}")
                return df
        
        except Exception as e:
            self.logger.error(f"Data extraction failed: {e}")
            return pd.DataFrame()
    
    def load_data(self, df: pd.DataFrame, table_name: str, 
                 db_type: str = 'postgresql', if_exists: str = 'append') -> bool:
        """Load data for ETL pipeline"""
        try:
            with self.get_connection(db_type) as conn:
                df.to_sql(table_name, conn, if_exists=if_exists, index=False)
                self.logger.info(f"Loaded {len(df)} rows to {table_name} in {db_type}")
                return True
        
        except Exception as e:
            self.logger.error(f"Data loading failed: {e}")
            return False
```

---

## 📊 **Real-World ETL Scenarios**

### 🎯 **Scenario 1: User Analytics Pipeline**
```python
class UserAnalyticsPipeline(DataPipelineManager):
    """
    Real pipeline for user behavior analysis in semi-product companies
    """
    
    def process_daily_user_metrics(self, date: str) -> bool:
        """Process daily user engagement metrics"""
        try:
            # 1. Extract raw user events from production DB
            raw_events_query = """
            SELECT 
                user_id,
                event_type,
                event_timestamp,
                page_url,
                session_id,
                device_type
            FROM user_events 
            WHERE DATE(event_timestamp) = %s
            AND event_type IN ('page_view', 'click', 'purchase')
            """
            
            raw_events = self.extract_data(
                raw_events_query, (date,), db_type='postgresql'
            )
            
            if raw_events.empty:
                self.logger.warning(f"No events found for {date}")
                return False
            
            # 2. Transform data - calculate session metrics
            session_metrics = self._calculate_session_metrics(raw_events)
            
            # 3. Load to analytics database
            success = self.load_data(
                session_metrics, 'daily_user_metrics', 
                db_type='postgresql', if_exists='replace'
            )
            
            # 4. Create summary for stakeholders
            if success:
                self._generate_daily_summary(session_metrics, date)
            
            return success
            
        except Exception as e:
            self.logger.error(f"User analytics pipeline failed: {e}")
            return False
    
    def _calculate_session_metrics(self, events_df: pd.DataFrame) -> pd.DataFrame:
        """Transform raw events into session-level metrics"""
        session_metrics = events_df.groupby(['user_id', 'session_id']).agg({
            'event_timestamp': ['min', 'max', 'count'],
            'page_url': 'nunique',
            'device_type': 'first'
        }).reset_index()
        
        # Flatten column names
        session_metrics.columns = [
            'user_id', 'session_id', 'session_start', 'session_end', 
            'total_events', 'unique_pages', 'device_type'
        ]
        
        # Calculate session duration
        session_metrics['session_duration_minutes'] = (
            pd.to_datetime(session_metrics['session_end']) - 
            pd.to_datetime(session_metrics['session_start'])
        ).dt.total_seconds() / 60
        
        return session_metrics
    
    def _generate_daily_summary(self, metrics_df: pd.DataFrame, date: str) -> None:
        """Generate executive summary for stakeholders"""
        summary = {
            'date': date,
            'total_users': metrics_df['user_id'].nunique(),
            'total_sessions': len(metrics_df),
            'avg_session_duration': metrics_df['session_duration_minutes'].mean(),
            'avg_pages_per_session': metrics_df['unique_pages'].mean()
        }
        
        # Store summary in SQLite for quick access
        summary_df = pd.DataFrame([summary])
        self.load_data(summary_df, 'daily_summaries', db_type='sqlite')
        
        self.logger.info(f"Daily summary generated: {summary}")
```

### 🎯 **Scenario 2: A/B Test Analysis**
```python
class ABTestAnalyzer(DataPipelineManager):
    """
    A/B test analysis pipeline for product decisions
    """
    
    def analyze_experiment(self, experiment_id: str) -> Dict[str, Any]:
        """Analyze A/B test results for product team"""
        try:
            # Extract experiment data
            experiment_query = """
            SELECT 
                u.user_id,
                u.variant,
                u.assigned_at,
                COALESCE(c.conversion_count, 0) as conversions,
                COALESCE(e.engagement_score, 0) as engagement
            FROM experiment_users u
            LEFT JOIN (
                SELECT user_id, COUNT(*) as conversion_count
                FROM conversions 
                WHERE experiment_id = %s
                GROUP BY user_id
            ) c ON u.user_id = c.user_id
            LEFT JOIN (
                SELECT user_id, AVG(engagement_score) as engagement_score
                FROM user_engagement 
                WHERE experiment_id = %s
                GROUP BY user_id
            ) e ON u.user_id = e.user_id
            WHERE u.experiment_id = %s
            """
            
            experiment_data = self.extract_data(
                experiment_query, (experiment_id, experiment_id, experiment_id)
            )
            
            # Statistical analysis
            results = self._perform_statistical_analysis(experiment_data)
            
            # Store results for product team
            self._store_experiment_results(experiment_id, results)
            
            return results
            
        except Exception as e:
            self.logger.error(f"A/B test analysis failed: {e}")
            return {}
    
    def _perform_statistical_analysis(self, data: pd.DataFrame) -> Dict[str, Any]:
        """Perform statistical analysis on A/B test data"""
        from scipy import stats
        
        control = data[data['variant'] == 'control']
        treatment = data[data['variant'] == 'treatment']
        
        # Conversion rate analysis
        control_conversion_rate = (control['conversions'] > 0).mean()
        treatment_conversion_rate = (treatment['conversions'] > 0).mean()
        
        # Statistical significance test
        control_conversions = (control['conversions'] > 0).sum()
        treatment_conversions = (treatment['conversions'] > 0).sum()
        
        chi2, p_value = stats.chi2_contingency([
            [control_conversions, len(control) - control_conversions],
            [treatment_conversions, len(treatment) - treatment_conversions]
        ])[:2]
        
        return {
            'control_users': len(control),
            'treatment_users': len(treatment),
            'control_conversion_rate': control_conversion_rate,
            'treatment_conversion_rate': treatment_conversion_rate,
            'lift': (treatment_conversion_rate - control_conversion_rate) / control_conversion_rate,
            'p_value': p_value,
            'is_significant': p_value < 0.05,
            'confidence_level': 0.95
        }
```

---

## 📊 **Data Quality & Monitoring**

### 🔍 **Production Data Quality Checks**
```python
class DataQualityMonitor(DataPipelineManager):
    """
    Data quality monitoring for production systems
    Essential for maintaining data integrity in semi-product companies
    """
    
    def run_daily_quality_checks(self) -> Dict[str, bool]:
        """Run comprehensive data quality checks"""
        checks = {
            'user_data_completeness': self._check_user_data_completeness(),
            'event_data_freshness': self._check_event_data_freshness(),
            'revenue_data_consistency': self._check_revenue_consistency(),
            'duplicate_detection': self._check_for_duplicates()
        }
        
        # Log results
        failed_checks = [check for check, passed in checks.items() if not passed]
        
        if failed_checks:
            self.logger.error(f"Data quality checks failed: {failed_checks}")
            self._send_alert(failed_checks)
        else:
            self.logger.info("All data quality checks passed")
        
        return checks
    
    def _check_user_data_completeness(self) -> bool:
        """Check if user data is complete"""
        query = """
        SELECT 
            COUNT(*) as total_users,
            COUNT(email) as users_with_email,
            COUNT(created_at) as users_with_signup_date
        FROM users 
        WHERE created_at >= CURRENT_DATE - INTERVAL '1 day'
        """
        
        result = self.extract_data(query)
        
        if result.empty:
            return False
        
        row = result.iloc[0]
        completeness_rate = min(
            row['users_with_email'] / row['total_users'],
            row['users_with_signup_date'] / row['total_users']
        )
        
        return completeness_rate >= 0.95  # 95% completeness threshold
    
    def _check_event_data_freshness(self) -> bool:
        """Check if event data is fresh (within last hour)"""
        query = """
        SELECT MAX(event_timestamp) as latest_event
        FROM user_events
        """
        
        result = self.extract_data(query)
        
        if result.empty:
            return False
        
        latest_event = pd.to_datetime(result.iloc[0]['latest_event'])
        now = pd.Timestamp.now(tz=latest_event.tz)
        
        # Data should be no older than 2 hours
        return (now - latest_event).total_seconds() < 7200
    
    def _send_alert(self, failed_checks: List[str]) -> None:
        """Send alert for failed data quality checks"""
        # In production, this would integrate with Slack, email, or PagerDuty
        alert_message = f"Data Quality Alert: {', '.join(failed_checks)} failed"
        self.logger.critical(alert_message)
        
        # Store alert in database for tracking
        alert_data = pd.DataFrame([{
            'timestamp': datetime.now(),
            'alert_type': 'data_quality',
            'failed_checks': ', '.join(failed_checks),
            'severity': 'high'
        }])
        
        self.load_data(alert_data, 'data_quality_alerts', db_type='sqlite')
```

---

# 💼 **Interview-Ready Knowledge**

## 🎯 **Common Interview Questions & Answers**

### ❓ **Q1: "How do you handle database connections in production?"**
```python
# ✅ GOOD ANSWER - Show connection pooling knowledge
from psycopg2 import pool

class ProductionDBManager:
    def __init__(self):
        self.connection_pool = psycopg2.pool.ThreadedConnectionPool(
            minconn=5,  # Minimum connections
            maxconn=20, # Maximum connections
            host=os.getenv('DB_HOST'),
            database=os.getenv('DB_NAME'),
            user=os.getenv('DB_USER'),
            password=os.getenv('DB_PASSWORD')
        )
    
    @contextmanager
    def get_connection(self):
        conn = self.connection_pool.getconn()
        try:
            yield conn
        finally:
            self.connection_pool.putconn(conn)

# Explain: "I use connection pooling to manage database connections efficiently.
# This prevents connection exhaustion and improves performance in production."
```

### ❓ **Q2: "How do you prevent SQL injection?"**
```python
# ✅ DEMONSTRATE with examples
def secure_user_lookup(user_id: int, email: str):
    # ❌ NEVER do this
    # query = f"SELECT * FROM users WHERE id = {user_id} AND email = '{email}'"
    
    # ✅ ALWAYS do this
    query = "SELECT * FROM users WHERE id = %s AND email = %s"
    
    with get_db_connection() as conn:
        cursor = conn.cursor()
        cursor.execute(query, (user_id, email))  # Parameterized query
        return cursor.fetchall()

# Explain: "I always use parameterized queries and validate inputs.
# This prevents SQL injection attacks and ensures data security."
```

### ❓ **Q3: "How do you optimize database performance?"**
```python
# ✅ Show multiple optimization techniques
class PerformanceOptimizer:
    
    def batch_insert_users(self, users: List[Dict]):
        """Use batch operations for better performance"""
        query = "INSERT INTO users (name, email, created_at) VALUES %s"
        
        with get_db_connection() as conn:
            cursor = conn.cursor()
            # Use execute_values for batch insert
            psycopg2.extras.execute_values(
                cursor, query, 
                [(u['name'], u['email'], u['created_at']) for u in users],
                template=None, page_size=1000
            )
    
    def create_performance_indexes(self):
        """Create indexes for common queries"""
        indexes = [
            "CREATE INDEX CONCURRENTLY idx_users_email ON users(email)",
            "CREATE INDEX CONCURRENTLY idx_events_user_timestamp ON events(user_id, timestamp)",
            "CREATE INDEX CONCURRENTLY idx_orders_status_date ON orders(status, created_date)"
        ]
        
        for index_sql in indexes:
            self.execute_query(index_sql)

# Explain: "I use batch operations, proper indexing, and query optimization.
# I also monitor query performance and use EXPLAIN ANALYZE for optimization."
```

---

## 🚀 **Career Development Roadmap**

### 📈 **6-Month Learning Plan**

#### **Months 1-2: Foundation**
- ✅ Master SQLite for prototyping
- ✅ Learn PostgreSQL production patterns
- ✅ Build first ETL pipeline
- ✅ Implement data quality checks

#### **Months 3-4: Intermediate**
- ✅ Connection pooling & optimization
- ✅ Advanced SQL (window functions, CTEs)
- ✅ Error handling & monitoring
- ✅ A/B test analysis pipelines

#### **Months 5-6: Advanced**
- ✅ Database migrations & schema changes
- ✅ Performance tuning & indexing
- ✅ Data pipeline orchestration
- ✅ Production deployment strategies

### 🏆 **Skills Validation Checklist**

#### **Junior Data Engineer Level**
- ☐ Can connect to both SQLite and PostgreSQL
- ☐ Writes secure, parameterized queries
- ☐ Implements basic ETL pipelines
- ☐ Handles errors gracefully
- ☐ Uses pandas for data transformation
- ☐ Implements basic data quality checks

#### **Mid-Level Data Engineer**
- ☐ Optimizes database performance
- ☐ Implements connection pooling
- ☐ Designs efficient data schemas
- ☐ Monitors data pipeline health
- ☐ Handles large-scale data processing
- ☐ Mentors junior developers

---

# 🏢 **Semi-Product Company Specifics**

## 💼 **What Makes Semi-Product Companies Different**

### 🎯 **Unique Challenges**
- **Rapid iteration** - Need both quick prototypes and stable production
- **Resource constraints** - Must be efficient with infrastructure costs
- **Diverse data sources** - APIs, databases, files, third-party services
- **Stakeholder variety** - Engineers, product managers, executives

### 🚀 **Success Strategies**
```python
class SemiProductDataStrategy:
    """
    Strategies specific to semi-product company environments
    """
    
    def prototype_to_production_pipeline(self):
        """Pattern for moving from prototype to production"""
        
        # 1. Start with SQLite for rapid prototyping
        prototype_config = {
            'sqlite_path': 'prototype_analysis.db'
        }
        
        # 2. Validate with small dataset
        self.validate_pipeline_logic(prototype_config)
        
        # 3. Scale to PostgreSQL for production
        production_config = {
            'pg_host': os.getenv('PROD_DB_HOST'),
            'pg_database': os.getenv('PROD_DB_NAME'),
            'pg_user': os.getenv('PROD_DB_USER'),
            'pg_password': os.getenv('PROD_DB_PASSWORD')
        }
        
        # 4. Deploy with monitoring
        self.deploy_with_monitoring(production_config)
    
    def cost_efficient_data_storage(self):
        """Optimize for cost in resource-constrained environment"""
        strategies = {
            'data_retention': 'Archive old data to cheaper storage',
            'query_optimization': 'Use indexes and efficient queries',
            'connection_pooling': 'Minimize database connections',
            'batch_processing': 'Process data in batches vs real-time'
        }
        return strategies
```

---

# 🎯 **Key Takeaways for Junior Data Engineers**

## ✅ **Technical Skills Mastered**
- **Database connectivity** patterns for both development and production
- **ETL pipeline** development with proper error handling
- **Data quality** monitoring and alerting systems
- **Performance optimization** for cost-effective operations
- **Security best practices** for production environments

## 💼 **Career-Ready Competencies**
- **Problem-solving** approach to data engineering challenges
- **Communication** skills for cross-functional collaboration
- **Production mindset** with monitoring and reliability focus
- **Cost awareness** for resource-constrained environments
- **Continuous learning** attitude for technology evolution

## 🚀 **Next Career Steps**
1. **Build portfolio** with real ETL projects
2. **Contribute to open source** data engineering tools
3. **Learn orchestration** tools (Airflow, Prefect)
4. **Master cloud platforms** (AWS, GCP, Azure)
5. **Develop specialization** (streaming, ML pipelines, analytics)

---

*Launch your data engineering career with confidence! 🚀*