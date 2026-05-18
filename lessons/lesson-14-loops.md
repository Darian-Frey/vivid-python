# Python for Hyperphantasic Minds
## Lesson 14 — Loops

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 14 of 25  
> **Topic**: Loops — conveyor belts, emergency stops, skip gates  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still on the Factory Floor. The junction (Lesson 13) sends material down one path or another, *once*. The next piece of control flow does the opposite — it sends *many* items down the *same* path, one after another. That construct is the **conveyor belt**.

```
                    ┌──────────────────────────────┐
                    │    SHIFT MANAGER'S OFFICE     │
                    └──────────────┬───────────────┘
                                   │
┌──────────┐                       ▼      ┌─────────────────────────┐
│          │        ┌──────────┐          │                         │
│TOOL STORE│───────▶│ GOODS IN │          │  ██ FACTORY FLOOR ██    │
│          │        │          │          │                         │
└──────────┘        └────┬─────┘          │      YOU ARE HERE       │
                         │                └────────────┬────────────┘
                         ▼                             │
              ┌──────────────────────┐                 │
              │      WAREHOUSE       │◀────────────────┘
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

Loops are how any real program with data does its work. You almost never want to write the same calculation a thousand times by hand; you put a thousand items on a belt and let the workstation process each one in turn.

---

## The Conveyor Belt — Physical Setup

Picture a circular conveyor that takes items in at one end and feeds them, one at a time, to a workstation in the middle. Each item that arrives at the workstation gets the same procedure applied to it. When the belt has nothing left to feed, it stops; the workstation goes idle and the procedure moves on to whatever follows the belt.

Python offers two kinds of belts:

- **The conveyor belt with counter** (`for`) — a belt loaded in advance with items from a collection. The belt runs once per item, then stops automatically. Called the *counter belt* for short.
- **The conveyor belt with sensor** (`while`) — a belt that keeps running as long as a sensor at the side reads its switch as `True`. It does not know how many items will go past; it just keeps running until the sensor's switch falls. Called the *sensor belt* for short.

Two emergency tools sit beside every belt:

- **The emergency stop** (`break`) — a large red button that halts the belt immediately, regardless of how many items remain.
- **The skip gate** (`continue`) — a panel partway through a pass that lets the current item drop through and rejoin the queue, skipping the rest of that pass's work.

That is the whole anatomy. The rest of the lesson is detail.

---

## The Conveyor Belt With Counter — `for`

The most common belt. Feed it a collection; it walks every item once.

```python
names = ["Alice", "Bob", "Cara"]

for name in names:
    print("Hello,", name)
```

Read this:

- `for name in names:` — load the belt with the items of `names`. Each pass, the next item is placed in the slot named `name`.
- `:` — the indented block is the workstation's procedure, applied to each item that arrives.
- `    print("Hello,", name)` — the procedure. Indented by four spaces; runs once per item.

Output:

```
Hello, Alice
Hello, Bob
Hello, Cara
```

Three items on the belt; the procedure runs three times. The slot `name` is reset each pass — on the first pass it holds `"Alice"`, on the second `"Bob"`, on the third `"Cara"`. When the belt is empty, it stops; control rejoins the main conveyor below the indented block.

The slot's name is yours to choose. `for x in names:` works identically; `for name in names:` is more readable.

---

## Counting Without a Collection — `range()`

Sometimes you want the belt to run a specific number of times, not to walk a collection you already have. Python's standard kit provides `range()` for this — a small workstation that produces a sequence of stones on demand.

```python
for i in range(5):
    print(i)
```

Output: `0 1 2 3 4`. `range(5)` produces the stones `0`, `1`, `2`, `3`, `4` — five stones starting from zero. The belt walks them in order.

`range` accepts up to three arguments:

```python
for i in range(2, 8):
    print(i)         # 2 3 4 5 6 7         — start, stop

for i in range(0, 20, 5):
    print(i)         # 0 5 10 15           — start, stop, step

for i in range(10, 0, -1):
    print(i)         # 10 9 8 ... 1        — counting down
```

The pattern is `range(start, stop, step)`. `start` defaults to `0`, `step` defaults to `1`. The stop value is **exclusive** — `range(5)` produces values up to but not including `5`. Same end-exclusive rule you saw in slicing in Lesson 6.

`range` is the everyday tool for *"run this procedure N times"* and for *"walk a sequence of numbers"*. You will see it in almost every program that does any quantitative work.

---

## Walking Over Each Kind of Collection

A `for` belt knows how to walk every collection item in the warehouse. Each kind has its own way of yielding contents.

**Numbered row.** Each cubbyhole's item, in order:

```python
for score in [10, 20, 30]:
    print(score)         # 10 20 30
```

**Scroll.** Each character, in order:

```python
for c in "abc":
    print(c)             # a b c
```

A scroll is a sequence of single-character scrolls. The belt walks them one at a time.

**Sealed crate.** Same as a numbered row:

```python
for x in (1, 2, 3):
    print(x)             # 1 2 3
```

**Unsorted bin.** Each item, in *some* order (not the insertion order — bins have no order to preserve):

```python
for tag in {"new", "urgent", "frontend"}:
    print(tag)
```

The order of output depends on Python's internal arrangement. Do not write a program whose behaviour depends on the bin walk's order — that is exactly what a bin promises *not* to provide.

**Filing cabinet.** A bare `for` over a cabinet walks the **keys**:

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}

for label in prices:
    print(label)         # apple bread milk
```

This is a common surprise — beginners expect to see values, but the cabinet's natural walk is over its labels. Three explicit forms cover all three needs:

```python
for label in prices.keys():
    print(label)                    # apple bread milk

for value in prices.values():
    print(value)                    # 0.5 2.4 1.2

for label, value in prices.items():
    print(label, value)             # apple 0.5  bread 2.4  milk 1.2
```

The last form uses the sealed-crate unpacking trick from Lesson 11 — each item yielded by `.items()` is a two-element crate, unpacked into `label` and `value` slots at the start of each pass.

Filing-cabinet iteration with `.items()` is one of the most-used Python patterns. Most code that processes structured data walks a cabinet this way.

---

## The Conveyor Belt With Sensor — `while`

A `for` belt is loaded in advance with a known set of items. A `while` belt is different — it does not know in advance how many passes there will be. A sensor at the side reads a switch at the start of each pass. If the switch is up, the belt runs another pass. If it falls, the belt stops.

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

Read this:

- `while count < 5:` — check the sensor. The comparison produces a switch; `True` means *another pass*; `False` means *stop*.
- The indented block runs once per pass.
- `count += 1` — without this, the sensor's switch would never fall.

Output: `0 1 2 3 4`. Same result as `for i in range(5)`, but the structure is different. `for` is "walk these known items". `while` is "keep going until the condition fails".

**When to choose which.**

- `for` is the right choice when you have a collection or a known number of passes. It is shorter, harder to get wrong, and clearer at a glance.
- `while` is the right choice when the stopping point depends on something that happens *inside* the loop — a user pressing a key, a value being read from a network, a calculation converging on a result.

A common `while` pattern: keep going while a flag is up.

```python
running = True
while running:
    response = input("Type something (or 'quit' to stop): ")
    if response == "quit":
        running = False
    else:
        print("You said:", response)
```

The sensor reads the `running` switch. The body of the belt sets it to `False` when the user types `quit`, and the belt stops on the next sensor check.

---

## The Infinite-Loop Trap

The most dangerous mistake with a `while` belt is forgetting to make the sensor's switch ever fall:

```python
count = 0
while count < 5:
    print(count)
    # forgot count += 1 — the belt runs forever
```

The belt prints `0` forever, never advancing. Your program looks frozen; the terminal pours out zeros.

The recovery in most terminals is **Ctrl-C** — Python catches the interrupt and stops the program. It is your emergency-stop-from-the-outside, and you will use it more than you would like as a beginner.

The fix in code is always the same: make sure *something* in the body of the belt changes the value the sensor reads. Either the loop variable advances, or a flag flips, or a value is read from a source that will eventually return a stop signal. If the sensor's input never changes, the belt never stops.

---

## The Emergency Stop — `break`

Sometimes you need to halt the belt early — partway through walking a collection, on a specific condition, before the belt would naturally have stopped. `break` is the large red button that does this:

```python
for n in [10, 20, 30, -5, 40]:
    if n < 0:
        print("Found a negative:", n)
        break
    print("processed:", n)
```

Output:

```
processed: 10
processed: 20
processed: 30
Found a negative: -5
```

Picture the belt walking through five items. On the fourth pass, the inspection gate inside the body opens (the `if n < 0:`), the `break` is hit, and the belt halts immediately. The fifth item (`40`) is still on the belt, but it is never processed.

`break` is most useful for *search* — *"walk until you find what you are looking for, then stop"*:

```python
for word in words:
    if word.startswith("secret"):
        found = word
        break
```

After the loop, `found` holds either the first matching word or — if no match — whatever it was before the loop (usually `None` or unset; a common pattern is to initialise it explicitly).

`break` works the same way inside both `for` and `while` belts. Its job is the same: halt this belt immediately.

---

## The Skip Gate — `continue`

Where `break` halts the belt, `continue` skips just the current pass. The current item drops through a partway gate; the belt does not stop, it simply moves on to the next item.

```python
for n in [10, 20, -5, 30, -1, 40]:
    if n < 0:
        continue           # skip negative numbers
    print(n)
```

Output: `10 20 30 40`. The negative items hit the skip gate and are dropped without being processed. The positive items run through the full body.

`continue` is most useful when the early part of the body is a *filter* — *"skip the items I do not want; process the rest as normal"*. The alternative is to wrap the entire body in `if condition:`, but `continue` keeps the indentation shallower and the intent clearer when the body is long:

```python
for record in records:
    if not record.active:
        continue                    # skip inactive records
    if record.error_count > 10:
        continue                    # skip records with too many errors
    process(record)                 # the real work, no nesting needed
```

Compare with the deeply-nested alternative — the `continue` form reads much more cleanly.

Like `break`, `continue` works inside both belt types.

---

## The Loop's `else` Clause — A Python Oddity

Both `for` and `while` belts can have an `else` clause that runs *when the belt finishes naturally*. If the belt stops because of `break`, the `else` is **skipped**. If the belt runs to completion without `break`, the `else` runs.

```python
numbers = [10, 20, 30, 40]

for n in numbers:
    if n < 0:
        print("Found a negative!")
        break
else:
    print("All non-negative.")
```

Output: `All non-negative.` — the belt walked every item without hitting the `break`, so the `else` ran.

Try it again with a negative number in the row:

```python
numbers = [10, 20, -5, 40]

for n in numbers:
    if n < 0:
        print("Found a negative!")
        break
else:
    print("All non-negative.")
```

Output: `Found a negative!` — the `break` fired, so the `else` was skipped.

This is one of Python's more unusual features. It is most useful for "search loops" where the natural meaning is *"did we make it all the way through without finding anything?"*. The same logic could be expressed with a separate `found` flag and an `if` after the loop, and many Python programmers prefer that for clarity. The `else` is here when you need it; do not feel obliged to use it.

---

## Index and Item Together — `enumerate()`

Sometimes the procedure inside the belt needs both the item *and* its position. The naive approach is to walk an index range and pull out the item manually:

```python
names = ["Alice", "Bob", "Cara"]
for i in range(len(names)):
    print(i, names[i])
```

This works, but Python's standard kit provides a cleaner workstation for this: `enumerate()`. Wrap the collection with it and the belt yields `(index, item)` crates each pass:

```python
for i, name in enumerate(names):
    print(i, name)
```

Output:

```
0 Alice
1 Bob
2 Cara
```

`enumerate(names)` produces a sequence of two-element sealed crates: `(0, "Alice")`, `(1, "Bob")`, `(2, "Cara")`. The crate is unpacked at the start of each pass into `i` and `name`. Cleaner than the index-then-look-up dance.

`enumerate` accepts an optional second argument for the starting index — useful when humans expect counting from `1`:

```python
for n, name in enumerate(names, start=1):
    print(f"{n}. {name}")
# 1. Alice
# 2. Bob
# 3. Cara
```

---

## Walking Two Collections in Parallel — `zip()`

When two collections have a one-to-one relationship — names and scores, keys and values, x-coordinates and y-coordinates — you can walk them in parallel with `zip()`:

```python
names = ["Alice", "Bob", "Cara"]
scores = [100, 75, 90]

for name, score in zip(names, scores):
    print(f"{name}: {score}")
```

Output:

```
Alice: 100
Bob: 75
Cara: 90
```

`zip` produces sealed crates pairing up the first items, then the second items, then the third, and so on. If the collections are different lengths, `zip` stops at the shorter one (no alarm, no padding — just stops).

`zip` can take any number of collections, not just two:

```python
for x, y, z in zip(xs, ys, zs):
    plot(x, y, z)
```

You will use `zip` constantly once you start working with parallel data.

---

## Nested Belts

A belt's body can contain another belt. Nested loops are the standard way to process grids, tables, and rows of rows (Lesson 7):

```python
grid = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

for row in grid:
    for cell in row:
        print(cell, end=" ")
    print()                       # newline after each row
```

Output:

```
1 2 3
4 5 6
7 8 9
```

Picture two belts running, with the inner belt completing all its passes for each single pass of the outer belt. Outer pass 1 — three items processed on the inner belt; line ends. Outer pass 2 — three more inner items; another line. And so on.

Deep nesting becomes hard to read quickly, just like nested junctions (Lesson 13). Three levels is usually the practical maximum. When you need more, the work usually belongs in a workstation that takes one row at a time.

---

## Common Patterns Worth Recognising

Almost all loop-using code falls into a few recurring shapes.

**Accumulator** — total over a collection:

```python
total = 0
for n in numbers:
    total += n
```

**Finding a maximum** — keep the largest seen so far:

```python
largest = numbers[0]
for n in numbers[1:]:
    if n > largest:
        largest = n
```

(Python provides built-in `sum` and `max` for these specific tasks, but the pattern is the universal shape of "fold a collection down to a single value".)

**Building a new row** — apply a procedure to each item, collect the results:

```python
squares = []
for n in numbers:
    squares.append(n * n)
```

**Counting by condition** — how many items match:

```python
count = 0
for item in items:
    if item.is_valid:
        count += 1
```

**Filtering** — keep only the items that match:

```python
positives = []
for n in numbers:
    if n > 0:
        positives.append(n)
```

You will see all five of these constantly. The next lesson (comprehensions) shows a much more compact form for the build-a-row and filter patterns specifically — but knowing the loop form first will make the comprehension form readable.

---

## What You Now Know

You can build belts of either kind — `for` for known collections and counted ranges, `while` for stopping conditions decided inside the loop. You can halt a belt early with `break` and skip an individual pass with `continue`. You know the loop `else` exists for the "completed without break" case. You can walk every collection in the warehouse, including the three explicit cabinet walks (`.keys()`, `.values()`, `.items()`). You can pair index with item using `enumerate`, and walk two collections in parallel with `zip`. You can nest belts to process grids and rows of rows.

You also know the most dangerous mistake in `while` belts — forgetting to make the sensor's switch ever fall — and how to recover (`Ctrl-C`).

Three of the most-recurring beginner loop shapes — accumulator, find-extremum, build-a-row — are now in your visual repertoire. The next lesson takes the last two of those shapes and shows the compact Python form for each.

---

## Quick Reference

| Python | Belt image |
|---|---|
| `for x in collection:` | Conveyor belt with counter — walks every item in order. |
| `for i in range(n):` | Conveyor belt with counter, loaded with stones `0..n-1`. |
| `range(start, stop)`, `range(start, stop, step)` | Customise the counter sequence. Stop is exclusive. |
| `for k in cabinet:` | Walks the cabinet's keys. |
| `for k, v in cabinet.items():` | Walks key-value crates; unpacks at each pass. |
| `for v in cabinet.values():` | Walks values directly. |
| `while condition:` | Conveyor belt with sensor — runs while the condition reads truthy. |
| `break` | Emergency stop — halt this belt immediately. |
| `continue` | Skip gate — drop this item, move to the next pass. |
| `for x in coll: ... else: ...` | The `else` runs only if the belt finished without `break`. |
| `for i, x in enumerate(coll):` | Yields `(index, item)` crates. |
| `for i, x in enumerate(coll, start=1):` | Same, but counting from 1. |
| `for a, b in zip(c1, c2):` | Walks two collections in parallel; stops at the shorter. |
| Nested belts | Inner belt completes once per pass of the outer belt. |
| `Ctrl-C` | The runtime emergency-stop for an infinite loop. |

---

## Try It

**A simple counter belt and a `range` belt:**

```python
for name in ["Alice", "Bob", "Cara"]:
    print("Hello,", name)

for i in range(5):
    print(i)

for i in range(10, 0, -1):
    print(i)
```

**Walking a cabinet — three forms:**

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}

for label in prices:
    print(label)

for value in prices.values():
    print(value)

for label, value in prices.items():
    print(f"{label}: {value}")
```

**A sensor belt with a flag:**

```python
running = True
count = 0
while running:
    count += 1
    if count >= 3:
        running = False
print("Stopped at", count)
```

**The emergency stop:**

```python
for n in [10, 20, 30, -5, 40]:
    if n < 0:
        print("Found a negative:", n)
        break
    print("processed:", n)
```

**The skip gate:**

```python
for n in [10, 20, -5, 30, -1, 40]:
    if n < 0:
        continue
    print(n)
```

**Loop `else`:**

```python
for n in [10, 20, 30]:
    if n < 0:
        print("Found a negative!")
        break
else:
    print("All non-negative.")
```

Now add a negative value to the list and re-run to feel the difference.

**`enumerate` and `zip`:**

```python
names = ["Alice", "Bob", "Cara"]
scores = [100, 75, 90]

for i, name in enumerate(names, start=1):
    print(f"{i}. {name}")

for name, score in zip(names, scores):
    print(f"{name}: {score}")
```

**Common patterns — type each one, then run:**

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# Accumulator
total = 0
for n in numbers:
    total += n
print("total:", total)

# Find max
largest = numbers[0]
for n in numbers[1:]:
    if n > largest:
        largest = n
print("largest:", largest)

# Build a new row
squares = []
for n in numbers:
    squares.append(n * n)
print("squares:", squares)

# Count by condition
big = 0
for n in numbers:
    if n > 4:
        big += 1
print("count >4:", big)

# Filter
big_ones = []
for n in numbers:
    if n > 4:
        big_ones.append(n)
print("filtered:", big_ones)
```

The last two — *build a new row* and *filter* — are the patterns the next lesson compresses into a single line.

---

## Where Next?

The build-a-row and filter patterns from this lesson become **comprehensions** — a compact one-line form that Python programmers reach for daily. Then come classes, inheritance, generators, and the remaining floor lessons.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 15 | Z5 — Factory Floor | Comprehensions — compact belt expressions |
| Lesson 16 | Z5 — Factory Floor | Classes — workshop blueprints and built workshops |
| Lesson 17 | Z5 — Factory Floor | Inheritance — extending a blueprint |
| Lesson 18 | Z5 — Factory Floor | Iterators and Generators — passing one item and waiting |

*See the full lesson map in **The Factory — A Complete Map**.*
