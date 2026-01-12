# Object-Oriented Programming - Multiple Choice Questions

## Comprehensive MCQ Set with Answers & Explanations

---

## ABSTRACTION

### Question 1: Logic Flow Completion
**Complete the abstract class implementation:**

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    # ??? What should be here for perimeter?

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
```

A) `def perimeter(self): pass`  
B) `@abstractmethod def perimeter(self): pass`  
C) Circle can be instantiated as is  
D) `def perimeter(self): return 0`

**Answer: B**

**Explanation:**  
To make Circle implement perimeter, Shape must declare it as abstract method using `@abstractmethod`. Without this, Circle doesn't need to implement perimeter. Option A makes it concrete (subclasses can skip it), Option C is wrong (Circle missing perimeter would cause error if Shape has it abstract), Option D makes it concrete.

---

### Question 2: Debugging
**Why does this code fail?**

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass

car = Vehicle()
```

A) Missing import  
B) Cannot instantiate abstract class  
C) Missing __init__ method  
D) start() should have implementation

**Answer: B**

**Explanation:**  
**Cannot instantiate abstract class** directly. Abstract classes serve as templates and must be subclassed. Attempting to create instance raises `TypeError: Can't instantiate abstract class Vehicle with abstract methods start`.

Correct usage:
```python
class Car(Vehicle):
    def start(self):
        return "Car starting"

car = Car()  # ✓ Works
```

---

### Question 3: Dry Run Output
**What is the output?**

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def sound(self):
        pass
    
    def describe(self):
        return f"{self.__class__.__name__} says {self.sound()}"

class Dog(Animal):
    def sound(self):
        return "Woof"

dog = Dog()
print(dog.describe())
```

A) `Animal says Woof`  
B) `Dog says Woof`  
C) `TypeError`  
D) `Woof`

**Answer: B**

**Explanation:**  
- `self.__class__.__name__` returns the actual class name ("Dog")
- `self.sound()` calls Dog's implementation ("Woof")
- Result: "Dog says Woof"

Even though describe() is in Animal, it uses the child class name and method when called on Dog instance.

---

### Question 4: Syntax Understanding
**What makes this a valid abstraction?**

```python
from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass
    
    @abstractmethod
    def validate(self):
        pass
    
    def log(self, message):
        print(f"Log: {message}")
```

A) Has all abstract methods  
B) Has mix of abstract and concrete methods  
C) Inherits from ABC  
D) All of the above

**Answer: B**

**Explanation:**  
Valid abstraction can have:
- Abstract methods (must be implemented by subclasses)
- Concrete methods (optional, can be used/overridden)

Here: `process_payment()` and `validate()` are abstract, `log()` is concrete. This provides interface contract while offering shared functionality.

---

### Question 5: Data Analysis
**Which design benefits most from abstraction?**

A) Single class with all logic  
B) Plugin system with multiple implementations  
C) Utility functions collection  
D) Static data storage

**Answer: B**

**Explanation:**  
**Plugin systems** benefit most from abstraction:
- Define abstract interface (e.g., Plugin class)
- Multiple implementations (FilePlugin, DatabasePlugin, etc.)
- System works with abstract interface, doesn't care about specific implementation
- Easy to add new plugins without modifying core code

Single class, utilities, and static storage don't leverage polymorphism.

---

## ENCAPSULATION

### Question 6: Logic Flow Completion
**Complete the property setter:**

```python
class Employee:
    def __init__(self, name, salary):
        self._name = name
        self._salary = salary
    
    @property
    def salary(self):
        return self._salary
    
    # ??? Missing setter
```

A) `def salary(self, value): self._salary = value`  
B) `@salary.setter def salary(self, value): self._salary = value if value > 0 else raise ValueError()`  
C) `@salary.setter def salary(self, value): self._salary = value`  
D) `@setter def salary(self, value): self._salary = value`

**Answer: C**

**Explanation:**  
Correct property setter syntax:
```python
@salary.setter
def salary(self, value):
    if value > 0:
        self._salary = value
    else:
        raise ValueError("Salary must be positive")
```

Option B has correct decorator but wrong raise syntax. Must use `@property_name.setter`, not `@setter`.

---

### Question 7: Debugging
**What's wrong with this encapsulation?**

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance
    
    def deposit(self, amount):
        self.__balance += amount
    
    def get_balance(self):
        return self.balance
```

A) Should use `self.__balance` in get_balance  
B) deposit() should validate amount  
C) __balance should be _balance  
D) Both A and B

**Answer: D**

**Explanation:**  
Two bugs:
1. **`return self.balance`** should be `return self.__balance` (wrong attribute name causes AttributeError)
2. **No validation** in deposit() - should check `amount > 0`

Option C is not a bug - `__balance` is fine for private attribute (name mangling).

---

### Question 8: Dry Run Output
**What is the output?**

```python
class MyClass:
    def __init__(self):
        self.public = "public"
        self._protected = "protected"
        self.__private = "private"

obj = MyClass()
print(obj.public)
print(obj._protected)
try:
    print(obj.__private)
except AttributeError:
    print("AttributeError")
```

A) public, protected, private  
B) public, protected, AttributeError  
C) public, AttributeError, AttributeError  
D) Error on line 2

**Answer: B**

**Explanation:**  
- `obj.public` ✓ Works (public)
- `obj._protected` ✓ Works (protected by convention, not enforced)
- `obj.__private` ✗ AttributeError (name mangled to `_MyClass__private`)

Access private as: `obj._MyClass__private`

---

### Question 9: Syntax Understanding
**What does this demonstrate?**

```python
class Password:
    def __init__(self):
        self.__password = None
    
    def set_password(self, pwd):
        self.__password = hash(pwd)
    
    def check_password(self, pwd):
        return hash(pwd) == self.__password
```

A) Abstraction  
B) Encapsulation and data hiding  
C) Polymorphism  
D) Inheritance

**Answer: B**

**Explanation:**  
This demonstrates **encapsulation and data hiding**:
- Private attribute `__password` cannot be accessed directly
- Controlled access through methods (`set_password`, `check_password`)
- Internal representation (hashed) hidden from user
- Validates and protects sensitive data

---

### Question 10: Data Analysis
**Which access modifier is most appropriate?**

```python
class Config:
    # Internal helper, never used by external code
    def _parse_config_file(self):
        pass
    
    # Public API method
    def load_config(self):
        self._parse_config_file()
```

A) Make _parse_config_file public  
B) Make _parse_config_file private (__)  
C) Keep as protected (_)  
D) Use static method

**Answer: C**

**Explanation:**  
**Protected (`_`)** is appropriate:
- Internal implementation detail
- Not meant for external use
- May be useful for subclasses
- Convention signals "internal, may change"

Private (`__`) would prevent subclass access unnecessarily. Public removes the "internal" signal.

---

## POLYMORPHISM

### Question 11: Logic Flow Completion
**Complete the polymorphic method:**

```python
class Animal:
    def speak(self):
        return "Some sound"

class Dog(Animal):
    # ??? Override speak method
```

A) `def speak(): return "Woof"`  
B) `def speak(self): return "Woof"`  
C) `def speak(self, sound): return sound`  
D) `@override def speak(self): return "Woof"`

**Answer: B**

**Explanation:**  
Override requires same signature with `self`:
```python
def speak(self):
    return "Woof"
```

Option A missing self (error), Option C changes signature (breaks polymorphism), Option D has invalid decorator (Python doesn't require @override).

---

### Question 12: Debugging
**Why does this operator overloading fail?**

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v3 = v1 + v2
print(v3 + 5)
```

A) __add__ doesn't handle int  
B) __str__ format is wrong  
C) Should use __repr__  
D) No error

**Answer: A**

**Explanation:**  
`v3 + 5` fails because `__add__` expects Vector, not int:
```python
# Causes: AttributeError: 'int' object has no attribute 'x'
```

Fix: Check type or add support:
```python
def __add__(self, other):
    if isinstance(other, Vector):
        return Vector(self.x + other.x, self.y + other.y)
    elif isinstance(other, (int, float)):
        return Vector(self.x + other, self.y + other)
    return NotImplemented
```

---

### Question 13: Dry Run Output
**What is the output?**

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

d = D()
print(d.method())
print(D.__mro__)
```

A) "A" then MRO  
B) "B" then MRO  
C) "C" then MRO  
D) Error

**Answer: B**

**Explanation:**  
Method Resolution Order (MRO): D → B → C → A → object

`d.method()` returns "B" (first in MRO after D)

MRO output:
```
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, 
 <class '__main__.A'>, <class 'object'>)
```

Python uses C3 linearization for MRO.

---

### Question 14: Syntax Understanding
**What type of polymorphism is this?**

```python
class Calculator:
    def add(self, *args):
        return sum(args)

calc = Calculator()
print(calc.add(1, 2))      # 3
print(calc.add(1, 2, 3))   # 6
print(calc.add(1, 2, 3, 4)) # 10
```

A) Runtime polymorphism  
B) Method overloading simulation  
C) Operator overloading  
D) Duck typing

**Answer: B**

**Explanation:**  
This simulates **method overloading** (compile-time polymorphism):
- Python doesn't support traditional overloading
- Using `*args` to accept variable number of arguments
- Same method name, different behavior based on argument count
- Achieves overloading effect

---

### Question 15: Data Analysis
**Which polymorphism approach is best?**

```python
# Scenario: Process different file types
files = [CSVFile(), JSONFile(), XMLFile()]
```

A) Check type with isinstance() in loop  
B) Abstract FileProcessor with process() method  
C) Separate function for each type  
D) Dictionary mapping types to handlers

**Answer: B**

**Explanation:**  
**Abstract base class** with polymorphic method is best:

```python
class FileProcessor(ABC):
    @abstractmethod
    def process(self):
        pass

for file in files:
    file.process()  # Polymorphic call
```

Benefits:
- Clean, maintainable code
- Easy to add new file types
- No type checking needed
- Follows Open/Closed Principle

Options A and C violate OOP principles, D is functional approach (less OOP).

---

## INHERITANCE

### Question 16: Logic Flow Completion
**Complete the constructor:**

```python
class Vehicle:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

class Car(Vehicle):
    def __init__(self, brand, model, doors):
        # ??? Missing line to call parent constructor
        self.doors = doors
```

A) `Vehicle.__init__(self, brand, model)`  
B) `super().__init__(brand, model)`  
C) Both A and B work  
D) `self.__init__(brand, model)`

**Answer: C**

**Explanation:**  
Both work, but `super()` is preferred:

```python
# Option A: Direct parent class call
Vehicle.__init__(self, brand, model)

# Option B: super() (preferred)
super().__init__(brand, model)
```

`super()` is better:
- Works with multiple inheritance
- Follows MRO correctly
- More maintainable

---

### Question 17: Debugging
**What's wrong with this inheritance?**

```python
class Parent:
    def __init__(self, name):
        self.name = name

class Child(Parent):
    def __init__(self, name, age):
        self.age = age
    
    def display(self):
        return f"{self.name} is {self.age}"

c = Child("Alice", 10)
print(c.display())
```

A) Missing super().__init__(name)  
B) display() syntax error  
C) Should not inherit __init__  
D) No error

**Answer: A**

**Explanation:**  
Child's `__init__` doesn't call parent's `__init__`, so `self.name` is never set:

```python
AttributeError: 'Child' object has no attribute 'name'
```

Fix:
```python
def __init__(self, name, age):
    super().__init__(name)
    self.age = age
```

---

### Question 18: Dry Run Output
**What is the output?**

```python
class A:
    def __init__(self):
        print("A init")

class B(A):
    def __init__(self):
        print("B init")
        super().__init__()

class C(A):
    def __init__(self):
        print("C init")
        super().__init__()

class D(B, C):
    def __init__(self):
        print("D init")
        super().__init__()

d = D()
```

A) D init, B init, A init  
B) D init, B init, C init, A init  
C) D init, A init  
D) Error

**Answer: B**

**Explanation:**  
Output with MRO (D → B → C → A):
```
D init
B init
C init
A init
```

Each class calls `super().__init__()`, which follows MRO:
1. D calls super → B
2. B calls super → C (not A!)
3. C calls super → A
4. A is end of chain

This is the diamond problem, solved by MRO.

---

### Question 19: Syntax Understanding
**What inheritance type is this?**

```python
class Animal:
    pass

class Mammal(Animal):
    pass

class Dog(Mammal):
    pass
```

A) Single inheritance  
B) Multiple inheritance  
C) Multilevel inheritance  
D) Hierarchical inheritance

**Answer: C**

**Explanation:**  
**Multilevel inheritance**: Chain of inheritance
- Animal → Mammal → Dog
- Each class inherits from previous

Single: A → B (one level)
Multiple: C(A, B) (multiple parents)
Hierarchical: A → B, A → C (one parent, multiple children)

---

### Question 20: Data Analysis
**When should you favor composition over inheritance?**

```python
# Scenario: Car needs an Engine
```

A) Never, always use inheritance  
B) When "has-a" relationship (Car has Engine)  
C) When "is-a" relationship (Car is Vehicle)  
D) When implementing interfaces

**Answer: B**

**Explanation:**  
**Favor composition for "has-a"** relationships:

```python
# ✓ Composition (has-a)
class Car:
    def __init__(self):
        self.engine = Engine()  # Car HAS an Engine

# ✗ Inheritance (is-a) - wrong!
class Car(Engine):  # Car IS an Engine? No!
    pass
```

Use inheritance for "is-a" (Car is-a Vehicle).

Composition is more flexible:
- Easier to change engine type
- Can swap at runtime
- Looser coupling

---

## SOLID PRINCIPLES

### Question 21: Logic Flow Completion
**Apply Single Responsibility Principle:**

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
    
    # What should NOT be here according to SRP?
```

A) `def update_profile(self):` ✓  
B) `def save_to_database(self):` ✗  
C) `def send_email(self):` ✗  
D) Both B and C

**Answer: D**

**Explanation:**  
**Single Responsibility Principle**: Class should have one reason to change.

User class should handle user data only:
- ✓ Profile operations
- ✗ Database operations (separate UserRepository)
- ✗ Email operations (separate EmailService)

Split responsibilities:
```python
class User:
    # User data only

class UserRepository:
    def save(self, user):  # Database logic

class EmailService:
    def send(self, user):  # Email logic
```

---

### Question 22: Debugging
**This violates which SOLID principle?**

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def set_width(self, width):
        self.width = width
    
    def set_height(self, height):
        self.height = height

class Square(Rectangle):
    def set_width(self, width):
        self.width = width
        self.height = width  # Must keep square property
    
    def set_height(self, height):
        self.width = height
        self.height = height

def area_test(r: Rectangle):
    r.set_width(5)
    r.set_height(4)
    assert r.width * r.height == 20  # Fails for Square!
```

A) Single Responsibility  
B) Open/Closed  
C) Liskov Substitution  
D) Interface Segregation

**Answer: C**

**Explanation:**  
Violates **Liskov Substitution Principle (LSP)**:
- Square cannot substitute Rectangle
- Changes width/height together breaks expected behavior
- `area_test()` works for Rectangle but fails for Square

LSP: Subclass should work wherever parent works. Square changes Rectangle's behavior unexpectedly.

---

### Question 23: Dry Run Output
**What happens with this Interface Segregation violation?**

```python
class Worker:
    def work(self):
        pass
    
    def eat(self):
        pass

class Human(Worker):
    def work(self):
        return "Working"
    
    def eat(self):
        return "Eating"

class Robot(Worker):
    def work(self):
        return "Working"
    
    def eat(self):
        raise NotImplementedError("Robots don't eat")

robot = Robot()
print(robot.work())
print(robot.eat())
```

A) Working, Eating  
B) Working, NotImplementedError  
C) Both return Working  
D) Compilation error

**Answer: B**

**Explanation:**  
Output:
```
Working
NotImplementedError: Robots don't eat
```

**Interface Segregation Principle** violation:
- Robot forced to implement `eat()` it doesn't need
- Throws exception instead of not having method

Better design:
```python
class Workable:
    def work(self): pass

class Eatable:
    def eat(self): pass

class Human(Workable, Eatable):  # Implements both
class Robot(Workable):  # Only work
```

---

### Question 24: Syntax Understanding
**Which SOLID principle does this follow?**

```python
class Database(ABC):
    @abstractmethod
    def save(self, data):
        pass

class UserService:
    def __init__(self, database: Database):
        self.db = database
    
    def save_user(self, user):
        self.db.save(user)

# Can use any database implementation
service = UserService(MySQLDatabase())
service = UserService(MongoDatabase())
```

A) Single Responsibility  
B) Open/Closed  
C) Liskov Substitution  
D) Dependency Inversion

**Answer: D**

**Explanation:**  
**Dependency Inversion Principle (DIP)**:
- Depend on abstractions (Database), not concretions (MySQL, Mongo)
- High-level module (UserService) doesn't depend on low-level modules
- Both depend on abstraction (Database interface)

Benefits:
- Easy to switch database
- Loose coupling
- Testable (can inject mock database)

---

### Question 25: Data Analysis
**Which principle is most important for microservices?**

A) Single Responsibility - each service does one thing  
B) Open/Closed - extend without modifying  
C) Liskov Substitution - service contracts  
D) All equally important

**Answer: A**

**Explanation:**  
**Single Responsibility** is crucial for microservices:
- Each service handles one business capability
- Clear boundaries
- Independent deployment
- Easier to scale and maintain

Example:
- User Service (user management)
- Payment Service (payments)
- Notification Service (notifications)

Other principles also apply, but SRP defines the fundamental microservices architecture pattern.

---

## CLASS DESIGN

### Question 26: Logic Flow Completion
**Complete the singleton pattern:**

```python
class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            # ??? Missing lines
        return cls._instance
```

A) `cls._instance = object.__new__(cls)`  
B) `cls._instance = super().__new__(cls)`  
C) Both A and B work  
D) `cls._instance = cls()`

**Answer: C**

**Explanation:**  
Both create single instance:

```python
# Option A
cls._instance = object.__new__(cls)

# Option B (preferred)
cls._instance = super().__new__(cls)
```

Option D causes infinite recursion (calls `__new__` again).

Singleton ensures only one instance exists across application.

---

### Question 27: Debugging
**What's wrong with this class method?**

```python
class Counter:
    count = 0
    
    @classmethod
    def increment(self):
        self.count += 1
    
    @classmethod
    def get_count(cls):
        return cls.count
```

A) Should use `cls` not `self` in increment  
B) count should be instance variable  
C) @classmethod decorator is wrong  
D) get_count should use self

**Answer: A**

**Explanation:**  
Class methods use `cls`, not `self`:

```python
@classmethod
def increment(cls):  # Not self!
    cls.count += 1
```

`self` refers to instance (instance methods), `cls` refers to class (class methods).

---

### Question 28: Dry Run Output
**What is the output?**

```python
class MyClass:
    count = 0
    
    def __init__(self):
        MyClass.count += 1
    
    @classmethod
    def get_count(cls):
        return cls.count

obj1 = MyClass()
obj2 = MyClass()
obj3 = MyClass()
print(MyClass.get_count())
```

A) 0  
B) 1  
C) 3  
D) Error

**Answer: C**

**Explanation:**  
Class variable `count` is shared across all instances:
- obj1 created: count = 1
- obj2 created: count = 2
- obj3 created: count = 3

`MyClass.get_count()` returns 3.

Class variables track data across all instances (like counting how many objects created).

---

### Question 29: Syntax Understanding
**What does `__call__` enable?**

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor
    
    def __call__(self, x):
        return x * self.factor

double = Multiplier(2)
print(double(5))
```

A) Makes class iterable  
B) Makes instance callable like function  
C) Enables property access  
D) Overloads multiplication

**Answer: B**

**Explanation:**  
`__call__` makes instances **callable like functions**:

```python
double = Multiplier(2)
double(5)  # Calls double.__call__(5) → returns 10
```

Creates "function objects" that maintain state:
- Object remembers factor
- Can be called with ()
- Useful for callbacks, decorators, functors

---

### Question 30: Data Analysis
**Which design pattern is most appropriate?**

```python
# Scenario: Create different types of database connections
# Requirements:
# - Abstract creation logic
# - Support MySQL, PostgreSQL, MongoDB
# - Client doesn't know concrete classes
```

A) Singleton - one instance  
B) Factory - create objects  
C) Observer - notify changes  
D) Decorator - add functionality

**Answer: B**

**Explanation:**  
**Factory Pattern** is perfect:

```python
class DatabaseFactory:
    @staticmethod
    def create_database(db_type):
        if db_type == "mysql":
            return MySQLDatabase()
        elif db_type == "postgresql":
            return PostgreSQLDatabase()
        elif db_type == "mongodb":
            return MongoDatabase()
        raise ValueError("Unknown database type")

# Client code
db = DatabaseFactory.create_database("mysql")
```

Benefits:
- Encapsulates object creation
- Client doesn't know concrete classes
- Easy to add new database types
- Follows Open/Closed Principle

---

**Total Questions: 30**  
**Topics Covered**: Abstraction (5), Encapsulation (5), Polymorphism (5), Inheritance (5), SOLID Principles (5), Class Design (5)  
**Question Types**: Logic Flow, Debugging, Dry Run, Syntax Understanding, Data Analysis
