What Each Adversarial Input Reveals

- SQL injection input reveals that directly concatenating user input can change query logic.

- PreparedStatement demonstrates how parameterized queries protect SQL structure.

- Incorrect field count reveals that file validation is necessary.

- Non-numeric amount reveals that numeric conversion must be handled safely.

- Empty lines reveal the need to ignore blank records.

- The program should continue processing after encountering an invalid record.

Alternative Concept: Escaping or Regular Expression Validation

If SQL input is escaped or validated using regular expressions:

- The application attempts to reject or modify dangerous input before constructing the SQL query.

- However, escaping rules can differ between databases and character encodings.

- Regular expressions cannot reliably replace parameterized query handling.

- Different query structures may require different validation rules.

Complexity Analysis:

- Regular expression validation: Usually O(m) for an input of length m with a suitable pattern.

- Escaping: O(m) for an input of length m.

- PreparedStatement: Parameter binding is generally O(m) for processing input, in addition to query execution.

- All approaches may have similar small input-processing costs.

- PreparedStatement is preferred because it separates SQL structure from user data.

Conclusion:

PreparedStatement provides a safer and more reliable solution than manually escaping or validating SQL input.
