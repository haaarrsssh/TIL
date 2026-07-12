## Encapsulation & Access Modifiers

What it is

Encapsulation means hiding an object's internal data and only exposing controlled ways to read or change it. Java enforces this with access modifiers (private, public, protected) that restrict what other classes can touch directly.

Why it matters

Without encapsulation, any code anywhere can reach into an object and set a field to garbage — like age = -5. Getters/setters give you a checkpoint to validate or transform data before it's read or written, and let you change internal implementation later without breaking code that uses the class.

The 80/20

ModifierWho can access itTypical useprivateOnly inside the same classFields — almost alwayspublicAnywhereMethods meant to be the class's APIprotectedSame package + subclassesRare early on, matters more with inheritance(none / default)Same package onlyRare, usually implicit

Direct access p.age would fail to compile — age is private, so it's invisible outside the class.

Quick contrast (Python & JS)

LanguageEnforced privacy?Common conventionPythonNo (convention only)_age — "please don't touch" by naming, not enforcementJavaScriptPartial (#age is truly private, ES2022+)Often skipped entirelyJavaYes, compiler-enforcedprivate field + public getter/setter is the default expectation

Java doesn't trust convention — it's a compile error to access a private field from outside the class, not just a style violation.

Mental model

Think of a class like an ATM. You never reach into the vault (the private field) directly — you interact through the screen and keypad (public methods), which validate your request before touching the actual money. The bank can redesign the vault internally any time without changing how you use the ATM.
