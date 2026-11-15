import java.sql.*;

public class SafeLogin {

    public static void main(String[] args) {

        String url =
            "jdbc:mysql://localhost:3306/campus_events";

        String user = "root";
        String password = "your_password";

        String sql =
            "SELECT * FROM users WHERE id=? AND pwd=?";

        try (Connection con =
                 DriverManager.getConnection(
                     url, user, password);

             PreparedStatement ps =
                 con.prepareStatement(sql)) {

            ps.setString(1, "' OR '1'='1' --");
            ps.setString(2, "anything");

            ResultSet rs = ps.executeQuery();

            if (rs.next()) {
                System.out.println("Login Successful");
            } else {
                System.out.println("Invalid Login");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
