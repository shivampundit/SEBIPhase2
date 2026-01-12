# Object-Oriented Programming - Comprehensive Notes

## Overview
Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects" that contain data (attributes) and code (methods).

### Four Pillars of OOP
1. **Abstraction** - Hiding complexity
2. **Encapsulation** - Data hiding
3. **Polymorphism** - Many forms
4. **Inheritance** - Code reuse

---

## 1. ABSTRACTION

### Definition
Abstraction means hiding complex implementation details and showing only essential features. It focuses on **what** an object does rather than **how** it does it.

### Key Concepts
- Hide internal implementation
- Show only relevant information
- Simplify complex systems
- Abstract classes and interfaces

---

### Abstract Classes

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    """
    Abstract base class for shapes
    Cannot be instantiated directly
    """
    
    @abstractmethod
    def area(self):
        """Calculate area - must be implemented by subclasses"""
        pass
    
    @abstractmethod
    def perimeter(self):
        """Calculate perimeter - must be implemented by subclasses"""
        pass
    
    def describe(self):
        """Concrete method - can be used by all subclasses"""
        return f"This is a {self.__class__.__name__}"

# Cannot create instance of abstract class
# shape = Shape()  # TypeError

class Rectangle(Shape):
    """Concrete implementation of Shape"""
    
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        """Implementation of abstract method"""
        return self.width * self.height
    
    def perimeter(self):
        """Implementation of abstract method"""
        return 2 * (self.width + self.height)

class Circle(Shape):
    """Another concrete implementation"""
    
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius

# Usage
rect = Rectangle(5, 3)
print(rect.area())        # 15
print(rect.perimeter())   # 16
print(rect.describe())    # This is a Rectangle

circle = Circle(7)
print(circle.area())      # 153.938
print(circle.perimeter()) # 43.982
```

#### Dry Run
```
Create Rectangle(5, 3):
- width = 5, height = 3
- Can call area() and perimeter()
- Implementation provided in Rectangle class

rect.area():
- Calls Rectangle's area() method
- Calculates: 5 * 3 = 15
- Returns 15

Cannot create Shape():
- Shape is abstract
- Has @abstractmethod decorators
- Must be subclassed
```

---

### Interfaces (Multiple Abstract Classes)

```python
from abc import ABC, abstractmethod

class Drawable(ABC):
    """Interface for drawable objects"""
    
    @abstractmethod
    def draw(self):
        pass

class Resizable(ABC):
    """Interface for resizable objects"""
    
    @abstractmethod
    def resize(self, scale):
        pass

class Image(Drawable, Resizable):
    """Class implementing multiple interfaces"""
    
    def __init__(self, filename, width, height):
        self.filename = filename
        self.width = width
        self.height = height
    
    def draw(self):
        return f"Drawing {self.filename} at {self.width}x{self.height}"
    
    def resize(self, scale):
        self.width *= scale
        self.height *= scale
        return f"Resized to {self.width}x{self.height}"

# Usage
img = Image("photo.jpg", 800, 600)
print(img.draw())       # Drawing photo.jpg at 800x600
print(img.resize(0.5))  # Resized to 400.0x300.0
```

---

### Real-World Example: Payment System

```python
from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    """Abstract payment method"""
    
    @abstractmethod
    def process_payment(self, amount):
        """Process payment - must be implemented"""
        pass
    
    @abstractmethod
    def validate(self):
        """Validate payment method"""
        pass

class CreditCard(PaymentMethod):
    """Credit card payment"""
    
    def __init__(self, card_number, cvv):
        self.card_number = card_number
        self.cvv = cvv
    
    def process_payment(self, amount):
        if self.validate():
            return f"Charged ${amount} to card ending in {self.card_number[-4:]}"
        return "Invalid card"
    
    def validate(self):
        return len(self.card_number) == 16 and len(self.cvv) == 3

class PayPal(PaymentMethod):
    """PayPal payment"""
    
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        if self.validate():
            return f"Charged ${amount} via PayPal to {self.email}"
        return "Invalid email"
    
    def validate(self):
        return '@' in self.email

# Usage - abstraction in action
def checkout(payment_method: PaymentMethod, amount: float):
    """
    Process checkout using any payment method
    Doesn't need to know implementation details
    """
    return payment_method.process_payment(amount)

# Different payment methods, same interface
cc = CreditCard("1234567890123456", "123")
pp = PayPal("user@example.com")

print(checkout(cc, 99.99))  # Charged $99.99 to card ending in 3456
print(checkout(pp, 49.99))  # Charged $49.99 via PayPal to user@example.com
```

---

### Benefits of Abstraction
1. **Simplicity**: Hide complexity
2. **Maintainability**: Change implementation without affecting users
3. **Code Reusability**: Define common interface
4. **Focus**: Work at higher level of abstraction

---

## 2. ENCAPSULATION

### Definition
Encapsulation is the bundling of data (attributes) and methods that operate on that data into a single unit (class), and restricting access to some components.

### Access Modifiers in Python

```python
class BankAccount:
    """
    Encapsulation example
    """
    
    def __init__(self, account_number, balance):
        self.account_number = account_number  # Public
        self._branch = "Main"                  # Protected (convention)
        self.__balance = balance               # Private (name mangling)
    
    # Public method
    def deposit(self, amount):
        """Anyone can deposit"""
        if amount > 0:
            self.__balance += amount
            return f"Deposited ${amount}. New balance: ${self.__balance}"
        return "Invalid amount"
    
    # Public method
    def withdraw(self, amount):
        """Withdrawal with validation"""
        if amount > 0 and amount <= self.__balance:
            self.__balance -= amount
            return f"Withdrew ${amount}. New balance: ${self.__balance}"
        return "Insufficient funds or invalid amount"
    
    # Getter (accessor)
    def get_balance(self):
        """Controlled access to private data"""
        return self.__balance
    
    # Setter (mutator)
    def set_balance(self, balance):
        """Controlled modification of private data"""
        if balance >= 0:
            self.__balance = balance
        else:
            raise ValueError("Balance cannot be negative")

# Usage
account = BankAccount("ACC001", 1000)

# Public access
print(account.account_number)  # ACC001

# Cannot directly access private
# print(account.__balance)  # AttributeError

# Must use public methods
print(account.get_balance())   # 1000
print(account.deposit(500))    # Deposited $500. New balance: $1500
print(account.withdraw(200))   # Withdrew $200. New balance: $1300
```

#### Dry Run
```
account = BankAccount("ACC001", 1000)
- account_number = "ACC001" (public)
- _branch = "Main" (protected)
- __balance = 1000 (private, mangled to _BankAccount__balance)

account.deposit(500):
- Checks: amount > 0 ✓
- Updates: __balance = 1000 + 500 = 1500
- Returns: "Deposited $500. New balance: $1500"

account.withdraw(200):
- Checks: 200 > 0 ✓ and 200 <= 1500 ✓
- Updates: __balance = 1500 - 200 = 1300
- Returns: "Withdrew $200. New balance: $1300"

Try direct access:
account.__balance → AttributeError
(Actual name: _BankAccount__balance)
```

---

### Property Decorators

```python
class Employee:
    """
    Using @property for encapsulation
    """
    
    def __init__(self, name, salary):
        self._name = name
        self._salary = salary
    
    # Getter
    @property
    def name(self):
        """Get name"""
        return self._name
    
    # Setter
    @name.setter
    def name(self, value):
        """Set name with validation"""
        if isinstance(value, str) and len(value) > 0:
            self._name = value
        else:
            raise ValueError("Invalid name")
    
    # Getter
    @property
    def salary(self):
        """Get salary"""
        return self._salary
    
    # Setter
    @salary.setter
    def salary(self, value):
        """Set salary with validation"""
        if value >= 0:
            self._salary = value
        else:
            raise ValueError("Salary must be positive")
    
    # Computed property (read-only)
    @property
    def annual_salary(self):
        """Calculate annual salary"""
        return self._salary * 12

# Usage
emp = Employee("Alice", 5000)

# Access like attributes, but using getters/setters
print(emp.name)           # Alice
print(emp.salary)         # 5000
print(emp.annual_salary)  # 60000

# Modify with validation
emp.salary = 5500         # Setter called
print(emp.salary)         # 5500

# Invalid modification blocked
# emp.salary = -100  # ValueError: Salary must be positive
```

---

### Data Hiding Example

```python
class Password:
    """
    Secure password storage with encapsulation
    """
    
    def __init__(self):
        self.__password = None
    
    def set_password(self, password):
        """Hash and store password"""
        if len(password) >= 8:
            # In real app, use proper hashing (bcrypt, etc.)
            self.__password = self.__hash(password)
            return "Password set successfully"
        return "Password too short (minimum 8 characters)"
    
    def check_password(self, password):
        """Verify password"""
        return self.__hash(password) == self.__password
    
    def __hash(self, password):
        """Private method to hash password"""
        # Simple hash (not secure - for demonstration only)
        return hash(password)

# Usage
pwd = Password()
pwd.set_password("MySecure123")

# Password is hidden
# Cannot access pwd.__password directly

# Can only verify
print(pwd.check_password("MySecure123"))  # True
print(pwd.check_password("WrongPass"))    # False
```

---

### Benefits of Encapsulation
1. **Data Protection**: Control access to sensitive data
2. **Validation**: Ensure data integrity
3. **Flexibility**: Change implementation without affecting users
4. **Maintainability**: Centralized data access logic

---

## 3. POLYMORPHISM

### Definition
Polymorphism means "many forms". Same interface/method name, different implementations. Allows objects of different types to be treated uniformly.

### Types of Polymorphism

#### A. Compile-Time Polymorphism (Method Overloading)

Python doesn't support traditional method overloading, but can achieve similar results:

```python
class Calculator:
    """
    Method overloading simulation
    """
    
    def add(self, *args):
        """
        Add variable number of arguments
        Polymorphic behavior based on argument count
        """
        if len(args) == 2:
            return args[0] + args[1]
        elif len(args) == 3:
            return args[0] + args[1] + args[2]
        else:
            return sum(args)
    
    def multiply(self, a, b=1, c=1):
        """
        Using default arguments
        """
        return a * b * c

# Usage
calc = Calculator()
print(calc.add(5, 3))         # 8
print(calc.add(5, 3, 2))      # 10
print(calc.add(1, 2, 3, 4))   # 10

print(calc.multiply(5))        # 5
print(calc.multiply(5, 3))     # 15
print(calc.multiply(5, 3, 2))  # 30
```

---

#### B. Runtime Polymorphism (Method Overriding)

```python
class Animal:
    """Base class"""
    
    def speak(self):
        return "Some sound"
    
    def move(self):
        return "Moving"

class Dog(Animal):
    """Override speak() method"""
    
    def speak(self):
        return "Woof!"
    
    def move(self):
        return "Running on four legs"

class Cat(Animal):
    """Override speak() method"""
    
    def speak(self):
        return "Meow!"
    
    def move(self):
        return "Walking gracefully"

class Bird(Animal):
    """Override both methods"""
    
    def speak(self):
        return "Chirp!"
    
    def move(self):
        return "Flying"

# Polymorphism in action
def animal_action(animal: Animal):
    """
    Same function works with different animal types
    Calls appropriate method based on actual object type
    """
    print(f"{animal.__class__.__name__} says: {animal.speak()}")
    print(f"{animal.__class__.__name__} is {animal.move()}")

# Different objects, same interface
animals = [Dog(), Cat(), Bird()]

for animal in animals:
    animal_action(animal)
    print()

# Output:
# Dog says: Woof!
# Dog is Running on four legs
#
# Cat says: Meow!
# Cat is Walking gracefully
#
# Bird says: Chirp!
# Bird is Flying
```

#### Dry Run
```
animals = [Dog(), Cat(), Bird()]

Loop iteration 1:
- animal = Dog instance
- animal.speak() → calls Dog's speak() → "Woof!"
- animal.move() → calls Dog's move() → "Running on four legs"

Loop iteration 2:
- animal = Cat instance
- animal.speak() → calls Cat's speak() → "Meow!"
- animal.move() → calls Cat's move() → "Walking gracefully"

Loop iteration 3:
- animal = Bird instance
- animal.speak() → calls Bird's speak() → "Chirp!"
- animal.move() → calls Bird's move() → "Flying"

Same method names, different behaviors! (Polymorphism)
```

---

### Operator Overloading

```python
class Vector:
    """
    Operator overloading example
    """
    
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
    
    def __repr__(self):
        """Official representation"""
        return f"Vector({self.x}, {self.y})"

# Usage
v1 = Vector(2, 3)
v2 = Vector(4, 1)

# Operator overloading in action
v3 = v1 + v2      # Calls __add__
print(v3)         # Vector(6, 4)

v4 = v1 - v2      # Calls __sub__
print(v4)         # Vector(-2, 2)

v5 = v1 * 3       # Calls __mul__
print(v5)         # Vector(6, 9)

print(v1 == v2)   # False (calls __eq__)
print(v1 == Vector(2, 3))  # True
```

---

### Duck Typing (Python's Polymorphism)

```python
class Duck:
    def swim(self):
        return "Duck swimming"
    
    def fly(self):
        return "Duck flying"

class Airplane:
    def fly(self):
        return "Airplane flying"

class Whale:
    def swim(self):
        return "Whale swimming"

def make_it_fly(entity):
    """
    "If it walks like a duck and quacks like a duck, it's a duck"
    Doesn't care about type, only if it has fly() method
    """
    return entity.fly()

def make_it_swim(entity):
    """Works with any object that has swim() method"""
    return entity.swim()

# Different types, same interface
duck = Duck()
plane = Airplane()
whale = Whale()

print(make_it_fly(duck))    # Duck flying
print(make_it_fly(plane))   # Airplane flying
# make_it_fly(whale)        # AttributeError (no fly method)

print(make_it_swim(duck))   # Duck swimming
print(make_it_swim(whale))  # Whale swimming
# make_it_swim(plane)       # AttributeError (no swim method)
```

---

### Real-World Example: File Handlers

```python
class FileHandler:
    """Base handler"""
    
    def process(self, filename):
        """Must be overridden"""
        raise NotImplementedError

class CSVHandler(FileHandler):
    """Handle CSV files"""
    
    def process(self, filename):
        return f"Processing CSV file: {filename}"

class JSONHandler(FileHandler):
    """Handle JSON files"""
    
    def process(self, filename):
        return f"Processing JSON file: {filename}"

class XMLHandler(FileHandler):
    """Handle XML files"""
    
    def process(self, filename):
        return f"Processing XML file: {filename}"

def process_file(handler: FileHandler, filename: str):
    """
    Process file using any handler
    Polymorphic - works with any FileHandler subclass
    """
    return handler.process(filename)

# Different handlers, same interface
handlers = {
    '.csv': CSVHandler(),
    '.json': JSONHandler(),
    '.xml': XMLHandler()
}

files = ['data.csv', 'config.json', 'document.xml']

for file in files:
    ext = '.' + file.split('.')[-1]
    handler = handlers.get(ext)
    if handler:
        print(process_file(handler, file))
```

---

### Benefits of Polymorphism
1. **Flexibility**: Write code that works with different types
2. **Extensibility**: Add new types without changing existing code
3. **Maintainability**: Centralized logic for similar operations
4. **Code Reuse**: Same interface for different implementations

---

## 4. INHERITANCE

### Definition
Inheritance is a mechanism where a new class (child/derived class) inherits properties and methods from an existing class (parent/base class).

### Types of Inheritance

#### A. Single Inheritance

```python
class Vehicle:
    """Base class"""
    
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def start(self):
        return f"{self.brand} {self.model} is starting"
    
    def stop(self):
        return f"{self.brand} {self.model} is stopping"

class Car(Vehicle):
    """Derived class - inherits from Vehicle"""
    
    def __init__(self, brand, model, doors):
        super().__init__(brand, model)  # Call parent constructor
        self.doors = doors
    
    def honk(self):
        """New method specific to Car"""
        return "Beep beep!"

# Usage
car = Car("Toyota", "Camry", 4)

# Inherited methods
print(car.start())  # Toyota Camry is starting
print(car.stop())   # Toyota Camry is stopping

# Own method
print(car.honk())   # Beep beep!

# Inherited attributes
print(f"{car.brand} {car.model} has {car.doors} doors")
```

---

#### B. Multiple Inheritance

```python
class Flyer:
    """First parent"""
    
    def fly(self):
        return "Flying in the sky"

class Swimmer:
    """Second parent"""
    
    def swim(self):
        return "Swimming in water"

class Duck(Flyer, Swimmer):
    """Inherits from both Flyer and Swimmer"""
    
    def quack(self):
        return "Quack quack!"

# Usage
duck = Duck()
print(duck.fly())    # Flying in the sky
print(duck.swim())   # Swimming in water
print(duck.quack())  # Quack quack!
```

---

#### C. Multilevel Inheritance

```python
class Animal:
    """Base class"""
    
    def eat(self):
        return "Eating"

class Mammal(Animal):
    """Inherits from Animal"""
    
    def feed_milk(self):
        return "Feeding milk to young"

class Dog(Mammal):
    """Inherits from Mammal (which inherits from Animal)"""
    
    def bark(self):
        return "Woof!"

# Usage
dog = Dog()
print(dog.eat())        # Eating (from Animal)
print(dog.feed_milk())  # Feeding milk to young (from Mammal)
print(dog.bark())       # Woof! (from Dog)
```

---

#### D. Hierarchical Inheritance

```python
class Shape:
    """Base class"""
    
    def __init__(self, color):
        self.color = color

class Circle(Shape):
    """First child"""
    
    def __init__(self, color, radius):
        super().__init__(color)
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2

class Rectangle(Shape):
    """Second child"""
    
    def __init__(self, color, width, height):
        super().__init__(color)
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Triangle(Shape):
    """Third child"""
    
    def __init__(self, color, base, height):
        super().__init__(color)
        self.base = base
        self.height = height
    
    def area(self):
        return 0.5 * self.base * self.height

# Usage
circle = Circle("red", 5)
rect = Rectangle("blue", 4, 6)
triangle = Triangle("green", 3, 4)

print(f"{circle.color} circle area: {circle.area()}")     # red circle area: 78.5
print(f"{rect.color} rectangle area: {rect.area()}")      # blue rectangle area: 24
print(f"{triangle.color} triangle area: {triangle.area()}")  # green triangle area: 6.0
```

---

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

# MRO: D -> B -> C -> A
d = D()
print(d.method())  # "B" (from class B, first in MRO after D)

# Check MRO
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```

---

### super() Function

```python
class Parent:
    def __init__(self, name):
        self.name = name
        print(f"Parent init: {name}")
    
    def greet(self):
        return f"Hello from {self.name}"

class Child(Parent):
    def __init__(self, name, age):
        super().__init__(name)  # Call parent's __init__
        self.age = age
        print(f"Child init: {name}, {age}")
    
    def greet(self):
        parent_greet = super().greet()  # Call parent's greet
        return f"{parent_greet}, Age: {self.age}"

# Usage
child = Child("Alice", 10)
# Output:
# Parent init: Alice
# Child init: Alice, 10

print(child.greet())
# Output: Hello from Alice, Age: 10
```

---

### Real-World Example: Employee System

```python
class Employee:
    """Base Employee class"""
    
    def __init__(self, name, employee_id, salary):
        self.name = name
        self.employee_id = employee_id
        self.salary = salary
    
    def get_details(self):
        return f"ID: {self.employee_id}, Name: {self.name}, Salary: ${self.salary}"
    
    def calculate_bonus(self):
        """Base bonus calculation"""
        return self.salary * 0.10

class Manager(Employee):
    """Manager - inherits from Employee"""
    
    def __init__(self, name, employee_id, salary, team_size):
        super().__init__(name, employee_id, salary)
        self.team_size = team_size
    
    def calculate_bonus(self):
        """Override bonus calculation"""
        base_bonus = super().calculate_bonus()
        team_bonus = self.team_size * 100
        return base_bonus + team_bonus
    
    def get_details(self):
        """Extend parent method"""
        base_details = super().get_details()
        return f"{base_details}, Team Size: {self.team_size}, Role: Manager"

class Developer(Employee):
    """Developer - inherits from Employee"""
    
    def __init__(self, name, employee_id, salary, programming_languages):
        super().__init__(name, employee_id, salary)
        self.programming_languages = programming_languages
    
    def calculate_bonus(self):
        """Override bonus calculation"""
        base_bonus = super().calculate_bonus()
        skill_bonus = len(self.programming_languages) * 50
        return base_bonus + skill_bonus
    
    def get_details(self):
        """Extend parent method"""
        base_details = super().get_details()
        skills = ", ".join(self.programming_languages)
        return f"{base_details}, Skills: {skills}, Role: Developer"

# Usage
manager = Manager("Alice", "M001", 80000, 5)
developer = Developer("Bob", "D001", 70000, ["Python", "Java", "C++"])

print(manager.get_details())
# ID: M001, Name: Alice, Salary: $80000, Team Size: 5, Role: Manager

print(f"Manager bonus: ${manager.calculate_bonus()}")
# Manager bonus: $8500 (8000 base + 500 team)

print(developer.get_details())
# ID: D001, Name: Bob, Salary: $70000, Skills: Python, Java, C++, Role: Developer

print(f"Developer bonus: ${developer.calculate_bonus()}")
# Developer bonus: $7150 (7000 base + 150 skills)
```

---

### Benefits of Inheritance
1. **Code Reuse**: Inherit existing functionality
2. **Extensibility**: Add new features easily
3. **Maintainability**: Changes in parent affect all children
4. **Hierarchical Classification**: Model real-world relationships

---

## OOP Best Practices

### 1. Composition over Inheritance

```python
# Instead of inheritance
class Engine:
    def start(self):
        return "Engine started"

class Car:
    """Uses composition instead of inheriting from Engine"""
    
    def __init__(self):
        self.engine = Engine()  # Composition
    
    def start(self):
        return self.engine.start()

car = Car()
print(car.start())  # Engine started
```

---

### 2. Single Responsibility Principle

```python
# Each class has one responsibility

class User:
    """Responsible only for user data"""
    def __init__(self, name, email):
        self.name = name
        self.email = email

class UserValidator:
    """Responsible only for validation"""
    @staticmethod
    def validate_email(email):
        return '@' in email

class UserRepository:
    """Responsible only for data storage"""
    def save(self, user):
        return f"Saving {user.name} to database"
```

---

### 3. Open/Closed Principle

```python
# Open for extension, closed for modification

class Shape:
    def area(self):
        pass

# Extend, don't modify
class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2

class Square(Shape):
    def __init__(self, side):
        self.side = side
    
    def area(self):
        return self.side ** 2
```

---

## Exam Tips

### 1. MCQ Questions

**Q**: What is abstraction?
**A**: Hiding implementation details, showing only essential features

**Q**: What does encapsulation provide?
**A**: Data hiding and controlled access

**Q**: What is polymorphism?
**A**: Same interface, multiple implementations

**Q**: What is inheritance used for?
**A**: Code reuse and creating hierarchical relationships

**Q**: What does super() do?
**A**: Calls parent class method

**Q**: Difference between public and private?
**A**: Public accessible everywhere, private (\_\_) only within class

### 2. Four Pillars Summary

| Pillar | Key Concept | Example |
|--------|-------------|---------|
| **Abstraction** | Hide complexity | Abstract classes, interfaces |
| **Encapsulation** | Data hiding | Private attributes, getters/setters |
| **Polymorphism** | Many forms | Method overriding, operator overloading |
| **Inheritance** | Code reuse | Parent-child class relationship |

### Key Points
1. **Abstraction**: Abstract classes with @abstractmethod
2. **Encapsulation**: Use \_\_ for private, @ property for controlled access
3. **Polymorphism**: Override methods, duck typing
4. **Inheritance**: Use super() to call parent methods
5. **MRO**: Method Resolution Order matters in multiple inheritance
6. **Composition**: Often better than inheritance
7. **SOLID Principles**: Write maintainable OOP code
