# Python for Hyperphantasic Minds
## Lesson 15 — Comprehensions

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 15 of 25  
> **Topic**: Comprehensions — compact belt expressions  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still on the Factory Floor. The last lesson ended with five named patterns built on top of a `for` belt — accumulator, find-max, build-a-row, count-by-condition, filter. Two of those — build-a-row and filter — turn out to be so common in Python that the language provides a *compact* version of each, fitting on a single readable line.

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

Today's construct is the **compact belt** — a single pre-assembled production unit where items enter at one end and a finished collection arrives at the other in a single sealed motion. No accumulator shelf visible, no intermediate `.append()` calls, no `result = []` before the loop. Python calls these *comprehensions*.

---

## From Stage-Built to Compact Belt

Recall the *build-a-row* pattern from Lesson 14:

```python
numbers = [1, 2, 3, 4, 5]

squares = []                           # an empty row to fill
for n in numbers:                      # walk the belt
    squares.append(n * n)              # add the squared stone to the row

print(squares)                         # [1, 4, 9, 16, 25]
```

Three lines, three explicit stages: declare an empty row, walk the input, append to the row each pass. Picture: a worker stands beside the belt with a fresh row on a wheeled frame, taking each squared stone off the workstation and bolting a new cubbyhole on for it.

Python provides a compact form that does the same job in a single line:

```python
squares = [n * n for n in numbers]
print(squares)                         # [1, 4, 9, 16, 25]
```

Picture: a single sealed unit on the floor. Items from `numbers` enter one end; squared stones come out the other end, already arranged in a numbered row. The fresh row, the loop, and the appends are all hidden inside the unit. From the outside, you see input and output only.

This is a **list comprehension** — a compact belt that produces a numbered row.

The structure to memorise:

```
[ expression  for  variable  in  collection ]
```

Read aloud: *"a row of expression, for each variable in collection."*

In our example: *"a row of `n * n`, for each `n` in `numbers`."*

Everything inside the square brackets is one sealed unit; everything that would have been an explicit `result.append(...)` in the loop form is implicit. The result is a brand-new row.

---

## Adding a Filter

The fourth pattern from Lesson 14 — *filter* — adds an `if` clause to the comprehension. Items that fail the condition are dropped before the expression is applied.

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

evens = [n for n in numbers if n % 2 == 0]
print(evens)                           # [2, 4, 6, 8, 10]
```

The structure:

```
[ expression  for  variable  in  collection  if  condition ]
```

Read aloud: *"a row of expression, for each variable in collection where condition holds."*

Picture: the compact belt has an inspection gate just before the workstation. Each item is checked; failing items drop out and never reach the workstation. Only items that pass continue through and contribute to the finished row.

You can combine the *transform* and *filter* roles in one comprehension — the most common comprehension shape:

```python
big_squares = [n * n for n in numbers if n > 5]
print(big_squares)                     # [36, 49, 64, 81, 100]
```

Read aloud: *"a row of `n * n`, for each `n` in `numbers` where `n > 5`."*

The order in the syntax — *expression first, then `for`, then `if`* — looks unusual at first, but is the order Python settled on. After reading a few hundred comprehensions it becomes the natural form.

---

## Multiple `for` Clauses — Nested Belts in Compact Form

A nested `for` loop (Lesson 14) walks two collections at once: an outer belt whose every pass triggers a full pass of an inner belt. The compact form supports the same structure, with two `for` clauses:

```python
pairs = [(x, y) for x in [1, 2, 3] for y in ['a', 'b']]
print(pairs)
# [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b'), (3, 'a'), (3, 'b')]
```

Read aloud: *"a row of `(x, y)`, for each `x` in `[1, 2, 3]`, for each `y` in `['a', 'b']`."*

The order matches the loop form: the *outer* belt is first; the *inner* belt is second.

You can also filter at this level — flattening a grid into the items that meet a condition:

```python
grid = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

evens = [n for row in grid for n in row if n % 2 == 0]
print(evens)                           # [2, 4, 6, 8]
```

This reads as: *"a row of `n`, for each `row` in `grid`, for each `n` in `row`, where `n` is even."* Two `for` clauses, one `if` clause, one expression — the whole thing on one line.

Multiple-`for` comprehensions are useful but easy to over-pack. When they grow beyond two `for` clauses or include several conditions, the loop form (Lesson 14) is usually clearer. We will return to this in the *When Not to Use* section.

---

## Dictionary Comprehensions

The compact-belt pattern works for filing cabinets too, producing a `dict` directly:

```python
numbers = [1, 2, 3, 4, 5]
squares = {n: n * n for n in numbers}
print(squares)                         # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

Structure:

```
{ key_expr : value_expr  for  variable  in  collection [if condition] }
```

Picture: a compact belt whose finished product is a filing cabinet, not a row. Each item that passes through produces a key-value pair, which is bolted onto the cabinet as a labelled drawer.

A common pattern: inverting a cabinet (swapping keys and values):

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}
by_price = {value: label for label, value in prices.items()}
print(by_price)                        # {0.5: 'apple', 2.4: 'bread', 1.2: 'milk'}
```

`.items()` from Lesson 14 yields `(label, value)` crates; the comprehension unpacks each crate at the start of its pass, then builds a new cabinet with the swap. Note this only works cleanly when the original values were unique — if two labels shared a value, the inversion would silently lose one of them. (Same one-label-only-per-drawer rule from Lesson 10.)

A filter clause works the same as in list comprehensions:

```python
affordable = {label: value for label, value in prices.items() if value < 2.00}
print(affordable)                      # {'apple': 0.5, 'milk': 1.2}
```

---

## Set Comprehensions

The compact belt can also produce an unsorted bin:

```python
words = ["alpha", "beta", "alpha", "gamma", "beta", "alpha"]
unique = {w for w in words}
print(unique)                          # {'alpha', 'beta', 'gamma'}
```

Structure:

```
{ expression  for  variable  in  collection [if condition] }
```

Curly brackets with one expression (not a `key: value` pair). The empty-bin caveat from Lesson 11 still applies — `{}` alone is a cabinet, not a bin. A set comprehension always has at least one `for` clause, so no ambiguity arises in practice.

Set comprehensions are useful when you want the deduplication that bins provide *as part of* the build:

```python
first_letters = {word[0].upper() for word in words if word}
print(first_letters)                   # {'A', 'B', 'G'}
```

Three rules apply at once: take the first letter, uppercase it, drop empty scrolls, deduplicate. All in one compact belt.

---

## A Brief Look at Generator Expressions

There is a fourth comprehension form, with **round** brackets:

```python
squares_gen = (n * n for n in numbers)
print(squares_gen)                     # <generator object ...>
```

This is *not* a sealed-crate comprehension (Python does not provide one — that form is reserved for generator expressions). What you get is a **generator** — a workstation that produces one item at a time, on demand, instead of building the whole collection at once.

Picture: a belt that does *not* run unless something asks for the next item. The first request makes it run one pass and hand over one squared stone. The next request makes another pass. When the source is exhausted, the belt reports it is done.

```python
squares_gen = (n * n for n in numbers)
print(next(squares_gen))               # 1
print(next(squares_gen))               # 4
print(next(squares_gen))               # 9
```

Or feed it to a workstation that consumes a sequence:

```python
print(sum(n * n for n in range(1, 11)))   # 385 — sum of squares 1²…10²
```

When a generator expression is the only argument to a workstation like `sum`, `min`, `max`, `any`, `all`, you can drop the outer parentheses, as above. The result is exceptionally compact and reads well.

Why use the generator form? Memory. A list comprehension over a million items builds a million-item row in memory all at once. A generator expression over the same source produces one item at a time, holding nothing extra — useful when the source is large or you only care about an aggregate of the results.

The full machinery of generators — `yield`, generator workstations defined with `def` — comes in Lesson 18. For now, recognise the round-bracket form and know that it is *lazy*: nothing happens until something asks.

---

## When NOT to Use a Comprehension

Comprehensions are wonderful when they fit on a single readable line. They become a liability when they do not.

Two warning signs that you should rewrite as a loop:

**Sign 1: The line wraps.** If a comprehension does not fit on one line of normal-width source code, the loop form is almost certainly clearer:

```python
# Hard to read:
results = [
    transform(x)
    for x in items
    if x.is_valid and x.score > threshold and x.user.active
]

# Clearer as a loop:
results = []
for x in items:
    if x.is_valid and x.score > threshold and x.user.active:
        results.append(transform(x))
```

The loop's vertical structure matches the reading order of the work; the comprehension's compact form ceased to be an advantage once it stopped being compact.

**Sign 2: Side effects.** A comprehension's purpose is to *produce a collection*. If the work inside is doing something else — printing, modifying shelves, sending output — write a loop. Using a comprehension purely for its side effects is a stylistic smell:

```python
# Bad — using a comprehension for its side effect:
[print(x) for x in items]              # the result is a row of None; not needed

# Good:
for x in items:
    print(x)
```

The loop says what is happening. The comprehension hides it.

A useful rule of thumb: **if the comprehension's result is going to be ignored or discarded, you wanted a loop.**

---

## What You Now Know

You can build any of the four comprehension forms — list, dict, set, generator — and add filters to each. You know that the structure inside the brackets reads naturally from left to right as *"a [collection] of expression, for each variable in source, where condition holds"*. You know the compact-belt metaphor: input enters, finished collection emerges, no intermediate accumulator visible.

You also know the readability cliff. Comprehensions are a tool, not a goal. When they fit on one line and produce a result that will be used, they are clearer than the loop form they replace. When they do not, the loop wins.

Comprehensions appear in essentially every nontrivial Python codebase. Build the visual model — *compact belt, item in, collection out* — and you will read them at a glance.

---

## Quick Reference

| Python | Compact-belt image |
|---|---|
| `[expr for x in coll]` | List comprehension — produces a numbered row. |
| `[expr for x in coll if cond]` | …with a filter; only items where `cond` is truthy pass through. |
| `[expr for x in c1 for y in c2]` | Nested — outer belt walks `c1`, inner belt walks `c2` per pass. |
| `{k: v for x in coll}` | Dict comprehension — produces a filing cabinet. |
| `{expr for x in coll}` | Set comprehension — produces an unsorted bin. |
| `(expr for x in coll)` | Generator expression — *lazy*; produces items on demand, not all at once. |
| `sum(n*n for n in r)` | Drop the outer parens when the generator is the sole argument. |
| Use a loop when… | The comprehension wraps to a second line, or has side effects. |

---

## Try It

**The build-a-row pattern, before and after:**

```python
numbers = [1, 2, 3, 4, 5]

# Loop form
squares = []
for n in numbers:
    squares.append(n * n)
print(squares)

# Comprehension form
squares = [n * n for n in numbers]
print(squares)
```

Both produce the same row. The comprehension hides the accumulator.

**With a filter:**

```python
numbers = list(range(1, 11))
evens = [n for n in numbers if n % 2 == 0]
print(evens)

big_squares = [n * n for n in numbers if n > 5]
print(big_squares)
```

**Walking a scroll:**

```python
vowels = [c for c in "hello world" if c in "aeiou"]
print(vowels)                           # ['e', 'o', 'o']

upper = [c.upper() for c in "hello"]
print(upper)                            # ['H', 'E', 'L', 'L', 'O']
```

The scroll yields characters one at a time, just as in Lesson 14.

**Flattening a grid:**

```python
grid = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

flat = [n for row in grid for n in row]
print(flat)                             # [1, 2, 3, 4, 5, 6, 7, 8, 9]

evens_only = [n for row in grid for n in row if n % 2 == 0]
print(evens_only)                       # [2, 4, 6, 8]
```

**Dict comprehension — inversion and filtering:**

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}

by_price = {value: label for label, value in prices.items()}
print(by_price)

affordable = {label: value for label, value in prices.items() if value < 2.00}
print(affordable)
```

**Set comprehension — dedupe and transform:**

```python
words = ["alpha", "beta", "alpha", "gamma"]
first_letters = {word[0].upper() for word in words}
print(first_letters)
```

**Generator expression with `sum`:**

```python
total = sum(n * n for n in range(1, 11))
print(total)                            # 385

biggest = max(len(w) for w in ["alpha", "beta", "elephant", "x"])
print(biggest)                          # 8
```

The outer parentheses are dropped because the generator is the sole argument.

**Try the bad form once, deliberately:**

```python
# Anti-pattern: comprehension for side effects only
result = [print(x) for x in [1, 2, 3]]
print(result)                           # [None, None, None]
```

Notice the row of `None`s — every `print` returned a vacant cubbyhole, and the comprehension collected them. Almost certainly not what you wanted. A loop is the right tool here.

---

## Where Next?

The next lessons leave belts behind and turn to the largest construct on the Factory Floor: the **workshop blueprint** and its built workshops. Classes, instances, attributes, the `self` slot, and inheritance — all the machinery for designing your own kinds of items.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 16 | Z5 — Factory Floor | Classes — workshop blueprints and built workshops |
| Lesson 17 | Z5 — Factory Floor | Inheritance — extending a blueprint |
| Lesson 18 | Z5 — Factory Floor | Iterators and Generators — passing one item and waiting |
| Lesson 19 | Z5 — Factory Floor | Lambda and Decorators — impromptu workstations and wrappers |

*See the full lesson map in **The Factory — A Complete Map**.*
