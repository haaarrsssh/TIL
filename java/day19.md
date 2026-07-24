## Streams API

What it is

A Stream is a pipeline for processing a sequence of elements — filter, transform, and collect data declaratively, instead of writing manual loops. A stream isn't a data structure itself; it's a one-time view over a source (usually a Collection from Day 13) that you chain operations onto.

Why it matters

Day 17 gave you lambdas as portable bits of behavior. Streams are where they shine — instead of writing a for loop with an if check and a manual list to collect results into, you describe what you want ("filter these, transform those, collect the result") and Java handles the how.

The 80/20
Operation Type Purpose Example
.filter(predicate) Intermediate Keep matching elements .filter(n -> n > 10)
.map(function) Intermediate Transform each element .map(n -> n * 2)
.forEach(consumer) Terminal Do something with each element .forEach(System.out::println)
.collect(collector) Terminal Gather results into a collection .collect(Collectors.toList())
.sorted() Intermediate Sort elements .sorted()

Intermediate operations return a new stream (chainable). Terminal operations end the pipeline and produce a result.

Old loop vs stream
java
// Old way — manual loop
ArrayList<Integer> result = new ArrayList<>();
for (int n : numbers) {
    if (n > 10) {
        result.add(n * 2);
    }
}

// Stream way — declarative pipeline
List<Integer> result = numbers.stream()
    .filter(n -> n > 10)   // keep n > 10
    .map(n -> n * 2)        // double each
    .collect(Collectors.toList()); // gather into a List

Same result, but the stream version reads like a description of intent rather than step-by-step instructions.

Getting a stream started
java
numbers.stream()       // from a Collection
Arrays.stream(array)   // from an array
Stream.of(1, 2, 3)      // from individual values
Quick contrast (Python & JS)
Language Equivalent Example
Python List comprehension [n*2 for n in nums if n > 10]
JavaScript Array .filter().map() chaining nums.filter(n => n>10).map(n => n*2)
Java Streams nums.stream().filter(...).map(...).collect(...)

JS's chained .filter().map() is the closest match conceptually — Java's version just needs the extra .stream() to start and .collect() to finish, since it's not a Collection method directly.

Mental model

Think of a stream like a factory conveyor belt. Items (elements) move down the belt through stations (.filter(), .map()) that each do one job, and at the end there's a collection bin (.collect()) where the finished items land. The belt itself isn't storage — it's just the process the items pass through once.
