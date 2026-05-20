# PySpark Data Analytics

A comprehensive PySpark-based data analytics project that performs data engineering and analysis on household, customer, and vehicle datasets. This project demonstrates ETL (Extract, Transform, Load) operations and advanced analytics using Apache Spark.

## Project Overview

This project consists of two main Jupyter notebooks:
1. **Data Engineering.ipynb** - Data integration and preparation
2. **Data Analysis.ipynb** - Exploratory data analysis and business insights

## Datasets

The project works with three main CSV files:
- **CARS.csv** - Vehicle information (500,000 records)
- **CUSTOMERS.csv** - Customer demographic data (499,999 records)
- **HOUSEHOLDS.csv** - Household and policy information (500,000 records)

## Data Engineering (Data Engineering.ipynb)

### Key Operations:

1. **Data Loading**
   - Load three CSV files (CARS, CUSTOMERS, HOUSEHOLDS) into Spark DataFrames
   - Set up Spark session with legacy time parser configuration

2. **Data Quality Checks**
   - Identify and remove columns with all null values
   - Count records in each dataset:

3. **Column Renaming**
   - Standardize column names for consistency

4. **Data Integration**
   - Join HOUSEHOLDS and CARS data on CAR_ID and State
   - Join resulting data with CUSTOMERS on CUST_ID
   - Convert date of birth strings to date format
   - Handle duplicate customer records by selecting max DOB

5. **Key Findings**
   - One customer exists in HOUSEHOLDS but not in CUSTOMERS 

## Data Analysis (Data Analysis.ipynb)

### Analysis Questions & Insights:

#### 1. **Distinct Counts**
   - Total households: 131,353
   - Active households: 105,331
   - Total customers: 499,851
   - Active customers: 401,093
   - Total vehicles: 372,536
   - Active vehicles: 315,055

#### 2. **Average Cars per Household**
   - Overall: 3.81 cars per household
   - Active households: 3.81 cars per household

#### 3. **Vehicle Age Distribution**
   - Analyzed count of cars by age (15-100 years)
   - Car age is calculated from customer DOB
   - Age 24 has the highest count of vehicles (9,788 vehicles)

#### 4. **Vehicle Make Distribution**
   - Breakdown across 7 manufacturers:
 

## Technologies Used

- **Apache Spark 3.5.1** - Distributed data processing
- **PySpark** - Python API for Spark
- **Python 3.10+** - Programming language
- **Jupyter Notebook** - Interactive computing environment
- **Py4j 0.10.9.7** - Java/Python integration

## Installation

```bash
# Install PySpark
pip install pyspark==3.5.1

# Install Jupyter (if not already installed)
pip install jupyter
```

## Usage

### Running the Notebooks

1. **Data Engineering First** - Run `Data Engineering.ipynb` to:
   - Load and clean raw data
   - Perform data integration
   - Generate combined dataset

2. **Data Analysis Second** - Run `Data Analysis.ipynb` to:
   - Analyze integrated data
   - Generate business insights
   - Create visualizations of key metrics

### Key Code Examples

**Loading Data:**
```python
spark = SparkSession.builder.appName("Read CSV").getOrCreate()
df = spark.read.csv("CARS.csv", header=True, inferSchema=True)
```

**Aggregation:**
```python
from pyspark.sql.functions import countDistinct
df.select(countDistinct("CAR_ID")).show()
```

## Performance Optimization

- Data is partitioned by state for efficient joins
- Distinct operations used for accurate unique counts
- Aggregations filtered before grouping to reduce data size
- Column renaming done early to prevent naming conflicts

## Project Structure

```
PySpark_Data_Analytics/
├── Data Engineering.ipynb    # ETL and data integration
├── Data Analysis.ipynb       # Exploratory analysis
├── README.md                 # This file
└── [Source CSV files]
    ├── CARS.csv
    ├── CUSTOMERS.csv
    └── HOUSEHOLDS.csv
```

## License

Open source - Feel free to use and modify for your own projects.

## Contact & Support

For questions or issues, please create an issue in the repository or contact the project maintainer.
