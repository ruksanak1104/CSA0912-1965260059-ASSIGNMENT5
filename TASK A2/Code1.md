import java.io.*;

public class BufferedCounter {

    public static void main(String[] args) {

        String file = "attendance.txt";

        int lines = 0;

        try (BufferedInputStream bis =
                new BufferedInputStream(
                    new FileInputStream(file))) {

            int data;

            while ((data = bis.read()) != -1) {

                if (data == '\n') {
                    lines++;
                }
            }

            System.out.println("Total Lines: " + lines);

        } catch (IOException e) {

            System.out.println(e.getMessage());
        }
    }
}
