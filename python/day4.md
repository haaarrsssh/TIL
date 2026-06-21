## Python Recap & Practice Problems

1. What Is This?

Four days of Python — distilled into one reference sheet, followed by 10 practice problems mixing everything together. Same active-recall format as Day 20's SQL recap. This also marks Day 30 of #100DaysOfCode — a third of the way there.

A month of consistent reps beats a week of motivation.

1. The Full Python Mental Map (Days 27–29)

DayTopicThe One Thing to Remember27Python Basics4 data structures: list (ordered), tuple (locked), dict (lookup), set (unique)28Functions Deep Divelambda shines as sorted(key=lambda x: ...); prefer list comprehensions over map/filter29OOP Basicsself = "this object"; __init__ runs on creation; ListNode/TreeNode are just classes

1. 10 Practice Problems — Write the Code

Easy

P1. Write a function is_even(n) that returns True if n is even, False otherwise.

P2. Given a list of numbers, return a new list with only the positive numbers (use a list comprehension).

P3. Write a function reverse_string(s) that returns the reverse of a string — no built-in [::-1] allowed, use a loop.

P4. Given a list of words, return them sorted by length, shortest first.

Medium

P5. Write a function count_vowels(s) that counts how many vowels (a, e, i, o, u) are in a string.

P6. Given a list of dicts representing users [{"name": "Harsh", "age": 22}, ...], return just the names of users older than 20, sorted alphabetically.

P7. Write a function find_duplicates(nums) that returns a list of numbers that appear more than once.

P8. Create a Rectangle class with width and height attributes, and methods area() and perimeter().

Hard

P9. Given a singly linked list (using ListNode), write a function reverse_linked_list(head) that reverses it and returns the new head.

P10. Write a function group_by_first_letter(words) that takes a list of words and returns a dict grouping them by their first letter. Example: ["apple", "ant", "bee"] → {"a": ["apple", "ant"], "b": ["bee"]}

1. Solutions

<details>
<summary>Click to reveal — try first!</summary>
python# P1
def is_even(n):
    return n % 2 == 0

# P2

def positive_only(nums):
    return [n for n in nums if n > 0]

# P3

def reverse_string(s):
    result = ""
    for char in s:
        result = char + result
    return result

# P4

def sort_by_length(words):
    return sorted(words, key=lambda w: len(w))

# P5

def count_vowels(s):
    vowels = "aeiouAEIOU"
    return sum(1 for char in s if char in vowels)

# P6

def names_above_20(users):
    filtered = [u["name"] for u in users if u["age"] > 20]
    return sorted(filtered)

# P7

def find_duplicates(nums):
    seen = set()
    duplicates = set()
    for n in nums:
        if n in seen:
            duplicates.add(n)
        seen.add(n)
    return list(duplicates)

# P8

class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

# P9

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverse_linked_list(head):
    prev = None
    curr = head
    while curr:
        next_node = curr.next   # save next before overwriting
        curr.next = prev        # reverse the pointer
        prev = curr             # move prev forward
        curr = next_node        # move curr forward
    return prev                 # prev is now the new head

# P10

def group_by_first_letter(words):
    groups = {}
    for word in words:
        first = word[0]
        if first not in groups:
            groups[first] = []
        groups[first].append(word)
    return groups

</details>

1. Pattern → Problem Map (The Real 80/20)

If the problem says...Reach for..."filter items matching a condition"List comprehension with if"transform every item"List comprehension or map()"sort by some custom rule"sorted(key=lambda x: ...)"find duplicates / track what's been seen"A set() to track seen items"group items by some key"A dict where you append to a list per key"model a real-world thing with data + actions"A class"reverse / rebuild a linked structure"Three pointers: prev, curr, next_node"this object knows about itself"self inside a class method

1. 30-Day Checkpoint

You've now covered:

Days 1–13: (prior foundation — assumed from your roadmap)
Days 14–20: SQL — basics through window functions and indexes
Days 21–26: Linux — commands, permissions, bash scripting, cron, env vars, networking
Days 27–29: Python — basics, functional tools, OOP

That's three full skill stacks in 30 days, each ending with a recap to lock it in. The compounding effect: SQL + Linux + Python is the exact toolkit for backend dev, data work, and DevOps — not a coincidence, that's the real-world stack.

Key Takeaway

The 10 problems above aren't random — they map directly to patterns that recur constantly in interviews and real code: filtering, transforming, custom sorting, deduplication, grouping, and basic OOP modeling. The linked list reversal (P9) is the single most repeated "hard" pattern in interviews — the three-pointer technique (prev, curr, next_node) appears in dozens of linked list problems beyond just reversal.
