## Control Flow (Java)

What it is

Control flow statements decide which code runs and how many times. Java gives you if/else, switch, and three loop types (for, while, do-while), plus an enhanced for-each for collections.

Why it matters

This is where your operators from Day 3 actually get used — comparisons and booleans drive every branch and loop condition. Java's switch also got a major upgrade (arrow syntax) that removes the old fall-through footgun, which is worth knowing since a lot of tutorials still teach the old style.

The 80/20

ConstructUse whenExampleif / else if / elseBranching on conditionsif (age >= 18) {...}switch (arrow style)Many fixed options, no fall-throughswitch(day) { case MON -> "Start"; }forKnown number of repetitionsfor (int i = 0; i < 5; i++)whileUnknown repetitions, condition-firstwhile (hasNext())do-whileRun at least once, check afterdo {...} while (cond);for-eachIterating arrays/collectionsfor (int n : numbers) {...}

Quick contrast (Python & JS)

LanguageLoop over a listSwitch equivalentPythonfor n in numbers:No native switch (uses match in 3.10+)JavaScriptfor (const n of numbers)switch — fall-through by defaultJavafor (int n : numbers)switch — arrow style avoids fall-through

Java's for-each reads almost identically to Python's for...in and JS's for...of — that part transfers cleanly.

Mental model

Think of if/else as a fork in a hiking trail, switch as a signpost with several fixed destinations, and loops as walking the same lap of a track — for when you know the lap count ahead of time, while when you just keep going until a sign tells you to stop.
