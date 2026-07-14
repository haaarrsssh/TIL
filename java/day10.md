## : Polymorphism

What it is

Polymorphism means "many forms" — the same method call behaves differently depending on the actual object involved. Java has two kinds: compile-time (overloading, from Day 6) and runtime (overriding, from Day 9).

Why it matters

Runtime polymorphism is what makes inheritance genuinely useful in practice: you can write code against a general type (Animal) and have it automatically run the correct specific behavior (Dog's bark, Cat's meow) without writing if/else chains checking "what type is this really?"

The 80/20

TypeWhen it's decidedMechanismExampleCompile-timeAt compile timeOverloading (Day 6)add(int,int) vs add(double,double)RuntimeAt runtimeOverriding (Day 9)Animal a = new Dog(); a.makeSound();

instanceof lets you check before downcasting, avoiding a ClassCastException.

Quick contrast (Python & JS)

LanguageRuntime polymorphismHow it's decidedPythonYes, alwaysDuck typing — no type declared at allJavaScriptYes, alwaysPrototype chain, no static typesJavaYes, but only through inheritanceStatic type at compile time, actual type at runtime

Python/JS get polymorphism "for free" since they never check types until the method is actually called. Java's version is deliberate — you build it through extends and @Override, and the compiler still checks method existence against the declared type.

Mental model

Think of "Animal" like a universal remote button labeled "Play Sound." Point it at a Dog, it barks. Point it at a Cat, it meows. The button (method call) looks the same every time — the remote doesn't need to know which device it's pointed at, the device itself decides what "Play Sound" means for it.
