
# 🐼 Pandas Data Manipulation for Junior Data Engineers

## 🎯 **Career-Focused Learning Path**
Master essential pandas operations for **Junior Data Engineer** roles at **mid-product companies** - where you'll clean, transform, and analyze customer data for product insights.

> 💡 **Industry Reality:** Mid-product companies have established data infrastructure but need engineers who can quickly extract insights from messy, real-world datasets.

---

# 🏢 **Mid-Product Company Context**

## 💼 **What You'll Actually Do**

### 🔄 **Daily Data Engineering Tasks**
| Task | Column Selection | Row Filtering | Missing Data Handling |
|------|------------------|---------------|----------------------|
| **Customer Analysis** | `df['user_id']` | `df[df['active'] == True]` | Fill missing emails |
| **Product Metrics** | `df[['revenue', 'units']]` | `df[df['purchase_date'] > '2024-01-01']` | Drop incomplete orders |
| **A/B Testing** | `df['conversion_rate']` | `df[df['experiment_group'] == 'treatment']` | Impute missing values |
| **Cohort Analysis** | `df['signup_month']` | `df[df['tenure_days'] >= 30]` | Forward fill time series |
| **Churn Prediction** | `df['last_activity']` | `df[df['days_inactive'] > 90]` | Replace nulls with defaults |

---

# 📊 **Part 1: Column Selection Mastery**

## 🎯 **Core Concepts**

### 🔧 **Single Column Selection**
```python
import pandas as pd
import numpy as np

# Create sample dataset
data = {
    'customer_id': [1, 2, 3, 4, 5],
    'name': ['Alice', 'Bob', 'Carol', 'David', 'Eve'],
    'age': [25, 30, 35, 40, 45],
    'revenue': [1200, 1500, 800, 2200, 1800]
}
df = pd.DataFrame(data)

# Single column selection - returns Series
customer_ids = df['customer_id']
print(type(customer_ids))  # <class 'pandas.core.series.Series'>

# Alternative syntax (attribute access)
ages = df.age  # Only works if column name is valid Python identifier
```

### 🔧 **Multiple Column Selection**
```python
# Multiple columns - returns DataFrame
profile_data = df[['customer_id', 'name', 'age']]
print(type(profile_data))  # <class 'pandas.core.frame.DataFrame'>

# Select all numeric columns
numeric_cols = df.select_dtypes(include=[np.number])
print(numeric_cols.columns.tolist())  # ['customer_id', 'age', 'revenue']

# Select columns by pattern
revenue_cols = [col for col in df.columns if 'revenue' in col.lower()]
revenue_data = df[revenue_cols]
```

### 🏭 **Production Column Selection**
```python
class ColumnSelector:
    """Professional column selection for production systems"""
    
    def __init__(self, df: pd.DataFrame):
        self.df = df
    
    def safe_select(self, columns):
        """Safely select columns with error handling"""
        if isinstance(columns, str):
            columns = [columns]
        
        missing_cols = [col for col in columns if col not in self.df.columns]
        if missing_cols:
            print(f"Warning: Missing columns {missing_cols}")
            columns = [col for col in columns if col in self.df.columns]
        
        return self.df[columns] if columns else pd.DataFrame()
    
    def select_business_metrics(self):
        """Select key business metrics"""
        metric_patterns = ['revenue', 'sales', 'conversion', 'engagement']
        metric_cols = []
        
        for col in self.df.columns:
            if any(pattern in col.lower() for pattern in metric_patterns):
                metric_cols.append(col)
        
        return self.df[metric_cols]

# Usage
selector = ColumnSelector(df)
safe_data = selector.safe_select(['customer_id', 'name', 'nonexistent_col'])
```

---

# 🔍 **Part 2: Row Filtering Excellence**

## 🎯 **Core Filtering Techniques**

### 🔧 **Basic Boolean Filtering**
```python
# Single condition
high_revenue = df[df['revenue'] > 1500]
print(f"High revenue customers: {len(high_revenue)}")

# Multiple conditions with & (AND)
high_value_young = df[(df['revenue'] > 1500) & (df['age'] < 40)]

# Multiple conditions with | (OR)
target_customers = df[(df['age'] > 35) | (df['revenue'] > 2000)]

# Using isin() for multiple values
selected_customers = df[df['customer_id'].isin([1, 3, 5])]
```

### 🔧 **Advanced Filtering with query()**
```python
# More readable complex conditions
filtered = df.query('age > 30 and revenue > 1000')

# String operations
name_filter = df.query('name.str.startswith("A")', engine='python')

# Using variables in query
min_age = 30
max_revenue = 2000
result = df.query('age > @min_age and revenue < @max_revenue')
```

### 🏭 **Production Filtering Patterns**
```python
class DataFilter:
    """Professional filtering for business analytics"""
    
    def __init__(self, df: pd.DataFrame):
        self.df = df
        self.filter_log = []
    
    def apply_filter(self, condition, description=""):
        """Apply filter with logging"""
        initial_count = len(self.df)
        
        if isinstance(condition, str):
            filtered_df = self.df.query(condition)
        else:
            filtered_df = self.df[condition]
        
        final_count = len(filtered_df)
        reduction_pct = ((initial_count - final_count) / initial_count) * 100
        
        self.filter_log.append({
            'description': description,
            'initial_count': initial_count,
            'final_count': final_count,
            'reduction_pct': round(reduction_pct, 2)
        })
        
        print(f"Filter: {description}")
        print(f"Rows: {initial_count} → {final_count} ({reduction_pct:.1f}% reduction)")
        
        return filtered_df
    
    def filter_high_value_customers(self, revenue_threshold=1500):
        """Business-specific filter for high-value customers"""
        condition = f'revenue > {revenue_threshold}'
        return self.apply_filter(condition, f"High-value customers (>${revenue_threshold})")
    
    def filter_customer_segment(self, age_min=None, age_max=None, min_revenue=None):
        """Flexible customer segmentation"""
        conditions = []
        
        if age_min is not None:
            conditions.append(f'age >= {age_min}')
        if age_max is not None:
            conditions.append(f'age <= {age_max}')
        if min_revenue is not None:
            conditions.append(f'revenue >= {min_revenue}')
        
        if conditions:
            condition = ' and '.join(conditions)
            return self.apply_filter(condition, "Customer segment filter")
        
        return self.df

# Usage
filter_engine = DataFilter(df)
high_value = filter_engine.filter_high_value_customers(1200)
target_segment = filter_engine.filter_customer_segment(age_min=25, age_max=40, min_revenue=1000)
```

---

# 🤔 **Part 3: Missing Data Handling**

## 🎯 **Core Missing Data Operations**

### 🔧 **Detection Methods**
```python
# Create dataset with missing values
data_with_missing = {
    'customer_id': [1, 2, 3, 4, 5],
    'name': ['Alice', 'Bob', None, 'David', 'Eve'],
    'age': [25, None, 35, 40, None],
    'email': ['alice@email.com', None, 'carol@email.com', None, 'eve@email.com'],
    'revenue': [1200, 1500, None, 2200, 1800]
}
df_missing = pd.DataFrame(data_with_missing)

# Check for missing values
print("Missing values per column:")
print(df_missing.isna().sum())

# Check for any missing values
has_missing = df_missing.isna().any().any()
print(f"Dataset has missing values: {has_missing}")

# Get rows with any missing values
rows_with_missing = df_missing[df_missing.isna().any(axis=1)]
print(f"Rows with missing data: {len(rows_with_missing)}")
```

### 🔧 **Removal Strategies**
```python
# Drop rows with any missing values
df_complete_cases = df_missing.dropna()
print(f"Complete cases: {len(df_complete_cases)} (from {len(df_missing)})")

# Drop rows with missing values in specific columns
df_email_complete = df_missing.dropna(subset=['email'])

# Drop columns with too many missing values
threshold = 0.5  # Drop if more than 50% missing
df_clean_cols = df_missing.dropna(axis=1, thresh=int(threshold * len(df_missing)))

# Drop rows that have missing values in more than 2 columns
df_clean_rows = df_missing.dropna(thresh=len(df_missing.columns) - 2)
```

### 🔧 **Imputation Strategies**
```python
# Fill with specific values
df_filled = df_missing.copy()

# Fill numeric columns with mean
df_filled['age'] = df_filled['age'].fillna(df_filled['age'].mean())
df_filled['revenue'] = df_filled['revenue'].fillna(df_filled['revenue'].median())

# Fill categorical columns with mode or custom value
df_filled['name'] = df_filled['name'].fillna('Unknown')
df_filled['email'] = df_filled['email'].fillna('no-email@company.com')

# Forward fill and backward fill
df_ffill = df_missing.fillna(method='ffill')  # Forward fill
df_bfill = df_missing.fillna(method='bfill')  # Backward fill

# Fill with different values per column
fill_values = {
    'age': df_missing['age'].mean(),
    'name': 'Unknown Customer',
    'email': 'missing@email.com',
    'revenue': 0
}
df_multi_fill = df_missing.fillna(fill_values)
```

### 🏭 **Production Missing Data Handler**
```python
class MissingDataHandler:
    """Professional missing data handling for production systems"""
    
    def __init__(self, df: pd.DataFrame):
        self.df = df.copy()
        self.missing_report = self._generate_report()
    
    def _generate_report(self):
        """Generate comprehensive missing data report"""
        report = []
        for col in self.df.columns:
            missing_count = self.df[col].isna().sum()
            missing_pct = (missing_count / len(self.df)) * 100
            
            report.append({
                'column': col,
                'missing_count': missing_count,
                'missing_percentage': round(missing_pct, 2),
                'data_type': str(self.df[col].dtype)
            })
        
        return pd.DataFrame(report).sort_values('missing_percentage', ascending=False)
    
    def handle_numeric_missing(self, column, strategy='mean'):
        """Handle missing values in numeric columns"""
        if not pd.api.types.is_numeric_dtype(self.df[column]):
            raise ValueError(f"Column {column} is not numeric")
        
        if strategy == 'mean':
            fill_value = self.df[column].mean()
        elif strategy == 'median':
            fill_value = self.df[column].median()
        elif strategy == 'mode':
            fill_value = self.df[column].mode().iloc[0]
        else:
            fill_value = strategy  # Use custom value
        
        self.df[column] = self.df[column].fillna(fill_value)
        print(f"Filled {column} missing values with {strategy}: {fill_value}")
    
    def handle_categorical_missing(self, column, strategy='mode'):
        """Handle missing values in categorical columns"""
        if strategy == 'mode':
            mode_val = self.df[column].mode()
            fill_value = mode_val.iloc[0] if len(mode_val) > 0 else 'Unknown'
        else:
            fill_value = strategy  # Use custom value
        
        self.df[column] = self.df[column].fillna(fill_value)
        print(f"Filled {column} missing values with: {fill_value}")
    
    def smart_imputation(self):
        """Apply smart imputation based on data types"""
        for col in self.df.columns:
            missing_count = self.df[col].isna().sum()
            if missing_count > 0:
                if pd.api.types.is_numeric_dtype(self.df[col]):
                    self.handle_numeric_missing(col, 'median')
                else:
                    self.handle_categorical_missing(col, 'Unknown')
    
    def get_clean_dataset(self, max_missing_pct=30):
        """Get cleaned dataset with configurable missing data threshold"""
        # Drop columns with too much missing data
        cols_to_keep = self.missing_report[
            self.missing_report['missing_percentage'] <= max_missing_pct
        ]['column'].tolist()
        
        clean_df = self.df[cols_to_keep].copy()
        
        # Apply smart imputation to remaining columns
        handler = MissingDataHandler(clean_df)
        handler.smart_imputation()
        
        return handler.df

# Usage
handler = MissingDataHandler(df_missing)
print("Missing Data Report:")
print(handler.missing_report)

clean_data = handler.get_clean_dataset(max_missing_pct=50)
print(f"\nCleaned dataset shape: {clean_data.shape}")
```

---

# 🚢 **Drill: Titanic Dataset Analysis**

## 🎯 **Complete Implementation**

```python
import pandas as pd
import numpy as np

class TitanicAnalyzer:
    """Complete Titanic analysis for Junior Data Engineer drill"""
    
    def __init__(self):
        self.df = None
        self.df_processed = None
    
    def load_data(self):
        """Load Titanic dataset"""
        try:
            # Try loading from seaborn
            import seaborn as sns
            self.df = sns.load_dataset('titanic')
            print("✅ Loaded Titanic dataset from seaborn")
        except ImportError:
            # Create sample data if seaborn not available
            print("⚠️ Creating sample Titanic dataset")
            self.df = self._create_sample_data()
        
        print(f"Dataset shape: {self.df.shape}")
        print(f"Columns: {list(self.df.columns)}")
        return self.df
    
    def _create_sample_data(self):
        """Create sample Titanic-like dataset"""
        np.random.seed(42)
        n = 891
        
        data = {
            'survived': np.random.choice([0, 1], n, p=[0.62, 0.38]),
            'pclass': np.random.choice([1, 2, 3], n, p=[0.24, 0.21, 0.55]),
            'sex': np.random.choice(['male', 'female'], n, p=[0.65, 0.35]),
            'age': np.random.normal(29.7, 14.5, n),
            'fare': np.random.exponential(32, n),
            'embarked': np.random.choice(['S', 'C', 'Q'], n, p=[0.72, 0.19, 0.09])
        }
        
        df = pd.DataFrame(data)
        
        # Add realistic missing values
        age_missing = np.random.random(n) < 0.2
        df.loc[age_missing, 'age'] = None
        
        return df
    
    def analyze_missing_data(self):
        """Analyze missing data patterns"""
        print("\n🔍 MISSING DATA ANALYSIS")
        print("=" * 30)
        
        missing_summary = self.df.isnull().sum()
        missing_pct = (missing_summary / len(self.df)) * 100
        
        missing_report = pd.DataFrame({
            'Missing Count': missing_summary,
            'Missing %': missing_pct.round(2)
        })
        
        print(missing_report[missing_report['Missing Count'] > 0])
        return missing_report
    
    def handle_missing_age(self):
        """Handle missing age values with average"""
        print("\n🤔 HANDLING MISSING AGE DATA")
        print("=" * 35)
        
        # Calculate statistics before filling
        missing_count = self.df['age'].isnull().sum()
        avg_age = self.df['age'].mean()
        
        print(f"Missing age values: {missing_count}")
        print(f"Average age: {avg_age:.1f} years")
        
        # Create processed copy
        self.df_processed = self.df.copy()
        
        # Fill missing ages with average
        self.df_processed['age'] = self.df_processed['age'].fillna(avg_age)
        
        # Verify
        remaining_missing = self.df_processed['age'].isnull().sum()
        print(f"Remaining missing ages: {remaining_missing}")
        
        return self.df_processed
    
    def filter_female_over_30(self):
        """Filter for female passengers over 30"""
        print("\n👩 FILTERING: FEMALE PASSENGERS OVER 30")
        print("=" * 45)
        
        if self.df_processed is None:
            raise ValueError("Must handle missing data first")
        
        # Step-by-step filtering
        print("Applying filters:")
        
        # Filter by gender
        female_mask = self.df_processed['sex'] == 'female'
        female_passengers = self.df_processed[female_mask]
        print(f"1. Female passengers: {len(female_passengers)}")
        
        # Filter by age
        age_mask = female_passengers['age'] > 30
        female_over_30 = female_passengers[age_mask]
        print(f"2. Female over 30: {len(female_over_30)}")
        
        # Calculate percentage
        total_passengers = len(self.df_processed)
        percentage = (len(female_over_30) / total_passengers) * 100
        print(f"3. Percentage of total: {percentage:.1f}%")
        
        return female_over_30
    
    def generate_insights(self, filtered_data):
        """Generate business insights from filtered data"""
        print("\n📊 BUSINESS INSIGHTS")
        print("=" * 25)
        
        # Survival rate
        survival_rate = filtered_data['survived'].mean() * 100
        print(f"1. Survival rate: {survival_rate:.1f}%")
        
        # Class distribution
        class_dist = filtered_data['pclass'].value_counts().sort_index()
        print("2. Class distribution:")
        for pclass, count in class_dist.items():
            pct = (count / len(filtered_data)) * 100
            print(f"   Class {pclass}: {count} ({pct:.1f}%)")
        
        # Age statistics
        print(f"3. Age statistics:")
        print(f"   Mean: {filtered_data['age'].mean():.1f} years")
        print(f"   Range: {filtered_data['age'].min():.1f} - {filtered_data['age'].max():.1f} years")
        
        # Fare analysis (if available)
        if 'fare' in filtered_data.columns:
            avg_fare = filtered_data['fare'].mean()
            print(f"4. Average fare: ${avg_fare:.2f}")
        
        return {
            'survival_rate': survival_rate,
            'class_distribution': class_dist.to_dict(),
            'age_stats': {
                'mean': filtered_data['age'].mean(),
                'min': filtered_data['age'].min(),
                'max': filtered_data['age'].max()
            }
        }
    
    def run_complete_analysis(self):
        """Run the complete drill analysis"""
        print("🚢 TITANIC DATASET ANALYSIS")
        print("=" * 35)
        
        # Step 1: Load data
        self.load_data()
        
        # Step 2: Analyze missing data
        missing_report = self.analyze_missing_data()
        
        # Step 3: Handle missing age data
        self.handle_missing_age()
        
        # Step 4: Filter female passengers over 30
        filtered_result = self.filter_female_over_30()
        
        # Step 5: Generate insights
        insights = self.generate_insights(filtered_result)
        
        # Step 6: Display sample results
        print("\n📋 SAMPLE RESULTS (First 10 rows):")
        print("=" * 40)
        
        display_cols = ['sex', 'age', 'pclass', 'survived']
        if 'fare' in filtered_result.columns:
            display_cols.append('fare')
        
        sample_data = filtered_result[display_cols].head(10)
        print(sample_data.to_string(index=False))
        
        print(f"\n✅ Analysis complete! Filtered dataset: {len(filtered_result)} records")
        
        return filtered_result, insights

# Execute the drill
def run_titanic_drill():
    """Execute the complete Titanic analysis drill"""
    analyzer = TitanicAnalyzer()
    
    try:
        filtered_data, insights = analyzer.run_complete_analysis()
        return filtered_data, insights
    except Exception as e:
        print(f"❌ Analysis failed: {e}")
        return None, None

# Run the drill
if __name__ == "__main__":
    result_data, analysis_insights = run_titanic_drill()
```

---

# 💼 **Interview-Ready Knowledge**

## 🎯 **Common Questions & Professional Answers**

### ❓ **Q1: "How do you handle missing data in production?"**
```python
# ✅ PROFESSIONAL APPROACH
def production_missing_data_strategy():
    steps = [
        "1. Assess missing data patterns (MCAR, MAR, MNAR)",
        "2. Understand business context and impact",
        "3. Choose appropriate strategy (drop, impute, flag)",
        "4. Validate imputation quality",
        "5. Document decisions for audit trail"
    ]
    return steps

# Explain: "I follow a systematic approach considering business impact,
# data patterns, and downstream effects before choosing a strategy."
```

### ❓ **Q2: "How do you optimize pandas for large datasets?"**
```python
# ✅ OPTIMIZATION TECHNIQUES
def pandas_optimization_tips():
    return {
        'vectorization': 'Use df["col1"] + df["col2"] instead of apply()',
        'data_types': 'Use category for strings, int32 vs int64',
        'chunking': 'Process large files in chunks',
        'query_method': 'Use df.query() for complex filtering',
        'memory_usage': 'Monitor with df.info(memory_usage="deep")'
    }

# Explain: "I optimize by using vectorized operations, appropriate data types,
# and processing data in chunks when memory is limited."
```

---

# 🎯 **Key Takeaways**

## ✅ **Technical Skills Mastered**
- **Column Selection**: Single/multiple columns, pattern matching, type-based selection
- **Row Filtering**: Boolean indexing, query() method, complex conditions
- **Missing Data**: Detection (.isna()), removal (.dropna()), imputation (.fillna())
- **Production Patterns**: Error handling, logging, business-focused implementations

## 🏢 **Mid-Product Company Readiness**
- **Business Context**: Understanding why data operations matter
- **Production Mindset**: Error handling, logging, documentation
- **Performance Awareness**: Optimizing for larger datasets
- **Quality Focus**: Systematic approach to data cleaning

## 🚀 **Career Advancement**
1. **Master these fundamentals** with real datasets
2. **Practice on messy data** to build problem-solving skills
3. **Learn advanced operations** (groupby, merge, pivot)
4. **Understand ETL pipelines** and data workflows
5. **Build domain expertise** in your industry

---

*Transform raw data into business insights with confidence! 🐼*
