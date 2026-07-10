## Methods (Java)

What it is

A method is a named, reusable block of code with a defined return type and parameter list. Every method in Java lives inside a class — there's no such thing as a free-floating function like in Python or JS.

Why it matters

Methods are how you stop repeating the loops/conditionals from Days 4-5 everywhere. Java also enforces a strict signature (return type + parameter types) which enables overloading — something Python and JS don't have, since they resolve calls dynamically at runtime instead of by type.

The 80/20

ConceptSyntaxExampleDefine a methodreturnType name(params) {...}int add(int a, int b) {...}No return valueUse voidvoid printHello() {...}Call a methodname(args)add(2, 3)Static methodstatic keyword, called on classMath.max(2, 3)Instance methodNo static, called on objectmyCar.start()Pass an arrayJust like any parametervoid printAll(int[] nums) {...}

Overloading — same name, different signature

javaint add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }

Java picks the right one based on argument types at compile time. Python/JS would just overwrite the first add with the second.

Quick contrast (Python & JS)

LanguageWhere methods liveOverloading?PythonFree functions or class methodsNo (last definition wins)JavaScriptFree functions or class methodsNo (last definition wins)JavaAlways inside a classYes — resolved by parameter types

Mental model

Think of a method like a vending machine slot: you put in specific inputs (parameters of a defined type), and it's only wired to give back one specific kind of output (the return type). Overloading is like having several slots labeled the same name but built for different coin types — the machine reads what you inserted and routes you to the right slot.
