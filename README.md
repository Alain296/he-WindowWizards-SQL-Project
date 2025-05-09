# The-WindowWizards-SQL-Project 
## Team Members
- BYIRINGIRO BAILLY 26593
- ALAIN MUGABO       26450

## Project Overview
This project explores SQL Window Functions using an e-commerce dataset. We've designed a dataset that tracks product transactions across different regions to demonstrate various analytical capabilities of window functions.

## Dataset Structure
Our dataset consists of three tables:

### Products Table
This table contains product information including ID, name, category, and price.

```sql
-- Create Products table
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2)
);
``` 
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Product%20table%20.png)
```sql


-- Insert sample products
INSERT INTO products (product_id, product_name, category, price) VALUES (1, 'Smartphone X', 'Electronics', 899.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (2, 'Laptop Pro', 'Electronics', 1299.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (3, 'Coffee Maker', 'Home Appliances', 129.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (4, 'Wireless Headphones', 'Electronics', 199.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (5, 'Blender', 'Home Appliances', 79.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (6, 'Smart Watch', 'Electronics', 349.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (7, 'Running Shoes', 'Sports', 89.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (8, 'Yoga Mat', 'Sports', 29.99);
INSERT INTO products (product_id, product_name, category, price) VALUES (9, 'Protein Powder', 'Health', 39.99);

INSERT INTO products (product_id, product_name, category, price) VALUES (10, 'Gaming Console', 'Electronics', 499.99);
```
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Insert%20sample%20data%20-%20Products%20.png)
### Regions Table
This table lists geographical regions where transactions occur.

```sql
-- Create Regions table
CREATE TABLE regions (
    region_id INT PRIMARY KEY,
    region_name VARCHAR(50)
);
``` 
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20REGION%20table.png)
```sql
-- Insert regions data
INSERT INTO regions (region_id, region_name) VALUES (1, 'North America');
INSERT INTO regions (region_id, region_name) VALUES (2, 'Europe');
INSERT INTO regions (region_id, region_name) VALUES (3, 'Asia');
INSERT INTO regions (region_id, region_name) VALUES (4, 'South America');
INSERT INTO regions (region_id, region_name) VALUES (5, 'Africa);
``` 
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Inserting%20sample%20data%2CRegion.png)
``
### Transactions Table
This table records sales transactions including products sold, region, date, quantity, and amount.

```sql
-- Create Transactions table
CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY,
    product_id INT,
    region_id INT,
    transaction_date DATE,
    quantity INT,
    total_amount DECIMAL(12, 2),
    FOREIGN KEY (product_id) REFERENCES products(product_id),
    FOREIGN KEY (region_id) REFERENCES regions(region_id)
);
``` 
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Transaction%20table.png)
```sql
-- Insert transaction data (sample entries shown)
INSERT INTO transactions (transaction_id, product_id, region_id, transaction_date, quantity, total_amount) VALUES (1, 1, 1, TO_DATE('2024-01-15', 'YYYY-MM-DD'), 5, 4499.95);
INSERT INTO transactions (transaction_id, product_id, region_id, transaction_date, quantity, total_amount) VALUES (2, 2, 1, TO_DATE('2024-01-16', 'YYYY-MM-DD'), 3, 3899.97);
INSERT INTO transactions (transaction_id, product_id, region_id, transaction_date, quantity, total_amount) VALUES (3, 1, 2, TO_DATE('2024-01-17', 'YYYY-MM-DD'), 4, 3599.96);
-- Additional transaction records...
```
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Inserting%20sample%20data%2CTransaction.png)

``
## Window Functions Explored

### 1. Compare Values with Previous or Next Records (LAG/LEAD)

**Query Purpose**: This query compares each transaction amount with the previous transaction chronologically, providing instant visibility into sales trends over time.

**SQL Query:**
```sql
SELECT 
    t.transaction_id,
    p.product_name,
    r.region_name,
    t.transaction_date,
    t.total_amount,
    LAG(t.total_amount) OVER (ORDER BY t.transaction_date) AS previous_amount,
    CASE 
        WHEN t.total_amount > LAG(t.total_amount) OVER (ORDER BY t.transaction_date) THEN 'HIGHER'
        WHEN t.total_amount < LAG(t.total_amount) OVER (ORDER BY t.transaction_date) THEN 'LOWER'
        WHEN t.total_amount = LAG(t.total_amount) OVER (ORDER BY t.transaction_date) THEN 'EQUAL'
        ELSE 'FIRST RECORD'
    END AS comparison_with_previous,
    LEAD(t.total_amount) OVER (ORDER BY t.transaction_date) AS next_amount
FROM 
    transactions t
JOIN 
    products p ON t.product_id = p.product_id
JOIN 
    regions r ON t.region_id = r.region_id
ORDER BY 
    t.transaction_date;
```
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20LAG()%20to%20compare%20current%20transaction%20amount%20with%20previous%20transaction%20.png)
### 2. Ranking Data within a Category (RANK/DENSE_RANK)

**Query Purpose**: Ranks products by total sales within each product category, showing both RANK() and DENSE_RANK() for comparison.

**SQL Query:**
```sql
SELECT 
    p.category,
    p.product_name,
    SUM(t.total_amount) AS total_sales,
    RANK() OVER (PARTITION BY p.category ORDER BY SUM(t.total_amount) DESC) AS sales_rank,
    DENSE_RANK() OVER (PARTITION BY p.category ORDER BY SUM(t.total_amount) DESC) AS sales_dense_rank
FROM 
    transactions t
JOIN 
    products p ON t.product_id = p.product_id
GROUP BY 
    p.category, p.product_name
ORDER BY 
    p.category, total_sales DESC;
```
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Ranking%20Data%20within%20a%20Category%20.png)

### 3. Identifying Top Records

**Query Purpose**: Identifies the top 3 best-selling products in each region.

**SQL Query:**
```sql
WITH ranked_sales AS (
    SELECT 
        r.region_name,
        p.product_name,
        SUM(t.total_amount) AS total_sales,
        DENSE_RANK() OVER (PARTITION BY r.region_name ORDER BY SUM(t.total_amount) DESC) AS sales_rank
    FROM 
        transactions t
    JOIN 
        products p ON t.product_id = p.product_id
    JOIN 
        regions r ON t.region_id = r.region_id
    GROUP BY 
        r.region_name, p.product_name
)
SELECT 
    region_name,
    product_name,
    total_sales
FROM 
    ranked_sales
WHERE 
    sales_rank <= 3
ORDER BY 
    region_name, sales_rank;
```
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20%20Finding%20top%203%20best-selling%20products%20per%20region.png)
### 4. Finding the Earliest Records

**Query Purpose**: Retrieves the first 2 transactions for each region based on transaction date.

**SQL Query:**
```sql
WITH earliest_transactions AS (
    SELECT 
        r.region_name,
        p.product_name,
        t.transaction_date,
        t.total_amount,
        ROW_NUMBER() OVER (PARTITION BY r.region_name ORDER BY t.transaction_date) AS date_rank
    FROM 
        transactions t
    JOIN 
        products p ON t.product_id = p.product_id
    JOIN 
        regions r ON t.region_id = r.region_id
)
SELECT 
    region_name,
    product_name,
    transaction_date,
    total_amount
FROM 
    earliest_transactions
WHERE 
    date_rank <= 2
ORDER BY 
    region_name, transaction_date;
```
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Finding%20the%20Earliest%20Records.png)
### 5. Aggregation with Window Functions

**Query Purpose**: Calculates maximum transaction amounts at both region level and overall company level.

**SQL Query:**
```sql
SELECT 
    t.transaction_id,
    r.region_name,
    p.product_name,
    t.total_amount,
    MAX(t.total_amount) OVER (PARTITION BY r.region_name) AS max_transaction_in_region,
    MAX(t.total_amount) OVER () AS max_transaction_overall,
    t.total_amount / MAX(t.total_amount) OVER (PARTITION BY r.region_name) * 100 AS percentage_of_region_max,
    t.total_amount / MAX(t.total_amount) OVER () * 100 AS percentage_of_overall_max
FROM 
    transactions t
JOIN 
    products p ON t.product_id = p.product_id
JOIN 
    regions r ON t.region_id = r.region_id
ORDER BY 
    r.region_name, t.total_amount DESC;
```
![Alt text](https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Aggregation%20with%20Window%20Functions.png)
## Real-Life Applications of Window Functions

1. **Financial Analysis**:
   - Calculating moving averages for stock prices
   - Comparing quarter-over-quarter performance
   - Analyzing cumulative financial metrics

2. **Human Resources**:
   - Salary comparisons within departments
   - Employee performance ranking
   - Tenure analysis and milestone identification

3. **E-commerce**:
   - Customer purchase pattern analysis
   - Product performance by category
   - Regional sales trend identification

4. **Manufacturing**:
   - Production efficiency comparisons
   - Quality control metrics across production lines
   - Defect rate analysis over time

5. **Healthcare**:
   - Patient readmission pattern analysis
   - Treatment outcome comparisons
   - Resource utilization optimization


## Setup Instructions

1. Create a database in your preferred SQL environment
2. Run the table creation scripts:
   ```sql
   CREATE TABLE products (...)
   CREATE TABLE regions (...)
   CREATE TABLE transactions (...)
   ```

3. Run the data insertion scripts:
   ```sql
   INSERT INTO products (product_id, product_name, category, price) VALUES (...);
   INSERT INTO regions (region_id, region_name) VALUES (...);
   INSERT INTO transactions (transaction_id, product_id, region_id, transaction_date, quantity, total_amount) VALUES (...);
   ```

4. Execute each of the window function queries to see the results

## Conclusion

SQL Window Functions provide powerful analytical capabilities without the complexity of self-joins or complex subqueries. They allow for sophisticated data analysis while maintaining the granularity of the original data. Through this project, we've demonstrated how these functions can be applied to derive meaningful business insights from transactional data.

