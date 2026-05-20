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

## Data Schema

### Combined Dataset Schema
The final integrated dataset contains the following columns:

**Household Information:**
- `HH_ID` - Household ID
- `Active HH` - Active household flag (1 or 0)
- `HH Start Date` - Household start date
- `Phone Number` - Contact phone number
- `ZIP` - Zip code
- `State` - State code
- `Country` - Country
- `Referral Source` - Source of referral

**Customer Information:**
- `CUST_ID` - Customer ID
- `Date of Birth` - Customer DOB (string format)
- `Gender` - Customer gender
- `Marital Status` - Marital status
- `Employment Type` - Type of employment
- `Income` - Income level
- `DOB` - Date of birth (date format)

**Vehicle Information:**
- `CAR_ID` - Vehicle ID
- `Status` - Policy status (In Force, Cancelled, etc.)
- `Year` - Vehicle year
- `Make` - Vehicle manufacturer
- `Body Style` - Vehicle type (4 door, 2 door, SUV, Truck)
- `Vehicle Value` - Assessed vehicle value
- `Annual Miles Driven` - Annual mileage
- `Business Use` - Business use flag
- `Antique Vehicle` - Antique vehicle flag
- `Lien` - Lien on vehicle flag
- `Lease` - Lease flag
- `Driver Safety Discount` - Driver safety discount flag
- `Vehicle Safety Discount` - Vehicle safety discount flag
- `Claim Payout` - Insurance claim payout amount
- `6 Month Premium Amount` - 6-month premium cost

## Data Engineering (Data Engineering.ipynb)

### Key Operations:

1. **Data Loading**
   - Load three CSV files (CARS, CUSTOMERS, HOUSEHOLDS) into Spark DataFrames
   - Set up Spark session with legacy time parser configuration

2. **Data Quality Checks**
   - Identify and remove columns with all null values
   - Count records in each dataset:
     - CARS: 500,000 rows
     - CUSTOMERS: 499,999 rows
     - HOUSEHOLDS: 500,000 rows

3. **Column Renaming**
   - Standardize column names for consistency
   - Rename `Car ID` → `CAR_CAR_ID`
   - Rename `State` → `CAR_State` (in cars file)

4. **Data Integration**
   - Join HOUSEHOLDS and CARS data on CAR_ID and State
   - Join resulting data with CUSTOMERS on CUST_ID
   - Convert date of birth strings to date format
   - Handle duplicate customer records by selecting max DOB

5. **Key Findings**
   - One customer exists in HOUSEHOLDS but not in CUSTOMERS (ID: 774461007)
   - Final combined dataset: 500,000 rows
   - Distinct households: 131,353
   - Distinct customers: 499,851
   - Distinct vehicles: 372,536

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
     - Manufacturer1: 74,373 vehicles
     - Manufacturer2: 74,536 vehicles
     - Manufacturer3: 74,558 vehicles
     - Manufacturer4: 25,932 vehicles
     - Manufacturer5: 26,003 vehicles
     - Manufacturer6: 48,580 vehicles
     - Manufacturer7: 48,554 vehicles

#### 5. **Vehicle Safety Classification**

**Safest Vehicles:**
- Criteria: Body Style = Truck, Vehicle Safety Discount = 1, Antique Vehicle = 0, Claim Payout = 0
- Count: 13,598 safe vehicles
- Reasoning: Trucks have better crash protection and zero claims indicate no accidents

**Most Dangerous Vehicles:**
- Criteria: Body Style = 2 door, Vehicle Safety Discount = 0, Antique Vehicle = 1, Claim Payout > 0
- Count: 21 dangerous vehicles
- Reasoning: 2-door vehicles are smaller and less safe; antique vehicles lack modern safety features; positive claims indicate accident history

Reference: [IIHS Vehicle Size and Weight Study](https://www.iihs.org/topics/vehicle-size-and-weight)

#### 6. **Geographic Distribution**
   - State with largest households: **Kentucky (KY)** with 2,227 households
   - Other top states: South Carolina (2,207), Alaska (2,197), Minnesota (2,187)

#### 7. **Customer Age Analysis**
   - Average customer age: **50 years**
   - Age range: 15-100 years
   - Consistent across active and inactive records

#### 8. **Age Variation by Region**
   - **Northeast**: Age range 15-100 years
   - **Midwest**: Age range 15-100 years
   - **South**: Age range 15-100 years
   - **West**: Age range 15-100 years
   - All regions have consistent age distribution

#### 9. **Claims by Age Group**
   - Highest claims: Age 21-30 ($118.1M)
   - Second highest: Age 31-40 ($90.1M)
   - Lowest claims: Age 81-90 ($53.1M)
   - Young drivers (21-30) represent the highest insurance risk

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

**Date Calculations:**
```python
from pyspark.sql.functions import months_between
df_age = df.withColumn('Age', (months_between(current_date(), col('DOB')) / 12).cast('int'))
```

**Complex Joins:**
```python
join_cond = [df1.CAR_ID == df2.CAR_ID, df1.State == df2.State]
result = df1.join(df2, on=join_cond)
```

## Key Metrics Summary

| Metric | Count |
|--------|-------|
| Total Records | 500,000 |
| Distinct Households | 131,353 |
| Distinct Customers | 499,851 |
| Distinct Vehicles | 372,536 |
| Average Household Size | 3.81 vehicles |
| Average Customer Age | 50 years |
| Top State by Households | Kentucky |
| Highest Claims Age Group | 21-30 years |

## Data Quality Notes

- One customer ID (774461007) exists in HOUSEHOLDS but not in CUSTOMERS (external hire)
- 10 households have Active HH flag = 1 for 2 different states
- Customer date of birth requires validation for accurate age calculations
- Some vehicles have multiple makes due to data entry variations

## Performance Optimization

- Data is partitioned by state for efficient joins
- Distinct operations used for accurate unique counts
- Aggregations filtered before grouping to reduce data size
- Column renaming done early to prevent naming conflicts

## Future Enhancements

- Add predictive modeling for insurance claims
- Implement customer segmentation analysis
- Create automated reporting dashboards
- Add data visualization with Matplotlib/Seaborn
- Implement data quality monitoring pipeline
- Add support for streaming data updates

## Author

Created by: neetastyagi

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
