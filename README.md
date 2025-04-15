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
