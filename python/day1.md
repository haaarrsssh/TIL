## Python Basics

1. What is Python?

Python is a high-level, interpreted programming language known for readable syntax that looks almost like plain English. No semicolons, no curly braces — just indentation defines structure.

Python reads like pseudocode that actually runs.

1. Why Does It Matter?

Python is the most versatile language in tech right now:

Automation — the bash scripts from Day 23, but more powerful
Data/AI — pandas, numpy, machine learning, the entire AI boom
Backend — Django, FastAPI power real production apps
LeetCode/DSA — clean syntax means less boilerplate, more logic

It's also usually the first language recommended for competitive programming and interviews because you can express algorithms with minimal code — exactly what you need for your CP practice.

1. The 20% That Covers 80% of Real Work

Variables & data types

pythonname = "Harsh"          # string
age = 22                # int
height = 5.9             # float
is_coding = True         # bool

print(type(age))         # <class 'int'>

Strings

pythonname = "Harsh"
greeting = f"Hello, {name}!"      # f-strings — use these always
print(greeting)                    # Hello, Harsh!

text = "  Day 27  "
print(text.strip())                # "Day 27" — removes whitespace
print(text.lower())                # lowercase
print("100daysofcode".upper())     # uppercase
print(",".join(["a", "b", "c"]))   # "a,b,c"
print("a,b,c".split(","))          # ['a', 'b', 'c']

Lists, Tuples, Dicts, Sets — the 4 core data structures

python# List — ordered, mutable
nums = [1, 2, 3]
nums.append(4)
nums[0] = 99
print(nums)              # [99, 2, 3, 4]

# Tuple — ordered, immutable

point = (3, 4)
x, y = point              # unpacking

# Dict — key-value pairs

user = {"name": "Harsh", "age": 22}
print(user["name"])       # Harsh
user["city"] = "Delhi"    # add a new key
print(user.get("email", "N/A"))   # safe access with default

# Set — unique values, no order

unique = {1, 2, 2, 3}
print(unique)              # {1, 2, 3}

Conditionals

pythonage = 20

if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teenager")
else:
    print("Child")

# Ternary (one-line if)

status = "Adult" if age >= 18 else "Minor"

Loops

python# for loop over a list
for num in [1, 2, 3]:
    print(num)

# for loop with range

for i in range(5):          # 0,1,2,3,4
    print(i)

# for loop with index — enumerate

for i, name in enumerate(["Harsh", "Riya"]):
    print(i, name)

# while loop

count = 0
while count < 5:
    print(count)
    count += 1

# List comprehension — Python's superpower

squares = [x**2 for x in range(5)]       # [0, 1, 4, 9, 16]
evens = [x for x in range(10) if x % 2 == 0]

Functions

pythondef greet(name):
    return f"Hello, {name}!"

def add(a, b=0):              # b has a default value
    return a + b

print(greet("Harsh"))
print(add(5, 3))               # 8
print(add(5))                  # 5 (uses default b=0)

# Multiple return values

def min_max(nums):
    return min(nums), max(nums)

low, high = min_max([3, 1, 4, 1, 5])

Error handling

pythontry:
    result = 10 / 0
except ZeroDivisionError:
    print("Can't divide by zero!")
except Exception as e:
    print(f"Something went wrong: {e}")
finally:
    print("This always runs")

Reading input (useful for LeetCode-style local testing)

pythonn = int(input())                       # read an int
nums = list(map(int, input().split())) # read a line of space-separated ints

1. Real-Life Mental Model

ConceptReal EquivalentList []A numbered to-do list — order mattersTuple ()A locked box — can't change once sealedDict {}A labeled filing cabinet — lookup by nameSet {} (unique)A guest list with no duplicates allowedfor loop"Do this for every item in the list"List comprehensionA one-line factory: input → transform → outputtry/except"Attempt this, but have a backup plan if it fails"

Indentation is the syntax:

pythonif True:
    print("inside if")    # 4 spaces — this IS the block
print("outside if")        # no indent — outside the if

Unlike Bash's [ ] or SQL's keywords, Python uses whitespace to define structure. Mixing tabs and spaces breaks everything — pick spaces and stay consistent.

Key Takeaway

Python's core toolkit is small: variables, the 4 data structures (list, tuple, dict, set), conditionals, loops, functions, and try/except. List comprehensions are the one Python-specific superpower worth mastering early — they replace 3-line loops with 1 readable line, which is exactly what you'll want for clean LeetCode solutions.
