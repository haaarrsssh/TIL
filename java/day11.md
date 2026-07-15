## Abstraction — Abstract Classes & Interfaces

What it is

Abstraction means defining what something must do without necessarily saying how. Java gives you two tools for this: abstract classes (partial implementation, can't be instantiated directly) and interfaces (pure contracts — just method signatures, no state).

Why it matters

Day 10's Animal.makeSound() worked, but nothing forced Dog or Cat to actually override it — if they forgot, they'd silently inherit a useless default. Abstraction makes that a compile error instead: if a class extends an abstract class or implements an interface, it must provide the missing pieces, or the code won't compile.

The 80/20

FeatureAbstract classInterfaceKeywordabstract classinterfaceSubclass keywordextendsimplementsCan have fields?YesNo (only constants)Can have method bodies?Yes, mixedMostly no (default methods are the exception)Instantiable?No — never new AbstractThing()NoMultiple inheritance?No — one abstract class onlyYes — a class can implement several

Abstract class

javaabstract class Animal {
    String name;
    Animal(String name) { this.name = name; }

    abstract void makeSound(); // no body — subclass MUST implement this

    void sleep() { // shared, concrete behavior
        System.out.println(name + " is sleeping");
    }
}

class Dog extends Animal {
    Dog(String name) { super(name); }
    @Override void makeSound() { System.out.println("Bark"); }
}

new Animal("x") would fail to compile — you can only instantiate Dog.

Interface

javainterface Movable {
    void move(); // implicitly public and abstract
}

class Dog extends Animal implements Movable {
    @Override public void move() { System.out.println("Dog runs"); }
}

A class can implements multiple interfaces, which is how Java fakes "multiple inheritance."

Quick contrast (Python & JS)

LanguageAbstract classesInterfacesPythonABC + @abstractmethod (opt-in module)No native concept — duck typing insteadJavaScriptNo native keyword — convention onlyNo native conceptJavaBuilt-in abstract keywordBuilt-in interface keyword, compiler-enforced

Python/JS mostly rely on convention and duck typing ("if it has a .move() method, treat it as movable"). Java bakes the contract into the type system, so violations are caught before the program ever runs.

Mental model

An abstract class is like a job offer letter with some parts pre-filled (salary, benefits — the concrete methods) and a blank line that says "your specific duties" (the abstract method) which you must fill in before signing. An interface is stricter still — it's just a checklist ("must be able to: move, eat, sleep") with no pre-filled parts at all, and you can be handed several checklists to satisfy at once.
