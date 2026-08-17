# JDBC Notes for Java Freshers

## Table of Contents

- [1. What Is JDBC?](#1-what-is-jdbc)
- [2. Why Is JDBC Used?](#2-why-is-jdbc-used)
- [3. JDBC Architecture](#3-jdbc-architecture)
- [4. JDBC Driver Types](#4-jdbc-driver-types)
- [5. Important JDBC Interfaces and Classes](#5-important-jdbc-interfaces-and-classes)
- [6. Basic Steps in JDBC](#6-basic-steps-in-jdbc)
- [7. Database Connection Example](#7-database-connection-example)
- [8. Statement](#8-statement)
- [9. PreparedStatement](#9-preparedstatement)
- [10. Common PreparedStatement Methods](#10-common-preparedstatement-methods)
- [11. Statement Execution Methods](#11-statement-execution-methods)
- [12. ResultSet](#12-resultset)
- [13. CRUD Operations](#13-crud-operations)
- [14. Generated Keys](#14-generated-keys)
- [15. Transaction Management](#15-transaction-management)
- [16. Batch Processing](#16-batch-processing)
- [17. CallableStatement](#17-callablestatement)
- [18. Exception Handling](#18-exception-handling)
- [19. Try-With-Resources](#19-try-with-resources)
- [20. DriverManager vs DataSource](#20-drivermanager-vs-datasource)
- [21. JDBC Best Practices](#21-jdbc-best-practices)
- [22. Common JDBC Errors](#22-common-jdbc-errors)
- [23. Frequently Asked Interview Questions](#23-frequently-asked-interview-questions)
- [24. Quick Revision](#24-quick-revision)

## 1. What Is JDBC?

**JDBC** stands for **Java Database Connectivity**.

It is a standard Java API used to connect Java applications with relational databases such as:

- MySQL
- Oracle
- PostgreSQL
- Microsoft SQL Server
- MariaDB

JDBC APIs are mainly available in the `java.sql` and `javax.sql` packages.

## 2. Why Is JDBC Used?

JDBC allows a Java application to:

- Establish a database connection
- Execute SQL statements
- Insert, update, delete, and retrieve records
- Execute stored procedures
- Manage transactions
- Process database errors

## 3. JDBC Architecture

```text
Java Application
       |
       v
    JDBC API
       |
       v
  JDBC Driver
       |
       v
    Database
```

The JDBC API provides standard interfaces, while the database-specific JDBC driver translates JDBC calls into commands understood by the database.

## 4. JDBC Driver Types

### Type 1: JDBC–ODBC Bridge Driver

Converts JDBC calls into ODBC calls.

- Requires an ODBC driver
- Platform-dependent
- Removed from Java 8

### Type 2: Native API Driver

Uses database-specific native libraries.

- Faster than Type 1
- Platform-dependent
- Requires native installation

### Type 3: Network Protocol Driver

Sends JDBC requests to middleware, which communicates with the database.

- Does not require native libraries
- Requires a middleware server

### Type 4: Thin Driver

Directly converts JDBC calls into the database protocol.

- Pure Java implementation
- Platform-independent
- Commonly used in modern applications

MySQL Connector/J is a Type 4 JDBC driver.

## 5. Important JDBC Interfaces and Classes

| Component | Purpose |
| --- | --- |
| `DriverManager` | Establishes database connections |
| `DataSource` | Provides connections, usually through a connection pool |
| `Connection` | Represents an active database connection |
| `Statement` | Executes simple static SQL statements |
| `PreparedStatement` | Executes parameterized SQL statements |
| `CallableStatement` | Executes stored procedures |
| `ResultSet` | Stores data returned by a query |
| `SQLException` | Represents database-related errors |
| `DatabaseMetaData` | Provides database information |
| `ResultSetMetaData` | Provides information about query-result columns |

## 6. Basic Steps in JDBC

A typical JDBC program performs the following operations:

1. Add the JDBC driver dependency.
2. Establish a connection.
3. Create a statement.
4. Execute an SQL command.
5. Process the result.
6. Close JDBC resources.

## 7. Database Connection Example

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnectionExample {

    private static final String URL =
            "jdbc:mysql://localhost:3306/training_db";

    private static final String USERNAME = "root";
    private static final String PASSWORD = "root";

    public static void main(String[] args) {

        try (Connection connection =
                     DriverManager.getConnection(
                             URL, USERNAME, PASSWORD)) {

            System.out.println("Database connected successfully.");

        } catch (SQLException exception) {
            exception.printStackTrace();
        }
    }
}
```

### Connection URL Format

```text
jdbc:mysql://hostname:port/database_name
```

Example:

```text
jdbc:mysql://localhost:3306/training_db
```

### Driver Loading

Older JDBC programs explicitly loaded the driver:

```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

This is normally unnecessary with JDBC 4.0 and later because suitable drivers are automatically discovered.

## 8. Statement

A `Statement` executes ordinary SQL commands.

```java
String sql = "SELECT employee_id, employee_name FROM employee";

try (Statement statement = connection.createStatement();
     ResultSet resultSet = statement.executeQuery(sql)) {

    while (resultSet.next()) {
        int id = resultSet.getInt("employee_id");
        String name = resultSet.getString("employee_name");

        System.out.println(id + " - " + name);
    }
}
```

Avoid using `Statement` to combine untrusted input with SQL.

```java
// Unsafe
String sql = "SELECT * FROM users WHERE username = '"
        + username + "'";
```

This can introduce an SQL injection vulnerability.

## 9. PreparedStatement

A `PreparedStatement` represents a precompiled, parameterized SQL statement.

```java
String sql = """
        INSERT INTO employee
        (employee_name, salary)
        VALUES (?, ?)
        """;

try (PreparedStatement statement =
             connection.prepareStatement(sql)) {

    statement.setString(1, "Anita Sharma");
    statement.setBigDecimal(
            2, new BigDecimal("45000.00"));

    int rowsAffected = statement.executeUpdate();

    System.out.println(rowsAffected + " record inserted.");
}
```

The first placeholder has index `1`, not `0`.

### Advantages

- Helps prevent SQL injection
- Supports parameterized queries
- Improves readability
- Handles Java-to-SQL value conversion
- May improve performance when executed repeatedly

## 10. Common PreparedStatement Methods

| Method | SQL type |
| --- | --- |
| `setInt()` | Integer value |
| `setLong()` | Long value |
| `setString()` | Character value |
| `setBigDecimal()` | Decimal value |
| `setBoolean()` | Boolean value |
| `setDate()` | SQL date |
| `setTimestamp()` | SQL date and time |
| `setObject()` | General value |
| `setNull()` | SQL `NULL` |

With modern JDBC drivers, Java time objects can often be passed using `setObject()`:

```java
statement.setObject(1, LocalDate.now());
statement.setObject(2, LocalDateTime.now());
```

## 11. Statement Execution Methods

| Method | Purpose | Return type |
| --- | --- | --- |
| `executeQuery()` | Executes `SELECT` | `ResultSet` |
| `executeUpdate()` | Executes `INSERT`, `UPDATE`, or `DELETE` | `int` |
| `execute()` | Executes an unknown or mixed SQL command | `boolean` |
| `executeBatch()` | Executes a collection of commands | `int[]` |

Example:

```java
int affectedRows = statement.executeUpdate();
```

The returned integer normally indicates the number of affected rows.

## 12. ResultSet

A `ResultSet` stores records returned by a `SELECT` query.

Initially, its cursor is positioned before the first record.

```java
while (resultSet.next()) {
    int id = resultSet.getInt("employee_id");
    String name = resultSet.getString("employee_name");
}
```

The `next()` method:

- Moves the cursor to the next row
- Returns `true` when a row exists
- Returns `false` after the final row

Values can be accessed by column name:

```java
resultSet.getString("employee_name");
```

They can also be accessed by column position:

```java
resultSet.getString(2);
```

Column names are generally easier to understand and maintain.

## 13. CRUD Operations

CRUD stands for:

| Operation | SQL command |
| --- | --- |
| Create | `INSERT` |
| Read | `SELECT` |
| Update | `UPDATE` |
| Delete | `DELETE` |

### Insert

```java
String sql = """
        INSERT INTO employee
        (employee_name, salary)
        VALUES (?, ?)
        """;
```

### Select

```java
String sql = """
        SELECT employee_id, employee_name, salary
        FROM employee
        WHERE employee_id = ?
        """;
```

### Update

```java
String sql = """
        UPDATE employee
        SET salary = ?
        WHERE employee_id = ?
        """;
```

### Delete

```java
String sql = """
        DELETE FROM employee
        WHERE employee_id = ?
        """;
```

## 14. Generated Keys

Generated primary-key values can be retrieved after an insertion.

```java
String sql = """
        INSERT INTO employee
        (employee_name, salary)
        VALUES (?, ?)
        """;

try (PreparedStatement statement =
             connection.prepareStatement(
                     sql, Statement.RETURN_GENERATED_KEYS)) {

    statement.setString(1, "Rahul Verma");
    statement.setBigDecimal(
            2, new BigDecimal("50000.00"));

    statement.executeUpdate();

    try (ResultSet keys = statement.getGeneratedKeys()) {
        if (keys.next()) {
            long employeeId = keys.getLong(1);
            System.out.println("Generated ID: " + employeeId);
        }
    }
}
```

## 15. Transaction Management

A transaction is a group of database operations treated as one logical unit.

A transaction should either:

- Complete all operations successfully, or
- Undo all operations when any operation fails

JDBC connections normally use auto-commit mode by default.

```java
connection.setAutoCommit(false);
```

### Transaction Example

```java
try {
    connection.setAutoCommit(false);

    debitStatement.executeUpdate();
    creditStatement.executeUpdate();

    connection.commit();

} catch (SQLException exception) {

    connection.rollback();
    throw exception;

} finally {
    connection.setAutoCommit(true);
}
```

### Important Methods

| Method | Purpose |
| --- | --- |
| `setAutoCommit(false)` | Begins manual transaction management |
| `commit()` | Permanently saves changes |
| `rollback()` | Cancels changes made in the transaction |
| `setSavepoint()` | Creates an intermediate rollback point |

## 16. Batch Processing

Batch processing sends multiple SQL operations together.

```java
String sql = """
        INSERT INTO employee
        (employee_name, salary)
        VALUES (?, ?)
        """;

try (PreparedStatement statement =
             connection.prepareStatement(sql)) {

    statement.setString(1, "Amit");
    statement.setBigDecimal(
            2, new BigDecimal("40000.00"));
    statement.addBatch();

    statement.setString(1, "Neha");
    statement.setBigDecimal(
            2, new BigDecimal("42000.00"));
    statement.addBatch();

    int[] results = statement.executeBatch();

    System.out.println(results.length + " operations executed.");
}
```

Batch processing is useful for bulk insert, update, and delete operations.

## 17. CallableStatement

A `CallableStatement` executes stored procedures.

```java
String sql = "{call find_employee_by_id(?)}";

try (CallableStatement statement =
             connection.prepareCall(sql)) {

    statement.setInt(1, 101);

    try (ResultSet resultSet = statement.executeQuery()) {
        while (resultSet.next()) {
            System.out.println(
                    resultSet.getString("employee_name"));
        }
    }
}
```

## 18. Exception Handling

JDBC operations can throw `SQLException`.

```java
catch (SQLException exception) {
    System.out.println("Message: "
            + exception.getMessage());

    System.out.println("SQL state: "
            + exception.getSQLState());

    System.out.println("Error code: "
            + exception.getErrorCode());
}
```

| Method | Information |
| --- | --- |
| `getMessage()` | Human-readable error description |
| `getSQLState()` | Standard SQL state code |
| `getErrorCode()` | Vendor-specific error code |
| `getNextException()` | Next exception in the exception chain |

## 19. Try-With-Resources

Try-with-resources automatically closes JDBC resources.

```java
try (Connection connection =
             DriverManager.getConnection(url, username, password);

     PreparedStatement statement =
             connection.prepareStatement(sql);

     ResultSet resultSet =
             statement.executeQuery()) {

    while (resultSet.next()) {
        System.out.println(resultSet.getString(1));
    }
}
```

Resources are normally closed in reverse order:

1. `ResultSet`
2. `Statement`
3. `Connection`

## 20. DriverManager vs DataSource

| `DriverManager` | `DataSource` |
| --- | --- |
| Suitable for basic learning | Preferred in enterprise applications |
| Creates connections directly | Can support connection pooling |
| Configuration is usually written in code | Configuration can be externalized |
| Limited enterprise features | Can support transactions and managed environments |

Spring Boot applications usually obtain connections through a configured `DataSource`.

## 21. JDBC Best Practices

- Use `PreparedStatement` instead of concatenating input into SQL.
- Use try-with-resources.
- Use transactions for related write operations.
- Roll back the transaction when an operation fails.
- Use batch processing for bulk operations.
- Avoid storing database credentials directly in source code.
- Use a connection pool in production applications.
- Retrieve only required columns.
- Use pagination for large result sets.
- Match Java data types with appropriate SQL types.
- Log errors without exposing passwords or sensitive information.
- Keep SQL and database code separate from presentation logic.

## 22. Common JDBC Errors

| Problem | Possible reason |
| --- | --- |
| `No suitable driver` | Driver dependency is missing or URL is incorrect |
| `Access denied` | Invalid username, password, or permissions |
| `Unknown database` | Database name is incorrect |
| `Communications link failure` | Server is stopped, unreachable, or using another port |
| Parameter index out of range | Incorrect `?` placeholder index |
| ResultSet is closed | Statement or connection was closed too early |
| Data truncation | Java value is larger than the SQL column permits |

## 23. Frequently Asked Interview Questions

### What Is JDBC?

JDBC is the standard Java API used to communicate with relational databases.

### Which JDBC Driver Is Commonly Used?

The Type 4 driver is commonly used because it is written entirely in Java and communicates directly with the database.

### What Is the Difference Between Statement and PreparedStatement?

`Statement` executes ordinary SQL text. `PreparedStatement` supports parameters, improves safety, and helps prevent SQL injection.

### What Is a ResultSet?

A `ResultSet` represents records returned by a database query.

### What Is Auto-Commit?

Auto-commit causes each SQL operation to be committed automatically. It is enabled by default for a new JDBC connection.

### What Is Connection Pooling?

Connection pooling maintains reusable database connections instead of creating a new physical connection for every request.

### Why Should JDBC Resources Be Closed?

Unclosed resources can cause connection leaks, memory problems, and database-resource exhaustion.

### Is Class.forName() Mandatory?

Usually not. JDBC 4.0-compatible drivers are automatically discovered when their driver library is available.

## 24. Quick Revision

```text
JDBC
 ├── Connection
 ├── Statement
 │    ├── Statement
 │    ├── PreparedStatement
 │    └── CallableStatement
 ├── ResultSet
 ├── Transaction Management
 ├── Batch Processing
 └── SQLException
```

The most important JDBC flow is:

```text
Connect → Prepare SQL → Set Parameters → Execute
        → Process Result → Commit/Rollback → Close
```
