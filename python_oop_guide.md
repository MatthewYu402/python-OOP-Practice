# Python Object-Oriented Programming: A Comprehensive Guide

This guide provides an in-depth exploration of Object-Oriented Programming (OOP) concepts in Python, designed for both learning and professional reference. Each section builds upon previous concepts, providing clear explanations, practical examples, and real-world applications.

---

## Table of Contents

1. [Basics - Classes, Objects, Attributes, Methods](#1-basics---classes-objects-attributes-methods)
2. [Encapsulation - Private Attributes, Properties, Getters/Setters](#2-encapsulation---private-attributes-properties-getterssetters)
3. [Inheritance - Single and Multiple Inheritance](#3-inheritance---single-and-multiple-inheritance)
4. [Polymorphism - Method Overriding, Duck Typing](#4-polymorphism---method-overriding-duck-typing)
5. [Special Methods - `__init__`, `__str__`, `__repr__`, etc.](#5-special-methods---init-str-repr-etc)
6. [Class & Static Methods - Decorators and Alternative Constructors](#6-class--static-methods---decorators-and-alternative-constructors)
7. [Abstract Classes - ABC Module, Interfaces](#7-abstract-classes---abc-module-interfaces)
8. [Composition - Has-a Relationships](#8-composition---has-a-relationships)
9. [Design Patterns - Singleton, Factory, Observer, etc.](#9-design-patterns---singleton-factory-observer-etc)
10. [Advanced Topics - Metaclasses, Descriptors, Context Managers](#10-advanced-topics---metaclasses-descriptors-context-managers)

---

## 1. Basics - Classes, Objects, Attributes, Methods

### What Are Classes and Objects?

Think of a **class** as a blueprint for a house, and an **object** as an actual house built from that blueprint. A class defines what attributes (data) and methods (functions) an object will have, while an object is a specific instance of that class.

**Class**: A template that defines the structure and behavior of objects
**Object**: A specific instance of a class with its own data
**Attributes**: Variables that belong to an object (data)
**Methods**: Functions that belong to an object (behavior)

### Why Are They Important?

OOP allows you to:
- **Organize code** logically around real-world entities
- **Reuse code** through classes and inheritance
- **Model complex systems** by breaking them into manageable objects
- **Maintain code** more easily by encapsulating related functionality

### How It Works Under the Hood

When you create a class in Python, Python creates a class object. When you instantiate an object, Python:
1. Calls the `__new__` method to create the object
2. Calls the `__init__` method to initialize it
3. Stores attributes in a dictionary (`__dict__`)
4. Binds methods to the instance

### Example 1: Basic Class Structure

```python
class Person:
    """A simple Person class demonstrating basic OOP concepts."""
    
    def __init__(self, name, age):
        """
        Constructor method - called when creating a new Person object.
        'self' refers to the instance being created.
        """
        self.name = name  # Instance attribute
        self.age = age    # Instance attribute
    
    def introduce(self):
        """Instance method - operates on the specific person instance."""
        return f"Hi, I'm {self.name} and I'm {self.age} years old"
    
    def have_birthday(self):
        """Method that modifies instance state."""
        self.age += 1
        return f"{self.name} is now {self.age} years old!"

# Creating objects (instances) of the Person class
person1 = Person("Alice", 30)
person2 = Person("Bob", 25)

print(person1.introduce())  # Output: Hi, I'm Alice and I'm 30 years old
print(person2.introduce())  # Output: Hi, I'm Bob and I'm 25 years old

# Each object maintains its own state
person1.have_birthday()
print(person1.age)  # Output: 31
print(person2.age)  # Output: 25 (unchanged)
```

### Example 2: Bank Account with State Management

```python
class BankAccount:
    """
    Demonstrates how objects maintain state and enforce business rules.
    Real-world scenario: Banking system where accounts have balances and operations.
    """
    
    def __init__(self, account_number, owner_name, initial_balance=0):
        self.account_number = account_number
        self.owner_name = owner_name
        self.balance = initial_balance  # State: current balance
    
    def deposit(self, amount):
        """Add money to the account."""
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")
        self.balance += amount
        return f"Deposited ${amount}. New balance: ${self.balance}"
    
    def withdraw(self, amount):
        """Remove money from the account with validation."""
        if amount <= 0:
            raise ValueError("Withdrawal amount must be positive")
        if amount > self.balance:
            raise ValueError("Insufficient funds")
        self.balance -= amount
        return f"Withdrew ${amount}. New balance: ${self.balance}"
    
    def get_balance(self):
        """Query method - doesn't modify state."""
        return self.balance

# Usage
account = BankAccount("12345", "John Doe", 1000)
print(account.deposit(500))   # Output: Deposited $500. New balance: $1500
print(account.withdraw(200))  # Output: Withdrew $200. New balance: $1300
print(account.get_balance())  # Output: 1300
```

### Example 3: Rectangle with Computed Properties

```python
class Rectangle:
    """
    Shows how methods can compute values from attributes.
    Real-world scenario: Graphics applications, UI layouts, CAD software.
    """
    
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        """Calculate area from width and height."""
        return self.width * self.height
    
    def perimeter(self):
        """Calculate perimeter from width and height."""
        return 2 * (self.width + self.height)
    
    def is_square(self):
        """Check if rectangle is actually a square."""
        return self.width == self.height
    
    def scale(self, factor):
        """Modify dimensions while maintaining aspect ratio."""
        self.width *= factor
        self.height *= factor

# Usage
rect = Rectangle(5, 10)
print(f"Area: {rect.area()}")           # Output: Area: 50
print(f"Perimeter: {rect.perimeter()}") # Output: Perimeter: 30
print(f"Is square: {rect.is_square()}") # Output: Is square: False

square = Rectangle(5, 5)
print(f"Is square: {square.is_square()}") # Output: Is square: True
```

### Real-World Scenarios

1. **E-commerce Systems**: `Product` class with attributes (name, price, stock) and methods (update_price, check_availability)
2. **User Management**: `User` class with authentication methods and profile data
3. **Game Development**: `Character` class with health, position, and movement methods

### Common Pitfalls and Best Practices

**Pitfalls:**
- Forgetting `self` parameter in method definitions
- Modifying class variables when you meant instance variables
- Not initializing all necessary attributes in `__init__`

**Best Practices:**
- Always use `self` for instance attributes and methods
- Initialize all attributes in `__init__`
- Use descriptive method names that indicate what they do
- Keep methods focused on a single responsibility
- Document your classes with docstrings

```python
# BAD: Missing self, unclear naming
class BadExample:
    def __init__(x, y):  # Should be self
        x.data = y
    
    def do():  # Should be self, unclear name
        pass

# GOOD: Clear, well-documented
class GoodExample:
    def __init__(self, initial_value):
        """Initialize with a starting value."""
        self.data = initial_value
    
    def process_data(self):
        """Process the stored data and return result."""
        return self.data * 2
```

---

## 2. Encapsulation - Private Attributes, Properties, Getters/Setters

### What Is Encapsulation?

**Encapsulation** is the principle of bundling data and methods that operate on that data within a single unit (class), while controlling access to internal details. It's like a vending machine: you interact with buttons (public interface) but can't access the internal mechanisms (private implementation).

### Why Is It Important?

- **Data Protection**: Prevents unauthorized access or modification
- **Maintainability**: Internal changes don't affect external code
- **Validation**: Ensures data integrity through controlled access
- **Abstraction**: Hides complexity, exposing only what's necessary

### How It Works Under the Hood

Python uses **name mangling** for "private" attributes (those starting with `__`). The name `__private` becomes `_ClassName__private` internally, making it harder (but not impossible) to access from outside.

### Example 1: Private Attributes with Name Mangling

```python
class BankAccount:
    """
    Demonstrates private attributes using name mangling.
    Real-world scenario: Financial systems where balance should be protected.
    """
    
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number
        self.__balance = initial_balance  # Private: name mangling
    
    def get_balance(self):
        """Public method to access balance."""
        return self.__balance
    
    def deposit(self, amount):
        """Public method to modify balance safely."""
        if amount > 0:
            self.__balance += amount
        else:
            raise ValueError("Amount must be positive")
    
    def withdraw(self, amount):
        """Public method with validation."""
        if amount > 0 and amount <= self.__balance:
            self.__balance -= amount
        else:
            raise ValueError("Invalid withdrawal amount")

# Usage
account = BankAccount("12345", 1000)
print(account.get_balance())  # Output: 1000

# This won't work (raises AttributeError):
# print(account.__balance)

# But this works (not recommended, but possible):
# print(account._BankAccount__balance)  # Name mangling revealed
```

### Example 2: Properties with Getters and Setters

```python
class Circle:
    """
    Demonstrates @property decorator for controlled attribute access.
    Real-world scenario: Graphics applications where radius must be positive.
    """
    
    def __init__(self, radius):
        self._radius = radius  # Protected attribute (convention)
    
    @property
    def radius(self):
        """Getter: Access radius value."""
        return self._radius
    
    @radius.setter
    def radius(self, value):
        """Setter: Validate and set radius."""
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value
    
    @property
    def area(self):
        """Computed property: Calculate area from radius."""
        import math
        return math.pi * self._radius ** 2
    
    @property
    def diameter(self):
        """Computed property: Calculate diameter."""
        return 2 * self._radius

# Usage
circle = Circle(5)
print(circle.radius)  # Output: 5 (accessed like attribute)
print(circle.area)    # Output: 78.53981633974483 (computed)

circle.radius = 10    # Uses setter automatically
print(circle.area)    # Output: 314.1592653589793

# This will raise ValueError:
# circle.radius = -5
```

### Example 3: Email Validation with Properties

```python
class User:
    """
    Demonstrates property-based validation.
    Real-world scenario: User registration systems requiring valid email formats.
    """
    
    def __init__(self, username, email):
        self.username = username
        self._email = None  # Initialize as None
        self.email = email  # Use setter to validate
    
    @property
    def email(self):
        """Getter for email."""
        return self._email
    
    @email.setter
    def email(self, value):
        """Setter with email validation."""
        if not isinstance(value, str):
            raise TypeError("Email must be a string")
        if '@' not in value or '.' not in value.split('@')[1]:
            raise ValueError("Invalid email format")
        self._email = value
    
    def __repr__(self):
        return f"User(username='{self.username}', email='{self._email}')"

# Usage
user = User("alice", "alice@example.com")
print(user.email)  # Output: alice@example.com

# This will raise ValueError:
# user.email = "invalid-email"

# This works:
user.email = "alice.new@example.com"
print(user.email)  # Output: alice.new@example.com
```

### Example 4: Read-Only Properties

```python
class Product:
    """
    Demonstrates read-only properties.
    Real-world scenario: Products with immutable IDs but changeable prices.
    """
    
    def __init__(self, product_id, name, price):
        self._product_id = product_id  # Private, set once
        self.name = name
        self._price = price
    
    @property
    def product_id(self):
        """Read-only property: ID cannot be changed after creation."""
        return self._product_id
    
    @property
    def price(self):
        """Readable and writable property."""
        return self._price
    
    @price.setter
    def price(self, value):
        """Price can be updated with validation."""
        if value < 0:
            raise ValueError("Price cannot be negative")
        self._price = value
    
    @property
    def display_name(self):
        """Computed read-only property."""
        return f"ID: {self._product_id} - {self.name}"

# Usage
product = Product("P001", "Laptop", 999.99)
print(product.display_name)  # Output: ID: P001 - Laptop
print(product.product_id)  # Output: P001

# This will raise AttributeError (no setter defined):
# product.product_id = "P002"

# This works:
product.price = 899.99
print(product.price)  # Output: 899.99
```

### Real-World Scenarios

1. **API Classes**: Hide authentication tokens while providing controlled access
2. **Configuration Objects**: Validate settings before storing
3. **Database Models**: Ensure data integrity through property setters

### Common Pitfalls and Best Practices

**Pitfalls:**
- Using `__` (double underscore) when `_` (single underscore) is sufficient
- Creating properties for simple attributes that don't need validation
- Forgetting that properties are accessed without parentheses

**Best Practices:**
- Use `_` (single underscore) for "protected" attributes (convention)
- Use `__` (double underscore) sparingly, only when you truly need name mangling
- Use `@property` when you need validation, computation, or controlled access
- Keep getters and setters simple; complex logic belongs in separate methods
- Document why an attribute is private/protected

```python
# BAD: Unnecessary complexity
class BadExample:
    def __init__(self, value):
        self.__value = value
    
    def get_value(self):
        return self.__value
    
    def set_value(self, value):
        self.__value = value

# GOOD: Use properties when you need validation
class GoodExample:
    def __init__(self, value):
        self._value = value  # Protected, not private
    
    @property
    def value(self):
        return self._value
    
    @value.setter
    def value(self, val):
        if val < 0:
            raise ValueError("Value must be non-negative")
        self._value = val
```

---

## 3. Inheritance - Single and Multiple Inheritance

### What Is Inheritance?

**Inheritance** allows a class (child/subclass) to inherit attributes and methods from another class (parent/superclass). It's like genetic inheritance: a child inherits traits from parents but can also have unique characteristics.

**Single Inheritance**: A class inherits from one parent class
**Multiple Inheritance**: A class inherits from multiple parent classes

### Why Is It Important?

- **Code Reuse**: Avoid duplicating code across similar classes
- **Polymorphism**: Treat different classes uniformly through a common interface
- **Extensibility**: Add new functionality without modifying existing code
- **Modeling Relationships**: Represent "is-a" relationships (e.g., Dog is an Animal)

### How It Works Under the Hood

Python uses the **Method Resolution Order (MRO)** to determine which method to call when multiple inheritance is involved. The MRO follows the C3 linearization algorithm, ensuring a consistent order.

### Example 1: Basic Single Inheritance

```python
class Animal:
    """
    Base class (parent) defining common animal behavior.
    Real-world scenario: Modeling biological classification systems.
    """
    
    def __init__(self, name, species):
        self.name = name
        self.species = species
    
    def make_sound(self):
        """Generic sound - to be overridden by subclasses."""
        return "Some generic sound"
    
    def move(self):
        """Generic movement."""
        return f"{self.name} is moving"
    
    def get_info(self):
        """Common method available to all animals."""
        return f"{self.name} is a {self.species}"

class Dog(Animal):
    """
    Derived class (child) inheriting from Animal.
    Demonstrates method overriding.
    """
    
    def __init__(self, name, breed):
        # Call parent constructor using super()
        super().__init__(name, "Dog")
        self.breed = breed
    
    def make_sound(self):
        """Override parent method with specific behavior."""
        return "Woof!"
    
    def fetch(self):
        """Dog-specific method not in parent class."""
        return f"{self.name} is fetching the ball"

class Cat(Animal):
    """Another derived class with different behavior."""
    
    def __init__(self, name, color):
        super().__init__(name, "Cat")
        self.color = color
    
    def make_sound(self):
        return "Meow!"
    
    def climb(self):
        """Cat-specific method."""
        return f"{self.name} is climbing"

# Usage
dog = Dog("Buddy", "Golden Retriever")
cat = Cat("Whiskers", "Orange")

print(dog.get_info())      # Output: Buddy is a Dog (inherited method)
print(dog.make_sound())    # Output: Woof! (overridden method)
print(dog.fetch())         # Output: Buddy is fetching the ball (unique method)

print(cat.get_info())      # Output: Whiskers is a Cat
print(cat.make_sound())    # Output: Meow!
print(cat.climb())         # Output: Whiskers is climbing
```

### Example 2: Multiple Levels of Inheritance

```python
class Vehicle:
    """Base class for all vehicles."""
    
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
    
    def start_engine(self):
        return f"{self.make} {self.model} engine started"
    
    def get_info(self):
        return f"{self.year} {self.make} {self.model}"

class Car(Vehicle):
    """Intermediate class - inherits from Vehicle."""
    
    def __init__(self, make, model, year, doors):
        super().__init__(make, model, year)
        self.doors = doors
    
    def honk(self):
        return "Beep beep!"

class ElectricCar(Car):
    """Derived from Car, which derives from Vehicle."""
    
    def __init__(self, make, model, year, doors, battery_capacity):
        super().__init__(make, model, year, doors)
        self.battery_capacity = battery_capacity
    
    def start_engine(self):
        """Override: Electric cars don't have traditional engines."""
        return f"{self.make} {self.model} electric motor activated"
    
    def charge(self):
        """ElectricCar-specific method."""
        return f"Charging {self.battery_capacity}kWh battery"

# Usage
tesla = ElectricCar("Tesla", "Model 3", 2023, 4, 75)
print(tesla.get_info())      # Output: 2023 Tesla Model 3 (inherited from Vehicle)
print(tesla.honk())          # Output: Beep beep! (inherited from Car)
print(tesla.start_engine())  # Output: Tesla Model 3 electric motor activated (overridden)
print(tesla.charge())        # Output: Charging 75kWh battery (unique method)
```

### Example 3: Multiple Inheritance

```python
class Flyer:
    """Mixin class for flying capability."""
    
    def fly(self):
        return "Flying through the air"

class Swimmer:
    """Mixin class for swimming capability."""
    
    def swim(self):
        return "Swimming in water"

class Duck(Flyer, Swimmer):
    """
    Multiple inheritance: Duck can both fly and swim.
    Real-world scenario: Modeling complex behaviors through mixins.
    """
    
    def __init__(self, name):
        self.name = name
    
    def quack(self):
        return "Quack!"

class Penguin(Swimmer):
    """Penguins can swim but not fly."""
    
    def __init__(self, name):
        self.name = name
    
    def waddle(self):
        return "Waddling on land"

# Usage
duck = Duck("Donald")
print(duck.fly())    # Output: Flying through the air (from Flyer)
print(duck.swim())   # Output: Swimming in water (from Swimmer)
print(duck.quack())  # Output: Quack! (unique to Duck)

penguin = Penguin("Pingu")
print(penguin.swim())   # Output: Swimming in water (from Swimmer)
print(penguin.waddle()) # Output: Waddling on land (unique to Penguin)
# penguin.fly()  # AttributeError: Penguins can't fly!
```

### Example 4: Using super() for Method Chaining

```python
class Employee:
    """Base employee class."""
    
    def __init__(self, name, employee_id, salary):
        self.name = name
        self.employee_id = employee_id
        self.salary = salary
    
    def calculate_pay(self):
        return self.salary
    
    def get_info(self):
        return f"{self.name} (ID: {self.employee_id})"

class Manager(Employee):
    """Manager extends Employee with additional responsibilities."""
    
    def __init__(self, name, employee_id, salary, department, team_size):
        # Call parent __init__ first
        super().__init__(name, employee_id, salary)
        self.department = department
        self.team_size = team_size
    
    def calculate_pay(self):
        """Override but also use parent calculation."""
        base_pay = super().calculate_pay()
        bonus = self.team_size * 1000  # $1000 per team member
        return base_pay + bonus
    
    def get_info(self):
        """Extend parent method."""
        base_info = super().get_info()
        return f"{base_info}, Manager of {self.department} ({self.team_size} team members)"

# Usage
manager = Manager("Alice", "M001", 80000, "Engineering", 5)
print(manager.get_info())
# Output: Alice (ID: M001), Manager of Engineering (5 team members)
print(f"Total pay: ${manager.calculate_pay()}")
# Output: Total pay: $85000 (base + team bonus)
```

### Real-World Scenarios

1. **GUI Frameworks**: `Button` inherits from `Widget`, `Window` inherits from `Container`
2. **Database ORMs**: `User` model inherits from `BaseModel` with common database operations
3. **Game Development**: `Enemy` inherits from `Character` with shared movement/combat logic

### Common Pitfalls and Best Practices

**Pitfalls:**
- Not calling `super().__init__()` in child classes
- Diamond inheritance problems (multiple inheritance conflicts)
- Overriding methods without calling parent methods when needed
- Deep inheritance hierarchies that are hard to understand

**Best Practices:**
- Use `super()` instead of calling parent class directly
- Prefer composition over deep inheritance
- Keep inheritance hierarchies shallow (2-3 levels max)
- Use mixins for shared functionality
- Document the inheritance relationship clearly

```python
# BAD: Not using super(), deep hierarchy
class BadExample:
    class A: pass
    class B(A): pass
    class C(B): pass
    class D(C): pass  # Too deep!

# GOOD: Shallow hierarchy, using super()
class GoodExample:
    class Animal:
        def __init__(self, name):
            self.name = name
    
    class Dog(Animal):
        def __init__(self, name, breed):
            super().__init__(name)  # Use super()
            self.breed = breed
```

---

## 4. Polymorphism - Method Overriding, Duck Typing

### What Is Polymorphism?

**Polymorphism** (Greek for "many forms") allows objects of different types to be treated through the same interface. In Python, this manifests in two ways:

1. **Method Overriding**: Subclasses provide specific implementations of parent methods
2. **Duck Typing**: "If it walks like a duck and quacks like a duck, it's a duck" - objects are used based on their behavior, not their type

### Why Is It Important?

- **Flexibility**: Write code that works with multiple types
- **Extensibility**: Add new types without modifying existing code
- **Abstraction**: Focus on what objects can do, not what they are
- **Code Reuse**: Write generic functions that work with any compatible object

### How It Works Under the Hood

Python's dynamic typing means type checking happens at runtime. When you call a method, Python looks it up in the object's class hierarchy. Duck typing means Python doesn't check types explicitly - it just tries to call the method and handles errors if it fails.

### Example 1: Method Overriding (Polymorphism)

```python
class Shape:
    """
    Base class demonstrating polymorphism through method overriding.
    Real-world scenario: Graphics applications with different shape types.
    """
    
    def area(self):
        """Abstract method - should be overridden."""
        raise NotImplementedError("Subclass must implement area()")
    
    def perimeter(self):
        """Abstract method - should be overridden."""
        raise NotImplementedError("Subclass must implement perimeter()")
    
    def describe(self):
        """Common method available to all shapes."""
        return f"This shape has area {self.area():.2f}"

class Circle(Shape):
    """Circle implementation."""
    
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        """Override parent method with circle-specific calculation."""
        import math
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        """Override parent method."""
        import math
        return 2 * math.pi * self.radius

class Rectangle(Shape):
    """Rectangle implementation."""
    
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        """Override parent method with rectangle-specific calculation."""
        return self.width * self.height
    
    def perimeter(self):
        """Override parent method."""
        return 2 * (self.width + self.height)

# Polymorphic usage: same interface, different implementations
shapes = [
    Circle(5),
    Rectangle(4, 6),
    Circle(3)
]

# Process all shapes uniformly
for shape in shapes:
    print(f"Area: {shape.area():.2f}, Perimeter: {shape.perimeter():.2f}")
    print(shape.describe())  # Uses common method
    print()

# Output:
# Area: 78.54, Perimeter: 31.42
# This shape has area 78.54
# 
# Area: 24.00, Perimeter: 20.00
# This shape has area 24.00
# 
# Area: 28.27, Perimeter: 18.85
# This shape has area 28.27
```

### Example 2: Duck Typing

```python
class Dog:
    """Dog class with speak method."""
    
    def speak(self):
        return "Woof!"

class Cat:
    """Cat class with speak method."""
    
    def speak(self):
        return "Meow!"

class Robot:
    """Robot class with speak method - not related to animals!"""
    
    def speak(self):
        return "Beep boop!"

def make_it_speak(thing):
    """
    Duck typing: doesn't care about type, only about behavior.
    If it has a speak() method, it works!
    Real-world scenario: Plugin systems, event handlers, callbacks.
    """
    # No type checking - just try to use the method
    return thing.speak()

# All these work because they all have speak() method
animals = [Dog(), Cat(), Robot()]

for animal in animals:
    print(make_it_speak(animal))

# Output:
# Woof!
# Meow!
# Beep boop!
```

### Example 3: File Handler Polymorphism

```python
class FileHandler:
    """
    Base class for file operations.
    Demonstrates polymorphism in I/O operations.
    """
    
    def read(self):
        raise NotImplementedError
    
    def write(self, data):
        raise NotImplementedError

class TextFileHandler(FileHandler):
    """Text file implementation."""
    
    def __init__(self, filename):
        self.filename = filename
    
    def read(self):
        return f"Reading text from {self.filename}"
    
    def write(self, data):
        return f"Writing text to {self.filename}: {data}"

class CSVFileHandler(FileHandler):
    """CSV file implementation."""
    
    def __init__(self, filename):
        self.filename = filename
    
    def read(self):
        return f"Reading CSV from {self.filename}"
    
    def write(self, data):
        return f"Writing CSV to {self.filename}: {data}"

class JSONFileHandler(FileHandler):
    """JSON file implementation."""
    
    def __init__(self, filename):
        self.filename = filename
    
    def read(self):
        return f"Reading JSON from {self.filename}"
    
    def write(self, data):
        return f"Writing JSON to {self.filename}: {data}"

def process_file(handler, data):
    """
    Polymorphic function: works with any FileHandler subclass.
    Real-world scenario: Data processing pipelines.
    """
    handler.write(data)
    return handler.read()

# Use different handlers polymorphically
handlers = [
    TextFileHandler("data.txt"),
    CSVFileHandler("data.csv"),
    JSONFileHandler("data.json")
]

for handler in handlers:
    result = process_file(handler, "sample data")
    print(result)
    print()

# Output:
# Reading text from data.txt
# Reading CSV from data.csv
# Reading JSON from data.json
```

### Example 4: Payment Processing with Duck Typing

```python
class CreditCard:
    """Credit card payment method."""
    
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        return f"Processing ${amount} with credit card ending in {self.card_number[-4:]}"

class PayPal:
    """PayPal payment method."""
    
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        return f"Processing ${amount} via PayPal account {self.email}"

class BankTransfer:
    """Bank transfer payment method."""
    
    def __init__(self, account_number):
        self.account_number = account_number
    
    def process_payment(self, amount):
        return f"Processing ${amount} via bank transfer to account {self.account_number}"

class PaymentProcessor:
    """
    Payment processor using duck typing.
    Doesn't care about specific payment type, only that it has process_payment().
    Real-world scenario: E-commerce checkout systems.
    """
    
    def __init__(self):
        self.payment_method = None
    
    def set_payment_method(self, method):
        """Accept any object with process_payment() method."""
        self.payment_method = method
    
    def checkout(self, amount):
        """Process payment using whatever method was set."""
        if self.payment_method is None:
            raise ValueError("No payment method set")
        return self.payment_method.process_payment(amount)

# Usage: All payment methods work the same way
processor = PaymentProcessor()

# Use credit card
processor.set_payment_method(CreditCard("1234567890123456"))
print(processor.checkout(100))
# Output: Processing $100 with credit card ending in 3456

# Switch to PayPal - same interface!
processor.set_payment_method(PayPal("user@example.com"))
print(processor.checkout(50))
# Output: Processing $50 via PayPal account user@example.com

# Switch to bank transfer - still works!
processor.set_payment_method(BankTransfer("ACC123456"))
print(processor.checkout(200))
# Output: Processing $200 via bank transfer to account ACC123456
```

### Real-World Scenarios

1. **Database Drivers**: Different database implementations (MySQL, PostgreSQL) with same interface
2. **Web Frameworks**: Different request handlers with common interface
3. **Serialization**: JSON, XML, YAML serializers with same methods

### Common Pitfalls and Best Practices

**Pitfalls:**
- Assuming all objects have the same methods without checking
- Not handling AttributeError when duck typing fails
- Over-engineering with abstract base classes when duck typing suffices

**Best Practices:**
- Embrace duck typing - it's Pythonic!
- Use abstract base classes (ABC) when you need to enforce interfaces
- Document expected methods/behavior clearly
- Use `hasattr()` or try/except for optional duck typing
- Keep interfaces simple and consistent

```python
# BAD: Unnecessary type checking
def bad_example(obj):
    if isinstance(obj, Dog):
        return obj.speak()
    elif isinstance(obj, Cat):
        return obj.speak()
    # Too rigid!

# GOOD: Duck typing
def good_example(obj):
    # Just use it - if it has speak(), it works!
    return obj.speak()

# Even better: Handle missing methods gracefully
def better_example(obj):
    if hasattr(obj, 'speak'):
        return obj.speak()
    else:
        return "No speak method available"
```

---

## 5. Special Methods - `__init__`, `__str__`, `__repr__`, etc.

### What Are Special Methods?

**Special methods** (also called "dunder methods" for "double underscore") are methods with special names that Python calls automatically in specific situations. They allow you to define how objects behave with built-in operations.

### Why Are They Important?

- **Integration**: Make your objects work seamlessly with Python's built-in functions
- **Readability**: Enable intuitive operations like `obj1 + obj2`
- **Debugging**: `__repr__` helps with debugging and logging
- **Container Behavior**: Make objects behave like lists, dictionaries, etc.

### How It Works Under the Hood

When you write `obj1 + obj2`, Python internally calls `obj1.__add__(obj2)`. When you write `print(obj)`, Python calls `obj.__str__()`. These methods hook into Python's internal mechanisms.

### Example 1: String Representation (`__str__` and `__repr__`)

```python
class Point:
    """
    Demonstrates __str__ and __repr__ for object representation.
    Real-world scenario: Coordinate systems, vector mathematics.
    """
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        """
        User-friendly string representation.
        Called by str(), print(), and format().
        """
        return f"Point({self.x}, {self.y})"
    
    def __repr__(self):
        """
        Unambiguous string representation.
        Should be valid Python code that recreates the object.
        Called by repr() and in containers.
        """
        return f"Point({self.x}, {self.y})"

# Usage
point = Point(3, 4)

print(str(point))   # Output: Point(3, 4) (uses __str__)
print(repr(point))  # Output: Point(3, 4) (uses __repr__)
print(point)        # Output: Point(3, 4) (uses __str__)

# In containers, __repr__ is used
points = [Point(1, 2), Point(3, 4)]
print(points)  # Output: [Point(1, 2), Point(3, 4)]
```

### Example 2: Comparison Methods

```python
class Student:
    """
    Demonstrates comparison special methods.
    Real-world scenario: Sorting students by grade, comparing performance.
    """
    
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade
    
    def __eq__(self, other):
        """Equality: == operator"""
        if not isinstance(other, Student):
            return NotImplemented
        return self.grade == other.grade
    
    def __lt__(self, other):
        """Less than: < operator"""
        if not isinstance(other, Student):
            return NotImplemented
        return self.grade < other.grade
    
    def __le__(self, other):
        """Less than or equal: <= operator"""
        return self < other or self == other
    
    def __gt__(self, other):
        """Greater than: > operator"""
        return not self <= other
    
    def __ge__(self, other):
        """Greater than or equal: >= operator"""
        return not self < other
    
    def __repr__(self):
        return f"Student('{self.name}', {self.grade})"

# Usage
alice = Student("Alice", 85)
bob = Student("Bob", 90)
charlie = Student("Charlie", 85)

print(alice == charlie)  # Output: True (same grade)
print(alice < bob)       # Output: True (85 < 90)
print(bob > alice)       # Output: True (90 > 85)

# Can now sort students
students = [Student("Alice", 85), Student("Bob", 90), Student("Charlie", 80)]
sorted_students = sorted(students)
print(sorted_students)
# Output: [Student('Charlie', 80), Student('Alice', 85), Student('Bob', 90)]
```

### Example 3: Arithmetic Operations

```python
class Vector:
    """
    Demonstrates arithmetic special methods.
    Real-world scenario: Mathematical computations, physics simulations.
    """
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        """Addition: + operator"""
        if not isinstance(other, Vector):
            return NotImplemented
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        """Subtraction: - operator"""
        if not isinstance(other, Vector):
            return NotImplemented
        return Vector(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        """Multiplication: * operator (scalar multiplication)"""
        if not isinstance(scalar, (int, float)):
            return NotImplemented
        return Vector(self.x * scalar, self.y * scalar)
    
    def __rmul__(self, scalar):
        """Right multiplication: allows 2 * vector (not just vector * 2)"""
        return self.__mul__(scalar)
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"

# Usage
v1 = Vector(3, 4)
v2 = Vector(1, 2)

print(v1 + v2)   # Output: Vector(4, 6)
print(v1 - v2)   # Output: Vector(2, 2)
print(v1 * 2)    # Output: Vector(6, 8)
print(2 * v1)    # Output: Vector(6, 8) (thanks to __rmul__)
```

### Example 4: Container Behavior (`__len__`, `__getitem__`, `__setitem__`)

```python
class Playlist:
    """
    Demonstrates making objects behave like containers.
    Real-world scenario: Custom data structures, collections.
    """
    
    def __init__(self):
        self._songs = []
    
    def add_song(self, song):
        """Add a song to the playlist."""
        self._songs.append(song)
    
    def __len__(self):
        """Length: len() function"""
        return len(self._songs)
    
    def __getitem__(self, index):
        """Indexing: playlist[0]"""
        return self._songs[index]
    
    def __setitem__(self, index, value):
        """Assignment: playlist[0] = 'new song'"""
        self._songs[index] = value
    
    def __contains__(self, item):
        """Membership: 'song' in playlist"""
        return item in self._songs
    
    def __iter__(self):
        """Iteration: for song in playlist"""
        return iter(self._songs)
    
    def __str__(self):
        return f"Playlist with {len(self._songs)} songs"

# Usage
playlist = Playlist()
playlist.add_song("Song 1")
playlist.add_song("Song 2")
playlist.add_song("Song 3")

print(len(playlist))        # Output: 3 (uses __len__)
print(playlist[0])          # Output: Song 1 (uses __getitem__)
playlist[1] = "New Song"    # Uses __setitem__
print(playlist[1])          # Output: New Song

print("Song 1" in playlist)  # Output: True (uses __contains__)

# Iteration works!
for song in playlist:        # Uses __iter__
    print(song)
# Output:
# Song 1
# New Song
# Song 3
```

### Example 5: Callable Objects (`__call__`)

```python
class Multiplier:
    """
    Demonstrates making objects callable like functions.
    Real-world scenario: Function factories, decorators, callbacks.
    """
    
    def __init__(self, factor):
        self.factor = factor
    
    def __call__(self, value):
        """Make object callable: multiplier(5)"""
        return value * self.factor
    
    def __repr__(self):
        return f"Multiplier({self.factor})"

# Usage
double = Multiplier(2)
triple = Multiplier(3)

print(double(5))   # Output: 10 (object called like function)
print(triple(4))    # Output: 12

# Can be used in higher-order functions
numbers = [1, 2, 3, 4, 5]
doubled = list(map(double, numbers))
print(doubled)  # Output: [2, 4, 6, 8, 10]
```

### Example 6: Boolean Conversion (`__bool__`)

```python
class Container:
    """
    Demonstrates custom boolean behavior.
    Real-world scenario: Custom collections, validation objects.
    """
    
    def __init__(self):
        self._items = []
    
    def add_item(self, item):
        self._items.append(item)
    
    def __bool__(self):
        """Boolean conversion: bool(container)"""
        return len(self._items) > 0
    
    def __len__(self):
        return len(self._items)

# Usage
container = Container()

if container:  # Uses __bool__
    print("Container has items")
else:
    print("Container is empty")  # This prints

container.add_item("item")

if container:  # Now True
    print("Container has items")  # This prints
```

### Real-World Scenarios

1. **Mathematical Libraries**: `Vector`, `Matrix` classes with arithmetic operations
2. **Data Structures**: Custom lists, queues, stacks with container behavior
3. **API Wrappers**: Objects that behave like dictionaries for easy access

### Common Pitfalls and Best Practices

**Pitfalls:**
- Forgetting to return `NotImplemented` for unsupported types
- Not implementing `__repr__` (falls back to default, which is unhelpful)
- Modifying immutable types incorrectly

**Best Practices:**
- Always implement `__repr__` for debugging
- Return `NotImplemented` (not `NotImplementedError`) for unsupported operations
- Make `__repr__` unambiguous and ideally executable
- Use `__str__` for user-friendly output, `__repr__` for developers
- Document which operations are supported

```python
# BAD: Missing __repr__, wrong return value
class BadExample:
    def __add__(self, other):
        if not isinstance(other, BadExample):
            raise TypeError("Unsupported")  # Should return NotImplemented

# GOOD: Proper special method implementation
class GoodExample:
    def __repr__(self):
        return f"GoodExample(...)"  # Unambiguous
    
    def __add__(self, other):
        if not isinstance(other, GoodExample):
            return NotImplemented  # Correct return value
        # ... implementation
```

---

## 6. Class & Static Methods - Decorators and Alternative Constructors

### What Are Class and Static Methods?

**Class Methods**: Methods bound to the class rather than an instance. They receive the class as the first argument (`cls` instead of `self`).

**Static Methods**: Methods that belong to a class but don't access instance or class data. They're essentially regular functions organized within a class namespace.

### Why Are They Important?

- **Alternative Constructors**: Create objects in different ways (e.g., `Date.from_string()`)
- **Factory Patterns**: Create instances of different subclasses
- **Utility Functions**: Organize related functions within a class
- **Class-Level Operations**: Work with class variables, not instance variables

### How It Works Under the Hood

`@classmethod` creates a method descriptor that passes the class as the first argument. `@staticmethod` creates a method descriptor that doesn't pass `self` or `cls` - it's just a regular function bound to the class namespace.

### Example 1: Alternative Constructors

```python
class Date:
    """
    Demonstrates class methods as alternative constructors.
    Real-world scenario: Parsing dates from different formats.
    """
    
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
    
    @classmethod
    def from_string(cls, date_string):
        """
        Alternative constructor: Create Date from string "YYYY-MM-DD".
        'cls' refers to the class (Date), not an instance.
        """
        parts = date_string.split('-')
        return cls(int(parts[0]), int(parts[1]), int(parts[2]))
    
    @classmethod
    def from_timestamp(cls, timestamp):
        """Another alternative constructor."""
        import datetime
        dt = datetime.datetime.fromtimestamp(timestamp)
        return cls(dt.year, dt.month, dt.day)
    
    def __repr__(self):
        return f"Date({self.year}, {self.month}, {self.day})"

# Usage
# Standard constructor
date1 = Date(2024, 1, 15)
print(date1)  # Output: Date(2024, 1, 15)

# Alternative constructor
date2 = Date.from_string("2024-01-15")
print(date2)  # Output: Date(2024, 1, 15)

# Another alternative constructor
date3 = Date.from_timestamp(1705276800)  # Unix timestamp
print(date3)  # Output: Date(2024, 1, 15)
```

### Example 2: Factory Pattern with Class Methods

```python
class Person:
    """Base Person class."""
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    @classmethod
    def create_child(cls, name):
        """Factory method: Create a child (age 5-12)."""
        import random
        age = random.randint(5, 12)
        return cls(name, age)
    
    @classmethod
    def create_adult(cls, name):
        """Factory method: Create an adult (age 18-65)."""
        import random
        age = random.randint(18, 65)
        return cls(name, age)
    
    @classmethod
    def create_senior(cls, name):
        """Factory method: Create a senior (age 65+)."""
        import random
        age = random.randint(65, 90)
        return cls(name, age)
    
    def __repr__(self):
        return f"Person('{self.name}', {self.age})"

# Usage
child = Person.create_child("Alice")
adult = Person.create_adult("Bob")
senior = Person.create_senior("Charlie")

print(child)   # Output: Person('Alice', 8) (random age 5-12)
print(adult)   # Output: Person('Bob', 35) (random age 18-65)
print(senior)  # Output: Person('Charlie', 72) (random age 65-90)
```

### Example 3: Static Methods for Utility Functions

```python
class MathUtils:
    """
    Demonstrates static methods for utility functions.
    Real-world scenario: Organizing related utility functions.
    """
    
    @staticmethod
    def is_prime(n):
        """Check if a number is prime. No need for instance or class."""
        if n < 2:
            return False
        for i in range(2, int(n ** 0.5) + 1):
            if n % i == 0:
                return False
        return True
    
    @staticmethod
    def factorial(n):
        """Calculate factorial. Pure function, no state needed."""
        if n < 0:
            raise ValueError("Factorial not defined for negative numbers")
        result = 1
        for i in range(1, n + 1):
            result *= i
        return result
    
    @staticmethod
    def gcd(a, b):
        """Calculate greatest common divisor using Euclidean algorithm."""
        while b:
            a, b = b, a % b
        return a

# Usage: Can be called on class or instance
print(MathUtils.is_prime(7))      # Output: True
print(MathUtils.factorial(5))      # Output: 120
print(MathUtils.gcd(48, 18))       # Output: 6

# Also works on instance (but not recommended)
utils = MathUtils()
print(utils.is_prime(11))          # Output: True (works but unnecessary)
```

### Example 4: Class Variables and Class Methods

```python
class Car:
    """
    Demonstrates class variables and class methods.
    Real-world scenario: Tracking instances, managing shared state.
    """
    
    total_cars = 0  # Class variable: shared across all instances
    
    def __init__(self, make, model):
        self.make = make
        self.model = model
        Car.total_cars += 1  # Increment class variable
    
    @classmethod
    def get_total_cars(cls):
        """Class method to access class variable."""
        return cls.total_cars
    
    @classmethod
    def reset_counter(cls):
        """Class method to modify class variable."""
        cls.total_cars = 0
    
    def __repr__(self):
        return f"Car('{self.make}', '{self.model}')"

# Usage
print(Car.get_total_cars())  # Output: 0

car1 = Car("Toyota", "Camry")
car2 = Car("Honda", "Civic")

print(Car.get_total_cars())  # Output: 2 (shared across all instances)
print(car1.get_total_cars())  # Output: 2 (can call on instance too)

car3 = Car("Ford", "Mustang")
print(Car.get_total_cars())  # Output: 3
```

### Example 5: Configuration-Based Construction

```python
class DatabaseConnection:
    """
    Demonstrates class method for configuration-based construction.
    Real-world scenario: Database connections from config files.
    """
    
    def __init__(self, host, port, database, username=None, password=None):
        self.host = host
        self.port = port
        self.database = database
        self.username = username
        self.password = password
    
    @classmethod
    def from_config(cls, config_dict):
        """
        Create connection from configuration dictionary.
        Useful for loading from JSON/YAML config files.
        """
        return cls(
            host=config_dict.get('host'),
            port=config_dict.get('port'),
            database=config_dict.get('database'),
            username=config_dict.get('username'),
            password=config_dict.get('password')
        )
    
    @classmethod
    def from_env(cls):
        """Create connection from environment variables."""
        import os
        return cls(
            host=os.getenv('DB_HOST', 'localhost'),
            port=int(os.getenv('DB_PORT', 5432)),
            database=os.getenv('DB_NAME'),
            username=os.getenv('DB_USER'),
            password=os.getenv('DB_PASS')
        )
    
    def __repr__(self):
        return f"DatabaseConnection(host='{self.host}', port={self.port})"

# Usage
config = {
    'host': 'localhost',
    'port': 5432,
    'database': 'mydb',
    'username': 'user',
    'password': 'pass'
}

db = DatabaseConnection.from_config(config)
print(db)  # Output: DatabaseConnection(host='localhost', port=5432)

# Or from environment
# db = DatabaseConnection.from_env()
```

### Real-World Scenarios

1. **Date/Time Libraries**: Multiple ways to create dates (`from_string`, `from_timestamp`)
2. **ORM Models**: `Model.objects.create()` class methods in Django
3. **Configuration Management**: Creating objects from config files

### Common Pitfalls and Best Practices

**Pitfalls:**
- Using `@staticmethod` when you need `@classmethod` (or vice versa)
- Forgetting `cls` parameter in class methods
- Using instance methods when class/static methods are more appropriate

**Best Practices:**
- Use `@classmethod` for alternative constructors and factory methods
- Use `@staticmethod` for utility functions that don't need class/instance data
- Use `cls` (not the class name) in class methods for inheritance to work correctly
- Document why a method is class/static, not instance

```python
# BAD: Wrong decorator, hardcoded class name
class BadExample:
    @staticmethod
    def create():
        return BadExample()  # Hardcoded, breaks inheritance
    
    def utility():
        return "utility"  # Missing @staticmethod

# GOOD: Proper use of class/static methods
class GoodExample:
    @classmethod
    def create(cls):
        return cls()  # Uses cls, works with inheritance
    
    @staticmethod
    def utility():
        return "utility"  # Properly decorated
```

---

## 7. Abstract Classes - ABC Module, Interfaces

### What Are Abstract Classes?

**Abstract classes** are classes that cannot be instantiated directly. They define a common interface that subclasses must implement. Think of them as contracts: "If you want to be this type of object, you must implement these methods."

### Why Are They Important?

- **Enforce Interfaces**: Guarantee that subclasses implement required methods
- **Documentation**: Clearly define what methods a class should have
- **Polymorphism**: Work with objects through their abstract interface
- **Prevent Errors**: Catch missing implementations at class definition time, not runtime

### How It Works Under the Hood

Python's `abc` module uses metaclasses to prevent instantiation of abstract classes. When you try to create an instance, Python checks if all abstract methods are implemented. If not, it raises a `TypeError`.

### Example 1: Basic Abstract Base Class

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    """
    Abstract base class defining the interface for all animals.
    Real-world scenario: Game development, simulation systems.
    """
    
    def __init__(self, name):
        self.name = name
    
    @abstractmethod
    def make_sound(self):
        """Abstract method: must be implemented by subclasses."""
        pass
    
    @abstractmethod
    def move(self):
        """Abstract method: must be implemented by subclasses."""
        pass
    
    def get_info(self):
        """Concrete method: available to all subclasses."""
        return f"{self.name} is an animal"

class Dog(Animal):
    """Concrete implementation of Animal."""
    
    def make_sound(self):
        return "Woof!"
    
    def move(self):
        return f"{self.name} is running"

class Bird(Animal):
    """Another concrete implementation."""
    
    def make_sound(self):
        return "Tweet!"
    
    def move(self):
        return f"{self.name} is flying"

# Usage
dog = Dog("Buddy")
bird = Bird("Tweety")

print(dog.make_sound())  # Output: Woof!
print(bird.move())       # Output: Tweety is flying

# This will raise TypeError:
# animal = Animal("Generic")  # Can't instantiate abstract class
```

### Example 2: Abstract Properties

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    """
    Abstract class with abstract property.
    Real-world scenario: Graphics libraries, CAD software.
    """
    
    @property
    @abstractmethod
    def area(self):
        """Abstract property: must be implemented by subclasses."""
        pass
    
    @property
    @abstractmethod
    def perimeter(self):
        """Abstract property: must be implemented."""
        pass

class Circle(Shape):
    """Concrete implementation."""
    
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2
    
    @property
    def perimeter(self):
        import math
        return 2 * math.pi * self._radius

class Rectangle(Shape):
    """Another concrete implementation."""
    
    def __init__(self, width, height):
        self._width = width
        self._height = height
    
    @property
    def area(self):
        return self._width * self._height
    
    @property
    def perimeter(self):
        return 2 * (self._width + self._height)

# Usage
circle = Circle(5)
rectangle = Rectangle(4, 6)

print(f"Circle area: {circle.area:.2f}")        # Output: Circle area: 78.54
print(f"Rectangle area: {rectangle.area}")      # Output: Rectangle area: 24
```

### Example 3: Abstract Class with Concrete Methods

```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):
    """
    Abstract class with template method pattern.
    Real-world scenario: Data processing pipelines.
    """
    
    @abstractmethod
    def load_data(self):
        """Abstract: Subclasses must implement."""
        pass
    
    @abstractmethod
    def process_data(self):
        """Abstract: Subclasses must implement."""
        pass
    
    @abstractmethod
    def save_data(self):
        """Abstract: Subclasses must implement."""
        pass
    
    def run(self):
        """
        Template method: Defines the algorithm structure.
        Concrete method that uses abstract methods.
        """
        print("Starting data processing pipeline...")
        self.load_data()
        self.process_data()
        self.save_data()
        print("Pipeline completed!")

class CSVProcessor(DataProcessor):
    """Concrete implementation for CSV files."""
    
    def load_data(self):
        print("Loading CSV data...")
    
    def process_data(self):
        print("Processing CSV data...")
    
    def save_data(self):
        print("Saving processed CSV data...")

class JSONProcessor(DataProcessor):
    """Concrete implementation for JSON files."""
    
    def load_data(self):
        print("Loading JSON data...")
    
    def process_data(self):
        print("Processing JSON data...")
    
    def save_data(self):
        print("Saving processed JSON data...")

# Usage
csv_processor = CSVProcessor()
csv_processor.run()
# Output:
# Starting data processing pipeline...
# Loading CSV data...
# Processing CSV data...
# Saving processed CSV data...
# Pipeline completed!

json_processor = JSONProcessor()
json_processor.run()
# Output:
# Starting data processing pipeline...
# Loading JSON data...
# Processing JSON data...
# Saving processed JSON data...
# Pipeline completed!
```

### Example 4: Interface Pattern

```python
from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    """
    Interface for payment methods.
    Real-world scenario: Payment processing systems.
    """
    
    @abstractmethod
    def process_payment(self, amount):
        """Process a payment. Must return transaction ID."""
        pass
    
    @abstractmethod
    def refund(self, transaction_id):
        """Refund a previous transaction."""
        pass

class CreditCard(PaymentMethod):
    """Credit card payment implementation."""
    
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        # Simulate payment processing
        transaction_id = f"CC-{self.card_number[-4:]}-{amount}"
        print(f"Processing ${amount} with credit card")
        return transaction_id
    
    def refund(self, transaction_id):
        print(f"Refunding transaction {transaction_id}")
        return f"REFUND-{transaction_id}"

class PayPal(PaymentMethod):
    """PayPal payment implementation."""
    
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        transaction_id = f"PP-{self.email}-{amount}"
        print(f"Processing ${amount} via PayPal")
        return transaction_id
    
    def refund(self, transaction_id):
        print(f"Refunding PayPal transaction {transaction_id}")
        return f"REFUND-{transaction_id}"

# Usage: All payment methods work the same way
def checkout(payment_method, amount):
    """Function that works with any PaymentMethod."""
    return payment_method.process_payment(amount)

credit_card = CreditCard("1234567890123456")
paypal = PayPal("user@example.com")

print(checkout(credit_card, 100))  # Output: Processing $100 with credit card
print(checkout(paypal, 50))         # Output: Processing $50 via PayPal
```

### Real-World Scenarios

1. **Plugin Systems**: Define interfaces that plugins must implement
2. **Database Drivers**: Abstract database operations, concrete implementations for different databases
3. **GUI Frameworks**: Abstract widget classes with concrete implementations

### Common Pitfalls and Best Practices

**Pitfalls:**
- Forgetting to call `super().__init__()` in abstract class constructors
- Not implementing all abstract methods (causes runtime error)
- Overusing abstract classes when duck typing would suffice

**Best Practices:**
- Use abstract classes when you need to enforce an interface
- Document abstract methods clearly
- Provide default implementations when possible
- Use `@abstractmethod` decorator properly
- Consider whether you really need abstract classes (Python's duck typing is powerful)

```python
# BAD: Abstract class without abstract methods, or missing implementation
class BadExample(ABC):
    def method(self):  # Not abstract, but should be
        pass

class BadImplementation(BadExample):
    pass  # Missing method implementation

# GOOD: Proper abstract class usage
class GoodExample(ABC):
    @abstractmethod
    def method(self):
        """Must be implemented by subclasses."""
        pass

class GoodImplementation(GoodExample):
    def method(self):
        return "Implemented!"
```

---

## 8. Composition - Has-a Relationships

### What Is Composition?

**Composition** is a design principle where a class contains instances of other classes, representing a "has-a" relationship. Unlike inheritance (is-a), composition builds complex objects by combining simpler ones.

**Composition**: "Car has an Engine" (Car contains Engine)
**Inheritance**: "Car is a Vehicle" (Car extends Vehicle)

### Why Is It Important?

- **Flexibility**: Change components without affecting the whole
- **Reusability**: Use components in multiple contexts
- **Maintainability**: Isolate changes to specific components
- **Avoids Deep Hierarchies**: Prevents complex inheritance chains

### How It Works Under the Hood

Composition is simply storing object references as attributes. When you create a `Car` with an `Engine`, the `Car` object contains a reference to the `Engine` object in its `__dict__`.

### Example 1: Basic Composition

```python
class Engine:
    """Component class: Engine."""
    
    def __init__(self, engine_type):
        self.engine_type = engine_type
    
    def start(self):
        return f"{self.engine_type} engine started"
    
    def stop(self):
        return f"{self.engine_type} engine stopped"

class Car:
    """
    Composite class: Car has an Engine.
    Real-world scenario: Modeling complex systems with components.
    """
    
    def __init__(self, make, model, engine_type):
        self.make = make
        self.model = model
        # Composition: Car HAS an Engine
        self.engine = Engine(engine_type)
    
    def start(self):
        """Delegate to engine component."""
        return self.engine.start()
    
    def stop(self):
        """Delegate to engine component."""
        return self.engine.stop()
    
    def get_info(self):
        return f"{self.make} {self.model} with {self.engine.engine_type} engine"

# Usage
car = Car("Toyota", "Camry", "V6")
print(car.start())    # Output: V6 engine started
print(car.get_info()) # Output: Toyota Camry with V6 engine

# Can access engine directly
print(car.engine.engine_type)  # Output: V6
```

### Example 2: Composition vs Inheritance

```python
class Person:
    """Base person class."""
    
    def __init__(self, name):
        self.name = name
    
    def think(self):
        return f"{self.name} is thinking"

class Brain:
    """Component: Brain."""
    
    def __init__(self):
        self.iq = 100
    
    def process(self, thought):
        return f"Processing: {thought}"

class Student:
    """
    Uses composition: Student HAS a Person and HAS a School.
    Better than inheritance when relationships are "has-a" not "is-a".
    """
    
    def __init__(self, person, school):
        self.person = person      # Composition: Student has Person
        self.school = school      # Composition: Student has School
        self.brain = Brain()      # Composition: Student has Brain
    
    def study(self, subject):
        thought = f"Studying {subject}"
        processed = self.brain.process(thought)
        return f"{self.person.name} at {self.school.name}: {processed}"

class School:
    """Component: School."""
    
    def __init__(self, name):
        self.name = name

# Usage
person = Person("Alice")
school = School("MIT")
student = Student(person, school)

print(student.study("Python"))
# Output: Alice at MIT: Processing: Studying Python
```

### Example 3: Library System with Composition

```python
class Book:
    """Component: Book."""
    
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_checked_out = False
    
    def checkout(self):
        self.is_checked_out = True
    
    def return_book(self):
        self.is_checked_out = False
    
    def __repr__(self):
        status = "checked out" if self.is_checked_out else "available"
        return f"Book('{self.title}', {status})"

class Library:
    """
    Composite: Library HAS many Books.
    Real-world scenario: Library management systems.
    """
    
    def __init__(self, name):
        self.name = name
        self.books = []  # Composition: Library contains list of Books
    
    def add_book(self, book):
        """Add a book to the library."""
        self.books.append(book)
    
    def remove_book(self, isbn):
        """Remove a book by ISBN."""
        self.books = [b for b in self.books if b.isbn != isbn]
    
    def find_book(self, title):
        """Find a book by title."""
        for book in self.books:
            if book.title == title:
                return book
        return None
    
    def get_available_books(self):
        """Get all available books."""
        return [book for book in self.books if not book.is_checked_out]

# Usage
library = Library("City Library")

book1 = Book("Python Guide", "John Doe", "123-456")
book2 = Book("OOP Basics", "Jane Smith", "789-012")

library.add_book(book1)
library.add_book(book2)

found = library.find_book("Python Guide")
if found:
    found.checkout()
    print(found)  # Output: Book('Python Guide', checked out)

available = library.get_available_books()
print(f"Available books: {len(available)}")  # Output: Available books: 1
```

### Example 4: Computer System Composition

```python
class CPU:
    """Component: CPU."""
    
    def __init__(self, model, speed):
        self.model = model
        self.speed = speed
    
    def process(self):
        return f"Processing with {self.model} at {self.speed}GHz"

class RAM:
    """Component: RAM."""
    
    def __init__(self, capacity, type):
        self.capacity = capacity
        self.type = type

class Storage:
    """Component: Storage."""
    
    def __init__(self, capacity, type):
        self.capacity = capacity
        self.type = type

class Computer:
    """
    Composite: Computer HAS CPU, RAM, and Storage.
    Real-world scenario: System configuration, hardware management.
    """
    
    def __init__(self, cpu, ram, storage):
        self.cpu = cpu      # Composition
        self.ram = ram      # Composition
        self.storage = storage  # Composition
    
    def get_specs(self):
        """Aggregate specs from all components."""
        return {
            'CPU': f"{self.cpu.model} @ {self.cpu.speed}GHz",
            'RAM': f"{self.ram.capacity}GB {self.ram.type}",
            'Storage': f"{self.storage.capacity}GB {self.storage.type}"
        }
    
    def upgrade_ram(self, new_ram):
        """Replace component (flexibility of composition)."""
        self.ram = new_ram

# Usage
cpu = CPU("Intel i7", 3.5)
ram = RAM(16, "DDR4")
storage = Storage(512, "SSD")

computer = Computer(cpu, ram, storage)
specs = computer.get_specs()
print(specs)
# Output: {'CPU': 'Intel i7 @ 3.5GHz', 'RAM': '16GB DDR4', 'Storage': '512GB SSD'}

# Easy to upgrade components
new_ram = RAM(32, "DDR4")
computer.upgrade_ram(new_ram)
print(computer.ram.capacity)  # Output: 32
```

### Example 5: Shopping Cart with Composition

```python
class Product:
    """Component: Product."""
    
    def __init__(self, name, price):
        self.name = name
        self.price = price

class CartItem:
    """Component: Cart Item (has Product)."""
    
    def __init__(self, product, quantity):
        self.product = product  # Composition: CartItem has Product
        self.quantity = quantity
    
    def get_total(self):
        return self.product.price * self.quantity

class ShoppingCart:
    """
    Composite: ShoppingCart HAS many CartItems.
    Real-world scenario: E-commerce systems.
    """
    
    def __init__(self):
        self.items = []  # Composition: Cart contains CartItems
    
    def add_item(self, cart_item):
        """Add item to cart."""
        self.items.append(cart_item)
    
    def remove_item(self, product_name):
        """Remove item by product name."""
        self.items = [item for item in self.items 
                     if item.product.name != product_name]
    
    def get_total_price(self):
        """Calculate total from all items."""
        return sum(item.get_total() for item in self.items)
    
    def get_item_count(self):
        """Get total number of items."""
        return sum(item.quantity for item in self.items)

# Usage
laptop = Product("Laptop", 999)
mouse = Product("Mouse", 25)

cart = ShoppingCart()
cart.add_item(CartItem(laptop, 1))
cart.add_item(CartItem(mouse, 2))

print(f"Total: ${cart.get_total_price()}")  # Output: Total: $1049
print(f"Items: {cart.get_item_count()}")     # Output: Items: 3
```

### Real-World Scenarios

1. **Game Development**: `Character` has `Weapon`, `Armor`, `Inventory`
2. **Web Applications**: `User` has `Profile`, `Settings`, `Permissions`
3. **GUI Applications**: `Window` has `MenuBar`, `ToolBar`, `StatusBar`

### Common Pitfalls and Best Practices

**Pitfalls:**
- Using inheritance when composition is more appropriate
- Creating circular dependencies between components
- Not properly managing component lifecycles

**Best Practices:**
- Prefer composition over inheritance when possible
- Keep components loosely coupled
- Use composition for "has-a" relationships
- Use inheritance for "is-a" relationships
- Make components reusable and independent

```python
# BAD: Using inheritance for "has-a" relationship
class BadCar(Vehicle):
    def __init__(self):
        self.engine = ...  # Should use composition, not inheritance

# GOOD: Using composition
class GoodCar:
    def __init__(self, engine):
        self.engine = engine  # Car HAS Engine (composition)
```

---

## 9. Design Patterns - Singleton, Factory, Observer, etc.

### What Are Design Patterns?

**Design patterns** are reusable solutions to common problems in software design. They're templates for solving problems that occur frequently in object-oriented programming.

### Why Are They Important?

- **Proven Solutions**: Address common design problems
- **Communication**: Provide shared vocabulary for developers
- **Best Practices**: Encapsulate best practices and design principles
- **Maintainability**: Make code more understandable and maintainable

### Singleton Pattern

Ensures a class has only one instance and provides global access to it.

```python
class DatabaseConnection:
    """
    Singleton pattern: Only one database connection instance.
    Real-world scenario: Database connections, configuration managers.
    """
    
    _instance = None
    
    def __new__(cls):
        """Control instance creation."""
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance._initialized = False
        return cls._instance
    
    def __init__(self):
        """Initialize only once."""
        if self._initialized:
            return
        self.connection_string = "database://localhost"
        self._initialized = True
    
    def connect(self):
        return f"Connected to {self.connection_string}"

# Usage
db1 = DatabaseConnection()
db2 = DatabaseConnection()

print(db1 is db2)  # Output: True (same instance)
print(db1.connect())  # Output: Connected to database://localhost
```

### Factory Pattern

Creates objects without specifying the exact class of object that will be created.

```python
class Vehicle:
    """Base vehicle class."""
    def start(self):
        pass

class Car(Vehicle):
    def start(self):
        return "Car engine started"

class Motorcycle(Vehicle):
    def start(self):
        return "Motorcycle engine started"

class Truck(Vehicle):
    def start(self):
        return "Truck engine started"

class VehicleFactory:
    """
    Factory pattern: Create vehicles without knowing exact class.
    Real-world scenario: Object creation based on configuration/user input.
    """
    
    @staticmethod
    def create_vehicle(vehicle_type):
        """Factory method: creates appropriate vehicle."""
        vehicles = {
            'car': Car,
            'motorcycle': Motorcycle,
            'truck': Truck
        }
        
        vehicle_class = vehicles.get(vehicle_type.lower())
        if vehicle_class:
            return vehicle_class()
        else:
            raise ValueError(f"Unknown vehicle type: {vehicle_type}")

# Usage
factory = VehicleFactory()
car = factory.create_vehicle('car')
motorcycle = factory.create_vehicle('motorcycle')

print(car.start())        # Output: Car engine started
print(motorcycle.start()) # Output: Motorcycle engine started
```

### Observer Pattern

Defines a one-to-many dependency between objects so that when one object changes state, all dependents are notified.

```python
class EventManager:
    """
    Subject: Notifies observers of events.
    Real-world scenario: Event systems, model-view architectures.
    """
    
    def __init__(self):
        self._subscribers = []
    
    def subscribe(self, subscriber):
        """Add an observer."""
        self._subscribers.append(subscriber)
    
    def unsubscribe(self, subscriber):
        """Remove an observer."""
        self._subscribers.remove(subscriber)
    
    def notify(self, event):
        """Notify all observers."""
        for subscriber in self._subscribers:
            subscriber.update(event)

class Subscriber:
    """Observer: Receives notifications."""
    
    def __init__(self, name):
        self.name = name
    
    def update(self, event):
        """Handle notification."""
        print(f"{self.name} received event: {event}")

# Usage
manager = EventManager()
subscriber1 = Subscriber("Alice")
subscriber2 = Subscriber("Bob")

manager.subscribe(subscriber1)
manager.subscribe(subscriber2)

manager.notify("User logged in")
# Output:
# Alice received event: User logged in
# Bob received event: User logged in
```

### Strategy Pattern

Defines a family of algorithms, encapsulates each one, and makes them interchangeable.

```python
class PaymentStrategy:
    """Strategy interface."""
    def pay(self, amount):
        pass

class CreditCardStrategy(PaymentStrategy):
    def __init__(self, card_number):
        self.card_number = card_number
    
    def pay(self, amount):
        return f"Paid ${amount} with credit card {self.card_number[-4:]}"

class PayPalStrategy(PaymentStrategy):
    def __init__(self, email):
        self.email = email
    
    def pay(self, amount):
        return f"Paid ${amount} via PayPal {self.email}"

class PaymentProcessor:
    """
    Context: Uses strategy pattern.
    Real-world scenario: Payment processing with multiple methods.
    """
    
    def __init__(self):
        self.strategy = None
    
    def set_strategy(self, strategy):
        """Set payment strategy."""
        self.strategy = strategy
    
    def process_payment(self, amount):
        """Process payment using current strategy."""
        if self.strategy is None:
            raise ValueError("No payment strategy set")
        return self.strategy.pay(amount)

# Usage
processor = PaymentProcessor()

processor.set_strategy(CreditCardStrategy("1234567890"))
print(processor.process_payment(100))
# Output: Paid $100 with credit card 7890

processor.set_strategy(PayPalStrategy("user@example.com"))
print(processor.process_payment(50))
# Output: Paid $50 via PayPal user@example.com
```

### Real-World Scenarios

1. **Singleton**: Database connections, logging systems, configuration managers
2. **Factory**: UI component creation, plugin systems, object serialization
3. **Observer**: Model-View-Controller (MVC), event-driven systems
4. **Strategy**: Sorting algorithms, compression methods, validation rules

### Common Pitfalls and Best Practices

**Pitfalls:**
- Overusing patterns when simple solutions suffice
- Implementing patterns incorrectly
- Creating overly complex solutions

**Best Practices:**
- Use patterns when they solve actual problems
- Understand the problem before applying a pattern
- Keep implementations simple and clear
- Document why a pattern is used

---

## 10. Advanced Topics - Metaclasses, Descriptors, Context Managers

### Metaclasses

Metaclasses are classes of classes. They control how classes are created.

```python
class SingletonMeta(type):
    """
    Metaclass for singleton pattern.
    Real-world scenario: Ensuring only one instance of critical classes.
    """
    
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    """Class using singleton metaclass."""
    pass

# Usage
db1 = Database()
db2 = Database()
print(db1 is db2)  # Output: True
```

### Descriptors

Descriptors control attribute access through `__get__`, `__set__`, and `__delete__` methods.

```python
class PositiveNumber:
    """
    Descriptor: Ensures values are positive.
    Real-world scenario: Data validation, type checking.
    """
    
    def __init__(self, name):
        self.name = name
    
    def __get__(self, obj, objtype=None):
        return obj.__dict__.get(self.name)
    
    def __set__(self, obj, value):
        if value < 0:
            raise ValueError(f"{self.name} must be positive")
        obj.__dict__[self.name] = value

class Rectangle:
    """Class using descriptor."""
    width = PositiveNumber('width')
    height = PositiveNumber('height')
    
    def __init__(self, width, height):
        self.width = width
        self.height = height

# Usage
rect = Rectangle(5, 4)
print(rect.width)  # Output: 5

# rect.width = -3  # Raises ValueError
```

### Context Managers

Context managers control entry and exit from a runtime context using `with` statements.

```python
class FileManager:
    """
    Context manager for file operations.
    Real-world scenario: Resource management, cleanup operations.
    """
    
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        """Enter runtime context."""
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        """Exit runtime context."""
        if self.file:
            self.file.close()
        return False  # Don't suppress exceptions

# Usage
with FileManager("test.txt", "w") as f:
    f.write("Hello, World!")
# File automatically closed
```

### Real-World Scenarios

1. **Metaclasses**: ORM frameworks, API generation, class registration
2. **Descriptors**: Property validation, lazy evaluation, method binding
3. **Context Managers**: File handling, database connections, locks

### Common Pitfalls and Best Practices

**Pitfalls:**
- Overusing metaclasses (they're complex and often unnecessary)
- Not properly handling exceptions in context managers
- Creating descriptors when properties would suffice

**Best Practices:**
- Use context managers for resource management
- Prefer simpler solutions over complex ones
- Document advanced features clearly
- Test thoroughly when using advanced features

---

## Conclusion

This guide has covered the fundamental and advanced concepts of Object-Oriented Programming in Python. Remember:

1. **Start Simple**: Master basics before moving to advanced topics
2. **Practice**: Write code, experiment, and learn from mistakes
3. **Read Code**: Study well-written Python libraries
4. **Think Design**: Consider how objects relate and interact
5. **Be Pythonic**: Embrace Python's philosophy (duck typing, simplicity)

Happy coding!
