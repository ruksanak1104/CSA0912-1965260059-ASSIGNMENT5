TASK A5: DESIGN FOR CORRECTNESS AND SAFETY 

A5.1 Transaction Design 

A student transfers ₹500 from a hostel wallet to a canteen wallet.

The operation must be completed entirely or not completed at all.

JDBC Pseudocode:

ALGORITHM TransferMoney

BEGIN

    ESTABLISH database connection

    DISABLE auto-commit

    TRY

        UPDATE hostel_wallet
        
        SET balance = balance - 500

        UPDATE canteen_wallet
        
        SET balance = balance + 500

        COMMIT TRANSACTION

        DISPLAY "Transfer Successful"

    CATCH SQLException

        ROLLBACK TRANSACTION

        DISPLAY "Transfer Failed"

    FINALLY

        CLOSE database connection

END


Explanation:

- setAutoCommit(false) starts manual transaction management.

- The first update deducts ₹500.

- The second update adds ₹500.

- commit() permanently saves both changes.

- rollback() reverses both changes if an error occurs.

Important: Both wallet updates must be performed within the same database transaction using transactional tables such as MySQL InnoDB.

A5.2 Injection Analysis 

Unsafe SQL Query:

String sql =

    "SELECT * FROM users WHERE id='" +
    
    id + "' AND pwd='" + pwd + "'";


Problem:

- User input is directly combined with the SQL statement.

- A malicious input can change the intended SQL logic.

- This can allow authentication without a valid password.

Example of an adversarial test input:

ID: ' OR '1'='1' --

Password: anything


The input can alter the query's condition so that the password check is bypassed in the unsafe version.Alternative Concept: Two Separate Auto-Commit Updates

If both wallet updates use auto-commit:

- The first update may complete successfully.

- If the program fails before the second update, the money is deducted from the hostel wallet but not added to the canteen wallet.

- This creates incorrect financial data.

Complexity Analysis:

- Transaction approach: O(1) database updates for the two wallet records, assuming indexed wallet IDs.

- Separate auto-commit approach: Also O(1) per update.

- The transaction may have additional commit and locking overhead.

- Both approaches require O(1) application-side space for these updates.

- The transaction approach provides atomicity and better data correctness.

Conclusion:

Transactions are essential for operations involving multiple related database updates.
