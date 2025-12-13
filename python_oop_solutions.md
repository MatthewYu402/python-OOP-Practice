# Python Object-Oriented Programming - Solutions

This document contains solutions to all 100 exercises in the Python OOP Exercises notebook. Use this as a reference after attempting each exercise.

---

## Section 1: Basics (Easy) - Solutions 1-15

### Exercise 1: Create Your First Class

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def introduce(self):
        return f"Hi, I'm {self.name} and I'm {self.age} years old"
```

**Explanation:** This is the most basic class structure. `__init__` is the constructor that initializes instance attributes, and `introduce` is an instance method.

---

### Exercise 2: Add Instance Attributes

```python
class Person:
    def __init__(self, name, age, email):
        self.name = name
        self.age = age
        self.email = email
    
    def introduce(self):
        return f"Hi, I'm {self.name} and I'm {self.age} years old"
    
    def get_info(self):
        return {
            'name': self.name,
            'age': self.age,
            'email': self.email
        }
```

---

### Exercise 3: Bank Account Class

```python
class BankAccount:
    def __init__(self, account_number, balance, owner_name):
        self.account_number = account_number
        self.balance = balance
        self.owner_name = owner_name
    
    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
    
    def withdraw(self, amount):
        if amount > 0 and amount <= self.balance:
            self.balance -= amount
    
    def get_balance(self):
        return self.balance
```

**Explanation:** Methods modify instance state. The `withdraw` method includes validation to prevent negative balances.

---

### Exercise 4: Rectangle Class

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
    
    def is_square(self):
        return self.width == self.height
```

---

### Exercise 5: Student Class

```python
class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
        self.grades = []
    
    def add_grade(self, grade):
        self.grades.append(grade)
    
    def get_average(self):
        if not self.grades:
            return 0
        return sum(self.grades) / len(self.grades)
    
    def get_letter_grade(self):
        average = self.get_average()
        if average >= 90:
            return 'A'
        elif average >= 80:
            return 'B'
        elif average >= 70:
            return 'C'
        elif average >= 60:
            return 'D'
        else:
            return 'F'
```

---

### Exercise 6: Car Class

```python
from datetime import datetime

class Car:
    def __init__(self, make, model, year, mileage=0):
        self.make = make
        self.model = model
        self.year = year
        self.mileage = mileage
    
    def drive(self, miles):
        if miles > 0:
            self.mileage += miles
    
    def get_info(self):
        return f"{self.year} {self.make} {self.model} - {self.mileage} miles"
    
    def is_vintage(self):
        current_year = datetime.now().year
        return (current_year - self.year) > 25
```

---

### Exercise 7: Library Book Class

```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_checked_out = False
    
    def checkout(self):
        self.is_checked_out = True
    
    def return_book(self):
        self.is_checked_out = False
    
    def get_status(self):
        return "Checked out" if self.is_checked_out else "Available"
```

---

### Exercise 8: Calculator Class

```python
class Calculator:
    def __init__(self):
        self.history = []
    
    def add(self, a, b):
        result = a + b
        self.history.append(('add', a, b, result))
        return result
    
    def subtract(self, a, b):
        result = a - b
        self.history.append(('subtract', a, b, result))
        return result
    
    def multiply(self, a, b):
        result = a * b
        self.history.append(('multiply', a, b, result))
        return result
    
    def divide(self, a, b):
        if b == 0:
            raise ValueError("Cannot divide by zero")
        result = a / b
        self.history.append(('divide', a, b, result))
        return result
    
    def get_history(self):
        return self.history
```

---

### Exercise 9: Employee Class

```python
class Employee:
    def __init__(self, name, employee_id, department, salary):
        self.name = name
        self.employee_id = employee_id
        self.department = department
        self.salary = salary
    
    def give_raise(self, percentage):
        self.salary += self.salary * (percentage / 100)
    
    def get_yearly_salary(self):
        return self.salary
    
    def change_department(self, new_dept):
        self.department = new_dept
```

---

### Exercise 10: Shopping Cart Class

```python
class ShoppingCart:
    def __init__(self):
        self.items = {}
    
    def add_item(self, item, quantity):
        if item in self.items:
            self.items[item] += quantity
        else:
            self.items[item] = quantity
    
    def remove_item(self, item):
        if item in self.items:
            del self.items[item]
    
    def get_total_items(self):
        return sum(self.items.values())
    
    def clear_cart(self):
        self.items.clear()
```

---

### Exercise 11: Temperature Converter Class

```python
class Temperature:
    def __init__(self, celsius=0):
        self.celsius = celsius
    
    def to_fahrenheit(self):
        return (self.celsius * 9/5) + 32
    
    def to_kelvin(self):
        return self.celsius + 273.15
    
    def set_celsius(self, temp):
        self.celsius = temp
    
    def set_fahrenheit(self, temp):
        self.celsius = (temp - 32) * 5/9
```

**Formula:** 
- Fahrenheit: $F = \frac{9}{5}C + 32$
- Kelvin: $K = C + 273.15$

---

### Exercise 12: Password Manager Class

```python
class PasswordManager:
    def __init__(self):
        self.passwords = {}
    
    def add_password(self, service, password):
        if self.validate_password(password):
            self.passwords[service] = password
        else:
            raise ValueError("Password must be at least 8 characters")
    
    def get_password(self, service):
        return self.passwords.get(service, None)
    
    def list_services(self):
        return list(self.passwords.keys())
    
    def validate_password(self, password):
        return len(password) >= 8
```

---

### Exercise 13: Dice Class

```python
import random

class Dice:
    def __init__(self, sides=6):
        self.sides = sides
    
    def roll(self):
        return random.randint(1, self.sides)
    
    def roll_multiple(self, times):
        return [self.roll() for _ in range(times)]
```

---

### Exercise 14: Timer Class

```python
import time

class Timer:
    def __init__(self):
        self.start_time = None
        self.end_time = None
    
    def start(self):
        self.start_time = time.time()
    
    def stop(self):
        self.end_time = time.time()
    
    def elapsed_time(self):
        if self.start_time is None:
            return 0
        end = self.end_time if self.end_time else time.time()
        return end - self.start_time
```

---

### Exercise 15: Fraction Class

```python
import math

class Fraction:
    def __init__(self, numerator, denominator):
        if denominator == 0:
            raise ValueError("Denominator cannot be zero")
        self.numerator = numerator
        self.denominator = denominator
    
    def add(self, other):
        new_num = self.numerator * other.denominator + other.numerator * self.denominator
        new_den = self.denominator * other.denominator
        result = Fraction(new_num, new_den)
        result.simplify()
        return result
    
    def multiply(self, other):
        new_num = self.numerator * other.numerator
        new_den = self.denominator * other.denominator
        result = Fraction(new_num, new_den)
        result.simplify()
        return result
    
    def simplify(self):
        gcd = math.gcd(self.numerator, self.denominator)
        self.numerator //= gcd
        self.denominator //= gcd
```

---

## Section 2: Encapsulation (Easy-Medium) - Solutions 16-25

### Exercise 16: Private Attributes

```python
class BankAccount:
    def __init__(self, account_number, balance, owner_name):
        self.account_number = account_number
        self.__balance = balance  # Private attribute
        self.owner_name = owner_name
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
    
    def withdraw(self, amount):
        if amount > 0 and amount <= self.__balance:
            self.__balance -= amount
    
    def get_balance(self):
        return self.__balance
```

**Explanation:** In Python, attributes prefixed with `__` (double underscore) are name-mangled, making them harder to access directly from outside the class.

---

### Exercise 17: Property Decorator

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value
    
    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2
```

**Explanation:** `@property` allows you to access methods like attributes. The setter validates input before setting the value.

---

### Exercise 18: Temperature with Properties

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        self._celsius = value
    
    @property
    def fahrenheit(self):
        return (self._celsius * 9/5) + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        self._celsius = (value - 32) * 5/9
```

---

### Exercise 19: Student with Protected Attributes

```python
class Student:
    def __init__(self, name):
        if not name:
            raise ValueError("Name cannot be empty")
        self._name = name
        self._grades = []
    
    def get_name(self):
        return self._name
    
    def set_name(self, name):
        if not name:
            raise ValueError("Name cannot be empty")
        self._name = name
    
    def add_grade(self, grade):
        if 0 <= grade <= 100:
            self._grades.append(grade)
        else:
            raise ValueError("Grade must be between 0 and 100")
    
    def get_grades(self):
        return self._grades.copy()
```

**Explanation:** Single underscore `_` indicates protected attributes (convention, not enforced by Python).

---

### Exercise 20: Bank Account with Validation

```python
class BankAccount:
    def __init__(self, account_number, balance, owner_name):
        self.account_number = account_number
        self.__balance = max(0, balance)  # Ensure non-negative
        self.owner_name = owner_name
        self.__transaction_history = []
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            self.__transaction_history.append(('deposit', amount, self.__balance))
    
    def withdraw(self, amount):
        if amount > 0 and amount <= self.__balance:
            self.__balance -= amount
            self.__transaction_history.append(('withdraw', amount, self.__balance))
    
    def get_balance(self):
        return self.__balance
    
    def get_transaction_history(self):
        return self.__transaction_history.copy()
```

---

### Exercise 21: Email Validator Property

```python
class User:
    def __init__(self, email):
        self._email = None
        self.email = email  # Use setter for validation
    
    @property
    def email(self):
        return self._email
    
    @email.setter
    def email(self, value):
        if '@' not in value or '.' not in value.split('@')[1]:
            raise ValueError("Invalid email format")
        self._email = value
```

---

### Exercise 22: Age Property with Validation

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = None
        self.age = age  # Use setter
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, value):
        if not (0 <= value <= 150):
            raise ValueError("Age must be between 0 and 150")
        self._age = value
```

---

### Exercise 23: Read-Only Property

```python
class Product:
    def __init__(self, name, price, product_id):
        self.name = name
        self.price = price
        self.product_id = product_id
    
    @property
    def display_name(self):
        return f"ID: {self.product_id} - {self.name}"
    
    @display_name.setter
    def display_name(self, value):
        raise AttributeError("display_name is read-only")
```

---

### Exercise 24: Computed Property

```python
import math

class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    @property
    def area(self):
        return self.width * self.height
    
    @property
    def perimeter(self):
        return 2 * (self.width + self.height)
    
    @property
    def diagonal(self):
        return math.sqrt(self.width ** 2 + self.height ** 2)
```

**Formula:** Diagonal = $\sqrt{width^2 + height^2}$

---

### Exercise 25: Password Property

```python
class UserAccount:
    def __init__(self, username, password):
        self.username = username
        self._password = password
    
    @property
    def password(self):
        return "****"
    
    def verify_password(self, input_password):
        return self._password == input_password
```

---

## Section 3: Inheritance (Medium) - Solutions 26-35

### Exercise 26: Basic Inheritance

```python
class Vehicle:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
    
    def start_engine(self):
        return "Engine started"

class Car(Vehicle):
    def honk(self):
        return "Beep beep!"
```

**Explanation:** `Car(Vehicle)` indicates that `Car` inherits from `Vehicle`. All attributes and methods from `Vehicle` are available in `Car`.

---

### Exercise 27: Method Overriding

```python
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species
    
    def make_sound(self):
        return "Some generic sound"

class Dog(Animal):
    def make_sound(self):
        return "Woof!"

class Cat(Animal):
    def make_sound(self):
        return "Meow!"
```

---

### Exercise 28: Super() Usage

```python
class Employee:
    def __init__(self, name, employee_id, salary):
        self.name = name
        self.employee_id = employee_id
        self.salary = salary

class Manager(Employee):
    def __init__(self, name, employee_id, salary, department, team_size):
        super().__init__(name, employee_id, salary)
        self.department = department
        self.team_size = team_size
```

**Explanation:** `super()` calls the parent class's methods, allowing you to extend functionality rather than replace it.

---

### Exercise 29: Multiple Levels of Inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def make_sound(self):
        return "Some sound"

class Mammal(Animal):
    def __init__(self, name):
        super().__init__(name)
        self.warm_blooded = True

class Dog(Mammal):
    def __init__(self, name):
        super().__init__(name)
        self.species = "Canine"
    
    def make_sound(self):
        return "Woof!"

class Labrador(Dog):
    def __init__(self, name, color):
        super().__init__(name)
        self.color = color
```

---

### Exercise 30: Shape Hierarchy

```python
import math

class Shape:
    def __init__(self, color):
        self.color = color
    
    def area(self):
        raise NotImplementedError("Subclass must implement area()")

class Circle(Shape):
    def __init__(self, color, radius):
        super().__init__(color)
        self.radius = radius
    
    def area(self):
        return math.pi * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, color, width, height):
        super().__init__(color)
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Triangle(Shape):
    def __init__(self, color, base, height):
        super().__init__(color)
        self.base = base
        self.height = height
    
    def area(self):
        return 0.5 * self.base * self.height
```

---

### Exercise 31: Employee Hierarchy

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
    
    def calculate_pay(self):
        return self.salary

class FullTimeEmployee(Employee):
    def __init__(self, name, salary, benefits):
        super().__init__(name, salary)
        self.benefits = benefits
    
    def calculate_pay(self):
        return self.salary + self.benefits

class PartTimeEmployee(Employee):
    def __init__(self, name, hourly_rate, hours_per_week):
        super().__init__(name, 0)  # No base salary
        self.hourly_rate = hourly_rate
        self.hours_per_week = hours_per_week
    
    def calculate_pay(self):
        return self.hourly_rate * self.hours_per_week * 4  # Monthly
```

---

### Exercise 32: Bank Account Hierarchy

```python
class BankAccount:
    def __init__(self, account_number, balance):
        self.account_number = account_number
        self.balance = balance
    
    def deposit(self, amount):
        if amount > 0:
            self.balance += amount

class SavingsAccount(BankAccount):
    def __init__(self, account_number, balance, interest_rate):
        super().__init__(account_number, balance)
        self.interest_rate = interest_rate
    
    def add_interest(self):
        self.balance += self.balance * self.interest_rate

class CheckingAccount(BankAccount):
    def __init__(self, account_number, balance, transaction_limit):
        super().__init__(account_number, balance)
        self.transaction_limit = transaction_limit
        self.transaction_count = 0
    
    def deposit(self, amount):
        if self.transaction_count < self.transaction_limit:
            super().deposit(amount)
            self.transaction_count += 1
```

---

### Exercise 33: Media Hierarchy

```python
class Media:
    def __init__(self, title, duration):
        self.title = title
        self.duration = duration  # in seconds

class Song(Media):
    def __init__(self, title, duration, artist):
        super().__init__(title, duration)
        self.artist = artist

class Movie(Media):
    def __init__(self, title, duration, director):
        super().__init__(title, duration)
        self.director = director

class Podcast(Media):
    def __init__(self, title, duration, host):
        super().__init__(title, duration)
        self.host = host
```

---

### Exercise 34: Vehicle Hierarchy

```python
class Vehicle:
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
    
    def fuel_efficiency(self):
        return 0  # Base implementation

class Car(Vehicle):
    def fuel_efficiency(self):
        return 30  # miles per gallon

class Motorcycle(Vehicle):
    def fuel_efficiency(self):
        return 50  # miles per gallon

class Truck(Vehicle):
    def fuel_efficiency(self):
        return 15  # miles per gallon
```

---

### Exercise 35: Access Modifiers in Inheritance

```python
class BaseClass:
    def __init__(self):
        self.public_attr = "public"
        self._protected_attr = "protected"
        self.__private_attr = "private"
    
    def get_private(self):
        return self.__private_attr

class DerivedClass(BaseClass):
    def __init__(self):
        super().__init__()
    
    def access_test(self):
        print(self.public_attr)  # Works
        print(self._protected_attr)  # Works (convention)
        # print(self.__private_attr)  # Won't work - name mangled
        print(self.get_private())  # Works - through method
```

**Explanation:** 
- Public: Accessible everywhere
- Protected (`_`): Convention, accessible but indicates internal use
- Private (`__`): Name-mangled, harder to access from outside

---

## Section 4: Polymorphism (Medium) - Solutions 36-45

### Exercise 36: Duck Typing

```python
class Dog:
    def speak(self):
        return "Woof!"

class Cat:
    def speak(self):
        return "Meow!"

class Duck:
    def speak(self):
        return "Quack!"

def make_animal_speak(animal):
    return animal.speak()
```

**Explanation:** Duck typing - "If it walks like a duck and quacks like a duck, it's a duck." Python doesn't check types, just that the method exists.

---

### Exercise 37: Method Overriding

```python
class Payment:
    def process_payment(self, amount):
        raise NotImplementedError

class CreditCard(Payment):
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        return f"Processing ${amount} via Credit Card {self.card_number[-4:]}"

class PayPal(Payment):
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        return f"Processing ${amount} via PayPal ({self.email})"

class BankTransfer(Payment):
    def __init__(self, account_number):
        self.account_number = account_number
    
    def process_payment(self, amount):
        return f"Processing ${amount} via Bank Transfer to {self.account_number}"
```

---

### Exercise 38: Operator Overloading

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
```

---

### Exercise 39: Polymorphic List Processing

```python
class Square:
    def __init__(self, side):
        self.side = side
    
    def area(self):
        return self.side ** 2

class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        import math
        return math.pi * self.radius ** 2

class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

def calculate_total_area(shapes):
    return sum(shape.area() for shape in shapes)
```

---

### Exercise 40: File Handler Polymorphism

```python
class FileHandler:
    def read(self):
        raise NotImplementedError
    
    def write(self, data):
        raise NotImplementedError

class TextFileHandler(FileHandler):
    def __init__(self, filename):
        self.filename = filename
    
    def read(self):
        with open(self.filename, 'r') as f:
            return f.read()
    
    def write(self, data):
        with open(self.filename, 'w') as f:
            f.write(data)

class CSVFileHandler(FileHandler):
    def __init__(self, filename):
        self.filename = filename
    
    def read(self):
        import csv
        with open(self.filename, 'r') as f:
            return list(csv.reader(f))
    
    def write(self, data):
        import csv
        with open(self.filename, 'w', newline='') as f:
            writer = csv.writer(f)
            writer.writerows(data)

class JSONFileHandler(FileHandler):
    def __init__(self, filename):
        self.filename = filename
    
    def read(self):
        import json
        with open(self.filename, 'r') as f:
            return json.load(f)
    
    def write(self, data):
        import json
        with open(self.filename, 'w') as f:
            json.dump(data, f)

def process_file(handler):
    data = handler.read()
    # Process data...
    handler.write(data)
```

---

### Exercise 41: Payment Processor

```python
class PaymentProcessor:
    def process(self, payment_method, amount):
        return payment_method.process_payment(amount)

class CreditCard:
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        return f"Charged ${amount} to card {self.card_number[-4:]}"

class DebitCard:
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        return f"Debited ${amount} from card {self.card_number[-4:]}"

class CryptoWallet:
    def __init__(self, wallet_address):
        self.wallet_address = wallet_address
    
    def process_payment(self, amount):
        return f"Sent {amount} crypto from {self.wallet_address[:10]}..."
```

---

### Exercise 42: Shape Renderer

```python
class Renderer:
    def render(self, shape):
        shape.draw()

class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def draw(self):
        print(f"Drawing circle with radius {self.radius}")

class Square:
    def __init__(self, side):
        self.side = side
    
    def draw(self):
        print(f"Drawing square with side {self.side}")

class Triangle:
    def __init__(self, base, height):
        self.base = base
        self.height = height
    
    def draw(self):
        print(f"Drawing triangle with base {self.base} and height {self.height}")
```

---

### Exercise 43: Database Connection Polymorphism

```python
class DatabaseConnection:
    def connect(self):
        raise NotImplementedError
    
    def query(self, sql):
        raise NotImplementedError

class MySQLConnection(DatabaseConnection):
    def connect(self):
        return "Connected to MySQL"
    
    def query(self, sql):
        return f"MySQL: Executing {sql}"

class PostgreSQLConnection(DatabaseConnection):
    def connect(self):
        return "Connected to PostgreSQL"
    
    def query(self, sql):
        return f"PostgreSQL: Executing {sql}"

class SQLiteConnection(DatabaseConnection):
    def connect(self):
        return "Connected to SQLite"
    
    def query(self, sql):
        return f"SQLite: Executing {sql}"

def execute_query(connection, sql):
    connection.connect()
    return connection.query(sql)
```

---

### Exercise 44: Notification System

```python
class Notification:
    def send(self, message):
        raise NotImplementedError

class EmailNotification(Notification):
    def __init__(self, email):
        self.email = email
    
    def send(self, message):
        return f"Email sent to {self.email}: {message}"

class SMSNotification(Notification):
    def __init__(self, phone):
        self.phone = phone
    
    def send(self, message):
        return f"SMS sent to {self.phone}: {message}"

class PushNotification(Notification):
    def __init__(self, device_id):
        self.device_id = device_id
    
    def send(self, message):
        return f"Push notification sent to {self.device_id}: {message}"

class NotificationService:
    def send(self, notification, message):
        return notification.send(message)
```

---

### Exercise 45: Calculator Operations

```python
class Operation:
    def execute(self, a, b):
        raise NotImplementedError

class AddOperation(Operation):
    def execute(self, a, b):
        return a + b

class SubtractOperation(Operation):
    def execute(self, a, b):
        return a - b

class MultiplyOperation(Operation):
    def execute(self, a, b):
        return a * b

class DivideOperation(Operation):
    def execute(self, a, b):
        if b == 0:
            raise ValueError("Cannot divide by zero")
        return a / b

class Calculator:
    def calculate(self, operation, a, b):
        return operation.execute(a, b)
```

---

## Section 5: Special Methods (Medium) - Solutions 46-55

### Exercise 46: __str__ and __repr__

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Point({self.x}, {self.y})"
```

**Explanation:** `__str__` is for user-friendly representation, `__repr__` is for developer/debugging representation (ideally should be valid Python code).

---

### Exercise 47: Comparison Methods

```python
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade
    
    def __eq__(self, other):
        return self.grade == other.grade
    
    def __lt__(self, other):
        return self.grade < other.grade
    
    def __le__(self, other):
        return self.grade <= other.grade
    
    def __gt__(self, other):
        return self.grade > other.grade
    
    def __ge__(self, other):
        return self.grade >= other.grade
```

---

### Exercise 48: __len__ Method

```python
class Playlist:
    def __init__(self):
        self.songs = []
    
    def add_song(self, song):
        self.songs.append(song)
    
    def __len__(self):
        return len(self.songs)
```

---

### Exercise 49: __getitem__ and __setitem__

```python
class Deck:
    def __init__(self):
        self.cards = ['Ace', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'Jack', 'Queen', 'King']
    
    def __getitem__(self, index):
        return self.cards[index]
    
    def __setitem__(self, index, value):
        self.cards[index] = value
```

---

### Exercise 50: __iter__ and __next__

```python
class Deck:
    def __init__(self):
        self.cards = ['Ace', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'Jack', 'Queen', 'King']
        self.index = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.index >= len(self.cards):
            raise StopIteration
        card = self.cards[self.index]
        self.index += 1
        return card
```

---

### Exercise 51: __call__ Method

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor
    
    def __call__(self, number):
        return self.factor * number
```

**Explanation:** `__call__` makes instances callable like functions.

---

### Exercise 52: __contains__ Method

```python
class Range:
    def __init__(self, start, end):
        self.start = start
        self.end = end
    
    def __contains__(self, item):
        return self.start <= item <= self.end
```

---

### Exercise 53: __add__ and __iadd__

```python
class Money:
    def __init__(self, amount, currency):
        self.amount = amount
        self.currency = currency
    
    def __add__(self, other):
        if self.currency != other.currency:
            raise ValueError("Cannot add different currencies")
        return Money(self.amount + other.amount, self.currency)
    
    def __iadd__(self, other):
        if self.currency != other.currency:
            raise ValueError("Cannot add different currencies")
        self.amount += other.amount
        return self
    
    def __str__(self):
        return f"{self.amount} {self.currency}"
```

---

### Exercise 54: __bool__ Method

```python
class Container:
    def __init__(self):
        self.items = []
    
    def add_item(self, item):
        self.items.append(item)
    
    def __bool__(self):
        return len(self.items) > 0
```

---

### Exercise 55: Complete Special Methods

```python
import math

class Fraction:
    def __init__(self, numerator, denominator):
        if denominator == 0:
            raise ValueError("Denominator cannot be zero")
        self.numerator = numerator
        self.denominator = denominator
        self.simplify()
    
    def simplify(self):
        gcd = math.gcd(self.numerator, self.denominator)
        self.numerator //= gcd
        self.denominator //= gcd
    
    def __str__(self):
        return f"{self.numerator}/{self.denominator}"
    
    def __repr__(self):
        return f"Fraction({self.numerator}, {self.denominator})"
    
    def __add__(self, other):
        new_num = self.numerator * other.denominator + other.numerator * self.denominator
        new_den = self.denominator * other.denominator
        return Fraction(new_num, new_den)
    
    def __sub__(self, other):
        new_num = self.numerator * other.denominator - other.numerator * self.denominator
        new_den = self.denominator * other.denominator
        return Fraction(new_num, new_den)
    
    def __mul__(self, other):
        return Fraction(self.numerator * other.numerator, self.denominator * other.denominator)
    
    def __eq__(self, other):
        return self.numerator == other.numerator and self.denominator == other.denominator
    
    def __lt__(self, other):
        return self.numerator * other.denominator < other.numerator * self.denominator
```

---

## Section 6: Class & Static Methods (Medium) - Solutions 56-65

### Exercise 56: Class Method - Alternative Constructor

```python
class Date:
    def __init__(self, day, month, year):
        self.day = day
        self.month = month
        self.year = year
    
    @classmethod
    def from_string(cls, date_string):
        year, month, day = map(int, date_string.split('-'))
        return cls(day, month, year)
```

**Explanation:** `@classmethod` receives the class as the first argument (`cls`), allowing alternative constructors.

---

### Exercise 57: Class Method - Factory Pattern

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    @classmethod
    def create_child(cls, name):
        return cls(name, 5)  # Default age for child
    
    @classmethod
    def create_adult(cls, name):
        return cls(name, 25)  # Default age for adult
    
    @classmethod
    def create_senior(cls, name):
        return cls(name, 70)  # Default age for senior
```

---

### Exercise 58: Static Method - Utility Functions

```python
class MathUtils:
    @staticmethod
    def is_prime(n):
        if n < 2:
            return False
        for i in range(2, int(n ** 0.5) + 1):
            if n % i == 0:
                return False
        return True
    
    @staticmethod
    def factorial(n):
        if n < 0:
            raise ValueError("Factorial not defined for negative numbers")
        if n == 0:
            return 1
        result = 1
        for i in range(1, n + 1):
            result *= i
        return result
    
    @staticmethod
    def gcd(a, b):
        import math
        return math.gcd(a, b)
```

**Explanation:** `@staticmethod` doesn't receive `self` or `cls` - it's just a function inside the class namespace.

---

### Exercise 59: Class Variable and Class Method

```python
class Car:
    total_cars = 0
    
    def __init__(self, make, model):
        self.make = make
        self.model = model
        Car.total_cars += 1
    
    @classmethod
    def get_total_cars(cls):
        return cls.total_cars
```

---

### Exercise 60: Class Method for Configuration

```python
class DatabaseConnection:
    def __init__(self, host, port, database):
        self.host = host
        self.port = port
        self.database = database
    
    @classmethod
    def from_config(cls, config_dict):
        return cls(
            host=config_dict['host'],
            port=config_dict['port'],
            database=config_dict['database']
        )
```

---

### Exercise 61: Static Method vs Instance Method

```python
class StringProcessor:
    def __init__(self, prefix):
        self.prefix = prefix
    
    def process(self, text):
        return f"{self.prefix}{text}"  # Uses instance attribute
    
    @staticmethod
    def is_valid(text):
        return isinstance(text, str) and len(text) > 0  # No instance needed
```

---

### Exercise 62: Class Method Chain

```python
class QueryBuilder:
    def __init__(self):
        self.select_fields = None
        self.from_table_name = None
        self.where_condition = None
    
    @classmethod
    def select(cls, fields):
        instance = cls()
        instance.select_fields = fields
        return instance
    
    def from_table(self, table):
        self.from_table_name = table
        return self
    
    def where(self, condition):
        self.where_condition = condition
        return self
    
    def build(self):
        query = f"SELECT {self.select_fields} FROM {self.from_table_name}"
        if self.where_condition:
            query += f" WHERE {self.where_condition}"
        return query
```

---

### Exercise 63: Static Method for Validation

```python
import re

class User:
    def __init__(self, username, email):
        if not User.validate_username(username):
            raise ValueError("Invalid username")
        if not User.validate_email(email):
            raise ValueError("Invalid email")
        self.username = username
        self.email = email
    
    @staticmethod
    def validate_username(username):
        return len(username) >= 3 and username.isalnum()
    
    @staticmethod
    def validate_email(email):
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return re.match(pattern, email) is not None
```

---

### Exercise 64: Class Method Counter

```python
class Book:
    total_books = 0
    total_pages = 0
    
    def __init__(self, title, pages):
        self.title = title
        self.pages = pages
        Book.total_books += 1
        Book.total_pages += pages
    
    @classmethod
    def get_total_books(cls):
        return cls.total_books
    
    @classmethod
    def get_average_pages(cls):
        if cls.total_books == 0:
            return 0
        return cls.total_pages / cls.total_books
```

---

### Exercise 65: Factory with Class Methods

```python
import math

class Shape:
    def area(self):
        raise NotImplementedError
    
    @classmethod
    def create_circle(cls, radius):
        return Circle(radius)
    
    @classmethod
    def create_rectangle(cls, width, height):
        return Rectangle(width, height)
    
    @classmethod
    def create_square(cls, side):
        return Square(side)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return math.pi * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)
```

---

## Section 7: Abstract Classes (Medium-Hard) - Solutions 66-75

### Exercise 66: ABC Base Class

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    def __init__(self, name):
        self.name = name
    
    @abstractmethod
    def make_sound(self):
        pass
    
    @abstractmethod
    def move(self):
        pass

class Dog(Animal):
    def make_sound(self):
        return "Woof!"
    
    def move(self):
        return "Running on four legs"

class Bird(Animal):
    def make_sound(self):
        return "Tweet!"
    
    def move(self):
        return "Flying"
```

**Explanation:** `ABC` (Abstract Base Class) prevents instantiation of incomplete classes. `@abstractmethod` requires subclasses to implement the method.

---

### Exercise 67: Abstract Properties

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @property
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    @property
    def area(self):
        import math
        return math.pi * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    @property
    def area(self):
        return self.width * self.height
```

---

### Exercise 68: Abstract Class with Concrete Methods

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    def __init__(self, make, model):
        self.make = make
        self.model = model
    
    @abstractmethod
    def start_engine(self):
        pass
    
    def get_info(self):
        return f"{self.make} {self.model} - Engine: {self.start_engine()}"

class Car(Vehicle):
    def start_engine(self):
        return "Vroom!"

class Motorcycle(Vehicle):
    def start_engine(self):
        return "Vroom vroom!"
```

---

### Exercise 69: Multiple Abstract Methods

```python
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def connect(self):
        pass
    
    @abstractmethod
    def disconnect(self):
        pass
    
    @abstractmethod
    def execute_query(self, query):
        pass
    
    @abstractmethod
    def commit(self):
        pass

class MySQLDatabase(Database):
    def connect(self):
        return "Connected to MySQL"
    
    def disconnect(self):
        return "Disconnected from MySQL"
    
    def execute_query(self, query):
        return f"MySQL: {query}"
    
    def commit(self):
        return "MySQL: Committed"

class PostgreSQLDatabase(Database):
    def connect(self):
        return "Connected to PostgreSQL"
    
    def disconnect(self):
        return "Disconnected from PostgreSQL"
    
    def execute_query(self, query):
        return f"PostgreSQL: {query}"
    
    def commit(self):
        return "PostgreSQL: Committed"
```

---

### Exercise 70: Abstract Class Hierarchy

```python
from abc import ABC, abstractmethod

class Employee(ABC):
    def __init__(self, name):
        self.name = name
    
    @abstractmethod
    def calculate_salary(self):
        pass

class FullTimeEmployee(Employee):
    def __init__(self, name, salary):
        super().__init__(name)
        self.salary = salary
    
    def calculate_salary(self):
        return self.salary

class PartTimeEmployee(Employee):
    def __init__(self, name, hourly_rate, hours):
        super().__init__(name)
        self.hourly_rate = hourly_rate
        self.hours = hours
    
    def calculate_salary(self):
        return self.hourly_rate * self.hours
```

---

### Exercise 71: Interface Pattern

```python
from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass
    
    @abstractmethod
    def refund(self, transaction_id):
        pass

class CreditCard(PaymentMethod):
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        return f"Processed ${amount} via Credit Card"
    
    def refund(self, transaction_id):
        return f"Refunded transaction {transaction_id} via Credit Card"

class PayPal(PaymentMethod):
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        return f"Processed ${amount} via PayPal"
    
    def refund(self, transaction_id):
        return f"Refunded transaction {transaction_id} via PayPal"
```

---

### Exercise 72: Abstract Factory Pattern

```python
from abc import ABC, abstractmethod

class Button(ABC):
    @abstractmethod
    def render(self):
        pass

class Dialog(ABC):
    @abstractmethod
    def render(self):
        pass

class WindowsButton(Button):
    def render(self):
        return "Windows Button"

class MacButton(Button):
    def render(self):
        return "Mac Button"

class WindowsDialog(Dialog):
    def render(self):
        return "Windows Dialog"

class MacDialog(Dialog):
    def render(self):
        return "Mac Dialog"

class UIFactory(ABC):
    @abstractmethod
    def create_button(self):
        pass
    
    @abstractmethod
    def create_dialog(self):
        pass

class WindowsFactory(UIFactory):
    def create_button(self):
        return WindowsButton()
    
    def create_dialog(self):
        return WindowsDialog()

class MacFactory(UIFactory):
    def create_button(self):
        return MacButton()
    
    def create_dialog(self):
        return MacDialog()
```

---

### Exercise 73: Template Method Pattern

```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):
    def run(self):
        data = self.load_data()
        processed = self.process_data(data)
        self.save_data(processed)
    
    @abstractmethod
    def load_data(self):
        pass
    
    @abstractmethod
    def process_data(self, data):
        pass
    
    @abstractmethod
    def save_data(self, data):
        pass

class CSVProcessor(DataProcessor):
    def load_data(self):
        return "CSV data loaded"
    
    def process_data(self, data):
        return f"Processed {data}"
    
    def save_data(self, data):
        return f"Saved {data} to CSV"
```

---

### Exercise 74: Abstract Validator

```python
from abc import ABC, abstractmethod
import re

class Validator(ABC):
    @abstractmethod
    def validate(self, value):
        pass

class EmailValidator(Validator):
    def validate(self, value):
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return bool(re.match(pattern, value))

class PhoneValidator(Validator):
    def validate(self, value):
        pattern = r'^\d{3}-\d{3}-\d{4}$'
        return bool(re.match(pattern, value))

class AgeValidator(Validator):
    def validate(self, value):
        return isinstance(value, int) and 0 <= value <= 150
```

---

### Exercise 75: Abstract Repository Pattern

```python
from abc import ABC, abstractmethod

class Repository(ABC):
    @abstractmethod
    def get(self, id):
        pass
    
    @abstractmethod
    def get_all(self):
        pass
    
    @abstractmethod
    def save(self, entity):
        pass
    
    @abstractmethod
    def delete(self, id):
        pass

class User:
    def __init__(self, name, email):
        self.id = None
        self.name = name
        self.email = email

class UserRepository(Repository):
    def __init__(self):
        self.users = {}
        self.next_id = 1
    
    def get(self, id):
        return self.users.get(id)
    
    def get_all(self):
        return list(self.users.values())
    
    def save(self, entity):
        if entity.id is None:
            entity.id = self.next_id
            self.next_id += 1
        self.users[entity.id] = entity
    
    def delete(self, id):
        if id in self.users:
            del self.users[id]

class ProductRepository(Repository):
    def __init__(self):
        self.products = {}
        self.next_id = 1
    
    def get(self, id):
        return self.products.get(id)
    
    def get_all(self):
        return list(self.products.values())
    
    def save(self, entity):
        if entity.id is None:
            entity.id = self.next_id
            self.next_id += 1
        self.products[entity.id] = entity
    
    def delete(self, id):
        if id in self.products:
            del self.products[id]
```

---

## Section 8: Composition (Medium-Hard) - Solutions 76-85

### Exercise 76: Has-a Relationship

```python
class Engine:
    def __init__(self, engine_type):
        self.engine_type = engine_type
    
    def start(self):
        return f"{self.engine_type} engine started"
    
    def stop(self):
        return f"{self.engine_type} engine stopped"

class Car:
    def __init__(self, make, engine):
        self.make = make
        self.engine = engine  # Composition: Car HAS-A Engine
    
    def start(self):
        return self.engine.start()  # Delegation
    
    def stop(self):
        return self.engine.stop()  # Delegation
```

**Explanation:** Composition means "has-a" relationship. Car doesn't inherit from Engine, it contains an Engine object.

---

### Exercise 77: Composition vs Inheritance

```python
class Brain:
    def think(self):
        return "Thinking..."

class Person:
    def __init__(self, name):
        self.name = name
        self.brain = Brain()  # Composition
    
    def think(self):
        return self.brain.think()

class School:
    def __init__(self, name):
        self.name = name

class Student:
    def __init__(self, person, school):
        self.person = person  # Composition
        self.school = school  # Composition
```

---

### Exercise 78: Library System

```python
class Book:
    def __init__(self, title, isbn):
        self.title = title
        self.isbn = isbn

class Library:
    def __init__(self):
        self.books = []  # Composition: Library contains Books
    
    def add_book(self, book):
        self.books.append(book)
    
    def remove_book(self, isbn):
        self.books = [b for b in self.books if b.isbn != isbn]
    
    def find_book(self, title):
        for book in self.books:
            if book.title == title:
                return book
        return None
```

---

### Exercise 79: Computer System Composition

```python
class CPU:
    def __init__(self, model, speed):
        self.model = model
        self.speed = speed
    
    def get_specs(self):
        return f"CPU: {self.model} @ {self.speed}GHz"

class RAM:
    def __init__(self, size, type):
        self.size = size
        self.type = type
    
    def get_specs(self):
        return f"RAM: {self.size}GB {self.type}"

class Storage:
    def __init__(self, capacity, type):
        self.capacity = capacity
        self.type = type
    
    def get_specs(self):
        return f"Storage: {self.capacity}GB {self.type}"

class Computer:
    def __init__(self, cpu, ram, storage):
        self.cpu = cpu  # Composition
        self.ram = ram  # Composition
        self.storage = storage  # Composition
    
    def get_specs(self):
        return [
            self.cpu.get_specs(),
            self.ram.get_specs(),
            self.storage.get_specs()
        ]
```

---

### Exercise 80: University Composition

```python
class Department:
    def __init__(self, name):
        self.name = name

class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id

class Professor:
    def __init__(self, name, employee_id):
        self.name = name
        self.employee_id = employee_id

class University:
    def __init__(self, name):
        self.name = name
        self.departments = []  # Composition
        self.students = []  # Composition
        self.professors = []  # Composition
    
    def add_department(self, department):
        self.departments.append(department)
    
    def add_student(self, student):
        self.students.append(student)
    
    def add_professor(self, professor):
        self.professors.append(professor)
    
    def get_total_students(self):
        return len(self.students)
```

---

### Exercise 81: Shopping Cart Composition

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

class CartItem:
    def __init__(self, product, quantity):
        self.product = product  # Composition
        self.quantity = quantity
    
    def get_total_price(self):
        return self.product.price * self.quantity

class ShoppingCart:
    def __init__(self):
        self.items = []  # Composition: Cart contains CartItems
    
    def add_item(self, cart_item):
        self.items.append(cart_item)
    
    def get_total_price(self):
        return sum(item.get_total_price() for item in self.items)
```

---

### Exercise 82: House Composition

```python
class Address:
    def __init__(self, street):
        self.street = street

class Room:
    def __init__(self, name, area):
        self.name = name
        self.area = area

class House:
    def __init__(self, address):
        self.address = address  # Composition
        self.rooms = []  # Composition
    
    def add_room(self, room):
        self.rooms.append(room)
    
    def remove_room(self, room_name):
        self.rooms = [r for r in self.rooms if r.name != room_name]
    
    def get_total_area(self):
        return sum(room.area for room in self.rooms)
```

---

### Exercise 83: Playlist Composition

```python
class Song:
    def __init__(self, title, artist, duration):
        self.title = title
        self.artist = artist
        self.duration = duration

class Playlist:
    def __init__(self, name):
        self.name = name
        self.songs = []  # Composition
    
    def add_song(self, song):
        self.songs.append(song)
    
    def remove_song(self, title):
        self.songs = [s for s in self.songs if s.title != title]
    
    def total_duration(self):
        return sum(song.duration for song in self.songs)
    
    def shuffle(self):
        import random
        random.shuffle(self.songs)
```

---

### Exercise 84: Company Structure

```python
class Employee:
    def __init__(self, name, role):
        self.name = name
        self.role = role

class Department:
    def __init__(self, name):
        self.name = name
        self.employees = []  # Composition
    
    def add_employee(self, employee):
        self.employees.append(employee)
    
    def get_employee_count(self):
        return len(self.employees)

class Company:
    def __init__(self, name):
        self.name = name
        self.ceo = None  # Composition
        self.departments = []  # Composition
    
    def set_ceo(self, employee):
        self.ceo = employee
    
    def add_department(self, department):
        self.departments.append(department)
    
    def get_total_employees(self):
        total = 1 if self.ceo else 0  # Count CEO
        total += sum(dept.get_employee_count() for dept in self.departments)
        return total
```

---

### Exercise 85: Game Character Composition

```python
class Weapon:
    def __init__(self, name, attack_power):
        self.name = name
        self.attack_power = attack_power

class Armor:
    def __init__(self, name, defense_power):
        self.name = name
        self.defense_power = defense_power

class Character:
    def __init__(self, name):
        self.name = name
        self.weapon = None  # Composition
        self.armor = None  # Composition
        self.inventory = []  # Composition
        self.base_attack = 10
        self.base_defense = 5
    
    def equip_weapon(self, weapon):
        self.weapon = weapon
    
    def equip_armor(self, armor):
        self.armor = armor
    
    def add_to_inventory(self, item):
        self.inventory.append(item)
    
    def get_total_attack(self):
        weapon_bonus = self.weapon.attack_power if self.weapon else 0
        return self.base_attack + weapon_bonus
    
    def get_total_defense(self):
        armor_bonus = self.armor.defense_power if self.armor else 0
        return self.base_defense + armor_bonus
```

---

## Section 9: Design Patterns (Hard) - Solutions 86-95

### Exercise 86: Singleton Pattern

```python
class DatabaseConnection:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.initialized = False
        return cls._instance
    
    def __init__(self):
        if not self.initialized:
            self.connection_string = "database://localhost"
            self.initialized = True
```

**Explanation:** `__new__` controls object creation. By checking `_instance`, we ensure only one instance exists.

---

### Exercise 87: Factory Pattern

```python
class Vehicle:
    def start(self):
        pass

class Car(Vehicle):
    def start(self):
        return "Car started"

class Motorcycle(Vehicle):
    def start(self):
        return "Motorcycle started"

class Truck(Vehicle):
    def start(self):
        return "Truck started"

class VehicleFactory:
    @staticmethod
    def create_vehicle(vehicle_type):
        vehicles = {
            "car": Car,
            "motorcycle": Motorcycle,
            "truck": Truck
        }
        vehicle_class = vehicles.get(vehicle_type.lower())
        if vehicle_class:
            return vehicle_class()
        raise ValueError(f"Unknown vehicle type: {vehicle_type}")
```

---

### Exercise 88: Observer Pattern

```python
class Subscriber:
    def __init__(self, name):
        self.name = name
    
    def notify(self, event):
        print(f"{self.name} received: {event}")

class EventManager:
    def __init__(self):
        self.subscribers = []
    
    def subscribe(self, subscriber):
        self.subscribers.append(subscriber)
    
    def unsubscribe(self, subscriber):
        if subscriber in self.subscribers:
            self.subscribers.remove(subscriber)
    
    def notify(self, event):
        for subscriber in self.subscribers:
            subscriber.notify(event)
```

---

### Exercise 89: Strategy Pattern

```python
class PaymentStrategy:
    def process_payment(self, amount):
        raise NotImplementedError

class CreditCardStrategy(PaymentStrategy):
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        return f"Processed ${amount} via Credit Card {self.card_number[-4:]}"

class PayPalStrategy(PaymentStrategy):
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        return f"Processed ${amount} via PayPal ({self.email})"

class CryptoStrategy(PaymentStrategy):
    def __init__(self, wallet_address):
        self.wallet_address = wallet_address
    
    def process_payment(self, amount):
        return f"Processed ${amount} via Crypto ({self.wallet_address[:10]}...)"

class PaymentProcessor:
    def __init__(self):
        self.strategy = None
    
    def set_strategy(self, strategy):
        self.strategy = strategy
    
    def process_payment(self, amount):
        if self.strategy:
            return self.strategy.process_payment(amount)
        raise ValueError("No payment strategy set")
```

---

### Exercise 90: Decorator Pattern

```python
class Coffee:
    def get_cost(self):
        return 2.0
    
    def get_description(self):
        return "Coffee"

class CoffeeDecorator(Coffee):
    def __init__(self, coffee):
        self.coffee = coffee
    
    def get_cost(self):
        return self.coffee.get_cost()
    
    def get_description(self):
        return self.coffee.get_description()

class Milk(CoffeeDecorator):
    def get_cost(self):
        return self.coffee.get_cost() + 0.5
    
    def get_description(self):
        return self.coffee.get_description() + ", Milk"

class Sugar(CoffeeDecorator):
    def get_cost(self):
        return self.coffee.get_cost() + 0.2
    
    def get_description(self):
        return self.coffee.get_description() + ", Sugar"

class WhippedCream(CoffeeDecorator):
    def get_cost(self):
        return self.coffee.get_cost() + 0.7
    
    def get_description(self):
        return self.coffee.get_description() + ", Whipped Cream"
```

---

### Exercise 91: Builder Pattern

```python
class Pizza:
    def __init__(self):
        self.toppings = []
    
    def add_topping(self, topping):
        self.toppings.append(topping)
    
    def get_toppings(self):
        return self.toppings

class PizzaBuilder:
    def __init__(self):
        self.pizza = Pizza()
    
    def add_cheese(self):
        self.pizza.add_topping("Cheese")
        return self
    
    def add_pepperoni(self):
        self.pizza.add_topping("Pepperoni")
        return self
    
    def add_mushrooms(self):
        self.pizza.add_topping("Mushrooms")
        return self
    
    def build(self):
        return self.pizza
```

---

### Exercise 92: Adapter Pattern

```python
class LegacySystem:
    def old_method(self):
        return "Legacy system operation"

class ModernInterface:
    def new_method(self):
        raise NotImplementedError

class Adapter(ModernInterface):
    def __init__(self, legacy_system):
        self.legacy_system = legacy_system
    
    def new_method(self):
        return self.legacy_system.old_method()
```

---

### Exercise 93: Command Pattern

```python
class Command:
    def execute(self):
        raise NotImplementedError
    
    def undo(self):
        raise NotImplementedError

class AddCommand(Command):
    def __init__(self, value):
        self.value = value
    
    def execute(self, target):
        target.value += self.value
        return target.value
    
    def undo(self, target):
        target.value -= self.value
        return target.value

class DeleteCommand(Command):
    def __init__(self, item):
        self.item = item
        self.index = None
    
    def execute(self, target):
        if self.item in target.items:
            self.index = target.items.index(self.item)
            target.items.remove(self.item)
    
    def undo(self, target):
        if self.index is not None:
            target.items.insert(self.index, self.item)

class Invoker:
    def __init__(self):
        self.history = []
    
    def execute(self, command, target):
        result = command.execute(target)
        self.history.append((command, target))
        return result
    
    def undo(self):
        if self.history:
            command, target = self.history.pop()
            command.undo(target)
```

---

### Exercise 94: State Pattern

```python
class TrafficLightState:
    def next_state(self, light):
        raise NotImplementedError
    
    def get_state_name(self):
        raise NotImplementedError

class RedState(TrafficLightState):
    def next_state(self, light):
        light.state = GreenState()
    
    def get_state_name(self):
        return "Red"

class YellowState(TrafficLightState):
    def next_state(self, light):
        light.state = RedState()
    
    def get_state_name(self):
        return "Yellow"

class GreenState(TrafficLightState):
    def next_state(self, light):
        light.state = YellowState()
    
    def get_state_name(self):
        return "Green"

class TrafficLight:
    def __init__(self):
        self.state = RedState()
    
    def next_state(self):
        self.state.next_state(self)
    
    def get_state(self):
        return self.state.get_state_name()
```

---

### Exercise 95: Facade Pattern

```python
class Amplifier:
    def on(self):
        return "Amplifier on"
    
    def off(self):
        return "Amplifier off"

class DVDPlayer:
    def play(self, movie):
        return f"Playing {movie}"
    
    def stop(self):
        return "DVD stopped"

class Projector:
    def on(self):
        return "Projector on"
    
    def off(self):
        return "Projector off"

class Lights:
    def dim(self):
        return "Lights dimmed"
    
    def bright(self):
        return "Lights bright"

class HomeTheaterFacade:
    def __init__(self):
        self.amplifier = Amplifier()
        self.dvd = DVDPlayer()
        self.projector = Projector()
        self.lights = Lights()
    
    def watch_movie(self, movie):
        results = []
        results.append(self.lights.dim())
        results.append(self.amplifier.on())
        results.append(self.projector.on())
        results.append(self.dvd.play(movie))
        return results
    
    def end_movie(self):
        results = []
        results.append(self.dvd.stop())
        results.append(self.projector.off())
        results.append(self.amplifier.off())
        results.append(self.lights.bright())
        return results
```

---

## Section 10: Advanced Topics (Hard) - Solutions 96-100

### Exercise 96: Context Manager (__enter__ and __exit__)

```python
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        return False  # Don't suppress exceptions
```

**Usage:**
```python
with FileManager("test.txt", "w") as f:
    f.write("Hello, World!")
# File automatically closed
```

---

### Exercise 97: Descriptor Protocol

```python
class PositiveNumber:
    def __init__(self):
        self.name = None
    
    def __set_name__(self, owner, name):
        self.name = f"_{name}"
    
    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return getattr(obj, self.name, 0)
    
    def __set__(self, obj, value):
        if value < 0:
            raise ValueError("Value must be positive")
        setattr(obj, self.name, value)

class Rectangle:
    width = PositiveNumber()
    height = PositiveNumber()
    
    def __init__(self, width, height):
        self.width = width
        self.height = height
```

**Explanation:** Descriptors control attribute access. `__get__` and `__set__` are called when accessing the attribute.

---

### Exercise 98: Metaclass

```python
class SingletonMeta(type):
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class SingletonClass(metaclass=SingletonMeta):
    def __init__(self):
        self.value = None
```

**Explanation:** Metaclasses control class creation. `__call__` in the metaclass controls instance creation.

---

### Exercise 99: Property Descriptor

```python
class CachedProperty:
    def __init__(self, func):
        self.func = func
        self.cache_name = f"_cached_{func.__name__}"
    
    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        
        # Check if cache exists and is valid
        if hasattr(obj, self.cache_name):
            return getattr(obj, self.cache_name)
        
        # Compute and cache
        value = self.func(obj)
        setattr(obj, self.cache_name, value)
        return value
    
    def __set__(self, obj, value):
        # Invalidate cache when any attribute changes
        if hasattr(obj, self.cache_name):
            delattr(obj, self.cache_name)

class MyClass:
    def __init__(self):
        self.some_attr = "initial"
    
    @CachedProperty
    def expensive_computation(self):
        print("Computing...")
        return len(self.some_attr) * 100
```

---

### Exercise 100: Complete OOP System

```python
from abc import ABC, abstractmethod
from datetime import datetime

# Abstract Base Classes
class Product(ABC):
    def __init__(self, name, price, product_id):
        self.name = name
        self.price = price
        self.product_id = product_id
    
    @abstractmethod
    def get_shipping_cost(self):
        pass

class PhysicalProduct(Product):
    def __init__(self, name, price, product_id, weight):
        super().__init__(name, price, product_id)
        self.weight = weight
    
    def get_shipping_cost(self):
        return self.weight * 0.5

class DigitalProduct(Product):
    def get_shipping_cost(self):
        return 0

# Composition
class Address:
    def __init__(self, street, city, zip_code):
        self.street = street
        self.city = city
        self.zip_code = zip_code

class PaymentMethod(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass

class CreditCardPayment(PaymentMethod):
    def __init__(self, card_number):
        self.card_number = card_number
    
    def process_payment(self, amount):
        return f"Charged ${amount} to card {self.card_number[-4:]}"

class PayPalPayment(PaymentMethod):
    def __init__(self, email):
        self.email = email
    
    def process_payment(self, amount):
        return f"Charged ${amount} via PayPal ({self.email})"

class Customer:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.address = None  # Composition
        self.payment_method = None  # Composition
    
    def set_address(self, address):
        self.address = address
    
    def set_payment_method(self, payment_method):
        self.payment_method = payment_method

# Observer Pattern
class PriceObserver:
    def __init__(self, name):
        self.name = name
    
    def notify(self, product, old_price, new_price):
        print(f"{self.name}: {product.name} price changed from ${old_price} to ${new_price}")

class ShoppingCart:
    def __init__(self):
        self.items = {}  # product_id -> quantity
        self.observers = []
    
    def add_observer(self, observer):
        self.observers.append(observer)
    
    def add_item(self, product, quantity=1):
        if product.product_id in self.items:
            self.items[product.product_id] += quantity
        else:
            self.items[product.product_id] = quantity
    
    def notify_price_change(self, product, old_price, new_price):
        for observer in self.observers:
            observer.notify(product, old_price, new_price)

# Strategy Pattern
class ShippingStrategy(ABC):
    @abstractmethod
    def calculate_cost(self, order):
        pass

class StandardShipping(ShippingStrategy):
    def calculate_cost(self, order):
        return 10.0

class ExpressShipping(ShippingStrategy):
    def calculate_cost(self, order):
        return 25.0

class FreeShipping(ShippingStrategy):
    def calculate_cost(self, order):
        return 0.0

class Order:
    def __init__(self, customer, cart):
        self.customer = customer
        self.cart = cart
        self.order_date = datetime.now()
        self.shipping_strategy = None  # Strategy pattern
    
    def set_shipping_strategy(self, strategy):
        self.shipping_strategy = strategy
    
    def calculate_total(self):
        total = 0
        for product_id, quantity in self.cart.items.items():
            # In real system, fetch product by ID
            pass
        if self.shipping_strategy:
            total += self.shipping_strategy.calculate_cost(self)
        return total
    
    def process_payment(self):
        if self.customer.payment_method:
            total = self.calculate_total()
            return self.customer.payment_method.process_payment(total)
        raise ValueError("No payment method set")

# Usage Example
customer = Customer("Alice", "alice@example.com")
customer.set_address(Address("123 Main St", "City", "12345"))
customer.set_payment_method(CreditCardPayment("1234-5678-9012-3456"))

cart = ShoppingCart()
cart.add_observer(PriceObserver("Email Notifier"))

product1 = PhysicalProduct("Laptop", 999, "P001", 5)
product2 = DigitalProduct("Software", 99, "P002")

cart.add_item(product1, 1)
cart.add_item(product2, 1)

order = Order(customer, cart)
order.set_shipping_strategy(StandardShipping())

print(order.process_payment())
```

---

## Conclusion

Congratulations on completing all 100 exercises! You've covered:

- **Basics**: Classes, objects, attributes, methods
- **Encapsulation**: Private/protected attributes, properties
- **Inheritance**: Single and multiple inheritance, method overriding
- **Polymorphism**: Duck typing, operator overloading
- **Special Methods**: `__init__`, `__str__`, `__repr__`, `__add__`, etc.
- **Class & Static Methods**: Alternative constructors, utility functions
- **Abstract Classes**: ABC module, interfaces
- **Composition**: Has-a relationships
- **Design Patterns**: Singleton, Factory, Observer, Strategy, etc.
- **Advanced Topics**: Context managers, descriptors, metaclasses

Keep practicing and applying these concepts in real-world projects!
