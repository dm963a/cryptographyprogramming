# Cryptography Programming Project README

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

## Sample Data & Output Screenshots

Enter the message to be encrypted (without numbers or symbols):
hello, my #1friend

Enter the keyword for encryption (no numbers, symbols, or duplicate characters):
security

Message to Be Encrypted:
HELLO MY FRIEND

Sequence from Letter Matrix:
3 2 
2 9 
3 6 
3 6 
3 9 
-1 2 
3 7 
4 9 
-1 2 
3 0 
4 2 
3 3 
2 9 
3 8 
2 8 

The Keyword:
SECURITY

The Keyword Matrix:
3 2 3 2 3 2 3 2 
2 9 2 9 2 9 2 9 
3 6 3 6 3 6 3 6 
3 6 3 6 3 6 3 6 
3 9 3 9 3 9 3 9 
-1 2 -1 2 -1 2 -1 2 
3 7 3 7 3 7 3 7 
4 9 4 9 4 9 4 9 
-1 2 -1 2 -1 2 -1 2 
3 0 3 0 3 0 3 0 
4 2 4 2 4 2 4 2 
3 3 3 3 3 3 3 3 
2 9 2 9 2 9 2 9 
3 8 3 8 3 8 3 8 
2 8 2 8 2 8 2 8 

The Alphabetical Keyword:
CEIRSTUY

The Re-ordered Matrix:
3 2 2 3 3 3 2 2 
2 9 9 2 2 2 9 9 
3 6 6 3 3 3 6 6 
3 6 6 3 3 3 6 6 
3 9 9 3 3 3 9 9 
-1 2 2 -1 -1 -1 2 2 
3 7 7 3 3 3 7 7 
4 9 9 4 4 4 9 9 
-1 2 2 -1 -1 -1 2 2 
3 0 0 3 3 3 0 0 
4 2 2 4 4 4 2 2 
3 3 3 3 3 3 3 3 
2 9 9 2 2 2 9 9 
3 8 8 3 3 3 8 8 
2 8 8 2 2 2 8 8 

The Fully Encrypted Message:
32333-134-1343232 296692792023988 296692792023988 32333-134-1343232 32333-134-1343232 32333-134-1343232 296692792023988 296692792023988

## Screenshot 1:

![Output Screenshot 1](https://github.com/dm963a/cryptographyprogramming/assets/159193565/e7b3a2dc-5b55-4d07-a6c3-eced94691a6c)

## Screenshot 2:

![Output Screenshot 2](https://github.com/dm963a/cryptographyprogramming/assets/159193565/65a94de7-c26a-4983-8f02-810eed28203d)

## Screenshot 3:

![Output Screenshot 3](https://github.com/dm963a/cryptographyprogramming/assets/159193565/e75dcb60-3f46-4bb9-ae1c-af021863e04c)


