---
title: "Understanding and Using 'Try-With-Resources' in Java"
datePublished: Tue Apr 23 2024 10:30:39 GMT+0000 (Coordinated Universal Time)
cuid: clvc8wcu9000109ky9rky5tlk
slug: understanding-and-using-try-with-resources-in-java
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713862766587/dd0687ec-5945-4d3e-90ef-71acd63dbb30.png
tags: java, code-quality, testautomation, exceptionhandling

---

### Introduction

*"Are you tired of dealing with messy*`finally`*blocks just to ensure your resources get closed properly?"*

*"Have you ever found yourself tangled in a web of nested*`try-catch`*blocks, only to realize that your application is still prone to resource leaks?"*

If so, you're not alone. Managing resources effectively is a common challenge for many Java programmers, including a surprising number of automation engineers I've interviewed. Despite being introduced in Java 7, the `try-with-resources` statement, a powerful feature that can greatly simplify resource management, remains underutilized in many areas, including test automation. Many engineers still continue to use the traditional approach, missing out on the benefits of safer, more readable code.

In this blog post, we will delve into the `try-with-resources` statement, exploring how it works and how we can use it to improve the Java code. So, let's get started and say goodbye to those messy `finally` blocks!

### Understanding Try-With-Resources

`try-with-resources` is a try statement that declares one or more resource declarations. An object that needs to be closed once the application has finished using it is called a resource. Files, network connections, and database connections are a few types of resources. ***The resources declared need to implement the***[***Closeable***](https://docs.oracle.com/javase/8/docs/api/java/io/Closeable.html)***or***[***AutoCloseable***](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html)***interfaces.***

An expression that uses `try-with-resources` declares the resource within the try statement. Upon completion of the `try` block, the resource is closed automatically. Compared to the conventional method, which required manually closing the resource in a `finally` block, this is a huge improvement.

**Benefits:**

The 'try-with-resources' statement has several benefits:

1. **Simpler Code:** We no longer need to write explicit code to close the resource in a `finally` block. This makes the code shorter and easier to read.
    
2. **Better Resource Management:** The `try-with-resources` statement ensures that the resource is closed promptly after the program is done using it. This can help prevent resource leaks that can cause the program to behave unpredictably.
    
3. **Improved Exception Handling:** If an exception is thrown both in the try block and when the resource is closed, the `try-with-resources` statement will suppress the exception thrown from the `try` block. This makes the exception-handling code more straightforward.
    

`All the examples discussed here can be found on` [`GitHub`](https://github.com/rakesh-vardan/java-examples/tree/main/src/main/java/io/learning/try_with_resources)

**Example:**

Let us understand the usage with an example. To read a file using `Scanner` class below is the code snippet we write using the traditional `try-catch-finally` syntax.

```java
    public void methodWithTryCatchFinally() {
        Scanner scanner = null;
        try {
            scanner = new Scanner(new File("input.txt"));
            while (scanner.hasNextLine()) {
                System.out.println(scanner.nextLine());
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (scanner != null) {
                scanner.close();
            }
        }
    }
```

This method reads and prints the contents of a file named "input.txt". It uses a `Scanner` object to read the file line by line. The `try` block attempts to open the file and read its contents. If the file is not found, a `FileNotFoundException` is caught and its stack trace is printed. ***Regardless of whether an exception is thrown, the***`finally`***block ensures that the***`Scanner`***object is closed to prevent resource leaks.***

Let us implement the same logic using `try-with-resources` syntax

```java
    public void methodWithTryWithResources() {
        try (Scanner scanner = new Scanner(new File("input.txt"))) {
            while (scanner.hasNextLine()) {
                System.out.println(scanner.nextLine());
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
```

Here the `try` block, with the `Scanner` declaration within its parentheses is the `try-with-resources` section. Using this syntax automatically closes the resources declared within the parentheses when the try block is exited, either normally or via an exception. ***This ensures that the***`Scanner`***object is closed to prevent resource leaks, without needing an explicit***`finally`***block*.** If the file is not found, a `FileNotFoundException` is caught and its stack trace is printed.

### Working with Multiple Files:

We can also use this syntax to declare and initialize multiple resources with `try-with-resources`

```java
public void methodWithTryCatchFinally() {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream("input.txt");
            fos = new FileOutputStream("output.txt");
            int data;
            while ((data = fis.read()) != -1) {
                fos.write(data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

This example is also similar to the previous one, but now we work with 2 files in the try-with-resources section. The code reads data from a file named "input.txt" and writes it to another file named "output.txt". It uses `FileInputStream` to read the input file and `FileOutputStream` to write to the output file. The `try` block attempts to open both files, read data from the input file, and write it to the output file. If an `IOException` occurs during this process (for example, if one of the files does not exist or cannot be opened), the exception is caught and its stack trace is printed. ***The***`finally`***block ensures that both the***`FileInputStream`***and***`FileOutputStream`***are closed, regardless of whether an exception occurred. This is important to prevent resource leaks***. If an `IOException` occurs while trying to close the files, it is also caught and its stack trace is printed.

Now, let us change it to a new syntax.

```java
public void methodWithTryWithResources() {
        try (FileInputStream fis = new FileInputStream("input.txt");
             FileOutputStream fos = new FileOutputStream("output.txt")) {
            int data;
            while ((data = fis.read()) != -1) {
                fos.write(data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

As you see the same logic has been written concisely using `try-with-resources`. It improves the readability of the program and reduces the complexity & redundant code for resource clean-up. ***The***`try-with-resources`***automatically closes the***`FileInputStream`***and***`FileOutputStream`***resources after use, preventing resource leaks.*** If an `IOException` occurs (like a file not found), it's caught and its stack trace is printed.

### Working with Database Connections:

One more real-time example in test automation is, connecting to the DB and fetching the test data required for our test scripts. Here is how we write the code for this using the traditional approach.

```java
    public void methodWithTryCatchFinally() {
        Connection conn = null;
        Statement stmt = null;
        try {
            conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/postgres",
                    "postgres", "password");
            stmt = conn.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM users");
            while (rs.next()) {
                // process the row
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

Here we are using native classes from `java.sql` JDBC classes to connect to a PostgreSQL database, execute a SQL query, and process the results. It uses a `Connection` object to establish a connection to the database and a `Statement` object to execute the query. The `try` block attempts to establish the connection, execute the query "SELECT \* FROM users", and process each row of the result. If a `SQLException` occurs during this process (for example, if the database connection fails or the query is invalid), the exception is caught and its stack trace is printed. ***The***`finally`***block ensures that both the***`Statement`***and***`Connection`***objects are closed, regardless of whether an exception occurred.If a***`SQLException`***occurs while trying to close the objects, it is also caught and its stack trace is printed.***

Let us convert this logic to use the new syntax.

```java
    public void methodWithTryWithResources() {
        try (Connection conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/postgres",
                "postgres", "password");
             Statement stmt = conn.createStatement()) {
            ResultSet rs = stmt.executeQuery("SELECT * FROM users");
            while (rs.next()) {
                // process the row
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
```

Here we are doing the same logic but using a `try-with-resources` block to automatically manage the `Connection` and `Statement` resources. ***The***`try-with-resources`***block automatically closes the***`Connection`***and***`Statement`***objects after use, preventing resource leaks.***

`A try-with-resources block can still have the finally block, which will work in the similar way as with a traditional try block.`

### Conclusion

In this blog, we discussed how we can leverage `try-with-resources` instead of using `trycatch` and `finally` blocks for exception handling and writing efficient code. If you're not already using `try-with-resources` in your Java code, consider starting today!