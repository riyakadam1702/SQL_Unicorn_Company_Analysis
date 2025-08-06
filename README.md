#  Unicorn Companies Analysis Using SQL

## 📖 Project Description

This project focuses on analyzing data related to unicorn companies — privately held startups valued at over $1 billion. Using SQL, we explore various aspects of these companies based on multiple dimensions such as geography, industry, valuation, funding, and time. The analysis aims to derive patterns, trends, and relationships from the dataset by leveraging the power of relational databases and structured queries. It also demonstrates the ability to join datasets, use aggregate functions, apply window functions, and filter meaningful insights from real-world business data.


---

## 📁 Dataset Overview

### `dates`

| Column         | Description                                   |
|----------------|-----------------------------------------------|
| `company_id`   | A unique ID for the company                   |
| `date_joined`  | The date that the company became a unicorn    |
| `year_founded` | The year the company was founded              |

---

### `funding`

| Column            | Description                                      |
|-------------------|--------------------------------------------------|
| `company_id`      | A unique ID for the company                      |
| `valuation`       | Company valuation in US dollars                  |
| `funding`         | Total funding raised in US dollars               |
| `select_investors`| Comma-separated list of key investors            |

---

### `industries`

| Column       | Description                              |
|--------------|------------------------------------------|
| `company_id` | A unique ID for the company              |
| `industry`   | The industry the company operates in     |

---

### `companies`

| Column       | Description                                      |
|--------------|--------------------------------------------------|
| `company_id` | A unique ID for the company                      |
| `company`    | The name of the company                          |
| `city`       | City where the company is headquartered          |
| `country`    | Country where the company is headquartered       |
| `continent`  | Continent where the company is headquartered     |

---



