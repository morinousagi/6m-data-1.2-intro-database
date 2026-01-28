# **🎓 Pre-Study Guide: Data Modeling & Schema Design**

**Topic:** Introduction to SQL Data Modeling **Estimated Time:** 30–45 Minutes **Prerequisites:** Basic understanding of Python variables and lists.

* **Watch:** [A Brief History of Databases](https://www.youtube.com/watch?v=WDTbDiQqPdA)  
* **Think:** Why do we need a database instead of just using a giant Excel spreadsheet?  
* **Setup:** Create a free account at [dbdiagram.io](https://dbdiagram.io/).

## **👋 Welcome to Module 1.2**

In our upcoming live session, we won't be spending much time looking at slides. Instead, we will be **building** database schemas for a Car Insurance company and an E-Commerce platform.

To maximize our time "doing" rather than "listening," please read through this guide and complete the quick setup task before class.

## **📚 Part 1: The Core Concepts**

*Read these 3 sections to understand the vocabulary we will use.*

### **1\. The Database Landscape**

Not all data is stored in rows and columns. We generally categorize databases into three types:

* **Relational (SQL):** Think of this like a **Bank Vault**. It is strict, organized, and safe. We use it for financial records, user accounts, and inventory. (Examples: PostgreSQL, MySQL).  
* **NoSQL:** Think of this like a **Messy Desk**. It is flexible and fast. You can throw documents (JSON) on it without worrying about filing cabinets. We use it for social media feeds or sensor data. (Examples: MongoDB).  
* **Vector:** Think of this like a **Brain**. It stores data by "meaning" rather than keywords. Used for AI and similarity searches (e.g., "Find me a song that sounds sad").

### **2\. The Anatomy of a Table**

In this lesson, we focus on **Relational (SQL)** databases. To link tables together, we use special IDs called **Keys**.

* **Primary Key (PK):** The unique ID of a row. Think of it like your **Social Security Number** or **Passport Number**. Every table needs one. It distinguishes "John Smith (ID: 1)" from "John Smith (ID: 2)".  
* **Foreign Key (FK):** A reference to someone else's ID. Think of this like your **Passport Number written on a Flight Manifest**. The flight manifest doesn't hold your actual passport; it just holds a *reference* (Key) that points back to you.

### **3\. Normalization (The Art of Tying Up)**

Normalization is a fancy word for "organizing data to reduce redundancy." Imagine an Excel sheet where you write a customer's address every time they buy a coffee. If they move house, you have to update 500 rows.

* **Un-normalized:** Storing the address 500 times.  
* **Normalized:** Storing the address *once* in a "Customers" table, and just referring to the `Customer_ID` in the "Orders" table.

* **Check out this:[Interactive Visulazation: Database Normalization](https://su-ntu-ctp.github.io/6m-data-1.2-intro-database/)**

## **🛠️ Part 2: Tool Setup**

We will be using a browser-based tool to draw our diagrams. Please ensure you have access.

1. Go to [**dbdiagram.io**](https://dbdiagram.io/).  
2. Click **"Go to App"** (No need to pay, the free version is sufficient).  
3. Play around for 2 minutes: Try typing `Table users { id int }` on the left side and see what happens on the right.

## **🧠 Part 3: "Ticket to Class" Activity**

*Complete this short thought experiment before the session.*

**The Receipt Challenge** Find a physical receipt (from a grocery store, cafe, or online order) or imagine one. Look at the data on it.

Identify which data belongs to the **"Transaction"** (happened once) and which data belongs to the **"Master Record"** (stays the same).

* **Date & Time:** (Transaction? or Master?)  
* **Store Address:** (Transaction? or Master?)  
* **Item Name (e.g., "Milk"):** (Transaction? or Master?)  
* **Item Price:** (Does this belong to the Milk? Or the Transaction? *Hint: What happens if the price of milk changes next week?*)

**Bring your answers to the session\!** We will use this to break down the "E-Commerce Deconstruction" activity.
