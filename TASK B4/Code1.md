
import java.io.*;

import java.sql.*;

import java.util.*;

public class PassMigration {

    public static void main(String[] args) {

        String url =
            "jdbc:mysql://localhost:3306/transport_db";

        String inputFile = "buspasses.txt";

        List<String[]> valid = new ArrayList<>();

        int failed = 0;

        // Read and validate file

        try (BufferedReader br =
                new BufferedReader(
                    new FileReader(inputFile));

             BufferedWriter error =
                new BufferedWriter(
                    new FileWriter("failed_records.txt"))) {

            String line;

            while ((line = br.readLine()) != null) {

                if (line.trim().isEmpty()) {
                    failed++;
                    error.write(
                        "Empty record: " + line);
                    error.newLine();
                    continue;
                }

                String[] p = line.split(",", -1);

                try {

                    if (p.length != 5) {
                        throw new IllegalArgumentException(
                            "Invalid field count");
                    }

                    Integer.parseInt(p[3]);
                    Double.parseDouble(p[4]);

                    valid.add(p);

                } catch (Exception e) {

                    failed++;

                    error.write(
                        line + " : " + e.getMessage());

                    error.newLine();
                }
            }

        } catch (IOException e) {

            System.out.println(e.getMessage());
            return;
        }

        // Load valid records into database

        try (Connection con =
                DriverManager.getConnection(
                    url, "root", "your_password")) {

            int loaded =
                PassLoader.loadAll(con, valid);

            System.out.println(
                "Successful Records: " + loaded);

            System.out.println(
                "Failed Records: " + failed);

            System.out.println(
                "Migration Completed");

        } catch (Exception e) {

            System.out.println(
                "Migration Failed: " + e.getMessage());
        }
    }
}

