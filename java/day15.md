## HashSet

What it is

A HashSet is a collection that stores unique elements with no guaranteed order. It's the third core Collections type alongside Day 13's ArrayList (ordered, allows duplicates) and HashMap (key-value pairs).

Why it matters

Sometimes you don't care about order or position — you just need to know "have I seen this before?" or "give me only the unique values." HashSet handles duplicate removal automatically instead of you writing manual contains() checks in a loop.

The 80/20

TaskMethodExampleCreatenew HashSet<Type>()HashSet<String> names = new HashSet<>();Add.add(value)names.add("Harsh");Check membership.contains(value)names.contains("Harsh")Remove.remove(value)names.remove("Harsh")Size.size()names.size()

Duplicates are silently ignored

javaHashSet<String> names = new HashSet<>();
names.add("Harsh");
names.add("Harsh"); // ignored — already present
System.out.println(names.size()); // 1, not 2

.add() returns false if the element was already there, instead of throwing an error — it just quietly does nothing.

Quick contrast: ArrayList vs HashSet

FeatureArrayListHashSetOrderInsertion order preservedNo guaranteed orderDuplicatesAllowedNot allowedAccess by indexYes, .get(i)No — no indexing at allBest forOrdered sequencesFast uniqueness checks

All three (List, Set, Map) are interfaces — ArrayList implements List, HashSet implements Set — the same abstraction pattern from Day 11, where you code against the interface type when possible: Set<String> names = new HashSet<>();

Quick contrast (Python & JS)

LanguageUnique collectionDuplicate handlingPythonset()Silently ignores duplicatesJavaScriptSetSilently ignores duplicatesJavaHashSet<Type>Silently ignores duplicates

Same behavior across all three — Java just requires the type declaration up front, as usual.

Mental model

Think of a HashSet like a guest list at a private event checked by name only — if someone tries to sign in twice, the second attempt is just ignored, and there's no seating chart (no order, no index) — you can only ask "is this person on the list?"
