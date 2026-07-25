## Generics (Writing Your Own)

What it is

Generics let a class or method work with any type while still being type-checked at compile time. You've been using generics since Day 13 (ArrayList<String>) — today's about writing your own generic classes and methods.

Why it matters

Without generics, you'd either write a separate class for every type (IntBox, StringBox, DoubleBox...) or use Object and lose type safety, needing manual casts everywhere. Generics give you one reusable class that stays type-safe for whatever type it's used with — the same guarantee Day 13's ArrayList<String> gave you, but now you can build it yourself.

The 80/20
Generic class
java
class Box<T> {
    private T content;

    void set(T content) { this.content = content; }
    T get() { return content; }
}

Box<String> stringBox = new Box<>();
stringBox.set("hello");
String value = stringBox.get(); // no cast needed, compiler knows it's a String

Box<Integer> intBox = new Box<>();
intBox.set(42);

T is a placeholder type — it's decided when the class is used, not when it's written.

Generic method
java
<T> void printItem(T item) {
    System.out.println(item);
}

printItem("hello"); // T becomes String
printItem(42);       // T becomes Integer

The <T> before the return type declares that this method has its own type parameter.

Bounded type parameters
java
<T extends Number> double sum(T a, T b) {
    return a.doubleValue() + b.doubleValue();
}

extends Number restricts T to Number or its subclasses (Integer, Double, etc.) — so you can call Number methods like .doubleValue() inside, which plain T wouldn't allow.

Concept Syntax Meaning
Generic class class Box<T> {} T decided when instantiated
Generic method <T> void method(T x) {} T decided per call
Bounded type <T extends Number> T restricted to a type or its subclasses
Quick contrast (Python & JS)
Language Generics? Why
Python No native concept (type hints only, unchecked) Dynamic typing — any type works everywhere already
JavaScript No native concept Same — dynamic typing
Java Yes, compiler-enforced Static typing needs a way to stay flexible without losing safety

Python/JS never needed generics because their functions already accept any type by default. Generics exist in Java specifically to bring that same flexibility to a statically typed language, without giving up compile-time checks.

Mental model

A generic class is like a shipping box with a label you fill in later: "Contents: ___." You design the box once — same size, same structure — and it can hold books, electronics, or clothes, but once you write "Books" on the label, the box (and anyone opening it) knows for certain to expect books, not electronics.
