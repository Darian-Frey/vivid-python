# Python for Hyperphantasic Minds
## Lesson 2 — Syntax & Output

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 2 of 25  
> **Topic**: Syntax & Output — calling out across the floor  
> **Factory zone**: Z11 — Outgoings  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

In Lesson 1 you stood inside the Warehouse and watched stones and scrolls being placed into named cubbyholes. A program that only stores things does nothing the world can see. To do something *useful*, the factory has to be able to send things out — call values across the floor, hand finished goods to the dispatch team, write a label that someone outside can read.

For that, you need to walk to the far end of the production line.

```
                    ┌──────────────────────────────┐
                    │    SHIFT MANAGER'S OFFICE     │
                    └──────────────┬───────────────┘
                                   │
┌──────────┐                       ▼        ┌─────────────────┐
│          │        ┌──────────┐            │                 │
│TOOL STORE│───────▶│ GOODS IN │            │  FACTORY FLOOR  │
│          │        │          │            │                 │
└──────────┘        └────┬─────┘            └────────┬────────┘
                         │                           │
                         ▼                           │
              ┌──────────────────────┐               │
              │      WAREHOUSE       │◀──────────────┘
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
  ┌──────────────────────┐
  │                      │
  │   ██ OUTGOINGS ██    │
  │                      │
  │     YOU ARE HERE     │
  └──────────────────────┘
```

Outgoings sits at the very end of the main production line. Nothing leaves this factory without passing through this room. If your program needs the world to see anything — a number, a message, a result — that thing comes through here.

You are taking a short journey to the end of the line. After this lesson you will turn around and head back to the Warehouse, because there is much more to learn about what can sit on those shelves. But you cannot have a working program until you know how to send something out, so this stop comes early.

---

## The Dispatch Bay

You walk along the side of the main building until the wall opens onto a large covered dock. The roof is high; the back wall is mostly door — wide roller doors raised to let in the daylight. Beyond the doors is the world outside the factory: the road, the loading lorries, the people the factory exists to serve.

A worker stands at the dispatch line with a clipboard and a clear, carrying voice. Their job is simple: when a finished item arrives at the bay, they read its label aloud so the world outside knows what just left the building. Sometimes they are calling out a single message. Sometimes a stream of values. Sometimes nothing — a deliberate silence, marking a break between batches.

This is **Outgoings**. Everything your program tells the outside world is dispatched from this room.

---

## Calling Out — `print()`

Picture the dispatch worker holding up a scroll that reads `"Hello, world!"`. They take a breath and call out, in a clear voice, what is written on it.

```python
print("Hello, world!")
```

That is **calling out across the floor** — Python's `print()`. Hand it something, and the dispatch worker reads that something aloud so the world (the terminal, the console, the screen) hears it.

The call lands as a line of text wherever the program is being run. The factory has spoken.

---

## Calling Out Anything

Whatever you hand the dispatch worker, they can read it aloud. A scroll. A stone. A switch. Any item that fits in a cubbyhole.

```python
print("Hello, world!")   # a scroll — read out as text
print(42)                # a stone — read out as a number
print(True)              # a switch — read out as the word True
```

The worker does not care what the item is made of. They look at it, work out how it should be voiced, and call it out.

This is different from the production line behind them. On the Factory Floor, types matter strictly — a stone and a scroll cannot be added together (you saw this in Lesson 1). Out here at the dispatch bay, types do not matter the same way: the worker can read out any item the factory hands them.

---

## Calling Out Several Things at Once

Hand the dispatch worker more than one item, and they will read each in turn, with a single space between, all on the same line.

```python
print("Hello,", "world!")
print(1, 2, 3)
print("score is", 27)
```

Picture the worker laying the items in a row on the counter, then reading from left to right. The call goes out:

```
Hello, world!
1 2 3
score is 27
```

Notice the third example — a scroll and a stone read out together. The dispatch worker handles each in turn; nothing has to match. This is one of the most common ways `print()` is used: two or three values, side by side, with a space between.

---

## Expressions — Working Out the Value First

You can hand the dispatch worker something that has not been worked out yet — an *expression* — and Python will figure it out before the worker reads it.

```python
print(1 + 2)
print("Hello, " + "world!")
```

What gets called out:

```
3
Hello, world!
```

Picture this as two steps: first, the value is calculated on the spot at the bay (`1 + 2` becomes a stone marked `3`; the two scrolls are joined into a single scroll reading `"Hello, world!"`); then the dispatch worker reads the result. The worker does not see the expression — only the answer.

The same goes for named cubbyholes from the Warehouse:

```python
name = "Shane"
score = 27
print("Player:", name)
print("Score:", score)
```

The dispatch worker reads what is currently sitting on each named shelf at the moment the call happens. Replace the value later and the worker will not know — they were only there for that one call-out.

---

## A Deliberate Silence

Sometimes you want a blank line — a beat between two outputs. Hand the dispatch worker nothing at all, and they call out silence: a line with nothing on it.

```python
print("Top of the report.")
print()
print("Bottom of the report.")
```

What appears:

```
Top of the report.

Bottom of the report.
```

A `print()` with empty parentheses still moves to a new line. It is a small thing, but useful — every printed report and every multi-section output uses it.

---

## Notes Pinned to the Shelves — Comments

Now and again you need to leave a message in the code that is *for the reader, not the machine*. A reminder of why something was done. A label. A warning to your future self.

Picture the warehouse foreman walking down an aisle with a pad of small notes and a box of pins. They pin a note to a shelf: *"the value here is the number of available seats — do not change without checking the booking system"*. The note is for whoever reads the warehouse. The forklifts and the conveyor belts go right past it as if it were not there.

In Python, a note is anything after a `#` on a line:

```python
# This is a note pinned to the code.
age = 27           # the player's current level
print(age)         # call it out for anyone watching
```

Python — the machine — sees the `#` and ignores everything after it on that line. The note is invisible to the factory. It is only there for the human reader.

There is no separate "block comment" in Python. For longer notes, pin a series of single-line notes one after another:

```python
# This block of code calls out the player's stats.
# It runs once at the start of every game.
# Replace with a richer report when the dashboard is ready.
print(name, "—", score)
```

A common shortcut you will see in other people's code is a triple-quoted scroll left lying around without a name pinned to it:

```python
"""
This is sometimes used as a multi-line note.
It is technically a scroll Python builds and immediately discards.
The forklift clears it on the next pass.
"""
```

It works as a stand-in for a block comment, but a stack of `#` notes is cleaner and more honest about what it is. Use `#`. Reserve triple-quoted scrolls for actual scrolls (Lesson 6) and for documentation strings (Lesson 8 onwards).

---

## One Job Order Per Line — Statements

Each line of Python is **one job order** — one instruction the factory will carry out. The factory reads from the top down: first line first, second line second, and so on, in order.

```python
name = "Shane"
score = 27
print("Player:", name)
print("Score:", score)
```

Four lines, four job orders, four things that happen in order: place a scroll on the `name` shelf; place a stone on the `score` shelf; call out the player; call out the score.

You do not need a semicolon at the end of a Python line. The end of the line *is* the end of the job order. Python is using whitespace where many other languages use punctuation — which leads us to the floor markings.

---

## Lane Markings on the Floor — Indentation

The factory's concrete floor is painted with lane markings. Every operation on the production line happens inside a lane. A worker who steps outside the lane is no longer part of that operation.

In Python, **indentation marks the lanes**. A line indented underneath another line belongs to that line's lane of work. A line at the left margin starts a new top-level job.

You will not write much indented code in this lesson — you have not yet met the structures that need it. But for context, here is what indentation will look like in later lessons:

```python
if score > 100:
    print("New high score!")
    print("Saving the game.")
print("Game ends.")
```

The two indented lines belong to the `if` lane — they only run when the condition is true. The unindented `print("Game ends.")` is at the left margin and runs no matter what. Picture the conveyor branching: only certain items take the indented detour; everything continues to the final line at the bottom.

For now you only need three rules:

1. **Use four spaces** to indent. The whole Python world has settled on this. Do not use tabs.
2. **Be consistent.** Every line inside the same lane uses exactly the same indentation. If one line has four spaces and the next has six, Python will stop and complain. The lane markings have to line up.
3. **Indent only when something asks you to.** If the code above the line did not start a new structure (an `if`, a `for`, a function definition…), do not indent. Random indentation is a syntax error in Python — which is rare among programming languages and one of the things that makes Python code so visually clean.

You will meet the structures that *do* ask for indentation from Lesson 13 onwards (junctions, loops, workstations). Until then, every line you write should sit at the left margin.

---

## What You Now Know

You have walked the production line from one end to the other and watched the dispatch bay at work. You have seen `print()` call out a single item, a row of items, an expression, and a deliberate silence. You have learned what a comment is and what indentation does — even though you will not yet need to indent anything you write.

You also know enough Python to build a complete, working program. From the first line to the last:

```python
# Player setup.
name = "Shane"
score = 27

# Final report.
print("Player:", name)
print("Score:", score)
print()
print("End of shift.")
```

Read it as a small journey through the factory. Two named cubbyholes are filled in the Warehouse. The dispatch worker is handed the player's name, then the player's score, then a deliberate silence, then a closing message. The world outside the factory hears all of it, in order.

That is a Python program. Everything else you learn from here on adds new rooms to the same factory.

---

## Quick Reference

| Python | Factory image |
|--------|---------------|
| `print("hello")` | Hand the dispatch worker a scroll. They call it out. |
| `print(42)` | Hand them a stone. They read its number aloud. |
| `print(a, b, c)` | Three items in a row on the counter; read out left to right with a space between. |
| `print(1 + 2)` | The value is worked out at the bay (a `3` stone) before being called out. |
| `print()` | A deliberate silence — a blank line in the dispatch log. |
| `# note` | A note pinned to the code for the human reader; the machine ignores it. |
| `    ` (four-space indent) | A lane marking on the floor — groups lines into one run of work. |
| One line | One job order. The factory reads top to bottom, in order. |

---

## Try It

Open a Python prompt or any Python editor. Type each block and run it.

**Calling out a single value:**

```python
print("Hello, world!")
print(42)
print(True)
```

Picture the dispatch worker reading each line aloud in turn.

**Calling out several values at once:**

```python
print("Hello,", "world!")
print(1, 2, 3)
print("score is", 27)
```

Notice the single space between each value. That is the worker's natural pause as they move from one item to the next.

**Calling out an expression:**

```python
print(1 + 2)
print(10 * 5)
print("Hello, " + "world!")
```

Notice that what comes out is the *answer*, not the expression. The value is worked out at the bay before the call.

**Calling out from named cubbyholes:**

```python
name = "your name"
score = 100
print("Player:", name)
print("Score:", score)
print()
print("End of report.")
```

Read this as four job orders carried out in order: two placements in the Warehouse, two call-outs from the dispatch bay, a silent line, and a closing call.

**Pinning a note for a future reader:**

```python
# This program prints the player's stats.
name = "Alice"   # used in the dashboard header
score = 85       # current score; will update each game
print(name, "scored", score)
```

Notice that the notes affect nothing the program does. Try removing all three notes — the output does not change. They were only there for *you*, the reader.

---

## Where Next?

You have a working program — a small one, but a real one. Now you turn around and walk back to the Warehouse. There is much more to learn about what kinds of items can sit on those shelves.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 3 | Z3 — Warehouse | Data Types — the full range of objects on the shelves |
| Lesson 4 | Z3 — Warehouse | Numbers — stones and vials in depth |
| Lesson 5 | Z3 → Z5 | Casting — pressing objects into moulds |
| Lesson 6 | Z3 — Warehouse | Strings — scrolls in depth (including f-strings) |

*See the full lesson map in **The Factory — A Complete Map**.*
