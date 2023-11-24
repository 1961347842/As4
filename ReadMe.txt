https://youtu.be/Bis5_74Y7nc
Step 1:
Install PostgreSQL if you haven't already. You can download it.

Step 2:
Open pgAdmin, the management tool for PostgreSQL.

Step 3:
Create a new database called AS4.

Right-click on Databases → Create → Database…
Enter AS4 in the Database field.
Step 4:
Create a table called students.

Open a SQL Query tool and run:
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name TEXT NOT NULL,
    last_name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE,
    enrollment_date DATE
);




Java application using JDBC:
Step1:Set up a new Java project in your preferred IDE
Step2:Download the PostgreSQL JDBC driver from here.
	Add the .jar file to your project as an external library.
Step 3:
package JDBC.com;
import java.sql.*;
import java.util.Scanner;

public class Assignment4 {
    String url = "jdbc:postgresql://localhost:5432/<databaseName>" ;
    String user = "usename" ;//usename
    String password = "password" ;//password
    public static void main(String[] args) {//
        Assignment4 dbOps = new Assignment4();
        Scanner scanner = new Scanner(System.in);

        // Add a user
        System.out.println("Would you like to add a student? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            Date d =Date.valueOf("2023-01-15");
            dbOps.addStudent("John Doe", "john@example.com","john.doe@example.com",d);
        }

        // Modify user's email
        System.out.println("Would you like to modify a student's email? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            dbOps.updateStudentemail(1, "john.doe@example.com");
        }

        // Delete user
        System.out.println("Would you like to delete a student? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            dbOps.deleteStudent(1);
        }

        //Get all
        System.out.println("Would you like to get all? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            dbOps.getAllStudents();
        }
        scanner.close();
    }


    public void getAllStudents(){

        String SQL = "SELECT * FROM students";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL);ResultSet resultSet=pstmt.executeQuery();) {
            while (resultSet.next()) {
                // Assuming the students table has columns: id, first_name, last_name, email, enrollment_date
                int id = resultSet.getInt("student_id");
                String firstName = resultSet.getString("first_name");
                String lastName = resultSet.getString("last_name");
                String email = resultSet.getString("email");
                Date enrollmentDate = resultSet.getDate("enrollment_date");

                // Output the student record
                System.out.println(id + ", " + firstName + ", " + lastName + ", " + email + ", " + enrollmentDate);
            }

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }
    public void addStudent(String first_name, String last_name, String email, Date enrollment_date){
        String sql = "INSERT INTO students (first_name, last_name, email, enrollment_date) VALUES (?, ?, ?, ?)";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {

            pstmt.setString(1, first_name);
            pstmt.setString(2, last_name);
            pstmt.setString(3, email);
            pstmt.setDate(4,enrollment_date);
            pstmt.executeUpdate();
            System.out.println("User added successfully!");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }
    public void updateStudentemail(int student_id,String newEmail){
        String SQL = "UPDATE Students SET email=? WHERE student_id=?";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setString(1, newEmail);
            pstmt.setInt(2, student_id);

            pstmt.executeUpdate();
            System.out.println("User email updated!");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }
    public void deleteStudent(int student_id){
        String SQL = "DELETE FROM Students WHERE student_id=?";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setInt(1, student_id);
            pstmt.executeUpdate();
            System.out.println("User deleted!");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }

}

Replace YOUR_USERNAME and YOUR_PASSWORD with your PostgreSQL username and password.
Step 4:
Run the Java program. If all goes well, it will add a user, update the user's email, and then delete the user.