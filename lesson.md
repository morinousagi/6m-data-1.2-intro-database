# **📘 Lesson Plan: Data Modeling & Schema Design**

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
## **🔵 Section 1: The Landscape of Data**

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


### **🟢 Activity 1: The Sorting Game (10 Mins)**

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
---
## **🔵 Section 2: Building the Blueprint (ERD)**

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


### **🟢 Workshop 2.2.2 : School System (15 Mins)**

Construct an ERD for a school system whose classes have students and teachers. Each student belongs to a single class. Each teacher may teach more than one class, and each class may have more than one teacher.

Each entity has the following attributes:

> - Student: id, name, address, phone, email, class_id
> - Teacher: id, name, address, phone, email
> - Class: id, name, teacher_id

* Write the DBML to create the ERD.
* Submit your code in Discord Peer-Review Channel.

---
## **🔵 Section 3: Organizing Your Data (Like Tidying Your Home)**

**Goal:** Learn how to arrange information efficiently so you don't store the same thing multiple times.

> **Interactive Tutorial:** Before reading further, try the [Normalization Interactive Visualization](https://su-ntu-ctp.github.io/6m-data-1.2-intro-database/). Open this file in your web browser and follow the 8-step guided tour to see these ideas come to life.

### **3.1 Why This Matters: The Coffee Shop Story**

Picture this: You're a regular at a coffee shop. Every time you buy coffee, the cashier writes down your order along with your complete home address on a paper slip.

Now imagine you move to a new apartment. The coffee shop would need to dig through hundreds of old receipts and update your address on every single one. That's exhausting and error-prone!

This problem is called an "Update Anomaly" - when changing one piece of information forces you to update it in many places.

**Normalization** is like organizing your home: instead of keeping your keys in random places throughout the house, you put them in one designated spot. Similarly, we organize data so each piece of information lives in exactly one place. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

***

### **3.2 The Three Rules of Good Organization**

Think of these as three levels of tidiness. Here's a simple way to remember them: [stackoverflow](https://stackoverflow.com/questions/2331838/normalization-in-plain-english)

**"Every fact should be about the key, the whole key, and nothing but the key."**

Let's break that down with real-world examples:

#### **Rule 1 (1NF): One Item Per Space - "No Grocery Bags in Cells"**

**Real-Life Analogy:** Imagine your closet. You wouldn't stuff 3 shirts onto a single hanger and write "blue shirt, red shirt, green shirt" on one label. Each shirt gets its own space. [red-gate](https://www.red-gate.com/blog/normalization-1nf-2nf-3nf)

**The Problem - Before 1NF:**

Your shopping list looks like this:

| Person | Favorite Fruits |
|--------|----------------|
| Sarah  | Apple, Banana, Orange |
| Mike   | Grape, Mango |

**What's wrong?** The "Favorite Fruits" cell contains multiple values crammed together. If you want to count how many people like apples, you'd have to split apart that cell manually.

**The Solution - After 1NF:**

| Person | Favorite Fruit |
|--------|---------------|
| Sarah  | Apple |
| Sarah  | Banana |
| Sarah  | Orange |
| Mike   | Grape |
| Mike   | Mango |

**Why it's better:** Now each cell contains exactly one piece of information. You can easily search, count, and organize. [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

***

#### **Rule 2 (2NF): Everything Relates to the Complete ID - "The Address Book Rule"**

**Real-Life Analogy:** Think of an address book. You wouldn't write someone's home address next to each phone call you made to them. The address belongs with the person's name, not with individual calls. [datacamp](https://www.datacamp.com/tutorial/normalization-in-sql)

**The Problem - Before 2NF:**

Imagine a library tracking who borrowed which books:

| Student ID | Student Name | Book ID | Book Title | Borrow Date |
|-----------|-------------|---------|-----------|-------------|
| 001 | Alice | B10 | Harry Potter | 2026-01-10 |
| 001 | Alice | B20 | Hobbit | 2026-01-12 |
| 002 | Bob | B10 | Harry Potter | 2026-01-11 |

**What's wrong?** Alice's name appears twice. If Alice changes her name (marriage, legal change), you'd have to update multiple rows. Also, "Book Title" really depends on "Book ID," not on the specific borrowing transaction.

**The Solution - After 2NF:**

Split into three tables:

**Students Table:**
| Student ID | Student Name |
|-----------|-------------|
| 001 | Alice |
| 002 | Bob |

**Books Table:**
| Book ID | Book Title |
|---------|-----------|
| B10 | Harry Potter |
| B20 | Hobbit |

**Borrowing Records:**
| Student ID | Book ID | Borrow Date |
|-----------|---------|-------------|
| 001 | B10 | 2026-01-10 |
| 001 | B20 | 2026-01-12 |
| 002 | B10 | 2026-01-11 |

**Why it's better:** Student information lives in one place, book information lives in one place. Each piece of information connects to its complete identifier. [guides.visual-paradigm](https://guides.visual-paradigm.com/a-comprehensive-guide-to-database-normalization-with-examples/)

***

#### **Rule 3 (3NF): No Chain Dependencies - "The Phone Directory Principle"**

**Real-Life Analogy:** In a phone directory, you look up a person's name to get their phone number. You don't look up their phone number to find their city, then use the city to find their zip code. That's a "chain" - one thing leading to another. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

**The Problem - Before 3NF:**

Your employee database looks like this:

| Employee ID | Employee Name | Department Code | Department Name | Department Manager |
|------------|--------------|-----------------|-----------------|-------------------|
| E001 | Alice | D10 | Sales | John Smith |
| E002 | Bob | D10 | Sales | John Smith |
| E003 | Carol | D20 | Marketing | Jane Doe |

**What's wrong?** "Department Name" and "Department Manager" don't really depend on the employee. They depend on the "Department Code." If the Sales manager changes from John Smith to Mary Johnson, you'd have to update every sales employee's row. [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

**Visual representation of the dependency chain:**
```
Employee ID → Department Code → Department Name
Employee ID → Department Code → Department Manager
```

This is called a "transitive dependency" - information depending on something that depends on something else. [datacamp](https://www.datacamp.com/tutorial/normalization-in-sql)

**The Solution - After 3NF:**

Split into two tables:

**Employees Table:**
| Employee ID | Employee Name | Department Code |
|------------|--------------|-----------------|
| E001 | Alice | D10 |
| E002 | Bob | D10 |
| E003 | Carol | D20 |

**Departments Table:**
| Department Code | Department Name | Department Manager |
|-----------------|-----------------|-------------------|
| D10 | Sales | John Smith |
| D20 | Marketing | Jane Doe |

**Why it's better:** When the Sales manager changes, you update ONE cell in the Departments table. All employees automatically reflect this change when you look up their department. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

***

### **🟢 Activity 3: The E-Commerce Deconstruction (Group Breakout - 20 Mins)**

Let's practice with a real online store example, going through all three rules step by step.

#### **Starting Point: The Messy Spreadsheet**

Imagine you're running an online store and currently track everything in one big table:

| OrderID | ItemID | ItemName | ItemPrice | CustomerID | CustomerName | OrderDate |
|---------|--------|----------|-----------|-----------|-------------|-----------|
| 100 | 10 | iPhone | 1000 | 1 | John | 2021-01-01 |
| 100 | 20 | iPad | 500 | 1 | John | 2021-01-01 |
| 200 | 30 | Macbook | 2000 | 1 | John | 2021-01-02 |
| 300 | 10 | iPhone | 1000 | 2 | Mary | 2021-01-03 |
| 300 | 30 | Macbook | 2000 | 2 | Mary | 2021-01-03 |

***

#### **Step 1: Applying Rule 1 (1NF) - Making Each Row Unique**

**The Problem:** Look at OrderID 100 - it appears in two rows (iPhone and iPad). Which row is "the order"? Both? This is confusing!

**Think of it like a receipt:** When you get a store receipt, it has ONE receipt number at the top, but multiple line items below. Each line item needs its own identifier.

**The Solution:** Add a line number to create a unique identifier for each row:

| OrderID | LineNumber | ItemID | ItemName | ItemPrice | CustomerID | CustomerName | OrderDate |
|---------|-----------|--------|----------|-----------|-----------|-------------|-----------|
| 100 | 1 | 10 | iPhone | 1000 | 1 | John | 2021-01-01 |
| 100 | 2 | 20 | iPad | 500 | 1 | John | 2021-01-01 |
| 200 | 1 | 30 | Macbook | 2000 | 1 | John | 2021-01-02 |
| 300 | 1 | 10 | iPhone | 1000 | 2 | Mary | 2021-01-03 |
| 300 | 2 | 30 | Macbook | 2000 | 2 | Mary | 2021-01-03 |

**Key Insight:** Now each row has a unique two-part ID: (OrderID + LineNumber). Just like "Building A, Apartment 301" uniquely identifies a location.

✅ **Rule 1 Complete!** Each row is now uniquely identifiable.

***

#### **Step 2: Applying Rule 2 (2NF) - Separating Order Info from Item Info**

**The Problem:** Look at John's information (CustomerID, CustomerName, OrderDate). Does this change between line items? No! Whether John bought an iPhone (line 1) or an iPad (line 2), he's still John, and the order date is still 2021-01-01.

**Visual Diagram - What depends on what:**

```
(OrderID + LineNumber) → ItemID, ItemName, ItemPrice ✓ (Needs both parts)
OrderID only → CustomerID, CustomerName, OrderDate ✓ (Doesn't need LineNumber)
```

The customer information only cares about OrderID, not LineNumber. This violates Rule 2. [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

**The Solution:** Split into two tables based on what the information really describes:

**Orders Table** (one row per order):

| OrderID | CustomerID | CustomerName | OrderDate |
|---------|-----------|-------------|-----------|
| 100 | 1 | John | 2021-01-01 |
| 200 | 1 | John | 2021-01-02 |
| 300 | 2 | Mary | 2021-01-03 |

**Order Line Items Table** (one row per item in an order):

| OrderID | LineNumber | ItemID | ItemName | ItemPrice |
|---------|-----------|--------|----------|-----------|
| 100 | 1 | 10 | iPhone | 1000 |
| 100 | 2 | 20 | iPad | 500 |
| 200 | 1 | 30 | Macbook | 2000 |
| 300 | 1 | 10 | iPhone | 1000 |
| 300 | 2 | 30 | Macbook | 2000 |

**Real-World Comparison:**
- **Orders Table** = The envelope with order details
- **Order Line Items Table** = The itemized list inside the envelope

✅ **Rule 2 Complete!** John's name now appears only once per order, not once per item.

***

#### **Step 3: Applying Rule 3 (3NF) - Creating a Product Catalog**

**The Problem:** Look at the iPhone - it appears in Order 100 and Order 300, both times with ItemName "iPhone" and ItemPrice 1000. That's duplicate information!

**Ask yourself:** Does the iPhone's name and price depend on which specific order purchased it? No! An iPhone is an iPhone regardless of who buys it. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

**Visual Diagram - The Chain Dependency:**

```
(OrderID + LineNumber) → ItemID → ItemName, ItemPrice
                         ↑         ↑
                         |         |
                    This is the chain we need to break!
```

ItemName and ItemPrice depend on ItemID, not on the specific order. This is a "transitive dependency". [datacamp](https://www.datacamp.com/tutorial/normalization-in-sql)

**The Solution:** Create a separate Products catalog:

**Products Table** (one row per product):

| ItemID | ItemName | ItemPrice |
|--------|----------|-----------|
| 10 | iPhone | 1000 |
| 20 | iPad | 500 |
| 30 | Macbook | 2000 |

**Simplified Order Line Items Table:**

| OrderID | LineNumber | ItemID |
|---------|-----------|--------|
| 100 | 1 | 10 |
| 100 | 2 | 20 |
| 200 | 1 | 30 |
| 300 | 1 | 10 |
| 300 | 2 | 30 |

**Real-World Comparison:**
- **Products Table** = Your store's product catalog
- **Order Line Items** = References pointing to items in the catalog

**The Big Win:** If the iPhone price changes to $1,200, you update ONE cell in the Products table. Every query that asks "What's the current price of an iPhone?" automatically gets the new price.

✅ **Rule 3 Complete!** No more chain dependencies!

***

### **📊 Visual Summary: Before and After**

**BEFORE NORMALIZATION:**
```
One Big Messy Table (25 rows for 5 orders × 5 details)
❌ John's name repeated 3 times
❌ iPhone details repeated 2 times
❌ Must update multiple places when anything changes
```

**AFTER NORMALIZATION (1NF → 2NF → 3NF):**
```
Orders Table: 3 rows
Order Line Items: 5 rows
Products Table: 3 rows
Total: 11 rows (less than half!)

✅ Each fact stored exactly once
✅ Easy to update anything anywhere
✅ No contradictions possible
```

***

### **🟢 Workshop 3.2.3: Your Turn to Practice**

**Your Assignment:**

Visit [dbdiagram.io](https://dbdiagram.io/d) and create a visual diagram showing how these three tables connect to each other. Here's what to show:

1. Draw three boxes (one for Orders, one for Order Line Items, one for Products)
2. Show lines connecting them (Orders → Order Line Items → Products)
3. Mark which columns are IDs that link the tables together

**Hint:** The connecting lines represent "relationships" - like "Each Order has many Line Items" and "Each Line Item refers to one Product."

Share your diagram in Discord: [https://discord.com/channels/1165846570177150996/1457586759667028094](https://discord.com/channels/1165846570177150996/1457586759667028094)

***

### **3.3 Group Discussion: When Breaking Rules Makes Sense**

**Scenario:** Your normalized database is working beautifully. Products are stored once, orders reference them cleanly.

**Question:** "In our cleaned-up design, if the iPhone price increases to $1,200 next year, what happens to Mary's order from 2021?"

**Think about it:**
- In our normalized design, there's ONE iPhone price in the Products table
- Mary ordered in 2021 when the price was $1,000
- If we update the price to $1,200, does Mary's historical order suddenly show $1,200?

**The Answer:** Yes, it would! And that's **wrong** for accounting purposes. [stackoverflow](https://stackoverflow.com/questions/2331838/normalization-in-plain-english)

**The Solution - Intentional Denormalization:**

Add a "sold_price" column to Order Line Items:

| OrderID | LineNumber | ItemID | **Sold_Price** |
|---------|-----------|--------|---------------|
| 100 | 1 | 10 | **1000** |
| 300 | 1 | 10 | **1000** |

This way:
- **Products Table** shows the current price ($1,200)
- **Order history** preserves what was actually paid ($1,000)

**Real-Life Analogy:** This is like keeping old receipts. Even if a store raises prices today, your receipt from last year still shows what you paid back then. The store needs both: current prices (for new sales) and historical prices (for accounting records). [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

**Key Lesson:** Normalization is a powerful tool, but sometimes you intentionally keep duplicate data for good business reasons. This is called "denormalization," and it's a conscious design choice, not a mistake.

***

### **🎯 Quick Reference Card**

Print this and keep it handy:

| Rule | Remember It As | Ask Yourself |
|------|---------------|--------------|
| **1NF** | One item per space | Are there any cells with lists or multiple values? |
| **2NF** | The whole key | Does every column need the ENTIRE ID to make sense? |
| **3NF** | Nothing but the key | Does any column depend on another regular column? |

**The Golden Question:** "If I change this piece of information, how many places do I need to update?"
- **Many places?** You probably need more normalization.
- **One place?** You're doing it right! [stackoverflow](https://stackoverflow.com/questions/2331838/normalization-in-plain-english)

***

