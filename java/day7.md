## Classes & Objects (OOP Basics)

What it is

A class is a blueprint that defines fields (data) and methods (behavior). An object is a concrete instance created from that blueprint using the new keyword. Everything you've written so far (methods living inside a class) was already OOP — Day 7 makes it explicit by building your own classes.

Why it matters

This is the shift from "writing instructions" to "modeling things." Real Java programs are built as networks of objects talking to each other — understanding fields, constructors, and this is the foundation everything else (inheritance, interfaces, collections of objects) builds on.

The 80/20

ConceptSyntaxExampleDefine a classclass Name {...}class Car {...}Field (state)type name;String color;ConstructorClassName(params) {...}Car(String c) { color = c; }this keywordRefers to current objectthis.color = color;Create an objectnew ClassName(args)Car myCar = new Car("red");Call instance methodobject.method()myCar.start();

Quick contrast (Python & JS)

LanguageClass keywordConstructor name"this" equivalentPythonclass Car:__init__(self)selfJavaScriptclass Car {}constructor()thisJavaclass Car {}Same name as classthis

Java's constructor rule is the new part: it must be named exactly like the class — no __init__ or generic constructor keyword.

Mental model

A class is like a cookie cutter, and each object is a cookie made from it. All cookies share the same shape (fields and methods defined by the class), but each one can have its own toppings (its own field values) — changing one cookie's toppings doesn't affect the others.
