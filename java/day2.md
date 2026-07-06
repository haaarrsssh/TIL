## Variables & Data Types (Java)

What it is

A variable is a named, typed storage slot in memory. Java is statically typed — you declare the type upfront, and it can never change for that variable.

Why it matters

The compiler catches type mistakes before your code ever runs. This trades a bit of typing effort for a big reduction in runtime surprises — especially valuable in large codebases where one wrong type can silently corrupt data.

The 80/20

You'll use these types almost all the time:

TypeStoresExampleintWhole numbersint age = 25;doubleDecimalsdouble price = 9.99;booleantrue/falseboolean isDone = false;charSingle characterchar grade = 'A';StringTextString name = "Harsh";

Everything else (byte, short, long, float) exists for memory/precision edge cases you'll rarely touch early on.

Quick contrast (since you've done Python & JS)

LanguageTyping styleDeclare a numberPythonDynamicage = 25JavaScriptDynamiclet age = 25;JavaStaticint age = 25;

The mental shift: in Java, you're telling the compiler what kind of box you're building before you put anything in it.

Mental model

Think of Java variables like labeled containers at a warehouse — a container labeled "liquids only" (double) will reject a solid item (String) even if it would technically fit. Python/JS containers, by contrast, relabel themselves automatically based on whatever you put in.
