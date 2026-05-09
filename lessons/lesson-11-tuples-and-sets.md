# Python for Hyperphantasic Minds
## Lesson 11 — Tuples and Sets

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 11 of 25  
> **Topic**: Tuples and Sets — sealed crates and unsorted bins  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Last day in the Warehouse. The remaining two collection items — the **sealed crate** and the **unsorted bin** — are smaller in everyday use than scrolls, numbered rows, or filing cabinets, but each has a specific job that nothing else does as well. After today, the warehouse tour is complete and you head out to the Factory Floor for the rest of the series.

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

The lesson is in two halves. First the sealed crate (tuple). Then the unsorted bin (set). They are unrelated as physical items but share a property worth flagging at the start: each fixes one specific weakness of an item you already know.

- The sealed crate is a *fixed* group of items — useful where the numbered row's mutability is an obstacle, or where the items being grouped are *fields of one thing* rather than *a sequence of similar things*.
- The unsorted bin is a *unique-and-fast-to-search* collection — useful when the question being asked is always *"have I seen this before?"*

---

# Part 1 — The Sealed Crate (`tuple`)

## The Sealed Crate, Looked At Closely

You first met the sealed crate in Lesson 3. The visual was: *a wooden crate that has been nailed shut. Items packed inside are visible through gaps in the slats. Cannot be repacked. Weight is fixed at packing time.*

Picture a crate sitting on a warehouse shelf. Through the slats you can see the contents — perhaps a stone marked `3` and a stone marked `4`, or three scrolls reading `"red"`, `"green"`, `"blue"`. The lid is nailed down. There is no way to open it without destroying it. To get a *different* crate, you build a new one.

```python
point = (3, 4)
colours = ("red", "green", "blue")
person = ("Shane", 27, True)
empty = ()
```

Round brackets define a sealed crate. Items inside are separated by commas.

**The single-element gotcha.** A crate with exactly one item needs a trailing comma:

```python
not_a_crate = (5)              # this is just the stone 5 in parentheses
also_not = ("hello")            # this is just the scroll "hello"

really_a_crate = (5,)           # one-item sealed crate
also_really = ("hello",)        # one-item sealed crate
```

Without the trailing comma, the parentheses are read as ordinary grouping (the kind you use in `(2 + 3) * 4`). With the comma, Python recognises the construct as a sealed crate. This trips up almost everyone the first time. Remember it once and never again.

A more relaxed alternative — and what most Python programmers actually write — is the form *without* the brackets at all:

```python
point = 3, 4              # Python recognises this as a sealed crate
person = "Shane", 27, True

x, y = point              # unpacking — same as Lesson 1's parallel placement
```

The brackets are technically optional in many positions; the comma is what makes the crate. Most prose-style examples in this series will use the bracketed form for clarity.

---

## Reading From a Sealed Crate

The crate is a sequence, just like the scroll and the numbered row, with positions numbered from zero. Indexing, slicing, length, and `in` all behave the same way:

```python
point = (3, 4, 5)
print(point[0])             # 3
print(point[-1])            # 5
print(point[0:2])           # (3, 4)   — slicing produces a new sealed crate
print(len(point))           # 3
print(4 in point)           # True
```

Nothing surprising here. If you need a refresher, the rules from Lessons 6 and 7 transfer directly.

The crate has only two built-in tools — far fewer than the row or scroll, because there is no editing to support:

```python
point = (3, 4, 4, 5, 4)
print(point.count(4))       # 3
print(point.index(5))       # 3
```

`count(x)` reports how many copies of `x` are in the crate; `index(x)` reports the position of the first.

---

## Sealed Means Sealed

```python
point = (3, 4)
point[0] = 99                  # TypeError — sealed crates do not support assignment
point.append(5)                # AttributeError — there is no append for crates
```

Both are alarms. The crate cannot be modified after construction. There is no `append`, no `remove`, no `sort`, no `clear`. The only way to get a "different" crate is to build a new one:

```python
point = (3, 4)
new_point = point + (5,)        # produces a NEW crate (3, 4, 5)
print(point)                    # (3, 4)   — original unchanged
```

`+` works on crates the way it does on scrolls and rows — it produces a new crate by joining two existing ones end to end.

---

## Why Use a Crate When a Row Would Do?

A common beginner question: *"if a crate is just a row that can't change, why ever use one?"*

Two answers, both rooted in *meaning* rather than mechanics.

**The crate signals "fixed group" to the reader.** When you see `(3, 4)` in code, you know the author meant *one thing made of two parts* — a point, a coordinate, an (x, y) pair. When you see `[3, 4]`, you read *a sequence that may grow or shrink*. The brackets carry meaning beyond what they technically enforce. Programmers reaching for tuples do so to *tell the reader* that this group is conceptually a single fact, not a list.

**The crate is allowed where the row is not.** A sealed crate is *immutable* (in the Lesson 6 sense), which means it can be used as a filing-cabinet drawer label and as a member of an unsorted bin. A numbered row cannot:

```python
locations = {(0, 0): "origin", (1, 0): "east", (0, 1): "north"}
print(locations[(0, 0)])             # 'origin'

unique_pairs = {(1, 2), (3, 4), (1, 2)}
print(unique_pairs)                  # {(1, 2), (3, 4)}   — duplicates rejected as expected
```

Try either of those with `[0, 0]` or `[1, 2]` instead of `(0, 0)` or `(1, 2)` and you get the alarm — *unhashable type: 'list'*. The crate is allowed precisely because it cannot change.

---

## Unpacking — The Crate's Best Trick

You first met parallel placement in Lesson 1: `a, b, c = 1, 2, 3`. That is a sealed crate being created on the right-hand side and *unpacked* on the left:

```python
point = (3, 4)
x, y = point
print(x, y)                  # 3 4
```

The two cubbyholes on the left receive the two items from the crate, in order. Picture: the crate is opened (mentally), and each item is placed in the matching position on the left.

Unpacking has to match the crate's length — usually:

```python
point = (3, 4, 5)
x, y = point                 # ValueError — three items into two slots
x, y, z = point              # OK
```

The exception is *extended unpacking* with a star — handy when you want one or two specific items and "everything else" in a numbered row:

```python
first, *rest = (10, 20, 30, 40)
print(first)                 # 10
print(rest)                  # [20, 30, 40]   — a numbered row, not a crate
```

The `*` collects whatever is left over into a row. There can only be one star per unpacking. This is occasionally exactly what you want; recognise the form when you see it.

---

## A Workstation Returning Many Things

You saw this in Lesson 8 already. When a workstation needs to send back several values at once, it returns a sealed crate, and the caller unpacks at the moment of assignment:

```python
def stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

low, high, average = stats([3, 1, 4, 1, 5, 9, 2, 6])
print(low, high, average)              # 1 9 3.875
```

The `return` line builds a sealed crate of three items and places it on the output belt. The receiving line unpacks the crate into three named cubbyholes. This is one of the tuple's daily uses, and one of the most distinctively Pythonic patterns.

---

# Part 2 — The Unsorted Bin (`set`)

## The Unsorted Bin, Looked At Closely

You met the bin in Lesson 3. The visual was: *a large open bin. Items thrown in — no order, no duplicates allowed (duplicates simply fail to land). You can check whether something is in the bin but cannot retrieve items by position.*

Picture a large open bin on a warehouse shelf. Items are tossed in without ceremony; they pile up in no particular order. Two strict rules govern the bin:

- **No duplicates.** If you try to add something already in the bin, the bin quietly does nothing. There is no second copy.
- **No positions.** You cannot ask for "the first item" — the bin has no first. You can ask whether a given item is present, but not where.

```python
unique = {1, 2, 3, 2, 1, 1, 1, 3}
print(unique)                # {1, 2, 3}   — duplicates failed to land
```

Curly brackets define a bin. Items inside are separated by commas.

**The empty-bin gotcha.** Empty curly brackets do *not* make an empty bin — they make an empty filing cabinet, because curly brackets were taken first by Lesson 10's cabinets. To make an empty bin, you must use the cabinet-named-`set` mould:

```python
not_a_bin = {}                # an empty cabinet (dict)
print(type(not_a_bin))        # <class 'dict'>

empty_bin = set()             # an empty bin
print(type(empty_bin))        # <class 'set'>
```

This is one Python quirk worth meeting once and remembering forever.

---

## Adding and Removing

The bin is mutable. Items can be added and removed.

**`.add(x)` — toss in one item.**

```python
seen = set()
seen.add("alice")
seen.add("bob")
seen.add("alice")            # already in the bin — does nothing
print(seen)                  # {'alice', 'bob'}
```

Picture: the worker walks up and tosses an item into the bin. The bin checks whether it is already there; if not, it joins the pile.

**`.update(other)` — toss in many items at once.**

```python
seen = {"alice"}
seen.update(["bob", "cara", "alice"])
print(seen)                  # {'alice', 'bob', 'cara'}
```

The argument can be any collection (a numbered row, a sealed crate, another bin, even a scroll — though a scroll is treated as a sequence of single-character scrolls).

**`.remove(x)` and `.discard(x)` — take an item out.**

```python
seen = {"alice", "bob", "cara"}
seen.remove("bob")           # OK
print(seen)                  # {'alice', 'cara'}

seen.remove("dave")          # KeyError — was not in the bin
seen.discard("dave")         # OK — discard never raises
```

The pair `remove` and `discard` differ only in their behaviour when the item is missing. `remove` triggers the alarm; `discard` quietly does nothing. Choose based on whether the item's absence is a problem.

**`.pop()` — remove and return any item.**

```python
seen = {"alice", "bob", "cara"}
who = seen.pop()
print(who)                   # any one of the three — bin has no order
print(seen)                  # the other two
```

Unlike rows, where `pop()` defaults to the last item, the bin's `pop` returns *some* item — there is no notion of "first" or "last". Useful only when you do not care which item you remove.

**`.clear()` — empty the bin in place.**

```python
seen.clear()
print(seen)                  # set()
```

---

## Membership — The Bin's Killer Feature

Here is the practical reason bins exist.

Asking *"is this item in this collection?"* of a numbered row requires walking the row item by item:

```python
big_row = list(range(1_000_000))
print(999_999 in big_row)          # True — but the worker walked the whole row
```

Asking the same question of an unsorted bin is *almost instant*, regardless of size:

```python
big_bin = set(range(1_000_000))
print(999_999 in big_bin)          # True — instantaneous
```

The bin uses a different internal arrangement (a *hash table*; an implementation detail that you do not need to picture in detail) that lets it find items without walking. For a small collection, the difference is invisible. For a collection of any meaningful size, it is dramatic.

If your program asks "have I seen this?" or "is this on the allowed list?" or "did this email already arrive today?" — and especially if it asks those questions repeatedly — the right collection is almost always an unsorted bin.

---

## The Bin's Algebra — Set Operations

Bins were invented for the mathematical concept of a *set*, and Python supports the four set-algebra operations directly. Picture each as a procedure involving two bins, side by side.

**Union — `|`**

```python
a = {1, 2, 3}
b = {3, 4, 5}
print(a | b)                # {1, 2, 3, 4, 5}
```

Picture: tip both bins into one large bin. Duplicates merge automatically.

**Intersection — `&`**

```python
print(a & b)                # {3}
```

Picture: the worker walks past the first bin, item by item. For each item, they check whether it is also in the second bin. If yes, it goes into a fresh new bin. If no, it is left alone. The new bin holds only items present in *both*.

**Difference — `-`**

```python
print(a - b)                # {1, 2}
print(b - a)                # {4, 5}
```

Picture: items in the first bin that are *not* in the second bin. Note this is asymmetric — `a - b` is not the same as `b - a`.

**Symmetric difference — `^`**

```python
print(a ^ b)                # {1, 2, 4, 5}
```

Picture: items in exactly one of the two bins — present in either, but not both. The "merged minus the overlap".

These four operators come in tool-form variants too: `a.union(b)`, `a.intersection(b)`, `a.difference(b)`, `a.symmetric_difference(b)`. The operator forms are usually clearer; the tool forms are useful when the right-hand side is not itself a bin (the tool forms accept any collection).

---

## A Quick Word on `frozenset`

The numbered row had a sealed-version cousin (the tuple). The unsorted bin has one too: `frozenset` — a bin that, once made, cannot be added to or removed from. It exists for one specific reason: where you want to use a bin as a filing-cabinet label or as a member of another bin.

```python
permanent_tags = frozenset({"new", "urgent"})
records = {permanent_tags: "high priority"}     # frozenset can be a dict key
```

You will rarely need it as a beginner; recognise the form when you see it. The everyday item is the regular `set`.

---

# Putting Them Side by Side

The four collections of the warehouse, summarised by character:

| Collection | Item | Ordered? | Mutable? | Duplicates? | Indexed by | Best for |
|---|---|---|---|---|---|---|
| `list` | Numbered row | Yes | Yes | Yes | Position | Sequences that grow and shrink |
| `tuple` | Sealed crate | Yes | No | Yes | Position | Fixed groups; coordinates; multi-return |
| `set` | Unsorted bin | No | Yes | No | (none) | Membership tests; deduping |
| `dict` | Filing cabinet | Insertion-ordered | Yes | (keys unique) | Key | Labelled records; mapping |

Most data-shaping work in real Python uses the row and the cabinet most heavily. The crate appears at function boundaries (multi-return) and as a record-shape signal. The bin appears wherever uniqueness or fast lookup matters.

---

## What You Now Know

The warehouse tour is complete. You have looked carefully at every kind of item the factory uses — five single-value items (stone, vial, scroll, switch, vacant cubbyhole) and four collection items (numbered row, sealed crate, unsorted bin, filing cabinet). Today's two are the smallest in everyday use but each fills a real role: the sealed crate makes a group of items behave as a single fact; the unsorted bin makes membership questions vanish.

You also know enough Python now to write programs of meaningful complexity. From the next lesson on, the series moves out of the warehouse and onto the Factory Floor, where the operators, junctions, conveyor belts, and workshop blueprints turn stored items into actual production.

The Warehouse will not vanish from your view. Every program you write spends most of its time placing items there, reading them out, and replacing them. But the *shape* of what happens inside the program — the choices, the loops, the workshops — is the Factory Floor's territory.

---

## Quick Reference

**Sealed Crate (`tuple`)**

| Python | Crate image |
|---|---|
| `(1, 2, 3)`, `(5,)`, `()` | A nailed-shut crate; round brackets define one. |
| `1, 2, 3` (no brackets) | Also a crate — the comma makes it. |
| `crate[i]`, `crate[a:b]` | Read by position, like the row or scroll. |
| `len(crate)`, `x in crate` | Length and membership. |
| `crate.count(x)`, `crate.index(x)` | Count copies, find first position. |
| `crate[0] = x` | **TypeError.** Sealed; produce a new crate instead. |
| `x, y = (1, 2)` | Unpack — items into matching slots on the left. |
| `first, *rest = (10, 20, 30)` | Extended unpack; `*rest` collects the remainder into a row. |
| `def f(): return a, b, c` | Multi-value return packs into a sealed crate. |

**Unsorted Bin (`set`)**

| Python | Bin image |
|---|---|
| `{1, 2, 3}`, `set()` | Curly brackets with items; **`set()` for empty** (`{}` is a cabinet). |
| `bin.add(x)` | Toss in one item; duplicates fail to land. |
| `bin.update(other)` | Toss in many items at once. |
| `bin.remove(x)` | Take out; KeyError if missing. |
| `bin.discard(x)` | Take out; silent if missing. |
| `bin.pop()` | Remove and return any one item. |
| `bin.clear()` | Empty in place. |
| `x in bin` | Membership — instantaneous regardless of bin size. |
| `a \| b` | Union — both bins combined. |
| `a & b` | Intersection — items in both. |
| `a - b` | Difference — in `a`, not in `b`. |
| `a ^ b` | Symmetric difference — in exactly one. |
| `frozenset({...})` | Sealed bin; usable as a cabinet label. |

---

## Try It

**Sealed crates:**

```python
point = (3, 4)
print(point)
print(point[0], point[1])
print(len(point))
print(4 in point)

x, y = point
print(x, y)

# point[0] = 99            # TypeError — try once and feel the alarm

new_point = point + (5,)
print(new_point)
print(point)               # original unchanged
```

**The single-element gotcha:**

```python
print(type((5)))           # <class 'int'>
print(type((5,)))          # <class 'tuple'>
```

**Multi-value return — sealed crate at the boundary:**

```python
def stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

low, high, avg = stats([3, 1, 4, 1, 5, 9, 2, 6])
print(low, high, avg)
```

**Unsorted bins — the empty-bin gotcha:**

```python
print(type({}))            # <class 'dict'>
print(type(set()))         # <class 'set'>
```

**Adding and removing:**

```python
seen = {"alice"}
seen.add("bob")
seen.add("alice")          # already in — does nothing
seen.update(["cara", "dave"])
print(seen)

seen.discard("zelda")      # safe
seen.remove("bob")
print(seen)
```

**Membership — the killer feature:**

```python
big_row = list(range(1_000_000))
big_bin = set(range(1_000_000))

print(999_999 in big_row)   # works, but slower
print(999_999 in big_bin)   # instant
```

You probably will not feel the difference at this scale on a modern machine, but the underlying behaviour is dramatically different. For repeated membership questions, the bin is the right shape.

**Set algebra:**

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)         # union
print(a & b)         # intersection
print(a - b)         # difference (asymmetric)
print(b - a)
print(a ^ b)         # symmetric difference
```

**Deduping a row:**

```python
emails = ["alice@example.com", "bob@example.com", "alice@example.com"]
unique = list(set(emails))
print(unique)
```

The classic three-step pattern: row → bin (drops duplicates) → row (back into a sequence). Note that the order is not preserved, because the bin has no order.

---

## Where Next?

The next lesson takes you out of the Warehouse for a long stay. Phase 3 is the Factory Floor in full — operators, junctions, conveyor belts, comprehensions, classes, inheritance, generators, lambda, and decorators. Eight lessons; the largest phase in the series.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 12 | Z5 — Factory Floor | Operators — the tools on the floor |
| Lesson 13 | Z5 — Factory Floor | If / Else — junctions and inspection gates |
| Lesson 14 | Z5 — Factory Floor | Loops — conveyor belts, emergency stops, skip gates |
| Lesson 15 | Z5 — Factory Floor | Comprehensions — compact belt expressions |

*See the full lesson map in **The Factory — A Complete Map**.*
