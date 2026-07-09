## Arrays (Java)

What it is

An array is a fixed-size, ordered collection of elements of the same type. Once created, its size can never change — that's the biggest mental shift from Python lists or JS arrays.

Why it matters

Arrays are the foundation for almost everything structured in Java — loops from Day 4 exist largely to walk through them. Knowing the fixed-size constraint upfront avoids the confusing ArrayIndexOutOfBoundsException that trips up beginners expecting Python/JS-style dynamic resizing.

The 80/20

TaskSyntaxExampleDeclare + sizetype[] name = new type[size];int[] nums = new int[5];Declare + valuestype[] name = {v1, v2, ...};int[] nums = {1, 2, 3};Access elementname[index]nums[0] → first elementUpdate elementname[index] = value;nums[0] = 10;Get lengthname.length (no parens!)nums.length → 3Loop throughfor-each (from Day 4)for (int n : nums) {...}

The pitfall

javaint[] nums = {1, 2, 3};
System.out.println(nums[3]); // ArrayIndexOutOfBoundsException

Valid indices are 0 to length - 1. There's no negative indexing like Python's list[-1].

2D arrays (brief)

javaint[][] grid = new int[3][3];   // 3x3 grid, all zeros
grid[0][1] = 5;                 // row 0, column 1

Think rows-then-columns — same order as most tables.

Quick contrast (Python & JS)

LanguageFixed size?LengthMixed types allowed?PythonNo (lists resize)len(nums)YesJavaScriptNo (arrays resize)nums.lengthYesJavaYesnums.length (property, no ())No — single type only

If you need resizable/mixed-type behavior in Java, that's what ArrayList (coming later) is for — arrays are the strict, fixed-size baseline.

Mental model

A Java array is like a row of numbered lockers bolted to a wall — fixed count, fixed size, all built for the same kind of item. You can swap what's inside a locker, but you can't add a locker #6 to a 5-locker row.
