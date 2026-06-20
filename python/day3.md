## Python OOP Basics: Classes & Objects

1. What Are They?

A class is a blueprint for creating objects — it defines what data (attributes) and behavior (methods) every object made from it will have. An object (or instance) is one actual thing built from that blueprint.

A class is the cookie cutter. Objects are the cookies — same shape, different details.

1. Why Does It Matter?

OOP (Object-Oriented Programming) is how most real-world software is structured:

Django/FastAPI models are classes — a User class represents every user in your app
LeetCode problems often give you a class already (TreeNode, ListNode) — you must understand classes to solve them
Any time you're modeling a "thing" with properties and actions (a User, a BankAccount, a Car) — that's a class

If you've used ListNode or TreeNode in your CP practice, you've already been using OOP without naming it.

1. The 20% That Covers 80% of Real Work

Defining a class and creating objects

pythonclass Dog:
    def __init__(self, name, breed):
        self.name = name      # attribute
        self.breed = breed    # attribute

    def bark(self):           # method
        return f"{self.name} says Woof!"

# Create objects (instances)

dog1 = Dog("Rex", "Labrador")
dog2 = Dog("Tommy", "Pug")

print(dog1.name)       # Rex
print(dog1.bark())     # Rex says Woof!
print(dog2.bark())     # Tommy says Woof!

__init__ is the constructor — runs automatically when you create an object.
self refers to the specific object calling the method — always the first parameter.

Attributes vs Methods

pythonclass BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner          # attribute — data
        self.balance = balance      # attribute — data

    def deposit(self, amount):      # method — behavior
        self.balance += amount
        return self.balance

    def withdraw(self, amount):
        if amount > self.balance:
            return "Insufficient funds"
        self.balance -= amount
        return self.balance

account = BankAccount("Harsh", 1000)
account.deposit(500)
print(account.balance)        # 1500
print(account.withdraw(2000)) # Insufficient funds

__str__ — control how an object prints

pythonclass Dog:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f"Dog(name={self.name})"

dog = Dog("Rex")
print(dog)          # Without __str__: <__main__.Dog object at 0x...>
                     # With __str__:    Dog(name=Rex)

Inheritance — building on existing classes

pythonclass Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return "Some generic sound"

class Dog(Animal):              # Dog inherits from Animal
    def speak(self):            # override the parent method
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow!"

dog = Dog("Rex")
cat = Cat("Whiskers")
print(dog.speak())     # Rex says Woof!
print(cat.speak())     # Whiskers says Meow!

python# Using super() to extend the parent's __init__
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

class Manager(Employee):
    def __init__(self, name, salary, team_size):
        super().__init__(name, salary)   # call parent's __init__
        self.team_size = team_size

mgr = Manager("Harsh", 80000, 5)
print(mgr.name, mgr.salary, mgr.team_size)   # Harsh 80000 5

Classes in LeetCode — recognize these instantly

python# Linked list node — appears in nearly every linked list problem
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# Binary tree node — appears in nearly every tree problem

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# Building a small linked list: 1 -> 2 -> 3

head = ListNode(1, ListNode(2, ListNode(3)))

# Traversing it

curr = head
while curr:
    print(curr.val)
    curr = curr.next

1. Real-Life Mental Model

ConceptReal EquivalentClassA blueprint / cookie cutter / form templateObject/InstanceOne actual thing made from the blueprint__init__The setup that happens the moment something is createdself"This specific object" — like saying "my" instead of "a"AttributeA noun — data the object holdsMethodA verb — something the object can doInheritanceA subclass that says "I'm a type of X, but different"super()"Also do whatever the parent class does first"

The mental shortcut for self:

pythonclass Dog:
    def __init__(self, name):
        self.name = name    # "THIS dog's name"

    def bark(self):
        return self.name    # "THIS dog's name" — same dog every time

self is just Python's way of saying "the object I'm currently working with."

Key Takeaway

A class bundles data (attributes) and behavior (methods) into one blueprint; objects are individual instances built from it. __init__ sets up new objects, self always refers to the current instance, and inheritance lets you reuse and override behavior across related classes. The moment you see ListNode or TreeNode in a LeetCode problem, you're already working with OOP — now you know exactly what's happening under the hood.
