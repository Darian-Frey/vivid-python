# Python for Hyperphantasic Minds
## Lesson 4 — Numbers

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 4 of 25  
> **Topic**: Numbers — stones and vials in depth  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You are back in the Warehouse. The cubbyholes are familiar. In Lesson 3 the foreman walked you down all nine kinds of items the factory uses; today you stop at the first two — the stone and the vial — and look at them properly.

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

Numbers are how the factory counts, measures, and reasons about quantity. Almost every program you write from here on will spend some of its time doing arithmetic on stones and vials. By the end of this lesson you will know which item to reach for, how to do every basic calculation, and the one famous trap that catches every beginner working with vials.

---

## Stones in Depth — `int`

A stone, you already know, is a small dense item with a whole number painted on its face. Here are the things worth knowing about stones beyond Lesson 1.

**Stones come in any size, positive or negative.**

```python
small = 7
medium = 1000
big = 1234567890
negative = -42
zero = 0
```

A negative number is still a stone — the minus sign is part of the number painted on the face, not a separate item.

**Python stones have no maximum size.**

This is unusual. In many programming languages, an integer is limited to about ±2 billion or ±9 quintillion before it overflows into nonsense. In Python, a stone simply grows as large as the number it carries. The factory has stones for any whole number you can think of, no matter how vast.

```python
huge = 12345678901234567890123456789
also_fine = 2 ** 200    # two raised to the power of two hundred
```

You can hand Python an integer with hundreds of digits and it will calmly carve a stone for it. Other languages cannot. This is one of Python's quiet superpowers.

**Underscores for readability.**

Long stones are hard to read at a glance. Python lets you put underscores between digits as visual breaks — they do not change the value, only the readability for humans:

```python
million = 1_000_000
distance = 384_400          # km to the moon
visa_card = 4242_4242_4242_4242
```

The stone marked `1_000_000` is the same stone as the one marked `1000000`. The underscores are pinned to the surface for the human reader; Python ignores them when calculating.

**Other ways to write the same stone.**

Python accepts a few alternative spellings for the same value, useful in specific contexts:

```python
ten_a = 10
ten_b = 0b1010      # binary — base 2
ten_c = 0o12        # octal — base 8
ten_d = 0xa         # hexadecimal — base 16
```

All four cubbyholes hold the same stone — a stone marked `10`. The prefix tells Python which numbering system you used to write it on the form. You will most often see `0x` (hex) when working with colours, file formats, or low-level computer details. Most beginner code never uses these forms; it is enough to know they exist.

---

## Vials in Depth — `float`

A vial is the second numeric item — a small glass bottle with a stopper, holding a number that may carry a fractional part. The liquid inside shifts when the vial is set down. The number is etched on the glass.

**Anything with a decimal point is a vial.**

```python
pi = 3.14159
gravity = 9.81
half = 0.5
negative_half = -0.5
also_a_vial = 3.0           # the .0 makes it a vial, even though the value is whole
```

That last example is worth pausing on. `3` is a stone. `3.0` is a vial. The numeric value is the same, but the items are different. The factory keeps them physically distinct because the operations available to each are slightly different.

**Scientific notation.**

For very large or very small numbers, vials accept a shorthand from physics and engineering:

```python
distance_to_sun = 1.496e11    # 1.496 × 10¹¹ — about 150 billion metres
electron_mass  = 9.109e-31    # 9.109 × 10⁻³¹ — extremely small
big_simple     = 2e3          # 2 × 10³ — exactly 2000.0
```

The `e` means "times ten to the power of". A number written this way is always a vial, even if the underlying value is a whole number. `2e3` is a vial reading `2000.0`, not a stone reading `2000`.

**Special vials — infinity.**

A small set of vials hold values that are not normal numbers. The most useful is positive infinity, which behaves as you would expect — larger than any real number you can name:

```python
infinity = float("inf")
print(infinity > 10**100)     # True — infinity is larger than anything finite
```

There is also `float("-inf")` for negative infinity and `float("nan")` for "not a number" (the result of mathematically undefined operations). You will rarely create these by hand; they appear in scientific calculations. It is enough to know they exist and that they are vials.

---

## The Vial Precision Trap

Here is the famous one. Type this and watch closely:

```python
print(0.1 + 0.2)
```

Python answers:

```
0.30000000000000004
```

Not `0.3`. Almost `0.3`. Off by a microscopic sliver. Every beginner meets this and thinks they have done something wrong. They have not. Python is being honest.

**Why it happens.**

Vials are physical glass containers holding a measured quantity of liquid. The number etched on the glass is the *intended* value. The actual amount of liquid inside is *almost exactly* that — but not quite. The factory has chosen to use vials of a standard manufacturing precision — the same precision every modern programming language uses, in fact, because computers all share the same underlying machinery for these values. For most numbers, the slop between the etched label and the real contents is invisibly small. For some numbers — `0.1` is a famous one — the underlying machinery cannot represent the value exactly in binary, so the vial holds an amount imperceptibly different from what its label says.

When you ask the factory to add the contents of two such vials, the resulting vial reflects the cumulative slop. Hence `0.30000000000000004`.

**What to do about it.**

Three rules cover almost every case you will meet:

- **Trust vials for ordinary measurement** — distances, weights, fractions, percentages, scientific quantities. The slop is far below any reasonable measurement error.
- **Do not compare two vials with `==`.** Asking *"is this vial exactly equal to that one?"* is a poorly-formed question. Ask instead *"is the difference smaller than some tolerance?"*: `abs(a - b) < 0.0001`.
- **Do not use vials for money.** A vial holding `1.10` may not be exactly `1.10`. For currency, use stones (count in pence or cents) or Python's exact-decimal toolkit (the `decimal` module — fetched from the Tool Store, Lesson 20).

This is not a Python flaw. Every modern language behaves this way for the same reason. Python is simply showing the real number it has, instead of hiding the slop.

---

## Working with Numbers — Arithmetic

The Factory Floor (Z5) is where serious work happens, but a small set of arithmetic tools live so close to the warehouse that they may as well be on the shelves themselves. You can use them right now without leaving Z3.

**Addition, subtraction, multiplication.**

```python
print(2 + 3)         # 5
print(10 - 4)        # 6
print(6 * 7)         # 42
```

Hand the factory two stones; it produces a third stone. Stone arithmetic in stone arithmetic out.

**True division — `/`.**

```python
print(10 / 4)        # 2.5
print(10 / 5)        # 2.0  — note the .0
print(8 / 2)         # 4.0  — also a vial!
```

This is the first surprise. The `/` operator *always returns a vial*, even when both inputs are stones and the result is a whole number. The reason: division is a measurement question ("how many of these fit inside that?") and measurement is a vial-shaped operation. If you wanted the integer answer, you would have asked for floor division.

**Floor division — `//`.**

```python
print(10 // 4)       # 2  — a stone
print(10 // 3)       # 3  — a stone
print(-10 // 3)      # -4  — rounds towards negative infinity
```

The `//` operator gives you the integer part of the division — the answer to "how many whole groups of `n` fit?". When both inputs are stones, the result is a stone.

That last example is worth a second look. Floor division rounds *down*, not towards zero — which means for negative numbers it goes more negative, not less. `-10 // 3` is `-4`, not `-3`. This rarely matters in beginner code, but it has surprised every Python programmer at least once.

**Modulo — `%`.**

```python
print(10 % 3)        # 1  — the remainder of 10 ÷ 3
print(20 % 4)        # 0  — 20 is exactly four 5s
print(7 % 2)         # 1  — odd numbers leave 1 when divided by 2
```

The `%` operator gives you the remainder — what is left over after the floor division. It is the everyday tool for "is this number even?", "is today the third item?", "how many minutes past the hour?". You will reach for it more often than you might guess.

**Exponentiation — `**`.**

```python
print(2 ** 10)       # 1024
print(3 ** 3)        # 27
print(2 ** 0.5)      # 1.4142135623730951  — square root of 2
```

The `**` operator raises the first number to the power of the second. For whole-number powers of stones you get a stone back. For non-integer powers you get a vial.

**Mixed types behave gracefully.**

```python
print(2 + 3.0)       # 5.0  — a vial
print(10 / 4)        # 2.5  — a vial
print(10 // 4.0)     # 2.0  — a vial
```

Whenever a stone and a vial meet in an arithmetic expression, Python promotes the stone to a vial first. The factory does this quietly; no error, no fuss. The result is always a vial when any vial is involved.

This is in deliberate contrast to mixing a stone and a scroll — that *does* fail, as you saw in Lesson 1, because adding text and numbers is not a meaningful operation. Mixing two numeric items is meaningful, so Python permits it.

**Order of operations.**

Python uses the standard mathematical order: `**` first, then `*`, `/`, `//`, `%`, then `+`, `-`. Use parentheses when you want to be explicit:

```python
print(2 + 3 * 4)        # 14   — multiplication first
print((2 + 3) * 4)      # 20   — parentheses force the addition first
print(2 ** 3 + 1)       # 9    — exponent first
```

When in doubt, parenthesise. The factory does not charge you for clarity.

---

## Updating a Number

Once a stone or a vial is on a shelf, you often want to *update* it — increase a score, decrement a counter, double a measurement. The natural way uses what you already know from Lesson 1:

```python
score = 0
score = score + 1       # the 0 stone is removed; a 1 stone is placed
score = score + 1       # now a 2 stone
score = score + 10      # now a 12 stone
print(score)            # 12
```

Picture each line: lift the current stone off the shelf, work out the new value, place a new stone in its place. The card on the shelf still reads `score`.

Because this pattern is so common, Python provides a shorthand. Each of these operators reads as *"replace this with the result of doing X to it"*:

```python
score += 1          # same as score = score + 1
total -= 5          # same as total = total - 5
count *= 2          # same as count = count * 2
distance /= 10      # same as distance = distance / 10
```

Use whichever form you find clearer. Most experienced Python programmers reach for the shorthand because it puts the variable name only once. The visual is identical: a stone is replaced with one slightly different.

---

## The Standard Toolbox

The factory keeps a handful of built-in tools always within arm's reach — small workstations for common numeric tasks, available without fetching anything from the Tool Store. We will explain *why* they are always available in Lesson 9; for now, just use them.

```python
print(abs(-7))             # 7    — absolute value (drop the minus sign)
print(round(3.7))          # 4    — round to the nearest stone
print(round(3.14159, 2))   # 3.14 — round to 2 decimal places
print(min(4, 1, 9, 2))     # 1    — the smallest of several values
print(max(4, 1, 9, 2))     # 9    — the largest
print(pow(2, 10))          # 1024 — same as 2 ** 10
```

`abs`, `round`, `min`, `max`, and `pow` are part of every Python program, everywhere, by default. You do not need to import anything to use them.

---

## Random Numbers — A Forward Glance

Many programs need random numbers — for games, simulations, sampling, shuffling. Python provides them, but through a separate toolkit (the `random` toolbox) that has to be fetched from the Tool Store before use:

```python
import random
print(random.randint(1, 6))     # a random stone between 1 and 6 — a die roll
```

That `import` line is a Z1 — Tool Store — operation. We will visit the Tool Store properly in Lesson 20. For now, know that random numbers exist and are easy to reach when you need them.

---

## A Brief Note on Crystals — `complex`

Python has a third numeric type used in advanced mathematical work: the **complex number**, whose canonical item in this series is the **crystal** — a geometric form with two distinct facets, one for the real part and one for the imaginary.

```python
z = 3 + 4j
print(type(z))      # <class 'complex'>
```

The lower-case `j` is Python's notation for the imaginary unit. Crystals are real, useful items — but they belong to electrical engineering, signal processing, and certain branches of physics, not to beginner Python. We will not cover them in this series. If you ever need them, you will recognise them; that is enough.

---

## What You Now Know

You have looked carefully at the two numeric items in the warehouse. Stones are whole numbers — any size, no maximum, with a few alternative spellings. Vials are decimal-pointed numbers, including the awkward truth that `0.1 + 0.2` is not exactly `0.3`. You can perform the full range of arithmetic on either kind, with the one rule that mixing a stone and a vial always produces a vial.

You also know how to update a number on a shelf, the small set of always-available tools (`abs`, `round`, `min`, `max`, `pow`), and that random numbers and crystals exist for when you need them.

Numbers are now no longer abstract. A stone is a stone. A vial is a vial. Adding them is a defined act. Dividing them produces a vial unless you ask for floor division. Comparing two vials for exact equality is a question worth refusing to ask.

---

## Quick Reference

| Python | Description |
|---|---|
| `27`, `-5`, `0` | A **stone** — whole number, any size. |
| `3.14`, `2e3`, `0.5` | A **vial** — decimal-pointed or scientific notation. |
| `1_000_000` | Underscores for readability; the value is unchanged. |
| `0b1010`, `0o12`, `0xa` | Binary, octal, hexadecimal — all stones marked `10`. |
| `+`, `-`, `*` | Add, subtract, multiply. Stones in → stone out; any vial → vial out. |
| `a / b` | True division — *always* returns a vial. |
| `a // b` | Floor division — integer part; rounds towards negative infinity. |
| `a % b` | Modulo — remainder after floor division. |
| `a ** b` | Exponentiation — `a` to the power of `b`. |
| `a += 1`, `a *= 2`, etc. | Shorthand for `a = a + 1`, `a = a * 2`, etc. |
| `abs(x)` | Absolute value — drop the minus sign. |
| `round(x)`, `round(x, n)` | Round to nearest stone, or to `n` decimal places. |
| `min(...)`, `max(...)` | Smallest, largest. |
| `pow(a, b)` | Same as `a ** b`. |
| `0.1 + 0.2` | The famous near-miss — vials are precise but not exact. Do not compare vials with `==`. |
| `complex` (`3 + 4j`) | A **crystal** — advanced numeric type, not covered in this series. |

---

## Try It

Open a Python prompt or any Python editor.

**Stones in their full glory:**

```python
small = 7
huge = 2 ** 200
print(small)
print(huge)
print(type(huge))
```

Hand the factory `2 ** 200` and watch it carve the stone without complaint. Most other languages cannot do this.

**Underscores and bases:**

```python
print(1_000_000)         # 1000000
print(0xff)              # 255
print(0b1010)            # 10
```

The underscores are gone in the output — they were only ever for the human reader.

**Vials and the precision trap:**

```python
print(0.1 + 0.2)
print(0.1 + 0.2 == 0.3)
print(abs((0.1 + 0.2) - 0.3) < 1e-9)
```

The first line is the famous near-miss. The second line is the wrong question (and Python answers it honestly: `False`). The third line is the right question (`True`).

**The five operators:**

```python
a = 17
b = 5
print(a + b)        # 22
print(a - b)        # 12
print(a * b)        # 85
print(a / b)        # 3.4   — vial
print(a // b)       # 3     — stone
print(a % b)        # 2     — stone
print(a ** b)       # 1419857
```

Notice which operations return stones and which return vials.

**Updating a stone in place:**

```python
score = 0
for _ in range(5):
    score += 1
    print(score)
```

You have not formally met `for ... in range(...)` yet — that is Lesson 14 — but read it as "do this five times". Each pass, the stone on the `score` shelf is replaced with one one larger. The output is `1 2 3 4 5`.

**The standard toolbox:**

```python
print(abs(-273))
print(round(3.14159, 2))
print(min(7, 1, 4))
print(max(7, 1, 4))
print(pow(3, 4))
```

These five tools handle a startling proportion of the everyday numeric work most programs need.

---

## Where Next?

You have now spent two lessons inside the Warehouse looking at items you already know exist. The next lesson is the first to *change* an item — taking a stone and pressing it into a scroll, or vice versa. That work happens at the boundary between the warehouse and the factory floor.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 5 | Z3 → Z5 | Casting — pressing objects into moulds |
| Lesson 6 | Z3 — Warehouse | Strings — scrolls in depth (including f-strings) |
| Lesson 7 | Z3 — Warehouse | Lists — numbered rows in depth |
| Lesson 8 | Z5 — Factory Floor | Functions — the first workstation |

*See the full lesson map in **The Factory — A Complete Map**.*
