# DBML Guide (Database Markup Language)

## Overview

DBML (Database Markup Language) is a simple, readable domain-specific language designed to define and document database structures. It was created by the team at Holistics to make database schema design more accessible and collaborative.

## DBML vs SQL

DBML serves the same purpose as SQL DDL (Data Definition Language) statements \- defining database schemas \- but with a simpler, more human-readable syntax.

**SQL DDL Example:**

CREATE TABLE users (

    id INTEGER PRIMARY KEY,

    username VARCHAR(50) NOT NULL UNIQUE,

    email VARCHAR(255) NOT NULL UNIQUE,

    created\_at TIMESTAMP DEFAULT CURRENT\_TIMESTAMP

);

CREATE TABLE posts (

    id INTEGER PRIMARY KEY,

    user\_id INTEGER NOT NULL,

    title VARCHAR(200),

    FOREIGN KEY (user\_id) REFERENCES users(id)

);

**Same Schema in DBML:**

Table users {

  id integer \[pk\]

  username varchar(50) \[not null, unique\]

  email varchar(255) \[not null, unique\]

  created\_at timestamp \[default: \`now()\`\]

}

Table posts {

  id integer \[pk\]

  user\_id integer \[not null\]

  title varchar(200)

}

Ref: posts.user\_id \> users.id

**Key Differences:**

- **DBML** is a design and documentation tool \- cleaner syntax, database-agnostic  
- **SQL DDL** is executable code \- database-specific, more verbose  
- DBML can be **converted to SQL** for any database system (PostgreSQL, MySQL, SQL Server, etc.)  
- DBML focuses on **schema design**, while SQL handles the actual database creation

**Workflow:**

1. Design your schema in DBML  
2. Convert DBML to SQL for your target database  
3. Execute the SQL to create your database

## Key Features

- **Human-readable syntax**: Easy to write and understand  
- **Version control friendly**: Plain text format works well with Git  
- **Cross-platform**: Can be converted to SQL for various database systems  
- **Visualization**: Can generate ER diagrams automatically  
- **Documentation**: Serves as living documentation for your database

## Basic Syntax

### Tables

Define tables using the `Table` keyword:

Table users {

  id integer \[primary key\]

  username varchar(50) \[not null, unique\]

  email varchar(255) \[not null, unique\]

  created\_at timestamp \[default: \`now()\`\]

  status varchar(20) \[default: 'active'\]

  Note: 'Users table to store user information'

}

This would define a table like this:

| id | username | email | created\_at | status |
| :---- | :---- | :---- | :---- | :---- |
| 1 | johndoe | [john@a.com](mailto:john@a.com) | 2024-01-01 10:00:00 | active |

### Data Types

Common data types supported in DBML:

| Data Type | Description | Parameters | Example Values |
| :---- | :---- | :---- | :---- |
| `integer`, `int` | Whole numbers without decimals | None | `42`, `-100`, `0` |
| `varchar(n)` | Variable-length text with max length | `n` \= max characters | `'hello'`, `'user@email.com'` |
| `text` | Variable-length text, no limit | None | Long paragraphs, articles |
| `boolean`, `bool` | True or false values | None | `true`, `false` |
| `date` | Calendar date only | None | `'2026-01-28'` |
| `time` | Time of day only | None | `'14:30:00'` |
| `timestamp` | Date and time combined | None | `'2026-01-28 14:30:00'` |
| `datetime` | Alias for timestamp | None | `'2026-01-28 14:30:00'` |
| `decimal(p,s)` | Exact decimal numbers | `p` \= precision (total digits) \`s\` \= scale (decimal places) | `decimal(10,2)` stores `12345.67` |
| `numeric(p,s)` | Alias for decimal | `p` \= precision \`s\` \= scale | Same as decimal |
| `json` | JSON formatted data | None | `'{"name": "Alice", "age": 30}'` |
| `jsonb` | Binary JSON (PostgreSQL) | None | More efficient storage than `json` |

**Key Differences:**

- **`varchar(n)` vs `text`**: `varchar` has a length limit (good for controlled fields like usernames), `text` has no limit (good for long content like descriptions)  
- **`integer` vs `decimal`**: `integer` for whole numbers, `decimal` for precise decimal values (like prices, measurements)  
- **`timestamp` vs `date` vs `time`**: `timestamp` includes both date and time, `date` only stores calendar dates, `time` only stores time of day  
- **`json` vs `jsonb`**: `jsonb` is PostgreSQL-specific and stores JSON in binary format for faster operations, `json` stores as plain text  
- **`decimal` vs `numeric`**: They are functionally identical; both provide exact decimal precision

### Column Settings

Available column settings in brackets `[...]`:

- **Primary key**: `[primary key]` or `[pk]`  
- **Not null**: `[not null]`  
- **Unique**: `[unique]`  
- **Default value**: `[default: value]`  
- **Auto increment**: `[increment]`  
- **Note**: `[note: 'description']`

Table products {

  id integer \[pk, increment\]

  name varchar(100) \[not null\]

  price decimal(10,2) \[not null, default: 0.00\]

  sku varchar(50) \[unique, note: 'Stock Keeping Unit'\]

}

### Relationships

Define relationships between tables using the `Ref` keyword.

**Important**: The `>` symbol always points from the **foreign key** to the **primary key** it references, regardless of how you describe the relationship. This is the DBML convention and ensures consistency.

#### One-to-Many / Many-to-One (1:n / n:1)

One-to-Many and Many-to-One are actually the **same relationship** viewed from different perspectives. The syntax is identical \- what changes is how you describe it:

Table users {

  id integer \[pk\]

  name varchar(50)

}

Table posts {

  id integer \[pk\]

  user\_id integer \[not null\]

  title varchar(200)

}

// The arrow always points from FK to PK

Ref: posts.user\_id \> users.id

// This relationship can be described as:

// \- One-to-Many: "One user has many posts" (from users' perspective)

// \- Many-to-One: "Many posts belong to one user" (from posts' perspective)

Another example:

Table customers {

  id integer \[pk\]

  name varchar(100)

}

Table orders {

  id integer \[pk\]

  customer\_id integer \[not null\]

  total decimal(10,2)

}

// FK \> PK (the side with the foreign key is the "many" side)

Ref: orders.customer\_id \> customers.id

// One customer has many orders / Many orders belong to one customer

#### One-to-One (1:1)

Table users {

  id integer \[pk\]

}

Table user\_profiles {

  id integer \[pk\]

  user\_id integer \[unique\]  // The UNIQUE constraint enforces 1:1

}

Ref: user\_profiles.user\_id \- users.id

// The '-' notation documents this as a 1:1 relationship

// But the actual enforcement comes from the UNIQUE constraint on user\_id

**Important**: The `unique` constraint on `user_id` is what actually enforces the one-to-one relationship at the database level. The `-` symbol is just DBML notation to indicate the relationship type.

#### Many-to-Many (n:n)

Many-to-Many relationships **require a junction table** (also called a join table or associative table) to connect the two entities. This is because you cannot directly model a many-to-many relationship in a relational database.

A Many-to-Many relationship is actually **two Many-to-One relationships** connected through the junction table:

- Many records from the junction table point to one record in the first table  
- Many records from the junction table point to one record in the second table

Table students {

  id integer \[pk\]

  name varchar(100)

}

Table courses {

  id integer \[pk\]

  name varchar(100)

}

// Junction table to connect students and courses

Table enrollments {

  student\_id integer \[pk\]

  course\_id integer \[pk\]

  enrolled\_at timestamp

}

// Two Many-to-One relationships:

Ref: enrollments.student\_id \> students.id  // Many enrollments to one student

Ref: enrollments.course\_id \> courses.id    // Many enrollments to one course

// Result: One student can enroll in many courses

//         One course can have many students

The junction table has composite primary keys (`student_id`, `course_id`) to ensure each student can enroll in a course only once. Attributes like `enrolled_at` can be added to capture additional information about the relationship.

### Inline Relationships

You can also define relationships inline:

Table posts {

  id integer \[pk\]

  user\_id integer \[ref: \> users.id\]

  category\_id integer \[ref: \> categories.id\]

}

### Indexes

Define indexes for performance optimization:

Table users {

  id integer \[pk\]

  email varchar(255)

  username varchar(50)

  created\_at timestamp

  indexes {

    email \[unique\]

    (username, created\_at) \[name: 'idx\_user\_created'\]

    created\_at \[type: btree, name: 'idx\_created'\]

  }

}

### Enums

Define enumerated types:

enum order\_status {

  pending

  processing

  shipped

  delivered

  cancelled

}

Table orders {

  id integer \[pk\]

  status order\_status \[default: 'pending'\]

}

### Table Groups

Organize related tables together:

TableGroup ecommerce {

  products

  orders

  customers

  order\_items

}

### Comments and Notes

Add documentation using notes:

Table users {

  id integer \[pk\]

  email varchar(255) \[note: 'Must be a valid email format'\]

  Note: 'This table stores all user accounts.

  Email addresses must be verified before activation.'

}

## Complete Example

Here's a complete e-commerce database schema:

Project ecommerce\_db {

  database\_type: 'PostgreSQL'

  Note: 'E-commerce database schema'

}

enum order\_status {

  pending

  confirmed

  shipped

  delivered

  cancelled

}

enum payment\_status {

  pending

  completed

  failed

  refunded

}

Table customers {

  id integer \[pk, increment\]

  first\_name varchar(50) \[not null\]

  last\_name varchar(50) \[not null\]

  email varchar(255) \[unique, not null\]

  phone varchar(20)

  created\_at timestamp \[default: \`now()\`\]

  Note: 'Customer information'

}

Table addresses {

  id integer \[pk, increment\]

  customer\_id integer \[not null, ref: \> customers.id\]

  street varchar(255) \[not null\]

  city varchar(100) \[not null\]

  state varchar(100)

  country varchar(100) \[not null\]

  postal\_code varchar(20)

  is\_default boolean \[default: false\]

}

Table products {

  id integer \[pk, increment\]

  name varchar(200) \[not null\]

  description text

  price decimal(10,2) \[not null\]

  stock\_quantity integer \[default: 0\]

  category\_id integer \[ref: \> categories.id\]

  created\_at timestamp \[default: \`now()\`\]

  indexes {

    category\_id

    (name, category\_id)

  }

}

Table categories {

  id integer \[pk, increment\]

  name varchar(100) \[not null, unique\]

  parent\_id integer \[ref: \> categories.id\]

}

Table orders {

  id integer \[pk, increment\]

  customer\_id integer \[not null, ref: \> customers.id\]

  order\_status order\_status \[default: 'pending'\]

  payment\_status payment\_status \[default: 'pending'\]

  total\_amount decimal(10,2) \[not null\]

  shipping\_address\_id integer \[ref: \> addresses.id\]

  created\_at timestamp \[default: \`now()\`\]

  updated\_at timestamp

  indexes {

    customer\_id

    (customer\_id, created\_at)

  }

}

Table order\_items {

  id integer \[pk, increment\]

  order\_id integer \[not null, ref: \> orders.id\]

  product\_id integer \[not null, ref: \> products.id\]

  quantity integer \[not null, default: 1\]

  unit\_price decimal(10,2) \[not null\]

  subtotal decimal(10,2) \[not null\]

}

Table payments {

  id integer \[pk, increment\]

  order\_id integer \[not null, ref: \> orders.id\]

  amount decimal(10,2) \[not null\]

  payment\_method varchar(50)

  payment\_status payment\_status

  transaction\_id varchar(255)

  paid\_at timestamp

}

## Tools and Resources

### Online Tools

1. **dbdiagram.io**: Online tool to create database diagrams using DBML  
     
   - Website: [https://dbdiagram.io](https://dbdiagram.io)  
   - Features: Live preview, export to SQL, PNG, PDF

   

2. **DBML CLI**: Command-line tool for working with DBML  
     
   npm install \-g @dbml/cli  
     
   \# Convert DBML to SQL  
     
   dbml2sql schema.dbml \--postgres  
     
   dbml2sql schema.dbml \--mysql  
     
   \# Convert SQL to DBML  
     
   sql2dbml dump.sql \--postgres  
     
3. **VS Code Extensions**:  
     
   - DBML Language Support  
   - Database Client

### Exporting to SQL

DBML can be converted to various SQL dialects:

**PostgreSQL:**

dbml2sql schema.dbml \--postgres \-o schema.sql

**MySQL:**

dbml2sql schema.dbml \--mysql \-o schema.sql

## Best Practices

1. **Use meaningful names**: Choose clear, descriptive names for tables and columns  
2. **Document thoroughly**: Add notes to explain business logic and constraints  
3. **Organize with groups**: Use table groups for large schemas  
4. **Version control**: Keep DBML files in Git for tracking changes  
5. **Consistent naming**: Use a consistent naming convention (snake\_case, camelCase, etc.)  
6. **Index strategically**: Define indexes on frequently queried columns  
7. **Normalize appropriately**: Follow normalization principles but balance with performance

## Advantages of Using DBML

1. **Simplicity**: Easier to write and read than raw SQL DDL  
2. **Collaboration**: Non-technical team members can understand the schema  
3. **Documentation**: Self-documenting database structure  
4. **Visualization**: Automatic ER diagram generation  
5. **Database agnostic**: Can export to multiple SQL dialects  
6. **Version control**: Plain text format works seamlessly with Git  
7. **Quick prototyping**: Rapidly design and iterate on database schemas

## Limitations

1. **Complex constraints**: Some advanced database features may not be supported  
2. **Database-specific features**: Vendor-specific features might not translate  
3. **Migration management**: Doesn't handle database migrations directly  
4. **Limited tooling**: Fewer tools compared to traditional SQL approaches

## References

- Official Documentation: [https://dbml.dbdiagram.io/](https://dbml.dbdiagram.io/)  
- GitHub Repository: [https://github.com/holistics/dbml](https://github.com/holistics/dbml)  
- dbdiagram.io: [https://dbdiagram.io](https://dbdiagram.io)

