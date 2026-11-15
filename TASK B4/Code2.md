
import java.sql.*;

public class DatabaseInfo {

    public static void main(String[] args) {

        String url =
            "jdbc:mysql://localhost:3306/transport_db";

        try (Connection con =
                DriverManager.getConnection(
                    url, "root", "your_password")) {

            DatabaseMetaData md =
                con.getMetaData();

            System.out.println(
                "Database Product: " +
                md.getDatabaseProductName());

            System.out.println(
                "Database Version: " +
                md.getDatabaseProductVersion());

            System.out.println(
                "Driver Name: " +
                md.getDriverName());

            System.out.println(
                "Driver Version: " +
                md.getDriverVersion());

            ResultSet rs =
                md.getTables(
                    null,
                    "transport_db",
                    "%",
                    new String[]{"TABLE"});

            System.out.println("Tables:");

            while (rs.next()) {

                System.out.println(
                    rs.getString("TABLE_NAME"));
            }

            rs.close();

        } catch (SQLException e) {

            e.printStackTrace();
        }
    }
}

