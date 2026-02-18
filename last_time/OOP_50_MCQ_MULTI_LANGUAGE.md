# OOP - 50 MCQs Multi-Language Edition
## **C++ | Java | Python - Complete Coverage with Explanations**

### 📋 **EXAM PATTERN ALIGNMENT**
This MCQ set covers all exam question types:
- ✅ **Logic Flow Completion** - Questions 1, 2, 4, 12, 38
- ✅ **Debugging** - Questions 3, 6, 9, 14, 25, 40
- ✅ **Syntax Understanding** - Questions 7, 8, 11, 16, 26, 31
- ✅ **Program Dry Run Output** - Questions 17, 19, 22, 27, 28, 30, 33, 36, 37, 39, 42, 48
- ✅ **Data Analysis** - Questions 13, 21, 24, 34, 41, 43, 44, 45, 46, 47, 50
- ✅ **Conceptual** - Questions 5, 10, 15, 18, 20, 23, 29, 32, 35, 49

---

## SECTION 1: ABSTRACTION (MCQs 1-8)

### **Question 1: Abstract Class Instantiation**

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass

class Car(Vehicle):
    def start(self):
        return "Car started"

# What's the output?
obj = Vehicle()
```

**A)** Creates Vehicle object successfully  
**B)** TypeError: Can't instantiate abstract class  
**C)** None  
**D)** Compilation error

**Answer: B**

**Explanation:**  
Abstract classes cannot be instantiated in Python. The ABC (Abstract Base Class) with `@abstractmethod` prevents direct instantiation. You must create a concrete subclass (like Car) that implements all abstract methods. This is true across all languages:
- **C++**: Pure virtual functions prevent instantiation
- **Java**: `abstract class` keyword prevents instantiation  
- **Python**: ABC with @abstractmethod prevents instantiation

---

### **Question 2: Multiple Interfaces**

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    public void fly() { System.out.println("Flying"); }
    // Missing swim method
}
```

**What happens?**

**A)** Compiles successfully  
**B)** Runtime error  
**C)** Compilation error: must implement swim()  
**D)** Duck can be instantiated

**Answer: C**

**Explanation:**  
In Java, when a class implements an interface, it MUST implement all methods from that interface. Since Duck implements Swimmable but doesn't define swim(), it's a compilation error. 

**Language Comparison:**
- **Java**: Compile-time check for interface implementation
- **C++**: Pure virtual functions must be implemented
- **Python**: No compile-time check, but will fail at instantiation if abstract methods missing

---

### **Question 3: Abstract vs Interface - C++**

```cpp
class Shape {
public:
    virtual double area() = 0;  // Pure virtual
    double perimeter() {        // Concrete method
        return 0.0;
    }
};
```

**What can you say about Shape?**

**A)** It's an interface  
**B)** It's a concrete class  
**C)** It's an abstract class with mixed methods  
**D)** Invalid syntax

**Answer: C**

**Explanation:**  
Shape is an abstract class (has at least one pure virtual function `area() = 0`). However, unlike interfaces, it also has a concrete method `perimeter()`. 

**Key Differences:**
- **Interface**: ALL methods are abstract
- **Abstract Class**: Can mix abstract and concrete methods
- **C++**: No separate `interface` keyword, use pure virtual classes
- **Java**: Has both `interface` and `abstract class`
- **Python**: Uses ABC for both concepts

---

### **Question 4: Java Interface Default Methods**

```java
interface Drawable {
    default void draw() {
        System.out.println("Drawing...");
    }
    void resize();  // Abstract
}

class Circle implements Drawable {
    public void resize() { }
}

// Usage:
Circle c = new Circle();
c.draw();
```

**What's the output?**

**A)** Compilation error  
**B)** Runtime error  
**C)** Drawing...  
**D)** Nothing printed

**Answer: C**

**Explanation:**  
Java 8+ allows `default` methods in interfaces, which provide default implementations. Circle inherits this default implementation without needing to override it. This makes interfaces more flexible.

**Language Comparison:**
- **Java 8+**: Interfaces can have default and static methods
- **C++**: No equivalent, use abstract class
- **Python**: ABC classes naturally support concrete methods

---

### **Question 5: Python ABC Override Check**

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def sound(self):
        pass

class Dog(Animal):
    def sound(self):
        return "Bark"
    
class Cat(Animal):
    pass  # Missing sound implementation

# What happens?
try:
    d = Dog()  # Line 1
    c = Cat()  # Line 2
except TypeError as e:
    print("Error")
```

**What's the output?**

**A)** Both objects created successfully  
**B)** Error at Line 1  
**C)** Error at Line 2  
**D)** No output

**Answer: C**

**Explanation:**  
Dog implements `sound()`, so Line 1 succeeds. Cat doesn't implement the abstract method `sound()`, so Line 2 raises TypeError at instantiation time. Python checks abstract method implementation when creating objects, not at class definition time.

---

### **Question 6: C++ Virtual Destructor**

```cpp
class Base {
public:
    ~Base() { }  // Non-virtual destructor
};

class Derived : public Base {
public:
    ~Derived() { }
};

Base* ptr = new Derived();
delete ptr;  // What happens?
```

**A)** Both destructors called: Derived, then Base  
**B)** Only Base destructor called (memory leak)  
**C)** Only Derived destructor called  
**D)** Compilation error

**Answer: B**

**Explanation:**  
Without `virtual` keyword on Base destructor, delete on Base pointer only calls Base destructor, NOT Derived destructor. This causes memory leaks if Derived allocated resources.

**CRITICAL RULE**: Always make destructor virtual in base classes meant for polymorphism:
```cpp
virtual ~Base() { }  // Correct
```

**Language Comparison:**
- **C++**: Must explicitly use `virtual` for destructor
- **Java**: No destructors, garbage collector handles cleanup
- **Python**: `__del__` called automatically, no virtual needed

---

### **Question 7: Interface Variables**

```java
interface Constants {
    int MAX_SIZE = 100;  // What are the implicit modifiers?
    void process();
}
```

**The variable MAX_SIZE is implicitly:**

**A)** public  
**B)** static  
**C)** final  
**D)** All of the above

**Answer: D**

**Explanation:**  
In Java, all interface variables are implicitly `public static final`. You cannot change them, and they belong to the interface itself.

```java
// These are equivalent:
int MAX_SIZE = 100;
public static final int MAX_SIZE = 100;
```

**Language Comparison:**
- **Java**: Interface variables automatically public static final
- **C++**: Pure virtual classes can have any type of members
- **Python**: No such restriction, convention-based

---

### **Question 8: Multi-Language Abstract Syntax**

**Which correctly defines an abstract method in respective languages?**

**A)** C++: `abstract void method();`  
**B)** Java: `void method() = 0;`  
**C)** Python: `@abstractmethod def method(self): pass`  
**D)** All of the above

**Answer: C**

**Explanation:**  
**Correct Syntax:**
- **C++**: `virtual void method() = 0;` (pure virtual)
- **Java**: `abstract void method();` (in abstract class or interface)
- **Python**: `@abstractmethod def method(self): pass`

Option A and B have wrong syntax for their respective languages.

---

## SECTION 2: ENCAPSULATION (MCQs 9-16)

### **Question 9: Access Modifiers - C++**

```cpp
class MyClass {
private:
    int a;
protected:
    int b;
public:
    int c;
};

class Child : public MyClass {
    void test() {
        a = 10;  // Line 1
        b = 20;  // Line 2
        c = 30;  // Line 3
    }
};
```

**Which lines cause compilation errors?**

**A)** Line 1 only  
**B)** Line 1 and 2  
**C)** Line 2 and 3  
**D)** No errors

**Answer: A**

**Explanation:**  
- **Line 1**: `a` is private - NOT accessible in derived class ❌
- **Line 2**: `b` is protected - accessible in derived class ✅
- **Line 3**: `c` is public - accessible everywhere ✅

**Access Summary:**
| Modifier | Same Class | Derived Class | Outside |
|----------|-----------|--------------|---------|
| private | ✅ | ❌ | ❌ |
| protected | ✅ | ✅ | ❌ |
| public | ✅ | ✅ | ✅ |

---

### **Question 10: Python Name Mangling**

```python
class Secret:
    def __init__(self):
        self.public = "I'm public"
        self._protected = "I'm protected"
        self.__private = "I'm private"

obj = Secret()
print(obj.public)      # Line 1
print(obj._protected)  # Line 2
print(obj.__private)   # Line 3
```

**What happens at Line 3?**

**A)** Prints "I'm private"  
**B)** AttributeError  
**C)** Prints encrypted value  
**D)** None

**Answer: B**

**Explanation:**  
Python name mangling changes `__private` to `_Secret__private` internally. Accessing with original name causes AttributeError.

**Access:**
```python
print(obj._Secret__private)  # Works! Prints "I'm private"
```

**Python Convention:**
- `public`: Normal attribute
- `_protected`: Convention (not enforced)
- `__private`: Name mangling (still accessible with _ClassName__attr)

**Note:** Unlike C++/Java, Python has NO true private members!

---

### **Question 11: Java Package Access**

```java
// File: package1/ClassA.java
package package1;
public class ClassA {
    int x = 10;  // What's the access modifier?
}

// File: package2/ClassB.java
package package2;
import package1.ClassA;
public class ClassB {
    void test() {
        ClassA a = new ClassA();
        System.out.println(a.x);  // Can we access x?
    }
}
```

**Can ClassB access x?**

**A)** Yes, x is public  
**B)** Yes, x is protected  
**C)** No, x has default (package-private) access  
**D)** Compilation error

**Answer: C**

**Explanation:**  
In Java, when no access modifier is specified, it's **default** (package-private). This means accessible only within the same package. ClassA and ClassB are in different packages, so x is not accessible.

**Java Access Levels (Narrowest to Widest):**
1. **private**: Class only
2. **default**: Package only
3. **protected**: Package + subclasses
4. **public**: Everywhere

---

### **Question 12: Property Decorators - Python**

```python
class Person:
    def __init__(self):
        self._age = 0
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value

p = Person()
p.age = -5  # What happens?
```

**What's the result?**

**A)** age set to -5  
**B)** ValueError raised  
**C)** age set to 0  
**D)** AttributeError

**Answer: B**

**Explanation:**  
The `@age.setter` decorator allows validation before setting the value. When `p.age = -5` is executed, the setter method runs, checks the condition, and raises ValueError.

**Benefits of @property:**
1. Validate data before setting
2. Compute values dynamically
3. Maintain backward compatibility
4. Pythonic way (no getAge/setAge methods)

**Equivalent in other languages:**
- **Java**: `getAge()` and `setAge(int age)` methods
- **C++**: `getAge()` and `setAge(int age)` methods

---

### **Question 13: Getters/Setters Convention**

**Which follows proper encapsulation?**

**A)**
```java
public class Person {
    public int age;
}
```

**B)**
```java
public class Person {
    private int age;
    public int getAge() { return age; }
    public void setAge(int a) { age = a; }
}
```

**C)**
```java
public class Person {
    private int age;
    public int getAge() { return age; }
    public void setAge(int a) {
        if (a > 0) age = a;
    }
}
```

**D)** Both B and C

**Answer: C**

**Explanation:**  
Option C is best because:
1. ✅ Private field (encapsulation)
2. ✅ Public getter (controlled access)
3. ✅ Public setter with validation (data integrity)

Option B lacks validation. Option A exposes field directly (no encapsulation).

**Best Practice**: Always validate in setters to maintain data integrity.

---

### **Question 14: C++ Friend Function**

```cpp
class Box {
private:
    int width;
    
public:
    Box(int w) : width(w) {}
    friend void printWidth(Box& b);
};

void printWidth(Box& b) {
    cout << b.width;  // Can access private member
}
```

**Friend function violates which OOP principle?**

**A)** Abstraction  
**B)** Encapsulation  
**C)** Inheritance  
**D)** Polymorphism

**Answer: B**

**Explanation:**  
`friend` keyword allows external functions to access private members, breaking encapsulation. While sometimes necessary, it should be used sparingly.

**Language Comparison:**
- **C++**: Has `friend` keyword
- **Java**: No friend concept
- **Python**: No friend (already accessible via name mangling)

**When to use:** Operator overloading, tightly coupled classes (rare cases)

---

### **Question 15: Mutable vs Immutable**

```java
public class Person {
    private final String name;
    private int age;
    
    public Person(String n, int a) {
        name = n;
        age = a;
    }
    
    public void setName(String n) {
        name = n;  // What happens?
    }
}
```

**What's the result?**

**A)** Compiles successfully  
**B)** Compilation error: cannot assign to final variable  
**C)** Runtime error  
**D)** name is null

**Answer: B**

**Explanation:**  
`final` keyword makes variable immutable after initialization. In constructor it's fine, but in `setName()` it's a compilation error.

**Language Comparison:**
- **Java**: `final` keyword
- **C++**: `const` keyword
- **Python**: No built-in keyword (use naming convention or dataclasses with frozen=True)

---

### **Question 16: Protected Inheritance - C++**

```cpp
class Base {
public:
    int x;
};

class Derived : protected Base {
};

int main() {
    Derived d;
    d.x = 10;  // Can we access x?
}
```

**What happens?**

**A)** Compiles successfully  
**B)** Compilation error: x is protected in Derived  
**C)** Runtime error  
**D)** x is private in Derived

**Answer: B**

**Explanation:**  
**Protected inheritance** in C++ makes all public members of Base become protected in Derived. So `x` (public in Base) becomes protected in Derived, making it inaccessible outside Derived.

**C++ Inheritance Access:**
```cpp
class D : public B     // public stays public
class D : protected B  // public becomes protected
class D : private B    // public becomes private
```

**Note:** Java and Python don't have this concept; they only have public inheritance.

---

## SECTION 3: INHERITANCE (MCQs 17-26)

### **Question 17: Constructor Chaining Order**

```python
class A:
    def __init__(self):
        print("A")

class B(A):
    def __init__(self):
        print("B")
        super().__init__()

class C(B):
    def __init__(self):
        super().__init__()
        print("C")

obj = C()
```

**What's the output?**

**A)** A B C  
**B)** C B A  
**C)** B A C  
**D)** A C B

**Answer: C**

**Explanation:**  
Execution flow:
1. `C.__init__()` called
2. `super().__init__()` calls `B.__init__()` → prints "B"
3. `B.__init__()` calls `super().__init__()` → `A.__init__()` → prints "A"
4. Back to `C.__init__()` → prints "C"

Output: **B A C**

**Constructor Execution:**
- Call order: Most derived → Base
- Actual execution: Base completes → Derived completes

---

### **Question 18: Multiple Inheritance Diamond Problem**

```python
class A:
    def method(self):
        print("A")

class B(A):
    def method(self):
        print("B")

class C(A):
    def method(self):
        print("C")

class D(B, C):
    pass

obj = D()
obj.method()  # What prints?
print(D.__mro__)
```

**What prints first?**

**A)** A  
**B)** B  
**C)** C  
**D)** Error

**Answer: B**

**Explanation:**  
Python uses **C3 Linearization (MRO)** for method resolution. For `D(B, C)`, the order is: D → B → C → A → object

So `obj.method()` finds method in B first, prints "B".

**MRO** (Method Resolution Order):  
`D → B → C → A → object`

**Language Comparison:**
- **Python**: Multiple inheritance with MRO (C3)
- **C++**: Multiple inheritance (diamond problem exists, use virtual inheritance)
- **Java**: NO multiple inheritance (only interfaces)

---

### **Question 19: Java super Keyword**

```java
class Parent {
    int x = 10;
}

class Child extends Parent {
    int x = 20;
    
    void display() {
        System.out.println(x);        // Line 1
        System.out.println(this.x);   // Line 2
        System.out.println(super.x);  // Line 3
    }
}

new Child().display();
```

**What's the output?**

**A)** 10 10 10  
**B)** 20 20 10  
**C)** 10 20 10  
**D)** 20 10 10

**Answer: B**

**Explanation:**  
- **Line 1**: `x` refers to Child's x → 20
- **Line 2**: `this.x` explicitly refers to Child's x → 20
- **Line 3**: `super.x` refers to Parent's x → 10

**Note:** This is field **hiding**, not overriding. Fields are not polymorphic!

---

### **Question 20: C++ Virtual Inheritance**

```cpp
class A {
public:
    int x;
};

class B : virtual public A { };
class C : virtual public A { };
class D : public B, public C { };

D obj;
obj.x = 10;  // How many copies of x exist?
```

**A)** 1 copy  
**B)** 2 copies  
**C)** 3 copies  
**D)** Compilation error

**Answer: A**

**Explanation:**  
**Virtual inheritance** solves the diamond problem by ensuring only ONE copy of base class A exists in D.

**Without virtual:**
```
    A
   / \
  B   C
   \ /
    D  (Two copies of A's members - ambiguous!)
```

**With virtual:**
```
    A
   / \
  B   C  (Both virtually inherit A)
   \ /
    D  (Only one copy of A)
```

---

### **Question 21: Method Hiding vs Overriding**

```java
class Parent {
    static void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    static void display() {
        System.out.println("Child");
    }
}

Parent obj = new Child();
obj.display();  // What prints?
```

**A)** Parent  
**B)** Child  
**C)** Compilation error  
**D)** Runtime error

**Answer: A**

**Explanation:**  
Static methods are NOT overridden, they are **hidden**. The method called depends on reference type (Parent), not object type (Child).

**Key Difference:**
- **Overriding**: Instance methods, runtime polymorphism
- **Hiding**: Static methods, compile-time resolution

```java
// To get "Child", use:
Child obj = new Child();
obj.display();  // Prints "Child"
```

---

### **Question 22: Multilevel Inheritance**

```java
class GrandParent {
    void show() { System.out.println("GrandParent"); }
}

class Parent extends GrandParent {
    void show() { System.out.println("Parent"); }
}

class Child extends Parent {
    void show() { System.out.println("Child"); }
}

GrandParent obj = new Child();
obj.show();
```

**What prints?**

**A)** GrandParent  
**B)** Parent  
**C)** Child  
**D)** Compilation error

**Answer: C**

**Explanation:**  
This is **runtime polymorphism**. Reference type is GrandParent, but object type is Child. At runtime, JVM calls Child's `show()` method.

**Multilevel Inheritance Chain:**
GrandParent → Parent → Child

All methods are virtual in Java by default, enabling polymorphism.

---

### **Question 23: Covariant Return Types**

```java
class Animal {
    Animal getAnimal() {
        return new Animal();
    }
}

class Dog extends Animal {
    Dog getAnimal() {  // Return type is subclass
        return new Dog();
    }
}
```

**Is this valid?**

**A)** Yes, covariant return types allowed  
**B)** No, must return Animal  
**C)** Compilation error  
**D)** Only valid in C++

**Answer: A**

**Explanation:**  
**Covariant return types** allow overriding method to return a subtype of the original return type. Introduced in Java 5.

**Rules:**
- ✅ Return type can be subclass
- ❌ Return type cannot be superclass or unrelated type
- ✅ Supported in Java 5+, C++
- ❌ Not checked in Python (duck typing)

---

### **Question 24: Is-A vs Has-A**

```java
class Engine { }
class Car {
    private Engine engine;  // What relationship?
}

class SportsCar extends Car { }  // What relationship?
```

**A)** Both are Is-A  
**B)** Both are Has-A  
**C)** Car Has-A Engine; SportsCar Is-A Car  
**D)** Car Is-A Engine; SportsCar Has-A Car

**Answer: C**

**Explanation:**  
- **Has-A (Composition)**: Car HAS-AN Engine (field/member)
- **Is-A (Inheritance)**: SportsCar IS-A Car (extends)

**When to use:**
- **Is-A**: True subtype relationship (Dog IS-AN Animal)
- **Has-A**: Object contains/uses another (Car HAS-AN Engine)

**Prefer composition over inheritance** for flexibility!

---

### **Question 25: Private Constructor**

```java
class Singleton {
    private static Singleton instance;
    
    private Singleton() { }  // Private constructor
    
    public static Singleton getInstance() {
        if (instance == null)
            instance = new Singleton();
        return instance;
    }
}

class SubClass extends Singleton { }  // Can we do this?
```

**What happens?**

**A)** Compiles successfully  
**B)** Compilation error: cannot inherit from class with private constructor  
**C)** Runtime error  
**D)** SubClass can be created

**Answer: B**

**Explanation:**  
Private constructor prevents:
1. Direct instantiation from outside
2. Inheritance (subclass needs to call parent constructor)

**Common Use:** Singleton pattern

**Language Comparison:**
- **Java/C++**: Private constructor prevents inheritance
- **Python**: No true private, but convention with `__init__`

---

### **Question 26: Final Class - Java**

```java
final class Parent {
    void display() { }
}

class Child extends Parent {  // What happens?
    void display() { }
}
```

**What's the result?**

**A)** Compiles successfully  
**B)** Compilation error: cannot inherit from final class  
**C)** Runtime error  
**D)** Child overrides Parent

**Answer: B**

**Explanation:**  
`final` keyword on class prevents inheritance. Common examples: `String`, `Integer`, `Math` classes in Java.

**Final Keyword Uses:**
- **final class**: Cannot be inherited
- **final method**: Cannot be overridden
- **final variable**: Cannot be reassigned

**Language Comparison:**
- **Java**: `final` keyword
- **C++**: `final` keyword (C++11)
- **Python**: No keyword (can use metaclasses to restrict)

---

## SECTION 4: POLYMORPHISM (MCQs 27-36)

### **Question 27: Method Overloading - Java**

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }
    
    int add(int a, int b, int c) {
        return a + b + c;
    }
    
    double add(double a, double b) {
        return a + b;
    }
}

Calculator calc = new Calculator();
System.out.println(calc.add(5, 10));
```

**What prints?**

**A)** 15  
**B)** 15.0  
**C)** Compilation error  
**D)** 5

**Answer: A**

**Explanation:**  
Method overloading chooses the best match at compile-time based on parameter types. `add(5, 10)` matches `int add(int a, int b)` exactly, returns 15.

**Overloading Rules:**
- ✅ Different number of parameters
- ✅ Different parameter types
- ✅ Different order of types
- ❌ Return type alone doesn't matter

---

### **Question 28: Python "Overloading"**

```python
class Math:
    def add(self, a, b):
        return a + b
    
    def add(self, a, b, c):  # What happens?
        return a + b + c

m = Math()
print(m.add(5, 10))
```

**What's the result?**

**A)** 15  
**B)** TypeError: missing 1 required positional argument  
**C)** 25  
**D)** Compilation error

**Answer: B**

**Explanation:**  
Python does NOT support method overloading. The second `add` definition **replaces** the first one. Calling `m.add(5, 10)` tries to call `add(a, b, c)` with only 2 arguments → TypeError.

**Python Alternative:**
```python
def add(self, a, b, c=0):  # Use default arguments
    return a + b + c
```

---

### **Question 29: Operator Overloading - C++**

```cpp
class Complex {
    int real, imag;
public:
    Complex(int r, int i) : real(r), imag(i) {}
    
    Complex operator+(const Complex& c) {
        return Complex(real + c.real, imag + c.imag);
    }
};

Complex c1(1, 2), c2(3, 4);
Complex c3 = c1 + c2;  // What are c3's values?
```

**A)** real=4, imag=6  
**B)** real=3, imag=8  
**C)** Compilation error  
**D)** real=1, imag=2

**Answer: A**

**Explanation:**  
`operator+` is overloaded to add complex numbers component-wise:
- real: 1 + 3 = 4
- imag: 2 + 4 = 6

**Operator Overloading Support:**
- **C++**: ✅ Full support
- **Java**: ❌ No support (except + for strings)
- **Python**: ✅ Via magic methods (`__add__`, `__sub__`, etc.)

---

### **Question 30: Runtime Polymorphism**

```java
class Animal {
    void sound() { System.out.println("Animal sound"); }
}

class Dog extends Animal {
    void sound() { System.out.println("Bark"); }
}

class Cat extends Animal {
    void sound() { System.out.println("Meow"); }
}

Animal a1 = new Dog();
Animal a2 = new Cat();
a1.sound();
a2.sound();
```

**What's the output?**

**A)** Animal sound (twice)  
**B)** Bark, Meow  
**C)** Compilation error  
**D)** Bark, Animal sound

**Answer: B**

**Explanation:**  
Runtime polymorphism (dynamic dispatch): method called is determined by object type at runtime, not reference type.

- `a1` references Dog object → `Dog.sound()` → "Bark"
- `a2` references Cat object → `Cat.sound()` → "Meow"

**This is the essence of polymorphism!**

---

### **Question 31: Method Signature**

```java
class Test {
    void method(int a) { }
    int method(int a) { return a; }  // Valid?
}
```

**Is this valid method overloading?**

**A)** Yes  
**B)** No, duplicate method  
**C)** Yes, different return types  
**D)** Only in C++

**Answer: B**

**Explanation:**  
**Method signature** = name + parameter types (NOT return type!)

Both methods have same signature: `method(int)`. Changing only return type doesn't create valid overloading → Compilation error.

**Valid Overloading:**
```java
void method(int a) { }
void method(double a) { }     // Different parameter type ✅
void method(int a, int b) { } // Different number of params ✅
```

---

### **Question 32: C++ Virtual Functions**

```cpp
class Base {
public:
    void display() {  // Non-virtual
        cout << "Base";
    }
};

class Derived : public Base {
public:
    void display() {
        cout << "Derived";
    }
};

Base* ptr = new Derived();
ptr->display();
```

**What prints?**

**A)** Base  
**B)** Derived  
**C)** Compilation error  
**D)** Runtime error

**Answer: A**

**Explanation:**  
Without `virtual` keyword, C++ uses **static binding** (compile-time). Method called is based on pointer type (Base), not object type (Derived).

**To get "Derived":**
```cpp
virtual void display() { }  // Make it virtual
```

**Key C++ Rule:** Non-virtual functions use static binding, virtual functions use dynamic binding.

---

### **Question 33: Python Operator Overloading**

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __str__(self):
        return f"({self.x}, {self.y})"

p1 = Point(1, 2)
p2 = Point(3, 4)
print(p1 + p2)
```

**What prints?**

**A)** Point object  
**B)** (4, 6)  
**C)** (1, 2)  
**D)** Error

**Answer: B**

**Explanation:**  
- `p1 + p2` calls `__add__(p1, p2)` → returns `Point(4, 6)`
- `print()` calls `__str__()` → returns "(4, 6)"

**Common Python Magic Methods:**
| Operation | Method |
|-----------|--------|
| `+` | `__add__` |
| `-` | `__sub__` |
| `*` | `__mul__` |
| `==` | `__eq__` |
| `<` | `__lt__` |
| `str()` | `__str__` |
| `len()` | `__len__` |

---

### **Question 34: Overriding vs Overloading**

**Which statement is correct?**

**A)** Overloading is runtime polymorphism  
**B)** Overriding is compile-time polymorphism  
**C)** Overloading is compile-time, Overriding is runtime  
**D)** Both are runtime polymorphism

**Answer: C**

**Explanation:**

| Feature | Overloading | Overriding |
|---------|------------|-----------|
| **Type** | Compile-time (Static) | Runtime (Dynamic) |
| **Same class?** | Yes | No (parent-child) |
| **Signature** | Different | Same |
| **Binding** | Early binding | Late binding |
| **Return type** | Can differ | Must be same/covariant |

---

### **Question 35: Final Method - Java**

```java
class Parent {
    final void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void display() {  // Can we override?
        System.out.println("Child");
    }
}
```

**What happens?**

**A)** Compiles successfully  
**B)** Compilation error: cannot override final method  
**C)** Runtime error  
**D)** Child method hides Parent method

**Answer: B**

**Explanation:**  
`final` on method prevents overriding. Use when you want to ensure behavior cannot be changed in subclasses.

**Examples in Java Standard Library:**
- `String.hashCode()` is final
- `Object.getClass()` is final

---

### **Question 36: Polymorphic Array**

```cpp
class Shape {
public:
    virtual void draw() { cout << "Shape"; }
    virtual ~Shape() { }
};

class Circle : public Shape {
public:
    void draw() override { cout << "Circle"; }
};

class Square : public Shape {
public:
    void draw() override { cout << "Square"; }
};

Shape* shapes[2];
shapes[0] = new Circle();
shapes[1] = new Square();

for (int i = 0; i < 2; i++)
    shapes[i]->draw();
```

**What prints?**

**A)** Shape Shape  
**B)** Circle Square  
**C)** Compilation error  
**D)** Undefined behavior

**Answer: B**

**Explanation:**  
This demonstrates the power of polymorphism! Array of base class pointers can hold derived class objects. Virtual functions ensure correct method is called at runtime:
- `shapes[0]->draw()` → Circle's draw()
- `shapes[1]->draw()` → Square's draw()

**Use Case:** Treat different objects uniformly while maintaining their specific behavior.

---

## SECTION 5: CLASSES & OBJECTS (MCQs 37-42)

### **Question 37: Static Members**

```java
class Counter {
    static int count = 0;
    
    Counter() {
        count++;
    }
}

Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();
System.out.println(Counter.count);
```

**What prints?**

**A)** 0  
**B)** 1  
**C)** 3  
**D)** Compilation error

**Answer: C**

**Explanation:**  
Static variable `count` is shared among ALL instances. Each constructor increments the same `count`:
- After c1: count = 1
- After c2: count = 2
- After c3: count = 3

**Static vs Instance:**
- **Static**: One copy per class (shared)
- **Instance**: One copy per object

---

### **Question 38: Constructor Overloading**

```cpp
class Rectangle {
    int width, height;
public:
    Rectangle() : width(0), height(0) { }
    Rectangle(int w) : width(w), height(w) { }
    Rectangle(int w, int h) : width(w), height(h) { }
};

Rectangle r1;        // Which constructor?
Rectangle r2(5);     // Which constructor?
Rectangle r3(5, 10); // Which constructor?
```

**A)** All call same constructor  
**B)** All call third constructor  
**C)** r1→1st, r2→2nd, r3→3rd  
**D)** Compilation error

**Answer: C**

**Explanation:**  
Constructor overloading allows multiple constructors with different parameters:
- `r1` → No args → Default constructor
- `r2` → One arg → Second constructor
- `r3` → Two args → Third constructor

**Benefit:** Flexibility in object creation

---

### **Question 39: Python Class vs Static Method**

```python
class MyClass:
    count = 0
    
    @classmethod
    def increment_class(cls):
        cls.count += 1
    
    @staticmethod
    def increment_static():
        MyClass.count += 1  # Must use class name
    
    def increment_instance(self):
        MyClass.count += 1

MyClass.increment_class()
MyClass.increment_static()
print(MyClass.count)
```

**What prints?**

**A)** 0  
**B)** 1  
**C)** 2  
**D)** Error

**Answer: C**

**Explanation:**  
Both methods increment the same class variable `count`:
- `increment_class()`: Uses `cls` parameter → count = 1
- `increment_static()`: Uses class name directly → count = 2

**Differences:**
| Method | Has cls/self? | Can access instance? | Can access class? |
|--------|--------------|---------------------|------------------|
| Instance | self | ✅ | ✅ |
| Class | cls | ❌ | ✅ |
| Static | ❌ | ❌ | Via name only |

---

### **Question 40: this Keyword - Java**

```java
class Person {
    String name;
    
    Person(String name) {
        name = name;  // Line 1
    }
    
    void setName(String name) {
        this.name = name;  // Line 2
    }
}

Person p = new Person("Alice");
System.out.println(p.name);
```

**What prints?**

**A)** Alice  
**B)** null  
**C)** Compilation error  
**D)** Empty string

**Answer: B**

**Explanation:**  
**Line 1:** `name = name` assigns parameter to itself (not the field). Field remains null.  
**Line 2:** `this.name = name` correctly assigns parameter to field.

**Fix for Line 1:**
```java
this.name = name;  // Use 'this' to refer to field
```

**Language Comparison:**
- **Java**: `this.field`
- **C++**: `this->field`
- **Python**: `self.field` (convention, not keyword)

---

### **Question 41: Copy Constructor - C++**

```cpp
class Box {
    int* data;
public:
    Box(int val) {
        data = new int(val);
    }
    
    // Shallow copy (default)
    // Deep copy needed:
    Box(const Box& other) {
        data = new int(*other.data);
    }
    
    ~Box() {
        delete data;
    }
};

Box b1(10);
Box b2 = b1;  // Copy constructor called
```

**Why is deep copy needed?**

**A)** Performance  
**B)** Prevent double deletion of same memory  
**C)** Not needed  
**D)** Syntax requirement

**Answer: B**

**Explanation:**  
**Shallow copy** (default): Copies pointer value → both objects point to SAME memory → double deletion upon destruction → crash!

**Deep copy**: Allocates new memory and copies value → each object has independent memory.

**When needed:** Class manages dynamic memory/resources

**Language Comparison:**
- **C++**: Must write copy constructor for deep copy
- **Java**: Use `clone()` method
- **Python**: Use `copy.deepcopy()`

---

### **Question 42: Destructor Order**

```cpp
class Base {
public:
    ~Base() { cout << "Base "; }
};

class Derived : public Base {
public:
    ~Derived() { cout << "Derived "; }
};

Derived obj;
// When obj goes out of scope, what's the output?
```

**A)** Base Derived  
**B)** Derived Base  
**C)** Base only  
**D)** Derived only

**Answer: B**

**Explanation:**  
**Destructor order**: Reverse of constructor order
1. Derived destructor called first → prints "Derived "
2. Base destructor called second → prints "Base "

**Remember:**
- **Construction**: Base → Derived
- **Destruction**: Derived → Base

**Reason:** Derived may use Base members, so Base must be destroyed last.

---

## SECTION 6: ADVANCED CONCEPTS (MCQs 43-50)

### **Question 43: Composition Example**

```java
class Engine {
    void start() { System.out.println("Engine started"); }
}

class Car {
    private Engine engine = new Engine();  // Composition
    
    void start() {
        engine.start();
    }
}

Car car = new Car();
car.start();
```

**What's the relationship?**

**A)** Car IS-A Engine  
**B)** Car HAS-A Engine  
**C)** Engine IS-A Car  
**D)** No relationship

**Answer: B**

**Explanation:**  
Car **HAS-AN** Engine (composition). Engine is part of Car.

**Composition Characteristics:**
- ✅ Car contains Engine as a field
- ✅ Strong ownership (Engine created with Car)
- ✅ Engine lifecycle tied to Car
- ✅ More flexible than inheritance

**When to use:** Object needs another object as a component/part

---

### **Question 44: Interface Segregation Principle**

```java
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Robot implements Worker {
    public void work() { /* OK */ }
    public void eat() { /* Robot doesn't eat! */ }
    public void sleep() { /* Robot doesn't sleep! */ }
}
```

**What SOLID principle is violated?**

**A)** Single Responsibility  
**B)** Open/Closed  
**C)** Liskov Substitution  
**D)** Interface Segregation

**Answer: D**

**Explanation:**  
**Interface Segregation Principle**: Don't force classes to implement methods they don't need.

**Better Design:**
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class Robot implements Workable { /* Only work() */ }
class Human implements Workable, Eatable, Sleepable { /* All */ }
```

---

### **Question 45: Liskov Substitution Principle**

```java
class Rectangle {
    protected int width, height;
    
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
    int getArea() { return width * height; }
}

class Square extends Rectangle {
    void setWidth(int w) {
        width = w;
        height = w;  // Keep it square
    }
    
    void setHeight(int h) {
        width = h;
        height = h;  // Keep it square
    }
}

Rectangle r = new Square();
r.setWidth(5);
r.setHeight(4);
System.out.println(r.getArea());  // Expected: 20, Actual?
```

**What prints?**

**A)** 20  
**B)** 16  
**C)** 25  
**D)** 9

**Answer: B**

**Explanation:**  
Square's `setHeight(4)` sets both width and height to 4 → area = 4×4 = 16

**LSP Violation:** Square cannot seamlessly replace Rectangle (behavior changed). Expected 5×4=20, got 4×4=16.

**Lesson:** Inheritance should preserve expected behavior. Square is NOT a proper subtype of Rectangle in OOP!

---

### **Question 46: Dependency Inversion**

```java
// Bad: High-level depends on low-level
class MySQLDatabase {
    void save() { }
}

class UserService {
    MySQLDatabase db = new MySQLDatabase();  // Tight coupling
}

// Good: Both depend on abstraction
interface Database {
    void save();
}

class MySQLDatabase implements Database {
    public void save() { }
}

class UserService {
    Database db;  // Depends on abstraction
    
    UserService(Database db) {
        this.db = db;
    }
}
```

**What principle does the good version follow?**

**A)** Single Responsibility  
**B)** Open/Closed  
**C)** Dependency Inversion  
**D)** Interface Segregation

**Answer: C**

**Explanation:**  
**Dependency Inversion Principle:**
1. High-level modules should not depend on low-level modules
2. Both should depend on abstractions

**Benefits:**
- ✅ Easy to switch implementations (MySQL → PostgreSQL)
- ✅ Easier testing (mock Database)
- ✅ Reduced coupling

---

### **Question 47: Association vs Aggregation vs Composition**

```java
// Association
class Teacher {
    void teach(Student s) { }  // Teacher knows Student
}

// Aggregation  
class Department {
    List<Teacher> teachers;  // Teachers exist independently
}

// Composition
class University {
    List<Department> departments;  // Departments die with University
}
```

**Which has strongest relationship?**

**A)** Association  
**B)** Aggregation  
**C)** Composition  
**D)** All equal

**Answer: C**

**Explanation:**

| Type | Relationship | Lifetime | Example |
|------|-------------|----------|---------|
| **Association** | Uses-A | Independent | Teacher uses Student |
| **Aggregation** | Has-A (weak) | Independent | Department has Teachers |
| **Composition** | Has-A (strong) | Dependent | University has Departments |

**Composition is strongest** (parts cannot exist without container).

---

### **Question 48: Factory Pattern**

```python
class Animal:
    def sound(self):
        pass

class Dog(Animal):
    def sound(self):
        return "Bark"

class Cat(Animal):
    def sound(self):
        return "Meow"

class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        return None

animal = AnimalFactory.create_animal("dog")
print(animal.sound())
```

**What prints?**

**A)** None  
**B)** Bark  
**C)** Meow  
**D)** Error

**Answer: B**

**Explanation:**  
Factory Pattern creates objects without exposing creation logic. `create_animal("dog")` returns Dog instance, `sound()` returns "Bark".

**Benefits:**
- ✅ Centralized creation logic
- ✅ Easier to extend (add new animals)
- ✅ Client doesn't need to know concrete classes

---

### **Question 49: Abstract Class Must Have Abstract Method?**

```java
abstract class MyClass {
    void concreteMethod() {
        System.out.println("Concrete");
    }
    // No abstract methods!
}

class Child extends MyClass { }

Child obj = new Child();
```

**Is this valid?**

**A)** Yes, abstract class can have only concrete methods  
**B)** No, abstract class must have at least one abstract method  
**C)** Compilation error  
**D)** Only valid in Python

**Answer: A**

**Explanation:**  
Abstract class does NOT require abstract methods! You can mark a class `abstract` just to prevent instantiation.

**Use Case:** Base class with concrete methods that shouldn't be instantiated directly.

**Language Comparison:**
- **Java**: Abstract class can have zero abstract methods
- **C++**: At least one pure virtual needed for abstract class
- **Python**: ABC can have zero abstract methods

---

### **Question 50: Multiple Inheritance vs Interface**

```java
// Java doesn't allow:
class Child extends Parent1, Parent2 { }  // ERROR!

// But allows:
class Child extends Parent implements Interface1, Interface2 { }  // OK!
```

**Why does Java allow multiple interfaces but not multiple classes?**

**A)** Performance reasons  
**B)** Avoid diamond problem with state  
**C)** Syntax limitation  
**D)** No particular reason

**Answer: B**

**Explanation:**  
**Diamond Problem with State:**
```
    Parent1         Parent2
   (field x)      (field x)
        \             /
          \         /
            Child
    (which x to inherit?)
```

**Interfaces** don't have state (fields), only contracts → no ambiguity!

**Language Solutions:**
- **Java**: Multiple interfaces ✅, Multiple classes ❌
- **C++**: Multiple inheritance ✅ (use virtual inheritance for diamond)
- **Python**: Multiple inheritance ✅ (MRO resolves conflicts)

---

## 🎯 EXAM TIPS

### High-Frequency Topics:
1. ✅ Abstract class vs Interface
2. ✅ Method overloading vs overriding
3. ✅ Access modifiers across languages
4. ✅ Constructor/destructor chaining
5. ✅ Static vs instance members
6. ✅ Virtual functions in C++
7. ✅ SOLID principles
8. ✅ Composition vs Inheritance
9. ✅ Polymorphism (runtime vs compile-time)
10. ✅ Multiple inheritance & MRO

### Common Traps:
- ❌ Return type is NOT part of method signature
- ❌ Static methods cannot be overridden (only hidden)
- ❌ Fields are hidden, not overridden
- ❌ Python has NO true method overloading
- ❌ C++ needs `virtual` for runtime polymorphism
- ❌ Java interfaces variables are always final
- ❌ Must have virtual destructor in C++ polymorphic base classes

### Language-Specific:
- **C++**: Virtual keyword, diamond problem, manual memory
- **Java**: No multiple inheritance, all methods virtual, GC
- **Python**: Duck typing, MRO, name mangling, no true private

---

## ANSWER KEY SUMMARY

1. B  | 11. C  | 21. A  | 31. B  | 41. B
2. C  | 12. B  | 22. C  | 32. A  | 42. B
3. C  | 13. C  | 23. A  | 33. B  | 43. B
4. C  | 14. B  | 24. C  | 34. C  | 44. D
5. C  | 15. B  | 25. B  | 35. B  | 45. B
6. B  | 16. B  | 26. B  | 36. B  | 46. C
7. D  | 17. C  | 27. A  | 37. C  | 47. C
8. C  | 18. B  | 28. B  | 38. C  | 48. B
9. A  | 19. B  | 29. A  | 39. C  | 49. A
10. B | 20. A  | 30. B  | 40. B  | 50. B

---

**🔥 Good Luck with Your Exam! 🔥**

**Total Questions**: 50  
**All Concepts Covered**: ✅  
**Multi-Language**: C++ | Java | Python  
**Detailed Explanations**: ✅
