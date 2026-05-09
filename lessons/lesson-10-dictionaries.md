# Python for Hyperphantasic Minds
## Lesson 10 — Dictionaries

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 10 of 25  
> **Topic**: Dictionaries — filing cabinets in depth  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still in the Warehouse. The scroll and the numbered row are behind you. Today is the **filing cabinet** — the third heavily-used item, and the one whose physical character is most clearly different from the others. Where the scroll and the row arrange their contents in a sequence, the filing cabinet arranges its contents *by label*.

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

You first met the filing cabinet in Lesson 3. The visual was: *a steel cabinet with labelled drawers. Each drawer has a key on the front (the dictionary key) and holds one item inside (the value). Retrieved by key, not by position.* In Lesson 5 you saw the `dict()` mould briefly. Today you take it apart properly.

---

## The Filing Cabinet, Looked At Closely

Picture the cabinet on a warehouse shelf. It is a single tall steel cabinet — one *item* the warehouse is keeping track of, the same way a numbered row is one item. The cabinet's name card might read `prices` or `player` or `config`.

The cabinet's surface is mostly drawers. Each drawer has a label on the front — a **key** — and inside the drawer is a single item — the **value**. To retrieve a value, you do not count drawers; you read the labels and pull open the one you want.

```python
prices = {
    "apple": 0.50,
    "bread": 2.40,
    "milk": 1.20,
}
```

That single literal places a filing cabinet on the `prices` shelf. Three drawers are bolted to the cabinet, each with its label and its contents:

```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ "apple"      │  │ "bread"      │  │ "milk"       │
│              │  │              │  │              │
│   0.50       │  │   2.40       │  │   1.20       │
└──────────────┘  └──────────────┘  └──────────────┘
```

Curly brackets define a filing cabinet. Each entry is a `key: value` pair, separated by commas. Empty cabinet: `{}`.

Two strict rules govern a cabinet:

- **Each label is unique.** Two drawers cannot share the same label. If you write the same key twice in a literal, the second wins; the first is silently overwritten.
- **Drawers can be added or removed freely.** Like the numbered row's wheeled frame, the cabinet itself stays put; the drawers come and go.

---

## Reading From a Drawer

To open a drawer and read what is inside, write the cabinet's name and the key in square brackets:

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}

print(prices["apple"])           # 0.5
print(prices["milk"])             # 1.2
```

Picture the worker walking up to the cabinet, finding the drawer labelled `"apple"`, sliding it open, and lifting out the value — the vial `0.50`. The cabinet is unchanged.

This is exactly the canonical action **open the labelled drawer**, which you saw glimpsed in Lesson 3.

**If the label is not found, an alarm.**

```python
print(prices["banana"])           # KeyError: 'banana'
```

There is no drawer with that label. Python triggers `KeyError` rather than guess.

**`.get()` — read with a fallback.**

If you would rather receive a default than an alarm when the key is missing, the cabinet's built-in tool `get` is the answer:

```python
print(prices.get("apple"))         # 0.5
print(prices.get("banana"))        # None — safe default
print(prices.get("banana", 0.0))   # 0.0  — your own default
```

`.get(key)` returns `None` if the key is missing. `.get(key, default)` returns `default` instead. This is the everyday tool for "read this, but don't panic if it isn't there".

---

## Adding and Updating Drawers

The cabinet is mutable, like the numbered row. To add a new drawer, write to a key that does not yet exist:

```python
prices = {"apple": 0.50}
prices["bread"] = 2.40
prices["milk"] = 1.20
print(prices)                     # {'apple': 0.5, 'bread': 2.4, 'milk': 1.2}
```

Picture: a worker bolts a new drawer onto the cabinet, pins the label `"bread"` on the front, places a vial `2.40` inside, and slides it shut.

To **update** an existing drawer, write to a key that already exists:

```python
prices["apple"] = 0.60            # the value behind 'apple' is replaced
print(prices["apple"])            # 0.6
```

Picture: the worker opens the `"apple"` drawer, removes the old vial, places the new one inside, and slides it shut. The drawer and label are unchanged.

Notice that *the same syntax* — `prices[key] = value` — handles both adding and updating. Python checks whether a drawer with that label already exists and either creates or updates accordingly. This is one of the cabinet's nicest properties: you do not have to know in advance whether the drawer is there.

---

## Removing Drawers

Three tools handle removal, mirroring the numbered row's repertoire.

**`del` — remove by key, no return.**

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}
del prices["bread"]
print(prices)                     # {'apple': 0.5, 'milk': 1.2}
```

Picture: the drawer is unbolted from the cabinet and removed. If the key does not exist, Python triggers `KeyError`.

**`pop(key)` — remove and return the value.**

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}
removed = prices.pop("bread")
print(removed)                    # 2.4
print(prices)                     # {'apple': 0.5, 'milk': 1.2}
```

`pop` is to the cabinet what it was to the row: it gives you back the item it took out. Useful when the value being removed is also one you want to use. A second argument provides a default to return when the key is absent (no alarm):

```python
prices.pop("banana", 0.0)         # 0.0 — safe pop
```

**`clear()` — empty the whole cabinet in place.**

```python
prices.clear()
print(prices)                     # {}   — cabinet is empty, still on the shelf
```

The cabinet itself stays on the `prices` shelf with all its drawers removed. The card is unchanged.

---

## The `in` Operator — Keys, Not Values

A subtle but very important point. The `in` operator on a cabinet asks *"is this a key?"* — *not* *"is this a value?"*

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}

print("apple" in prices)          # True   — yes, there is a drawer labelled 'apple'
print("banana" in prices)         # False
print(0.50 in prices)             # False  — !  0.50 is a value, not a key
```

That last line catches every Python beginner at least once. The cabinet is searched *by label*, not by content. To check whether a value appears, you have to ask explicitly:

```python
print(0.50 in prices.values())    # True
```

Picture: `prices.values()` is a worker walking past the cabinet, opening every drawer in turn and gathering the contents into a temporary numbered row. The `in` operator then asks the row whether `0.50` is anywhere in it.

The pattern *"is there any drawer holding this value?"* is much rarer than *"is there a drawer with this label?"*, which is why the cabinet's natural `in` answers the latter. When you really do need the value question, `.values()` is the bridge.

---

## The Three Inspections — `.keys()`, `.values()`, `.items()`

Three built-in tools let you survey the whole cabinet:

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}

print(list(prices.keys()))        # ['apple', 'bread', 'milk']
print(list(prices.values()))      # [0.5, 2.4, 1.2]
print(list(prices.items()))       # [('apple', 0.5), ('bread', 2.4), ('milk', 1.2)]
```

Picture each as a different walk past the cabinet:

- **`keys()`** — read every label aloud, in order.
- **`values()`** — open every drawer in turn, take out the contents, hand them over.
- **`items()`** — open every drawer, hand back a sealed crate `(label, contents)` for each one.

The `list(...)` mould you saw in Lesson 5 is wrapped around each call here so the result prints clearly. Without it, you would see something like `dict_keys([...])` — Python's specialised view-of-a-cabinet item, which behaves almost exactly like a numbered row for most beginner purposes.

These three are the foundation of every loop over a cabinet. You will use them constantly from Lesson 14 onwards (`for key in cabinet.keys():`, `for key, value in cabinet.items():` are the standard forms). For now, *recognise* the trio and what each one returns.

---

## Length and the Empty Cabinet

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}
print(len(prices))                # 3 — number of drawers

empty = {}
print(len(empty))                 # 0
```

`len` works on every collection item the same way: it counts contents. For the cabinet, *contents* means *drawers*, not the items inside the drawers.

The empty cabinet is an everyday creation pattern: build an empty cabinet, then fill it with `cabinet[key] = value` as the program runs.

```python
counts = {}
counts["apple"] = 5
counts["bread"] = 2
counts["milk"] = 8
print(counts)
```

This is one of the most common patterns in beginner Python: a cabinet that grows during the run, accumulating data.

---

## Merging Two Cabinets

When you have two cabinets and want their drawers combined, Python provides two natural ways.

**`update()` — modify in place.**

```python
defaults = {"colour": "blue", "size": "medium"}
overrides = {"size": "large", "weight": 2}

defaults.update(overrides)
print(defaults)                   # {'colour': 'blue', 'size': 'large', 'weight': 2}
```

Picture: the worker carries the second cabinet's drawers across and bolts each one onto the first cabinet — except where a label already exists, in which case the existing drawer's contents are replaced. The first cabinet is modified; the second is unchanged.

**`|` — produce a new cabinet.**

```python
defaults = {"colour": "blue", "size": "medium"}
overrides = {"size": "large", "weight": 2}

merged = defaults | overrides
print(merged)                     # {'colour': 'blue', 'size': 'large', 'weight': 2}
print(defaults)                   # {'colour': 'blue', 'size': 'medium'}   — unchanged
```

The `|` operator (Python 3.9+) builds a fresh cabinet from both, with the right-hand side winning on label collisions. Neither original is modified. This is the cabinet equivalent of `+` on numbered rows.

---

## What Can Be a Drawer Label

A drawer label has to be permanent. Once written and pinned to a drawer, the label cannot change for as long as the drawer is on the cabinet. So labels can only be made of items that themselves are unchangeable.

**Allowed as labels:**

- Scrolls (`"apple"`)
- Stones (`42`)
- Vials (`3.14` — though rarely useful, due to the precision trap of Lesson 4)
- Switches (`True`, `False`)
- Sealed crates (`(3, 4)`)
- Vacant cubbyholes (`None`)

**Not allowed as labels:**

- Numbered rows (`[1, 2]`)
- Filing cabinets
- Unsorted bins

```python
ok = {(1, 2): "point", "name": "Shane", 42: "answer"}     # all fine

bad = {[1, 2]: "value"}                                    # TypeError
```

The reason the row, cabinet, and bin are forbidden: each of them can be modified after creation, which would mean the drawer's label could quietly change while it is sitting on the cabinet. That breaks every assumption Python makes about labels staying findable, so Python refuses the construction at the point of attempt.

In practice, the overwhelming majority of cabinet labels are scrolls. Stones come up occasionally (a cabinet keyed by user ID); sealed crates rarely (a cabinet keyed by `(x, y)` coordinates). The other forms exist in case you need them.

---

## Cabinets Inside Cabinets — Records

A drawer's contents can be any item, including another filing cabinet. This is how Python represents *records* — structured data with named fields.

```python
players = {
    "alice": {"score": 100, "level": 5, "active": True},
    "bob":   {"score": 75,  "level": 3, "active": True},
    "cara":  {"score": 200, "level": 8, "active": False},
}

print(players["alice"]["score"])      # 100
print(players["bob"]["level"])        # 3

players["alice"]["score"] = 150        # update a nested field
players["dave"] = {"score": 0, "level": 1, "active": True}    # add a whole record
```

Picture: a large cabinet on a single shelf. Each of its drawers contains a smaller cabinet with its own labelled drawers — `score`, `level`, `active`. To reach a single field, open two drawers in sequence: outer first, then inner.

This pattern is the bread-and-butter of working with structured data — JSON from a web API, configuration files, game state, user records. You will see deeply nested cabinets of cabinets and rows of cabinets in real Python code constantly. The reading rule is always the same: each pair of brackets opens one more drawer.

---

## What You Now Know

You have looked at the filing cabinet in detail. You can create one, read from it (`cabinet[key]` for the strict version, `.get(key, default)` for the safe one), add and update drawers (the same syntax handles both), remove drawers (`del`, `.pop`, `.clear`), and survey it (`.keys`, `.values`, `.items`). You know the cabinet's `in` operator checks labels, not values, and that `.values()` is the bridge for the rarer value-membership question. You know how to merge two cabinets (`update` in place, `|` for a new one), what kinds of items are allowed as labels (the immutable ones), and how to nest cabinets to build records.

Cabinets are the most flexible of the four collections. They scale from two drawers to millions; they accept any value; they are the natural shape for almost every "labelled data" problem you will encounter. Combined with the numbered row, they cover the great majority of data-shaping work in real Python programs.

The next lesson finishes the warehouse tour by looking at the two smaller collections — the **sealed crate** and the **unsorted bin**.

---

## Quick Reference

| Python | Filing cabinet image |
|---|---|
| `{"a": 1, "b": 2}`, `{}` | A cabinet with labelled drawers; curly brackets define one. |
| `dict[key]` | Open the drawer labelled `key`. KeyError if missing. |
| `dict.get(key)` | Read the drawer; return `None` if missing. |
| `dict.get(key, default)` | Read the drawer; return `default` if missing. |
| `dict[key] = value` | Add a new drawer, or update an existing one. Same syntax for both. |
| `del dict[key]` | Unbolt the drawer. KeyError if missing. |
| `dict.pop(key)` | Unbolt and return the value. |
| `dict.pop(key, default)` | Unbolt and return; default if missing (no alarm). |
| `dict.clear()` | Remove every drawer; cabinet stays on the shelf. |
| `key in dict` | Switch — does a drawer with this label exist? |
| `value in dict.values()` | Switch — does any drawer contain this value? |
| `dict.keys()` | A walk past every label. |
| `dict.values()` | A walk past every drawer's contents. |
| `dict.items()` | A walk past every (label, contents) pair, as sealed crates. |
| `len(dict)` | Number of drawers. |
| `dict1.update(dict2)` | Merge `dict2` into `dict1` in place; right-hand side wins on collision. |
| `dict1 \| dict2` | New cabinet merged from both. Neither original is modified. |
| Drawer labels | Must be immutable: scrolls, stones, vials, switches, sealed crates, None. |
| Nested cabinets | `cabinet[key1][key2]` opens two drawers in sequence. |

---

## Try It

Open a Python prompt or any Python editor.

**Build a cabinet, read from it, update it:**

```python
prices = {"apple": 0.50, "bread": 2.40}
print(prices["apple"])
prices["apple"] = 0.60
prices["milk"] = 1.20
print(prices)
print(len(prices))
```

Picture each line as a physical action on the cabinet.

**The KeyError vs `.get()`:**

```python
prices = {"apple": 0.50}
# print(prices["banana"])              # KeyError — try it once
print(prices.get("banana"))            # None
print(prices.get("banana", 0.0))       # 0.0
```

**The `in` gotcha:**

```python
prices = {"apple": 0.50, "bread": 2.40}
print("apple" in prices)               # True
print(0.50 in prices)                  # False — !
print(0.50 in prices.values())         # True
```

**The three inspections:**

```python
prices = {"apple": 0.50, "bread": 2.40, "milk": 1.20}
print(list(prices.keys()))
print(list(prices.values()))
print(list(prices.items()))
```

**Merging two cabinets:**

```python
defaults = {"colour": "blue", "size": "medium"}
overrides = {"size": "large", "weight": 2}

merged = defaults | overrides
print(merged)
print(defaults)        # unchanged

defaults.update(overrides)
print(defaults)        # now modified
```

**Records — nested cabinets:**

```python
players = {
    "alice": {"score": 100, "level": 5},
    "bob":   {"score": 75,  "level": 3},
}
print(players["alice"]["score"])
players["alice"]["score"] = 150
players["cara"] = {"score": 200, "level": 8}
print(players)
```

**The mutable-key alarm — see it once:**

```python
bad = {[1, 2]: "value"}                # TypeError
```

The error message is `unhashable type: 'list'`. *Hashable* is the formal Python word for "can be a label" — but the visual you should keep is the simpler one: labels have to stay put, and a row that can change does not stay put.

**Build a cabinet that grows:**

```python
counts = {}
counts["apple"] = 5
counts["bread"] = 2
counts["milk"] = 8
print(counts)
print(len(counts))
```

The pattern of starting empty and accumulating is one you will reach for constantly.

---

## Where Next?

The warehouse tour is almost complete. One lesson remains for the two smaller collections — the sealed crate and the unsorted bin — both of which you have already glimpsed. After that, the series moves out onto the Factory Floor for the operators that turn the workstations into a real production line.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 11 | Z3 — Warehouse | Tuples and Sets — sealed crates and unsorted bins |
| Lesson 12 | Z5 — Factory Floor | Operators — the tools on the floor |
| Lesson 13 | Z5 — Factory Floor | If / Else — junctions and inspection gates |
| Lesson 14 | Z5 — Factory Floor | Loops — conveyor belts, emergency stops, skip gates |

*See the full lesson map in **The Factory — A Complete Map**.*
