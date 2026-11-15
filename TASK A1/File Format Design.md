TASK A1: DESIGN A FILE FORMAT 

A1.1 Represent the Record 

The transport office needs to store bus-pass information for approximately 8,000 students every year.

1. Delimited Text File Format

- File name: buspasses.txt

- Each record is stored in one line.

- Fields are separated by commas.

Field order:

1. Pass ID

2. Student Register Number

3. Student Name

4. Route Number

5. Boarding Stop

6. Validity Date

7. Amount Paid

Sample record:

P1001,192311001,Arun,12,Central Bus Stop,2027-03-31,4500.00

P1002,192311002,Divya,7,Anna Nagar,2027-03-31,3800.00

Handling names containing delimiters:

- If a student's name contains a comma, directly separating fields using commas may cause errors.

- Use CSV escaping by enclosing the field in double quotes.

- For example: "Arun, Kumar".

- Another option is to use a delimiter that is not allowed in the data.

2. Binary File Format using DataOutputStream

- File name: buspasses.dat

- Records are stored in binary format.

- Use DataOutputStream to write primitive data types.

Field order and data types:

- Pass ID: writeUTF()

- Register Number: writeUTF()

- Student Name: writeUTF()

- Route Number: writeInt()

- Boarding Stop: writeUTF()

- Validity Date: writeUTF()

- Amount Paid: writeDouble()

Advantages:

- Binary files store data in a compact format.

- Primitive data types can be directly written and read.

- Data is not easily readable or editable using a normal text editor.

- writeUTF() stores strings with their encoded length, so fixed-length padding is not necessary.

A1.2 Design the Operations 

Operation 1: Add a New Bus Pass

Pseudocode:

ALGORITHM AddBusPass

BEGIN
    OPEN file using BufferedWriter in APPEND mode

    READ pass ID, register number, name,
        
         route number, boarding stop,
         
         validity date and amount

    VALIDATE the input data

    FORMAT all fields into one delimited record

    WRITE the record into the file

    WRITE a new line

    CLOSE the file

    DISPLAY "Bus pass added successfully"

END


Stream classes used:

- FileWriter: Writes character data into a text file.

- BufferedWriter: Improves writing efficiency by buffering characters.

- FileWriter in append mode prevents existing records from being overwritten.

Operation 2: Find a Bus Pass by Pass ID

Pseudocode:

ALGORITHM FindBusPass(passID)

BEGIN
    
    OPEN file using BufferedReader

    WHILE a line is available DO

        READ one line

        SPLIT the line into fields

        IF number of fields is not 7 THEN
            
            SKIP the damaged line
        
        ENDIF

        IF first field equals passID THEN
           
            DISPLAY the complete bus-pass record
            
            CLOSE the file
            
            RETURN
        
        ENDIF

    ENDWHILE

    DISPLAY "Pass not found"

    CLOSE the file

END


Stream classes used:

- FileReader: Reads character data from the file.

- BufferedReader: Reads complete lines efficiently using readLine().

- It avoids processing the file one character at a time.

Operation 3: List All Passes on a Given Route

Pseudocode:

ALGORITHM ListPassesByRoute(routeNumber)

BEGIN
    
    OPEN file using BufferedReader

    SET found = FALSE

    WHILE a line is available DO

        READ one line

        SPLIT the line into fields

        IF number of fields is not 7 THEN
           
            SKIP the damaged line
        
        ENDIF

        IF route field equals routeNumber THEN
        
            DISPLAY the bus-pass record
            
            SET found = TRUE
        
        ENDIF

    ENDWHILE

    IF found = FALSE THEN
       
        DISPLAY "No passes found for this route"
    
    ENDIF

    CLOSE the file

END


Stream classes used:

- BufferedReader for efficient line-by-line reading.

- FileReader for reading character-based records.

- BufferedWriter and FileWriter for adding records.

A1.3 Design for Damaged Data 

1. Damaged Line

- Check whether every line contains exactly seven fields.

- Validate numeric fields such as route number and amount.


- Skip invalid records and display an error message.

- Continue reading the remaining records.

2. Missing File

- Use exception handling with FileNotFoundException or IOException.

- Display an appropriate error message.

- If the file is expected to be created for the first time, create it using CREATE.

3. Incomplete Write Due to Power Failure

- A power failure may leave an incomplete record.

- Write new records to a temporary file before replacing the original file when performing a full-file update.

- Use atomic file replacement where supported.

- Validate each record when reading.

- For stronger durability, use a database transaction instead of relying only on a text file.

Alternative Concept: RandomAccessFile

If fixed-length records are used with RandomAccessFile:

- Every record occupies the same number of bytes.

- Record position can be calculated using:

\[

\text{Position}=i\times\text{recordSize}

\]

- The program can use seek() to move directly to the required record.

Comparison:

- Delimited text file:

  - Finding a pass requires scanning records.

  - Time complexity: O(n).

  - Space complexity: O(1) when reading one line at a time.

- Fixed-length RandomAccessFile:

  - Finding a record by its known index takes O(1) seek time.

  - Finding a pass by ID still requires O(n) scanning unless an index is maintained.

  - Fixed-length fields may waste space because shorter values require padding.

Conclusion:

RandomAccessFile is useful when records have fixed sizes and their positions are known. A delimited text file is simpler when records must be easily readable and modified
