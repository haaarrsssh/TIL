## Operators & Expressions (Java)

What it is

Operators combine values into new values. Java groups them into arithmetic, comparison, logical, and assignment operators — plus casting, which converts one type into another.

Why it matters

Because Java is statically typed, operators behave strictly based on the types involved. The classic trap: int / int always gives an int, even if you expected a decimal — this silently truncates instead of erroring, so it's a common source of bugs for beginners.

The 80/20

CategoryOperatorsExampleArithmetic+ - */ %int sum = 5 + 3;Comparison== != > < >= <=age >= 18Logical&& || !isAdult && hasIDAssignment= += -=*= /=score += 10;

The gotcha: integer division

int result = 7 / 2;       // 3, not 3.5 — truncated!
double result2 = 7 / 2;   // still 3.0 — division happens before assignment
double result3 = 7.0 / 2; // 3.5 — correct

Fix: make at least one operand a double before the division happens.

Casting

TypeDirectionSafe?ExampleWideningsmall → big (int → double)Automatic, safedouble d = 5;Narrowingbig → small (double → int)Must cast explicitly, can lose dataint i = (int) 5.9; → 5

Quick contrast (Python & JS)

Language7 / 2 resultPython 33.5 (true division by default)JavaScript3.5 (no int/float distinction)Java3 (int division truncates)

This is the single biggest "wait, why?" moment coming from Python or JS — Java forces you to be explicit about intent.

Mental model

Think of casting like pouring liquid between containers: widening is pouring a small cup into a big jug — nothing spills. Narrowing is pouring a jug into a small cup — you have to choose to do it, and you will lose some of what didn't fit.
