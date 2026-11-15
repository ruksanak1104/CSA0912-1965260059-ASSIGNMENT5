import java.nio.file.*;

import java.io.IOException;

import java.util.List;

public class ReadAllLinesExample {

    public static void main(String[] args) {

        try {

            List<String> lines =
                Files.readAllLines(
                    Paths.get("attendance.txt"));

            System.out.println(
                "Total Lines: " + lines.size());

        } catch (IOException e) {

            System.out.println(e.getMessage());
        }
    }
}
