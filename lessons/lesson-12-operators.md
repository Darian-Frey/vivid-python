# Python for Hyperphantasic Minds
## Lesson 12 — Operators

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 12 of 25  
> **Topic**: Operators — the tools on the floor  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You leave the Warehouse for an extended stay. Lessons 12 through 19 all happen on the **Factory Floor**. You have been here twice before — Lesson 5 for the Mould Workshop, Lesson 8 to build a workstation — but you have not yet looked properly at the *tools* that fill the rest of the Floor. Today is the orientation.

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

An **operator** is a small physical tool on the Factory Floor — a calculator, a scales, a switch-combiner, a labelling stamp. Each tool has a defined input shape and a defined output. You hand it items, it does its specific job, it gives you back a result.

You have already met seven of the most-used operators in Lesson 4 (arithmetic). This lesson rounds out the full set: comparisons that produce switches, the logical tools that combine switches, the identity check that tells you whether two name cards point at the same physical item, the membership check across collections, and a handful of more specialised tools you will reach for less often.

---

## The Tools on the Floor

Picture the Factory Floor with a long workbench along one wall. The bench is loaded with tools, each one labelled. Some are simple — a calculator that adds two numbers. Some are paired — a scales that compares the weight of two items and produces a switch. Some are odd-looking — a pair of magnets called `is` that only fire if two name cards are pinned to the same physical shelf.

Each tool produces a *result*, which becomes a new item somewhere — either placed on a shelf, handed into another tool, or passed to a junction that decides what happens next.

The full inventory falls into seven groups:

1. **Arithmetic** — calculator-like tools for stones and vials (met in Lesson 4)
2. **Assignment** — the placement tool and its shorthand siblings (met in Lessons 1 and 4)
3. **Comparison** — scales and equality-checkers that produce switches
4. **Logical** — switch-combiners (`and`, `or`, `not`)
5. **Identity** — the are-these-the-same-shelf check (`is`)
6. **Membership** — the is-this-in-the-collection check (`in`)
7. **Bitwise** — specialist tools for bit-level work on stones

Most everyday Python uses 1–6 heavily. Group 7 you will reach for only occasionally; it is here for completeness.

---

## Arithmetic — Brief Recap

You learned these in Lesson 4. Listing them again here so the inventory is complete:

```python
print(2 + 3)        # 5
print(10 - 4)       # 6
print(6 * 7)        # 42
print(10 / 4)       # 2.5    — / always returns a vial
print(10 // 4)      # 2      — floor division
print(10 % 3)       # 1      — modulo (remainder)
print(2 ** 10)      # 1024   — exponent
```

The same rules from Lesson 4 still hold: stones in, stone out (except `/`); any vial involved, vial out; comparisons with scrolls trigger an alarm.

---

## Assignment — Brief Recap

Also already covered, in Lessons 1 and 4. The placement tool and its shorthands:

```python
x = 10              # place a stone on the shelf
x += 1              # replace with a stone one larger
x -= 2              # replace with a stone two smaller
x *= 3              # replace with a stone three times the value
x /= 2              # replace with a vial half the value
```

There are augmented forms for every arithmetic operator: `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `**=`. All read as *"replace this with the result of applying the operator to it"*.

One subtle but important point about assignment: it is *not* an operator in the way `+` is. You cannot use it inside an expression. `(x = 10) + 5` is a syntax error. Assignment is a statement on its own line, not a tool that produces a value. (Python 3.8 introduced a walrus form `:=` for assignment-inside-expression in certain contexts; you will not need it as a beginner.)

---

## Comparison — Scales That Produce Switches

The comparison tools answer questions about two items. Each one produces a **switch** — `True` or `False`. They never change the items they compare.

```python
print(3 == 3)       # True
print(3 == 4)       # False
print(3 != 4)       # True   — not equal
print(5 > 3)        # True
print(5 < 3)        # False
print(5 >= 5)       # True
print(5 <= 5)       # True
```

Six tools. Read each as a question:

- `a == b` — *are these equal?*
- `a != b` — *are these different?*
- `a < b`, `a <= b`, `a > b`, `a >= b` — *is `a` less than / less than or equal / greater than / greater than or equal to `b`?*

The result is always a switch, regardless of what was compared. This means comparisons can be stored on shelves:

```python
adult = age >= 18
print(adult)        # True or False, depending on age
```

The shelf `adult` holds a switch. Whatever workstation reads it later sees a clean yes/no answer.

**Comparing different types of items.**

The numeric items compare freely with each other — a stone and a vial of the same value compare equal:

```python
print(3 == 3.0)     # True
print(3 < 4.5)      # True
```

Scrolls compare alphabetically, which means by character codes — lowercase letters compare *greater than* uppercase, for example:

```python
print("apple" < "banana")    # True
print("Z" < "a")              # True   — lowercase has a higher character code than uppercase
print("apple" == "Apple")    # False  — case-sensitive
```

Comparing across very different types (a stone with a scroll, say) usually triggers an alarm:

```python
print(3 < "hello")   # TypeError — stones and scrolls cannot be compared
```

The only universal exception is `==` and `!=`, which always work — they just return `False` when comparing genuinely different kinds of items:

```python
print(3 == "3")     # False   — a stone is not a scroll
print(3 == 3.0)     # True    — but stones and vials compare by value
```

A common beginner pattern: comparison chains. Python supports the natural mathematical form:

```python
age = 25
print(18 <= age < 65)        # True — both conditions checked in one expression
```

Read as "is `age` between 18 and 65?". This expands internally to `18 <= age and age < 65`, but is much clearer in the natural form.

---

## Logical — Combining Switches

Three tools combine switches into other switches.

**`and` — both must be up.**

```python
print(True and True)     # True
print(True and False)    # False
print(False and True)    # False
print(False and False)   # False
```

Picture two switches feeding into the `and` tool. The output switch goes up only if both inputs are up.

**`or` — at least one must be up.**

```python
print(True or False)     # True
print(False or False)    # False
print(True or True)      # True
```

Picture two switches feeding into `or`. The output goes up if either input is up — or both.

**`not` — flip a switch.**

```python
print(not True)          # False
print(not False)         # True
```

The simplest tool. Picture a single switch passing through `not`; the output is its opposite.

**Combining checks naturally.**

```python
age = 25
income = 30_000
eligible = age >= 18 and income > 20_000
print(eligible)          # True
```

Three items being read: `age`, `income`, and the result of two comparisons combined with `and`. The whole right-hand side evaluates to a switch and is placed on the `eligible` shelf.

**Short-circuit evaluation — the tool stops early.**

A subtle but important property of `and` and `or`: they only look at as much of the expression as they need to.

```python
def is_dangerous():
    print("danger check called!")
    return False

False and is_dangerous()    # is_dangerous is NEVER called
True or is_dangerous()      # is_dangerous is NEVER called
```

Picture: the `and` tool examines its left input first. If it sees `False`, it already knows the result must be `False` — no point looking at the right side. The right-hand expression is never even evaluated. Similarly for `or` with a `True` left-hand side.

This is *short-circuit* evaluation, and it has a useful consequence: you can put a cheap check first and a costly one second, and the costly one only runs when needed:

```python
if cached_value is not None and expensive_lookup(cached_value):
    ...
```

If `cached_value` is `None`, the expensive lookup is not even attempted. This pattern is common in production Python.

**One last quirk worth knowing.** `and` and `or` return the *value that decided the answer*, not always a literal switch. Most of the time the distinction is invisible. Occasionally it powers an idiom:

```python
name = user_input or "anonymous"      # if user_input is empty / None / 0 / False, use "anonymous"
```

If `user_input` is something "truthy" (any non-empty value), `or` returns it. If it is falsy (empty, zero, None), `or` returns `"anonymous"`. The Lesson 5 truthiness rule — empty and zero are False, everything else is True — does the work for you.

---

## Identity — Same Item or Just Equal? (`is` vs `==`)

This is the most subtle pair on the Floor. The trap they create catches every Python programmer at some point.

`==` asks *"are these two items equal in value?"* — examine both, compare their contents.

`is` asks *"are these two name cards pointing at the same physical shelf?"* — a much narrower question.

You met this distinction in disguise in Lesson 7, in the copy trap. Here it is again with the operator that makes it explicit:

```python
a = [1, 2, 3]
b = a                # one row, two cards
c = a.copy()         # two rows; same contents

print(a == b)        # True — same contents
print(a == c)        # True — same contents
print(a is b)        # True — same physical row
print(a is c)        # False — different physical rows
```

Picture the warehouse: `a` and `b` are two cards pinned to the same wheeled frame. `c` is a different frame, with copies of the same items inside. `a == b` and `a == c` both come back True — the contents match. But `a is b` is True (same shelf), and `a is c` is False (different shelves).

**Use `==` when you care about *value*.** Use `is` when you care about *physical identity*. Almost every comparison you write should be `==`.

**The one exception where `is` is the right tool: `None`.**

There is only ever one `None` in the entire factory. Every cubbyhole that holds "vacant" is pointing at the same physical "absence". So the idiomatic way to check whether a shelf is vacant is:

```python
if result is None:
    ...
```

Not `result == None`. Both work, but `is None` is the convention because it states the intent precisely: *the shelf is the vacant one*, not *the shelf holds something equal to the vacant cubbyhole*. The same applies to `is True`, `is False` — though you almost never need those (`if x:` is the usual form).

**The famous trap.**

```python
a = 256
b = 256
print(a is b)        # True

a = 257
b = 257
print(a is b)        # False (in most Python implementations)
```

For tiny stones, Python keeps a single physical copy of each value as an optimisation — so two assignments of `256` happen to point at the same shelf. For larger stones, separate copies are made — so `a is b` is False even though `a == b` is True.

This is an implementation detail you should never rely on. The lesson is the deeper one: **`is` is never the right tool for checking value equality**, even when it appears to work. Use `==`. Reserve `is` for `None` checks and the rare cases where physical identity genuinely is what you mean.

`is not` is the negation, paired with `is` the way `!=` is with `==`:

```python
if result is not None:
    process(result)
```

---

## Membership — Recap

You met `in` and `not in` across the collection lessons. They are operators, and they belong on this floor for completeness.

```python
print(3 in [1, 2, 3])             # True
print("py" in "python")           # True
print("apple" in {"apple", "bread"})  # True (set)
print("name" in {"name": "Shane"})    # True (dict — checks keys, not values)

print(99 not in [1, 2, 3])        # True
```

The collection-specific behaviour is in Lessons 6 (scrolls), 7 (rows), 10 (cabinets), 11 (bins). The `in` and `not in` operators are the consistent surface across all of them.

---

## Bitwise — A Brief Tour

These are specialist tools that operate on stones at the level of their internal binary form. Most beginner Python never uses them. They appear in low-level work — flag combinations, network protocols, bit-twiddling tricks.

```python
print(0b1100 & 0b1010)     # 0b1000  — bitwise AND
print(0b1100 | 0b1010)     # 0b1110  — bitwise OR
print(0b1100 ^ 0b1010)     # 0b0110  — bitwise XOR
print(~0b1100)             # an oddly-large negative — bitwise NOT
print(1 << 4)              # 16      — left shift (× 2 four times)
print(64 >> 2)             # 16      — right shift (÷ 2 twice)
```

Two things to know:

- **The `|`, `&`, `^` symbols are reused across the language.** On stones, they are bitwise. On bins, they are set algebra (Lesson 11). On filing cabinets, `|` is dict merge (Lesson 10). Python disambiguates by the types involved.
- **`<<` and `>>` shift bits left and right.** Each left shift doubles the value; each right shift halves it (with integer truncation).

That is the entire bitwise toolset. Move on; we will not use it in the rest of this series.

---

## Operator Precedence — Which Tool Runs First

When several operators appear in the same expression, Python evaluates them in a fixed order. The full precedence table is long; the essentials you need to remember are:

1. `**` first
2. `*`, `/`, `//`, `%`
3. `+`, `-`
4. comparison operators (`==`, `!=`, `<`, `>`, `<=`, `>=`)
5. `not`
6. `and`
7. `or`

A small example showing several at work:

```python
result = 3 + 4 * 2 ** 2 > 15 and not False
#        3 + 4 * 4 > 15 and not False
#        3 + 16 > 15 and not False
#        19 > 15 and True
#        True and True
#        True
```

When in doubt, parenthesise. Python charges nothing for clarity. The expression below is equivalent and easier to read:

```python
result = ((3 + (4 * (2 ** 2))) > 15) and (not False)
```

---

## What You Now Know

The tools on the Floor are now familiar. Arithmetic (Lesson 4), assignment (Lessons 1 and 4), comparison (today), logical (today), identity (today), membership (Lessons 6, 7, 10, 11), and bitwise (today, briefly).

You know that comparisons produce switches; that `and` and `or` short-circuit; that `==` asks about value and `is` asks about physical identity (and `is None` is the one place where `is` is the natural choice); and that operator precedence has a predictable order — but parenthesising is always free.

You now have everything you need to read and write expressions. The next lesson takes a small set of these — comparisons feeding into a junction — and builds the first piece of *control flow*: deciding what work to do next based on the answer.

---

## Quick Reference

**Arithmetic** (Lesson 4 in depth)

| Operator | Meaning |
|---|---|
| `+`, `-`, `*` | Add, subtract, multiply. |
| `/` | True division — *always* returns a vial. |
| `//` | Floor division — integer part, rounds towards negative infinity. |
| `%` | Modulo — remainder. |
| `**` | Exponent. |

**Comparison** (returns a switch)

| Operator | Meaning |
|---|---|
| `==`, `!=` | Equal, not equal — always work; safe across types. |
| `<`, `<=`, `>`, `>=` | Less-than-style comparisons; require comparable types. |
| `a < b < c` | Comparison chain — natural mathematical form. |

**Logical** (combines switches)

| Operator | Meaning |
|---|---|
| `and` | Both must be true. Short-circuits on a false left. |
| `or` | At least one must be true. Short-circuits on a true left. |
| `not` | Flips a switch. |

**Identity**

| Operator | Meaning |
|---|---|
| `is`, `is not` | Same physical shelf? Reserve for `is None` checks. |
| `==`, `!=` | Same value? Use for almost everything else. |

**Membership** (Lessons 6, 7, 10, 11)

| Operator | Meaning |
|---|---|
| `in`, `not in` | Is this item present in the collection? |

**Bitwise** (specialist; rarely needed)

| Operator | Meaning |
|---|---|
| `&`, `\|`, `^`, `~` | Bitwise AND, OR, XOR, NOT. |
| `<<`, `>>` | Left and right shift. |

**Precedence (essentials, high to low)**

`**` → `*` `/` `//` `%` → `+` `-` → comparisons → `not` → `and` → `or`

When in doubt, parenthesise.

---

## Try It

**Comparison — switches produced, switches stored:**

```python
age = 25
is_adult = age >= 18
is_senior = age >= 65
print(is_adult, is_senior)

print("apple" < "banana")
print("Z" < "a")
print(3 == 3.0)
print(3 == "3")
```

**Comparison chains:**

```python
age = 25
print(18 <= age < 65)
print(0 < age < 18)
```

**Logical — combining:**

```python
age = 25
income = 30_000
eligible = age >= 18 and income > 20_000
print(eligible)

is_holiday = False
is_weekend = True
day_off = is_holiday or is_weekend
print(day_off)

print(not True)
```

**Short-circuit — see it in action:**

```python
def noisy():
    print("noisy() was called")
    return True

print("--- and with False on left:")
result = False and noisy()       # noisy() should NOT be called
print(result)

print("--- and with True on left:")
result = True and noisy()        # noisy() IS called
print(result)
```

**The `or default` idiom:**

```python
user_input = ""
name = user_input or "anonymous"
print(name)

user_input = "Shane"
name = user_input or "anonymous"
print(name)
```

**`is` vs `==`:**

```python
a = [1, 2, 3]
b = a
c = a.copy()

print(a == b, a is b)        # True True
print(a == c, a is c)        # True False
```

**`is None` — the idiomatic check:**

```python
def find(x, row):
    if x in row:
        return row.index(x)
    return None

result = find(7, [1, 2, 3])
if result is None:
    print("not found")
else:
    print(f"found at position {result}")
```

**Precedence — without parentheses and with:**

```python
print(3 + 4 * 2)               # 11   — * before +
print((3 + 4) * 2)             # 14   — parentheses force the addition first

print(True or False and False) # True — `and` before `or`
print((True or False) and False)  # False — parentheses flip the order
```

That last pair catches a lot of beginners. Python evaluates `and` before `or`. When you want the other order, parenthesise.

---

## Where Next?

The first thing the Floor does with a switch is *route material* down one path or another. That is the **junction**, and it is the next lesson. From there, the conveyor belts (loops), the comprehensions (compact belt expressions), and eventually the workshop blueprints (classes) fill out the Floor.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 13 | Z5 — Factory Floor | If / Else — junctions and inspection gates |
| Lesson 14 | Z5 — Factory Floor | Loops — conveyor belts, emergency stops, skip gates |
| Lesson 15 | Z5 — Factory Floor | Comprehensions — compact belt expressions |
| Lesson 16 | Z5 — Factory Floor | Classes — workshop blueprints and built workshops |

*See the full lesson map in **The Factory — A Complete Map**.*
