## Python Functions Deep Dive

1. What is This?

Day 27 covered basic functions. Today goes deeper into Python's functional toolkit — *args/**kwargs for flexible arguments, lambda for throwaway one-line functions, and map/filter/sorted with custom logic. These show up constantly in clean Python code and in LeetCode solutions.

Yesterday: functions that take fixed inputs. Today: functions flexible enough to handle anything.

1. Why Does It Matter?

This is the difference between beginner Python and Python that looks professional:

Library code (pandas, requests, etc.) uses *args/**kwargs everywhere — you need to read it
lambda + sorted/map/filter lets you write in 1 line what would take a 5-line loop
LeetCode solutions are graded partly on cleanliness — this is how experienced solvers write tight code

1. The 20% That Covers 80% of Real Work

*args — accept any number of positional arguments

pythondef total(*nums):
    return sum(nums)

print(total(1, 2, 3))        # 6
print(total(1, 2, 3, 4, 5))  # 15 — works with any count

# *args is just a tuple inside the function

def show_args(*args):
    print(type(args))   # <class 'tuple'>
    print(args)          # (1, 2, 3)

**kwargs — accept any number of keyword arguments

pythondef show_profile(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

show_profile(name="Harsh", age=22, city="Delhi")

# name: Harsh

# age: 22

# city: Delhi

# **kwargs is just a dict inside the function

def show_kwargs(**kwargs):
    print(type(kwargs))   # <class 'dict'>

Combining both

pythondef describe(name, *args, **kwargs):
    print(f"Name: {name}")
    print(f"Extra args: {args}")
    print(f"Extra info: {kwargs}")

describe("Harsh", "coder", "learner", age=22, city="Delhi")

# Name: Harsh

# Extra args: ('coder', 'learner')

# Extra info: {'age': 22, 'city': 'Delhi'}

lambda — anonymous one-line functions

python# Regular function
def square(x):
    return x ** 2

# Same thing as a lambda

square = lambda x: x ** 2
print(square(5))    # 25

# Multiple arguments

add = lambda a, b: a + b
print(add(3, 4))     # 7

# Use case: lambdas shine when passed directly into another function

# (you rarely assign them to a variable like above in real code)

sorted() with custom logic — the #1 real use of lambda

pythonstudents = [("Harsh", 85), ("Riya", 92), ("Arjun", 78)]

# Sort by score (2nd item in tuple)

sorted_students = sorted(students, key=lambda s: s[1])
print(sorted_students)

# [('Arjun', 78), ('Harsh', 85), ('Riya', 92)]

# Sort descending

sorted_desc = sorted(students, key=lambda s: s[1], reverse=True)

# Sort a list of dicts by a field

users = [{"name": "Harsh", "age": 22}, {"name": "Riya", "age": 19}]
sorted_users = sorted(users, key=lambda u: u["age"])

map() — apply a function to every item

pythonnums = [1, 2, 3, 4]

squared = list(map(lambda x: x ** 2, nums))
print(squared)    # [1, 4, 9, 16]

# Same as this list comprehension (often preferred for readability)

squared = [x ** 2 for x in nums]

filter() — keep items that match a condition

pythonnums = [1, 2, 3, 4, 5, 6]

evens = list(filter(lambda x: x % 2 == 0, nums))
print(evens)    # [2, 4, 6]

# Same as this list comprehension

evens = [x for x in nums if x % 2 == 0]

Real talk: list comprehensions are usually more Pythonic and readable than map/filter + lambda. But you'll see map/filter in other people's code constantly — know both.

Default + keyword arguments together

pythondef create_user(name, age=18, *, role="user"):
    # the* forces role to be passed as a keyword only
    return f"{name}, {age}, {role}"

print(create_user("Harsh"))                    # Harsh, 18, user
print(create_user("Harsh", 22))                 # Harsh, 22, user
print(create_user("Harsh", 22, role="admin"))   # Harsh, 22, admin

1. Real-Life Mental Model

ToolReal Equivalent*args"Pass me as many items as you want, I'll collect them in a tuple"**kwargs"Pass me any labeled settings, I'll collect them in a dict"lambdaA sticky note function — quick, disposable, no name neededsorted(key=lambda...)"Sort by THIS specific thing, not the default"map()A conveyor belt — every item gets the same transformationfilter()A sieve — only items passing the test get through

When to reach for each:

python# Use *args/**kwargs when:

# → writing a function that wraps/forwards to another function

# → you don't know how many arguments will be passed

# Use lambda when

# → you need a quick function as an ARGUMENT to another function (sorted, map, filter)

# → NOT for naming a reusable function — use def for that

# Use list comprehension over map/filter when

# → readability matters more than one-liner cleverness (almost always)

Key Takeaway

*args and **kwargs let functions accept flexible input — essential for understanding library code. lambda is for quick, throwaway logic passed into another function — its #1 real use is as the key= argument in sorted(). Know map/filter to read others' code, but default to list comprehensions for your own — they're more readable and more "Pythonic."
