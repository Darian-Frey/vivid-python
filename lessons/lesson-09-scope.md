# Python for Hyperphantasic Minds
## Lesson 9 — Scope

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 9 of 25  
> **Topic**: Scope — the locked rooms in full  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Back in the Warehouse. The detour to the Factory Floor in Lesson 8 did its job — you have built workstations and watched them run. This lesson finishes the picture by laying out the four kinds of shelves a workstation can see, where they live, and the rules that say which shelf is found first when the same name appears in more than one place.

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
              │                      │               │
              │   ██ WAREHOUSE ██    │◀──────────────┘
              │                      │
              │     YOU ARE HERE     │
              └──────────────────────┘
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

This is the most subtle topic in beginner Python and the one that confuses newcomers most often. Approach it slowly. The visual layout — *where each shelf is in the building* — is the whole game. Once that picture is in place, the syntax is just labels.

---

## The Layout of the Warehouse — All Four Levels

The warehouse is not a single open space. It has four kinds of shelves, arranged in concentric layers. From innermost to outermost:

```
              ┌─────────────────────────────────────────────┐
              │ THE FACTORY STANDARD KIT                    │
              │ (print, len, range, type, str, int, abs,    │
              │  round, min, max, sorted... always there)   │
              │                                             │
              │   ┌─────────────────────────────────────┐   │
              │   │ THE OPEN AISLE                      │   │
              │   │ (top-level shelves of your program; │   │
              │   │  visible to every workstation)      │   │
              │   │                                     │   │
              │   │   ┌─────────────────────────────┐   │   │
              │   │   │ ANTEROOM (if any)           │   │   │
              │   │   │ (an enclosing workstation's │   │   │
              │   │   │  workspace, visible to       │   │   │
              │   │   │  workstations nested inside) │   │   │
              │   │   │                             │   │   │
              │   │   │   ┌─────────────────────┐   │   │   │
              │   │   │   │ THE LOCKED ROOM     │   │   │   │
              │   │   │   │ (one workstation's  │   │   │   │
              │   │   │   │  private workspace) │   │   │   │
              │   │   │   └─────────────────────┘   │   │   │
              │   │   └─────────────────────────────┘   │   │
              │   └─────────────────────────────────────┘   │
              └─────────────────────────────────────────────┘
```

Four levels:

- **The locked room** — a single workstation's private workspace. Created when a job order arrives; cleared the moment the job ends.
- **The anteroom** — only present when a workstation is *nested* inside another workstation. The outer workstation's locked room becomes an anteroom from the inner workstation's perspective.
- **The open aisle** — the warehouse's top-level shelves. Created at the start of the shift; cleared at the end. Every workstation in the factory can see them.
- **The factory standard kit** — pre-installed tools and shelves that come with every Python factory: `print`, `len`, `range`, `type`, `str`, `int`, `bool`, `abs`, `round`, `min`, `max`, `sorted`, and many others. Available everywhere, without fetching from the Tool Store.

When a workstation wants to know what a name points to, Python walks *outward* — locked room first, then any anteroom, then the open aisle, then the standard kit. The first shelf with that name wins.

This four-letter sequence is the rule programmers refer to as **LEGB** — Local, Enclosing, Global, Built-in. Same picture, more compact name.

---

## The Locked Room — Local Scope

Every job-in-progress has its own locked room — a private workspace that only that one running job can see. You met this briefly in Lessons 1 and 8. Here it is in full.

```python
def double(x):
    result = x * 2          # 'result' is created in the locked room
    return result

print(double(5))            # 10
# print(result)             # NameError — no 'result' shelf in the warehouse
```

When the job order `double(5)` arrives, the welders unlock a fresh empty room, place the delivered stone `5` on a cubbyhole labelled `x`, and start running the procedure. The line `result = x * 2` creates a new cubbyhole — *inside the locked room* — labelled `result`, holding stone `10`. The next line returns that stone via the output belt, the procedure ends, and the room is cleared completely. Both `x` and `result` are gone. They never existed as far as the rest of the warehouse is concerned.

**Two simultaneous calls each have their own locked room.**

```python
def double(x):
    result = x * 2
    return result

a = double(5)               # locked room A: x=5, result=10, then cleared
b = double(7)               # locked room B: x=7, result=14, then cleared
```

The two rooms never see each other. Every line `result = ...` creates a new cubbyhole *in this call's room*, not in any other call's room. This is what makes workstations safe to call from anywhere, in any order, without their internal scratch shelves interfering.

The input slots are part of the locked room too. The slot labels (`x` in this example) become cubbyhole names *inside the room* the moment the job order arrives.

---

## The Open Aisle — Global Scope

Shelves declared at the top level of your program — outside of any workstation — live in **the open aisle**. The open aisle is created at the start of the shift and persists until the shift ends. Every workstation in the factory can read from it.

```python
PI = 3.14159                          # placed in the open aisle
RADIUS = 10                           # placed in the open aisle

def circle_area():
    return PI * RADIUS * RADIUS       # reads PI and RADIUS from the open aisle

print(circle_area())                  # 314.159
```

When `circle_area` runs, Python encounters the names `PI` and `RADIUS`. It checks the locked room — neither shelf is there. It walks outward to the open aisle and finds both. They are read freely.

This is how you keep configuration, constants, and shared data accessible across the whole program. The convention from Lesson 1 is to write open-aisle constants in `UPPER_CASE` — `PI`, `MAX_PLAYERS`, `DEFAULT_TIMEOUT` — as a signal to readers that these are not meant to be replaced.

---

## Reading From the Open Aisle (Versus Writing)

Reading from the open aisle is free. Writing is *not free* — and this is where most beginners trip up.

Inside a workstation, when you write `name = value`, Python *always creates a new shelf in the locked room*. It does not modify the open-aisle shelf, even if a shelf with the same name already exists out there:

```python
counter = 0                          # in the open aisle

def try_to_increment():
    counter = counter + 1            # ⚠ this counts as a write — locked-room shelf
    print("inside:", counter)

try_to_increment()
print("outside:", counter)
```

This program does not even run. Python sees the line `counter = counter + 1`, decides "the workstation writes to `counter`, so `counter` is a locked-room shelf inside this procedure", and then complains that the *read* on the right-hand side (`counter + 1`) is reading a locked-room shelf that has not been placed yet. `UnboundLocalError`.

The picture: Python looks at the whole workstation's body before running it, decides which names will be written to, and reserves a fresh locked-room shelf for each one. Any read of that name then refers to the locked-room shelf, not the open-aisle one.

**The fix when you genuinely want to read from the open aisle and not write to it.** Use a different name on the left:

```python
counter = 0

def report():
    new_value = counter + 1          # only reads counter; writes to a fresh local shelf
    print("inside:", new_value)

report()
print("outside:", counter)           # 0 — open aisle untouched
```

This is the most common pattern: a workstation reads from the open aisle and produces a finished product, without modifying the open aisle at all.

---

## Writing to the Open Aisle — `global`

When you genuinely *do* need a workstation to modify an open-aisle shelf, declare your intent at the top of the procedure with the `global` keyword:

```python
counter = 0                          # in the open aisle

def increment():
    global counter
    counter += 1                     # now refers to the open-aisle shelf

increment()
increment()
increment()
print(counter)                       # 3
```

The line `global counter` says: *"In this procedure, the name `counter` refers to the open-aisle shelf, not a fresh locked-room one."* From that point on, every read and write to `counter` inside the workstation goes to the open-aisle shelf.

`global` is a declaration, not an action. Putting it on its own line at the top of the procedure is the convention; it does not "do" anything, only tells Python how to interpret the name in the rest of the body.

---

## Why You Should Almost Never Use `global`

Once a workstation modifies the open aisle, *anyone reading the program has to keep track of every workstation that might do so*. The same shelf can be changed by any workstation that declared `global` for it. The program's behaviour is no longer captured by reading workstations one at a time.

The cleaner pattern, almost always, is to **pass the value in as delivered material and send the new value back as a finished product**:

```python
def increment(counter):
    return counter + 1

counter = 0
counter = increment(counter)
counter = increment(counter)
counter = increment(counter)
print(counter)                       # 3
```

The workstation declares its dependency on `counter` through its input slot, and its effect through its finished product. Reading just the workstation tells you everything it does. The open aisle is left clean.

You will see `global` used legitimately for module-level configuration that genuinely is global state — caches, registered handlers, certain singletons. But for a beginner-to-intermediate program, you can write hundreds of lines of working Python without ever needing it. When you find yourself reaching for `global`, ask first whether the workstation could simply receive its input and return its output.

---

## Workstations Inside Workstations — The Anteroom

A workstation can be defined *inside* another workstation. The inner one is built when the outer one's procedure runs:

```python
def outer():
    x = 10                           # x lives in outer's locked room

    def inner():
        print(x)                     # reads x from the anteroom

    inner()                          # 10

outer()
```

From `inner`'s perspective, `outer`'s locked room is the **anteroom** — a workspace that sits between `inner`'s own locked room and the open aisle. `inner` cannot see the deepest insides of `outer` (a `nonlocal` shelf would be needed; see next section), but it *can read* shelves on the anteroom.

When `outer()` runs:
1. A locked room for `outer` is opened. The cubbyhole `x` is filled with the stone `10`.
2. The procedure builds a workstation called `inner` — its name plate is mounted inside `outer`'s locked room.
3. The procedure sends a job order to `inner`. A second locked room is opened, *nested* inside `outer`'s.
4. `inner` runs `print(x)`. It checks its own locked room — no `x`. It walks outward to the anteroom (`outer`'s locked room) and finds `x` there. Stone `10` is read out and printed.
5. `inner`'s locked room is cleared.
6. The procedure of `outer` ends. `outer`'s locked room is cleared, including the now-disposable `inner` workstation.

The anteroom is a real and important architectural feature for one specific kind of programming pattern called *closures*, which become useful when a workstation's job depends on values captured from where it was built. We will not need closures much at the beginner level, but the anteroom is part of the LEGB rule, and it appears the moment you nest workstations.

---

## Writing to the Anteroom — `nonlocal`

Just as writing to the open aisle from a workstation requires `global`, writing to the anteroom from a nested workstation requires `nonlocal`:

```python
def make_counter():
    count = 0                        # lives in make_counter's locked room

    def tick():
        nonlocal count
        count += 1
        return count

    return tick                      # send the inner workstation back to the caller

counter = make_counter()
print(counter())                     # 1
print(counter())                     # 2
print(counter())                     # 3
```

This is a closure. The inner `tick` workstation, *built* inside `make_counter`, was sent back as the finished product. Even though `make_counter` has long ended, its locked room has not been cleared — because `tick` is still using it as an anteroom. The `count` cubbyhole persists for as long as `tick` does. Each call to `tick` finds the same `count` cubbyhole, increments it, and returns the new value.

Closures are deeper than this lesson; the example above is enough to recognise the form when you see it. The takeaway: `nonlocal` writes go to the *nearest enclosing* anteroom (not the open aisle, not a more outer anteroom). If no anteroom holds the name, Python triggers an alarm.

---

## The Factory Standard Kit — Built-in Scope

The outermost layer holds Python's pre-installed workstations and shelves: `print`, `len`, `range`, `type`, `str`, `int`, `float`, `bool`, `abs`, `round`, `min`, `max`, `sorted`, `reversed`, `list`, `tuple`, `set`, `dict`, `True`, `False`, `None`, and several dozen more. These are always available, in every part of every program, without `import` or any other action. They are built into the factory itself.

You have used a dozen of these already. The rule for them is exactly the LEGB rule: Python looks for a name in the locked room, the anterooms, the open aisle, and finally — if none of those have it — the standard kit.

This means the standard kit is **the last resort**, which has consequences if you accidentally use a built-in's name for one of your own shelves.

---

## The Same-Name Shadow Trap

The most famous scope-related bug in Python: accidentally giving a shelf the same name as a built-in workstation, then expecting the built-in to still work.

```python
list = [1, 2, 3]                     # ⚠ shadows the built-in 'list' mould
print(list)                          # [1, 2, 3]

new_list = list("abc")               # TypeError — 'list' is now a row, not a mould
```

The line `list = [1, 2, 3]` placed a numbered row on the open-aisle shelf labelled `list`. From now on in this program, any read of `list` finds the row first — *before* Python even looks at the standard kit. The built-in mould is shadowed. Calling `list("abc")` is now equivalent to calling the row as if it were a workstation, which produces the error.

**Common shadow names to avoid as cubbyhole names:**

`list`, `dict`, `set`, `tuple`, `str`, `int`, `float`, `bool`, `type`, `id`, `len`, `min`, `max`, `sum`, `print`, `input`, `open`, `range`, `format`, `next`, `iter`.

When in doubt, add a small qualifier: `my_list`, `score_list`, `current_list`. The cost of the longer name is nothing compared to the cost of debugging a shadow bug a week later.

---

## The Lookup Order — LEGB in Practice

Putting it all together, here is what Python does every time it encounters a name in a workstation's procedure:

1. **L** — check the **locked room** (the current job's private workspace).
2. **E** — check any **enclosing** workspaces (anterooms), from innermost outward.
3. **G** — check the **open aisle** (the program's top-level shelves).
4. **B** — check the **standard kit** (Python's built-in names).
5. If none of the above, raise `NameError`.

The first shelf found wins. Subsequent shelves with the same name are never checked. Shadowing is a direct consequence of this rule.

The same rule applies to writes, with one key difference: a write inside a workstation creates a fresh locked-room shelf *unless* `global` or `nonlocal` redirects it.

---

## What You Now Know

You have seen the full layout of the warehouse from a workstation's point of view. There are four kinds of shelves — locked room, anterooms, open aisle, standard kit — and Python looks them up in that order, innermost outward, when it needs to find a name.

You know that writing inside a workstation always creates a fresh locked-room shelf, unless you declare `global` (to write to the open aisle) or `nonlocal` (to write to an anteroom). You know that `global` is rarely the right tool, and that passing values as delivered material and returning them as finished products keeps programs much easier to reason about.

You know the same-name shadow trap and the small list of built-in names not to reuse. You have seen the bones of a closure — a nested workstation that survives its outer one, taking its anteroom shelves with it.

Most working Python programs spend almost all their time in two of the four layers: the locked rooms (where workstations do their work) and the open aisle (where constants and a small amount of shared state live). The anteroom and the standard kit are present in every program but rarely require explicit thought once you understand the LEGB rule.

---

## Quick Reference

| Python | Shelf image |
|---|---|
| `x = value` (top level) | Place a shelf in the **open aisle**. |
| `x = value` (inside a workstation) | Place a shelf in the **locked room**, regardless of whether one with that name exists outside. |
| `x` (read) | Walk outward — locked room → anterooms → open aisle → standard kit. First match wins. |
| `global x` | "In this workstation, `x` refers to the open-aisle shelf." Use sparingly. |
| `nonlocal x` | "In this nested workstation, `x` refers to the nearest anteroom shelf." Used for closures. |
| `def inner(): ...` (inside another `def`) | A nested workstation. The outer workstation's locked room becomes its **anteroom**. |
| `print`, `len`, `int`, `list`, `range`, ... | The **standard kit** — pre-installed; do not shadow them with your own shelves. |
| `NameError` on a read | Python walked all four layers and found no shelf with that name. |
| `UnboundLocalError` | Python saw a write to that name inside the workstation, reserved a locked-room shelf, then encountered a read before the placement happened. |

---

## Try It

Open a Python prompt or any Python editor.

**A locked room and what survives:**

```python
def double(x):
    result = x * 2
    return result

print(double(5))
print(result)         # NameError — try it and see the alarm
```

**Reading from the open aisle:**

```python
PI = 3.14159

def area(r):
    return PI * r * r

print(area(10))
```

**The classic write-without-`global` mistake:**

```python
counter = 0

def increment():
    counter = counter + 1     # UnboundLocalError — Python decided counter is local

increment()
```

Run it once to feel the alarm. Then fix it the clean way (no `global`):

```python
def increment(counter):
    return counter + 1

counter = 0
counter = increment(counter)
counter = increment(counter)
print(counter)
```

And see the `global` version too, for comparison:

```python
counter = 0

def increment():
    global counter
    counter += 1

increment()
increment()
print(counter)
```

Both produce `2`. The second style — the one without `global` — is preferable in almost every situation.

**A nested workstation reading from its anteroom:**

```python
def outer():
    message = "from the anteroom"
    def inner():
        print(message)
    inner()

outer()
```

**A closure with `nonlocal`:**

```python
def make_counter():
    count = 0
    def tick():
        nonlocal count
        count += 1
        return count
    return tick

counter = make_counter()
print(counter())
print(counter())
print(counter())
```

The `count` cubbyhole survives every call because `tick` is still using `make_counter`'s locked room as an anteroom.

**The shadow trap:**

```python
list = [1, 2, 3]
print(list)
print(list("abc"))     # TypeError — list is now a row, not the mould
```

Recover by deleting the shadowing shelf:

```python
del list
print(list("abc"))     # ['a', 'b', 'c'] — the mould is back
```

This is exactly why you should not shadow built-in names in the first place.

---

## Where Next?

You now have the full picture of how shelves work — types, contents, mutability, and the four layers of visibility. The remaining warehouse lessons fill in the last two collection items: dictionaries (filing cabinets) and the smaller pair of tuples and sets.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 10 | Z3 — Warehouse | Dictionaries — filing cabinets in depth |
| Lesson 11 | Z3 — Warehouse | Tuples and Sets — sealed crates and unsorted bins |
| Lesson 12 | Z5 — Factory Floor | Operators — the tools on the floor |
| Lesson 13 | Z5 — Factory Floor | If / Else — junctions and inspection gates |

*See the full lesson map in **The Factory — A Complete Map**.*
