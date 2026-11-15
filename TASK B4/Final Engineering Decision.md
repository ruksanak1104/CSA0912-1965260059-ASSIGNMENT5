TASK B4: FINAL ENGINEERING DECISION 

B4.1 Implement a Practical Strategy 

The transport office now serves 12 campuses and has 100 times the original number of bus passes.

A migration program is required to transfer records from the existing text file into MySQL.

Migration Steps:

- Read the existing bus-pass file using BufferedReader.

- Read each record line by line.

- Validate the number of fields.

- Validate numeric values.

- Store valid records in a collection.

- Insert records into MySQL using PreparedStatement batches.

- Use a single transaction for the migration.

- Record invalid records in a separate error report.

- Commit if successful.

- Roll back if the database insertion fails.Why is DatabaseMetaData useful?

- It provides information about the database product.

- It identifies the JDBC driver.

- It displays the database version.

- It lists available database tables.

- It helps identify compatibility issues when moving the application to another server.

- It makes database applications easier to maintain across different environments.

B4.3 Production Decision 

The transport office serves 12 campuses and handles a large number of bus-pass records.

1. Text Files

Advantages:

- Simple to create and maintain.

- Human-readable.

- Useful for small amounts of data.

Disadvantages:

- Searching requires scanning records.

- Concurrent updates can cause data consistency problems.

- Transaction management is difficult.

- Not ideal for multiple counters accessing records simultaneously.

2. Serialization

Advantages:

- Saves Java objects directly.

- Supports complex object relationships.

- Convenient for Java-specific applications.

Disadvantages:

- Not easily accessible from other programming languages.

- Class-version compatibility must be managed.

- Not suitable for multiple users updating shared records simultaneously.

- Requires additional synchronization and file-management mechanisms.

3. Relational Database

Advantages:

- Supports multiple users and concurrent transactions.

- Provides data consistency.

- Supports indexing for faster searching.

- Supports PreparedStatement and batch processing.

- Provides transaction management.

- Supports centralized storage for all campuses.

Final decision: MySQL Database

Reason for Selection:

- MySQL can centrally store records from all 12 campuses.

- Multiple counters can access the database concurrently.

- Transactions protect data during updates.

- Indexes improve pass-ID searching.

- Batch processing supports large imports.

- Database security and access control provide better protection.

Alternative Concept: Separate Serialized File for Each Campus

If each campus stores its own serialized file:

- Each campus maintains an independent file.

- Searching one campus requires reading its corresponding file.

- Searching all campuses requires searching all 12 files.

- Concurrent access must be controlled using synchronization or file locking.

- Updating data across campuses becomes difficult.

- Data duplication and inconsistent records may occur.

Complexity Analysis:

Assume each campus has n records.

- Searching one campus: O(n).

- Searching all 12 campuses sequentially: O(12n), which is O(n) when 12 is treated as a constant.

- Total storage: O(12n), which is O(n) for a fixed number of campuses.

- Separate files require additional coordination for concurrent updates.

Database Approach:

- Searching by an indexed pass ID is approximately O(log n) using a typical B-tree index.

- Multiple counters can access records through database transactions.

- The database provides centralized consistency and concurrency control.

Final Conclusion:

A centralized relational database is the best option for the transport office because it provides efficient searching, transaction safety, concurrent access and scalability across all 12 campuses.
