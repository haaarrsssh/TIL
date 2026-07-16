## Exception Handling

What it is

An exception is an object representing an error that occurs during execution. Java lets you catch and handle these gracefully using try/catch/finally, instead of letting the program crash outright.

Why it matters

Real programs deal with things that can fail — bad input, missing files, network issues. Without exception handling, one bad case crashes the whole program. Java also forces you to acknowledge certain failure types at compile time (checked exceptions), which is stricter than Python/JS's "deal with it if it happens" approach.

The 80/20

KeywordPurposeExampletryCode that might throwtry { risky(); }catchHandle a specific exception typecatch (Exception e) {...}finallyAlways runs, cleanup codefinally { close(); }throwManually raise an exceptionthrow new IllegalArgumentException("bad");throwsDeclare a method might throwvoid read() throws IOException {...}

Checked vs unchecked

TypeChecked atMust handle?ExampleCheckedCompile timeYes — try/catch or throws requiredIOExceptionUncheckedRuntime onlyNo — optionalNullPointerException, ArithmeticException

If a method might throw a checked exception, the compiler forces you to either catch it or declare throws — you literally cannot ignore it.

Quick contrast (Python & JS)

LanguageTry/catch syntaxChecked exceptions?Pythontry / exceptNo — all exceptions are "unchecked"JavaScripttry / catchNoJavatry / catch / finallyYes — compiler enforces handling for some

Python and JS never force you to handle anything at compile time — errors only surface when the code actually runs. Java's checked exceptions catch missed error handling before the program even starts.

Mental model

Think of try/catch like a safety net under a tightrope walker. The try block is the risky walk, catch is the net that catches a specific kind of fall, and finally is the crew that packs up the equipment no matter whether the walker fell or made it across. Checked exceptions are like a venue that requires you to prove the net is set up before they let the walker start.
