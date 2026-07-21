## File I/O

What it is

File I/O (input/output) means reading data from files into your program and writing data back out. Java's core tools: Scanner and BufferedReader for reading, FileWriter for writing.

Why it matters

Most real programs need to persist data beyond runtime — configs, logs, saved data. File operations can fail for reasons outside your control (missing file, no permissions, full disk), which is exactly why Java requires handling IOException — a direct callback to Day 12's checked exceptions.

The 80/20
Reading a file
java
Scanner reader = new Scanner(new File("data.txt"));
while (reader.hasNextLine()) {
    System.out.println(reader.nextLine());
}
reader.close();
Writing a file
java
FileWriter writer = new FileWriter("output.txt");
writer.write("Hello, file!");
writer.close(); // must close to actually save the data
Task Class Notes
Read line-by-line Scanner Simple, .hasNextLine() / .nextLine()
Read efficiently (large files) BufferedReader Faster for big files
Write text FileWriter Overwrites by default; new FileWriter(name, true) appends
Always .close() Releases the file — skipping this can lose unsaved writes
Why IOException is checked

File operations touch the outside world (disk, OS). That's exactly the case Day 12 flagged for checked exceptions — the compiler forces a try/catch or throws IOException, so a failed file operation can't be silently ignored.

Quick contrast (Python & JS)
Language Read a file Handle errors
Python with open("data.txt") as f: (auto-closes) Optional try/except
JavaScript fs.readFileSync() (Node) Optional try/catch
Java Scanner / BufferedReader Required try/catch or throws

Python's with auto-closes files. Java's equivalent is try-with-resources (try (Scanner reader = ...) {...}), which auto-closes without an explicit .close() call.

Mental model

A file is like a shared library notebook. Scanner/BufferedReader reads it page by page, FileWriter writes into it, and .close() returns it to the shelf — walk away without closing it, and your changes might not actually save.
