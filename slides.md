---
# You can also start simply with 'default'
theme: default

# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: CST 363 Final Exam Review
info: |
  ## CST 363
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
mdc: true
# open graph
# seoMeta:
#  ogImage: https://cover.sli.dev
---

# CST 363 Review


---


<br><br>

Exam is closed-book, but you can bring 3 physical pages 8.5 x 11 (both sides) or a 6 page Google Doc 

(You will submit this as a lab)


<br>

Section 1 Exam will be Wednesday May 14 at 4pm.


...

---

<br><br>


A relational database contains:

(Choose one)


- at most one table
- one or more tables

<br>

<v-click>

Answer is "one or more tables"

</v-click>


---

<br><br>


Explain the purpose of a **foreign key constraint** in SQL.

<br>


<v-click>

A foreign key constraint ensures that a column or set of columns in one table refers to a primary key in another table, maintaining **referential integrity** between the tables.

</v-click>


---

## SQL PATTERNS

<br>

```sql
customers(customer_id, name)
orders(order_id, customer_id, total_amount)
```

Write a SQL query to list all customers and their orders, **even if the customer has no orders.**

*(Hint: Use a `LEFT JOIN`)*


<br>

<v-click>
Sample answer:

```sql
SELECT c.customer_id, c.name, o.order_id, o.total_amount
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

</v-click>

---

## WHERE vs. HAVING?

<br>

`WHERE`

  - Filters rows before any grouping or aggregation happens.
  - Can only be used with individual row values.
  - Works in the `SELECT ... FROM ... WHERE ...` clause.


<br>

<v-click>


`HAVING`
- Filters groups after GROUP BY and aggregation.
- Used to filter based on aggregate functions like COUNT(), AVG(), SUM().
- Must be used with GROUP BY.


</v-click>

---

## Examples

<br>

```sql
SELECT * FROM Employees
WHERE salary > 50000;
```

<br><br>

<v-click>

```sql
SELECT dept, AVG(salary) AS avg_salary
FROM Employees
GROUP BY dept
HAVING AVG(salary) > 60000;
```

</v-click>

---

<br><br>

Explain the concept of a **correlated subquery**.

<v-click>

- A correlated subquery is a subquery that references a column from the outer query. 
- It is evaluated once for each row processed by the outer query.


</v-click>


---



## Data Design Review

What does this tell us? What kind of relationship is this?

![](/image2.png){width=250px lazy}


<v-click>

Many-to-one (N:1) relationship with optional participation on both sides.

</v-click>

---

## Normalization

A good reason to normalize a schema is:

(Choose one)

- improved query performance
- reduce redundancy
- reduce number of tables

<br>

<v-click>

answer is: "reduce redundancy"

</v-click>


---

<br><br>

Does this violate 1NF?

```sql
orders(order_id, customer_name, item_list)
```

<br>

<v-click>

Yes, `item_list` would likely contain a comma-separated list like "pen, notebook, eraser".


</v-click>

---

<br><br>

Explain the difference between Second Normal Form (2NF) and Third Normal Form (3NF).

<br>


<v-click>

2NF removes **partial dependencies**: every non-key attribute must depend on the whole primary key (relevant in composite keys).

</v-click>

<v-click>

3NF removes **transitive dependencies**: non-key attributes must only depend on the key, not on other non-key attributes.

</v-click> 


<!-- 
<v-click>
</v-click> 
-->
---

## Transitive dependency example:

<br>

```sql
student(student_id, name, advisor_name, advisor_office)
```
<br>


<v-click>

If `advisor_office` depends on `advisor_name`, and `advisor_name` depends on `student_id`, that’s a transitive dependency.


</v-click> 



---

## INDEXES

<br>

What is the main purpose of an **index** in a database?

<br>


<v-click>

An index is a data structure that improves the speed of data retrieval by providing a quick lookup mechanism to locate rows without scanning the entire table.

</v-click> 

---

Given this query, what type of index would help most?

```sql
SELECT * FROM Books WHERE price BETWEEN 10 AND 20;
```

<br>

<v-click>

A B-tree index would help most for this query.

The query performs a range scan on the price column. B-tree indexes are ideal for:

- Equality conditions (price = 15)
- Range conditions (`BETWEEN`, <, >, <=, >=)
- `ORDER BY` (if the sort matches the index order)

</v-click>

<br>

<v-click>

Hash indexes, in contrast:
- Are optimized only for equality lookups
- Cannot efficiently support range queries because hashing destroys order

</v-click>


---

Yes / No?

Is a database index useful for the query 

```sql
SELECT * FROM instructor;
```

<v-click>

Answer: no  (because for this query we need to scan the entire table)

</v-click>

---

<br><br>

Briefly explain one potential anomaly that can occur in concurrent transactions without proper isolation.


<v-click>

**Answer:**

A potential anomaly is a "dirty read," where a transaction reads data that has been written by another transaction but has not yet been committed. If the second transaction is later rolled back, the first transaction has read invalid data. Another is a "lost update," where two transactions try to update the same data, and one transaction's changes are overwritten by the other.


</v-click>


---

<br><br>

What is sharding in the context of distributed databases like MongoDB?


<v-click>

- Sharding is a horizontal partitioning technique that distributes data across multiple database instances or machines. 
- It improves scalability and load distribution by having each shard hold a portion of the overall dataset.

</v-click>