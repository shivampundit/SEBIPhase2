# Object-Oriented Programming - Advanced Multi-Language Guide
## 🎯 Complete Exam Preparation with Deep Concepts, Multiple Languages & Tricky Questions

---

## 📚 Complete Coverage

This guide covers:
- ✅ **4 Languages**: Python, Java, C++, C#
- ✅ **All OOP Concepts**: Abstraction, Encapsulation, Polymorphism, Inheritance
- ✅ **Advanced Topics**: SOLID, Design Patterns, Best Practices
- ✅ **Visual Aids**: Diagrams, Charts, Tables, Flow Diagrams
- ✅ **Exam-Focused**: MCQs, Debugging, Dry Runs, Logic Completion, Data Analysis
- ✅ **Tricky Questions**: Edge cases, Common pitfalls, Language-specific quirks

---

## 📑 Table of Contents

### Part 1: Core Concepts with Multi-Language Implementation
1. [Abstraction](#1-abstraction)
2. [Encapsulation](#2-encapsulation)
3. [Polymorphism](#3-polymorphism)
4. [Inheritance](#4-inheritance)

### Part 2: Advanced Concepts
5. [SOLID Principles Deep Dive](#5-solid-principles)
6. [Design Patterns](#6-design-patterns)
7. [Class Relationships](#7-class-relationships)
8. [Language-Specific Features](#8-language-specific-features)

### Part 3: Exam Preparation
9. [Tricky MCQs - All Categories](#9-tricky-mcqs)
10. [Common Pitfalls & Edge Cases](#10-common-pitfalls)
11. [Debugging Scenarios](#11-debugging-scenarios)
12. [Quick Reference Charts](#12-quick-reference)

---

# PART 1: CORE CONCEPTS

## 📊 The Four Pillars - Visual Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                     OOP FOUR PILLARS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ╔═══════════════╗  ╔═══════════════╗  ╔═══════════════╗      │
│  ║ ABSTRACTION   ║  ║ ENCAPSULATION ║  ║ POLYMORPHISM  ║      │
│  ╚═══════════════╝  ╚═══════════════╝  ╚═══════════════╝      │
│        ↓                   ↓                   ↓               │
│   Hide Complex         Bundle Data        Many Forms           │
│   Implementation       + Methods          Same Interface       │
│                       Hide Details                             │
│                                                                 │
│  ╔═══════════════╗                                             │
│  ║ INHERITANCE   ║                                             │
│  ╚═══════════════╝                                             │
│        ↓                                                        │
│   Parent-Child                                                  │
│   Code Reuse                                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 1️⃣ ABSTRACTION

### 📌 Concept Map

```
                    ABSTRACTION
                        ↓
        ┌───────────────┴───────────────┐
        ↓                               ↓
   WHAT (Interface)              HOW (Implementation)
        ↓                               ↓
   Show Essential                   Hide Complex
      Features                         Details
        ↓                               ↓
   ┌─────────┐                   ┌──────────┐
   │Abstract │                   │ Concrete │
   │ Class   │                   │  Class   │
   └─────────┘                   └──────────┘
        ↑                               ↑
        └───────────────┬───────────────┘
                        ↓
                  Implementation
                   Relationship
```

### 🌍 Language Comparison Table

| Feature | Python | Java | C++ | C# |
|---------|--------|------|-----|-----|
| **Abstract Class Keyword** | `ABC` class | `abstract` | No keyword (pure virtual) | `abstract` |
| **Abstract Method** | `@abstractmethod` | `abstract` modifier | `virtual ... = 0` | `abstract` modifier |
| **Interface** | Multiple ABC inheritance | `interface` keyword | Pure abstract class | `interface` keyword |
| **Can have constructors** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **Can have concrete methods** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **Multiple inheritance** | ✅ Yes | ❌ No (via interfaces) | ✅ Yes | ❌ No (via interfaces) |
| **Default method in interface** | ✅ Yes | ✅ Yes (Java 8+) | N/A | ✅ Yes (C# 8+) |

---

### 🐍 Python - Abstract Classes

```python
from abc import ABC, abstractmethod
from typing import List

class DatabaseConnection(ABC):
    """
    Abstract base class for database connections
    Defines contract that all database implementations must follow
    """
    
    def __init__(self, host: str, port: int):
        """Concrete constructor - shared by all implementations"""
        self.host = host
        self.port = port
        self.is_connected = False
    
    @abstractmethod
    def connect(self) -> bool:
        """Abstract method - MUST be implemented by subclasses"""
        pass
    
    @abstractmethod
    def disconnect(self) -> bool:
        """Abstract method - MUST be implemented"""
        pass
    
    @abstractmethod
    def execute_query(self, query: str) -> List:
        """Abstract method - MUST be implemented"""
        pass
    
    def get_connection_info(self) -> str:
        """Concrete method - shared implementation"""
        return f"Host: {self.host}, Port: {self.port}, Connected: {self.is_connected}"
    
    def __repr__(self) -> str:
        """Concrete method using abstract concept"""
        return f"{self.__class__.__name__}({self.host}:{self.port})"

class MySQLConnection(DatabaseConnection):
    """Concrete implementation for MySQL"""
    
    def __init__(self, host: str, port: int, database: str):
        super().__init__(host, port)
        self.database = database
    
    def connect(self) -> bool:
        print(f"Connecting to MySQL at {self.host}:{self.port}/{self.database}")
        self.is_connected = True
        return True
    
    def disconnect(self) -> bool:
        print(f"Disconnecting from MySQL")
        self.is_connected = False
        return True
    
    def execute_query(self, query: str) -> List:
        if not self.is_connected:
            raise ConnectionError("Not connected to database")
        print(f"Executing MySQL query: {query}")
        return [{"result": "mock data"}]

class PostgreSQLConnection(DatabaseConnection):
    """Concrete implementation for PostgreSQL"""
    
    def __init__(self, host: str, port: int, database: str):
        super().__init__(host, port)
        self.database = database
    
    def connect(self) -> bool:
        print(f"Connecting to PostgreSQL at {self.host}:{self.port}/{self.database}")
        self.is_connected = True
        return True
    
    def disconnect(self) -> bool:
        print(f"Disconnecting from PostgreSQL")
        self.is_connected = False
        return True
    
    def execute_query(self, query: str) -> List:
        if not self.is_connected:
            raise ConnectionError("Not connected to database")
        print(f"Executing PostgreSQL query: {query}")
        return [{"result": "mock data"}]

class MongoDBConnection(DatabaseConnection):
    """Concrete implementation for MongoDB"""
    
    def __init__(self, host: str, port: int, database: str):
        super().__init__(host, port)
        self.database = database
    
    def connect(self) -> bool:
        print(f"Connecting to MongoDB at {self.host}:{self.port}/{self.database}")
        self.is_connected = True
        return True
    
    def disconnect(self) -> bool:
        print(f"Disconnecting from MongoDB")
        self.is_connected = False
        return True
    
    def execute_query(self, query: str) -> List:
        if not self.is_connected:
            raise ConnectionError("Not connected to database")
        print(f"Executing MongoDB query: {query}")
        return [{"result": "mock data"}]

# Polymorphic usage - abstraction in action
def perform_database_operations(db: DatabaseConnection, queries: List[str]):
    """
    Works with ANY database implementation
    Doesn't need to know the specific type
    This is the power of abstraction!
    """
    db.connect()
    print(db.get_connection_info())
    
    for query in queries:
        results = db.execute_query(query)
        print(f"Results: {results}")
    
    db.disconnect()

# Usage Example
if __name__ == "__main__":
    # Cannot instantiate abstract class
    # db = DatabaseConnection("localhost", 5432)  # ❌ TypeError
    
    # Use concrete implementations
    mysql_db = MySQLConnection("localhost", 3306, "mydb")
    postgres_db = PostgreSQLConnection("localhost", 5432, "mydb")
    mongo_db = MongoDBConnection("localhost", 27017, "mydb")
    
    # Same function works with all implementations
    databases = [mysql_db, postgres_db, mongo_db]
    
    for db in databases:
        print(f"\n--- Using {db} ---")
        perform_database_operations(db, ["SELECT * FROM users", "SELECT * FROM orders"])
```

**🔍 Detailed Dry Run:**

```
Step-by-Step Execution:

1. mysql_db = MySQLConnection("localhost", 3306, "mydb")
   ├─ Calls MySQLConnection.__init__
   ├─ super().__init__("localhost", 3306) → DatabaseConnection.__init__
   │  ├─ Sets self.host = "localhost"
   │  ├─ Sets self.port = 3306
   │  └─ Sets self.is_connected = False
   └─ Sets self.database = "mydb"

2. perform_database_operations(mysql_db, ["SELECT * FROM users"])
   ├─ db parameter = mysql_db (MySQLConnection instance)
   ├─ db.connect() → calls MySQLConnection.connect()
   │  ├─ Prints: "Connecting to MySQL at localhost:3306/mydb"
   │  ├─ Sets self.is_connected = True
   │  └─ Returns: True
   │
   ├─ db.get_connection_info() → calls DatabaseConnection.get_connection_info()
   │  └─ Returns: "Host: localhost, Port: 3306, Connected: True"
   │
   ├─ Loop: query = "SELECT * FROM users"
   │  ├─ db.execute_query(query) → calls MySQLConnection.execute_query()
   │  ├─ Checks: self.is_connected == True ✓
   │  ├─ Prints: "Executing MySQL query: SELECT * FROM users"
   │  └─ Returns: [{"result": "mock data"}]
   │
   └─ db.disconnect() → calls MySQLConnection.disconnect()
      ├─ Prints: "Disconnecting from MySQL"
      ├─ Sets self.is_connected = False
      └─ Returns: True

Key Points:
- Abstract class defines contract (interface)
- Concrete classes provide implementation
- Polymorphism allows same code to work with different types
- Client code doesn't need to know specific implementation
```

---

### ☕ Java - Abstract Classes and Interfaces

```java
// Abstract class with both abstract and concrete methods
abstract class DatabaseConnection {
    protected String host;
    protected int port;
    protected boolean isConnected;
    
    // Constructor in abstract class
    public DatabaseConnection(String host, int port) {
        this.host = host;
        this.port = port;
        this.isConnected = false;
    }
    
    // Abstract methods - must be implemented
    public abstract boolean connect();
    public abstract boolean disconnect();
    public abstract java.util.List<String> executeQuery(String query);
    
    // Concrete method - shared implementation
    public String getConnectionInfo() {
        return String.format("Host: %s, Port: %d, Connected: %b", 
                           host, port, isConnected);
    }
    
    @Override
    public String toString() {
        return String.format("%s(%s:%d)", 
                           getClass().getSimpleName(), host, port);
    }
}

// Concrete implementation
class MySQLConnection extends DatabaseConnection {
    private String database;
    
    public MySQLConnection(String host, int port, String database) {
        super(host, port);  // Call parent constructor
        this.database = database;
    }
    
    @Override
    public boolean connect() {
        System.out.println("Connecting to MySQL at " + host + ":" + port + "/" + database);
        isConnected = true;
        return true;
    }
    
    @Override
    public boolean disconnect() {
        System.out.println("Disconnecting from MySQL");
        isConnected = false;
        return true;
    }
    
    @Override
    public java.util.List<String> executeQuery(String query) {
        if (!isConnected) {
            throw new RuntimeException("Not connected to database");
        }
        System.out.println("Executing MySQL query: " + query);
        return java.util.Arrays.asList("result1", "result2");
    }
}

// Interface for additional capabilities
interface Transactional {
    void beginTransaction();
    void commit();
    void rollback();
}

// Interface with default method (Java 8+)
interface Cacheable {
    boolean isCacheEnabled();
    
    // Default implementation
    default void clearCache() {
        System.out.println("Clearing cache...");
    }
}

// Class implementing multiple interfaces
class AdvancedPostgreSQLConnection extends DatabaseConnection 
    implements Transactional, Cacheable {
    
    private String database;
    private boolean inTransaction = false;
    private boolean cacheEnabled = true;
    
    public AdvancedPostgreSQLConnection(String host, int port, String database) {
        super(host, port);
        this.database = database;
    }
    
    @Override
    public boolean connect() {
        System.out.println("Connecting to PostgreSQL at " + host + ":" + port + "/" + database);
        isConnected = true;
        return true;
    }
    
    @Override
    public boolean disconnect() {
        if (inTransaction) {
            System.out.println("Rolling back uncommitted transaction");
            rollback();
        }
        System.out.println("Disconnecting from PostgreSQL");
        isConnected = false;
        return true;
    }
    
    @Override
    public java.util.List<String> executeQuery(String query) {
        if (!isConnected) {
            throw new RuntimeException("Not connected to database");
        }
        System.out.println("Executing PostgreSQL query: " + query);
        return java.util.Arrays.asList("result1", "result2");
    }
    
    // Implementing Transactional interface
    @Override
    public void beginTransaction() {
        System.out.println("Beginning transaction");
        inTransaction = true;
    }
    
    @Override
    public void commit() {
        if (inTransaction) {
            System.out.println("Committing transaction");
            inTransaction = false;
        }
    }
    
    @Override
    public void rollback() {
        if (inTransaction) {
            System.out.println("Rolling back transaction");
            inTransaction = false;
        }
    }
    
    // Implementing Cacheable interface
    @Override
    public boolean isCacheEnabled() {
        return cacheEnabled;
    }
    
    // Can use default clearCache() or override it
}

// Usage
public class AbstractionDemo {
    // Polymorphic method - works with any DatabaseConnection
    public static void performOperations(DatabaseConnection db, String[] queries) {
        db.connect();
        System.out.println(db.getConnectionInfo());
        
        for (String query : queries) {
            java.util.List<String> results = db.executeQuery(query);
            System.out.println("Results: " + results);
        }
        
        db.disconnect();
    }
    
    public static void main(String[] args) {
        // Cannot instantiate abstract class
        // DatabaseConnection db = new DatabaseConnection("localhost", 5432);  // ❌ Error
        
        // Create concrete implementations
        MySQLConnection mysql = new MySQLConnection("localhost", 3306, "mydb");
        AdvancedPostgreSQLConnection postgres = 
            new AdvancedPostgreSQLConnection("localhost", 5432, "mydb");
        
        // Polymorphism in action
        System.out.println("=== MySQL ===");
        performOperations(mysql, new String[]{"SELECT * FROM users"});
        
        System.out.println("\n=== PostgreSQL with Transactions ===");
        postgres.connect();
        postgres.beginTransaction();
        postgres.executeQuery("INSERT INTO users VALUES (1, 'Alice')");
        postgres.commit();
        postgres.clearCache();  // Uses default method
        postgres.disconnect();
    }
}
```

**📊 Java Abstract Class vs Interface:**

```
┌────────────────────────────────────────────────────────────┐
│         ABSTRACT CLASS vs INTERFACE (Java)                 │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  ABSTRACT CLASS               INTERFACE                    │
│  ├─ Can have constructors    ├─ No constructors          │
│  ├─ Can have instance vars   ├─ Only constants           │
│  ├─ Abstract + concrete      ├─ Abstract + default (8+)  │
│  ├─ Single inheritance       ├─ Multiple implementation   │
│  ├─ Any access modifiers     ├─ Public only              │
│  └─ "IS-A" relationship      └─ "CAN-DO" capability      │
│                                                            │
│  WHEN TO USE:                                              │
│  Abstract Class → Shared base with state                   │
│  Interface → Pure contract/capability                      │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

### ⚡ C++ - Pure Virtual Functions

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <memory>
using namespace std;

/**
 * Abstract base class with pure virtual functions
 * Pure virtual function: virtual return_type function_name() = 0;
 */
class DatabaseConnection {
protected:
    string host;
    int port;
    bool isConnected;
    
public:
    // Constructor
    DatabaseConnection(const string& h, int p) 
        : host(h), port(p), isConnected(false) {
        cout << "DatabaseConnection constructor called" << endl;
    }
    
    // Virtual destructor (CRITICAL for polymorphism!)
    virtual ~DatabaseConnection() {
        cout << "DatabaseConnection destructor called" << endl;
    }
    
    // Pure virtual functions (= 0 makes them abstract)
    virtual bool connect() = 0;
    virtual bool disconnect() = 0;
    virtual vector<string> executeQuery(const string& query) = 0;
    
    // Concrete method
    string getConnectionInfo() const {
        return "Host: " + host + ", Port: " + to_string(port) + 
               ", Connected: " + (isConnected ? "true" : "false");
    }
    
    // Virtual function with default implementation (can be overridden)
    virtual void logActivity(const string& message) {
        cout << "[LOG] " << message << endl;
    }
};

/**
 * Concrete implementation - MySQL
 */
class MySQLConnection : public DatabaseConnection {
private:
    string database;
    
public:
    MySQLConnection(const string& host, int port, const string& db)
        : DatabaseConnection(host, port), database(db) {
        cout << "MySQLConnection constructor called" << endl;
    }
    
    ~MySQLConnection() {
        cout << "MySQLConnection destructor called" << endl;
        if (isConnected) {
            disconnect();
        }
    }
    
    // Must implement all pure virtual functions
    bool connect() override {
        cout << "Connecting to MySQL at " << host << ":" << port << "/" << database << endl;
        isConnected = true;
        logActivity("MySQL connected");
        return true;
    }
    
    bool disconnect() override {
        cout << "Disconnecting from MySQL" << endl;
        isConnected = false;
        logActivity("MySQL disconnected");
        return true;
    }
    
    vector<string> executeQuery(const string& query) override {
        if (!isConnected) {
            throw runtime_error("Not connected to database");
        }
        cout << "Executing MySQL query: " << query << endl;
        return {"result1", "result2"};
    }
};

/**
 * Another concrete implementation - PostgreSQL
 */
class PostgreSQLConnection : public DatabaseConnection {
private:
    string database;
    
public:
    PostgreSQLConnection(const string& host, int port, const string& db)
        : DatabaseConnection(host, port), database(db) {
        cout << "PostgreSQLConnection constructor called" << endl;
    }
    
    ~PostgreSQLConnection() {
        cout << "PostgreSQLConnection destructor called" << endl;
    }
    
    bool connect() override {
        cout << "Connecting to PostgreSQL at " << host << ":" << port << "/" << database << endl;
        isConnected = true;
        return true;
    }
    
    bool disconnect() override {
        cout << "Disconnecting from PostgreSQL" << endl;
        isConnected = false;
        return true;
    }
    
    vector<string> executeQuery(const string& query) override {
        if (!isConnected) {
            throw runtime_error("Not connected to database");
        }
        cout << "Executing PostgreSQL query: " << query << endl;
        return {"result1", "result2"};
    }
    
    // Override virtual function with custom implementation
    void logActivity(const string& message) override {
        cout << "[PostgreSQL LOG] " << message << endl;
    }
};

/**
 * Interface-like class (pure abstract class)
 */
class Transactional {
public:
    virtual ~Transactional() {}
    virtual void beginTransaction() = 0;
    virtual void commit() = 0;
    virtual void rollback() = 0;
};

/**
 * Multiple inheritance - C++ supports it!
 */
class TransactionalPostgreSQL : public DatabaseConnection, public Transactional {
private:
    string database;
    bool inTransaction;
    
public:
    TransactionalPostgreSQL(const string& host, int port, const string& db)
        : DatabaseConnection(host, port), database(db), inTransaction(false) {}
    
    bool connect() override {
        cout << "Connecting to Transactional PostgreSQL" << endl;
        isConnected = true;
        return true;
    }
    
    bool disconnect() override {
        if (inTransaction) {
            rollback();
        }
        cout << "Disconnecting from Transactional PostgreSQL" << endl;
        isConnected = false;
        return true;
    }
    
    vector<string> executeQuery(const string& query) override {
        if (!isConnected) {
            throw runtime_error("Not connected");
        }
        cout << "Executing transactional query: " << query << endl;
        return {"result"};
    }
    
    // Implement Transactional interface
    void beginTransaction() override {
        cout << "BEGIN TRANSACTION" << endl;
        inTransaction = true;
    }
    
    void commit() override {
        if (inTransaction) {
            cout << "COMMIT" << endl;
            inTransaction = false;
        }
    }
    
    void rollback() override {
        if (inTransaction) {
            cout << "ROLLBACK" << endl;
            inTransaction = false;
        }
    }
};

// Polymorphic function
void performOperations(DatabaseConnection* db, const vector<string>& queries) {
    db->connect();
    cout << db->getConnectionInfo() << endl;
    
    for (const string& query : queries) {
        vector<string> results = db->executeQuery(query);
        cout << "Results size: " << results.size() << endl;
    }
    
    db->disconnect();
}

int main() {
    cout << "=== C++ Abstraction Demo ===" << endl;
    
    // Cannot instantiate abstract class
    // DatabaseConnection* db = new DatabaseConnection("localhost", 5432);  // ❌ Error
    
    // Using smart pointers (modern C++)
    unique_ptr<DatabaseConnection> mysql = 
        make_unique<MySQLConnection>("localhost", 3306, "mydb");
    
    unique_ptr<DatabaseConnection> postgres = 
        make_unique<PostgreSQLConnection>("localhost", 5432, "mydb");
    
    // Polymorphism with raw pointers
    cout << "\n--- MySQL Operations ---" << endl;
    performOperations(mysql.get(), {"SELECT * FROM users"});
    
    cout << "\n--- PostgreSQL Operations ---" << endl;
    performOperations(postgres.get(), {"SELECT * FROM orders"});
    
    // Multiple inheritance example
    cout << "\n--- Transactional PostgreSQL ---" << endl;
    TransactionalPostgreSQL* txDb = new TransactionalPostgreSQL("localhost", 5432, "mydb");
    txDb->connect();
    txDb->beginTransaction();
    txDb->executeQuery("INSERT INTO users VALUES (1, 'Alice')");
    txDb->commit();
    txDb->disconnect();
    delete txDb;
    
    cout << "\n--- Destructors will be called automatically ---" << endl;
    // Smart pointers automatically delete when going out of scope
    
    return 0;
}
```

**⚠️ C++ Critical Points:**

```
┌───────────────────────────────────────────────────────────┐
│          C++ VIRTUAL FUNCTIONS - KEY CONCEPTS             │
├───────────────────────────────────────────────────────────┤
│                                                           │
│  1. PURE VIRTUAL: virtual void func() = 0;               │
│     → Makes class abstract                                │
│     → Must be implemented by derived class                │
│                                                           │
│  2. VIRTUAL DESTRUCTOR: Always use in base class!         │
│     → virtual ~ClassName() {}                             │
│     → Ensures proper cleanup in polymorphism              │
│     → Missing = memory leaks!                             │
│                                                           │
│  3. OVERRIDE KEYWORD (C++11+): Use for safety            │
│     → void func() override;                               │
│     → Compiler checks if actually overriding              │
│     → Catches typos and signature mismatches              │
│                                                           │
│  4. MULTIPLE INHERITANCE: C++ supports it                 │
│     → class D : public B1, public B2 {}                   │
│     → Can inherit from multiple abstract classes          │
│     → Beware of diamond problem!                          │
│                                                           │
│  5. SMART POINTERS: Use unique_ptr/shared_ptr            │
│     → Automatic memory management                         │
│     → No manual delete needed                             │
│     → Exception-safe                                      │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

---

### 🔷 C# - Abstract Classes and Interfaces

```csharp
using System;
using System.Collections.Generic;

/// <summary>
/// Abstract base class for database connections
/// </summary>
abstract class DatabaseConnection
{
    protected string host;
    protected int port;
    protected bool isConnected;
    
    // Constructor in abstract class
    public DatabaseConnection(string host, int port)
    {
        this.host = host;
        this.port = port;
        this.isConnected = false;
    }
    
    // Abstract methods - must be implemented
    public abstract bool Connect();
    public abstract bool Disconnect();
    public abstract List<string> ExecuteQuery(string query);
    
    // Virtual method - can be overridden (optional)
    public virtual void LogActivity(string message)
    {
        Console.WriteLine($"[LOG] {message}");
    }
    
    // Concrete method - shared implementation
    public string GetConnectionInfo()
    {
        return $"Host: {host}, Port: {port}, Connected: {isConnected}";
    }
    
    public override string ToString()
    {
        return $"{GetType().Name}({host}:{port})";
    }
}

/// <summary>
/// Concrete implementation for MySQL
/// </summary>
class MySQLConnection : DatabaseConnection
{
    private string database;
    
    public MySQLConnection(string host, int port, string database) 
        : base(host, port)
    {
        this.database = database;
    }
    
    public override bool Connect()
    {
        Console.WriteLine($"Connecting to MySQL at {host}:{port}/{database}");
        isConnected = true;
        LogActivity("MySQL connected");
        return true;
    }
    
    public override bool Disconnect()
    {
        Console.WriteLine("Disconnecting from MySQL");
        isConnected = false;
        LogActivity("MySQL disconnected");
        return true;
    }
    
    public override List<string> ExecuteQuery(string query)
    {
        if (!isConnected)
            throw new InvalidOperationException("Not connected to database");
        
        Console.WriteLine($"Executing MySQL query: {query}");
        return new List<string> { "result1", "result2" };
    }
}

/// <summary>
/// Interface for transactional capabilities
/// </summary>
interface ITransactional
{
    void BeginTransaction();
    void Commit();
    void Rollback();
}

/// <summary>
/// Interface with default implementation (C# 8.0+)
/// </summary>
interface ICacheable
{
    bool IsCacheEnabled { get; }
    
    // Default interface method (C# 8.0+)
    void ClearCache()
    {
        Console.WriteLine("Clearing cache...");
    }
}

/// <summary>
/// Class implementing multiple interfaces
/// </summary>
class AdvancedPostgreSQLConnection : DatabaseConnection, ITransactional, ICacheable
{
    private string database;
    private bool inTransaction;
    private bool cacheEnabled;
    
    public AdvancedPostgreSQLConnection(string host, int port, string database)
        : base(host, port)
    {
        this.database = database;
        this.inTransaction = false;
        this.cacheEnabled = true;
    }
    
    public override bool Connect()
    {
        Console.WriteLine($"Connecting to PostgreSQL at {host}:{port}/{database}");
        isConnected = true;
        return true;
    }
    
    public override bool Disconnect()
    {
        if (inTransaction)
        {
            Console.WriteLine("Rolling back uncommitted transaction");
            Rollback();
        }
        Console.WriteLine("Disconnecting from PostgreSQL");
        isConnected = false;
        return true;
    }
    
    public override List<string> ExecuteQuery(string query)
    {
        if (!isConnected)
            throw new InvalidOperationException("Not connected to database");
        
        Console.WriteLine($"Executing PostgreSQL query: {query}");
        return new List<string> { "result1", "result2" };
    }
    
    // Override virtual method
    public override void LogActivity(string message)
    {
        Console.WriteLine($"[PostgreSQL LOG] {message}");
    }
    
    // Implement ITransactional
    public void BeginTransaction()
    {
        Console.WriteLine("BEGIN TRANSACTION");
        inTransaction = true;
    }
    
    public void Commit()
    {
        if (inTransaction)
        {
            Console.WriteLine("COMMIT");
            inTransaction = false;
        }
    }
    
    public void Rollback()
    {
        if (inTransaction)
        {
            Console.WriteLine("ROLLBACK");
            inTransaction = false;
        }
    }
    
    // Implement ICacheable
    public bool IsCacheEnabled => cacheEnabled;
    
    // Can use default ClearCache() or override it
}

/// <summary>
/// Polymorphic method
/// </summary>
class DatabaseOperations
{
    public static void PerformOperations(DatabaseConnection db, string[] queries)
    {
        db.Connect();
        Console.WriteLine(db.GetConnectionInfo());
        
        foreach (string query in queries)
        {
            List<string> results = db.ExecuteQuery(query);
            Console.WriteLine($"Results count: {results.Count}");
        }
        
        db.Disconnect();
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine("=== C# Abstraction Demo ===\n");
        
        // Cannot instantiate abstract class
        // DatabaseConnection db = new DatabaseConnection("localhost", 5432);  // ❌ Error
        
        // Create concrete implementations
        MySQLConnection mysql = new MySQLConnection("localhost", 3306, "mydb");
        AdvancedPostgreSQLConnection postgres = 
            new AdvancedPostgreSQLConnection("localhost", 5432, "mydb");
        
        // Polymorphism in action
        Console.WriteLine("--- MySQL Operations ---");
        DatabaseOperations.PerformOperations(mysql, new[] { "SELECT * FROM users" });
        
        Console.WriteLine("\n--- PostgreSQL with Transactions ---");
        postgres.Connect();
        postgres.BeginTransaction();
        postgres.ExecuteQuery("INSERT INTO users VALUES (1, 'Alice')");
        postgres.Commit();
        postgres.ClearCache();  // Uses default interface method
        postgres.Disconnect();
        
        Console.WriteLine("\n--- Demonstrating Polymorphism ---");
        DatabaseConnection[] databases = { mysql, postgres };
        
        foreach (DatabaseConnection db in databases)
        {
            Console.WriteLine($"\nUsing: {db}");
            db.Connect();
            Console.WriteLine(db.GetConnectionInfo());
            db.Disconnect();
        }
    }
}
```

**📊 C# Properties and Features:**

```
┌──────────────────────────────────────────────────────────┐
│            C# ABSTRACTION FEATURES                       │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  ABSTRACT CLASS:                                         │
│  ├─ abstract class ClassName { }                         │
│  ├─ abstract methods with no body                        │
│  ├─ virtual methods (can be overridden)                  │
│  └─ regular methods (concrete)                           │
│                                                          │
│  INTERFACE:                                              │
│  ├─ interface IName { }                                  │
│  ├─ All members implicitly public                        │
│  ├─ No implementation (pre-C# 8)                         │
│  ├─ Default methods (C# 8+)                              │
│  └─ Multiple interfaces allowed                          │
│                                                          │
│  PROPERTIES:                                             │
│  ├─ get; set; accessors                                  │
│  ├─ Auto-implemented properties                          │
│  ├─ Expression-bodied members                            │
│  └─ Property => expression;                              │
│                                                          │
│  OVERRIDE MODIFIERS:                                     │
│  ├─ abstract - must override                             │
│  ├─ virtual - can override                               │
│  ├─ override - overriding                                │
│  └─ sealed - prevent further override                    │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 🎯 Tricky MCQs on Abstraction

### Question 1: Multi-Language Abstract Instantiation

**Which of the following correctly prevents instantiation?**

```python
# Python
from abc import ABC, abstractmethod
class A(ABC):
    @abstractmethod
    def method(self): pass
```

```java
// Java
abstract class B {
    abstract void method();
}
```

```cpp
// C++
class C {
public:
    virtual void method() = 0;
};
```

```csharp
// C#
abstract class D {
    public abstract void Method();
}
```

**Options:**
- A) Only Python and Java prevent instantiation
- B) Only C++ and C# prevent instantiation
- C) All four prevent instantiation
- D) None prevent instantiation

**Answer: C) All four prevent instantiation**

**Explanation:**
All four languages prevent direct instantiation of abstract classes:
- Python: `ABC` with `@abstractmethod` → TypeError
- Java: `abstract` keyword → Compilation error
- C++: Pure virtual function `= 0` → Compilation error
- C#: `abstract` keyword → Compilation error

```python
# Python
a = A()  # ❌ TypeError: Can't instantiate abstract class A

# Java
B b = new B();  // ❌ Error: B is abstract; cannot be instantiated

// C++
C* c = new C();  // ❌ Error: cannot declare variable 'c' to be of abstract type 'C'

// C#
D d = new D();  // ❌ Error: Cannot create an instance of the abstract class
```

---

### Question 2: Abstract Method with Body

**Which language(s) allow abstract methods to have a method body?**

```python
# Python
class A(ABC):
    @abstractmethod
    def method(self):
        print("Base implementation")  # ← Has body!
```

```java
// Java (pre-8)
abstract class B {
    abstract void method() {
        System.out.println("Body");  // Possible?
    }
}
```

```cpp
// C++
class C {
public:
    virtual void method() = 0 {  // Possible?
        cout << "Body";
    }
};
```

**Options:**
- A) Python only
- B) Python and C++
- C) All three languages
- D) None - abstract methods cannot have bodies

**Answer: A) Python only**

**Explanation:**
- **Python**: Abstract methods CAN have implementation! Subclasses can call it via `super()`.
```python
class Child(A):
    def method(self):
        super().method()  # Calls abstract method's body
        print("Child implementation")
```

- **Java (pre-8)**: Abstract methods CANNOT have body → Compilation error
- **Java 8+**: Can use `default` methods in interfaces (but not in abstract methods)
- **C++**: Pure virtual functions CANNOT have body in declaration (but can define separately - rare)

---

### Question 3: Interface vs Abstract Class Selection

**Scenario: Building a payment processing system**

Requirements:
- Support CreditCard, PayPal, Bitcoin, BankTransfer
- All need `processPayment(amount)` method
- CreditCard and BankTransfer need CVV validation
- Bitcoin and PayPal need email validation

**Which design is BEST?**

A)
```java
abstract class Payment {
    abstract void processPayment(double amount);
}
```

B)
```java
interface Payment {
    void processPayment(double amount);
}
interface SecurePayment extends Payment {
    boolean validateCVV(String cvv);
}
```

C)
```java
abstract class Payment {
    protected String paymentId;
    abstract void processPayment(double amount);
    void logTransaction() { /* shared implementation */ }
}
interface Validatable {
    boolean validate(String data);
}
```

D)
```java
class Payment {
    void processPayment(double amount, String type) {
        if (type.equals("credit")) { /* ... */ }
        else if (type.equals("paypal")) { /* ... */ }
        // ... more if-else
    }
}
```

**Answer: C**

**Explanation:**
- **Option A**: Too simple, no shared functionality
- **Option B**: Interfaces can't have state (paymentId), forces interface segregation issue
- **Option C**: ✅ BEST
  - Abstract class provides shared state (`paymentId`) and behavior (`logTransaction`)
  - Interface provides validation contract without forcing all payments to implement it
  - Flexible: `class CreditCard extends Payment implements Validatable`
- **Option D**: ❌ Violates OOP principles, not extensible

**Design Principle**: Use abstract class for "IS-A" with shared state/behavior + interfaces for "CAN-DO" capabilities.

---

### Question 4: Constructor in Abstract Class

```java
abstract class Vehicle {
    private String brand;
    
    public Vehicle(String brand) {  // Line 1
        this.brand = brand;
        System.out.println("Vehicle constructor: " + brand);
    }
    
    abstract void start();
}

class Car extends Vehicle {
    public Car(String brand) {
        super(brand);  // Line 2
        System.out.println("Car constructor");
    }
    
    void start() {
        System.out.println("Car started");
    }
}

public class Test {
    public static void main(String[] args) {
        Car c = new Car("Toyota");  // Line 3
    }
}
```

**What is the output?**
- A) Compilation error at Line 1
- B) Compilation error at Line 2
- C) Compilation error at Line 3
- D) Vehicle constructor: Toyota\nCar constructor

**Answer: D**

**Output:**
```
Vehicle constructor: Toyota
Car constructor
```

**Explanation:**
- Abstract classes CAN have constructors
- Cannot instantiate abstract class directly, but constructor is called when subclass is created
- Line 3: Creates Car object
  - Calls Car constructor
  - Car constructor calls `super(brand)` → Vehicle constructor
  - Vehicle constructor prints "Vehicle constructor: Toyota"
  - Returns to Car constructor, prints "Car constructor"

**Key Point**: Abstract class constructors initialize shared state for all subclasses.

---

### Question 5: Multiple Inheritance Diamond Problem

```python
from abc import ABC, abstractmethod

class A(ABC):
    @abstractmethod
    def method(self):
        return "A"

class B(A):
    def method(self):
        return super().method() + " B"

class C(A):
    def method(self):
        return super().method() + " C"

class D(B, C):
    def method(self):
        return super().method() + " D"

obj = D()
print(obj.method())
print(D.__mro__)
```

**What is the output?**
- A) Error: Cannot call abstract method
- B) "A B D"
- C) "A B C D"
- D) "A C B D"

**Answer: C) "A B C D"**

**Explanation - Method Resolution Order (MRO):**

```
D.__mro__ = (D, B, C, A, object)

Call chain:
1. D.method() called
   - super().method() → follows MRO → B.method()
   - Returns: (B's return) + " D"

2. B.method() called
   - super().method() → follows MRO → C.method() (NOT A!)
   - Returns: (C's return) + " B"

3. C.method() called
   - super().method() → follows MRO → A.method()
   - Returns: (A's return) + " C"

4. A.method() called
   - Returns: "A"

Final chain: "A" → "A C" → "A C B" → "A C B D"
Output: "A C B D"
```

**Key Insight**: Python uses C3 Linearization for MRO, ensuring consistent method resolution in multiple inheritance. Even though B and C both inherit from A, A is called only once!

---

