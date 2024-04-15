# Cryptography Programming Project README

Daniel Mariscal

## Introduction
This Java program offers a customizable encryption tool for securing messages using a user-provided keyword. It facilitates the encryption process by converting letters to their corresponding two-digit representations and rearranging them based on the alphabetical order of the keyword.

## Code Structure and Documentation
The program comprises the following classes:

- MessageEncryptor: The main class responsible for executing the encryption process. It handles user input, generates matrices, rearranges matrix order, and prints the encrypted message.
- Each method within the MessageEncryptor class is documented extensively to clarify its purpose and functionality.

## How to Compile and Run the Program
- Ensure you have Java Development Kit (JDK) installed on your system.
- Download or clone the repository containing the Java files.
- Open your preferred Java development environment (e.g., IntelliJ IDEA, Eclipse).
- Create a new Java project or open an existing one.
- Add the Java file (MessageEncryptor.java) to your project.
- Make sure the file is included in the source package.
- Compile the program by selecting "Build" or "Clean and Build" in your IDE.
- After compilation, right-click on the project and select "Run" or use the corresponding shortcut to execute the program.
- The program will prompt you to enter a message and a keyword for encryption.
- Follow the on-screen instructions to complete the encryption process and view the encrypted message.
  
**Note: Ensure to provide valid input as per the program's requirements for seamless execution.**

## Code

```

import java.util.Scanner;
import java.util.Arrays;

public class MessageEncryptor {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Step 1: Input message
        System.out.println("Enter the message to be encrypted (without numbers or symbols):");
        String originalMessage = scanner.nextLine().replaceAll("[^a-zA-Z ]", "").toUpperCase().trim();

        // Step 2: Input keyword
        String keyword;
        do {
            System.out.println("Enter the keyword for encryption (no numbers, symbols, or duplicate characters):");
            keyword = scanner.nextLine().replaceAll("[^a-zA-Z]", "").toUpperCase();
            if (!isUnique(keyword)) {
                System.out.println("Keyword must not contain duplicate characters. Please enter a new keyword.");
            }
        } while (!isUnique(keyword));

        // Step 3: Generate matrices
        int[][] messageMatrix = generateMessageMatrix(originalMessage);
        int[][] keyMatrix = generateKeyMatrix(messageMatrix, keyword);
        int[][] reorderedMatrix = rearrangeMatrixOrder(keyMatrix, keyword);

        // Step 4: Print encrypted message
        System.out.println("\nMessage to Be Encrypted:");
        System.out.println(originalMessage);
        System.out.println("\nSequence from Letter Matrix:");
        printMatrix(messageMatrix);
        System.out.println("\nThe Keyword:");
        System.out.println(keyword);
        System.out.println("\nThe Keyword Matrix:");
        printMatrix(keyMatrix);
        System.out.println("\nThe Alphabetical Keyword:");
        char[] sortedKeyword = keyword.toCharArray();
        Arrays.sort(sortedKeyword);
        System.out.println(String.valueOf(sortedKeyword));
        System.out.println("\nThe Re-ordered Matrix:");
        printMatrix(reorderedMatrix);
        System.out.println("\nThe Fully Encrypted Message:");
        printEncryptedMessage(reorderedMatrix);
    }

    // Method to check if a string contains only unique characters
    private static boolean isUnique(String str) {
        return str.length() == str.chars().distinct().count();
    }

    // Method to generate the message matrix
    private static int[][] generateMessageMatrix(String message) {
        int[][] matrix = new int[message.length()][2];
        for (int i = 0; i < message.length(); i++) {
            char ch = message.charAt(i);
            matrix[i][0] = ch / 10 - 4; // Get the row number
            matrix[i][1] = ch % 10; // Get the column number
        }
        return matrix;
    }

    // Method to generate the key matrix
    private static int[][] generateKeyMatrix(int[][] messageMatrix, String keyword) {
        int rows = messageMatrix.length;
        int cols = keyword.length();
        int[][] matrix = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                matrix[i][j] = messageMatrix[i][j % 2];
            }
        }
        return matrix;
    }

    // Method to rearrange the matrix order based on alphabetical order of the keyword
    private static int[][] rearrangeMatrixOrder(int[][] keyMatrix, String keyword) {
        char[] sortedKeyword = keyword.toCharArray();
        Arrays.sort(sortedKeyword);
        int cols = keyword.length();
        int[][] matrix = new int[keyMatrix.length][cols];
        int[] order = new int[cols];
        for (int i = 0; i < cols; i++) {
            char ch = sortedKeyword[i];
            int index = keyword.indexOf(ch);
            order[i] = index;
        }
        for (int i = 0; i < cols; i++) {
            for (int j = 0; j < keyMatrix.length; j++) {
                matrix[j][i] = keyMatrix[j][order[i]];
            }
        }
        return matrix;
    }

    // Method to print a matrix
    private static void printMatrix(int[][] matrix) {
        for (int[] row : matrix) {
            for (int num : row) {
                System.out.print(num + " ");
            }
            System.out.println();
        }
    }

    // Method to print the encrypted message
    private static void printEncryptedMessage(int[][] matrix) {
        for (int j = 0; j < matrix[0].length; j++) {
            for (int[] row : matrix) {
                System.out.print(row[j]);
            }
            System.out.print(" ");
        }
        System.out.println();
    }
}

```
