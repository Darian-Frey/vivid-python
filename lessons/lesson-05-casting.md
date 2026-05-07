# Python for Hyperphantasic Minds
## Lesson 5 — Casting

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 5 of 25  
> **Topic**: Casting — pressing items into moulds  
> **Factory zone**: Z3 → Z5 — Warehouse to Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Today is your first time stepping out of the Warehouse. Not far — the door at the back of the Warehouse opens directly onto the **Factory Floor**, the second of the major zones. The Floor is where almost every transformation in your programs will eventually happen, but most of it is for later lessons. Today you are visiting only the very first workshop you see when you cross the threshold: the **Mould Workshop**, where one kind of item is reshaped into another.

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

A note on the route: the lesson is a brief excursion. After this, you turn around and come back into the Warehouse for the next several lessons — there is much more to learn about scrolls, numbered rows, filing cabinets and the rest. The Factory Floor itself, with all its workshops, opens up properly from Lesson 8 onwards.

---

## The Mould Workshop

You step through the Warehouse's back door and find yourself on the Factory Floor for the first time. The space is enormous — much bigger than the Warehouse, with high industrial ceilings and rows of distinct workshops disappearing into the distance. You will visit them in later lessons.

The first workshop, just inside the door, is the **Mould Workshop**. It is small and tidy. Along one wall is a row of large iron moulds, each one shaped like a different kind of warehouse item. There is a stone-shaped mould. A scroll-shaped mould. A vial-shaped mould. A switch-shaped mould. A few moulds for the collection items at the back. A worker stands at a press in the middle of the room.

The work is straightforward. A learner brings an item from the Warehouse — say, a stone marked `27`. The worker studies it, selects the appropriate mould — say, the scroll-shaped one — and presses the stone into the mould. Out comes a new item of the target shape: a scroll bearing the characters `"27"`. The original stone is set aside, completely unchanged. There are now two items where there was one.

This is **casting**. The canonical verb for the whole operation is **press into a mould**, and you saw a glimpse of it in Lesson 1.

---

## The Scroll Mould — `str()`

The most forgiving mould in the workshop is the one shaped like a scroll. It will accept any item the factory uses — any of the nine — and produce a scroll bearing a readable representation of that item.

```python
age = 27
text = str(age)
print(age, type(age))      # 27 <class 'int'>
print(text, type(text))    # 27 <class 'str'>
```

Picture: the `27` stone sits on the warehouse shelf labelled `age`. The worker carries a copy of the stone to the press, fits the scroll mould, and produces a fresh scroll bearing the characters `"27"`. The new scroll is placed in the cubbyhole labelled `text`. The original stone on the `age` shelf is untouched.

The output of the two `print` calls makes the same value look identical (`27 ... 27`) but the inspector tells you they are different items: one is `<class 'int'>`, the other is `<class 'str'>`.

The same mould works on every other kind of item:

```python
print(str(3.14))       # '3.14'   from a vial
print(str(True))       # 'True'   from a switch
print(str(None))       # 'None'   from a vacant cubbyhole
print(str([1, 2, 3]))  # '[1, 2, 3]'   from a numbered row
print(str({"a": 1}))   # "{'a': 1}"   from a filing cabinet
```

Whatever you hand it, the scroll mould produces a scroll. This is exactly what `print()` does internally before calling out — every value you `print` is first quietly pressed into a scroll so the dispatch worker has something to read.

---

## A New Item, Not a Modified One

This is the heart of casting, and it is worth pausing on.

When you press the `27` stone into a scroll mould, you do not *change* the stone into a scroll. You produce a **new item** — a scroll — alongside the original. The stone is still a stone, sitting where it sat. The scroll is a brand-new item, born from the mould.

```python
age = 27
text = str(age)

# age is still a stone — completely unchanged
print(age + 1)         # 28
print(type(age))       # <class 'int'>

# text is a new scroll
print(text + " years") # "27 years"
print(type(text))      # <class 'str'>
```

Picture the warehouse afterwards: two cubbyholes, two cards, two items. The shelf labelled `age` holds the original stone. The shelf labelled `text` holds the new scroll. They are independent. Replacing one does not affect the other.

This pattern — *casting produces a new item* — applies to every mould in the workshop. None of them ever change the original.

---

## The Stone Mould — `int()`

The stone mould produces a stone — a whole number with no decimal part. It accepts:

**A vial.** The fractional part is discarded — *truncated*, not rounded:

```python
print(int(3.7))        # 3   — not 4
print(int(3.2))        # 3
print(int(-3.7))       # -3  — truncates towards zero
print(int(-3.2))       # -3
```

Picture: the vial is examined; the worker reads the whole-number part of the value and carves a stone bearing exactly that. The fractional liquid is poured away. **This is not the same as `round()`** — rounding would give `4` for `3.7`, but the stone mould always truncates.

**A scroll, if it spells a whole number.**

```python
print(int("42"))       # 42
print(int("-7"))       # -7
print(int("0"))        # 0
```

The worker reads the scroll, recognises a number written on it, and carves a matching stone.

**A switch.**

```python
print(int(True))       # 1
print(int(False))      # 0
```

Switches have a long-standing convention: `True` is `1`, `False` is `0`. The mould respects the convention.

**Most things that are not numbers fail.**

```python
print(int("hello"))    # ValueError
print(int("3.14"))     # ValueError — even a decimal string fails!
print(int(None))       # TypeError
```

Picture the worker examining the scroll, finding no whole number on it, and triggering the alarm — Quality Control intercepts before the bad item enters the production line. We will meet this kind of intercept properly in Lesson 22; for now, know that the mould refuses input it cannot honestly turn into a stone.

That second example surprises everyone. The scroll `"3.14"` cannot become a stone in one step. To turn it into a stone, you press it into a vial first, and *then* into a stone:

```python
print(int(float("3.14")))    # 3
```

Two presses, two new items, and the final result is a stone marked `3`.

---

## The Vial Mould — `float()`

The vial mould produces a vial. It is more lenient than the stone mould about what it will accept.

**A stone — straightforward.**

```python
print(float(7))        # 7.0
```

The worker carves the same value into a vial. `7` becomes `7.0`. The numeric value is unchanged; the item type is different.

**A scroll, if it spells any number — whole or decimal.**

```python
print(float("3.14"))   # 3.14
print(float("42"))     # 42.0
print(float("-0.5"))   # -0.5
print(float("2e3"))    # 2000.0
```

Unlike the stone mould, the vial mould accepts decimal strings. It also accepts scientific notation written into a scroll.

**A switch.**

```python
print(float(True))     # 1.0
print(float(False))    # 0.0
```

Same convention as the stone mould — `True → 1.0`, `False → 0.0`.

**Things that are not numbers fail.**

```python
print(float("hello"))  # ValueError
print(float(None))     # TypeError
```

Same alarm as before. The mould refuses scrolls it cannot honestly turn into vials.

---

## The Switch Mould — `bool()`

The switch mould produces a switch — `True` or `False`. It accepts every kind of item, and the rule it follows is *truthiness*: most values are considered true, but a small set are considered false.

```python
print(bool(1))         # True
print(bool(0))         # False    — the only false stone
print(bool(3.14))      # True
print(bool(0.0))       # False    — the only false vial
print(bool("hello"))   # True
print(bool(""))        # False    — the empty scroll is false
print(bool([1, 2]))    # True
print(bool([]))        # False    — the empty numbered row is false
print(bool({}))        # False    — the empty filing cabinet is false
print(bool(None))      # False    — the vacant cubbyhole is false
```

The pattern: **emptiness and zero are false; everything else is true**. The empty scroll, the empty row, the empty cabinet, the empty bin, the zero stone, the zero vial, the vacant cubbyhole — these all press into the `False` switch. Anything with content presses into `True`.

This rule will become very important in Lesson 13 (Junctions), where Python will silently call `bool()` on the value you put inside an `if` statement. For now, it is enough to know how the mould behaves.

---

## The Collection Moulds — `list()`, `tuple()`, `set()`, `dict()`

The four collection items also have moulds. They are useful when you have one kind of collection and need it as another — most often because an operation you want is available on the new shape but not the old one.

**`list()` — numbered row mould.** Accepts any sequence-like input.

```python
print(list("hello"))            # ['h', 'e', 'l', 'l', 'o']
print(list((1, 2, 3)))          # [1, 2, 3]    — sealed crate to numbered row
print(list({1, 2, 3}))          # [1, 2, 3]    — unsorted bin to numbered row (order may vary)
```

The first line is worth seeing once: a scroll, pressed into a numbered row mould, becomes a row of single-character scrolls — one for each letter of the original. This is occasionally exactly what you want.

**`tuple()` — sealed crate mould.** Accepts the same kinds of inputs.

```python
print(tuple([1, 2, 3]))         # (1, 2, 3)    — numbered row to sealed crate
print(tuple("abc"))             # ('a', 'b', 'c')
```

Whenever you have a numbered row whose contents you want to "freeze" — to lock in so nothing can change — pressing into a sealed crate is the way.

**`set()` — unsorted bin mould.** Removes duplicates and order.

```python
print(set([1, 2, 2, 3, 1]))     # {1, 2, 3}    — duplicates fail to land
print(set("hello"))             # {'h', 'e', 'l', 'o'}    — duplicates removed
```

This is the standard way to dedupe a numbered row of items.

**`dict()` — filing cabinet mould.** More specialised; needs key/value pairs as input.

```python
pairs = [("name", "Shane"), ("score", 27)]
print(dict(pairs))              # {'name': 'Shane', 'score': 27}
```

The deep dive on these collection moulds — and on the kinds of inputs each accepts — comes in their respective depth lessons (7, 10, 11). What matters here is the pattern: every type has a mould bearing its name, and pressing one item into another's mould produces a new item of the new type.

---

## When the Mould Refuses

You have seen a few moulds raise an alarm rather than complete the press. The pattern is consistent: when the input genuinely cannot be turned into the target type, Python triggers the alarm rather than guess.

Two alarms come up most often when casting:

- **`ValueError`** — the input is the right *kind* of thing, but its content is wrong. `int("hello")` is a scroll, which is something the stone mould generally accepts, but `"hello"` is not a number.
- **`TypeError`** — the input is the wrong kind of thing entirely. `int(None)` hands a vacant cubbyhole to the stone mould, and the worker has nothing to work with.

Quality Control (Lesson 22) is where you learn to handle these alarms gracefully. For now, you only need to recognise the messages when you see them: an alarm means the mould could not honestly do the work, and the program stopped rather than produce a wrong answer.

---

## What You Now Know

You have crossed the threshold from the Warehouse onto the Factory Floor for the first time, and visited the very first workshop on the Floor — the Mould Workshop.

You know the three central moulds: `str()`, `int()`, `float()`. You know the switch mould `bool()` follows the rule "emptiness and zero are false". You know the four collection moulds — `list()`, `tuple()`, `set()`, `dict()` — and that they are useful when you have one shape of collection and need another.

Most importantly, you know the deep rule of casting: **a press produces a new item; the original is unchanged.** Every line `text = str(age)` adds a new shelf to the warehouse without touching the existing one.

The next lesson takes you back into the Warehouse, where scrolls become a much richer item than they appeared in Lesson 1. You will return to the Factory Floor properly in Lesson 8.

---

## Quick Reference

| Python | Mould image |
|---|---|
| `str(value)` | Press any item into the **scroll mould**. Produces a readable scroll. Always succeeds. |
| `int(value)` | Press into the **stone mould**. Accepts vials (truncates), whole-number scrolls, switches. |
| `float(value)` | Press into the **vial mould**. Accepts stones, numeric scrolls, scientific notation, switches. |
| `bool(value)` | Press into the **switch mould**. False if empty or zero; True otherwise. |
| `list(value)` | Press into the **numbered row mould**. Reshapes other collections, or splits a scroll into single-character scrolls. |
| `tuple(value)` | Press into the **sealed crate mould**. Locks an existing collection's contents. |
| `set(value)` | Press into the **unsorted bin mould**. Removes duplicates and order. |
| `dict(pairs)` | Press into the **filing cabinet mould**. Needs key/value pairs as input. |
| `int("hello")` | The mould refuses — `ValueError`. The scroll has no number on it. |
| `int(None)` | The mould refuses — `TypeError`. The vacant cubbyhole has nothing to press. |
| Casting in general | Produces a **new item**. The original is never modified. |

---

## Try It

Open a Python prompt or any Python editor.

**The forgiving scroll mould:**

```python
print(str(27))
print(str(3.14))
print(str(True))
print(str(None))
print(str([1, 2, 3]))
```

The scroll mould accepts everything and gives you a readable scroll back.

**The stone mould — three flavours of input:**

```python
print(int(3.7))         # truncate, do not round
print(int("42"))
print(int(True))
```

Try the inverse: deliberately confuse it.

```python
print(int("3.14"))      # ValueError — needs a whole-number string
```

Now press it through the vial mould first:

```python
print(int(float("3.14")))   # 3 — two presses, one final stone
```

**The vial mould — more lenient:**

```python
print(float(7))
print(float("3.14"))
print(float("2e3"))
print(float(True))
```

**The switch mould — emptiness rules:**

```python
print(bool(0))
print(bool(""))
print(bool([]))
print(bool({}))
print(bool(None))
print(bool("anything"))
print(bool([0]))         # a numbered row containing one stone — non-empty, so True
```

Notice that last one: the row contains a `0`, which is itself false-y, but the row itself has *content*, so the row is true.

**The original is unchanged:**

```python
age = 27
text = str(age)
print(age, type(age))
print(text, type(text))
print(age + 1)          # the stone still works as a stone
```

Two shelves, two items, both still serving their original purposes.

**Useful collection casts:**

```python
print(set([1, 2, 2, 3, 1]))         # dedupe
print(list("hello"))                 # split a scroll into characters
print(tuple([1, 2, 3]))              # freeze a numbered row
```

These three patterns are common in real Python code.

---

## Where Next?

You return to the Warehouse. The next several lessons go deep on each item — starting with the most heavily used of all of them.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 6 | Z3 — Warehouse | Strings — scrolls in depth (including f-strings) |
| Lesson 7 | Z3 — Warehouse | Lists — numbered rows in depth |
| Lesson 8 | Z5 — Factory Floor | Functions — the first workstation |
| Lesson 9 | Z3 — Warehouse | Scope — the locked rooms in full |

*See the full lesson map in **The Factory — A Complete Map**.*
