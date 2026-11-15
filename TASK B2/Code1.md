
import java.sql.*;

public class UnsafeLogin {

    public static void main(String[] args) {

        String url =
            "jdbc:mysql://localhost:3306/campus_events";

        String user = "root";
        String password = "your_password";

        String id = "' OR '1'='1' -- ";
        String pwd = "wrongpassword";

        String sql =
            "SELECT * FROM users WHERE id='" +
            id + "' AND pwd='" + pwd + "'";

        try (Connection con =
                DriverManager.getConnection(
                    url, user, password);

             Statement st = con.createStatement()) {

            ResultSet rs = st.executeQuery(sql);

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

