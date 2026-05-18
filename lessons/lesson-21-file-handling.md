# Python for Hyperphantasic Minds
## Lesson 21 — File Handling

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 21 of 25  
> **Topic**: File Handling — crates at the loading bay  
> **Factory zone**: Z2 — Goods In, Z8 — The Records Department  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Today's lesson visits *two* buildings. **Goods In** (Z2) is the loading bay where material arrives from the outside world — files coming into your program. **The Records Department** (Z8) is the archive at the side of the warehouse — files written out for keeping. Both buildings deal in **crates**, which is the canonical Python image for a file.

```
                    ┌──────────────────────────────┐
                    │    SHIFT MANAGER'S OFFICE     │
                    └──────────────┬───────────────┘
                                   │
┌──────────┐                       ▼      ┌─────────────────┐
│          │        ┌─────────────────┐   │                 │
│TOOL STORE│───────▶│ ██ GOODS IN ██  │   │  FACTORY FLOOR  │
│          │        │ YOU ARE HERE    │   │                 │
└──────────┘        └────────┬────────┘   └────────┬────────┘
                             │                     │
                             ▼                     │
                  ┌──────────────────────┐         │
                  │      WAREHOUSE       │◀────────┘
                  └──────────┬───────────┘
                             │
              ┌──────────────┼─────────────┐
              ▼              ▼             ▼
      ┌──────────────┐  ┌─────────┐  ┌─────────────────┐
      │   QUALITY    │  │ TESTING │  │ ██ RECORDS ██   │
      │   CONTROL    │  │   LAB   │  │ YOU ARE HERE    │
      └──────┬───────┘  └─────────┘  └─────────────────┘
             │
             ▼
      ┌──────────────┐
      │  OUTGOINGS   │
      └──────────────┘
```

The two zones differ in *direction*. Goods In is incoming: a crate arrives at the loading door, is opened, its contents are read into the warehouse. The Records Department is outgoing: a package is built, labelled, and shelved into the permanent archive. The Python machinery for both is almost identical — only the *mode* of the operation differs.

---

## Crates at the Loading Bay

Picture a wide loading door at the side of the building. Crates arrive on lorries from the outside world. Each crate has a label printed on the side — its **filename and path**. A worker opens the crate, reads what is inside, and either feeds the contents into the warehouse or — if the program is about to write — labels and dispatches a fresh empty crate to be filled.

The canonical verb for opening a file is **receive a crate at the loading bay**. The Python workstation is `open()`:

```python
crate = open("notes.txt", "r")
```

Two materials are delivered to `open`:

- The filename (or path) — what label the crate has on its side.
- The mode — what you want to do with the crate.

What `open` sends back is a **crate-receipt** (formally, a *file handle*) — a small item on a shelf that represents the open crate. You use the receipt to read from or write to the underlying crate. The crate itself remains on the loading bay; the receipt is your interface with it.

---

## The Open Modes

A handful of mode codes cover almost all everyday use:

| Mode | Meaning |
|---|---|
| `"r"` | **Read** — open an existing crate for reading. Alarm if it does not exist. (Default if mode is omitted.) |
| `"w"` | **Write** — open a crate for writing, *erasing any existing contents*. Creates a new crate if none exists. |
| `"a"` | **Append** — open for writing, *adding to the end* of any existing contents. |
| `"r+"` | Open for both reading and writing; existing contents preserved. |
| `"b"` | (Suffix) Binary mode — the crate is read or written as raw bytes, not text. Used as `"rb"`, `"wb"`. |
| `"t"` | (Suffix) Text mode — the default. Rarely written explicitly. |

The most-used modes by far are `"r"`, `"w"`, and `"a"`. The binary suffix appears when you handle images, audio, compressed archives, or any non-text format.

A note on the destructiveness of `"w"`: it *erases existing contents the moment the crate is opened*, not when you write. If you `open("important.txt", "w")` and then immediately close it without writing anything, you have replaced the contents of `important.txt` with an empty crate. Use `"a"` when you mean *"add to the existing contents"* and `"w"` only when you mean *"start fresh"*.

---

## Reading the Crate

Several built-in tools on the crate-receipt let you read its contents in different shapes.

**`.read()` — the whole crate as one scroll.**

```python
crate = open("notes.txt", "r")
contents = crate.read()
crate.close()                       # always close — see the next section
print(contents)
```

For a small crate, this is the simplest read. For a large crate (gigabytes of text), this loads everything into memory at once — usually the wrong shape.

**`.readline()` — one line at a time.**

```python
crate = open("notes.txt", "r")
first_line = crate.readline()
second_line = crate.readline()
crate.close()
```

Each call advances a *cursor* inside the crate. Once the cursor reaches the end, subsequent calls return an empty scroll.

**`.readlines()` — every line as a row of scrolls.**

```python
crate = open("notes.txt", "r")
lines = crate.readlines()
crate.close()
print(lines)        # ['line 1\n', 'line 2\n', 'line 3\n']
```

Note that the newline characters are included on each line. This pattern is convenient but, again, loads the entire crate at once.

**Iteration — the idiomatic form.**

A crate-receipt is itself an iterable (Lesson 18). A `for` belt walks it line by line, holding only one line in memory at a time:

```python
crate = open("notes.txt", "r")
for line in crate:
    print(line.rstrip())            # strip the trailing newline
crate.close()
```

This is the standard pattern for *reading a file of any size*. The cursor advances one line per pass. The memory footprint stays small no matter how large the crate is.

---

## Closing the Crate — `.close()`

Every crate you open should be closed afterwards. `close()` releases the operating-system resources tied to it and ensures any buffered writes are flushed to disk:

```python
crate = open("notes.txt", "r")
contents = crate.read()
crate.close()
```

Forgetting `close()` is one of the most common beginner mistakes. The consequences depend on the operating system and what you were doing with the crate, but the worst cases are real:

- **On Windows**, an unclosed write-crate may keep the underlying file locked, preventing other programs from accessing it.
- **For writes**, contents that were buffered in memory may never reach disk if the program crashes before the crate is closed.
- **Eventually**, the operating system may refuse to open new crates if too many are left dangling. ("Too many open files.")

Even if you do remember `close()`, a problem remains: if anything between `open()` and `close()` triggers an alarm, the `close()` line is skipped entirely. The crate stays open.

This is enough of a problem that Python provides a structural fix.

---

## The Lockable Room — `with`

The canonical pattern for any operation that needs to be cleaned up afterwards — file handles, network connections, database transactions — is the **lockable room**. You walk into the room, do your work, and walk out. The door locks itself behind you automatically, *whether you walked out normally or were thrown out by an alarm*. The cleanup is guaranteed.

Python's keyword for entering a lockable room is `with`:

```python
with open("notes.txt", "r") as crate:
    contents = crate.read()
    print(contents)

# Outside the with block, the crate is closed — guaranteed.
```

Read this:

- `with open(...) as crate:` — enter the lockable room, with the crate-receipt assigned to a shelf labelled `crate` inside the room.
- The indented block runs with the crate open.
- The moment the indented block ends — for any reason, including an alarm — Python calls `close()` on the crate automatically.

This is the standard Python pattern for file handling. You will almost never see `open()` without `with` in real Python code. From this lesson on, every example uses `with`.

The pattern extends: you can open multiple crates in one `with` line:

```python
with open("source.txt", "r") as src, open("dest.txt", "w") as dst:
    for line in src:
        dst.write(line.upper())
```

Both crates are guaranteed to be closed, even if anything inside the block triggers an alarm.

---

## Writing — Sending Out a Package

Writes happen at the Records Department side of the building. The canonical verbs are **pack and dispatch** (for transient output) and **send a package to Records** (for permanent storage). At the Python level both are the same operation; the difference is only in what you do with the resulting crate afterwards.

**`.write()` — write a single scroll.**

```python
with open("output.txt", "w") as crate:
    crate.write("Hello, world!\n")
    crate.write("This is the second line.\n")
```

Note the explicit `\n` newlines. Unlike `print()` — which adds a newline automatically — `.write()` writes *exactly* what you give it. If you forget the newline, your lines run together on one line.

**`.writelines()` — write a row of scrolls.**

```python
lines = ["First line\n", "Second line\n", "Third line\n"]
with open("output.txt", "w") as crate:
    crate.writelines(lines)
```

Note the same quirk: `.writelines()` does *not* add newlines for you. Each item in the row must already end with `\n` if you want lines. The name is a bit deceptive — it just writes the items in sequence with no separator, *not* "writes each as a line".

**Append mode — adding to existing content.**

```python
with open("log.txt", "a") as crate:
    crate.write("New entry at the bottom.\n")
```

Each program run adds a new line at the bottom of `log.txt`. The existing contents are preserved. This is the standard pattern for log files.

---

## Text vs Binary

Everything above assumed text — scrolls of human-readable characters with one of several encodings (almost always UTF-8 by default). For non-text files — images, audio, ZIP archives, anything where the contents are raw bytes — append `"b"` to the mode:

```python
with open("photo.jpg", "rb") as crate:
    data = crate.read()
print(type(data))                   # <class 'bytes'>
```

The `bytes` item is a different kind of scroll — a *sealed scroll* in VOCABULARY's metaphor, holding raw byte values rather than characters. Operations are similar but not identical — no `.upper()`, for instance, because the contents are not characters.

Most beginner Python sticks to text mode. Recognise the `"b"` suffix when you see it; reach for it when you need it.

---

## Paths — Where Crates Live

So far we have used bare filenames like `"notes.txt"`, which Python resolves relative to wherever you ran the program. Real programs need to navigate folder structures, and Python provides two toolkits for this from the standard library.

The older one is `os.path`:

```python
import os.path

path = os.path.join("data", "users", "alice.txt")
print(path)                                      # data/users/alice.txt
print(os.path.exists(path))                      # True or False
print(os.path.dirname(path))                     # 'data/users'
print(os.path.basename(path))                    # 'alice.txt'
```

The newer, cleaner one is `pathlib`:

```python
from pathlib import Path

path = Path("data") / "users" / "alice.txt"
print(path)                                      # data/users/alice.txt
print(path.exists())                             # True or False
print(path.parent)                               # PosixPath('data/users')
print(path.name)                                 # 'alice.txt'
print(path.suffix)                               # '.txt'

# Open it directly via the Path:
with path.open("r") as crate:
    contents = crate.read()
```

`pathlib` treats paths as built workshops (Lesson 16's blueprints) with proper attributes and methods. The `/` operator joins path components. For new code, prefer `pathlib`; recognise `os.path` in older code.

---

## JSON — A Common Convenience

A common need: read or write structured data — cabinets and rows — as a file. The standard library's `json` toolkit handles this in two lines:

```python
import json

data = {
    "name": "Shane",
    "score": 27,
    "items": ["sword", "potion", "key"],
}

# Write to a file
with open("save.json", "w") as crate:
    json.dump(data, crate, indent=2)

# Read from a file
with open("save.json", "r") as crate:
    loaded = json.load(crate)

print(loaded)                       # the same cabinet, restored
```

`json.dump(data, crate)` writes the cabinet (or row, or scroll, or number) into the open crate as JSON text. `json.load(crate)` reads JSON text from the crate and rebuilds the corresponding Python items. The `indent=2` keyword makes the output human-readable rather than packed onto one line.

JSON is the standard format for configuration files, save files, and API responses. The `json` toolkit makes round-tripping data through a file a two-line operation.

For other formats:

- **CSV** — `csv` toolkit. The standard for spreadsheet-like data.
- **YAML** — third-party `pyyaml` toolkit. Common for configuration.
- **TOML** — `tomllib` in the standard library (Python 3.11+). Common in modern Python config (`pyproject.toml`).

All of these follow the same `open()`-with-`with` pattern. Once you have the file-handling pattern in your fingers, every format-specific toolkit slots into it the same way.

---

## What You Now Know

You can receive a crate at the loading bay with `open(path, mode)`, choose a mode based on what you intend to do (`"r"`, `"w"`, `"a"`, with `"b"` for binary), and read the contents in any of four shapes — whole crate, one line, all lines as a row, or one-at-a-time via iteration. You can write a crate with `.write()` or `.writelines()`, remembering that neither adds newlines for you.

You know that every open crate must be closed, and that the structural fix for this is the `with` lockable-room pattern — which guarantees the crate is closed when you leave the block, no matter how you leave. From now on, every file operation you write should use `with`.

You know `pathlib` is the modern way to construct and work with paths, and that the `json` toolkit makes reading and writing structured data trivial. The same `open`-with-`with` pattern underlies every file format Python supports.

The next lesson moves from receiving crates to **inspecting** them — error handling, the inspection belt at Quality Control.

---

## Quick Reference

| Python | Loading-bay / records image |
|---|---|
| `open(path, "r")` | Receive a crate at the loading bay for reading. |
| `open(path, "w")` | Open a write-crate; *erases existing contents*. |
| `open(path, "a")` | Open in append mode; existing contents preserved. |
| `open(path, "rb")` | Binary mode — raw bytes, not text. |
| `crate.read()` | Read everything as one scroll. |
| `crate.readline()` | Read one line; cursor advances. |
| `crate.readlines()` | Read all lines as a row of scrolls (with `\n`). |
| `for line in crate:` | Idiomatic — reads one line per pass; constant memory. |
| `crate.write(scroll)` | Write the scroll exactly; no automatic newline. |
| `crate.writelines(rows)` | Write each scroll in turn; no separator added. |
| `crate.close()` | Release the crate. Always required if you do not use `with`. |
| `with open(path) as crate:` | The lockable room — automatic close on exit, even via alarm. |
| `pathlib.Path("a") / "b"` | Build paths the modern way. |
| `json.dump(data, crate)` | Write structured data as JSON. |
| `json.load(crate)` | Read JSON back into Python items. |

---

## Try It

For these exercises, you can use any text file you like. The first block creates one for you to use in the others.

**Make a small text file:**

```python
with open("notes.txt", "w") as crate:
    crate.write("First line.\n")
    crate.write("Second line.\n")
    crate.write("Third line.\n")
```

**Read it whole:**

```python
with open("notes.txt", "r") as crate:
    print(crate.read())
```

**Read line by line — the idiomatic form:**

```python
with open("notes.txt", "r") as crate:
    for line in crate:
        print(line.rstrip())        # strip the trailing newline
```

**Read all lines into a row:**

```python
with open("notes.txt", "r") as crate:
    lines = crate.readlines()
print(lines)
print(len(lines))
```

**Append:**

```python
with open("notes.txt", "a") as crate:
    crate.write("Fourth line, added later.\n")

# Read the file again to see the appended line:
with open("notes.txt", "r") as crate:
    print(crate.read())
```

**Copy a file line by line, transforming as you go:**

```python
with open("notes.txt", "r") as src, open("notes-upper.txt", "w") as dst:
    for line in src:
        dst.write(line.upper())

with open("notes-upper.txt", "r") as crate:
    print(crate.read())
```

**Round-trip a Python structure through JSON:**

```python
import json

data = {
    "name": "Shane",
    "score": 27,
    "items": ["sword", "potion"],
}

with open("save.json", "w") as crate:
    json.dump(data, crate, indent=2)

with open("save.json", "r") as crate:
    loaded = json.load(crate)

print(loaded)
print(loaded["items"])
```

**Use pathlib for paths:**

```python
from pathlib import Path

p = Path("notes.txt")
print(p.exists())
print(p.name)
print(p.suffix)
print(p.absolute())

with p.open("r") as crate:
    print(crate.read())
```

**See an alarm — open a file that does not exist:**

```python
with open("does-not-exist.txt", "r") as crate:
    print(crate.read())            # FileNotFoundError
```

This is the natural lead-in to the next lesson: handling alarms at Quality Control.

---

## Where Next?

The next lesson tackles what to do when something goes wrong on the production line — the **inspection belt** at Quality Control, where alarms are caught and handled rather than allowed to halt the shift. After that, two more zones — the Records Department for databases, and Goods In in depth for user input — before the closing lesson in the Shift Manager's Office.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 22 | Z7 — Quality Control | Error Handling — the inspection belt |
| Lesson 23 | Z8 — Records Dept | Databases — the permanent archive |
| Lesson 24 | Z2 — Goods In | User Input in Depth — work orders and terminals |
| Lesson 25 | Z4 — Shift Manager's Office | Putting It Together — the complete shift |

*See the full lesson map in **The Factory — A Complete Map**.*
