# Object-Oriented Programming - Complete Revision Guide

## Quick Reference: All Key Concepts & Patterns

---

## THE FOUR PILLARS OF OOP

```
┌─────────────────────────────────────────┐
│  1. ABSTRACTION  - Hide complexity      │
│  2. ENCAPSULATION - Data hiding         │
│  3. POLYMORPHISM - Many forms           │
│  4. INHERITANCE  - Code reuse           │
└─────────────────────────────────────────┘
```

---

## 1. ABSTRACTION

### Definition
**Abstraction** = Hiding complex implementation details, showing only essential features.  
**Focus**: *What* an object does, not *how* it does it.

### Key Concepts Table

| Concept | Purpose | Implementation |
|---------|---------|----------------|
| **Abstract Class** | Cannot instantiate, must subclass | `@abstractmethod` decorator |
| **Interface** | Contract for methods | Multiple abstract classes |
| **Concrete Class** | Implements abstract methods | Complete implementation |

### Abstract Class Pattern

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    """Abstract base class"""
    
    @abstractmethod
    def area(self):
        """Must be implemented by subclasses"""
        pass
    
    @abstractmethod
    def perimeter(self):
        """Must be implemented by subclasses"""
        pass
    
    def describe(self):
        """Concrete method - optional to override"""
        return f"This is a {self.__class__.__name__}"

class Rectangle(Shape):
    """Concrete implementation"""
    
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

# Usage
# shape = Shape()  # ❌ TypeError - cannot instantiate abstract class
rect = Rectangle(5, 3)  # ✅ Concrete class
print(rect.area())       # 15
```

### Multiple Interfaces (Multiple Inheritance)

```python
class Drawable(ABC):
    @abstractmethod
    def draw(self):
        pass

class Resizable(ABC):
    @abstractmethod
    def resize(self, scale):
        pass

class Image(Drawable, Resizable):
    """Implements multiple interfaces"""
    
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def draw(self):
        return f"Drawing at {self.width}x{self.height}"
    
    def resize(self, scale):
        self.width *= scale
        self.height *= scale
```

### When to Use Abstraction

| Scenario | Use Abstraction |
|----------|----------------|
| Multiple implementations | ✅ Define common interface |
| Hide complexity | ✅ Show only necessary methods |
| Framework design | ✅ Let users extend your classes |
| Enforce structure | ✅ Force subclasses to implement |

---

## 2. ENCAPSULATION

### Definition
**Encapsulation** = Bundling data + methods together, restricting direct access to some components.  
**Goal**: Data protection, validation, controlled access.

### Access Modifiers in Python

| Modifier | Syntax | Access | Use Case |
|----------|--------|--------|----------|
| **Public** | `name` | Everywhere | Public API |
| **Protected** | `_name` | Class + subclasses | Internal use |
| **Private** | `__name` | Class only | Sensitive data |

### Name Mangling
```python
class MyClass:
    def __init__(self):
        self.public = "public"
        self._protected = "protected"
        self.__private = "private"

obj = MyClass()
print(obj.public)      # ✅ 'public'
print(obj._protected)  # ⚠️  'protected' (convention, not enforced)
# print(obj.__private) # ❌ AttributeError
print(obj._MyClass__private)  # ✅ 'private' (name mangled)
```

### Encapsulation Pattern - Bank Account

```python
class BankAccount:
    """Encapsulation example"""
    
    def __init__(self, account_number, balance):
        self.account_number = account_number  # Public
        self._branch = "Main"                  # Protected
        self.__balance = balance               # Private
    
    def deposit(self, amount):
        """Public method with validation"""
        if amount > 0:
            self.__balance += amount
            return f"Deposited ${amount}"
        return "Invalid amount"
    
    def withdraw(self, amount):
        """Public method with validation"""
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return f"Withdrew ${amount}"
        return "Insufficient funds"
    
    # Getter
    def get_balance(self):
        return self.__balance
    
    # Setter
    def set_balance(self, balance):
        if balance >= 0:
            self.__balance = balance
        else:
            raise ValueError("Balance cannot be negative")

# Usage
account = BankAccount("ACC001", 1000)
# print(account.__balance)  # ❌ AttributeError
print(account.get_balance())  # ✅ 1000
account.deposit(500)           # ✅ Validated
```

### Property Decorators (@property)

```python
class Employee:
    """Using @property for clean encapsulation"""
    
    def __init__(self, name, salary):
        self._name = name
        self._salary = salary
    
    @property
    def name(self):
        """Getter"""
        return self._name
    
    @name.setter
    def name(self, value):
        """Setter with validation"""
        if isinstance(value, str) and len(value) > 0:
            self._name = value
        else:
            raise ValueError("Invalid name")
    
    @property
    def salary(self):
        return self._salary
    
    @salary.setter
    def salary(self, value):
        if value >= 0:
            self._salary = value
        else:
            raise ValueError("Salary must be positive")
    
    @property
    def annual_salary(self):
        """Read-only computed property"""
        return self._salary * 12

# Usage - access like attributes
emp = Employee("Alice", 5000)
print(emp.name)           # ✅ Getter called
emp.salary = 5500         # ✅ Setter called with validation
print(emp.annual_salary)  # ✅ Computed property
# emp.annual_salary = 70000  # ❌ AttributeError (read-only)
```

### Benefits of Encapsulation

| Benefit | Description |
|---------|-------------|
| **Data Protection** | Control access to sensitive data |
| **Validation** | Ensure data integrity before modification |
| **Flexibility** | Change implementation without breaking users |
| **Maintainability** | Centralized data access logic |

---

## 3. POLYMORPHISM

### Definition
**Polymorphism** = "Many forms". Same interface, different implementations.  
**Types**: Compile-time (overloading) and Runtime (overriding).

### Types of Polymorphism

| Type | Description | Python Support |
|------|-------------|----------------|
| **Method Overloading** | Same name, different parameters | ⚠️ Via `*args`, defaults |
| **Method Overriding** | Subclass redefines parent method | ✅ Full support |
| **Operator Overloading** | Custom operator behavior | ✅ Via `__add__`, etc. |
| **Duck Typing** | "If it quacks like a duck..." | ✅ Python's strength |

### Method Overloading (Simulation)

```python
class Calculator:
    """Python doesn't support traditional overloading"""
    
    def add(self, *args):
        """Variable arguments - polymorphic behavior"""
        return sum(args)
    
    def multiply(self, a, b=1, c=1):
        """Default arguments"""
        return a * b * c

calc = Calculator()
print(calc.add(5, 3))        # 8
print(calc.add(1, 2, 3, 4))  # 10
print(calc.multiply(5))      # 5
print(calc.multiply(5, 3))   # 15
```

### Method Overriding (Runtime Polymorphism)

```python
class Animal:
    """Base class"""
    def speak(self):
        return "Some sound"

class Dog(Animal):
    """Override speak()"""
    def speak(self):
        return "Woof!"

class Cat(Animal):
    """Override speak()"""
    def speak(self):
        return "Meow!"

class Bird(Animal):
    """Override speak()"""
    def speak(self):
        return "Chirp!"

# Polymorphism in action
def animal_sound(animal: Animal):
    """Same function, different behaviors"""
    print(animal.speak())

# Different objects, same interface
animals = [Dog(), Cat(), Bird()]
for animal in animals:
    animal_sound(animal)

# Output:
# Woof!
# Meow!
# Chirp!
```

### Operator Overloading

```python
class Vector:
    """Custom operators for vector math"""
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        """Overload + operator"""
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        """Overload - operator"""
        return Vector(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        """Overload * operator"""
        return Vector(self.x * scalar, self.y * scalar)
    
    def __eq__(self, other):
        """Overload == operator"""
        return self.x == other.x and self.y == other.y
    
    def __str__(self):
        """String representation"""
        return f"Vector({self.x}, {self.y})"

# Usage
v1 = Vector(2, 3)
v2 = Vector(4, 1)

v3 = v1 + v2      # Calls __add__
print(v3)         # Vector(6, 4)

v4 = v1 * 3       # Calls __mul__
print(v4)         # Vector(6, 9)

print(v1 == v2)   # False (calls __eq__)
```

### Common Magic Methods

| Method | Operator | Example |
|--------|----------|---------|
| `__add__` | `+` | `a + b` |
| `__sub__` | `-` | `a - b` |
| `__mul__` | `*` | `a * b` |
| `__truediv__` | `/` | `a / b` |
| `__eq__` | `==` | `a == b` |
| `__lt__` | `<` | `a < b` |
| `__le__` | `<=` | `a <= b` |
| `__gt__` | `>` | `a > b` |
| `__ge__` | `>=` | `a >= b` |
| `__len__` | `len()` | `len(a)` |
| `__getitem__` | `[]` | `a[key]` |
| `__str__` | `str()` | `str(a)` |
| `__repr__` | `repr()` | `repr(a)` |

### Duck Typing

```python
class Duck:
    def swim(self):
        return "Duck swimming"
    
    def fly(self):
        return "Duck flying"

class Airplane:
    def fly(self):
        return "Airplane flying"

def make_it_fly(entity):
    """
    Doesn't check type, only if fly() exists
    "If it walks like a duck and quacks like a duck..."
    """
    return entity.fly()

duck = Duck()
plane = Airplane()

print(make_it_fly(duck))   # Duck flying
print(make_it_fly(plane))  # Airplane flying
# No type checking needed!
```

---

## 4. INHERITANCE

### Definition
**Inheritance** = Deriving new class from existing class, inheriting attributes and methods.  
**Purpose**: Code reuse, hierarchical relationships, extending functionality.

### Inheritance Types

| Type | Description | Syntax |
|------|-------------|--------|
| **Single** | One parent, one child | `class Child(Parent):` |
| **Multiple** | Multiple parents | `class Child(Parent1, Parent2):` |
| **Multilevel** | Chain: A → B → C | `class C(B):` where `class B(A):` |
| **Hierarchical** | Multiple children from one parent | Multiple `class Child(Parent):` |

### Single Inheritance

```python
class Vehicle:
    """Parent/Base/Super class"""
    
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def info(self):
        return f"{self.brand} {self.model}"

class Car(Vehicle):
    """Child/Derived/Sub class"""
    
    def __init__(self, brand, model, doors):
        super().__init__(brand, model)  # Call parent constructor
        self.doors = doors
    
    def info(self):
        """Override parent method"""
        return f"{super().info()} with {self.doors} doors"

# Usage
car = Car("Toyota", "Camry", 4)
print(car.info())  # Toyota Camry with 4 doors
```

### Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "Flying"

class Swimmable:
    def swim(self):
        return "Swimming"

class Duck(Flyable, Swimmable):
    """Inherits from multiple classes"""
    def quack(self):
        return "Quack!"

duck = Duck()
print(duck.fly())    # Flying
print(duck.swim())   # Swimming
print(duck.quack())  # Quack!
```

### Method Resolution Order (MRO)

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):
    pass

# MRO: D → B → C → A → object
print(D.__mro__)
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)

d = D()
print(d.method())  # "B" (follows MRO)
```

### super() Function

```python
class Animal:
    def __init__(self, species):
        self.species = species
    
    def sound(self):
        return "Some sound"

class Dog(Animal):
    def __init__(self, species, breed):
        super().__init__(species)  # Call parent __init__
        self.breed = breed
    
    def sound(self):
        parent_sound = super().sound()  # Access parent method
        return f"{parent_sound} - Woof!"

dog = Dog("Canine", "Labrador")
print(dog.species)  # Canine (from parent)
print(dog.breed)    # Labrador (from child)
print(dog.sound())  # Some sound - Woof!
```

### Inheritance Best Practices

| Practice | Description |
|----------|-------------|
| **Is-A Relationship** | Use inheritance only for true "is-a" relationships |
| **Favor Composition** | "Has-a" relationships use composition, not inheritance |
| **Liskov Substitution** | Child should work wherever parent works |
| **Avoid Deep Hierarchies** | Keep inheritance tree shallow (max 2-3 levels) |
| **Use super()** | Call parent methods properly |

---

## RELATIONSHIPS BETWEEN CLASSES

### 1. Association
```python
# "Uses-a" relationship
class Teacher:
    def teach(self, student):
        return f"Teaching {student.name}"

class Student:
    def __init__(self, name):
        self.name = name

# Loose coupling
teacher = Teacher()
student = Student("Alice")
print(teacher.teach(student))
```

### 2. Aggregation ("Has-a")
```python
# Weak ownership - objects can exist independently
class Department:
    def __init__(self, name):
        self.name = name
        self.employees = []  # Has employees
    
    def add_employee(self, employee):
        self.employees.append(employee)

class Employee:
    def __init__(self, name):
        self.name = name

# Both can exist independently
emp = Employee("Bob")
dept = Department("IT")
dept.add_employee(emp)
# emp still exists even if dept is deleted
```

### 3. Composition ("Owns-a")
```python
# Strong ownership - child can't exist without parent
class Engine:
    def __init__(self, hp):
        self.horsepower = hp

class Car:
    def __init__(self, brand, hp):
        self.brand = brand
        self.engine = Engine(hp)  # Owns engine
    
    def __del__(self):
        print(f"Car {self.brand} destroyed")
        # Engine is also destroyed

car = Car("Honda", 200)
# If car is deleted, engine is also destroyed
```

### Relationship Comparison

| Relationship | Connection | Lifetime | Example |
|--------------|-----------|----------|---------|
| **Association** | Uses-a | Independent | Teacher uses Student |
| **Aggregation** | Has-a | Independent | Department has Employees |
| **Composition** | Owns-a | Dependent | Car owns Engine |
| **Inheritance** | Is-a | N/A | Dog is an Animal |

---

## DESIGN PRINCIPLES

### SOLID Principles

#### 1. Single Responsibility Principle (SRP)
```python
# ❌ Bad - Multiple responsibilities
class User:
    def save_to_db(self):
        pass
    
    def send_email(self):
        pass

# ✅ Good - Single responsibility
class User:
    def __init__(self, name):
        self.name = name

class UserRepository:
    def save(self, user):
        pass

class EmailService:
    def send(self, user):
        pass
```

#### 2. Open/Closed Principle (OCP)
```python
# ✅ Open for extension, closed for modification
class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

# Add new shapes without modifying existing code
class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
```

#### 3. Liskov Substitution Principle (LSP)
```python
# ✅ Child class should work wherever parent works
class Bird:
    def move(self):
        return "Moving"

class Sparrow(Bird):
    def move(self):
        return "Flying"

class Penguin(Bird):
    def move(self):
        return "Walking"

def let_bird_move(bird: Bird):
    return bird.move()

# Both work
sparrow = Sparrow()
penguin = Penguin()
print(let_bird_move(sparrow))  # Flying
print(let_bird_move(penguin))  # Walking
```

#### 4. Interface Segregation Principle (ISP)
```python
# ✅ Many specific interfaces better than one general
class Workable(ABC):
    @abstractmethod
    def work(self):
        pass

class Eatable(ABC):
    @abstractmethod
    def eat(self):
        pass

class Human(Workable, Eatable):
    def work(self):
        return "Working"
    
    def eat(self):
        return "Eating"

class Robot(Workable):
    def work(self):
        return "Working"
    # No eat() - robot doesn't eat!
```

#### 5. Dependency Inversion Principle (DIP)
```python
# ✅ Depend on abstractions, not concretions
class Database(ABC):
    @abstractmethod
    def save(self, data):
        pass

class MySQLDatabase(Database):
    def save(self, data):
        return f"Saving to MySQL: {data}"

class PostgreSQLDatabase(Database):
    def save(self, data):
        return f"Saving to PostgreSQL: {data}"

class UserService:
    def __init__(self, database: Database):
        self.db = database  # Depends on abstraction
    
    def save_user(self, user):
        return self.db.save(user)

# Can switch database easily
service1 = UserService(MySQLDatabase())
service2 = UserService(PostgreSQLDatabase())
```

---

## CLASS VS INSTANCE

### Comparison Table

| Feature | Instance | Class |
|---------|----------|-------|
| **Variables** | Unique per object | Shared across all objects |
| **Methods** | Access via `self` | Access via `cls` or class name |
| **Declaration** | In `__init__` | Outside methods |
| **Access** | `obj.variable` | `ClassName.variable` |
| **Use Case** | Object-specific data | Shared data, constants |

### Example

```python
class Employee:
    # Class variable
    company = "TechCorp"
    employee_count = 0
    
    def __init__(self, name, salary):
        # Instance variables
        self.name = name
        self.salary = salary
        Employee.employee_count += 1
    
    def display(self):
        """Instance method - uses self"""
        return f"{self.name} works at {Employee.company}"
    
    @classmethod
    def get_count(cls):
        """Class method - uses cls"""
        return f"Total employees: {cls.employee_count}"
    
    @staticmethod
    def is_valid_salary(salary):
        """Static method - no self or cls"""
        return salary > 0

# Usage
emp1 = Employee("Alice", 5000)
emp2 = Employee("Bob", 6000)

print(emp1.name)                # Alice (instance)
print(Employee.company)         # TechCorp (class)
print(Employee.get_count())     # Total employees: 2
print(Employee.is_valid_salary(4000))  # True
```

---

## SPECIAL METHODS (MAGIC/DUNDER)

### Common Special Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `__init__` | Constructor | `obj = MyClass()` |
| `__str__` | String representation | `str(obj)`, `print(obj)` |
| `__repr__` | Official representation | `repr(obj)` |
| `__len__` | Length | `len(obj)` |
| `__getitem__` | Indexing | `obj[key]` |
| `__setitem__` | Set item | `obj[key] = value` |
| `__call__` | Call as function | `obj()` |
| `__iter__` | Make iterable | `for x in obj` |
| `__next__` | Iterator protocol | `next(obj)` |
| `__enter__/__exit__` | Context manager | `with obj:` |

### Example Implementation

```python
class MyList:
    """Custom list-like class"""
    
    def __init__(self, items):
        self.items = items
    
    def __str__(self):
        """Called by str() and print()"""
        return f"MyList({self.items})"
    
    def __len__(self):
        """Called by len()"""
        return len(self.items)
    
    def __getitem__(self, index):
        """Called by obj[index]"""
        return self.items[index]
    
    def __setitem__(self, index, value):
        """Called by obj[index] = value"""
        self.items[index] = value
    
    def __iter__(self):
        """Make iterable"""
        return iter(self.items)
    
    def __contains__(self, item):
        """Called by 'in' operator"""
        return item in self.items

# Usage
ml = MyList([1, 2, 3, 4, 5])
print(ml)           # MyList([1, 2, 3, 4, 5])
print(len(ml))      # 5
print(ml[2])        # 3
ml[2] = 10          # Set item
print(3 in ml)      # False
print(10 in ml)     # True

for item in ml:
    print(item, end=" ")  # 1 2 10 4 5
```

---

## QUICK REFERENCE CHECKLIST

### When to Use Which?

| Scenario | Use |
|----------|-----|
| Hide implementation details | **Abstraction** |
| Protect data | **Encapsulation** |
| Same interface, different behavior | **Polymorphism** |
| Code reuse, "is-a" relationship | **Inheritance** |
| "Has-a" relationship | **Composition** |
| Shared data across instances | **Class variables** |
| Object-specific data | **Instance variables** |
| Custom operators | **Operator overloading** |
| Make class iterable | `__iter__` and `__next__` |
| Context management | `__enter__` and `__exit__` |

### Common Patterns Summary

```python
# 1. Abstract class pattern
class Base(ABC):
    @abstractmethod
    def method(self):
        pass

# 2. Encapsulation pattern
class Secure:
    def __init__(self):
        self.__private = value
    
    @property
    def private(self):
        return self.__private

# 3. Polymorphism pattern
def process(obj: Base):
    return obj.method()  # Different behavior per subclass

# 4. Inheritance pattern
class Child(Parent):
    def __init__(self, args):
        super().__init__(parent_args)
```

---

## INTERVIEW QUICK TIPS

### Key Differences to Remember

**Abstraction vs Encapsulation**:
- Abstraction: Hide complexity (what to show)
- Encapsulation: Hide data (how to protect)

**Inheritance vs Composition**:
- Inheritance: "is-a" (Dog is an Animal)
- Composition: "has-a" (Car has an Engine)

**Method Overloading vs Overriding**:
- Overloading: Same name, different parameters (compile-time)
- Overriding: Subclass redefines parent method (runtime)

**Abstract Class vs Interface**:
- Abstract class: Can have concrete methods
- Interface: All methods abstract (Python: all `@abstractmethod`)

### Common Interview Questions

1. **What are the four pillars of OOP?**  
   Abstraction, Encapsulation, Polymorphism, Inheritance

2. **What is the difference between `__init__` and `__new__`?**  
   `__new__` creates instance, `__init__` initializes it

3. **What is MRO?**  
   Method Resolution Order - order in which Python looks for methods

4. **What is super()?**  
   Call parent class methods from child class

5. **What is a property decorator?**  
   Convert methods to attributes with getters/setters

---

## END OF OOP REVISION GUIDE

**Master these concepts, practice with real examples, ace your interviews! 🚀**
