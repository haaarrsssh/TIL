## Optional

What it is

Optional<T> is a container that either holds a value or holds nothing (empty). It makes "this might not have a value" an explicit part of a method's return type, instead of silently returning null and hoping the caller remembers to check.

Why it matters

NullPointerException is one of the most common runtime crashes in Java — calling a method on something that turned out to be null. Optional doesn't eliminate nulls, but it forces the possibility of "no value" into the type signature itself, so you're nudged to handle the empty case instead of assuming a value is always there.

The 80/20
Task Method Example
Wrap a value Optional.of(value) Optional.of("hi")
Wrap a possibly-null value Optional.ofNullable(value) Optional.ofNullable(name)
Empty optional Optional.empty() Optional<String> none = Optional.empty();
Check if present .isPresent() opt.isPresent() → true/false
Get with fallback .orElse(default) opt.orElse("Unknown")
Transform if present .map(function) opt.map(String::toUpperCase)
Act only if present .ifPresent(consumer) opt.ifPresent(System.out::println)
Old way vs Optional way
java
// Old way — manual null check
String name = findUser(); // might return null
if (name != null) {
    System.out.println(name.toUpperCase());
} else {
    System.out.println("Unknown");
}

// Optional way — the possibility of "no value" is explicit
Optional<String> name = findUser(); // return type signals "might be empty"
System.out.println(name.map(String::toUpperCase).orElse("Unknown"));

The method signature itself (Optional<String> vs String) tells the caller "check before assuming this has a value" — you don't need to read docs or guess.

With Streams (Day 18)
java
Optional<Integer> first = numbers.stream()
    .filter(n -> n > 10)
    .findFirst(); // returns Optional — might be empty if nothing matched

System.out.println(first.orElse(-1)); // fallback if no match
Quick contrast (Python & JS)
Language Handling "might not exist" Common pitfall
Python Returns None, checked with if x is not None Easy to forget the check
JavaScript Returns undefined/null, optional chaining ?. helps Same risk, mitigated by ?.
Java Optional<T> return type Type system forces awareness upfront

JS's ?. optional chaining softens the same problem at the call site. Java's Optional pushes the responsibility earlier — into the method's declared return type — so it's visible before you even call it.

Mental model

Think of Optional like a gift box that might be empty. Instead of just handing someone a box and letting them assume there's a gift inside (risking disappointment — a NullPointerException — when they reach in), Optional makes the box's "might be empty" status part of how it's labeled, so you check before you reach in.
