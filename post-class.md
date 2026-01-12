# Assignment

## Brief

Create an ERD for each of the following case study question.

## Instructions

Paste the answer as DBML in the answer code section below each question.

### Question 1

Construct an ERD for a social media company whose database includes information about users, their followers, and the posts that they make. Users can follow multiple users and create multiple posts.

Each entity has the following attributes:

- User: id, username, email, created_at
- Post: id, title, body, user_id, status, created_at
- Follows: following_user_id, followed_user_id, created_at

Answer:

```dbml

```

### Question 2

Construct an ERD for a company that sells books online. The company has a website where customers can browse available books and add them to their shopping carts. Each cart can contain multiple books.

There are 4 entities, think of what attributes each entity should have.

- Customer
- Book
- Cart
- CartItem

Answer:

```dbml

```

## **Post-Class: Deep Dive (Advanced Learners)**

*These topics are optional but recommended for those pursuing Data Engineering.*

### **Referential Integrity**

A deeper look at ON DELETE CASCADE vs ON DELETE SET NULL.

* **Challenge:** If you delete a Teacher, what happens to the Students?

### **Data Types Cheat Sheet**

| Type | Use Case |
| :---- | :---- |
| INT | Counts, IDs. |
| VARCHAR | Names, Short descriptions. |
| TEXT | Long comments, blogs. |
| DATETIME | Timestamps. |
| BOOLEAN | IsActive, HasPaid. |

### **Self-Study Assignment: The Music Streamer**

Design a schema for a Spotify-clone. How do you handle a "Many-to-Many" relationship between Songs and Playlists?  
(Hint: You will need a "Junction Table" in the middle\!) 

## **💡Please Share Your Answers & Thoughts in Discord💡**
