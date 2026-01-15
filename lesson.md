# **📘 Lesson Plan: Data Modeling & Schema Design (3 Hours)**

Module: 1.2 Data Modeling

Target Audience: Adult Learners (Basic Python knowledge)

Methodology: Flipped Classroom / Code-Along / Problem-Based Learning

## **🎯 Learning Objectives**

By the end of this session, learners will be able to:

1. **Analyze** the differences between Relational, NoSQL, and Vector databases to select the correct tool for a business problem.  
2. **Apply** the concepts of Primary Keys and Foreign Keys to **create** a logical Entity-Relationship Diagram (ERD) using DBML.  
3. **Evaluate** a raw, un-normalized dataset and **decompose** it into a 3rd Normal Form (3NF) schema to reduce data redundancy.

## **⏱️ Agenda & Topic Prioritization**

| Time | Section | Focus | Top 3 Core Topics (Must Cover) | Optional Topics (Mention briefly or Skip) |
| :---- | :---- | :---- | :---- | :---- |
| **0:00 \- 0:50** | **1\. The Data Landscape** | **Analyze** | **Relational vs. Non-Relational Context:** Understanding *when* to use SQL is as important as *how*. | History of SQL, Storage engines, Vector math details. |
| *0:50 \- 1:00* | *Break* |  |  |  |
| **1:00 \- 1:50** | **2\. Blueprinting (ERD)** | **Create** | **ERD Construction & Keys (PK/FK):** The logic of linking tables via Foreign Keys. | Recursive relationships, Complex Many-to-Many logic. |
| *1:50 \- 2:00* | *Break* |  |  |  |
| **2:00 \- 2:50** | **3\. Normalization** | **Evaluate** | **Practical Normalization (1NF-3NF):** Decomposing flat spreadsheets into relational structures. | BCNF, 4NF, 5NF, Formal definitions of "Transitive Dependency". |
| **2:50 \- 3:00** | **Wrap Up** | **Synthesize** | Q\&A, Next Steps. |  |

---
## **🔵 Section 1: The Landscape of Data (50 Mins)**

**Goal:** Demystify databases and establish why SQL is the standard for structured data.

### **1.1 The Narrative**

Welcome everyone. Since you know Python, you know how to store data in variables. But what happens when you turn the computer off? The data is gone.

To persist data, we need a Database. But not all data is created equal.

* You wouldn't use a spreadsheet to store a 4K movie.  
* You wouldn't use a video player to calculate your taxes.

Today, we aren't just learning code; we are learning **Architecture**. We are going to learn how to decide *where* data lives.

### **1.2 Concept: The Three Pillars**

*Display this comparison table.*

| Type | The "Vibe" | Best For... | Tech Examples |
| :---- | :---- | :---- | :---- |
| **Relational (SQL)** | **Structured & Rigid.** Think of it like a bank vault. Highly organized, strict rules. | Financials, Inventories, User Accounts. | PostgreSQL, MySQL, Snowflake |
| **NoSQL** | **Flexible & Fast.** Think of it like a messy desk. Throw documents anywhere, find them fast. | Social Media feeds, Chat logs, IoT sensor data. | MongoDB, Cassandra |
| **Vector** | **Semantic & AI.** Think of it like a brain association game. "King" is close to "Queen". | Image search, LLM Memory, Recommendation engines. | Pinecone, Weaviate |

### **1.3 SQL Data Types**

Here are the common data types, every database has its own set of data types.

| Data Type  | Description                |
| ---------- | -------------------------- |
| `INT`      | Integer, whole number.     |
| `VARCHAR`  | Variable length character. |
| `TEXT`     | Long text.                 |
| `DATE`     | Date.                      |
| `TIME`     | Time.                      |
| `DATETIME` | Date and time.             |
| `BOOLEAN`  | True or false.             |


### **🟢 Activity 1: The Sorting Game (15 Mins)**

Type: Class Discussion / Quick Poll

Prompt: "I am the CEO of a new Startup. I have 4 features I need to build. Tell me which database type I should use and WHY."

1. **User Profile Pictures:** 
    <details>
      <summary>Answer: </summary>
      NoSQL/Object Store for the image binary, or SQL for the file path.
    </details>

2. **Payment Processing:**  
   <details>
     <summary>Answer: </summary>
     SQL. We need ACID (Atomicity, Consistency, Isolation, and Durability) compliance/Transactions.
   </details>

3. **"Find me songs that sound like Jazz":** 
   <details>
     <summary>Answer: </summary>  
     Vector Database
   </details>

4. **A live chat for a video game:** 
   <details>
     <summary>Answer: </summary>
     NoSQL. High volume, simple structure.
   </details>

## **🔵 Section 2: Building the Blueprint (ERD) (50 Mins)**

**Goal:** Learn the syntax of relationships using DBML.

### **2.1 The Narrative**

Before we write SQL queries, we need a map. An Architect draws a blueprint before the construction crew lays bricks. An **ERD (Entity Relationship Diagram)** is our blueprint.

The glue that holds this blueprint together is the **ID**.

### **2.2 Concept: The Keys**

1. **Primary Key (PK):** The unique NRIC number of a row.  
   * *Rule:* It creates identity. If two rows have the same PK, the database explodes (throws an error).  
2. **Foreign Key (FK):** The reference pointing to someone else's PK.  
   * *Rule:* It creates relationships. "I belong to that person over there."

### **🟢 Activity 2.2.1: Code-Along \- Car Insurance Schema (15 Mins)**

Tools: [dbdiagram.io](https://dbdiagram.io/d)

Instructions: Paste the following code. Review the comments as we go.

```dbml
// --- 1. Define the Customer ---
// A 'Table' represents a noun (a person, place, or thing).
Table customers {
  id int [pk, increment] // PRIMARY KEY: The unique ID for every customer
  name varchar           // Their full name
  address varchar        // Where they live
  phone varchar          // Contact info
  email varchar          // Contact info
}

// --- 2. Define the Car ---
// A car cannot exist in our system without an owner.
Table cars {
  id int [pk, increment]
  make varchar           // e.g., Toyota
  model varchar          // e.g., Corolla
  year int
  car_plate varchar
  
  // FOREIGN KEY: This is the critical link.
  // It holds the ID of the customer who owns this car.
  customer_id int 
}

// --- 3. Define the Link (Relationship) ---
// The '>' symbol translates to "One-to-Many".
// Read as: "One Customer can have Many Cars"
Ref: cars.customer_id > customers.id 


// --- 🟢 CHALLENGE ---
// Task: Add an 'accidents' table below.
// Requirements:
// 1. Accidents have a date, location, and description.
// 2. An accident happens to a specific CAR.
// 3. Link the accident to the car.
```
### **Solution**

<details>

  <summary>Click here to view solution</summary>

#### **Activity 2.2.1**
```dbml
Table accidents {
  id int [pk, increment]
  date datetime
  location varchar
  description text
  
  car_id int // FK pointing to the car
}
Ref: accidents.car_id > cars.id
```
</details>


### **🟢 Activity 2.2.2 Workshop: School System (10 Mins)**

Construct an ERD for a school system whose classes have students and teachers. Each student belongs to a single class. Each teacher may teach more than one class, and each class may have more than one teacher.

Each entity has the following attributes:

> - Student: id, name, address, phone, email, class_id
> - Teacher: id, name, address, phone, email
> - Class: id, name, teacher_id

* Write the DBML to create the ERD.
* Submit your code in Discord Peer-Review Channel: https://discord.com/channels/1165846570177150996/1457586759667028094

## **🔵 Section 3: Normalization (Cleaning the House) (50 Mins)**

**Goal:** Convert messy data into efficient tables using Normal Forms.

### **3.1 The Narrative**

Imagine a spreadsheet where every time you bought a coffee, the store wrote down your full home address next to the coffee price.

If you move house, they have to find every coffee you ever bought and update your address.

This is called an Update Anomaly.

Normalization is the process of splitting tables apart so we store data in exactly one place.

### **3.2 Concept: The Three Normal Forms**

* **1NF (First Normal Form):** **No Lists.** You cannot have a cell with "Apple, Banana, Pear". Break them into rows.  
* **2NF (Second Normal Form):** **The Whole Key.** (Mostly relevant for composite keys). Every column must rely on the *entire* unique ID.  
* **3NF (Third Normal Form):** **No Pass-Throughs.** A column should not depend on another non-key column. (e.g., ItemPrice depends on the Item, not the Order).

### **🟢 Activity 3: The E-Commerce Deconstruction (Group Breakout \- 20 Mins)**

#### 3.2.1 First Normal Form (1NF)

The `OrderDetails` table is in 1NF because each row is unique and each column has a single value.

| OrderID | ItemID | ItemName | ItemPrice | CustomerID | CustomerName | OrderDate  |
| ------- | ------ | -------- | --------- | ---------- | ------------ | ---------- |
| 100     | 10     | iPhone   | 1000      | 1          | John         | 2021-01-01 |
| 100     | 20     | iPad     | 500       | 1          | John         | 2021-01-01 |
| 200     | 30     | Macbook  | 2000      | 1          | John         | 2021-01-02 |
| 300     | 10     | iPhone   | 1000      | 2          | Mary         | 2021-01-03 |
| 300     | 30     | Macbook  | 2000      | 2          | Mary         | 2021-01-03 |

The problem is that now we don’t have a unique primary key. That is, 100 occurs in
the `OrderID` column in two different rows.

To create a unique primary (composite) key, let's number the lines in each order by adding a new column called `LineNumber`.

| OrderID | LineNumber | ItemID | ItemName | ItemPrice | CustomerID | CustomerName | OrderDate  |
| ------- | ---------- | ------ | -------- | --------- | ---------- | ------------ | ---------- |
| 100     | 1          | 10     | iPhone   | 1000      | 1          | John         | 2021-01-01 |
| 100     | 2          | 20     | iPad     | 500       | 1          | John         | 2021-01-01 |
| 200     | 1          | 30     | Macbook  | 2000      | 1          | John         | 2021-01-02 |
| 300     | 1          | 10     | iPhone   | 1000      | 2          | Mary         | 2021-01-03 |
| 300     | 2          | 30     | Macbook  | 2000      | 2          | Mary         | 2021-01-03 |

Now we have a unique primary key, which is the combination of `OrderID` and `LineNumber`.

#### 3.2.2 Second Normal Form (2NF)

To reach 2NF, we need to remove partial dependencies. A partial dependency is when one or more columns in a table depend on a subset of the primary key, but not on the whole primary key; it can only occur only when the _primary key is composite_.

In this case, the last three columns (`CustomerID`, `CustomerName`, `OrderDate`) depend on only part of the primary key (`OrderID`). They do not depend on the `LineNumber` column.

To fix this, we need to split the table into two tables: `Orders` and `OrderLineItems`.

`Orders` table:

| OrderID | CustomerID | CustomerName | OrderDate  |
| ------- | ---------- | ------------ | ---------- |
| 100     | 1          | John         | 2021-01-01 |
| 200     | 1          | John         | 2021-01-02 |
| 300     | 2          | Mary         | 2021-01-03 |

`OrderLineItems` table:

| OrderID | LineNumber | ItemID | ItemName | ItemPrice |
| ------- | ---------- | ------ | -------- | --------- |
| 100     | 1          | 10     | iPhone   | 1000      |
| 100     | 2          | 20     | iPad     | 500       |
| 200     | 1          | 30     | Macbook  | 2000      |
| 300     | 1          | 10     | iPhone   | 1000      |
| 300     | 2          | 30     | Macbook  | 2000      |

#### 3.2.3 Third Normal Form (3NF)

We have a messy table called `OrderLineItems`. It violates 3NF because `ItemName` and `ItemPrice` depend on `ItemID`, not on the specific order. This is a transitive dependency. A transitive dependency is when one or more columns in a table depend on a non-key column in that table.

**Workshop 3.2.3: Let's break `OrderLineItems` into two tables: `OrderLineItems` and `Items`.**

> Using [dbdiagram.io](https://dbdiagram.io/d), learners decompose this into two clean tables: OrderLineItems and Items.

* Write the DBML to create the ERD.
* Submit your code in Discord Peer-Review Channel: https://discord.com/channels/1165846570177150996/1457586759667028094


### **3.3 Synthesis & Discussion**

* **Instructor:** "In our solution, if the price of the iPhone goes up to $1200 next year, does the old order history change?"  
* **Learner Goal:** Realize that while Normalization is good, sometimes we *denormalize* (copy data) like sold\_price to preserve historical accuracy.
* [More in Post-Class](./post-class.md)


## **🏁 Wrap Up (10 Mins)**

1. **Review Objectives:** Did we choose the right database? Did we build an ERD? Did we normalize a table?  
2. **Homework:** Take a screenshot of an app you use (e.g., Instagram, Spotify) and try to draw the ERD for just one screen.  
3. **Next Session Teaser:** "Now that we have our Blueprint, next week we actually start building the house using SQL CREATE and SELECT commands."


### **Solution**

<details>

  <summary>Click here to view solution</summary>

#### **Workshop 3.2.3**

```dbml
// 1. The "Master" List of Products
// Information here only changes if the Product itself changes.
Table items {
  item_id int [pk]      // The Key
  item_name varchar     // Depends on item_id
  current_price int     // Depends on item_id
}
// 2. The Transaction List
// Information here is about the specific event of buying.
Table order_line_items {
  order_id int
  line_number int
  
  // Link to the item
  item_id int 
  
  // NOTE: We might keep 'sold_price' here to freeze the history!
  // This is a great discussion point for advanced learners.
  sold_price int 
}

Ref: order_line_items.item_id > items.item_id
```
</details>
