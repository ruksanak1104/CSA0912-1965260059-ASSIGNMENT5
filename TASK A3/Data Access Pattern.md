TASK A3: DISCOVER THE DATA-ACCESS DESIGN PATTERN 

A3.1 Identify the Common Pattern 

The project contains three classes: StudentDB, CourseDB and MarksDB.

Repeated operations:

- Establishing a database connection.

- Creating SQL queries.

- Executing SQL statements.

- Reading records from ResultSet.

- Converting database records into Java objects.

- Closing database connections.

Parts that differ:

- Table names.

- Column names.

- SQL queries.

- Java object types.

- Primary key values.

- Mapping between database columns and object fields.

Problem:

- The same database operations are repeated in multiple classes.

- This increases code duplication.

- Maintaining and modifying the application becomes difficult.

A3.2 Design a Generic Data-Access Interface 

A generic DAO (Data Access Object) interface provides common database operations.Explanation:

- Dao<T,K> defines common database operations.

- T represents the Java object type.

- K represents the primary key type.

- StudentDao implements the interface for Student objects.

- The same interface can be reused for Course and Marks entities.

A3.3 Map Objects to Tables 

Student object fields:

- studentId maps to student_id.

- name maps to name.

- email maps to email.

SQL Table:

CREATE TABLE student (
   
    student_id INT PRIMARY KEY,
    
    name VARCHAR(100),
    
    email VARCHAR(100)
);


Primary Key:

- student_id uniquely identifies each student.

- It prevents duplicate student IDs.

- It is used by findById(), update() and delete().

Alternative Concept: SQL Directly Inside Service or GUI Classes

If SQL queries are directly written inside service or GUI classes:

- Every class must contain its own database queries.

- When a database column is renamed, all affected SQL queries must be modified.

- The same SQL logic may be repeated in several places.

Comparison:

- Generic DAO:

  - SQL operations are centralized.

  - Fewer places need modification.

  - Easier maintenance and testing.

- Direct SQL:

  - SQL is scattered throughout the application.

  - More places must be changed.

  - Greater possibility of inconsistent updates.

Complexity Analysis:

- Both approaches generally have the same database operation time complexity.

- Searching by a primary key with an index is approximately O(log n) for a typical B-tree index.

- Without an index, a search may require O(n).

- Space complexity per operation is generally O(1) for a single-record operation, excluding returned collections.

- DAO improves maintainability, not necessarily execution speed.

