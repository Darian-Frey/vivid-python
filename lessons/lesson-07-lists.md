# Python for Hyperphantasic Minds
## Lesson 7 — Lists

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 7 of 25  
> **Topic**: Lists — numbered rows in depth  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still in the Warehouse. The scroll is behind you; the **numbered row** is in front of you. The two items have a great deal in common — both are sequences, both can be indexed and sliced, both have a length, both answer the `in` question. Today's lesson will lean on that recent experience. The biggest difference is the property the scroll *did not* have: a numbered row can be edited.

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

---

## The Numbered Row, Looked At Closely

You first met the numbered row in Lesson 3. The visual was: *a row of small cubbyholes bolted together on a wheeled frame, numbered from zero. Each cubbyhole within the row holds one item. The frame can be extended.*

Picture it now in detail. A long wheeled frame sits on a single warehouse shelf — the frame is the *row* itself, a single item that the warehouse is keeping track of. The card on the shelf reads, say, `scores`. Bolted onto the frame are smaller cubbyholes, each numbered: `0`, `1`, `2`, `3`, and so on. Each numbered cubbyhole holds one item — usually all of the same kind, though the frame does not insist on it.

```python
scores = [10, 20, 30, 40]
```

That single line places a wheeled frame on the `scores` shelf, with four cubbyholes bolted to it, holding stones marked `10`, `20`, `30`, `40` in that order.

```
position:   0    1    2    3
contents:  10   20   30   40
```

The square brackets define a numbered row literally. Items inside are separated by commas. To create an empty row, write `[]` — a wheeled frame with no cubbyholes yet bolted on.

**Items in a row do not have to be of one kind.**

```python
mixed = [1, "two", 3.0, True, None]
```

The frame accepts any item the factory uses. You will mostly see rows that hold items of one kind (a row of stones, a row of scrolls), but the frame itself is universal — like the cubbyholes of the warehouse, but in miniature.

---

## Length, Indexing, Slicing — The Same Operations

Almost everything you learned about scrolls applies here unchanged.

```python
scores = [10, 20, 30, 40]

print(len(scores))           # 4         — length of the row

print(scores[0])             # 10        — first item
print(scores[-1])            # 40        — last item
print(scores[1:3])           # [20, 30]  — slice — a new shorter row

print(20 in scores)          # True      — membership question
print(99 in scores)          # False
```

`len`, indexing with both positive and negative positions, slicing with start/end/step, the `in` operator — all behave exactly as they did for scrolls. If you need a refresher, return to Lesson 6; the rules transfer directly.

A small but useful note: a slice of a row is itself a *new row*. Slicing produces a fresh wheeled frame with copies of the selected items bolted onto it. The original row is unchanged. This is the same rule as scroll slicing.

---

## A Numbered Row Can Be Edited

Here is the deep difference from the scroll.

A scroll, once written, is sealed. Any operation that *seems* to change a scroll actually produces a new one. A numbered row is the opposite: the wheeled frame stays where it is, and the items in its cubbyholes can be removed, replaced, added to, or rearranged. The frame is the same frame the whole time.

**Replacing one item:**

```python
scores = [10, 20, 30, 40]
scores[1] = 99
print(scores)               # [10, 99, 30, 40]
```

Picture: the worker reaches into the cubbyhole at position `1`, lifts out the `20` stone, and replaces it with a `99` stone. The frame is unchanged. The card on the shelf still reads `scores`. Only the contents of one cubbyhole have been swapped.

This is why we call this property **mutability**: the row can *mutate* — change in place — while remaining the same row. Compare with a scroll, where `greeting[0] = "J"` triggers an alarm.

**Replacing a slice:**

```python
scores = [10, 20, 30, 40]
scores[1:3] = [88, 99]
print(scores)               # [10, 88, 99, 40]
```

Picture: the worker pops out two cubbyholes at positions `1` and `2`, bolts in two new ones containing `88` and `99`. The frame is the same; two cubbyholes have been swapped out.

A slice replacement can change the *length* of the row. Replace two cubbyholes with three, and the row grows. Replace three with one, and the row shrinks:

```python
scores = [10, 20, 30, 40]
scores[1:3] = [55, 66, 77, 88]
print(scores)               # [10, 55, 66, 77, 88, 40]   — row is now 6 long
```

This is the most flexible single operation in the row's whole repertoire.

---

## Adding to a Numbered Row

Three built-in tools handle the everyday job of adding new items.

**`append` — add one item to the end.**

```python
scores = [10, 20]
scores.append(30)
print(scores)               # [10, 20, 30]
scores.append(40)
print(scores)               # [10, 20, 30, 40]
```

Picture: a new cubbyhole is bolted onto the right-hand end of the frame, holding the new item. The row grows by one. This is the most-used row tool by a wide margin.

**`extend` — add several items in one go.**

```python
scores = [10, 20]
scores.extend([30, 40, 50])
print(scores)               # [10, 20, 30, 40, 50]
```

Picture: three new cubbyholes are bolted onto the end at once, each holding one item from the source. This is faster (and reads more clearly) than calling `append` three times.

A subtle point worth catching once: `extend` adds the *items* of its argument to the row. `append` would add the argument *itself* as a single item:

```python
a = [1, 2]
a.append([3, 4])
print(a)                    # [1, 2, [3, 4]]   — a row of three items, one of which is a row

b = [1, 2]
b.extend([3, 4])
print(b)                    # [1, 2, 3, 4]     — a row of four items
```

Most beginners want `extend` here. The `append`-of-a-row form is occasionally what you want — see *rows of rows*, later in this lesson.

**`insert` — add an item at a specific position.**

```python
scores = [10, 30, 40]
scores.insert(1, 20)
print(scores)               # [10, 20, 30, 40]
```

`insert(i, x)` puts `x` at position `i`, sliding the existing items at `i` onwards one cubbyhole to the right. Picture: the worker bolts a new cubbyhole into the frame at position `1`, after sliding the existing `30` and `40` along to make room.

`insert` is less common than `append` (because adding to the end is the cheapest operation), but indispensable when order matters and the new item belongs in the middle.

**Concatenation with `+` — produces a new row.**

```python
a = [1, 2, 3]
b = [4, 5, 6]
joined = a + b
print(joined)               # [1, 2, 3, 4, 5, 6]
print(a)                    # [1, 2, 3]   — original unchanged
```

`+` works on numbered rows the way it does on scrolls: it produces a *new* row holding the items of both originals. Neither original is modified. Use `+` when you specifically want a new row; use `extend` when you want to grow an existing one in place.

---

## Removing From a Numbered Row

The mirror image of adding. Three common tools, plus the `del` keyword you already met for variables in Lesson 1.

**`remove(x)` — remove the first occurrence of a value.**

```python
scores = [10, 20, 30, 20, 40]
scores.remove(20)
print(scores)               # [10, 30, 20, 40]   — only the first 20 is gone
```

Picture: the worker walks along the frame from left to right looking for a cubbyhole holding `20`, finds the one at position `1`, removes it, and slides the remaining cubbyholes left to close the gap.

If the value is not present, `remove` triggers the alarm:

```python
scores.remove(99)            # ValueError — 99 was not in the row
```

**`pop(i)` — remove and *return* the item at position `i`.**

```python
scores = [10, 20, 30]
last = scores.pop()         # default is the last position
print(last)                 # 30
print(scores)               # [10, 20]

scores = [10, 20, 30]
first = scores.pop(0)
print(first)                # 10
print(scores)               # [20, 30]
```

`pop` is unique among row tools in that it gives you back the item it removed. This is what makes a row useful as a *stack* (always pop the last) or a *queue* (always pop the first). You will reach for `pop` whenever the item being removed is also one you want to use.

**`del` — remove by position, do not return.**

```python
scores = [10, 20, 30, 40]
del scores[1]
print(scores)               # [10, 30, 40]

del scores[1:3]
print(scores)               # [10]   — slice deletion is allowed too
```

`del` is the keyword you met in Lesson 1 for taking down a name card. On a row, it removes a cubbyhole (or a range of cubbyholes) without returning anything. Use it when you want to remove an item by position and have no need to inspect what was there.

**`clear` — empty the row in place.**

```python
scores = [10, 20, 30, 40]
scores.clear()
print(scores)               # []   — the frame is empty but still on the shelf
```

The wheeled frame stays on the `scores` shelf. The card is unchanged. The cubbyholes are simply all unbolted and removed. The result is an empty row, ready to be filled again.

---

## Searching and Ordering

Two built-in tools answer questions about the row's contents.

**`index(x)` — position of the first occurrence.**

```python
scores = [10, 20, 30, 20]
print(scores.index(20))     # 1
print(scores.index(99))     # ValueError — 99 was not in the row
```

Picture: the worker walks the frame from left to right looking for a cubbyhole holding `x` and reports the position number on its outside.

**`count(x)` — how many cubbyholes hold this value.**

```python
scores = [10, 20, 30, 20, 20]
print(scores.count(20))     # 3
print(scores.count(99))     # 0   — no alarm; absence is just zero
```

Note the asymmetry: `index` triggers an alarm when the item is missing, but `count` quietly returns `0`. This is a consistent Python convention — *find me the position* needs an answer or an alarm; *count me how many* always has a finite answer.

Two more tools handle ordering.

**`sort()` — rearrange the row in place.**

```python
scores = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3]
scores.sort()
print(scores)               # [1, 1, 2, 3, 3, 4, 5, 5, 6, 9]

scores.sort(reverse=True)
print(scores)               # [9, 6, 5, 5, 4, 3, 3, 2, 1, 1]
```

Picture: a sort worker rearranges the cubbyholes on the frame so that their contents run in order — smallest on the left, largest on the right. The frame is the same frame, on the same shelf; only the order of the cubbyholes has changed. The `reverse=True` flag flips the direction.

Sorting works on rows of stones, vials, scrolls — anything Python can compare. Mixing types in a row will usually trigger an alarm during sort (`stone < scroll` is not a meaningful question).

**`reverse()` — flip the order in place.**

```python
scores = [10, 20, 30, 40]
scores.reverse()
print(scores)               # [40, 30, 20, 10]
```

Picture: the worker slides every cubbyhole to mirror its position. Position `0` swaps with the last, `1` swaps with the second-to-last, and so on.

**`sorted()` and `reversed()` — the new-row versions.**

For each in-place tool, Python provides a *new-row* counterpart that does not modify the original:

```python
scores = [3, 1, 4, 1, 5]
new_row = sorted(scores)
print(new_row)              # [1, 1, 3, 4, 5]
print(scores)               # [3, 1, 4, 1, 5]   — original unchanged
```

`sorted(row)` produces a fresh sorted row. `reversed(row)` produces a sequence in reverse order (technically not a row, but for now the difference is invisible — pass it through `list(...)` if you want a row back).

Choose between the in-place and new-row forms based on what you want:

- **In-place** (`sort`, `reverse`) — when you no longer need the original ordering and want to save the cost of a fresh row.
- **New-row** (`sorted`, `reversed`) — when you need the original to remain in its original order, or when the row is one you should not modify (a row passed in from elsewhere, for example).

A useful note on what *not* to do: `sort()` returns `None`, not the sorted row. The line `result = scores.sort()` places `None` into `result` — almost certainly not what you wanted. Use `sorted(scores)` if you want the sorted row in a new place.

---

## The Copying Trap — One Of Python's Famous Stumbles

Now the most important warning in this lesson. Picture a row sitting on a shelf labelled `a`:

```python
a = [1, 2, 3]
```

You decide you want a copy. The natural-looking thing is:

```python
b = a
```

This is *not* a copy. What you have done is pin a second card — `b` — to the same wheeled frame.

Picture the two shelves: the shelf labelled `a` has a card, *and* the shelf labelled `b` has a card. Both cards point at the same physical frame. There is one numbered row in the warehouse, with two name cards.

Watch what happens when you "modify `a`":

```python
a = [1, 2, 3]
b = a
a.append(99)
print(a)                    # [1, 2, 3, 99]
print(b)                    # [1, 2, 3, 99]   — !
```

Both cards still point at the same row, and that row has been modified. Reading `b` gives you the same modified row. Beginners are convinced their program is haunted at this point. It is not. There was only ever one row.

This is the same property the warehouse always had — `score = age` did not link the two shelves either, because *the value being copied was a stone, and stones are immutable*. Replacing one stone with another never affects any other shelf. With numbered rows, the picture is different: the row itself can change, and any name card pointing at it sees those changes.

**To make a real copy, ask for one explicitly.**

```python
a = [1, 2, 3]
b = a.copy()
a.append(99)
print(a)                    # [1, 2, 3, 99]
print(b)                    # [1, 2, 3]   — independent
```

`copy()` builds a fresh wheeled frame and bolts on a new set of cubbyholes containing copies of the items. Two physical rows, two name cards, fully independent.

There are several other ways to ask for a copy; all of them produce the same result for a flat row:

```python
b = a.copy()
b = a[:]                    # the full slice — produces a new row
b = list(a)                 # the list mould from Lesson 5
```

Use whichever you find clearest. `a.copy()` reads most explicitly.

**One more detail: a copy is *shallow*.**

If the row contains other rows, the copy shares those inner rows with the original. This is rarely a problem for beginner code, but worth knowing about:

```python
a = [[1, 2], [3, 4]]
b = a.copy()
a[0].append(99)
print(a)                    # [[1, 2, 99], [3, 4]]
print(b)                    # [[1, 2, 99], [3, 4]]   — !
```

The outer frames are independent — appending an item to `a` itself would not show up in `b`. But the *inner* frames are shared, because the copy duplicated the cubbyhole contents (which are pointers to the inner rows), not the inner rows themselves. For deeper copies, Python's `copy` module provides `copy.deepcopy(x)` — a Lesson 20 concern. For now, recognise the situation when you see it.

---

## Rows of Rows

A numbered row can hold any kind of item, including other numbered rows. This produces a 2D structure — a grid, a table, a board.

```python
grid = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]
print(grid[0])              # [1, 2, 3]   — the first row
print(grid[0][1])           # 2           — the item at row 0, column 1
print(grid[2][2])           # 9
```

Picture: an outer wheeled frame on the warehouse shelf. Each of *its* cubbyholes holds a smaller wheeled frame, and each smaller frame has its own cubbyholes. To pick a single item, you index twice — once for the outer position, once for the inner.

Common uses for rows of rows include game boards (chess, sudoku), tables of data (rows of records), and matrices in scientific calculation. Most everyday Python uses dictionaries (Lesson 10) for tabular data, but the numbered-row-of-numbered-rows pattern is a natural starting point and remains common in some contexts.

---

## What You Now Know

You have looked closely at the numbered row. Like the scroll, it has a length, can be indexed, sliced, joined with `+`, repeated with `*`, and asked the `in` question. Unlike the scroll, it can be edited: items can be replaced, added with `append` / `extend` / `insert`, or removed with `remove` / `pop` / `del` / `clear`. Order can be changed with `sort` and `reverse` (in place) or `sorted` and `reversed` (new-row).

You have also seen the deepest beginner trap in Python — that `b = a` for a numbered row pins a second card to the same frame, not a copy. To get a real copy you must say so: `a.copy()`, `a[:]`, or `list(a)`.

The contrast between the scroll and the numbered row is the contrast between **immutable** and **mutable** items. The deeper consequences of this distinction will come up in every later lesson — when you pass a row to a workstation (Lesson 8), what happens depends on it. For now, the visual is enough: a scroll is sealed at writing and any change produces a new scroll; a row's wheeled frame is the same frame from creation to destruction, while its cubbyholes come and go.

---

## Quick Reference

| Python | Numbered row image |
|---|---|
| `[1, 2, 3]`, `[]` | A wheeled frame with cubbyholes holding the items, numbered from zero. Square brackets define one. |
| `len(row)` | Number of cubbyholes on the frame. |
| `row[i]`, `row[-1]` | Pick the item at position `i`. Negatives count from the end. |
| `row[a:b]`, `row[::-1]` | A new shorter row sliced from the original; `[::-1]` is reversed. |
| `x in row` | Switch — does the row contain `x`? |
| `row[i] = new` | Replace the item at position `i` in place. |
| `row[a:b] = new_row` | Replace a slice; can change the row's length. |
| `row.append(x)` | Bolt a cubbyhole on the right end holding `x`. |
| `row.extend(other)` | Bolt cubbyholes on the right end holding each item of `other`. |
| `row.insert(i, x)` | Bolt a new cubbyhole in at position `i`; existing items slide right. |
| `a + b` | A new row with the items of both. Originals unchanged. |
| `row * n` | A new row with `n` copies of the items. |
| `row.remove(x)` | Remove the first cubbyhole holding `x`. ValueError if missing. |
| `row.pop(i)` | Remove and return the item at position `i` (default last). |
| `del row[i]` | Remove the item at position `i`. No return. |
| `row.clear()` | Empty the row in place; the frame remains. |
| `row.index(x)` | Position of first occurrence of `x`. ValueError if missing. |
| `row.count(x)` | How many cubbyholes hold `x`. |
| `row.sort()`, `row.sort(reverse=True)` | Rearrange in place into order. Returns `None`. |
| `row.reverse()` | Flip in place. Returns `None`. |
| `sorted(row)`, `reversed(row)` | New-row counterparts; original unchanged. |
| `b = a` | **Not a copy.** Two name cards on the same frame. |
| `b = a.copy()`, `b = a[:]`, `b = list(a)` | Real copy. Two frames, two cards. |
| `grid[i][j]` | Index into a row of rows: outer first, then inner. |

---

## Try It

Open a Python prompt or any Python editor.

**Build a row, measure it, slice it:**

```python
nums = [10, 20, 30, 40, 50]
print(len(nums))
print(nums[0], nums[-1])
print(nums[1:4])
print(nums[::-1])
print(30 in nums)
```

Same operations you ran on the scroll yesterday. The mental model — positions numbered from zero, slicing exclusive at the end — transfers directly.

**Mutate the row:**

```python
nums = [10, 20, 30, 40]
nums[1] = 99
print(nums)
nums.append(100)
print(nums)
nums.insert(0, 0)
print(nums)
nums.remove(30)
print(nums)
last = nums.pop()
print(last, nums)
```

Picture the frame staying on the same shelf the whole time, with cubbyholes being swapped, added, and removed.

**Sort and reverse, in place vs new-row:**

```python
nums = [3, 1, 4, 1, 5, 9, 2, 6, 5]
new = sorted(nums)
print(nums)         # original — unchanged
print(new)          # sorted copy

nums.sort()
print(nums)         # original — now sorted in place

nums.sort(reverse=True)
print(nums)         # descending
```

**The copy trap — see it once and never forget:**

```python
a = [1, 2, 3]
b = a
a.append(99)
print("a:", a)
print("b:", b)
```

Two cards, one row.

```python
a = [1, 2, 3]
b = a.copy()
a.append(99)
print("a:", a)
print("b:", b)
```

Two cards, two rows.

**Rows of rows:**

```python
grid = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]
print(grid[1])
print(grid[1][2])
grid[2][2] = 99
print(grid)
```

Picture the outer frame holding three smaller frames, each of which has its own cubbyholes.

---

## Where Next?

You are still in the Warehouse, and now know two of its items in depth — the scroll and the numbered row. Before going on to the other collections, the series takes a brief detour to the Factory Floor proper to introduce the **workstation**: Python's word for *function*. Workstations live on the Floor, but the lesson on **scope** that follows depends entirely on understanding what a workstation is — so the detour is unavoidable here.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 8 | Z5 — Factory Floor | Functions — the first workstation |
| Lesson 9 | Z3 — Warehouse | Scope — the locked rooms in full |
| Lesson 10 | Z3 — Warehouse | Dictionaries — filing cabinets in depth |
| Lesson 11 | Z3 — Warehouse | Tuples and Sets — sealed crates and unsorted bins |

*See the full lesson map in **The Factory — A Complete Map**.*
