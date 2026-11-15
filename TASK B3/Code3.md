
import java.sql.*;

import java.util.*;

public class PassLoaderTest {

    public static void main(String[] args) {

        String url =
            "jdbc:mysql://localhost:3306/transport_db";

        try (Connection con =
                DriverManager.getConnection(
                    url, "root", "your_password")) {

            List<String[]> passes =
                new ArrayList<>();

            for (int i = 1; i <= 10000; i++) {

                passes.add(new String[]{
                    "P" + i,
                    "REG" + i,
                    "Student" + i,
                    "12",
                    "4500.0"
                });
            }

            int count =
                PassLoader.loadAll(con, passes);

            System.out.println(
                "Total Records Loaded: " + count);

            System.out.println(
                "Batch Insertion Completed");

        } catch (Exception e) {

            System.out.println(
                "Insertion Failed: " + e.getMessage());
        }
    }
}

