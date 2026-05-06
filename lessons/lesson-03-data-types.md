# Python for Hyperphantasic Minds
## Lesson 3 — Data Types

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 3 of 25  
> **Topic**: Data Types — the full range of items on the shelves  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You have walked back from the dispatch bay. The Warehouse is familiar now — rows of cubbyholes, name cards, the quiet hum of a building doing its job. Today you are taking a guided tour. The warehouse foreman is going to open cubbyhole after cubbyhole and show you, for the first time, the full range of items the factory uses.

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

You already know two of the items by name — stones and scrolls, met in Lesson 1. Today you meet the rest. Nine in total. Each one has a distinct physical character. Each one is for a different kind of work. The whole factory's vocabulary of *things-on-shelves* fits on a single tour.

---

## The Walk Through the Warehouse

The foreman meets you at the entrance with a clipboard. They will show you each kind of item briefly — enough that you recognise it on sight and know roughly what it is for. Deeper details — what you can do with each item, the operations and methods that act on it — are saved for later lessons. This is the orientation tour.

Two ground rules before you start:

1. **Every Python value is one of these nine items.** When the factory hands you something, it is a stone, a scroll, a switch — one of the nine. There is no tenth.
2. **The cubbyhole still does not care.** From Lesson 1: any cubbyhole accepts any item. The shelf is universal. The character — the *type* — belongs to the item, not the shelf.

Let's go.

---

## 1. The Stone — `int`

The stone is small, smooth, and dense. A whole number is painted on its face in white. No fractions, no decimals — just the integer itself. Stones come in any size: a stone marked `7`, a stone marked `-1000000`, a stone marked `0`. The character on the stone is fixed; you cannot change `7` into `8` by polishing it. To get an `8` you fetch a different stone.

```python
age = 27
year = 2026
temperature_change = -5
zero = 0
```

You met stones in Lesson 1. The foreman points at one and moves on. Stones are the bread and butter of any factory that counts things.

*Deep dive: Lesson 4 (Numbers).*

---

## 2. The Vial — `float`

A small glass vial with a stopper. The liquid inside shifts slightly when the vial is set down. A number is etched onto the glass — but unlike the stone, this number can carry a fractional part. `3.14`. `9.81`. `0.5`. `-273.15`. The vial is precise and slightly fragile-looking, where the stone is hard-edged and absolute.

```python
pi = 3.14159
price = 9.99
gravity = 9.81
absolute_zero = -273.15
```

Whenever a value has a decimal point, Python builds you a vial, not a stone. Even a whole-number-looking `3.0` is a vial — the decimal point is the giveaway. Stones and vials are both numeric, but they are not the same item, and Python keeps them visibly distinct on the shelves.

*Deep dive: Lesson 4 (Numbers).*

---

## 3. The Scroll — `str`

A roll of parchment. A short scroll fits in the palm and is tied with a ribbon; a long one is a heavy roll. The characters of the text are visible on the surface if you look closely. A scroll might hold a single letter, a word, a paragraph, or an entire chapter — Python does not mind.

```python
name = "Shane"
greeting = 'Hello, world!'
empty = ""
long_text = "the quick brown fox jumps over the lazy dog"
```

Notice that Python accepts both single quotes (`'...'`) and double quotes (`"..."`) — pick one and stick to it. The empty scroll (`""`) is still a scroll. It is just a roll of parchment with nothing written on it.

You met scrolls in Lesson 1. The foreman holds one up briefly, then moves on.

*Deep dive: Lesson 6 (Strings, including f-strings).*

---

## 4. The Switch — `bool`

A small physical toggle. Two positions only — up or down. There is no halfway, no "maybe", no "almost". The switch is either up (`True`) or down (`False`), and the click between them is definitive.

```python
is_ready = True
game_over = False
has_unsaved_changes = True
```

The switch comes from a small family of just two values: `True` and `False`. Both must be capitalised. Lower-case `true` is not a switch — Python will not recognise it.

Switches are how the factory keeps track of yes/no questions. Was the form submitted? Has the user logged in? Is the game over? Each is a switch, in one of two positions.

A subtle but important detail: comparisons in Python produce switches. When you ask a question like `age > 18`, what comes back is a switch — `True` if it is, `False` if it isn't. You will see far more of this from Lesson 12 onwards.

*Deep dive: Lesson 12 (Operators) and Lesson 13 (Junctions and switches in use).*

---

## 5. The Vacant Cubbyhole — `None`

The strangest item on the tour, because it is not really an item at all.

A vacant cubbyhole is a shelf with a name card pinned to the front and *nothing inside*. Not a stone marked `0`. Not an empty scroll. Genuinely nothing. The card is there. The space is there. The space is empty on purpose.

```python
result = None
default_user = None
not_found = None
```

Python's word for this deliberate emptiness is `None` (capital N). It signals: *there is no value here, and that is intentional*. It is different from "I haven't created the shelf yet" — the shelf exists, it has a card, and the card is `result`. The shelf simply has no item on it right now.

You will see `None` used in three common ways:
- A placeholder until a real value arrives ("not yet")
- A signal that an operation found nothing ("no result")
- The natural answer when a workstation finishes without producing anything (Lesson 8)

That is the full inventory of single-value items: stones, vials, scrolls, switches, and vacant cubbyholes. From here, the foreman walks you to the back of the warehouse, where the *collections* live.

---

## 6. The Numbered Row — `list`

A row of small cubbyholes bolted together on a wheeled frame. The whole row sits on a single shelf. Each cubbyhole within the row is numbered — starting from **zero** — and holds one item. The row can be extended (more cubbyholes welded on at the end), shortened, rearranged.

```python
scores = [10, 20, 30]
names = ["Alice", "Bob", "Charlie"]
mixed = [1, "two", 3.0, True]
empty_row = []
```

Square brackets are the rule: `[ ... ]` for a numbered row. Items inside are separated by commas.

A few things to picture from the start:

- **The numbered row is itself a single item.** It sits in one cubbyhole; its name card might say `scores`. Inside the row are smaller cubbyholes holding stones, but the row as a whole is one thing the warehouse is keeping track of.
- **The contents can be different kinds.** The third example holds a stone, a scroll, a vial, and a switch all in the same row. Python is comfortable with this; in practice, most rows hold items of one kind.
- **Numbering starts at zero.** The first cubbyhole inside the row is `0`, not `1`. This is a Python rule worth getting used to early — it will save you from a class of off-by-one mistakes that tortures beginners in many other languages.

You will use numbered rows constantly. Almost every program in the rest of this series will involve a row at some point.

*Deep dive: Lesson 7 (Lists).*

---

## 7. The Sealed Crate — `tuple`

A wooden crate that has been nailed shut. The contents are visible through gaps in the slats — you can read what is inside — but the crate cannot be opened, repacked, or rearranged. The weight and contents are decided at the moment of packing, and that is final.

```python
point = (3, 4)
colours = ("red", "green", "blue")
person = ("Shane", 27, True)
empty_crate = ()
```

Round brackets are the rule: `( ... )`. Like the numbered row, items are separated by commas.

Sealed crates are for groups of items that *belong together and should not change*. A point on a graph: `(3, 4)` — the x and y are a single fact, not two facts; you would not reach into the crate to swap the x. The colour `(255, 128, 0)` — three numbers, one colour. The first row of a database record. The return values of a workstation that needs to send back two things at once (Lesson 8).

A sealed crate looks almost like a numbered row to the casual observer — both are sequences of items in order. The visible difference is the bracket type. The deeper difference is the seal: nothing inside a crate ever changes once it is packed.

*Deep dive: Lesson 11 (Tuples and Sets).*

---

## 8. The Unsorted Bin — `set`

A large open bin. Items are thrown in, with no order and no positions. Two strict rules govern the bin:

- **No duplicates.** Throw the same stone in twice and the second one simply fails to land. The bin already has it.
- **No order.** Items inside the bin have no first, no second, no last. You cannot retrieve the third stone from the unsorted bin, because there is no "third". You can only ask, "is this stone in the bin?"

```python
unique_scores = {10, 20, 30}
tags = {"new", "urgent", "frontend"}
seen_today = {1, 5, 9, 5, 1, 9}   # the bin will only hold {1, 5, 9}
empty_bin = set()
```

Curly brackets are the rule: `{ ... }`. (One important quirk: `{}` on its own creates a *filing cabinet* — see the next item — not an empty bin. To make an empty bin, you must say `set()`.)

Unsorted bins are for asking *membership* questions: have we seen this email address before? Is this user in the admin group? Which words appear in this document? The duplicate rule does the work for you automatically — there is no risk of accidentally counting the same email twice.

*Deep dive: Lesson 11 (Tuples and Sets).*

---

## 9. The Filing Cabinet — `dict`

A steel cabinet with labelled drawers. Each drawer has a key written on the front — the **dictionary key** — and inside the drawer is one item — the **value**. To find something, you do not count drawers; you read the labels and pull open the one you want.

```python
player = {"name": "Shane", "score": 27, "ready": True}
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}
empty_cabinet = {}
```

Curly brackets again — but with `key: value` pairs separated by commas. Each key is unique within the cabinet (you cannot have two drawers with the same label).

Reading a value out is a matter of opening the right drawer:

```python
player["name"]    # opens the drawer labelled "name" — finds the scroll "Shane"
player["score"]   # opens the drawer labelled "score" — finds the stone 27
```

This is what the canonical verb **open the labelled drawer** describes — pulling a value out of the cabinet by its key.

Filing cabinets are how the factory keeps track of *records* — a player's profile, a price list, a configuration, a translation table from one language to another. Anywhere you would naturally pair a label with a value, you reach for a filing cabinet.

*Deep dive: Lesson 10 (Dictionaries).*

---

## The Warehouse Inspector — `type()`

When you are unsure what kind of item is on a shelf, the warehouse has an inspector. The inspector picks up the item, examines it carefully, and reads out what it is.

```python
type(27)             # <class 'int'>
type(3.14)           # <class 'float'>
type("hello")        # <class 'str'>
type(True)           # <class 'bool'>
type([1, 2, 3])      # <class 'list'>
type((3, 4))         # <class 'tuple'>
type({1, 2, 3})      # <class 'set'>
type({"a": 1})       # <class 'dict'>
type(None)           # <class 'NoneType'>
```

`type()` is a built-in member of the factory's standard kit (Lesson 9 explains what that means properly). Hand it any item and it tells you which of the nine kinds you are looking at.

This is enormously useful when something is going wrong and you are not sure why. Print the type of the value alongside it:

```python
score = 27
print(score, type(score))
# 27 <class 'int'>
```

In a single line, you have both the value and its identity. When Python complains that "stone + scroll cannot be added together" (Lesson 1), `type()` is how you find out which is which.

---

## The Cubbyhole Doesn't Care

You saw this in Lesson 1, but it bears repeating now that you have seen the full inventory:

The cubbyhole has no fixed shape. It does not say "stones only" or "filing cabinets only". You may place any of the nine kinds of items inside any cubbyhole — and you may *replace* one kind with another in the same shelf:

```python
value = 27              # a stone is on the shelf
value = 3.14            # now a vial
value = "hello"         # now a scroll
value = [1, 2, 3]       # now a numbered row
value = None            # now a vacant cubbyhole
```

Each line replaces what was on the shelf. The card still reads `value`. Python adapts to whatever is currently in there. This is **dynamic typing**, met briefly in Lesson 1 and now extending across all nine items.

The flexibility is convenient — you do not have to declare the type of every shelf in advance — but it places a real responsibility on you: the *meaning* of a shelf depends on what is on it right now. A shelf called `score` should hold a stone. If you accidentally place a scroll on it, Python will not stop you, but the next workstation that tries to do arithmetic with it certainly will. The warehouse trusts you to use it well.

---

## What You Now Know

You have seen the full set of items the factory uses. Five single-value items — stone, vial, scroll, switch, vacant cubbyhole — and four collections — numbered row, sealed crate, unsorted bin, filing cabinet. Nine in total. Every Python value is one of these nine items.

You also met the warehouse inspector, `type()`, which can identify any item on sight.

You do not need to remember every detail of every item right now. The depth comes lesson by lesson — numbers in Lesson 4, casting between types in Lesson 5, scrolls in Lesson 6, numbered rows in Lesson 7, filing cabinets in Lesson 10, sealed crates and unsorted bins in Lesson 11. What matters today is **recognition**: when you see `[1, 2, 3]` in a piece of code, you should picture a numbered row, not a stack of brackets. When you see `True`, picture a switch in the up position. When you see `None`, picture a name card on an empty shelf.

The factory's whole vocabulary of *things* fits on a single page, and it is now in your head.

---

## Quick Reference

The cornerstone table of the warehouse — keep this somewhere you can return to.

| Python type | Canonical item | Literal syntax | Character | Deep dive |
|---|---|---|---|---|
| `int` | **Stone** | `27`, `-5`, `0` | Whole number, hard-edged. | Lesson 4 |
| `float` | **Vial** | `3.14`, `9.81`, `-0.5` | Number with a decimal point. | Lesson 4 |
| `str` | **Scroll** | `"hello"`, `'Shane'`, `""` | Text of any length. | Lesson 6 |
| `bool` | **Switch** | `True`, `False` | Two positions only. | Lesson 12 |
| `list` | **Numbered row** | `[1, 2, 3]`, `["a", "b"]`, `[]` | Ordered, changeable, items numbered from zero. | Lesson 7 |
| `tuple` | **Sealed crate** | `(3, 4)`, `("r","g","b")`, `()` | Ordered, sealed at packing — cannot change. | Lesson 11 |
| `set` | **Unsorted bin** | `{1, 2, 3}`, `set()` | Unordered, no duplicates. `{}` alone makes a `dict`, not a `set`. | Lesson 11 |
| `dict` | **Filing cabinet** | `{"name": "Shane", "score": 27}`, `{}` | Labelled drawers — accessed by key. | Lesson 10 |
| `None` | **Vacant cubbyhole** | `None` | The deliberate absence of any value. | (used throughout) |
| `type(value)` | **The warehouse inspector** | `type(27)` → `<class 'int'>` | Identifies the item on any shelf. | (used throughout) |

---

## Try It

Open a Python prompt or any Python editor.

**Place one of each item on a shelf and inspect it:**

```python
a_stone = 27
a_vial = 3.14
a_scroll = "hello"
a_switch = True
nothing = None
a_row = [1, 2, 3]
a_crate = (3, 4)
a_bin = {1, 2, 3}
a_cabinet = {"name": "Shane", "score": 27}

print(a_stone, type(a_stone))
print(a_vial, type(a_vial))
print(a_scroll, type(a_scroll))
print(a_switch, type(a_switch))
print(nothing, type(nothing))
print(a_row, type(a_row))
print(a_crate, type(a_crate))
print(a_bin, type(a_bin))
print(a_cabinet, type(a_cabinet))
```

Read each line of output as the inspector announcing what was on the shelf.

**Watch the unsorted bin reject duplicates:**

```python
print({1, 2, 3, 2, 1, 1, 1, 3})
```

Eight items thrown in, three remain. Picture the duplicates failing to land.

**Open a labelled drawer in a filing cabinet:**

```python
player = {"name": "Shane", "score": 27, "ready": True}
print(player["name"])
print(player["score"])
```

Picture pulling open the labelled drawer and reading what is inside.

**Confirm the cubbyhole's flexibility:**

```python
shelf = 27
print(shelf, type(shelf))

shelf = "twenty-seven"
print(shelf, type(shelf))

shelf = [27]
print(shelf, type(shelf))

shelf = None
print(shelf, type(shelf))
```

The card on the cubbyhole still reads `shelf`. Only the item changes.

**The mistake everyone makes:**

```python
not_a_set = {}
print(type(not_a_set))     # what does the inspector say?
empty_set = set()
print(type(empty_set))     # and now?
```

Curly brackets alone produce a filing cabinet, not an unsorted bin. The empty bin requires the spelling-out `set()`. This is a small Python quirk worth meeting once.

---

## Where Next?

You are still in the Warehouse, and now know what every kind of item looks like. The next lessons begin to deepen each one — starting with the two flavours of number.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 4 | Z3 — Warehouse | Numbers — stones and vials in depth |
| Lesson 5 | Z3 → Z5 | Casting — pressing objects into moulds |
| Lesson 6 | Z3 — Warehouse | Strings — scrolls in depth (including f-strings) |
| Lesson 7 | Z3 — Warehouse | Lists — numbered rows in depth |

*See the full lesson map in **The Factory — A Complete Map**.*
