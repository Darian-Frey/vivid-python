# Python for Hyperphantasic Minds
## Lesson 13 — If / Else

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 13 of 25  
> **Topic**: If / Else — junctions and inspection gates  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still on the Factory Floor. Lesson 12 laid out the tools — including the comparison and logical tools that produce switches. Today you put those switches to work. A switch is most useful as the input to a **junction**: a point on the floor where material is examined and sent down one of several routes.

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

Junctions are the first piece of **control flow** — the part of a program that decides what work to do next. Without them, every line of every program would run, every time, in order. With them, the program can react to the value of a stone, the contents of a scroll, the state of a switch.

---

## The Junction — Physical Setup

Picture a forking section of the factory's main conveyor. Material arrives from the left. Just before the fork, there is an **inspection gate** — a barrier with a sensor mounted on it. The sensor reads a switch (or anything Python can press into a switch — see *Truthiness* later in the lesson). If the switch is up, the gate opens and the material is routed down the gate's path. If the switch is down, the gate stays closed and the material continues straight.

A simple junction has just one gate and two outcomes — *take the gated path* or *carry straight on*. A more complex junction has several gates in a row, each with its own switch sensor. The material approaches the gates in order; the first one whose switch is up opens and accepts the item. Subsequent gates never see it.

This is the entire architecture of every Python `if` statement.

---

## A Simple Junction — `if`

The simplest junction: one gate, one possible diversion. The material either takes the diversion or carries straight on.

```python
age = 25

if age >= 18:
    print("You can enter.")
```

Read this line by line:

- `age = 25` — place a stone on the `age` shelf.
- `if age >= 18` — build a junction. The comparison tool produces a switch; that switch becomes the gate's sensor input. Since `25 >= 18` is `True`, the gate opens.
- `:` — what follows is the *route taken when the gate opens*. The colon marks the start of an indented lane, just as in a workstation's body (Lesson 8).
- `    print("You can enter.")` — the diverted path's procedure. Indented by four spaces; runs only when the gate opens.

The output is `You can enter.`. If `age` had been `12`, the gate would have stayed closed, the indented line would have been skipped, and nothing would have been printed.

After the indented block, control rejoins the main conveyor. Any non-indented line that follows runs regardless of whether the gate opened.

```python
age = 25

if age >= 18:
    print("You can enter.")

print("Welcome to the building.")
```

Both lines print when `age` is at least 18. When `age` is below 18, only the second line prints. The two paths *merge back together* at the un-indented line.

---

## A Two-Path Junction — `if` / `else`

Often you want a different action when the gate stays closed, not just *no action*. The `else` clause names the path taken when the `if` gate does not open.

```python
age = 12

if age >= 18:
    print("You can enter.")
else:
    print("Sorry, adults only.")
```

Picture: the same junction as before, but now both the gated path and the *straight-through* path lead to clearly-labelled stations. The material always goes through one or the other; the two paths run *in parallel*, never both.

The output here is `Sorry, adults only.`. With `age = 25`, the output would be `You can enter.`.

The `else` branch is everything-not-the-`if`. There is no condition on the `else`; it catches whatever the `if` did not.

---

## A Multi-Path Junction — `if` / `elif` / `else`

When you have several conditions to check in order, use `elif` (a contraction of *else if*) for each intermediate gate. The structure is *"try the first gate; if it doesn't open, try the next; …; if none open, fall through to `else`."*

```python
score = 75

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(grade)              # 'C'
```

Picture: four inspection gates in a row, then a final straight-through path labelled `else`. The material approaches the first gate. Its sensor checks `score >= 90` — switch is down, gate stays shut. It approaches the second — `score >= 80`, down, shut. Third — `score >= 70`, up, gate opens, material accepted. The fourth and the `else` are never reached. The procedure beyond the third gate runs; control rejoins the main conveyor.

**Important:** the gates are checked *in order*, and only the *first* opening gate matters. This is why the conditions above are written in descending order. Reversing them would cause the first gate (`score >= 60`) to open for almost every score, and the higher grades would never be reached. The order is the meaning.

You may have any number of `elif` branches — zero, one, several, dozens. The `else` at the end is optional; without it, a piece of material whose value matches none of the gates simply carries on.

---

## What Lives Inside a Gate's Sensor — Truthiness

A gate's sensor accepts not just a switch but *any* item, pressing it through the switch mould (Lesson 5) if necessary. The rule, from Lesson 5, applies here directly:

- **False values:** `0`, `0.0`, `False`, `""`, `[]`, `{}`, `set()`, `None`
- **True values:** everything else — non-empty scrolls, non-zero numbers, non-empty rows / cabinets / bins, anything that is "something"

This means the gate can be sensitive to *presence* rather than a specific comparison:

```python
name = input("What is your name? ")

if name:
    print("Hello,", name)
else:
    print("You didn't tell me your name.")
```

Picture: the gate's sensor reads the contents of the `name` shelf. The scroll is pressed through `bool()`. If the scroll has any content, the result is `True`, and the gate opens. If the scroll is empty (the user just pressed Enter), the result is `False`, and the `else` branch runs.

This is the Pythonic way to ask *"is there a value here?"* It is more idiomatic than `if name != "":` or `if len(name) > 0:`. Once you trust the truthiness rule, the gate's sensor lines become shorter and clearer.

A few patterns that come up constantly:

```python
if items:                        # "is the row non-empty?"
    print("First item:", items[0])

if config.get("debug"):          # "did the user opt into debug mode?"
    print("Debug mode on.")

while running:                   # "keep going while the switch is up"  (Lesson 14)
    ...
```

Each of these reads cleanly because the gate's sensor takes the value as it is, not after an unnecessary explicit comparison.

---

## A Common Beginner Habit to Drop

Two patterns to avoid early:

```python
if is_ready == True:             # 🛑 redundant
    ...

if is_ready:                     # ✅ clean
    ...
```

If `is_ready` already holds a switch, the gate's sensor already reads it correctly. Adding `== True` is double-work and harder to read.

Similarly:

```python
if some_list == []:              # 🛑 works but unusual
    ...

if not some_list:                # ✅ Pythonic
    ...
```

`not some_list` is `True` when the row is empty, by truthiness. The shorter form is preferred by every Python style guide.

---

## Junctions Inside Junctions — Nesting

A junction's indented block can itself contain another junction. Picture the geometry: after the first gate, the material reaches a *second* fork, with its own gate.

```python
age = 25
has_ticket = True

if age >= 18:
    if has_ticket:
        print("Welcome inside.")
    else:
        print("You're old enough, but no ticket.")
else:
    print("Too young.")
```

Two nested junctions. The first decides on age; the second, only on the "old enough" branch, decides on the ticket. Output: `Welcome inside.`.

**Deep nesting becomes hard to read quickly.** Three levels are usually the practical maximum before the indentation alone makes the code hard to scan. When you find yourself nesting beyond two or three levels, the structure is usually trying to tell you that the conditions should be combined or that the logic belongs in a workstation:

```python
# Hard to read at four levels:
if user:
    if user.active:
        if user.has_permission:
            if not user.locked:
                print("Welcome.")

# Often clearer as a single combined check:
if user and user.active and user.has_permission and not user.locked:
    print("Welcome.")
```

The combined form is also short-circuit safe (Lesson 12) — `user.active` is never read if `user` is `None`, because `and` stops at the first false term. Combining checks is often the right tool, not deeper nesting.

---

## The Inline Form — A Tiny Junction

When the whole junction is just choosing between two values to place on a shelf, Python provides a compact form:

```python
age = 25
category = "adult" if age >= 18 else "child"
print(category)             # 'adult'
```

Read aloud: *"category gets `'adult'` if `age >= 18`, else `'child'`."* The structure is `value_if_true if condition else value_if_false`. Picture a tiny inline junction with two clearly-labelled outputs — the value flows through one or the other based on the gate's switch.

This form is most useful for *value selection*, not for *taking different actions*. If both branches need to do real work — print, call a workstation, run several lines — use the full `if/else` form. The inline form's strength is brevity for the case "pick one of two values, place it on a shelf".

Some examples where the inline form reads well:

```python
greeting = "Hi" if casual else "Good evening"
sign = -1 if n < 0 else 1
suffix = "s" if count != 1 else ""
print(f"You have {count} message{suffix}.")
```

If you find yourself writing an inline form that does not fit on one readable line, switch back to the full `if/else`.

---

## A Brief Look at `match` / `case`

Python 3.10 added a powerful junction-like form called **structural pattern matching**, using the `match` and `case` keywords. It is useful for *value dispatch* — when you have one item and want to match it against several specific patterns.

```python
command = "start"

match command:
    case "start":
        print("Starting up...")
    case "stop":
        print("Shutting down...")
    case "status":
        print("All systems normal.")
    case _:
        print("Unknown command.")
```

The `case _:` at the end is the catch-all — equivalent to the `else` of a junction. Each `case` is a separate pattern to match against the matched value.

`match` can do much more than this — destructure sealed crates, filing cabinets, and even class instances — but the simple value-matching form above is the easiest entry point. For now, recognise it. You will reach for it occasionally when an `elif` chain becomes long. We will not depend on it elsewhere in this series.

---

## Quick Recap — What the Gate Does

Every junction in your code follows the same physical pattern:

1. Material approaches the fork.
2. The gate's sensor reads its switch input (pressing any non-switch item through `bool()` first).
3. The first gate whose switch is up opens; the material is routed down its path; the rest of the procedure beyond that gate runs.
4. If no gate opens, the `else` path runs (or nothing, if there is no `else`).
5. After whichever path ran, control rejoins the main conveyor at the un-indented line.

If you keep that physical model in mind, every `if` / `elif` / `else` you read will be transparent.

---

## What You Now Know

You can build junctions of any shape — one gate, two gates with an `else`, a chain of `elif` gates with or without an `else`. You can nest junctions inside other junctions, and you know when nesting starts becoming a problem. You can write a value-selecting junction inline. You know that any value can serve as a gate's sensor input, with the truthiness rule deciding up or down — and that this lets you write the natural Pythonic forms `if name:`, `if items:`, `if not config:` instead of explicit equality checks.

You have also met `match` / `case` and know it exists for the cases an `elif` chain handles awkwardly.

This is the second of two essential control-flow constructs. The first was the workstation (Lesson 8) — *what work happens here*. The junction is *which path through the work*. The next lesson introduces the third — the conveyor belt — which is *how many times the same work happens*.

---

## Quick Reference

| Python | Junction image |
|---|---|
| `if cond:` `    body` | One gate; gate opens when `cond` is truthy; body runs. |
| `if cond:` … `else:` | Two paths; the `else` runs when the gate is closed. |
| `if a:` `elif b:` `elif c:` `else:` | Multi-gate junction; first opening gate wins. |
| `if items:` | Truthiness — opens when `items` is non-empty / non-zero / non-None. |
| `if not items:` | Truthiness inverted — opens when `items` is empty / zero / None. |
| `x if cond else y` | Inline form — picks one of two values. For value selection only. |
| `match value: case "x":` | Structural matching (Python 3.10+); use for value-dispatch problems. |
| Avoid `if x == True:` | The gate already reads the switch. Write `if x:`. |
| Avoid deep nesting | Three levels is the practical max; combine conditions or refactor into a workstation. |

---

## Try It

**A simple one-gate junction:**

```python
age = 25
if age >= 18:
    print("Adult.")
print("Done.")
```

Run with `age = 12` and see how the output changes.

**Two-path junction:**

```python
age = 12
if age >= 18:
    print("Welcome.")
else:
    print("Too young.")
```

**Multi-gate junction — order matters:**

```python
score = 75
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"
print(grade)
```

Now try this version, with the *wrong* order, to feel the trap:

```python
score = 75
if score >= 60:
    grade = "D"          # opens for almost everyone
elif score >= 70:
    grade = "C"
elif score >= 80:
    grade = "B"
elif score >= 90:
    grade = "A"
print(grade)             # always 'D' for any score >= 60
```

The first gate that opens wins. Order is the meaning.

**Truthiness in the gate:**

```python
name = ""
if name:
    print("Hello,", name)
else:
    print("No name given.")

items = []
if items:
    print("Got items:", items)
else:
    print("Empty.")

config = {"debug": True}
if config.get("debug"):
    print("Debug mode on.")
```

Each gate's sensor reads the value through `bool()`. Empty containers and missing values all read as `False`.

**Nesting — and a flatter alternative:**

```python
age = 25
has_ticket = True

# Nested:
if age >= 18:
    if has_ticket:
        print("Welcome inside.")
    else:
        print("Old enough, but no ticket.")
else:
    print("Too young.")

# Flatter, using combined conditions:
if age >= 18 and has_ticket:
    print("Welcome inside.")
elif age >= 18:
    print("Old enough, but no ticket.")
else:
    print("Too young.")
```

**Inline form — value selection:**

```python
count = 1
suffix = "s" if count != 1 else ""
print(f"You have {count} message{suffix}.")

count = 3
suffix = "s" if count != 1 else ""
print(f"You have {count} message{suffix}.")
```

**`match` — basic value dispatch:**

```python
command = "start"
match command:
    case "start":
        print("Starting up...")
    case "stop":
        print("Shutting down...")
    case _:
        print("Unknown command.")
```

---

## Where Next?

The junction routes material once. The next construct on the Floor routes material *over and over* — the **conveyor belt**, the home of `for` and `while`. After loops come comprehensions, classes, inheritance, and the remaining floor lessons.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 14 | Z5 — Factory Floor | Loops — conveyor belts, emergency stops, skip gates |
| Lesson 15 | Z5 — Factory Floor | Comprehensions — compact belt expressions |
| Lesson 16 | Z5 — Factory Floor | Classes — workshop blueprints and built workshops |
| Lesson 17 | Z5 — Factory Floor | Inheritance — extending a blueprint |

*See the full lesson map in **The Factory — A Complete Map**.*
