# 🍕 Pizza Sales SQL Analysis

SQL-based analysis of pizza sales data — covering revenue, order trends, peak hours, category distribution, and top-performing pizzas.

---

## 📁 Project Structure

```
pizza-sales-sql-analysis/
│
├── Datasets/
│   ├── orders.csv
│   ├── order_details.csv
│   ├── pizzas.csv
│   └── pizza_types.csv
│
├── total_orders.sql
├── total_revenue.sql
├── highest_priced_pizza.sql
├── most_ordered_pizza_size.sql
├── orders_by_hour.sql
├── top_5_pizzas_by_quantity.sql
├── quantity_by_category.sql
├── pizza_count_by_category.sql
├── avg_daily_pizzas_ordered.sql
├── revenue_share_by_category.sql
├── top_3_pizzas_by_revenue.sql
├── cumulative_revenue_over_time.sql
├── top_3_pizzas_by_revenue_per_category.sql
└── README.md
```

---

## 🗄️ Dataset Overview

| File | Description | Rows |
|---|---|---|
| `orders.csv` | Order ID, date, and time of each order | ~21,350 |
| `order_details.csv` | Order ID, pizza ID, and quantity per line item | ~48,620 |
| `pizzas.csv` | Pizza ID, type, size, and price | 96 |
| `pizza_types.csv` | Pizza type ID, name, category, and ingredients | 32 |

### Table Relationships

```
orders          order_details       pizzas          pizza_types
----------      -------------       ----------      -----------
order_id  ───► order_id             pizza_id  ────► pizza_type_id
date            order_details_id    pizza_type_id   name
time            pizza_id      ─────►size            category
                quantity            price           ingredients
```

---

## 🛠️ How to Set Up in MySQL Workbench

### Step 1 — Create a new database

Open MySQL Workbench, connect to your local server, and run:

```sql
CREATE DATABASE pizza_sales;
USE pizza_sales;
```

### Step 2 — Create the tables

Run the following in the Query Editor:

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    date DATE NOT NULL,
    time TIME NOT NULL
);

CREATE TABLE order_details (
    order_details_id INT PRIMARY KEY,
    order_id INT NOT NULL,
    pizza_id VARCHAR(50) NOT NULL,
    quantity INT NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(order_id)
);

CREATE TABLE pizza_types (
    pizza_type_id VARCHAR(50) PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    category VARCHAR(50) NOT NULL,
    ingredients TEXT NOT NULL
);

CREATE TABLE pizzas (
    pizza_id VARCHAR(50) PRIMARY KEY,
    pizza_type_id VARCHAR(50) NOT NULL,
    size CHAR(2) NOT NULL,
    price DECIMAL(5,2) NOT NULL,
    FOREIGN KEY (pizza_type_id) REFERENCES pizza_types(pizza_type_id)
);
```

### Step 3 — Import the CSV files

> ⚠️ Import `pizza_types` and `pizzas` **before** `orders` and `order_details` to avoid foreign key errors. Follow this order:
> 1. `pizza_types`
> 2. `pizzas`
> 3. `orders`
> 4. `order_details`

For each table:

1. In MySQL Workbench, expand your `pizza_sales` database in the left panel
2. Right-click the table name → **Table Data Import Wizard**
3. Click **Browse** and select the matching `.csv` file from the `Datasets/` folder
4. Set **Field Separator** to `,` and make sure **Use existing table** is selected
5. Click **Next** through the column mapping screen (columns should auto-match)
6. Click **Next** → **Finish** to import

Repeat for all 4 tables in the order listed above.

### Step 4 — Verify the import

```sql
SELECT COUNT(*) FROM orders;           -- should return ~21350
SELECT COUNT(*) FROM order_details;    -- should return ~48620
SELECT COUNT(*) FROM pizzas;           -- should return 96
SELECT COUNT(*) FROM pizza_types;      -- should return 32
```

---

## 📊 SQL Queries

| File | Question Answered |
|---|---|
| `total_orders.sql` | How many orders were placed in total? |
| `total_revenue.sql` | What is the total revenue from pizza sales? |
| `highest_priced_pizza.sql` | Which pizza has the highest price? |
| `most_ordered_pizza_size.sql` | What is the most commonly ordered pizza size? |
| `orders_by_hour.sql` | At which hours of the day are orders most frequent? |
| `top_5_pizzas_by_quantity.sql` | Which 5 pizza types were ordered the most? |
| `quantity_by_category.sql` | What is the total quantity ordered per pizza category? |
| `pizza_count_by_category.sql` | How many pizza types exist in each category? |
| `avg_daily_pizzas_ordered.sql` | What is the average number of pizzas ordered per day? |
| `revenue_share_by_category.sql` | What percentage of revenue does each category contribute? |
| `top_3_pizzas_by_revenue.sql` | Which 3 pizza types generate the most revenue overall? |
| `cumulative_revenue_over_time.sql` | How does cumulative revenue grow over time? |
| `top_3_pizzas_by_revenue_per_category.sql` | Which 3 pizzas earn the most revenue within each category? |

---

## ▶️ How to Run a Query

1. Open MySQL Workbench and connect to your server
2. Click **File → Open SQL Script** and select any `.sql` file from this repo
3. Make sure `pizza_sales` is the active database:
   ```sql
   USE pizza_sales;
   ```
4. Press **Ctrl + Shift + Enter** (or click the lightning bolt ⚡ icon) to run the query

---

## 🔧 Requirements

- [MySQL](https://dev.mysql.com/downloads/mysql/) 8.0 or higher
- [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) 8.0 or higher

---

## 👤 Author

Made as a portfolio project to practice SQL analysis on real-world data.
