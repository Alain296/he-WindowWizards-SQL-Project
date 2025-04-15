# The-WindowWizards-SQL-Project 
## Team Members
- Byiringiro Bailly 26593
- Alain Mugabo 26450

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
!(https://github.com/Alain296/he-WindowWizards-SQL-Project/blob/main/Screenshot%20Product%20table%20.png) 
