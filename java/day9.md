## Inheritance

What it is

Inheritance lets one class (subclass) acquire the fields and methods of another (superclass) using extends. It models an "is-a" relationship — a Dog is an Animal.

Why it matters

Without inheritance, you'd copy-paste shared behavior into every related class. Inheritance lets you write common logic once in a superclass, then let subclasses reuse it and only override what's actually different — directly building on the encapsulation from Day 8 (subclasses still respect private fields).

The 80/20

ConceptSyntaxExampleInherit from a classclass Child extends Parent {}class Dog extends Animal {}Call parent constructorsuper(args)super(name);Call parent methodsuper.methodName()super.makeSound();Override a method@Override + same signature@Override void makeSound() {...}

Note: name was declared protected in Day 8's terms, not private — subclasses need at least protected access to reach parent fields directly.

Quick contrast (Python & JS)

LanguageInherit syntaxCall parent constructorPythonclass Dog(Animal):super().__init__(name)JavaScriptclass Dog extends Animal {}super(name)Javaclass Dog extends Animal {}super(name)

Java and JS are nearly identical here — the @Override annotation is Java's extra safety net, catching typos in method signatures at compile time (Python/JS have no equivalent check).

Mental model

Think of a superclass like a general job description ("Animal: can make a sound") and a subclass like a specific job title that inherits the general duties but customizes some of them ("Dog: makes a sound, specifically barking"). super() is like saying "first, do the standard onboarding from the general role" before adding your specialization.
