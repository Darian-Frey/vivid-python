# Python for Hyperphantasic Minds
## Lesson 25 — Putting It Together

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 25 of 25  
> **Topic**: Putting It Together — the complete shift  
> **Factory zone**: Z4 — The Shift Manager's Office  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

The final lesson. You visit the one building you have not yet entered — **the Shift Manager's Office**, sitting at the top of the complex above all the other zones. From its window, the entire factory is visible.

```
                    ┌──────────────────────────────┐
                    │ ██ SHIFT MANAGER'S OFFICE ██ │
                    │       YOU ARE HERE           │
                    └──────────────┬───────────────┘
                                   │
┌──────────┐                       ▼      ┌─────────────────┐
│          │        ┌──────────┐          │                 │
│TOOL STORE│───────▶│ GOODS IN │          │  FACTORY FLOOR  │
│          │        │          │          │                 │
└──────────┘        └────┬─────┘          └────────┬────────┘
                         │                         │
                         ▼                         │
              ┌──────────────────────┐             │
              │      WAREHOUSE       │◀────────────┘
              └──────────┬───────────┘
                         │
          ┌──────────────┼─────────────┐
          ▼              ▼             ▼
  ┌──────────────┐  ┌─────────┐  ┌──────────┐
  │   QUALITY    │  │ TESTING │  │ RECORDS  │
  │   CONTROL    │  │   LAB   │  │  DEPT    │
  └──────┬───────┘  └─────────┘  └──────────┘
         │
         ▼
  ┌──────────────┐
  │  OUTGOINGS   │
  └──────────────┘
```

You have now seen every building. The factory is, in your mind, a complete world.

This lesson does three things:

- **Introduces the Shift Manager's Office** — the program's entry point, where the shift begins and ends.
- **Explains the script-vs-library boundary** — the `if __name__ == "__main__":` convention you have been seeing since Lesson 20.
- **Closes with a complete worked example** — a small but real program that touches every zone on the main production line: Goods In, Warehouse, Factory Floor, Quality Control, Outgoings, and Records.

After this, the advanced phases (Testing Lab, CCTV Room, Night Shift Wing) add professional habits and concurrency. The fundamentals are complete.

---

## The Shift Manager's Office

Picture a small administrative building above the factory floor — a corner office with windows looking out over every zone. The Shift Manager sits at a desk. Their job is straightforward but vital:

- **At the start of the shift**, the Shift Manager reads the day's work order — the command-line arguments, the environment, the configuration — and decides what work the factory will do.
- **During the shift**, the Office is hands-off. The Manager does not run the workstations. The Floor, the warehouse, and the supporting buildings do the work.
- **At the end of the shift**, the Manager files the day's report and signs off. The shift ends; the forklift clears the warehouse; the factory goes quiet.

In Python, the Shift Manager's Office corresponds to the *entry point* of your program — the small region at the bottom of the main file where execution begins, drives the workstations, and ends cleanly.

---

## The Shift's Beginning and End

The canonical verbs from VOCABULARY are simple:

- **The shift begins** — when Python starts running your file.
- **The shift ends** — when Python finishes running it.

What happens during a shift depends entirely on what the file contains. Without an entry point, a Python file is just a sequence of statements: imports, definitions, top-level code, possibly some calls. Python runs them top to bottom and stops. That is a shift.

The interesting question is *what should top-level code actually do?* And the answer in real Python programs is *as little as possible*. All the *definitions* — workstations, blueprints, constants — live at the top level. The *actual work* lives inside a single workstation called `main()`, which top-level code calls at the very end.

```python
def main():
    # the actual shift's work
    ...


if __name__ == "__main__":
    main()
```

This is the standard shape of every nontrivial Python program. The body of `main()` is the shift; everything above it is the *blueprint of the shift*. The `if __name__ == "__main__":` line is the *trigger* that says "if this file is the program being run, send a job order to main()".

Let's break that line down.

---

## `if __name__ == "__main__":` — The Boundary

A Python file is two things at once:

- **A program** that can be run directly. `python myfile.py` runs the file's top-level code from top to bottom.
- **A toolkit** that can be fetched by other files (Lesson 20). `import myfile` runs the file's top-level code *once* — to define its workstations and constants — then makes those definitions available to the importer.

Both modes run the file's top-level code. The difference: when run directly, you usually want to *do work* — process arguments, call workstations, produce output. When imported, you want to *expose definitions*, not silently start doing work in the importer's process.

The `__name__` shelf is Python's way of telling the file which mode it is in:

- When run directly with `python myfile.py`, `__name__` is set to the scroll `"__main__"`.
- When imported as a toolkit by `import myfile`, `__name__` is set to the scroll `"myfile"` — the toolkit's name.

So the guard:

```python
if __name__ == "__main__":
    main()
```

reads as: *"if this file is being run directly, call `main()`; otherwise (we are being imported) do not."* The work happens only when the file is the program; the work is silent when the file is being imported for its definitions.

Picture: the Shift Manager looks at the work order at the start of the shift and asks, *"am I in charge today, or am I just a supplier of tools to another factory?"* If in charge, run `main()`. If a supplier, stay quiet.

This is a convention, not a Python rule. Code without the guard still works when run directly. But code without the guard runs its top-level work *whenever it is imported*, which is almost never what you want. The guard is universal in real Python code.

---

## The `main()` Workstation

There is nothing magical about the name `main`. It is a convention, and Python does not enforce it. But every Python programmer recognises it, and using the same name makes your program instantly readable to anyone familiar with the language.

A well-shaped `main()`:

- **Reads input** — command-line arguments (Lesson 24), environment variables, sometimes interactive prompts.
- **Calls workstations** that do the actual work.
- **Handles top-level alarms** — the outermost `try/except` for anything that escapes lower levels.
- **Returns nothing important**, or returns an exit code stone (`0` for success, non-zero for failure) that can be passed to `sys.exit`.

The shape:

```python
def main():
    args = parse_args()
    try:
        do_the_work(args)
    except SomeRecoverableError as e:
        print(f"Failed: {e}", file=sys.stderr)
        sys.exit(1)


if __name__ == "__main__":
    main()
```

For very small scripts, `main()` may be a single line — `do_the_work(parse_args())` — and that is fine. For larger programs, `main()` is the orchestrator; the substance lives in workstations and workshop blueprints elsewhere in the file or in other toolkits.

---

## Program Structure — Where Things Live

A well-organised Python file follows a conventional layout from top to bottom. You will see this shape in essentially every Python file:

```python
#!/usr/bin/env python3
"""A short module docstring describing what this file is for."""

# 1. Imports (Lesson 20)
import argparse
import os
import sys
from pathlib import Path

# 2. Constants (Lesson 1's UPPER_CASE convention)
DEFAULT_OUTPUT = "report.txt"
MAX_RETRIES = 3

# 3. Workshop blueprints (Lesson 16)
class Report:
    def __init__(self, title):
        self.title = title
        self.lines = []
    # ... methods ...

# 4. Workstations (Lesson 8)
def parse_args():
    ...

def do_the_work(args):
    ...

# 5. main()
def main():
    args = parse_args()
    do_the_work(args)

# 6. The entry-point guard
if __name__ == "__main__":
    main()
```

A few notes:

- **The first line** (`#!/usr/bin/env python3`) is optional but conventional for scripts intended to be run directly without `python` in front. It tells the operating system how to run the file.
- **The module docstring** (a triple-quoted scroll at the very top) is the toolkit's introduction — printed by `help(modulename)` and used by documentation generators.
- **Imports are alphabetised by convention** within their groups, and the `import x` form sits above the `from x import y` form within each group.
- **The layout reads top-to-bottom** — toolkits brought in, constants placed, blueprints drawn, workstations built, main() defined last, then the trigger.

For programs that grow beyond a single file, this same layout applies to each individual file, with the *additional* structure of organising files into a package (Lesson 20's `mypackage/` folder pattern).

---

## A Complete Worked Example

A small but realistic program — a word-frequency counter — that touches every main-line zone. The brief:

- Read a text file's path from the command line.
- Count how often each word appears.
- Write the top results to an output file, and print a summary to the console.
- Handle missing files, empty files, and bad command-line arguments gracefully.

The complete program:

```python
#!/usr/bin/env python3
"""Count word frequencies in a text file."""

import argparse
import sys
from collections import Counter
from pathlib import Path


DEFAULT_TOP = 10


def read_text(path):
    """Receive a crate at the loading bay and return its contents as a scroll."""
    with path.open("r", encoding="utf-8") as crate:
        return crate.read()


def count_words(text):
    """Return a filing cabinet of word → count."""
    cleaned = (text
               .lower()
               .replace(",", " ")
               .replace(".", " ")
               .replace(";", " ")
               .replace(":", " ")
               .replace("!", " ")
               .replace("?", " "))
    words = cleaned.split()
    return Counter(words)


def write_report(path, counts, top_n):
    """Send a package to Records — write the top results as a numbered table."""
    with path.open("w", encoding="utf-8") as crate:
        crate.write(f"Top {top_n} most frequent words:\n\n")
        for word, count in counts.most_common(top_n):
            crate.write(f"{word:20s} {count}\n")


def parse_args():
    parser = argparse.ArgumentParser(description="Count word frequencies in a text file.")
    parser.add_argument("input", type=Path, help="Path to the input file.")
    parser.add_argument("--output", type=Path, default=Path("word-report.txt"),
                        help="Where to write the report (default: word-report.txt).")
    parser.add_argument("--top", type=int, default=DEFAULT_TOP,
                        help=f"How many of the most frequent words to keep (default: {DEFAULT_TOP}).")
    return parser.parse_args()


def main():
    args = parse_args()

    try:
        text = read_text(args.input)
    except FileNotFoundError:
        print(f"Input file not found: {args.input}", file=sys.stderr)
        sys.exit(1)
    except PermissionError:
        print(f"No permission to read: {args.input}", file=sys.stderr)
        sys.exit(1)

    if not text.strip():
        print(f"Warning: {args.input} is empty.", file=sys.stderr)
        sys.exit(1)

    counts = count_words(text)
    write_report(args.output, counts, args.top)

    print(f"Counted {sum(counts.values())} words, {len(counts)} unique.")
    print(f"Report saved to {args.output}.")
    print()
    print(f"Top {args.top}:")
    for word, count in counts.most_common(args.top):
        print(f"  {word:20s} {count}")


if __name__ == "__main__":
    main()
```

Walk this through the factory:

- **Z1 — Tool Store.** The `import` block at the top fetches four toolkits: `argparse`, `sys`, `collections.Counter` (a specialised filing cabinet for counting), and `pathlib.Path`.
- **Z2 — Goods In.** `parse_args()` reads the work order. `read_text()` receives a crate at the loading bay. Both are concentrated in the Goods In zone.
- **Z3 — Warehouse.** Almost every line places named cubbyholes — `text`, `counts`, `args`, the per-iteration `word` and `count`. The `cleaned` scroll, the `words` numbered row, the `counts` filing cabinet — all live on warehouse shelves.
- **Z5 — Factory Floor.** Three workstations (`read_text`, `count_words`, `write_report`) do the work. `count_words` uses chained scroll tools (`.lower()`, `.replace(...)`, `.split()`) and the `Counter` workshop blueprint, which is a built-in filing-cabinet variant for counting.
- **Z7 — Quality Control.** The `try/except` block in `main()` catches `FileNotFoundError` and `PermissionError` — converting them into clean error messages and a non-zero exit code. The empty-file check is a manual validation alarm, called out cleanly with `sys.exit(1)`.
- **Z8 — Records Department.** `write_report` sends a package — the formatted text report — to the output crate at `args.output`.
- **Z11 — Outgoings.** The closing `print()` block calls out a summary across the floor: total words, unique words, top results.
- **Z4 — Shift Manager's Office.** `main()` orchestrates the whole shift; the `if __name__ == "__main__":` guard fires it.

Eight zones, one program, ~80 lines. This is the natural shape of a small Python tool.

Run it like this:

```
$ python wordcount.py article.txt
Counted 2347 words, 712 unique.
Report saved to word-report.txt.

Top 10:
  the                  124
  a                    78
  ...
```

The script is small enough to understand at a glance and big enough to be a real, useful tool. From here, every program you write follows the same architecture.

---

## What the Map Looks Like Now

You have visited every zone in the factory:

| Zone | Name | Where met |
|---|---|---|
| Z1 | Tool Store | Lesson 20 |
| Z2 | Goods In | Lessons 21, 24 |
| Z3 | Warehouse | Lessons 1, 3, 4, 6, 7, 9, 10, 11 |
| Z4 | Shift Manager's Office | This lesson |
| Z5 | Factory Floor | Lessons 5, 8, 12–19 |
| Z6 | Testing Laboratory | Advanced Phase 5 (next) |
| Z7 | Quality Control | Lesson 22 |
| Z8 | Records Department | Lessons 21, 23 |
| Z9 | CCTV Room | Advanced Phase 5 (next) |
| Z10 | Night Shift Wing | Advanced Phase 6 |
| Z11 | Outgoings | Lesson 2 |

Eight of the eleven zones have been fully covered. The remaining three are the *advanced* phases — habits and capabilities that make working Python *professional*, but are not strictly required for writing useful programs.

---

## What's Next — The Advanced Phases

The advanced phases are organised as two pairs:

**Phase 5 — Advanced Systems** (two lessons)

- **Advanced 5a — Testing** (Z6 Testing Laboratory). The Lab is the building beside the Floor where new workstations are validated before going into production. `pytest`, `assert`, station checks. Writing tests is the difference between hoping your program works and *knowing* it works.
- **Advanced 5b — Logging** (Z9 CCTV Room). The CCTV Room watches every zone and writes down what it sees. `logging` is the standard tool for structured, severity-tagged messages — far more useful than scattered `print()` calls when you need to debug a running system.

**Phase 6 — Concurrency** (two lessons)

- **Advanced 6a — Threading** (Z10 Night Shift Wing). The Night Shift Wing has two parallel production lines running at once. `threading.Thread` and the perils of shared warehouse shelves.
- **Advanced 6b — Async** (Z10 Night Shift Wing). A single worker who never sits idle — switching between many waiting tasks. `async` / `await` and the modern Python pattern for concurrent I/O.

Phase 6 is deliberately last. Concurrency is genuinely harder than everything else, and approaching it before the rest of the factory is comfortable is a common path to confusion. Wait until the main factory feels natural before you try the Night Shift.

---

## What You Now Know

You have walked through every zone of the factory. You have seen the warehouse from the inside, built workstations and workshops on the Factory Floor, dispatched goods through Outgoings, received crates at Goods In, sent packages to Records, caught alarms at Quality Control, fetched toolkits from the Tool Store, and today, finally, stood in the Shift Manager's Office.

You can structure a Python program from imports to `main()` and the `if __name__ == "__main__":` guard. You can read input from a terminal, the work order, environment variables, files, and databases. You can do work on the Floor — arithmetic, comparisons, junctions, conveyor belts, comprehensions, blueprints, inheritance, generators, decorators. You can write output to the console, to files, to databases. You can catch and handle alarms.

This is the full shape of a Python program. Every Python tool you encounter from here on slots into the same factory — different shelves, different workstations, but the same building.

**The factory is one world. You can walk around in it.**

---

## Quick Reference

| Python | Shift Manager's Office image |
|---|---|
| `if __name__ == "__main__":` | The shift-start trigger — run `main()` only when this file is the program. |
| `def main():` | The shift's orchestrator workstation. By convention. |
| `sys.exit(0)`, `sys.exit(1)` | End the shift with success (0) or failure (non-zero). |
| `print(..., file=sys.stderr)` | Print to the error stream, not the dispatch bay. |
| File header order | `#!` line, module docstring, imports, constants, blueprints, workstations, `main()`, guard. |
| The `__name__` shelf | `"__main__"` when run directly; the toolkit's name when imported. |

---

## Try It

**The shape of a minimal program:**

```python
# minimal.py
def main():
    print("Hello from main()")

if __name__ == "__main__":
    main()
```

Run it directly:

```
$ python minimal.py
Hello from main()
```

Now demonstrate the import behaviour. From a Python prompt:

```
>>> import minimal
>>>
```

Nothing is printed. The guard prevents `main()` from running on import. The `minimal.main` workstation is now available as a fetched item, ready to be called explicitly:

```
>>> minimal.main()
Hello from main()
```

This is the script-vs-library distinction in action.

**A worked example without the guard — see what goes wrong:**

```python
# greet.py
def greet(name):
    return f"Hello, {name}"

# No guard — this runs every time
print("greet.py is doing something at import time!")
```

Now write a second file that imports it:

```python
# uses_greet.py
import greet

print(greet.greet("Alice"))
```

Run `python uses_greet.py`. You will see:

```
greet.py is doing something at import time!
Hello, Alice
```

The print from `greet.py` appeared during the import — almost certainly not what you wanted. Now wrap the print in a `main()` and a guard, and re-run:

```python
# greet.py — fixed
def greet(name):
    return f"Hello, {name}"

def main():
    print("greet.py is being run as a program!")

if __name__ == "__main__":
    main()
```

`python uses_greet.py` is now silent on import; only the `print(greet.greet("Alice"))` line produces output. The shape is correct.

**Type and run the word-counter example.** Save the full program above as `wordcount.py`. Make a small text file `sample.txt`:

```
The quick brown fox jumps over the lazy dog.
The dog sleeps. The fox runs.
```

Run it:

```
$ python wordcount.py sample.txt --top 5
```

You should see a count of the words and a `word-report.txt` file in the current folder. Try variations:

- A file that doesn't exist (`python wordcount.py missing.txt`) — feel the clean error message.
- A file that is empty (create one and try it) — feel the empty-file check.
- `--help` for the auto-generated documentation.

This is the shape of every Python tool you will write from here. Build the structure once; the work fits inside.

---

## Where Next?

The main series ends here. The advanced phases are next, and after that you are no longer following a tutorial — you are writing Python.

| Next lesson | Zone | Topic |
|---|---|---|
| Advanced 5a | Z6 — Testing Laboratory | Testing — station checks with pytest |
| Advanced 5b | Z9 — CCTV Room | Logging — permanent records from every zone |
| Advanced 6a | Z10 — Night Shift Wing | Threading — two lines running in parallel |
| Advanced 6b | Z10 — Night Shift Wing | Async — one worker, no idle time |

*See the full lesson map in **The Factory — A Complete Map**.*

---

*The factory is one world. Walk around in it.*
