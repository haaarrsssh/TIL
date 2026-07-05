## Java Basics

1. What is Java?
Java is a statically typed, object-oriented language that runs on the JVM (Java Virtual Machine) — meaning the same code runs on any OS without changes. Every variable must declare its type upfront, and every program must live inside a class.

"Write once, run anywhere." — Java's core promise since 1995.

1. Why Does It Matter?
Android development — Java is the original Android language
Enterprise backends — Spring Boot (Java) powers Netflix, LinkedIn, Amazon
Coding interviews — heavily used in FAANG-style interviews alongside Python/C++
Strong foundation — learning Java makes every other typed language easier
2. The Core Concepts (80/20)

The 3 things Java enforces that Python/JS don't

RuleWhat it meansDeclare typesEvery variable needs a type: int, String, boolean, etc. — you can't change it laterEverything in a classNo standalone functions — all code lives inside a classDeclare return typesEvery method must say what it returns (int, void, String, etc.)

Primitive types — the building blocks

TypeHoldsExampleintWhole numbers42, -7doubleDecimal numbers3.14, 9.99booleanTrue or falsetrue, falsecharA single character'A', 'z'

These are lowercase and stored directly in memory — fast and simple.

Reference types — the objects

TypeHoldsNotesStringTextUppercase — it's an object, not a primitiveint[]A fixed array of integersSize must be decided at creation — can't growAny classComplex dataEverything else you'll define yourself

Unlike Python lists, Java arrays are fixed size — once created at length 5, they stay at 5.

The entry point — memorize this once, use it forever

Every Java program starts from a method called main inside a class. The class name must match the filename. This is just Java's required boilerplate — not something you design, just something you always write.

Control flow — same ideas, different syntax

if / else if / else — same logic as Python and JS, just with {} blocks
switch — cleaner than long if-else chains when checking one variable against many values
for loop — index-based, like JS (for int i = 0; i < n; i++)
for-each loop — cleaner iteration over arrays, like Python's for x in arr
while loop — same concept across all languages

Methods — Java's version of functions

Every method in Java needs:

An access modifier (public, private)
Whether it's static or not (use static in main and helper methods for now)
A return type (int, String, void if it returns nothing)
A name and parameters with their types

Think of it like TypeScript's function signatures — just more mandatory.

Input — Scanner

Java uses a built-in tool called Scanner to read user input from the terminal. It reads one line or one value at a time. For LeetCode and most interview judges, input is handled automatically — you'll rarely write Scanner yourself except for local testing.

1. Java vs What You Already Know

ConceptPythonJavaScript/TSJavaVariablex = 5const x = 5int x = 5;Printprint()console.log()System.out.println()Arraylist (dynamic)[] (dynamic)int[] (fixed size)No return(nothing needed)void (TS)void (required)Strict types❌✅ (TS)✅ (always)Runs inInterpreterBrowser/NodeJVM

1. Mental Model — How to Think About Java

Java is TypeScript taken to its logical extreme.

TypeScript adds optional types to JS → Java makes types mandatory, always
TypeScript compiles to JS → Java compiles to bytecode that runs on the JVM
TypeScript has interfaces and classes → Java is built entirely around classes

If TypeScript felt strict, Java feels stricter — but that strictness is exactly why it's trusted for large-scale enterprise systems and why the compiler catches bugs Python and JS would silently miss.

Key Takeaway

Java = types + classes + strict rules. The boilerplate is annoying at first but becomes muscle memory fast. Everything you know about variables, loops, conditionals, and functions from Python/JS applies here — Java just makes you be explicit about every type and return value. That explicitness is the feature, not the bug.
