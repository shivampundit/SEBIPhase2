# 🎯 OOP: Encapsulation, Polymorphism & Inheritance - Deep Dive

## 📚 Table of Contents
1. [Encapsulation](#encapsulation)
2. [Access Modifiers](#access-modifiers)
3. [Polymorphism](#polymorphism)
4. [Inheritance](#inheritance)
5. [Tricky MCQs](#tricky-mcqs)
6. [Common Pitfalls](#common-pitfalls)

---

## 🔒 ENCAPSULATION

### Definition
**Encapsulation** is the bundling of data (variables) and methods that operate on that data within a single unit (class), and restricting direct access to some of the object's components.

### Visual Representation
```
┌─────────────────────────────────┐
│         Class (Capsule)         │
├─────────────────────────────────┤
│  Private Data                   │
│  - name                         │
│  - age                          │
│  - salary                       │
├─────────────────────────────────┤
│  Public Methods (Interface)     │
│  + getName()                    │
│  + setName()                    │
│  + calculateBonus()             │
└─────────────────────────────────┘
```

### 1. Python Implementation

```python
class BankAccount:
    def __init__(self, account_number, balance):
        self.__account_number = account_number  # Private (name mangling)
        self.__balance = balance                # Private
        self._transaction_fee = 2.5             # Protected (convention)
    
    # Getter methods
    def get_balance(self):
        return self.__balance
    
    # Setter methods with validation
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return True
        return False
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return True
        return False
    
    # Property decorator (Pythonic way)
    @property
    def account_number(self):
        return self.__account_number
    
    @account_number.setter
    def account_number(self, value):
        raise ValueError("Cannot modify account number")

# Usage
account = BankAccount("ACC123", 1000)
print(account.get_balance())           # 1000
account.deposit(500)                   # 1500
print(account.account_number)          # ACC123 (via property)

# Direct access fails (name mangling)
# print(account.__balance)             # AttributeError
print(account._BankAccount__balance)   # 1500 (can still access - not truly private)
```

**Python Encapsulation Levels:**
- `public`: accessible everywhere
- `_protected`: convention, accessible but shouldn't be used externally
- `__private`: name mangling, harder to access but not impossible

### 2. Java Implementation

```java
public class BankAccount {
    // Private instance variables
    private String accountNumber;
    private double balance;
    protected double transactionFee;  // Accessible in subclasses
    
    // Constructor
    public BankAccount(String accountNumber, double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
        this.transactionFee = 2.5;
    }
    
    // Public getter
    public double getBalance() {
        return balance;
    }
    
    // Public getter
    public String getAccountNumber() {
        return accountNumber;
    }
    
    // Public method with validation
    public boolean deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            return true;
        }
        return false;
    }
    
    // Public method with business logic
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
    
    // Private helper method
    private void logTransaction(String type, double amount) {
        System.out.println(type + ": $" + amount);
    }
}

// Usage
class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("ACC123", 1000);
        System.out.println(account.getBalance());  // 1000
        account.deposit(500);                      // 1500
        // account.balance = 9999;                 // Compilation Error!
    }
}
```

### 3. C++ Implementation

```cpp
#include <iostream>
#include <string>
using namespace std;

class BankAccount {
private:
    string accountNumber;
    double balance;
    
    // Private helper method
    void logTransaction(string type, double amount) {
        cout << type << ": $" << amount << endl;
    }

protected:
    double transactionFee;

public:
    // Constructor
    BankAccount(string accNum, double bal) 
        : accountNumber(accNum), balance(bal), transactionFee(2.5) {}
    
    // Public getter
    double getBalance() const {
        return balance;
    }
    
    // Public getter
    string getAccountNumber() const {
        return accountNumber;
    }
    
    // Public method with validation
    bool deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            logTransaction("Deposit", amount);
            return true;
        }
        return false;
    }
    
    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            logTransaction("Withdrawal", amount);
            return true;
        }
        return false;
    }
    
    // Friend function (breaks encapsulation deliberately)
    friend void displayAccountDetails(const BankAccount& acc);
};

// Friend function can access private members
void displayAccountDetails(const BankAccount& acc) {
    cout << "Account: " << acc.accountNumber 
         << ", Balance: $" << acc.balance << endl;
}

int main() {
    BankAccount account("ACC123", 1000);
    cout << account.getBalance() << endl;   // 1000
    account.deposit(500);                   // 1500
    displayAccountDetails(account);
    // cout << account.balance;             // Compilation Error!
    return 0;
}
```

### 4. C# Implementation

```csharp
using System;

public class BankAccount {
    // Private fields (backing fields)
    private string accountNumber;
    private double balance;
    protected double transactionFee;
    
    // Constructor
    public BankAccount(string accountNumber, double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
        this.transactionFee = 2.5;
    }
    
    // Properties (C# way - combines getter/setter)
    public double Balance {
        get { return balance; }
        private set { balance = value; }  // Private setter
    }
    
    // Read-only property
    public string AccountNumber {
        get { return accountNumber; }
    }
    
    // Auto-implemented property
    public DateTime CreatedDate { get; private set; } = DateTime.Now;
    
    // Public methods
    public bool Deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            return true;
        }
        return false;
    }
    
    public bool Withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
    
    // Private helper method
    private void LogTransaction(string type, double amount) {
        Console.WriteLine($"{type}: ${amount}");
    }
}

// Usage
class Program {
    static void Main() {
        BankAccount account = new BankAccount("ACC123", 1000);
        Console.WriteLine(account.Balance);     // 1000
        account.Deposit(500);                   // 1500
        // account.balance = 9999;              // Compilation Error!
        // account.Balance = 9999;              // Compilation Error (private setter)
    }
}
```

---

## 🔑 ACCESS MODIFIERS COMPARISON

### Comprehensive Access Modifiers Table

| Access Level | Python | Java | C++ | C# | Description |
|-------------|--------|------|-----|-----|-------------|
| **Public** | `name` | `public` | `public` | `public` | Accessible everywhere |
| **Protected** | `_name` | `protected` | `protected` | `protected` | Accessible in class & subclasses |
| **Private** | `__name` | `private` | `private` | `private` | Accessible only within class |
| **Package/Internal** | N/A | default | N/A | `internal` | Same package/assembly only |
| **Protected Internal** | N/A | N/A | N/A | `protected internal` | Protected OR Internal (union) |
| **Private Protected** | N/A | N/A | N/A | `private protected` | Protected AND Internal (intersection) |

### Access Modifiers Behavior Chart

```
┌─────────────────────────────────────────────────────────┐
│              VISIBILITY SCOPE DIAGRAM                    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  PUBLIC          ████████████████████████████████████   │
│  (Everywhere)                                            │
│                                                          │
│  PROTECTED       ████████████████████                    │
│  (Class + Subclass + Package*)                           │
│                                                          │
│  DEFAULT/PKG     ████████████                            │
│  (Same Package)                                          │
│                                                          │
│  PRIVATE         ████                                    │
│  (Same Class)                                            │
│                                                          │
└─────────────────────────────────────────────────────────┘
* Language dependent
```

### Detailed Access Matrix

| Modifier | Same Class | Same Package | Subclass (Same Pkg) | Subclass (Diff Pkg) | Different Package |
|----------|------------|--------------|---------------------|---------------------|-------------------|
| `public` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `protected` (Java) | ✅ | ✅ | ✅ | ✅ | ❌ |
| `default` (Java) | ✅ | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ | ❌ |
| `protected` (C++) | ✅ | ❌ | ✅ | ✅ | ❌ |

### Edge Cases and Tricky Points

**1. Python Name Mangling:**
```python
class Test:
    def __init__(self):
        self.__private = 10
        self._protected = 20

obj = Test()
print(obj._Test__private)  # 10 - Can still access!
print(obj._protected)       # 20 - Just convention
```

**2. Java Protected Access:**
```java
package pkg1;
public class Parent {
    protected int value = 10;
}

package pkg2;
import pkg1.Parent;
public class Child extends Parent {
    void test() {
        System.out.println(value);        // ✅ OK - inherited
        Parent p = new Parent();
        // System.out.println(p.value);   // ❌ Error - different package
    }
}
```

**3. C++ Friend Classes:**
```cpp
class A {
private:
    int secret = 42;
    friend class B;  // B can access A's private members
};

class B {
public:
    void reveal(A& a) {
        cout << a.secret;  // ✅ OK - friend class
    }
};
```

---

## 🎭 POLYMORPHISM

### Definition
**Polymorphism** means "many forms" - the ability of objects to take on different forms or behave differently based on context.

### Types of Polymorphism

```
Polymorphism
    ├── Compile-Time (Static) Polymorphism
    │   ├── Method Overloading
    │   ├── Operator Overloading
    │   └── Constructor Overloading
    │
    └── Run-Time (Dynamic) Polymorphism
        ├── Method Overriding
        └── Virtual Functions
```

---

## 📝 COMPILE-TIME POLYMORPHISM

### 1. Method Overloading (Same name, different parameters)

#### Python (No true overloading, uses default arguments)
```python
class Calculator:
    def add(self, a, b=0, c=0):
        return a + b + c
    
    # Alternative: Using *args
    def multiply(self, *args):
        result = 1
        for num in args:
            result *= num
        return result

calc = Calculator()
print(calc.add(5))           # 5
print(calc.add(5, 10))       # 15
print(calc.add(5, 10, 15))   # 30
print(calc.multiply(2, 3, 4)) # 24
```

#### Java
```java
class Calculator {
    // Overloaded methods
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public String add(String a, String b) {
        return a + b;
    }
}

class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(5, 10));        // 15 (int version)
        System.out.println(calc.add(5, 10, 15));    // 30 (three params)
        System.out.println(calc.add(5.5, 10.5));    // 16.0 (double version)
        System.out.println(calc.add("Hello", "World")); // HelloWorld
    }
}
```

#### C++
```cpp
#include <iostream>
#include <string>
using namespace std;

class Calculator {
public:
    // Method overloading
    int add(int a, int b) {
        return a + b;
    }
    
    int add(int a, int b, int c) {
        return a + b + c;
    }
    
    double add(double a, double b) {
        return a + b;
    }
    
    string add(string a, string b) {
        return a + b;
    }
};

int main() {
    Calculator calc;
    cout << calc.add(5, 10) << endl;          // 15
    cout << calc.add(5, 10, 15) << endl;      // 30
    cout << calc.add(5.5, 10.5) << endl;      // 16.0
    cout << calc.add("Hello", "World") << endl; // HelloWorld
    return 0;
}
```

#### C#
```csharp
class Calculator {
    public int Add(int a, int b) {
        return a + b;
    }
    
    public int Add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double Add(double a, double b) {
        return a + b;
    }
    
    // Named parameters
    public int Add(int a = 0, int b = 0, int c = 0) {
        return a + b + c;
    }
}
```

### 2. Operator Overloading

#### Python
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):  # Overload + operator
        return Point(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):  # Overload - operator
        return Point(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):  # Overload * operator
        return Point(self.x * scalar, self.y * scalar)
    
    def __str__(self):
        return f"({self.x}, {self.y})"
    
    def __eq__(self, other):  # Overload == operator
        return self.x == other.x and self.y == other.y

p1 = Point(1, 2)
p2 = Point(3, 4)
p3 = p1 + p2
print(p3)  # (4, 6)
```

#### C++
```cpp
class Point {
private:
    int x, y;
public:
    Point(int x = 0, int y = 0) : x(x), y(y) {}
    
    // Overload + operator
    Point operator+(const Point& other) const {
        return Point(x + other.x, y + other.y);
    }
    
    // Overload - operator
    Point operator-(const Point& other) const {
        return Point(x - other.x, y - other.y);
    }
    
    // Overload * operator (scalar)
    Point operator*(int scalar) const {
        return Point(x * scalar, y * scalar);
    }
    
    // Overload == operator
    bool operator==(const Point& other) const {
        return x == other.x && y == other.y;
    }
    
    // Overload << for cout
    friend ostream& operator<<(ostream& os, const Point& p) {
        os << "(" << p.x << ", " << p.y << ")";
        return os;
    }
};
```

#### C#
```csharp
class Point {
    public int X { get; set; }
    public int Y { get; set; }
    
    public Point(int x, int y) {
        X = x;
        Y = y;
    }
    
    // Overload + operator
    public static Point operator +(Point p1, Point p2) {
        return new Point(p1.X + p2.X, p1.Y + p2.Y);
    }
    
    // Overload - operator
    public static Point operator -(Point p1, Point p2) {
        return new Point(p1.X - p2.X, p1.Y - p2.Y);
    }
    
    // Overload * operator
    public static Point operator *(Point p, int scalar) {
        return new Point(p.X * scalar, p.Y * scalar);
    }
    
    // Overload == operator
    public static bool operator ==(Point p1, Point p2) {
        return p1.X == p2.X && p1.Y == p2.Y;
    }
    
    public static bool operator !=(Point p1, Point p2) {
        return !(p1 == p2);
    }
}
```

---

## 🔄 RUN-TIME POLYMORPHISM

### Method Overriding (Same signature in parent and child)

#### Python
```python
class Animal:
    def speak(self):
        return "Some sound"
    
    def move(self):
        return "Moving"

class Dog(Animal):
    def speak(self):  # Override
        return "Woof!"
    
    def move(self):  # Override
        return "Running on four legs"

class Cat(Animal):
    def speak(self):  # Override
        return "Meow!"

# Polymorphic behavior
def make_animal_speak(animal):
    print(animal.speak())

dog = Dog()
cat = Cat()
animal = Animal()

make_animal_speak(dog)     # Woof!
make_animal_speak(cat)     # Meow!
make_animal_speak(animal)  # Some sound

# List of different animals
animals = [Dog(), Cat(), Animal()]
for animal in animals:
    print(animal.speak())
# Output:
# Woof!
# Meow!
# Some sound
```

#### Java
```java
class Animal {
    public void speak() {
        System.out.println("Some sound");
    }
    
    public void move() {
        System.out.println("Moving");
    }
}

class Dog extends Animal {
    @Override
    public void speak() {
        System.out.println("Woof!");
    }
    
    @Override
    public void move() {
        System.out.println("Running on four legs");
    }
}

class Cat extends Animal {
    @Override
    public void speak() {
        System.out.println("Meow!");
    }
}

class Main {
    // Polymorphic method
    public static void makeAnimalSpeak(Animal animal) {
        animal.speak();
    }
    
    public static void main(String[] args) {
        Animal dog = new Dog();    // Upcasting
        Animal cat = new Cat();    // Upcasting
        Animal animal = new Animal();
        
        makeAnimalSpeak(dog);      // Woof!
        makeAnimalSpeak(cat);      // Meow!
        makeAnimalSpeak(animal);   // Some sound
        
        // Array of different animals
        Animal[] animals = {new Dog(), new Cat(), new Animal()};
        for (Animal a : animals) {
            a.speak();
        }
    }
}
```

#### C++ (Virtual Functions)
```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    // Virtual function - enables runtime polymorphism
    virtual void speak() {
        cout << "Some sound" << endl;
    }
    
    virtual void move() {
        cout << "Moving" << endl;
    }
    
    // Virtual destructor (important!)
    virtual ~Animal() {
        cout << "Animal destructor" << endl;
    }
};

class Dog : public Animal {
public:
    void speak() override {  // override keyword (C++11)
        cout << "Woof!" << endl;
    }
    
    void move() override {
        cout << "Running on four legs" << endl;
    }
    
    ~Dog() {
        cout << "Dog destructor" << endl;
    }
};

class Cat : public Animal {
public:
    void speak() override {
        cout << "Meow!" << endl;
    }
    
    ~Cat() {
        cout << "Cat destructor" << endl;
    }
};

void makeAnimalSpeak(Animal* animal) {
    animal->speak();
}

int main() {
    Animal* dog = new Dog();    // Pointer to base class
    Animal* cat = new Cat();
    Animal* animal = new Animal();
    
    makeAnimalSpeak(dog);       // Woof!
    makeAnimalSpeak(cat);       // Meow!
    makeAnimalSpeak(animal);    // Some sound
    
    // Clean up
    delete dog;    // Calls Dog destructor, then Animal destructor
    delete cat;
    delete animal;
    
    return 0;
}
```

#### C#
```csharp
using System;

class Animal {
    // Virtual method - can be overridden
    public virtual void Speak() {
        Console.WriteLine("Some sound");
    }
    
    public virtual void Move() {
        Console.WriteLine("Moving");
    }
}

class Dog : Animal {
    public override void Speak() {
        Console.WriteLine("Woof!");
    }
    
    public override void Move() {
        Console.WriteLine("Running on four legs");
    }
}

class Cat : Animal {
    public override void Speak() {
        Console.WriteLine("Meow!");
    }
}

class Program {
    static void MakeAnimalSpeak(Animal animal) {
        animal.Speak();
    }
    
    static void Main() {
        Animal dog = new Dog();
        Animal cat = new Cat();
        Animal animal = new Animal();
        
        MakeAnimalSpeak(dog);      // Woof!
        MakeAnimalSpeak(cat);      // Meow!
        MakeAnimalSpeak(animal);   // Some sound
    }
}
```

---

## 🌳 INHERITANCE

### Types of Inheritance

```
Inheritance Types
    ├── Single Inheritance
    ├── Multiple Inheritance
    ├── Multilevel Inheritance
    ├── Hierarchical Inheritance
    └── Hybrid Inheritance
```

### 1. Single Inheritance

**Diagram:**
```
    ┌─────────┐
    │  Parent │
    └────┬────┘
         │
         ▼
    ┌─────────┐
    │  Child  │
    └─────────┘
```

#### Python
```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand
    
    def start(self):
        return f"{self.brand} is starting"

class Car(Vehicle):
    def __init__(self, brand, model):
        super().__init__(brand)
        self.model = model
    
    def drive(self):
        return f"Driving {self.brand} {self.model}"

car = Car("Toyota", "Camry")
print(car.start())   # Toyota is starting
print(car.drive())   # Driving Toyota Camry
```

### 2. Multiple Inheritance

**Diagram:**
```
    ┌──────────┐     ┌──────────┐
    │ Parent1  │     │ Parent2  │
    └─────┬────┘     └────┬─────┘
          │               │
          └───────┬───────┘
                  ▼
            ┌──────────┐
            │  Child   │
            └──────────┘
```

#### Python (Supports Multiple Inheritance)
```python
class Flyer:
    def fly(self):
        return "Flying in the sky"

class Swimmer:
    def swim(self):
        return "Swimming in water"

class Duck(Flyer, Swimmer):  # Multiple inheritance
    def quack(self):
        return "Quack quack!"

duck = Duck()
print(duck.fly())    # Flying in the sky
print(duck.swim())   # Swimming in water
print(duck.quack())  # Quack quack!
```

### 3. Multilevel Inheritance

**Diagram:**
```
    ┌──────────┐
    │ GrandP   │
    └────┬─────┘
         │
         ▼
    ┌──────────┐
    │  Parent  │
    └────┬─────┘
         │
         ▼
    ┌──────────┐
    │  Child   │
    └──────────┘
```

#### Python
```python
class Animal:
    def breathe(self):
        return "Breathing"

class Mammal(Animal):
    def feed_milk(self):
        return "Feeding milk to young"

class Dog(Mammal):
    def bark(self):
        return "Woof!"

dog = Dog()
print(dog.breathe())    # Breathing (from Animal)
print(dog.feed_milk())  # Feeding milk to young (from Mammal)
print(dog.bark())       # Woof! (from Dog)
```

### 4. Hierarchical Inheritance

**Diagram:**
```
         ┌──────────┐
         │  Parent  │
         └────┬─────┘
              │
      ┌───────┼───────┐
      ▼       ▼       ▼
   ┌─────┐ ┌─────┐ ┌─────┐
   │Child1│ │Child2│ │Child3│
   └─────┘ └─────┘ └─────┘
```

#### Python
```python
class Shape:
    def __init__(self, color):
        self.color = color

class Circle(Shape):
    def __init__(self, color, radius):
        super().__init__(color)
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, color, length, width):
        super().__init__(color)
        self.length = length
        self.width = width
    
    def area(self):
        return self.length * self.width

class Triangle(Shape):
    def __init__(self, color, base, height):
        super().__init__(color)
        self.base = base
        self.height = height
    
    def area(self):
        return 0.5 * self.base * self.height
```

---

## 💎 DIAMOND PROBLEM & MRO

### Diamond Problem

**Diagram:**
```
         ┌─────┐
         │  A  │
         └──┬──┘
            │
     ┌──────┴──────┐
     ▼             ▼
   ┌───┐         ┌───┐
   │ B │         │ C │
   └─┬─┘         └─┬─┘
     │             │
     └──────┬──────┘
            ▼
          ┌───┐
          │ D │
          └───┘
```

If A has a method, B and C both override it, which version does D inherit?

### Python's MRO (Method Resolution Order)

```python
class A:
    def show(self):
        print("A's show()")

class B(A):
    def show(self):
        print("B's show()")

class C(A):
    def show(self):
        print("C's show()")

class D(B, C):  # Diamond inheritance
    pass

d = D()
d.show()  # B's show() - Uses C3 Linearization

# Check MRO
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, 
#  <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

print(D.mro())  # Same as __mro__ but as a list
```

**MRO Algorithm (C3 Linearization):**
1. Start with the child class
2. Move left to right in the parent list
3. Go depth-first, but avoid repetition
4. Maintain the order specified in parent classes

### Complex MRO Example

```python
class X:
    def method(self):
        print("X")

class Y:
    def method(self):
        print("Y")

class A(X, Y):
    pass

class B(Y, X):
    pass

# This will raise TypeError: Cannot create a consistent MRO
# class C(A, B):  # Error!
#     pass

# Why? Because A says X before Y, but B says Y before X
# Python can't resolve this conflict
```

### C++ Diamond Problem (Virtual Inheritance)

```cpp
#include <iostream>
using namespace std;

class A {
public:
    int value;
    A() : value(10) {
        cout << "A constructor" << endl;
    }
};

// Without virtual inheritance - Problem
class B : public A {
public:
    B() { cout << "B constructor" << endl; }
};

class C : public A {
public:
    C() { cout << "C constructor" << endl; }
};

class D : public B, public C {
public:
    D() { cout << "D constructor" << endl; }
    // Problem: D has TWO copies of A!
};

// With virtual inheritance - Solution
class B2 : virtual public A {
public:
    B2() { cout << "B2 constructor" << endl; }
};

class C2 : virtual public A {
public:
    C2() { cout << "C2 constructor" << endl; }
};

class D2 : public B2, public C2 {
public:
    D2() { cout << "D2 constructor" << endl; }
    // Now D2 has only ONE copy of A
};

int main() {
    D2 d;
    cout << d.value << endl;  // ✅ OK - single copy of A
    return 0;
}
```

---

## 🎯 TRICKY MCQs (15 Questions)

### Q1: Python Name Mangling
```python
class Test:
    def __init__(self):
        self.__x = 10
        self._y = 20
        self.z = 30

t = Test()
print(t._Test__x, t._y, t.z)
```

**What is the output?**
A) Error  
B) 10 20 30  
C) None 20 30  
D) 10 None 30

**Answer: B) 10 20 30**

**Explanation:** Python's name mangling transforms `__x` to `_Test__x`. It's still accessible, just harder to access accidentally. `_y` is just a convention (not enforced), and `z` is public.

---

### Q2: Java Method Overloading
```java
class Test {
    public void print(int x) {
        System.out.println("int: " + x);
    }
    
    public void print(double x) {
        System.out.println("double: " + x);
    }
    
    public static void main(String[] args) {
        Test t = new Test();
        t.print(5);
        t.print(5.0);
        t.print(5L);
    }
}
```

**What is the output?**
A) int: 5, double: 5.0, int: 5  
B) int: 5, double: 5.0, double: 5.0  
C) int: 5, double: 5.0, Error  
D) double: 5.0, double: 5.0, double: 5.0

**Answer: B) int: 5, double: 5.0, double: 5.0**

**Explanation:** `5` matches `int`, `5.0` matches `double`, `5L` is a long which gets promoted to `double` (widening conversion).

---

### Q3: C++ Virtual Function
```cpp
class Base {
public:
    void show() {
        cout << "Base";
    }
};

class Derived : public Base {
public:
    void show() {
        cout << "Derived";
    }
};

int main() {
    Base* ptr = new Derived();
    ptr->show();
    delete ptr;
    return 0;
}
```

**What is the output?**
A) Base  
B) Derived  
C) Error  
D) Base Derived

**Answer: A) Base**

**Explanation:** Without the `virtual` keyword in the base class, C++ uses static binding. The pointer type (Base*) determines which function is called, not the object type. To get "Derived", make `show()` virtual in Base.

---

### Q4: Python MRO
```python
class A:
    def method(self):
        print("A", end=" ")

class B(A):
    def method(self):
        print("B", end=" ")
        super().method()

class C(A):
    def method(self):
        print("C", end=" ")
        super().method()

class D(B, C):
    def method(self):
        print("D", end=" ")
        super().method()

D().method()
```

**What is the output?**
A) D B A  
B) D B C A  
C) D A  
D) D B C

**Answer: B) D B C A**

**Explanation:** MRO for D is [D, B, C, A, object]. `super()` follows this order. So: D calls super → B calls super → C calls super → A.

---

### Q5: Java Protected Access
```java
// File: pkg1/Parent.java
package pkg1;
public class Parent {
    protected int value = 10;
}

// File: pkg2/Child.java
package pkg2;
import pkg1.Parent;
public class Child extends Parent {
    public void test() {
        System.out.println(value);        // Line 1
        Parent p = new Parent();
        System.out.println(p.value);      // Line 2
    }
}
```

**What happens?**
A) Both lines compile successfully  
B) Line 1 error, Line 2 OK  
C) Line 1 OK, Line 2 error  
D) Both lines error

**Answer: C) Line 1 OK, Line 2 error**

**Explanation:** In Java, protected members are accessible in subclass only through inheritance (Line 1), not through a reference to parent class object in different package (Line 2).

---

### Q6: C# Method Hiding vs Overriding
```csharp
class Base {
    public virtual void Show() {
        Console.Write("Base ");
    }
}

class Derived1 : Base {
    public override void Show() {
        Console.Write("Derived1 ");
    }
}

class Derived2 : Base {
    public new void Show() {
        Console.Write("Derived2 ");
    }
}

// Usage
Base b1 = new Derived1();
Base b2 = new Derived2();
b1.Show();
b2.Show();
```

**What is the output?**
A) Derived1 Derived2  
B) Base Base  
C) Derived1 Base  
D) Base Derived2

**Answer: C) Derived1 Base**

**Explanation:** `override` provides runtime polymorphism (calls Derived1). `new` keyword hides the base method but doesn't override it, so with Base reference it calls Base's version.

---

### Q7: Python Multiple Inheritance
```python
class A:
    def __init__(self):
        self.x = 1
        print("A init")

class B(A):
    def __init__(self):
        super().__init__()
        self.x = 2
        print("B init")

class C(A):
    def __init__(self):
        super().__init__()
        self.x = 3
        print("C init")

class D(B, C):
    def __init__(self):
        super().__init__()
        print("D init")

d = D()
print(d.x)
```

**What is the output?**
A) A init, B init, D init, 2  
B) A init, B init, A init, C init, D init, 3  
C) A init, C init, B init, D init, 2  
D) A init, B init, C init, D init, 3

**Answer: C) A init, C init, B init, D init, 2**

**Explanation:** MRO is [D, B, C, A]. `super()` follows this order. A → C (sets x=3) → B (sets x=2) → D. Final value is 2.

---

### Q8: Java Constructor Chaining
```java
class Parent {
    Parent() {
        System.out.print("1");
        display();
    }
    
    void display() {
        System.out.print("2");
    }
}

class Child extends Parent {
    int x = 3;
    
    Child() {
        System.out.print("4");
    }
    
    void display() {
        System.out.print(x);
    }
}

public class Test {
    public static void main(String[] args) {
        new Child();
    }
}
```

**What is the output?**
A) 1234  
B) 1324  
C) 1042  
D) 1024

**Answer: D) 1024**

**Explanation:** Parent constructor runs first (prints 1), calls display() which is overridden (prints x=0, default int value before Child constructor runs), then Child constructor runs (prints 4).

---

### Q9: C++ Operator Overloading
```cpp
class Number {
    int val;
public:
    Number(int v = 0) : val(v) {}
    
    Number operator+(const Number& n) {
        return Number(val + n.val);
    }
    
    Number operator++() {  // Prefix
        ++val;
        return *this;
    }
    
    Number operator++(int) {  // Postfix
        Number temp = *this;
        val++;
        return temp;
    }
    
    int getValue() { return val; }
};

int main() {
    Number n1(5), n2(3);
    Number n3 = n1 + n2;
    Number n4 = ++n3;
    Number n5 = n3++;
    cout << n3.getValue() << " " << n4.getValue() << " " << n5.getValue();
    return 0;
}
```

**What is the output?**
A) 8 8 8  
B) 9 9 9  
C) 10 9 9  
D) 9 8 8

**Answer: C) 10 9 9**

**Explanation:** n3 = 8, ++n3 makes n3=9 and returns n3 (n4=9), n3++ returns old value (n5=9) then increments n3 to 10.

---

### Q10: Python Property Decorators
```python
class Temperature:
    def __init__(self):
        self._celsius = 0
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Below absolute zero!")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        return (self._celsius * 9/5) + 32

t = Temperature()
t.celsius = 100
print(t.fahrenheit)
t.celsius = -300
```

**What happens?**
A) Prints 212, then ValueError  
B) Prints 32, then ValueError  
C) AttributeError  
D) Prints 212, then -508

**Answer: A) Prints 212, then ValueError**

**Explanation:** Setting celsius to 100 works, fahrenheit is calculated as 212. Setting celsius to -300 triggers the validation in setter, raising ValueError.

---

### Q11: Java Abstract Classes
```java
abstract class Animal {
    abstract void sound();
    
    void sleep() {
        System.out.print("Sleeping ");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.print("Woof ");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound();
        a.sleep();
    }
}
```

**What is the output?**
A) Error: Cannot instantiate abstract class  
B) Woof Sleeping  
C) Sleeping Woof  
D) Error: sound() is abstract

**Answer: B) Woof Sleeping**

**Explanation:** You can't instantiate abstract classes, but you can create references. Dog provides implementation for sound(), inherits sleep().

---

### Q12: C++ Virtual Destructor
```cpp
class Base {
public:
    ~Base() {
        cout << "Base destructor ";
    }
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "Derived destructor ";
    }
};

int main() {
    Base* ptr = new Derived();
    delete ptr;
    return 0;
}
```

**What is the output?**
A) Base destructor  
B) Derived destructor Base destructor  
C) Base destructor Derived destructor  
D) Derived destructor

**Answer: A) Base destructor**

**Explanation:** Without virtual destructor, only Base's destructor is called (memory leak!). Make Base destructor virtual to call Derived's destructor first, then Base's.

---

### Q13: Python `super()` with Arguments
```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width
    
    def area(self):
        return self.length * self.width

class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)

s = Square(5)
print(s.area())
```

**What is the output?**
A) Error: __init__() takes 2 arguments, 3 given  
B) 10  
C) 25  
D) 5

**Answer: C) 25**

**Explanation:** Square passes `side` twice to Rectangle's __init__, so length=5, width=5, area=25.

---

### Q14: Java Interface Default Methods
```java
interface A {
    default void show() {
        System.out.print("A ");
    }
}

interface B {
    default void show() {
        System.out.print("B ");
    }
}

class C implements A, B {
    // Must override due to conflict
    public void show() {
        A.super.show();
        B.super.show();
    }
}

public class Test {
    public static void main(String[] args) {
        C c = new C();
        c.show();
    }
}
```

**What is the output?**
A) Error: C doesn't override show()  
B) A B  
C) B A  
D) A

**Answer: B) A B**

**Explanation:** When multiple interfaces have default methods with same signature, implementing class must override. Can call specific interface's method using `Interface.super.method()`.

---

### Q15: C# Sealed Classes
```csharp
sealed class FinalClass {
    public void Display() {
        Console.WriteLine("Final");
    }
}

class Derived : FinalClass {  // Line 1
    
}
```

**What happens?**
A) Compiles successfully  
B) Error at Line 1: Cannot inherit from sealed class  
C) Runtime error  
D) Compiles with warning

**Answer: B) Error at Line 1: Cannot inherit from sealed class**

**Explanation:** `sealed` keyword in C# (like `final` in Java) prevents inheritance. This is used for security and optimization.


---

## ⚠️ COMMON PITFALLS & EDGE CASES

### 1. Constructor Call in Inheritance

**Pitfall:**
```python
class Parent:
    def __init__(self):
        print("Parent init")

class Child(Parent):
    def __init__(self):
        # Forgot to call super().__init__()
        print("Child init")

c = Child()  # Only prints "Child init"
```

**Fix:**
```python
class Child(Parent):
    def __init__(self):
        super().__init__()  # Always call parent constructor
        print("Child init")
```

### 2. Mutable Default Arguments

**Pitfall:**
```python
class MyClass:
    def __init__(self, items=[]):  # Dangerous!
        self.items = items

obj1 = MyClass()
obj1.items.append(1)
obj2 = MyClass()
print(obj2.items)  # [1] - Shared list!
```

**Fix:**
```python
class MyClass:
    def __init__(self, items=None):
        self.items = items if items is not None else []
```

### 3. Overriding with Different Signatures

**Pitfall (Java):**
```java
class Parent {
    public void method(int x) { }
}

class Child extends Parent {
    // This is overloading, not overriding!
    public void method(double x) { }
}

Parent p = new Child();
p.method(5);  // Calls Parent's method
```

### 4. Forgetting Virtual in C++

**Pitfall:**
```cpp
class Base {
public:
    void show() { cout << "Base"; }  // Not virtual!
};

class Derived : public Base {
public:
    void show() { cout << "Derived"; }
};

Base* ptr = new Derived();
ptr->show();  // Prints "Base" - not polymorphic!
```

### 5. Access Modifier Confusion

**Python:**
```python
class Test:
    def __init__(self):
        self.__private = 1    # Name mangled to _Test__private
        self._protected = 2   # Just convention
        self.public = 3       # Public

t = Test()
# All are accessible externally (Python trusts developers)
```

**Java:**
```java
class Test {
    private int x;      // Only in class
    protected int y;    // Class + subclass + package
    int z;              // Package (default)
    public int w;       // Everywhere
}
```

### 6. Diamond Problem Without Virtual Inheritance (C++)

```cpp
class A { public: int x; };
class B : public A { };
class C : public A { };
class D : public B, public C { };

D d;
// d.x = 5;  // Error: Ambiguous! Which x? B's or C's?
d.B::x = 5;  // Must specify
```

### 7. Method Resolution Order Conflicts (Python)

```python
class X: pass
class Y: pass
class A(X, Y): pass
class B(Y, X): pass

# This raises TypeError
# class C(A, B): pass  # Inconsistent MRO
```

### 8. Calling Overridden Method from Constructor

```java
class Parent {
    Parent() {
        display();  // Calls overridden method before Child fully initialized!
    }
    void display() { }
}

class Child extends Parent {
    int x = 10;
    void display() {
        System.out.println(x);  // Prints 0 (default value)
    }
}
```

### 9. Covariant Return Types

**Java:**
```java
class Animal {
    Animal reproduce() {
        return new Animal();
    }
}

class Dog extends Animal {
    @Override
    Dog reproduce() {  // Covariant return type (allowed since Java 5)
        return new Dog();
    }
}
```

### 10. Interface vs Abstract Class Decision

| Feature | Interface | Abstract Class |
|---------|-----------|----------------|
| Multiple Inheritance | ✅ Yes (Java, C#) | ❌ No |
| State (fields) | ❌ No | ✅ Yes |
| Constructor | ❌ No | ✅ Yes |
| Access Modifiers | Public only | Any |
| When to Use | "Can do" relationship | "Is a" relationship |

---

## 📊 Summary Tables

### Polymorphism Types

| Type | Binding Time | Mechanism | Languages |
|------|--------------|-----------|-----------|
| Compile-time | Compilation | Method Overloading | Java, C++, C# |
| Compile-time | Compilation | Operator Overloading | Python, C++, C# |
| Runtime | Execution | Method Overriding | All OOP languages |
| Runtime | Execution | Virtual Functions | C++ |

### Inheritance Support

| Language | Single | Multiple | Multilevel | Hierarchical |
|----------|--------|----------|------------|--------------|
| Python | ✅ | ✅ | ✅ | ✅ |
| Java | ✅ | ❌ (Interfaces) | ✅ | ✅ |
| C++ | ✅ | ✅ | ✅ | ✅ |
| C# | ✅ | ❌ (Interfaces) | ✅ | ✅ |

---

## �� Key Takeaways

1. **Encapsulation** bundles data and methods, restricting access to internal state
2. **Access modifiers** vary by language but follow similar principles
3. **Polymorphism** enables one interface, multiple implementations
4. **Compile-time polymorphism** (overloading) resolves at compilation
5. **Runtime polymorphism** (overriding) resolves at execution
6. **Inheritance** enables code reuse and establishes "is-a" relationships
7. **Diamond problem** is handled differently: Python (MRO), C++ (virtual inheritance), Java/C# (interfaces)
8. **MRO** in Python uses C3 linearization algorithm
9. **Virtual functions** in C++ enable runtime polymorphism
10. Always call parent constructors in inheritance hierarchy

---

**End of Document** 🎯
