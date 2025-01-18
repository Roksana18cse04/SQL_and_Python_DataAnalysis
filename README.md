# e-Commerce (Target) Sales Dataset and Data Analysis with Python and SQL

## Overview
This project demonstrates the process of analyzing an e-commerce (Target) sales dataset using Python and SQL. The workflow includes:

1. **Data Loading:** Import CSV files containing e-commerce data into a MySQL database.
2. **Database Interaction:** Use SQL queries for data extraction and transformation.
3. **Data Analysis:** Perform in-depth analysis on sales trends, customer behavior, and product performance.
4. **Visualization:** Use Python libraries to visualize insights.

---

## Prerequisites

### Tools and Libraries:
- **Python 3.x**
- **MySQL Database**
- **Python Libraries:**
  - `pandas`
  - `mysql.connector`
  - `matplotlib`
  - `seaborn`

Ensure these tools are installed in your environment before proceeding.

---

## Dataset Structure

The dataset contains the following tables:

| Table Name   | Columns                                                                                      |
|--------------|----------------------------------------------------------------------------------------------|
| `customers`  | `customer_id`, `customer_unique_id`, `customer_zipcode`, `customer_city`, `customer_state`   |
| `products`   | `product_id`, `product_category`                                                            |
| `orders`     | `order_id`, `customer_id`, `order_purchase_timestamp`                                        |
| `order_items`| `order_id`, `order_item_id`, `product_id`, `seller_id`, `shipping_limit_date`, `price`, `freight_value` |
| `payments`   | `order_id`, `payment_sequential`, `payment_type`, `payment_installments`, `payment_value`    |
| `sellers`    | `seller_id`, `seller_zip_code_prefix`, `seller_city`, `seller_state`                        |
| `geolocation`| `geolocation_zip_code_prefix`, `geolocation_lat`, `geolocation_lng`, `geolocation_city`, `geolocation_state` |

---

## Key Steps

### 1. Loading CSV Files into MySQL

#### Process:
1. **Read CSV Files:**
   - Use `pandas` to load CSV files.
   - Handle missing data by replacing `NaN` with `None`.
2. **Create SQL Tables:**
   - Dynamically generate `CREATE TABLE` queries based on column data types.
3. **Insert Data into Tables:**
   - Use `INSERT` queries to populate MySQL tables.

#### Example Code:
```python
import pandas as pd
import mysql.connector
import os

# List of CSV files and their corresponding table names
csv_files = [
    ('customers.csv', 'customers'),
    ('orders.csv', 'orders'),
    ('products.csv', 'products'),
    ('payments.csv', 'payments'),  # Added payments.csv for specific handling
    ('sellers.csv', 'sellers'),
    ('order_items.csv', 'order_items'),
    ('geolocation.csv', 'geolocation') 
]

# Connect to the MySQL database
conn = mysql.connector.connect(
    host='127.0.0.1',
    user='root',
    password='Roksana@2000',
    database='XYZ_Company'
)
cursor = conn.cursor()

# Folder containing the CSV files
folder_path = 'F:/DATA Analysis/ECOMARCEdATASET'

# Function to map pandas dtypes to SQL data types
def get_sql_type(dtype):
    if pd.api.types.is_integer_dtype(dtype):
        return 'INT'
    elif pd.api.types.is_float_dtype(dtype):
        return 'FLOAT'
    elif pd.api.types.is_bool_dtype(dtype):
        return 'BOOLEAN'
    elif pd.api.types.is_datetime64_any_dtype(dtype):
        return 'DATETIME'
    else:
        return 'TEXT'

# Process each CSV file
for csv_file, table_name in csv_files:
    file_path = os.path.join(folder_path, csv_file)
    
    # Read the CSV file into a pandas DataFrame
    df = pd.read_csv(file_path)
    
    # Replace NaN with None to handle SQL NULL
    df = df.where(pd.notnull(df), None)
    
    # Debugging: Check for NaN values
    print(f"Processing {csv_file}")
    print(f"NaN values before replacement:\n{df.isnull().sum()}\n")

    # Clean column names
    df.columns = [col.replace(' ', '_').replace('-', '_').replace('.', '_') for col in df.columns]

    # Generate the CREATE TABLE statement with appropriate data types
    columns = ', '.join([f'`{col}` {get_sql_type(df[col].dtype)}' for col in df.columns])
    create_table_query = f'CREATE TABLE IF NOT EXISTS `{table_name}` ({columns})'
    cursor.execute(create_table_query)

    # Insert DataFrame data into the MySQL table
    for _, row in df.iterrows():
        # Convert row to tuple and handle NaN/None explicitly
        values = tuple(None if pd.isna(x) else x for x in row)
        sql = f"INSERT INTO `{table_name}` ({', '.join(['`' + col + '`' for col in df.columns])}) VALUES ({', '.join(['%s'] * len(row))})"
        cursor.execute(sql, values)

    # Commit the transaction for the current CSV file
    conn.commit()

# Close the connection
cursor.close()
conn.close()

print("Data successfully loaded into the database.")

```

---

### 2. Data Analysis with SQL

#### Sample Analysis 1: Orders Per Month
**Objective:** Calculate the number of orders per month in 2018.

**SQL Query:**
```sql
SELECT
    MONTHNAME(order_purchase_timestamp) AS month,
    COUNT(order_id) AS order_count
FROM orders
WHERE YEAR(order_purchase_timestamp) = 2018
GROUP BY month
ORDER BY MONTH(order_purchase_timestamp);
```

#### Sample Analysis 2: Average Products Per Order by City
**Objective:** Find the average number of products per order grouped by customer city.

**SQL Query:**
```sql
SELECT
    customers.customer_city,
    AVG(order_items.order_item_id) AS avg_products_per_order
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
JOIN order_items ON orders.order_id = order_items.order_id
GROUP BY customers.customer_city
ORDER BY avg_products_per_order DESC;
```

---

### 3. Visualization

#### Tools Used:
- **Matplotlib** and **Seaborn** for generating bar charts, pie charts, and histograms.

#### Example Code:
```python
import matplotlib.pyplot as plt

# Bar chart for monthly orders
plt.figure(figsize=(10, 6))
plt.bar(df['month'], df['order_count'], color='skyblue', width=0.6)
plt.title('Orders Per Month (2018)')
plt.xlabel('Month')
plt.ylabel('Number of Orders')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

---

## Summary
This project showcases the end-to-end workflow for analyzing e-commerce sales data using Python and SQL. It covers data ingestion, transformation, analysis, and visualization to provide actionable insights.
