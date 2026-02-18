# OOP Quick Revision Guide - Multi-Language
## **Last Minute Revision - All Concepts Covered**

### 📚 Coverage: C++ | Java | Python
### ⚡ Focus: Concepts over Code | Tables | Quick Reference

---

## 1. THE FOUR PILLARS OF OOP

| Pillar | Definition | Purpose | Key Benefit |
|--------|------------|---------|-------------|
| **Abstraction** | Hiding complexity, showing only essentials | Focus on WHAT, not HOW | Simplifies interface |
| **Encapsulation** | Bundling data and methods, restricting access | Data hiding & protection | Security & integrity |
| **Inheritance** | Child class acquires parent properties | Code reuse | Reduces redundancy |
| **Polymorphism** | Same interface, different implementations | One name, many forms | Flexibility |

---

## 2. ABSTRACTION

### Concept Overview
- **Abstract Class**: Cannot instantiate, must be inherited
- **Interface**: Pure contract (all methods abstract)
- **Concrete Class**: Fully implemented, can instantiate

### Language Comparison

| Feature | C++ | Java | Python |
|---------|-----|------|--------|
| **Abstract Class** | `virtual` functions with `= 0` | `abstract class` keyword | `ABC` + `@abstractmethod` |
| **Interface** | Pure virtual class | `interface` keyword | Multiple inheritance from ABC |
| **Must Override** | Pure virtual functions | Abstract methods | Methods with `@abstractmethod` |
| **Can Have Concrete** | Yes | Yes (default methods) | Yes |

### Syntax Snapshot

**C++:**
```cpp
class Shape {  // Abstract class
    virtual double area() = 0;  // Pure virtual
};
```

**Java:**
```java
abstract class Shape {  // Abstract class
    abstract double area();  // Abstract method
}
interface Drawable {  // Interface
    void draw();
}
```

**Python:**
```python
from abc import ABC, abstractmethod
class Shape(ABC):  # Abstract class
    @abstractmethod
    def area(self): pass
```

---

## 3. ENCAPSULATION

### Access Modifiers Comparison

| Modifier | C++ | Java | Python | Visible To |
|----------|-----|------|--------|------------|
| **Public** | `public:` | `public` | No keyword (default) | Everyone |
| **Protected** | `protected:` | `protected` | `_single_underscore` | Class + subclasses |
| **Private** | `private:` | `private` | `__double_underscore` | Class only |
| **Package** | N/A | Default (no keyword) | N/A | Same package |

### Getters and Setters

| Language | Getter Pattern | Setter Pattern | Property Decorator |
|----------|---------------|----------------|-------------------|
| **C++** | `int getAge()` | `void setAge(int a)` | N/A |
| **Java** | `int getAge()` | `void setAge(int a)` | N/A |
| **Python** | `@property` | `@attribute.setter` | Yes (Pythonic way) |

### Key Principles
- ✅ Make fields private/protected
- ✅ Provide public getters/setters with validation
- ✅ Hide implementation details
- ❌ Don't expose internal state directly

---

## 4. INHERITANCE

### Types of Inheritance

| Type | Description | C++ | Java | Python | Use Case |
|------|-------------|-----|------|--------|----------|
| **Single** | One parent, one child | ✅ | ✅ | ✅ | Basic extension |
| **Multiple** | Multiple parents | ✅ | ❌ | ✅ | Mix capabilities |
| **Multilevel** | Chain: A→B→C | ✅ | ✅ | ✅ | Hierarchy |
| **Hierarchical** | One parent, many children | ✅ | ✅ | ✅ | Common base |
| **Hybrid** | Combination | ✅ | ❌ | ✅ | Complex scenarios |

### Method Resolution Order (MRO)

| Language | Diamond Problem | Solution | Order |
|----------|----------------|----------|-------|
| **C++** | Yes | Virtual inheritance | Ambiguous without virtual |
| **Java** | No (interface only) | Interface default methods | Top-down |
| **Python** | Yes | C3 Linearization (MRO) | Left-to-right, depth-first |

### Constructor Chaining

| Language | Parent Constructor Call | Keyword |
|----------|------------------------|---------|
| **C++** | Initializer list: `Child() : Parent()` | N/A |
| **Java** | First line: `super()` | `super` |
| **Python** | Anywhere: `super().__init__()` | `super()` |

### Key Syntax

**C++:**
```cpp
class Child : public Parent {  // Public inheritance
    Child() : Parent() { }  // Call parent constructor
};
```

**Java:**
```java
class Child extends Parent {  // Inheritance
    Child() { super(); }  // Call parent constructor
}
```

**Python:**
```python
class Child(Parent):  # Inheritance
    def __init__(self):
        super().__init__()  # Call parent constructor
```

---

## 5. POLYMORPHISM

### Types of Polymorphism

| Type | Binding Time | Mechanism | Example |
|------|-------------|-----------|---------|
| **Compile-time** (Static) | Compile | Method overloading, Operator overloading | `add(int, int)` vs `add(float, float)` |
| **Runtime** (Dynamic) | Runtime | Method overriding | Parent ref → Child object |

### Compile-time Polymorphism

| Feature | C++ | Java | Python |
|---------|-----|------|--------|
| **Method Overloading** | ✅ | ✅ | ❌ (use default args) |
| **Operator Overloading** | ✅ | ❌ (except +) | ✅ (magic methods) |

**Method Overloading Examples:**

| Language | Syntax |
|----------|--------|
| **C++** | `void func(int x)`, `void func(double x)` |
| **Java** | `void func(int x)`, `void func(double x)` |
| **Python** | Use default args: `def func(x=0, y=None)` |

**Operator Overloading:**

| Language | Addition | Comparison | Index Access |
|----------|----------|------------|--------------|
| **C++** | `operator+` | `operator==` | `operator[]` |
| **Java** | ❌ | ❌ | ❌ |
| **Python** | `__add__` | `__eq__` | `__getitem__` |

### Runtime Polymorphism

| Requirement | C++ | Java | Python |
|-------------|-----|------|--------|
| **Keyword** | `virtual` | Default | Default |
| **Override** | `override` (optional) | `@Override` (optional) | No decorator needed |
| **Pure Virtual** | `= 0` | `abstract` | `@abstractmethod` |

**Key Concepts:**

| Concept | C++ | Java | Python |
|---------|-----|------|--------|
| **Virtual Method Table** | Yes (vtable) | Yes | Yes (built-in) |
| **Late Binding** | With `virtual` keyword | Default for all methods | Default for all methods |
| **Final Methods** | `final` (C++11) | `final` | No keyword |

---

## 6. CLASSES & OBJECTS

### Class Components

| Component | Purpose | Access | Example |
|-----------|---------|--------|---------|
| **Attributes/Fields** | Store state | Private/Protected | `age`, `name` |
| **Methods** | Define behavior | Public | `getAge()`, `setName()` |
| **Constructor** | Initialize object | Public | `__init__`, Constructor |
| **Destructor** | Cleanup resources | Public | `~ClassName()`, `__del__` |

### Special Members

| Feature | C++ | Java | Python |
|---------|-----|------|--------|
| **Constructor** | `ClassName()` | `ClassName()` | `__init__(self)` |
| **Destructor** | `~ClassName()` | `finalize()` (deprecated) | `__del__(self)` |
| **Copy Constructor** | `ClassName(const ClassName&)` | ❌ (use clone) | ❌ (use copy module) |
| **Default Constructor** | Auto if no constructor | Auto if no constructor | Auto if no `__init__` |

### Static vs Instance

| Feature | Static (Class-level) | Instance (Object-level) |
|---------|---------------------|------------------------|
| **Belongs to** | Class | Individual object |
| **Memory** | One copy shared | One copy per object |
| **Access** | `ClassName.member` | `object.member` |
| **Use Case** | Counters, utilities | Object-specific data |

| Language | Static Keyword | Class Method |
|----------|---------------|--------------|
| **C++** | `static` | `static` |
| **Java** | `static` | `static` |
| **Python** | ❌ (use class var) | `@classmethod` |

---

## 7. SPECIAL CONCEPTS

### Constructor Types

| Type | Purpose | C++ | Java | Python |
|------|---------|-----|------|--------|
| **Default** | No args | `ClassName()` | `ClassName()` | `__init__(self)` |
| **Parameterized** | With args | `ClassName(int x)` | `ClassName(int x)` | `__init__(self, x)` |
| **Copy** | Copy from another | `ClassName(const ClassName&)` | Use `.clone()` | Use `copy.copy()` |

### Method Types

| Type | C++ | Java | Python |
|------|-----|------|--------|
| **Instance Method** | Default | Default | `def method(self)` |
| **Class Method** | `static` | `static` | `@classmethod def method(cls)` |
| **Static Method** | `static` | `static` | `@staticmethod def method()` |

### this vs self vs this

| Language | Reference Name | Usage |
|----------|---------------|--------|
| **C++** | `this` (pointer) | `this->attribute` |
| **Java** | `this` (reference) | `this.attribute` |
| **Python** | `self` (convention) | `self.attribute` |

---

## 8. ADVANCED CONCEPTS

### Composition vs Inheritance

| Aspect | Inheritance | Composition |
|--------|------------|-------------|
| **Relationship** | "IS-A" | "HAS-A" |
| **Coupling** | Tight (child knows parent) | Loose (objects independent) |
| **Flexibility** | Less flexible | More flexible |
| **Example** | `Dog IS-A Animal` | `Car HAS-A Engine` |
| **When to use** | True subtype relationship | Need parts/components |

### Aggregation vs Composition

| Feature | Aggregation | Composition |
|---------|------------|-------------|
| **Relationship** | "HAS-A" (weak) | "HAS-A" (strong) |
| **Lifetime** | Independent | Dependent on container |
| **Example** | `Department HAS Employees` | `Car HAS Engine` |
| **Deletion** | Parts survive | Parts destroyed with container |

### Interface vs Abstract Class

| Feature | Interface | Abstract Class |
|---------|-----------|---------------|
| **Methods** | All abstract (traditionally) | Can mix abstract & concrete |
| **Variables** | Constants only | Any type allowed |
| **Inheritance** | Multiple allowed | Single (Java), Multiple (C++, Python) |
| **Purpose** | Define contract | Share common code |
| **When to use** | Unrelated classes, same behavior | Related classes, shared code |

---

## 9. DESIGN PRINCIPLES - SOLID

| Principle | Abbreviation | Meaning | Quick Rule |
|-----------|--------------|---------|------------|
| **Single Responsibility** | S | One class, one job | Class should have only one reason to change |
| **Open/Closed** | O | Open for extension, closed for modification | Use inheritance/interfaces to extend |
| **Liskov Substitution** | L | Subclass replaces parent seamlessly | Child should work wherever parent works |
| **Interface Segregation** | I | Many specific interfaces > one general | Don't force unused methods |
| **Dependency Inversion** | D | Depend on abstractions, not concrete | Use interfaces/abstract classes |

---

## 10. METHOD OVERRIDING RULES

### Override Requirements

| Language | Parent Method | Child Method | Return Type |
|----------|--------------|--------------|-------------|
| **C++** | Must be `virtual` | Can use `override` | Covariant allowed |
| **Java** | Default virtual | Can use `@Override` | Covariant allowed |
| **Python** | Default virtual | Just redefine | Any type (duck typing) |

### Access Level Rules

| Language | Rule |
|----------|------|
| **C++** | Can change (usually same) |
| **Java** | Cannot be more restrictive (can be less) |
| **Python** | No restriction |

---

## 11. EXCEPTION IN OOP

### Constructor Exceptions

| Language | Can throw? | How to handle? |
|----------|-----------|----------------|
| **C++** | Yes | Try-catch at creation point |
| **Java** | Yes | Try-catch at creation point |
| **Python** | Yes | Try-except at creation point |

### Destructor Exceptions

| Language | Should throw? | Best Practice |
|----------|--------------|---------------|
| **C++** | ❌ Never | Catch internally |
| **Java** | N/A (no destructor) | Use try-with-resources |
| **Python** | ❌ Avoid | Catch internally |

---

## 12. MEMORY MANAGEMENT

### Object Creation Location

| Location | C++ | Java | Python |
|----------|-----|------|--------|
| **Stack** | `Object obj;` | Primitive only | N/A |
| **Heap** | `Object* obj = new Object();` | `Object obj = new Object();` | All objects |

### Garbage Collection

| Language | GC? | Manual Delete? | Reference Counting? |
|----------|-----|---------------|-------------------|
| **C++** | ❌ | ✅ (`delete`) | ❌ |
| **Java** | ✅ | ❌ | ❌ |
| **Python** | ✅ | ❌ | ✅ (+ GC) |

---

## 13. COMMON INTERVIEW TRAPS

### Conceptual Traps

| Trap | Wrong Assumption | Correct Answer |
|------|-----------------|----------------|
| **Static polymorphism** | Happens at runtime | Happens at compile-time |
| **Multiple inheritance in Java** | Java supports it | Only via interfaces |
| **Python private** | Truly private with `__` | Name mangling, still accessible |
| **C++ virtual destructor** | Not needed | MUST have in polymorphic base classes |
| **Interface variables** | Can change | Always final/constant |

### Syntax Traps

| Language | Common Error | Correct Form |
|----------|-------------|--------------|
| **C++** | `class Child : Parent` | `class Child : public Parent` |
| **Java** | `class Child implements ParentClass` | `class Child extends ParentClass` |
| **Python** | `super.init()` | `super().__init__()` |

---

## 14. QUICK COMPARISON TABLE

### All Three Languages Side-by-Side

| Feature | C++ | Java | Python |
|---------|-----|------|--------|
| **Multiple Inheritance** | ✅ | ❌ (interfaces only) | ✅ |
| **Operator Overloading** | ✅ | ❌ | ✅ |
| **Method Overloading** | ✅ | ✅ | ❌ (default args) |
| **Access Modifiers** | public, protected, private | public, protected, private, default | Convention-based (`_`, `__`) |
| **Abstract Keyword** | `virtual =0` | `abstract` | `@abstractmethod` |
| **Interface** | Pure virtual class | `interface` | ABC multiple inheritance |
| **Constructor Name** | Same as class | Same as class | `__init__` |
| **Destructor** | `~ClassName()` | `finalize()` (deprecated) | `__del__()` |
| **Garbage Collection** | ❌ | ✅ | ✅ |
| **Pointers** | ✅ | ❌ (references only) | ❌ (references only) |
| **Virtual by Default** | ❌ (need `virtual`) | ✅ | ✅ |
| **Final Classes** | `final` | `final` | No keyword |

---

## 15. MUST REMEMBER POINTS

### Abstraction
- ✅ Abstract class CAN have constructors (all languages)
- ✅ Abstract class CAN have concrete methods
- ✅ Interface variables are constants
- ❌ Cannot instantiate abstract class directly

### Encapsulation
- ✅ Use getters/setters for validation
- ✅ Private = accessible in class only
- ✅ Protected = accessible in class + subclasses
- ❌ Don't expose mutable collection fields directly

### Inheritance
- ✅ Constructor called: Parent → Child
- ✅ Destructor called: Child → Parent
- ✅ Can override methods but not fields (hiding)
- ❌ Cannot inherit constructors (Java has special case with `super()`)

### Polymorphism
- ✅ Method signature = name + parameters
- ✅ Return type NOT part of signature (for overloading)
- ✅ Override = same signature, different implementation
- ✅ Overload = same name, different parameters

### Static Members
- ✅ Shared across all instances
- ✅ Can access without object
- ❌ Cannot access instance members directly
- ❌ Cannot be overridden (Java)

### Composition
- ✅ Prefer composition over inheritance (flexibility)
- ✅ Strong composition = parts die with container
- ✅ Weak aggregation = parts survive independently

---

## 16. KEY DIFFERENCES SUMMARY

### C++ Specific
- **Pointers**: Manual memory management
- **Virtual**: Must use `virtual` for runtime polymorphism
- **Multiple Inheritance**: Fully supported (diamond problem exists)
- **Virtual Destructor**: MUST have in polymorphic base classes
- **Diamond Problem**: Solved with virtual inheritance

### Java Specific
- **No Multiple Inheritance**: Only through interfaces
- **Virtual by Default**: All methods are virtual
- **No Operator Overloading**: Except `+` for strings
- **Interfaces**: Can have default and static methods (Java 8+)
- **Final Keyword**: Prevents inheritance/overriding

### Python Specific
- **Duck Typing**: "If it walks like a duck..."
- **No Access Modifiers**: Convention-based (`_`, `__`)
- **MRO**: C3 Linearization for multiple inheritance
- **Magic Methods**: `__add__`, `__eq__`, etc. for operator overloading
- **Everything is Object**: Even classes and functions

---

## 17. INTERVIEW QUICK CHECKS

### Questions to Ask Yourself

1. **Is-A vs Has-A?** → Inheritance vs Composition
2. **Need multiple behaviors from unrelated classes?** → Interface
3. **Need to share code among related classes?** → Abstract Class
4. **Same name, different params?** → Overloading (Compile-time)
5. **Same signature, different implementation?** → Overriding (Runtime)
6. **Parent reference, child object?** → Polymorphism
7. **One class, one responsibility?** → Single Responsibility Principle
8. **Open for extension, closed for modification?** → Open/Closed Principle
9. **Child works where parent works?** → Liskov Substitution
10. **Depend on abstraction, not concrete?** → Dependency Inversion

---

## 18. FINAL REVISION CHECKLIST

### Before Exam - Confirm You Know:

- [ ] Four pillars of OOP and their purposes
- [ ] Abstract class vs Interface (all 3 languages)
- [ ] Access modifiers in C++, Java, Python
- [ ] Method overloading vs overriding
- [ ] Static vs instance members
- [ ] Constructor/destructor chaining
- [ ] MRO in Python
- [ ] Diamond problem in C++ (virtual inheritance)
- [ ] Composition vs Inheritance (When to use which)
- [ ] SOLID principles (at least S, O, L)
- [ ] Virtual keyword necessity in C++
- [ ] Why virtual destructor needed in C++
- [ ] Interface variables are final/constant
- [ ] Cannot instantiate abstract class
- [ ] Return type not part of method signature
- [ ] Covariant return types allowed in overriding

---

## 19. RAPID FIRE CONCEPTS

| Concept | In 5 Words |
|---------|-----------|
| Abstraction | Hide complexity, show essentials |
| Encapsulation | Bundle data, restrict access |
| Inheritance | Child acquires parent properties |
| Polymorphism | One interface, many implementations |
| Overloading | Same name, different parameters |
| Overriding | Same signature, different behavior |
| Static | Shared, class-level, no object |
| Virtual | Enables runtime polymorphism |
| Abstract | Cannot instantiate, must implement |
| Interface | Pure contract, all abstract |
| Composition | Has-a relationship, object contains |
| Aggregation | Weak has-a, independent lifetime |
| Constructor | Initialize object on creation |
| Destructor | Cleanup when object destroyed |
| MRO | Method resolution order, Python |

---

## 20. COMMON CODE PATTERNS

### Singleton Pattern

| Language | Key Technique |
|----------|--------------|
| **C++** | Static instance, private constructor |
| **Java** | Static getInstance(), private constructor |
| **Python** | `__new__` method, class variable |

### Factory Pattern

| Language | Key Technique |
|----------|--------------|
| **C++** | Static method returns pointer to base class |
| **Java** | Static method returns interface/abstract type |
| **Python** | Classmethod returns instance |

### Strategy Pattern

| Concept | Implementation |
|---------|---------------|
| **Purpose** | Select algorithm at runtime |
| **How** | Pass behavior object to context |
| **OOP Concept** | Composition + Polymorphism |

---

## END OF QUICK REVISION

### 🎯 You're now ready for your exam!
### ✅ All concepts covered
### 🔥 Good luck!

---

**Last Updated**: Perfect for SEBI Phase 2
**Usage**: Last 24 hours before exam
**Time to Review**: 45-60 minutes
