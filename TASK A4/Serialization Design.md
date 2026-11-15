TASK A4: WHAT SHOULD BE SERIALIZED? 

A4.1 Decide What to Persist 

The Hostel object contains Room objects, and each Room contains Resident objects.

Fields that should be serialized:

- Hostel information.

- Room information.

- Resident name.

- Resident register number.

Fields that should be marked transient:

- Resident password.

- Database Connection reference.

Reasons:

- Passwords should not be stored in serialized files in plain text.

- A live database connection is a runtime resource.

- A Connection object cannot normally be serialized.

- The connection must be established again when the application starts.Object Graph Serialization:

- When the Hostel object is serialized, its reachable Room objects are also serialized.

- The Resident objects contained in those Room objects are also serialized.

- All objects in the reachable object graph must implement Serializable, unless handled specially.

- Transient fields are not included in the serialized state.

- After deserialization, transient fields receive their default values, such as null for object references.

A4.2 Plan for Change 

When a new field called phoneNumber is added to Resident after old files have been saved:

If serialVersionUID remains compatible:

- The old serialized file can generally be read.

- The new field receives its default value because it was not present in the old file.

- For a String field, the default value is null.

- The application can assign a suitable value after loading.

If serialVersionUID is incompatible:

- Java may throw InvalidClassException.

- Deserialization fails because the saved class version does not match the current class version.

Example:

private static final long serialVersionUID = 1L;

String phoneNumber;

Conclusion:

Using an explicit serialVersionUID helps control compatibility between different versions of serialized classes.

Alternative Concept: CSV or JSON

If hostel information is stored in CSV or JSON:

CSV:

- Stores records as comma-separated text.

- Easy to open and inspect.

- Requires proper escaping for commas and special characters.

- Nested Room and Resident relationships require additional handling.

JSON:

- Supports structured objects and nested arrays.

- Easier to represent Hostel → Room → Resident relationships.

- More readable and portable across programming languages.

Complexity Analysis:

- Serialization: O(n) time to save and load n residents.

- CSV: O(n) time to save and load n residents.

- JSON: O(n) time to save and load n residents for ordinary sequential serialization and parsing.

- All three require O(n) storage for the complete data representation.

- Actual file sizes depend on encoding, metadata and formatting.

Handling New Fields:

- Java serialization uses class-version compatibility and default values for compatible changes.

- CSV requires an agreed column format and appropriate handling of missing columns.

- JSON can usually accommodate additional fields more flexibly, depending on the parser and application design.

Conclusion:

JSON is a suitable choice when portability and structured data are important. Java serialization is convenient for Java-specific object persistence.
