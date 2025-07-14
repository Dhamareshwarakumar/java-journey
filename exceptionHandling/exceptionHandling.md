# Exception Handling

-   [Why Exception Handling?](#why-exception-handling)
-   [Types of Errors](#types-of-errors)
    -   [Compile-time Errors](#compile-time-errors)
    -   [Runtime Errors](#runtime-errors)
    -   [Logical Errors](#logical-errors)
-   [Errors Vs Exceptions](#errors-vs-exceptions)
-   [Checked Vs Unchecked Exceptions](#checked-vs-unchecked-exceptions)
-   [What is an Exception?](#what-is-an-exception)
-   [The Exception Hierarchy](#the-exception-hierarchy)
-   [Syntax](#syntax)
-   Try-with-Resources
    -   TODO
-   [Throwing Exceptions](#throwing-exceptions)
-   [Custom Exceptions](#custom-exceptions)

## Why Exception Handling?

-   A single error can crash the entire application, leaving users frustrated and data potentially corrupted.
-   Simply Exception Handling is a safety for preventing crashes and gracefully handling unexpected situations.

## Types of Errors

### **Compile-time Errors**

-   Errors that occur during the compilation of the code.
-   There are detected by the compiler before program execution.
-   Most of them are syntax errors
-   Examples
    -   Missing Semicolon
    -   Undeclared variables etc.,

### **Runtime Errors**

-   Errors that occur during the execution (after the program is successfully compiled)
-   Unhandled runtime errors will crash the system.
-   Unexpected issues that occur during runtime are categorised into
    -   Errors
    -   Exceptions.
-   Examples
    -   Division by Zero
    -   FileNotFoundException
    -   Array Index Out of Bounds etc.,

### **Logical Errors**

-   These are errors where the program runs without crashing but gives wrong output.
-   Example
    -   Incorrect algorithm or formula

**NOTE:** Exception handling is

## Errors Vs Exceptions

### Error

-   Error is a serious, unrecoverable condition that a program **should not** try to handle.
-   Usually these errors occur at system-level.
-   Examples
    -   OutOfMemoryError
    -   StackOverflowError
    -   VirtualMachineError etc.,

### Exceptions

-   Exceptions are recoverable issues that a programmer can decide to handle it or not.
    -   NullPointerException
    -   IOExceptions etc.,
-   (In Java) Exceptions are further classified into
    -   Checked Exceptions
    -   Unchecked Exceptions

## Checked Vs Unchecked Exceptions

### Checked Exceptions

-   Exceptions that are expected to handle are checked Exceptions
-   (In Java) Compiler forces you to handle the checked exceptions
-   Situation
    -   If you are trying to read a file which is not existing you would like to handle the situation and continue further with an appropriate message to requester instead of crashing the program.
    -   Similarly when calling an API but getting a timeout.
-   Examples
    -   IOException
    -   SQLException etc.,

### Unchecked Exceptions

-   These often indicate programming errors.
-   (In Java) Compiler does not force you to handle them
-   Situation
    -   If you are accessing 6th element of an array of length, it raises runtime error which we don’t want handle because this might be happening due to programming error or violating pre-conditions
    -   And we want to fix the error before compilation
-   Examples
    -   NullPointerException
    -   ArrayIndexOutOfBoundsException etc.,

## What is an Exception?

-   An exception is an error or unexpected condition that occurs during program execution.
-   When an exception occurs, Java created an object that contains
    -   **What happened:** The type of error (ex FileNotFoundException)
    -   **Where it happened:** The exact line of code and method
    -   **Additional Context:** A descriptive message about the issue

### Exception Lifecycle

1.  **Creation**: Java creates an exception object
2.  **Throwing**: The exception is "thrown" up the call stack
3.  **Searching**: Java looks for code that can handle this exception
4.  **Handling**: If found, the exception is caught and handled
5.  **Termination**: If not found, the program terminates

## The Exception Hierarchy

```
java.lang.Object
    └── java.lang.Throwable
        ├── java.lang.Error
        │   ├── OutOfMemoryError
        │   ├── StackOverflowError
        │   └── VirtualMachineError
        └── java.lang.Exception
            ├── RuntimeException (Unchecked)
            │   ├── NullPointerException
            │   ├── ArrayIndexOutOfBoundsException
            │   ├── IllegalArgumentException
            │   └── NumberFormatException
            └── Checked Exceptions
                ├── IOException
                ├── SQLException
                ├── ClassNotFoundException
                └── InterruptedException
```

## Syntax

```java
class Main {
    public static void main(String[] args) {
        int i = 0;
        int j = 0;

        try {
            i = i/j;
        } catch (ArithmeticExcpetion e) {
            System.out.println("Please enter non-zero positive integers for division");
        } catch (Exception e) {
            System.out.println("Something went wrong, Please try again later sometime!");
            System.out.println("ERROR: " + e);
        } finally {
            System.out.println("Executing finally block");
        }

        System.out.println("Op Result: " + i);
        System.out.println("End of program");
    }
}
```

-   The `try` block contains code that might throw an exception.
-   The `catch` block specifies what to do if a specific type of exception occurs.
-   The `finally` is an optional block, executes no matter what happens in the try block. It usually uses for cleanup operations.

**NOTE:** The exception object (often named `e`) contains information about what went wrong, including the error message and stack trace.

## Try-with-Resources: TODO

-   Java 7 introduced try-with-resources, which automatically handles resource cleanup for you.
-   This is particularly useful for file operations, database connections, and other resources that need explicit cleanup.

```java
public class TryWithResourcesExample {
    public void readFileModern(String filename) {
        // Resources declared here are automatically closed
        try (FileReader file = new FileReader(filename);
             BufferedReader reader = new BufferedReader(file)) {

            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }

        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
        // File and reader are automatically closed here
    }

    public void multipleResources() {
        try (FileInputStream input = new FileInputStream("input.txt");
             FileOutputStream output = new FileOutputStream("output.txt");
             BufferedReader reader = new BufferedReader(new InputStreamReader(input));
             PrintWriter writer = new PrintWriter(output)) {

            String line;
            while ((line = reader.readLine()) != null) {
                writer.println(line.toUpperCase());
            }

        } catch (IOException e) {
            System.out.println("File operation failed: " + e.getMessage());
        }
        // All resources are automatically closed in reverse order
    }
}
```

## Throwing Exceptions

-   The `throw` statement is used to manually throw an exception.

```java
class Main {
    public static void main(String[] args) {
        int i = 10;
        int j = 20;
        int temp = i;

        try {
            i = i/j;

            if (i == 0) {
                throw new ArithmeticException("Second input must be greater than or equal to first input");
            }
        } catch (ArithmeticException e) {
            System.out.println("Proceeding with default result...");
            i = temp;
        } catch (Exception e) {
            System.out.println("Something went wrong, Please try again later sometime!");
            System.out.println("ERROR: " + e);
        }

        System.out.println("Op Result: " + i);
        System.out.println("End of program");
    }
}
```

## Exception Propagation

-   Exception propagation is how exceptions travel up through the call stack when they're not handled at the current level.
-   `throws` keyword is used to propagate checked exceptions up higher the call stack since unhandled checked exceptions will throw error at compile time.

```java
public class CheckedDemo {
    public static void main(String[] args) {
        try {
            methodA(); // Must handle because methodA throws IOException
        } catch (IOException e) {
            System.out.println("Caught IO exception");
        }
    }

    // MUST include 'throws IOException' or compiler error
    public static void methodA() throws IOException {
        methodB(); // Compiler knows this might throw IOException
    }

    // MUST declare throws or handle internally
    public static void methodB() throws IOException {
        throw new IOException("File not found");
    }
}
```

## Custom Exceptions

```java
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

class Main {
    public static void main(String[] args) {
        int i = 10;
        int j = 20;
        int temp = i;

        try {
            i = i/j;

            if (i == 0) {
                throw new CustomException("Second input must be greater than or equal to first input");
            }
        } catch (CustomException e) {
            System.out.println("Proceeding with default result...");
            i = temp;
        } catch (Exception e) {
            System.out.println("Something went wrong, Please try again later sometime!");
            System.out.println("ERROR: " + e);
        }

        System.out.println("Op Result: " + i);
        System.out.println("End of program");
    }
}
```
