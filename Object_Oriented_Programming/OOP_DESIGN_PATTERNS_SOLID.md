# 🎯 OOP: Design Patterns & SOLID Principles

## 📚 Table of Contents
1. [SOLID Principles](#solid-principles)
2. [Creational Patterns](#creational-patterns)
3. [Structural Patterns](#structural-patterns)
4. [Behavioral Patterns](#behavioral-patterns)
5. [Anti-Patterns](#anti-patterns)
6. [Tricky MCQs](#tricky-mcqs)

---

## 🔷 SOLID PRINCIPLES

### Overview

```
SOLID
  ├── S - Single Responsibility Principle
  ├── O - Open/Closed Principle
  ├── L - Liskov Substitution Principle
  ├── I - Interface Segregation Principle
  └── D - Dependency Inversion Principle
```

---

## 1️⃣ Single Responsibility Principle (SRP)

**Definition:** A class should have one, and only one, reason to change.

### ❌ Bad Example (Violates SRP)

#### Python
```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
    
    def calculate_pay(self):
        # Business logic
        return self.salary * 1.1
    
    def save_to_database(self):
        # Database logic
        print(f"Saving {self.name} to database")
    
    def generate_report(self):
        # Reporting logic
        return f"Report for {self.name}: ${self.salary}"
```

**Problem:** This class has THREE reasons to change:
1. Pay calculation logic changes
2. Database technology changes
3. Report format changes

### ✅ Good Example (Follows SRP)

#### Python
```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

class PayCalculator:
    def calculate_pay(self, employee):
        return employee.salary * 1.1

class EmployeeRepository:
    def save(self, employee):
        print(f"Saving {employee.name} to database")

class EmployeeReportGenerator:
    def generate_report(self, employee):
        return f"Report for {employee.name}: ${employee.salary}"
```

### Multi-Language Examples

#### Java
```java
// ✅ Good: Each class has single responsibility
class Employee {
    private String name;
    private double salary;
    
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
    
    // Getters and setters
    public String getName() { return name; }
    public double getSalary() { return salary; }
}

class SalaryCalculator {
    public double calculateBonus(Employee emp) {
        return emp.getSalary() * 0.1;
    }
}

class EmployeeDAO {
    public void save(Employee emp) {
        // Save to database
        System.out.println("Saving " + emp.getName());
    }
}

class ReportGenerator {
    public String generate(Employee emp) {
        return "Employee: " + emp.getName() + ", Salary: " + emp.getSalary();
    }
}
```

#### C++
```cpp
// Employee class - only data
class Employee {
private:
    string name;
    double salary;
public:
    Employee(string n, double s) : name(n), salary(s) {}
    string getName() const { return name; }
    double getSalary() const { return salary; }
};

// Separate class for salary calculations
class SalaryCalculator {
public:
    double calculateBonus(const Employee& emp) {
        return emp.getSalary() * 0.1;
    }
};

// Separate class for persistence
class EmployeeRepository {
public:
    void save(const Employee& emp) {
        cout << "Saving " << emp.getName() << endl;
    }
};
```

#### C#
```csharp
// Employee - pure data model
public class Employee {
    public string Name { get; set; }
    public double Salary { get; set; }
    
    public Employee(string name, double salary) {
        Name = name;
        Salary = salary;
    }
}

// Separate concerns
public class SalaryCalculator {
    public double CalculateBonus(Employee emp) {
        return emp.Salary * 0.1;
    }
}

public class EmployeeRepository {
    public void Save(Employee emp) {
        Console.WriteLine($"Saving {emp.Name}");
    }
}

public class ReportGenerator {
    public string Generate(Employee emp) {
        return $"Employee: {emp.Name}, Salary: {emp.Salary}";
    }
}
```

---

## 2️⃣ Open/Closed Principle (OCP)

**Definition:** Software entities should be open for extension but closed for modification.

### ❌ Bad Example (Violates OCP)

#### Python
```python
class DiscountCalculator:
    def calculate(self, customer_type, amount):
        if customer_type == "regular":
            return amount * 0.9
        elif customer_type == "premium":
            return amount * 0.8
        elif customer_type == "gold":
            return amount * 0.7
        # Adding new customer type requires modifying this class!
```

### ✅ Good Example (Follows OCP)

#### Python
```python
from abc import ABC, abstractmethod

class DiscountStrategy(ABC):
    @abstractmethod
    def calculate(self, amount):
        pass

class RegularDiscount(DiscountStrategy):
    def calculate(self, amount):
        return amount * 0.9

class PremiumDiscount(DiscountStrategy):
    def calculate(self, amount):
        return amount * 0.8

class GoldDiscount(DiscountStrategy):
    def calculate(self, amount):
        return amount * 0.7

# Easy to extend - just add new class
class PlatinumDiscount(DiscountStrategy):
    def calculate(self, amount):
        return amount * 0.6

class DiscountCalculator:
    def __init__(self, strategy: DiscountStrategy):
        self.strategy = strategy
    
    def calculate_price(self, amount):
        return self.strategy.calculate(amount)
```

### Multi-Language Examples

#### Java
```java
interface DiscountStrategy {
    double calculate(double amount);
}

class RegularDiscount implements DiscountStrategy {
    public double calculate(double amount) {
        return amount * 0.9;
    }
}

class PremiumDiscount implements DiscountStrategy {
    public double calculate(double amount) {
        return amount * 0.8;
    }
}

class GoldDiscount implements DiscountStrategy {
    public double calculate(double amount) {
        return amount * 0.7;
    }
}

// Closed for modification, open for extension
class ShoppingCart {
    private DiscountStrategy strategy;
    
    public ShoppingCart(DiscountStrategy strategy) {
        this.strategy = strategy;
    }
    
    public double calculateTotal(double amount) {
        return strategy.calculate(amount);
    }
}
```

#### C++
```cpp
class DiscountStrategy {
public:
    virtual double calculate(double amount) = 0;
    virtual ~DiscountStrategy() {}
};

class RegularDiscount : public DiscountStrategy {
public:
    double calculate(double amount) override {
        return amount * 0.9;
    }
};

class PremiumDiscount : public DiscountStrategy {
public:
    double calculate(double amount) override {
        return amount * 0.8;
    }
};

class ShoppingCart {
private:
    DiscountStrategy* strategy;
public:
    ShoppingCart(DiscountStrategy* s) : strategy(s) {}
    
    double calculateTotal(double amount) {
        return strategy->calculate(amount);
    }
};
```

---

## 3️⃣ Liskov Substitution Principle (LSP)

**Definition:** Objects of a superclass should be replaceable with objects of its subclasses without breaking the application.

### ❌ Bad Example (Violates LSP)

#### Python
```python
class Bird:
    def fly(self):
        return "Flying"

class Sparrow(Bird):
    def fly(self):
        return "Sparrow flying"

class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins can't fly!")  # Violates LSP!

def make_bird_fly(bird: Bird):
    return bird.fly()

sparrow = Sparrow()
penguin = Penguin()

print(make_bird_fly(sparrow))  # Works
print(make_bird_fly(penguin))  # Breaks! Exception raised
```

### ✅ Good Example (Follows LSP)

#### Python
```python
from abc import ABC, abstractmethod

class Bird(ABC):
    @abstractmethod
    def move(self):
        pass

class FlyingBird(Bird):
    def move(self):
        return self.fly()
    
    def fly(self):
        return "Flying in the sky"

class FlightlessBird(Bird):
    def move(self):
        return self.walk()
    
    def walk(self):
        return "Walking on ground"

class Sparrow(FlyingBird):
    def fly(self):
        return "Sparrow flying"

class Penguin(FlightlessBird):
    def walk(self):
        return "Penguin walking"

def make_bird_move(bird: Bird):
    return bird.move()

# Both work correctly
sparrow = Sparrow()
penguin = Penguin()
print(make_bird_move(sparrow))  # Works
print(make_bird_move(penguin))  # Works
```

### Multi-Language Examples

#### Java
```java
// ✅ Good design respecting LSP
abstract class Shape {
    abstract double area();
}

class Rectangle extends Shape {
    protected double width;
    protected double height;
    
    public Rectangle(double w, double h) {
        width = w;
        height = h;
    }
    
    public double area() {
        return width * height;
    }
}

class Square extends Shape {
    private double side;
    
    public Square(double side) {
        this.side = side;
    }
    
    public double area() {
        return side * side;
    }
}

// LSP satisfied: both can be used interchangeably
class AreaCalculator {
    public double totalArea(Shape[] shapes) {
        double total = 0;
        for (Shape shape : shapes) {
            total += shape.area();
        }
        return total;
    }
}
```

#### C#
```csharp
// Following LSP
abstract class Vehicle {
    public abstract void StartEngine();
}

class Car : Vehicle {
    public override void StartEngine() {
        Console.WriteLine("Car engine started with key");
    }
}

class ElectricCar : Vehicle {
    public override void StartEngine() {
        Console.WriteLine("Electric car started with button");
    }
}

// Both can substitute Vehicle without breaking logic
class Driver {
    public void DriveVehicle(Vehicle vehicle) {
        vehicle.StartEngine();  // Works for both
    }
}
```

---

## 4️⃣ Interface Segregation Principle (ISP)

**Definition:** Clients should not be forced to depend on interfaces they don't use.

### ❌ Bad Example (Violates ISP)

#### Python
```python
from abc import ABC, abstractmethod

class Worker(ABC):
    @abstractmethod
    def work(self):
        pass
    
    @abstractmethod
    def eat(self):
        pass
    
    @abstractmethod
    def sleep(self):
        pass

class Human(Worker):
    def work(self):
        return "Human working"
    
    def eat(self):
        return "Human eating"
    
    def sleep(self):
        return "Human sleeping"

class Robot(Worker):
    def work(self):
        return "Robot working"
    
    def eat(self):
        raise Exception("Robots don't eat!")  # Forced to implement
    
    def sleep(self):
        raise Exception("Robots don't sleep!")  # Forced to implement
```

### ✅ Good Example (Follows ISP)

#### Python
```python
from abc import ABC, abstractmethod

class Workable(ABC):
    @abstractmethod
    def work(self):
        pass

class Eatable(ABC):
    @abstractmethod
    def eat(self):
        pass

class Sleepable(ABC):
    @abstractmethod
    def sleep(self):
        pass

class Human(Workable, Eatable, Sleepable):
    def work(self):
        return "Human working"
    
    def eat(self):
        return "Human eating"
    
    def sleep(self):
        return "Human sleeping"

class Robot(Workable):  # Only implements what it needs
    def work(self):
        return "Robot working"
```

### Multi-Language Examples

#### Java
```java
// Split into smaller, focused interfaces
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class Human implements Workable, Eatable, Sleepable {
    public void work() {
        System.out.println("Human working");
    }
    
    public void eat() {
        System.out.println("Human eating");
    }
    
    public void sleep() {
        System.out.println("Human sleeping");
    }
}

class Robot implements Workable {
    public void work() {
        System.out.println("Robot working");
    }
}
```

#### C++
```cpp
// Segregated interfaces
class IWorkable {
public:
    virtual void work() = 0;
    virtual ~IWorkable() {}
};

class IEatable {
public:
    virtual void eat() = 0;
    virtual ~IEatable() {}
};

class Human : public IWorkable, public IEatable {
public:
    void work() override {
        cout << "Human working" << endl;
    }
    
    void eat() override {
        cout << "Human eating" << endl;
    }
};

class Robot : public IWorkable {
public:
    void work() override {
        cout << "Robot working" << endl;
    }
};
```

---

## 5️⃣ Dependency Inversion Principle (DIP)

**Definition:** 
1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.

### ❌ Bad Example (Violates DIP)

#### Python
```python
class MySQLDatabase:
    def connect(self):
        return "Connected to MySQL"
    
    def get_data(self):
        return "Data from MySQL"

class UserService:
    def __init__(self):
        self.db = MySQLDatabase()  # Tight coupling!
    
    def get_user(self):
        self.db.connect()
        return self.db.get_data()

# Cannot easily switch to PostgreSQL or MongoDB
```

### ✅ Good Example (Follows DIP)

#### Python
```python
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def connect(self):
        pass
    
    @abstractmethod
    def get_data(self):
        pass

class MySQLDatabase(Database):
    def connect(self):
        return "Connected to MySQL"
    
    def get_data(self):
        return "Data from MySQL"

class PostgreSQLDatabase(Database):
    def connect(self):
        return "Connected to PostgreSQL"
    
    def get_data(self):
        return "Data from PostgreSQL"

class MongoDB(Database):
    def connect(self):
        return "Connected to MongoDB"
    
    def get_data(self):
        return "Data from MongoDB"

class UserService:
    def __init__(self, database: Database):
        self.db = database  # Depends on abstraction
    
    def get_user(self):
        self.db.connect()
        return self.db.get_data()

# Easy to switch databases
mysql_service = UserService(MySQLDatabase())
postgres_service = UserService(PostgreSQLDatabase())
mongo_service = UserService(MongoDB())
```

### Multi-Language Examples

#### Java
```java
// Abstraction
interface IDatabase {
    void connect();
    String getData();
}

// Low-level modules
class MySQLDatabase implements IDatabase {
    public void connect() {
        System.out.println("Connected to MySQL");
    }
    
    public String getData() {
        return "Data from MySQL";
    }
}

class PostgreSQLDatabase implements IDatabase {
    public void connect() {
        System.out.println("Connected to PostgreSQL");
    }
    
    public String getData() {
        return "Data from PostgreSQL";
    }
}

// High-level module depends on abstraction
class UserService {
    private IDatabase database;
    
    public UserService(IDatabase db) {
        this.database = db;
    }
    
    public String getUser() {
        database.connect();
        return database.getData();
    }
}

// Usage
IDatabase db = new MySQLDatabase();
UserService service = new UserService(db);
```

#### C#
```csharp
// Abstraction
public interface IDatabase {
    void Connect();
    string GetData();
}

// Implementations
public class MySQLDatabase : IDatabase {
    public void Connect() {
        Console.WriteLine("Connected to MySQL");
    }
    
    public string GetData() {
        return "Data from MySQL";
    }
}

public class PostgreSQLDatabase : IDatabase {
    public void Connect() {
        Console.WriteLine("Connected to PostgreSQL");
    }
    
    public string GetData() {
        return "Data from PostgreSQL";
    }
}

// High-level class
public class UserService {
    private readonly IDatabase _database;
    
    public UserService(IDatabase database) {
        _database = database;
    }
    
    public string GetUser() {
        _database.Connect();
        return _database.GetData();
    }
}
```

---

## 🏭 CREATIONAL PATTERNS

### 1. Singleton Pattern

**Purpose:** Ensure a class has only one instance and provide a global point of access to it.

**Visual:**
```
┌─────────────────────────┐
│   Singleton Class       │
├─────────────────────────┤
│ - static instance       │
│ - private constructor   │
├─────────────────────────┤
│ + static getInstance()  │
└─────────────────────────┘
```

#### Python
```python
class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
    
    def __init__(self):
        self.value = None

# Alternative: Using decorator
def singleton(cls):
    instances = {}
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class DatabaseConnection:
    def __init__(self):
        self.connection = "Connected"

# Usage
db1 = DatabaseConnection()
db2 = DatabaseConnection()
print(db1 is db2)  # True - same instance
```

#### Java
```java
// Thread-safe Singleton
public class Singleton {
    // Volatile ensures visibility across threads
    private static volatile Singleton instance;
    
    private Singleton() {
        // Private constructor
    }
    
    // Double-checked locking
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// Alternative: Enum Singleton (Best practice)
public enum SingletonEnum {
    INSTANCE;
    
    public void doSomething() {
        System.out.println("Doing something");
    }
}
```

#### C++
```cpp
class Singleton {
private:
    static Singleton* instance;
    
    // Private constructor
    Singleton() {}
    
    // Prevent copying
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

public:
    static Singleton* getInstance() {
        if (instance == nullptr) {
            instance = new Singleton();
        }
        return instance;
    }
};

// Initialize static member
Singleton* Singleton::instance = nullptr;

// Modern C++11 approach (thread-safe)
class ModernSingleton {
public:
    static ModernSingleton& getInstance() {
        static ModernSingleton instance;  // Thread-safe in C++11
        return instance;
    }
    
private:
    ModernSingleton() {}
    
public:
    ModernSingleton(const ModernSingleton&) = delete;
    void operator=(const ModernSingleton&) = delete;
};
```

#### C#
```csharp
// Thread-safe Singleton
public sealed class Singleton {
    private static readonly Lazy<Singleton> instance = 
        new Lazy<Singleton>(() => new Singleton());
    
    private Singleton() {
        // Private constructor
    }
    
    public static Singleton Instance {
        get {
            return instance.Value;
        }
    }
}

// Alternative: Static initialization
public sealed class SingletonStatic {
    private static readonly SingletonStatic instance = new SingletonStatic();
    
    static SingletonStatic() {
        // Static constructor
    }
    
    private SingletonStatic() {
    }
    
    public static SingletonStatic Instance {
        get {
            return instance;
        }
    }
}
```

---

### 2. Factory Pattern

**Purpose:** Define an interface for creating objects, but let subclasses decide which class to instantiate.

**Visual:**
```
     ┌──────────────┐
     │   Creator    │
     │  (abstract)  │
     └───────┬──────┘
             │
     ┌───────┴──────────┐
     │                  │
┌────▼────┐      ┌─────▼─────┐
│Creator A│      │Creator B  │
└─────────┘      └───────────┘
```

#### Python
```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class Bird(Animal):
    def speak(self):
        return "Chirp!"

class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        elif animal_type == "bird":
            return Bird()
        else:
            raise ValueError(f"Unknown animal type: {animal_type}")

# Usage
factory = AnimalFactory()
dog = factory.create_animal("dog")
cat = factory.create_animal("cat")
print(dog.speak())  # Woof!
print(cat.speak())  # Meow!
```

#### Java
```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

class Rectangle implements Shape {
    public void draw() {
        System.out.println("Drawing Rectangle");
    }
}

class Triangle implements Shape {
    public void draw() {
        System.out.println("Drawing Triangle");
    }
}

class ShapeFactory {
    public Shape getShape(String shapeType) {
        if (shapeType == null) {
            return null;
        }
        
        switch (shapeType.toLowerCase()) {
            case "circle":
                return new Circle();
            case "rectangle":
                return new Rectangle();
            case "triangle":
                return new Triangle();
            default:
                throw new IllegalArgumentException("Unknown shape: " + shapeType);
        }
    }
}

// Usage
ShapeFactory factory = new ShapeFactory();
Shape circle = factory.getShape("circle");
circle.draw();  // Drawing Circle
```

---

### 3. Observer Pattern

**Purpose:** Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.

**Visual:**
```
   ┌──────────┐
   │ Subject  │
   └────┬─────┘
        │ notifies
   ┌────┴──────────────┐
   │    │    │         │
┌──▼──┐ │    │    ┌───▼───┐
│Obs 1│ │    │    │ Obs N │
└─────┘ │    │    └───────┘
     ┌──▼──┐ │
     │Obs 2│ │
     └─────┘ │
          ┌──▼──┐
          │Obs 3│
          └─────┘
```

#### Python
```python
from abc import ABC, abstractmethod

class Observer(ABC):
    @abstractmethod
    def update(self, message):
        pass

class Subject:
    def __init__(self):
        self._observers = []
    
    def attach(self, observer):
        self._observers.append(observer)
    
    def detach(self, observer):
        self._observers.remove(observer)
    
    def notify(self, message):
        for observer in self._observers:
            observer.update(message)

class EmailObserver(Observer):
    def update(self, message):
        print(f"Email notification: {message}")

class SMSObserver(Observer):
    def update(self, message):
        print(f"SMS notification: {message}")

class PushObserver(Observer):
    def update(self, message):
        print(f"Push notification: {message}")

# Usage
subject = Subject()
email = EmailObserver()
sms = SMSObserver()
push = PushObserver()

subject.attach(email)
subject.attach(sms)
subject.attach(push)

subject.notify("New message arrived!")
# Output:
# Email notification: New message arrived!
# SMS notification: New message arrived!
# Push notification: New message arrived!
```

#### Java
```java
import java.util.*;

interface Observer {
    void update(String message);
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

class EmailObserver implements Observer {
    public void update(String message) {
        System.out.println("Email: " + message);
    }
}

class SMSObserver implements Observer {
    public void update(String message) {
        System.out.println("SMS: " + message);
    }
}

// Usage
Subject subject = new Subject();
subject.attach(new EmailObserver());
subject.attach(new SMSObserver());
subject.notifyObservers("Order shipped!");
```

---

### 4. Strategy Pattern

**Purpose:** Define a family of algorithms, encapsulate each one, and make them interchangeable.

#### Python
```python
from abc import ABC, abstractmethod

class PaymentStrategy(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

class CreditCardPayment(PaymentStrategy):
    def __init__(self, card_number):
        self.card_number = card_number
    
    def pay(self, amount):
        return f"Paid ${amount} using Credit Card {self.card_number}"

class PayPalPayment(PaymentStrategy):
    def __init__(self, email):
        self.email = email
    
    def pay(self, amount):
        return f"Paid ${amount} using PayPal {self.email}"

class BitcoinPayment(PaymentStrategy):
    def __init__(self, wallet_address):
        self.wallet_address = wallet_address
    
    def pay(self, amount):
        return f"Paid ${amount} using Bitcoin {self.wallet_address}"

class ShoppingCart:
    def __init__(self, payment_strategy: PaymentStrategy):
        self.payment_strategy = payment_strategy
        self.total = 0
    
    def add_item(self, price):
        self.total += price
    
    def checkout(self):
        return self.payment_strategy.pay(self.total)

# Usage
cart1 = ShoppingCart(CreditCardPayment("1234-5678"))
cart1.add_item(100)
cart1.add_item(50)
print(cart1.checkout())  # Paid $150 using Credit Card 1234-5678

cart2 = ShoppingCart(PayPalPayment("user@email.com"))
cart2.add_item(200)
print(cart2.checkout())  # Paid $200 using PayPal user@email.com
```

---

### 5. Decorator Pattern

**Purpose:** Attach additional responsibilities to an object dynamically.

#### Python
```python
from abc import ABC, abstractmethod

class Coffee(ABC):
    @abstractmethod
    def cost(self):
        pass
    
    @abstractmethod
    def description(self):
        pass

class SimpleCoffee(Coffee):
    def cost(self):
        return 5
    
    def description(self):
        return "Simple Coffee"

class CoffeeDecorator(Coffee):
    def __init__(self, coffee):
        self._coffee = coffee
    
    def cost(self):
        return self._coffee.cost()
    
    def description(self):
        return self._coffee.description()

class Milk(CoffeeDecorator):
    def cost(self):
        return self._coffee.cost() + 1
    
    def description(self):
        return self._coffee.description() + ", Milk"

class Sugar(CoffeeDecorator):
    def cost(self):
        return self._coffee.cost() + 0.5
    
    def description(self):
        return self._coffee.description() + ", Sugar"

class WhippedCream(CoffeeDecorator):
    def cost(self):
        return self._coffee.cost() + 2
    
    def description(self):
        return self._coffee.description() + ", Whipped Cream"

# Usage
coffee = SimpleCoffee()
print(f"{coffee.description()} = ${coffee.cost()}")
# Simple Coffee = $5

coffee_with_milk = Milk(coffee)
print(f"{coffee_with_milk.description()} = ${coffee_with_milk.cost()}")
# Simple Coffee, Milk = $6

fancy_coffee = WhippedCream(Sugar(Milk(SimpleCoffee())))
print(f"{fancy_coffee.description()} = ${fancy_coffee.cost()}")
# Simple Coffee, Milk, Sugar, Whipped Cream = $8.5
```

#### Java
```java
interface Coffee {
    double cost();
    String description();
}

class SimpleCoffee implements Coffee {
    public double cost() {
        return 5.0;
    }
    
    public String description() {
        return "Simple Coffee";
    }
}

abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
    
    public double cost() {
        return coffee.cost();
    }
    
    public String description() {
        return coffee.description();
    }
}

class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }
    
    public double cost() {
        return coffee.cost() + 1.0;
    }
    
    public String description() {
        return coffee.description() + ", Milk";
    }
}

class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }
    
    public double cost() {
        return coffee.cost() + 0.5;
    }
    
    public String description() {
        return coffee.description() + ", Sugar";
    }
}

// Usage
Coffee coffee = new SimpleCoffee();
coffee = new Milk(coffee);
coffee = new Sugar(coffee);
System.out.println(coffee.description() + " = $" + coffee.cost());
// Simple Coffee, Milk, Sugar = $6.5
```

---

## ❌ ANTI-PATTERNS TO AVOID

### 1. God Object

**Problem:** A class that knows too much or does too much.

```python
# ❌ Bad - God Object
class Application:
    def __init__(self):
        self.database = None
        self.ui = None
        self.network = None
    
    def connect_database(self): pass
    def render_ui(self): pass
    def send_request(self): pass
    def process_data(self): pass
    def validate_input(self): pass
    def generate_report(self): pass
    def send_email(self): pass
    def log_activity(self): pass
    # ... 50 more methods
```

**Fix:** Follow Single Responsibility Principle - split into multiple classes.

### 2. Spaghetti Code

**Problem:** Code with complex and tangled control structures.

```python
# ❌ Bad
def process_order(order):
    if order.status == "pending":
        if order.payment == "paid":
            if order.items > 0:
                if order.customer.verified:
                    if order.shipping_address:
                        # Process order
                        pass
```

**Fix:** Use early returns and clear logic flow.

```python
# ✅ Good
def process_order(order):
    if order.status != "pending":
        return "Order not pending"
    
    if order.payment != "paid":
        return "Payment not received"
    
    if order.items <= 0:
        return "No items in order"
    
    if not order.customer.verified:
        return "Customer not verified"
    
    if not order.shipping_address:
        return "No shipping address"
    
    # Process order
    return "Order processed"
```

### 3. Tight Coupling

**Problem:** Classes depend directly on concrete implementations.

```python
# ❌ Bad
class EmailService:
    def send(self, message):
        print(f"Sending email: {message}")

class NotificationManager:
    def __init__(self):
        self.email_service = EmailService()  # Tight coupling
    
    def notify(self, message):
        self.email_service.send(message)
```

**Fix:** Depend on abstractions (Dependency Inversion).

### 4. Magic Numbers

**Problem:** Using literal values without explanation.

```python
# ❌ Bad
def calculate_discount(price):
    if price > 1000:
        return price * 0.1
    elif price > 500:
        return price * 0.05
    return 0

# ✅ Good
LARGE_ORDER_THRESHOLD = 1000
MEDIUM_ORDER_THRESHOLD = 500
LARGE_ORDER_DISCOUNT_RATE = 0.1
MEDIUM_ORDER_DISCOUNT_RATE = 0.05

def calculate_discount(price):
    if price > LARGE_ORDER_THRESHOLD:
        return price * LARGE_ORDER_DISCOUNT_RATE
    elif price > MEDIUM_ORDER_THRESHOLD:
        return price * MEDIUM_ORDER_DISCOUNT_RATE
    return 0
```

### 5. Copy-Paste Programming

**Problem:** Duplicating code instead of reusing.

```python
# ❌ Bad
def calculate_area_circle(radius):
    return 3.14159 * radius * radius

def calculate_area_sphere(radius):
    return 4 * 3.14159 * radius * radius

# ✅ Good
PI = 3.14159

def calculate_circle_area(radius):
    return PI * radius ** 2

def calculate_sphere_surface_area(radius):
    return 4 * PI * radius ** 2
```


---

## 🎯 TRICKY MCQs (10 Questions)

### Q1: Singleton Pattern
```python
class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

s1 = Singleton()
s2 = Singleton()
s1.value = 10
print(s2.value)
```

**What is the output?**
A) Error  
B) None  
C) 10  
D) Undefined

**Answer: C) 10**

**Explanation:** Both s1 and s2 refer to the same instance. Setting s1.value affects s2.value because they're the same object.

---

### Q2: SOLID - SRP Violation
```java
class Employee {
    private String name;
    private double salary;
    
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
    
    public double calculatePay() {
        return salary * 1.1;
    }
    
    public void save() {
        // Save to database
    }
    
    public String generateReport() {
        return "Report for " + name;
    }
}
```

**Which SOLID principle is violated?**
A) Open/Closed Principle  
B) Single Responsibility Principle  
C) Liskov Substitution Principle  
D) Interface Segregation Principle

**Answer: B) Single Responsibility Principle**

**Explanation:** The Employee class has multiple responsibilities: pay calculation, persistence, and reporting. Each is a separate reason to change.

---

### Q3: Factory Pattern
```python
class AnimalFactory:
    @staticmethod
    def create(animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        return None

class Dog:
    def speak(self):
        return "Woof"

class Cat:
    def speak(self):
        return "Meow"

factory = AnimalFactory()
animal = factory.create("bird")
print(animal.speak())
```

**What happens?**
A) Prints "Chirp"  
B) Prints None  
C) AttributeError: 'NoneType' object has no attribute 'speak'  
D) Prints empty string

**Answer: C) AttributeError: 'NoneType' object has no attribute 'speak'**

**Explanation:** Factory returns None for unknown types. Calling speak() on None raises AttributeError.

---

### Q4: Liskov Substitution Principle
```java
class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int w) { width = w; }
    public void setHeight(int h) { height = h; }
    
    public int area() {
        return width * height;
    }
}

class Square extends Rectangle {
    public void setWidth(int w) {
        width = w;
        height = w;
    }
    
    public void setHeight(int h) {
        width = h;
        height = h;
    }
}

public class Test {
    public static void main(String[] args) {
        Rectangle rect = new Square();
        rect.setWidth(5);
        rect.setHeight(10);
        System.out.println(rect.area());
    }
}
```

**What is the output?**
A) 50  
B) 100  
C) 25  
D) Error

**Answer: B) 100**

**Explanation:** Square overrides both setWidth and setHeight to maintain square property. Last call sets both to 10, so area = 10 * 10 = 100. This violates LSP because Rectangle's behavior is not preserved.

---

### Q5: Observer Pattern
```python
class Subject:
    def __init__(self):
        self.observers = []
    
    def attach(self, observer):
        self.observers.append(observer)
    
    def notify(self, msg):
        for obs in self.observers:
            obs.update(msg)

class Observer:
    def __init__(self, name):
        self.name = name
    
    def update(self, msg):
        print(f"{self.name}: {msg}")

subject = Subject()
o1 = Observer("O1")
o2 = Observer("O2")

subject.attach(o1)
subject.attach(o2)
subject.notify("Hello")
subject.observers.clear()
subject.notify("World")
```

**What is the output?**
A) O1: Hello, O2: Hello, O1: World, O2: World  
B) O1: Hello, O2: Hello  
C) Error  
D) O1: World, O2: World

**Answer: B) O1: Hello, O2: Hello**

**Explanation:** First notify triggers both observers. observers.clear() removes all observers, so second notify has no effect.

---

### Q6: Dependency Inversion Principle
```csharp
class MySQLDatabase {
    public string GetData() {
        return "MySQL data";
    }
}

class DataService {
    private MySQLDatabase db = new MySQLDatabase();
    
    public string FetchData() {
        return db.GetData();
    }
}
```

**What's the problem with this design?**
A) No abstraction - violates DIP  
B) Too many classes  
C) Missing error handling  
D) No problem

**Answer: A) No abstraction - violates DIP**

**Explanation:** DataService depends directly on concrete MySQLDatabase class. Should depend on an interface/abstraction to allow easy switching to other databases.

---

### Q7: Strategy Pattern
```java
interface Strategy {
    int execute(int a, int b);
}

class Add implements Strategy {
    public int execute(int a, int b) { return a + b; }
}

class Multiply implements Strategy {
    public int execute(int a, int b) { return a * b; }
}

class Context {
    private Strategy strategy;
    
    public Context(Strategy s) {
        strategy = s;
    }
    
    public int doOperation(int a, int b) {
        return strategy.execute(a, b);
    }
}

public class Test {
    public static void main(String[] args) {
        Context ctx = new Context(new Add());
        System.out.println(ctx.doOperation(5, 3));
        
        ctx = new Context(new Multiply());
        System.out.println(ctx.doOperation(5, 3));
    }
}
```

**What is the output?**
A) 8, 15  
B) 15, 8  
C) 8, 8  
D) 15, 15

**Answer: A) 8, 15**

**Explanation:** First context uses Add strategy: 5+3=8. Second context uses Multiply strategy: 5*3=15.

---

### Q8: Decorator Pattern
```python
class Component:
    def operation(self):
        return "Component"

class DecoratorA:
    def __init__(self, component):
        self.component = component
    
    def operation(self):
        return f"A({self.component.operation()})"

class DecoratorB:
    def __init__(self, component):
        self.component = component
    
    def operation(self):
        return f"B({self.component.operation()})"

comp = Component()
decorated = DecoratorB(DecoratorA(comp))
print(decorated.operation())
```

**What is the output?**
A) A(B(Component))  
B) B(A(Component))  
C) Component  
D) B(Component)

**Answer: B) B(A(Component))**

**Explanation:** DecoratorA wraps Component, returning "A(Component)". DecoratorB wraps that, returning "B(A(Component))".

---

### Q9: Open/Closed Principle
```python
class Shape:
    def __init__(self, type):
        self.type = type

class AreaCalculator:
    def calculate(self, shape):
        if shape.type == "circle":
            return 3.14 * shape.radius ** 2
        elif shape.type == "rectangle":
            return shape.length * shape.width
        # Need to modify to add new shapes
```

**What principle is violated?**
A) Single Responsibility  
B) Open/Closed  
C) Liskov Substitution  
D) Dependency Inversion

**Answer: B) Open/Closed**

**Explanation:** Class is not closed for modification - must change calculate() to add new shapes. Should use polymorphism instead.

---

### Q10: Interface Segregation Principle
```java
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Human implements Worker {
    public void work() { System.out.println("Working"); }
    public void eat() { System.out.println("Eating"); }
    public void sleep() { System.out.println("Sleeping"); }
}

class Robot implements Worker {
    public void work() { System.out.println("Working"); }
    public void eat() { throw new UnsupportedOperationException(); }
    public void sleep() { throw new UnsupportedOperationException(); }
}
```

**Which principle is violated?**
A) Single Responsibility  
B) Open/Closed  
C) Interface Segregation  
D) Dependency Inversion

**Answer: C) Interface Segregation**

**Explanation:** Robot is forced to implement eat() and sleep() methods it doesn't need. Interface should be split into smaller, more specific interfaces.

---

## 📊 Summary Tables

### Design Patterns Categories

| Category | Patterns | Purpose |
|----------|----------|---------|
| **Creational** | Singleton, Factory, Builder, Prototype | Object creation |
| **Structural** | Decorator, Adapter, Facade, Proxy | Object composition |
| **Behavioral** | Observer, Strategy, Command, State | Object interaction |

### SOLID Principles Quick Reference

| Principle | Means | Violation Sign |
|-----------|-------|----------------|
| **S**RP | One class, one responsibility | Class has multiple reasons to change |
| **O**CP | Open for extension, closed for modification | Must modify existing code to add features |
| **L**SP | Subtypes must be substitutable | Subclass breaks parent's behavior |
| **I**SP | Many specific interfaces > one general | Interface has methods not used by all implementers |
| **D**IP | Depend on abstractions, not concretions | High-level modules depend on low-level modules |

### When to Use Which Pattern

| Use Case | Pattern |
|----------|---------|
| Need exactly one instance | Singleton |
| Create objects without specifying exact class | Factory |
| Add responsibilities dynamically | Decorator |
| Define family of interchangeable algorithms | Strategy |
| Notify multiple objects of state changes | Observer |
| Adapt incompatible interfaces | Adapter |
| Simplify complex subsystem | Facade |

---

## 🎓 Key Takeaways

1. **SOLID principles** guide good OOP design
2. **Design patterns** provide proven solutions to common problems
3. **Singleton** ensures single instance across application
4. **Factory** abstracts object creation logic
5. **Observer** enables loose coupling between objects
6. **Strategy** makes algorithms interchangeable
7. **Decorator** adds behavior without modifying original class
8. **Avoid anti-patterns** like God Object and tight coupling
9. **Follow SRP** - each class should have one responsibility
10. **Depend on abstractions**, not concrete implementations

---

**Real-World Applications:**

- **Singleton**: Database connections, loggers, configuration managers
- **Factory**: GUI component creation, document parsers
- **Observer**: Event handling systems, MVC frameworks
- **Strategy**: Payment processing, sorting algorithms
- **Decorator**: Input/output streams, UI components

---

**End of Document** 🎯
