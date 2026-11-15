TASK A2: DON'T READ WHAT YOU DON'T NEED 

A2.1 Predict the Cost 

The program reads a 50 MB attendance log using FileInputStream.read() one byte at a time.

Calculation:

- File size = 50 MB.

- 1 MB = 1,048,576 bytes.

- Total bytes:

\[

50\times 1,048,576=52,428,800

\]

Therefore:

- The program makes approximately 52,428,800 read() calls.

- Each call reads only one byte.

- Repeated method calls create additional overhead.

- The program becomes slow because it does not read multiple bytes in one operation.

A2.2 Redesign Using Buffering 

Use BufferedInputStream to read multiple bytes efficiently.How buffering improves performance:

- BufferedInputStream maintains an internal buffer.

- Instead of requesting one byte from the operating system each time, it reads a larger block of data.

- Subsequent reads are served from the buffer.

- This reduces expensive underlying I/O operations.

- The time complexity remains O(n), but the practical execution time is reduced.

A2.3 When Buffering Does Not Help 

- Buffering may provide little benefit when the program reads data only once and the data is already available in memory.

- It may also provide little improvement when processing a very small file.

- Additional buffering is unnecessary if the existing API already performs efficient buffered operations.

Alternative Concept: Files.readAllLines()Complexity Analysis:

- Buffered stream:

  - Time complexity: O(n).

  - Space complexity: O(b), where b is the buffer size.

  - Suitable for large files because only a small buffer is maintained.

- Files.readAllLines():

  - Time complexity: O(n).

  - Space complexity: O(n), because the entire file and its lines are stored in memory.

  - Suitable for small or moderate-sized files that comfortably fit in memory.

Conclusion:

For a 50 MB attendance file, buffered reading is generally preferable because it reduces memory usage while maintaining efficient sequential reading.

