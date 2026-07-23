## Lambdas & Functional Interfaces

What it is

A functional interface is an interface with exactly one abstract method (Day 11's interfaces, restricted to one method). A lambda expression is compact syntax for providing that one method's implementation inline, without writing a whole named class.

Why it matters

Before lambdas, implementing a one-method interface meant writing a full anonymous class just to pass around a tiny bit of behavior. Lambdas cut that ceremony down to one line — and they're everywhere in modern Java, especially with Collections (Day 13) for sorting, filtering, and iterating.

The 80/20
Lambda syntax
java
(parameters) -> expression
(parameters) -> { statements; }
Before vs after
java
// Old way — anonymous class implementing Runnable (one method: run())
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};

// New way — lambda
Runnable r2 = () -> System.out.println("Running");
Common built-in functional interfaces
Interface Abstract method Typical use
Runnable run() Code to execute, no input/output
Comparator<T> compare(a, b) Custom sorting logic
Predicate<T> test(value) Returns true/false — filtering
Function<T,R> apply(value) Transforms input into output
Real use: sorting with a lambda
java
ArrayList<String> names = new ArrayList<>(List.of("Charlie", "Alice", "Bob"));

names.sort((a, b) -> a.compareTo(b)); // ascending order
System.out.println(names); // [Alice, Bob, Charlie]

.sort() expects a Comparator<String> — instead of writing a whole class, the lambda (a, b) -> a.compareTo(b) provides that logic directly.

Quick contrast (Python & JS)
Language Equivalent Example
Python lambda keyword lambda a, b: a - b
JavaScript Arrow functions (a, b) => a - b
Java Lambda expression, but only for functional interfaces (a, b) -> a.compareTo(b)

The syntax feels almost identical to JS arrow functions. The difference: in Python/JS, a lambda is just a function value you can pass anywhere. In Java, a lambda must match a functional interface's single method signature — it's not a free-floating function type.

Mental model

Think of a functional interface like a job posting with exactly one task listed ("compare two items"). Before lambdas, filling that job meant hiring a full-time employee (writing a whole class). A lambda is like hiring a freelancer for just that one task — quick, disposable, no extra setup.
