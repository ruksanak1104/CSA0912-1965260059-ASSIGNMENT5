TASK B3: STATEMENT VS PREPAREDSTATEMENT VS BATCH 

B3.1 Correctness on Small Data 

The bus-pass records are transferred into a MySQL database using JDBC.

Step 1: Create Database

Execute the following SQL commands in MySQL Workbench.

CREATE DATABASE transport_db;

USE transport_db;

CREATE TABLE bus_pass (

    pass_id VARCHAR(20) PRIMARY KEY,

    reg_no VARCHAR(30) NOT NULL,

    name VARCHAR(100) NOT NULL,

    route_no INT NOT NULL,

    amount DOUBLE NOT NULL

);


Step 2: Insert 10 Records

- Insert 10 records using Statement.

- Insert the same 10 records using PreparedStatement.

- Insert 10 records using PreparedStatement with batching.

- Before repeating the experiment, clear the table to avoid duplicate primary keys.

Step 3: Verify Records

SELECT * FROM bus_pass;

DESCRIBE bus_pass;B3.2 Increase the Data Size 

The program must insert:

- 1,000 records.

- 10,000 records.

- 100,000 records.

Three approaches:

1. Statement

   - Builds SQL commands using string concatenation.

   - Executes each insert separately.

3. PreparedStatement

   - Uses parameterized SQL.

   - Executes each insert separately.

   - Allows the SQL statement to be reused.

5. PreparedStatement with Batch Processing

   - Uses addBatch() to collect multiple insert operations.

   - Uses executeBatch() to execute them together.

   - Reduces the number of database communication round trips.

Expected Performance Interpretation:

- Statement generally has more SQL parsing overhead.

- PreparedStatement can reuse the prepared SQL structure.

- Batch processing generally improves performance for large datasets.

- Actual timings depend on hardware, MySQL configuration, network and JDBC driver.

B3.3 Auto-commit vs Transaction 

Auto-commit ON:

- Each successful insert is committed separately.

- If a failure occurs halfway, earlier inserted rows remain in the database.

- Later rows are not inserted.

Single Transaction:

- All insert operations are performed within one transaction.

- If all operations succeed, commit() saves them.

- If any operation fails, rollback() reverses the transaction.

- No partial set of inserted records remains.

B3.4 Engineering Trade-Off 

For a nightly import of 100,000 passes:

- Use PreparedStatement with batch processing.

- Divide the records into batches of 1,000.

- Use a transaction to ensure correctness.

- Validate the input before insertion.

For a single pass issued at the counter:

- Use PreparedStatement.

- Insert one record at a time.

- Commit the transaction after successful insertion.

Conclusion:

Batch processing is more appropriate for large imports, while a single PreparedStatement is suitable for individual pass issuance.Test Transaction Failure

To test rollback:

- Create a list of valid records.

- Insert an invalid amount such as "INVALID" halfway through the list.

- The Double.parseDouble() method throws NumberFormatException.

- The catch block executes rollback().

- The transaction is cancelled.

Expected Output:

Insertion Failed: For input string: "INVALID"

Verify the Database:

SELECT COUNT(*) FROM bus_pass;


If the table was empty before the test and all inserts belonged to the failed transaction:

COUNT(*)

0


Alternative Concept: Multi-row INSERT and LOAD DATA

Multi-row INSERT:

- Multiple records are included in one SQL INSERT command.

- It reduces the number of database communication round trips.

- The SQL statement becomes larger as the number of records increases.

LOAD DATA:

- MySQL provides LOAD DATA for importing records from files.

- It is useful for large-scale data imports.

- It can load structured data efficiently.

Complexity Analysis:

- PreparedStatement batch:

  - Total processing time is approximately O(n), excluding database-specific overhead.

  - Application-side batch memory: O(b), where b is the batch size.

- Multi-row INSERT:

  - Processing time is approximately O(n), plus SQL parsing and database overhead.

  - Memory depends on the size of the generated SQL statement, approximately O(n) for one complete statement.

- LOAD DATA:

  - Processing time is approximately O(n) for reading and loading n records.

  - Memory depends on implementation and buffering.

  - It is generally suitable for large imports.

Conclusion:

PreparedStatement batching provides a good balance between performance, memory usage and transaction safety. LOAD DATA can be considered for very large MySQL imports.
