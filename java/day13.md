## Collections Framework — ArrayList & HashMap

What it is

The Collections Framework is Java's built-in library of resizable, ready-made data structures. The two you'll use constantly: ArrayList (a resizable list) and HashMap (key-value pairs). Both are backed by interfaces (List, Map) — a direct callback to Day 11's abstraction.

Why it matters

Day 5's arrays were fixed-size and single-type only. Real programs constantly need to grow/shrink collections and look things up by key — ArrayList and HashMap are the answer, and you'll use them far more often than raw arrays in everyday Java code.

The 80/20

ArrayList

TaskMethodExampleCreatenew ArrayList<Type>()ArrayList<String> names = new ArrayList<>();Add.add(value)names.add("Harsh");Get.get(index)names.get(0)Remove.remove(index)names.remove(0)Size.size()names.size()

HashMap

TaskMethodExampleCreatenew HashMap<KeyType, ValueType>()HashMap<String, Integer> ages = new HashMap<>();Add/update.put(key, value)ages.put("Harsh", 25);Get.get(key)ages.get("Harsh") → 25Check key exists.containsKey(key)ages.containsKey("Harsh")Remove.remove(key)ages.remove("Harsh")

The <Type> part — generics (just enough to use them)

javaArrayList<String> names = new ArrayList<>(); // only Strings allowed in
names.add("Harsh");   // fine
names.add(42);         // compile error — Integer isn't String

The angle brackets lock the collection to one type — this is what plain arrays couldn't do more flexibly (arrays are single-type too, but fixed-size on top of that).

Quick contrast (Python & JS)

ConceptPythonJavaScriptJavaResizable listlist (built-in)Array (built-in)ArrayList<Type>Key-value storedictObject / MapHashMap<K, V>Type-locked?NoNoYes, via generics

Python's list and dict are used identically in spirit — Java just requires you to declare the type upfront and wraps it in more verbose syntax.

Mental model

An ArrayList is like a stretchy expandable folder instead of Day 5's fixed-size lockers — it grows or shrinks as you add/remove papers. A HashMap is like a labeled filing cabinet where you look things up by a tag (the key) instead of a position number — you say "give me the folder labeled 'Harsh'" rather than "give me folder #3."
