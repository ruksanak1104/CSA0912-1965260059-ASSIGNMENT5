import java.sql.*;

public class MetadataCheck {

    public static void main(String[] args) {

        String url =
            "jdbc:mysql://localhost:3306/transport_db";

        try (Connection con =
                DriverManager.getConnection(
                    url, "root", "your_password");

             Statement st = con.createStatement();

             ResultSet rs =
                st.executeQuery("SELECT * FROM bus_pass")) {

            ResultSetMetaData md =
                rs.getMetaData();

            int count = md.getColumnCount();

            for (int i = 1; i <= count; i++) {

                System.out.println(
                    "Column: " +
                    md.getColumnName(i));

                System.out.println(
                    "Type: " +
                    md.getColumnTypeName(i));

                System.out.println("----------------");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
