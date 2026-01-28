# What is Database Normalization?

Normalization is the process of structuring relational stables to reduce redundancy and prevent data anomalies. It involves decomposing tables into smaller, well-structured tables and defining relationships between them.

---

## Why Normalize?

Without normalization, the following data anomalies can occur:

- **Update Anomaly:** If data is duplicated across multiple rows, updating one instance may lead to inconsistencies if other instances are not updated.  
- **Insert Anomaly:** Inability to add data due to missing related data.  
- **Delete Anomaly:** Deleting a record may inadvertently remove other valuable data.

Consider the following unnormalized table `Orders`:

| order\_id | customer\_name | customer\_email | product\_name | product\_price | quantity |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Alice | [alice@a.com](mailto:alice@a.com) | Orange | 3.00 | 1 |
| 2 | Bob | [bob@a.com](mailto:bob@a.com) | Banana | 2.00 | 2 |
| 3 | Alice | [alice@a.com](mailto:alice@a.com) | Apple | 4.00 | 1 |
| 4 | Alice | [alice@a.com](mailto:alice@a.com) | Grape | 5.00 | 3 |

---

## Update Anomaly Example

An update anomaly occurs when we need to update the same data in multiple places otherwise the database becomes inconsistent.

If Alice changes her email, we must update it in multiple rows:

| order\_id | customer\_name | customer\_email | product\_name | product\_price | quantity |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Alice | [alice\_new@a.com](mailto:alice_new@a.com) 🆕 | Orange | 3.00 | 1 |
| 2 | Bob | [bob@a.com](mailto:bob@a.com) | Banana | 2.00 | 2 |
| 3 | Alice | [alice\_new@a.com](mailto:alice_new@a.com) 🆕 | Apple | 4.00 | 1 |
| 4 | Alice | [alice@a.com](mailto:alice@a.com) 🚨 | Grape | 5.00 | 3 |

This can lead to inconsistencies if we forget to update one of the rows.

---

## Insertion Anomaly Example

An insertion anomaly occurs when we cannot add data due to missing related data.

We cannot add a new customer without placing an order:

| order\_id | customer\_name | customer\_email | product\_name | product\_price | quantity |
| :---- | :---- | :---- | :---- | :---- | :---- |
| no data 🚨 | Charlie | [charlie@a.com](mailto:charlie@a.com) | no data 🚨 | no data 🚨 | no data 🚨 |

Also we cannot add a new product without an order:

| order\_id | customer\_name | customer\_email | product\_name | product\_price | quantity |
| :---- | :---- | :---- | :---- | :---- | :---- |
| no data 🚨 | no data 🚨 | no data 🚨 | Mango | no data 🚨 | no data 🚨 |

Even if we could set those fields to `NULL`, it leads to meaningless records. The logical correctness would be compromised e.g. a customer exists independently of orders in an `Order` table. Furthermore, primary key constraints may prevent `NULL` values.

Hence an insertion anomaly still exists even if we allow `NULL` values, because the schema does not accurately represent the real-world entities and their relationships.

---

## Deletion Anomaly Example

A deletion anomaly occurs when deleting a record inadvertently removes other valuable data.

If Bob cancels his order and we delete his record:

| order\_id | customer\_name | customer\_email | product\_name | product\_price | quantity |
| :---- | :---- | :---- | :---- | :---- | :---- |
| no data 🚨 | no data 🚨 | no data 🚨 | no data 🚨 | no data 🚨 | no data 🚨 |

Then we lose all information about Bob, including his email, which may be needed for future marketing or communication. The product information is also lost, which may be relevant for inventory or sales analysis.

---

## The Normal Forms

Normalization of data is a process of analyzing the given relation schemas to achieve:

1. Minimal redundancy  
2. Minimal insertion, deletion, and update anomalies

The process involves applying a series of rules called "normal forms". The most commonly used normal forms are:

- **First Normal Form (1NF):** Eliminate repeating groups; ensure each cell contains atomic values.  
- **Second Normal Form (2NF):** Remove partial dependencies; ensure all non-key attributes depend on the entire primary key.  
- **Third Normal Form (3NF):** Remove transitive dependencies; ensure non-key attributes depend only on the primary key.

By applying these normal forms, we can create a well-structured database that minimizes redundancy and maintains data integrity.

### 1NF

Rule

- Each field contains only one value i.e. no lists.  
- Each row is unique.

The `Orders` table is currently not 1NF. Because each order can have multiple products, we have to either:

1. Use a delimiter to separate products in a single cell e.g. "Apple, Banana, Pear"  
2. Create multiple rows for each product in the same order

For the first option, storing multiple values in a single cell violates 1NF because each field must contain only one value.

For the second option, we create multiple rows for each product in the same order:

| order\_id | customer\_name | customer\_email | product\_name | product\_price | quantity |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Alice | [alice@a.com](mailto:alice@a.com) | Orange | 3.00 | 1 |
| 1 | Alice | [alice@a.com](mailto:alice@a.com) | Apple | 4.00 | 1 |
| 2 | Bob | [bob@a.com](mailto:bob@a.com) | Banana | 2.00 | 2 |

Now the table is in 1NF because each cell contains atomic values and each row is unique, identified by the composite primary key `(order_id, product_name)` because an order can have multiple products.

### 2NF

The table has potential anomalies still because e.g. if the `product_price` of "Apple" changes, we have to update multiple rows. If we miss one, we get inconsistencies.

Rule

- Be in 1NF.  
- Remove partial dependencies; ensure all non-key attributes depend on the entire primary key.

A functional dependency is when one attribute uniquely determines another attribute. A partial dependency occurs when a non-key attribute depends on only part of a composite primary key.

In our table, `quantity` depends on the entire primary key `(order_id, product_name)`, but

- `product_price` depends only on `product_name`.

These are partial dependencies beacause they do not depend on the entire primary key. So the table mixes order facts, customer facts, and product facts together.

For 2NF, each table should store facts that depend on the entire primary key only.

So we must:

- Move product-related data to a separate `Products` table.

| product\_name | product\_price |
| :---- | :---- |
| Apple | 4.00 |

Then our `Orders` table becomes:

| order\_id | customer\_name | customer\_email | product\_name | quantity |
| :---- | :---- | :---- | :---- | :---- |
| 1 | Alice | [alice@a.com](mailto:alice@a.com) | Orange | 1 |
| 1 | Alice | [alice@a.com](mailto:alice@a.com) | Apple | 1 |
| 2 | Bob | [bob@a.com](mailto:bob@a.com) | Banana | 2 |

Now both tables are in 2NF because all non-key attributes depend on the entire primary key.

### 3NF

Rule

- Be in 2NF.  
- Remove transitive dependencies; ensure non-key attributes depend only on the primary key.

A transitive dependency occurs when a non-key attribute depends on another non-key attribute. In our `Orders` table, `customer_email` depends on `customer_name`, which is a non-key attribute. This creates a transitive dependency because `customer_email` does not depend directly on the primary key `order_id`.

`order_id` → `customer_name` → `customer_email`

To achieve 3NF, we must remove this transitive dependency by creating a separate `Customers` table:

| customer\_name | customer\_email |
| :---- | :---- |
| Alice | [alice@a.com](mailto:alice@a.com) |
| Bob | [bob@a.com](mailto:bob@a.com) |

Then our `Orders` table becomes:

| order\_id | customer\_name | product\_name | quantity |
| :---- | :---- | :---- | :---- |
| 1 | Alice | Orange | 1 |
| 1 | Alice | Apple | 1 |
| 2 | Bob | Banana | 2 |

Now both tables are in 3NF because all non-key attributes depend only on the primary key.

---

# A Note on Normalization for App Development vs Data Science

In application development, when we model entities and relationships correctly (ER modeling), we will almost always end up with normalized tables that is already in 3NF.

This happens because ER modeling encourages us to think about entities (tables) and their attributes (columns) separately, leading to a natural separation of concerns. In ER modeling, we ask what thing does as attribute describe? If it describes another entity, it should be in a separate table. This eliminates redundancy and ensures that each fact is stored in exactly one place. The *fact* refers to a piece of information stored in the database.

Normalization may still be necessary when after designing the ER model, we find that some tables still contain partial or transitive dependencies. However, this is less common if the ER model is well-designed from the start.

But for Data Science, the source data we get may be aggregated or denormalized for performance reasons. Such flat tables compress multiple entities and relationships into a single table e.g.

| order\_id | order\_date | user\_id | user\_name | product\_id | product\_name | quantity | price |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | 2023-10-01 | 101 | Alice | 201 | Apple | 2 | 4.00 |

We learn normalization to not design databases, but to understand, reason about and reshape flat data for analysis when necessary.  
