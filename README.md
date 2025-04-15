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

-- Insert regions data
INSERT INTO regions (region_id, region_name) VALUES (1, 'North America');
INSERT INTO regions (region_id, region_name) VALUES (2, 'Europe');
INSERT INTO regions (region_id, region_name) VALUES (3, 'Asia');
INSERT INTO regions (region_id, region_name) VALUES (4, 'South America');
INSERT INTO regions (region_id, region_name) VALUES (5, 'Africa);
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

-- Insert transaction data (sample entries shown)
INSERT INTO transactions (transaction_id, product_id, region_id, transaction_date, quantity, total_amount) VALUES (1, 1, 1, TO_DATE('2024-01-15', 'YYYY-MM-DD'), 5, 4499.95);
INSERT INTO transactions (transaction_id, product_id, region_id, transaction_date, quantity, total_amount) VALUES (2, 2, 1, TO_DATE('2024-01-16', 'YYYY-MM-DD'), 3, 3899.97);
INSERT INTO transactions (transaction_id, product_id, region_id, transaction_date, quantity, total_amount) VALUES (3, 1, 2, TO_DATE('2024-01-17', 'YYYY-MM-DD'), 4, 3599.96);
-- Additional transaction records...
```
![Alt text](
