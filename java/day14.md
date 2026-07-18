## Strings & StringBuilder

What it is

A String in Java is an immutable sequence of characters — once created, it can never be changed; every "modification" actually creates a brand new String. StringBuilder is the mutable alternative for when you need to build/change text efficiently, especially in loops.

Why it matters

Immutability makes Strings safe to share and predictable (no surprise changes from elsewhere in your code), but it has a hidden cost: concatenating Strings in a loop silently creates a new object every single time, which gets slow fast. StringBuilder avoids that by modifying one object in place.

The 80/20

Common String methods

TaskMethodExampleLength.length()"hello".length() → 5Substring.substring(start, end)"hello".substring(1,3) → "el"Compare content.equals()a.equals(b)Compare reference==Compares memory location, not contentTo uppercase/lowercase.toUpperCase() / .toLowerCase()"hi".toUpperCase() → "HI"Contains.contains()"hello".contains("ell") → true

The .equals() vs == trap

javaString a = new String("hi");
String b = new String("hi");
System.out.println(a == b);       // false — different objects in memory
System.out.println(a.equals(b));  // true — same content

Always use .equals() for comparing String content. == compares whether they're literally the same object.

StringBuilder — when concatenation gets expensive

java// Bad in a loop — creates a new String object every iteration
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i; // slow — 1000 throwaway String objects
}

// Good — modifies one object in place
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i);
}
String result = sb.toString(); // convert back to String at the end

Quick contrast (Python & JS)

LanguageStrings mutable?Content comparisonBuild strings efficientlyPythonNo (immutable)== (compares content directly)"".join(list)JavaScriptNo (immutable)=== (compares content for primitives)Array .join() or template literalsJavaNo (immutable).equals(), not ==StringBuilder

The immutability is the same across all three — Java's twist is that == doesn't do what you'd expect from Python/JS, since Java's == always checks object identity, not content.

Mental model

A String is like a printed document — you can't edit it, only photocopy it with changes (a new object each time). A StringBuilder is like a whiteboard — you keep erasing and rewriting the same surface, and only print the final document (.toString()) once you're done.
