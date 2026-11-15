
import java.io.*;

public class ValidatePasses {

    public static void main(String[] args) {

        try (BufferedReader br =
                new BufferedReader(
                    new FileReader("damaged.txt"))) {

            String line;
            int lineNumber = 0;

            while ((line = br.readLine()) != null) {

                lineNumber++;

                if (line.trim().isEmpty()) {
                    System.out.println(
                        "Line " + lineNumber +
                        ": Empty line skipped");
                    continue;
                }

                String[] fields = line.split(",", -1);

                if (fields.length != 5) {
                    System.out.println(
                        "Line " + lineNumber +
                        ": Invalid field count");
                    continue;
                }

                try {

                    Integer.parseInt(fields[3]);
                    Double.parseDouble(fields[4]);

                    System.out.println(
                        "Line " + lineNumber +
                        ": Valid record");

                } catch (NumberFormatException e) {

                    System.out.println(
                        "Line " + lineNumber +
                        ": Invalid numeric value");
                }
            }

        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
    }
}

