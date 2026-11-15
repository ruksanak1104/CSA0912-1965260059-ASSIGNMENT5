import java.sql.*;

import java.util.*;

public class StudentDao implements Dao<Student, Integer> {

    private Connection con;

    public StudentDao(Connection con) {
        this.con = con;
    }

    @Override
    public void insert(Student s) {

        String sql =
            "INSERT INTO student VALUES (?, ?, ?)";

        try (PreparedStatement ps =
                 con.prepareStatement(sql)) {

            ps.setInt(1, s.getStudentId());
            ps.setString(2, s.getName());
            ps.setString(3, s.getEmail());

            ps.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    @Override
    public Student findById(Integer id) {

        String sql =
            "SELECT * FROM student WHERE student_id=?";

        try (PreparedStatement ps =
                 con.prepareStatement(sql)) {

            ps.setInt(1, id);

            ResultSet rs = ps.executeQuery();

            if (rs.next()) {

                return new Student(
                    rs.getInt("student_id"),
                    rs.getString("name"),
                    rs.getString("email")
                );
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

        return null;
    }

    @Override
    public List<Student> findAll() {

        List<Student> students = new ArrayList<>();

        String sql = "SELECT * FROM student";

        try (Statement st = con.createStatement();
             ResultSet rs = st.executeQuery(sql)) {

            while (rs.next()) {

                students.add(new Student(
                    rs.getInt("student_id"),
                    rs.getString("name"),
                    rs.getString("email")
                ));
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

        return students;
    }

    @Override
    public void update(Student s) {

        String sql =
            "UPDATE student SET name=?, email=? WHERE student_id=?";

        try (PreparedStatement ps =
                 con.prepareStatement(sql)) {

            ps.setString(1, s.getName());
            ps.setString(2, s.getEmail());
            ps.setInt(3, s.getStudentId());

            ps.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void delete(Integer id) {

        String sql =
            "DELETE FROM student WHERE student_id=?";

        try (PreparedStatement ps =
                 con.prepareStatement(sql)) {

            ps.setInt(1, id);
            ps.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
