# Python for Hyperphantasic Minds
## Lesson 6 — Strings

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 6 of 25  
> **Topic**: Strings — scrolls in depth (including f-strings)  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You are back in the Warehouse. The brief excursion to the Mould Workshop is behind you. From here through Lesson 11, every lesson stays inside this building (with one short visit to a workstation in Lesson 8). Today is the long-promised deep look at the scroll.

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

You first met the scroll in Lesson 1 — a roll of parchment with characters visible on the surface. In Lesson 3 you saw it on the warehouse tour. In Lesson 5 you saw the scroll mould produce one from any other item. Today you take it apart: how to measure it, how to read a single character off it, how to cut a section out, how to join two together, how to build one from the contents of named cubbyholes, and the small set of built-in tools every scroll carries.

This is the longest lesson in the series so far. The scroll is the most heavily used item in the factory; it deserves the time.

---

## The Scroll, Looked At Closely

A scroll is a roll of parchment. Short scrolls fit in the palm and are tied with a ribbon. Long scrolls are heavy rolls, sometimes spilling off the edge of the worktop.

Every character on the scroll is at a definite position. The first character sits at position **0**. The next at **1**. The next at **2**, and so on, all the way to the end. (Position numbering starts at zero for the same reason numbered rows do — both are sequences, and the numbering is shared.)

A short scroll, laid flat:

```
positions:   0  1  2  3  4
characters:  H  e  l  l  o
```

Five characters, positions `0` through `4`. The scroll itself is a single item that holds the whole sequence — but the *contents* are arranged in this orderly way, and many of the operations you will meet in this lesson reach into the scroll at specific positions.

Two strict rules govern scrolls:

- **Every character is part of the scroll.** Spaces, punctuation, newlines — all of them count. `"a b"` is a scroll of *three* characters, not two.
- **A scroll, once written, cannot be edited.** You cannot reach in and change the `H` to a `J`. To get a different scroll, you produce a new one. We will return to this rule near the end of the lesson — it is the deepest property of scrolls and shapes how every scroll operation behaves.

---

## Length — `len()`

The first thing you ever want to know about a scroll is how long it is.

```python
greeting = "Hello"
print(len(greeting))         # 5

empty = ""
print(len(empty))            # 0

long = "the quick brown fox"
print(len(long))             # 19
```

`len()` is a built-in member of the factory's standard kit. Hand it a scroll and it returns a stone — the count of characters. The empty scroll has length `0`. Every other scroll has length `1` or more.

`len()` works on every collection item too — numbered rows, sealed crates, unsorted bins, filing cabinets. We will see those uses in the relevant lessons. For scrolls, `len()` counts characters.

---

## Picking a Single Character — Indexing

To pick a single character off a scroll, write the scroll's name followed by the position in square brackets:

```python
greeting = "Hello"
print(greeting[0])           # 'H'
print(greeting[1])           # 'e'
print(greeting[4])           # 'o'
```

Picture: the scroll is laid flat. You point at position `0` and pick up the character there. What you receive is a tiny scroll — a single-character scroll, type `str` like its parent. There is no "character" type in Python distinct from `str`; a single character is simply a scroll of length one.

**Negative positions count from the end.**

```python
print(greeting[-1])          # 'o'   — the last character
print(greeting[-2])          # 'l'   — the second-to-last
print(greeting[-5])          # 'H'   — equivalent to position 0 for this scroll
```

Negative indexing is one of Python's quiet conveniences. Without it, you would write `greeting[len(greeting) - 1]` to get the last character, every time. With it, `greeting[-1]` is enough.

**Reaching past the end raises an alarm.**

```python
print(greeting[5])           # IndexError — there is no position 5
```

The scroll has positions `0` through `4`. Position `5` does not exist. Python triggers the alarm rather than guess. You will meet this kind of intercept properly in Lesson 22.

---

## Cutting a Section Out — Slicing

Often you want not a single character but a section — the first three characters, the last five, everything from position 4 onwards. The operation is called **slicing**, and the syntax is `scroll[start:end]`:

```python
greeting = "Hello, world!"
print(greeting[0:5])         # 'Hello'
print(greeting[7:12])        # 'world'
print(greeting[7:13])        # 'world!'
```

Picture: a scribe places a copy of the scroll on the bench, places markers at positions `start` and `end`, and copies everything between the markers onto a new, shorter scroll. The original is unchanged. The new scroll is what comes back.

**The end position is *exclusive*.** `greeting[0:5]` gives you positions `0`, `1`, `2`, `3`, `4` — five characters total. Position `5` is *not* included. This rule trips up beginners constantly. The way to remember it: `end - start` is exactly the length of the resulting scroll.

**Either marker can be omitted.**

```python
print(greeting[:5])          # 'Hello'   — start defaults to 0
print(greeting[7:])          # 'world!'  — end defaults to len(scroll)
print(greeting[:])           # 'Hello, world!'  — a complete copy
```

If you leave off `start`, the slice begins at position `0`. If you leave off `end`, it goes all the way to the end. If you leave off both, you get a fresh copy of the whole scroll.

**Negative markers work the same way.**

```python
print(greeting[-6:])         # 'world!'
print(greeting[:-7])         # 'Hello,'
```

Negative markers count from the end, just as in indexing. `greeting[-6:]` is "from six characters before the end, to the end".

**A third marker — the step.**

```python
print(greeting[::2])         # 'Hlo ol!'   — every second character
print(greeting[::-1])        # '!dlrow ,olleH'   — reversed
```

The optional third marker is the step. Default is `1` (every character). `2` means every second one. `-1` means walk backwards — which is the canonical way to reverse a scroll in Python.

Slicing is exact, mechanical, and produces a new scroll every time. The original is preserved.

---

## Joining and Repeating — `+` and `*`

Two scrolls can be joined end-to-end with `+`. The result is a new, longer scroll.

```python
first = "Hello"
second = "world"
combined = first + ", " + second + "!"
print(combined)              # 'Hello, world!'
```

Picture three scrolls being laid in sequence on a long bench, then carefully glued together at the seams to produce a single, longer scroll. The originals are unchanged; the new scroll is the result.

`+` only joins two scrolls at a time. To insert a stone into the middle, you must press the stone into the scroll mould first (Lesson 5), or use a fill-in scroll (next section), which is usually cleaner:

```python
score = 27
print("Score: " + score)         # TypeError — stone + scroll cannot be added
print("Score: " + str(score))    # 'Score: 27'   — works after pressing
```

A scroll multiplied by a stone is a repetition.

```python
print("ha" * 3)              # 'hahaha'
print("-" * 20)              # '--------------------'
```

This is occasionally exactly what you want — drawing a separator line, padding a value with spaces, repeating a pattern.

---

## Asking Whether One Scroll Contains Another — `in`

A common question: *does this scroll contain that one?*

```python
greeting = "Hello, world!"
print("world" in greeting)       # True
print("Hello" in greeting)       # True
print("python" in greeting)      # False
print("WORLD" in greeting)       # False  — case-sensitive
```

The `in` operator returns a switch — `True` if the smaller scroll is found anywhere inside the larger, `False` otherwise. Picture the worker scanning the long scroll for the short one and pressing a switch at the end depending on whether it appeared.

This is your everyday tool for checks like *"is this email already in the database scroll?"* or *"does the file path end with `.pdf`?"* (although the second has a more elegant tool, coming up).

---

## Quotes and Multi-Line Scrolls

You have already seen two ways to write a scroll — single quotes (`'...'`) and double quotes (`"..."`). They are interchangeable; pick whichever is convenient and stick to it. The most common reason to choose one over the other is to avoid escaping a quote that appears inside the scroll:

```python
say = 'He said "hello".'
ask = "Don't be late."
```

The first uses single quotes outside so the doubles inside are unambiguous. The second uses doubles outside so the apostrophe is fine.

For longer scrolls — anything that spans multiple lines — Python provides a third form: **triple quotes**. Three single quotes (`'''`) or three double quotes (`"""`), at the start and end:

```python
poem = """The owl and the pussycat went to sea
in a beautiful pea-green boat.
They took some honey, and plenty of money,
wrapped up in a five-pound note."""
print(poem)
```

The line breaks inside the scroll are part of it. When you `print` this scroll, the dispatch worker reads it line by line, with each newline produced by the natural line break in the source.

Triple-quoted scrolls are also the standard form for documentation strings — a kind of label pinned to the front of a workstation describing what it does. We will meet that use formally in Lesson 8.

---

## Escape Sequences

Some characters cannot be written directly into a scroll without confusing Python. A newline, a tab, a quote that matches the scroll's outer quotes, the backslash itself. For these, Python provides a small set of shorthands that begin with a backslash. When the scroll-reader encounters a backslash, it knows to interpret the next character specially.

| Shorthand | Meaning |
|---|---|
| `\n` | A newline — the same character a line break produces |
| `\t` | A tab — used for column-aligned output |
| `\\` | A literal backslash (because the backslash is the escape marker, you have to escape it to mean *itself*) |
| `\"` | A literal double quote (useful inside a `"..."` scroll) |
| `\'` | A literal single quote (useful inside a `'...'` scroll) |

Examples:

```python
print("first\nsecond")           # first
                                  # second

print("name\tscore")             # name    score

print("She said \"hello\".")     # She said "hello".

print("path: C:\\Users\\Shane")  # path: C:\Users\Shane
```

That last one comes up often on Windows file paths and in regular expressions, where backslashes are common. Always remember that `\\` in the source means a single backslash in the actual scroll.

There is also a less-common companion form: prefix the opening quote with `r` to make a **raw scroll**, in which backslashes are taken literally and escape sequences are not recognised:

```python
print(r"path: C:\Users\Shane")   # path: C:\Users\Shane
```

Raw scrolls are popular for regular-expression patterns and Windows paths. You will not need them often as a beginner; recognise the form when you see it.

---

## Building a Scroll From Other Items — f-strings

Almost every program needs to combine fixed text with values from named cubbyholes. *"Player Shane scored 27."* *"The temperature is 23.4 degrees."* *"Welcome, alice@example.com."* You could build these scrolls with `+` and `str()` — but the result is verbose and easy to get wrong:

```python
name = "Shane"
score = 27
clumsy = "Player " + name + " scored " + str(score) + "."
```

Python provides a much cleaner tool. Picture a scroll-maker preparing a scroll with rectangular **windows** cut into it — gaps with a name written above each one. At the moment the scroll is sealed, the scroll-maker walks to each named cubbyhole in the warehouse, reads the current value, and writes that value into the matching window. The finished scroll has no windows visible; the values are simply written in their places.

This is an **f-string** — a *fill-in scroll*. The `f` prefix marks the scroll as a fill-in. Each `{...}` is a window referring to a named cubbyhole or expression:

```python
name = "Shane"
score = 27
clean = f"Player {name} scored {score}."
print(clean)                     # 'Player Shane scored 27.'
```

The window `{name}` is filled with the current contents of the `name` cubbyhole — the scroll `Shane`. The window `{score}` is filled with the contents of the `score` cubbyhole — the stone `27`. (When a non-scroll value goes through a window, Python presses it through the scroll mould first, exactly as it would for `print()`.)

**The window can hold any expression.**

```python
a = 10
b = 3
print(f"{a} + {b} = {a + b}")            # '10 + 3 = 13'
print(f"{a} divided by {b} is {a / b}")  # '10 divided by 3 is 3.333...'
```

The expression inside `{...}` is worked out at the moment the scroll is sealed, and the result is written into the window.

**Format specifiers — controlling how a value is written.**

A useful refinement: after the expression, a colon and a *format specifier* can shape how the value is written into the window. The full grammar is detailed; the patterns you will use most are:

```python
price = 9.5
print(f"{price:.2f}")            # '9.50'   — two decimal places
print(f"{price:.4f}")            # '9.5000' — four decimal places

big = 1234567
print(f"{big:,}")                # '1,234,567'   — thousands separators

pct = 0.834
print(f"{pct:.1%}")              # '83.4%'   — as a percentage
```

`.2f` means "format as a vial-style number with two decimal places". `,` means "use thousands separators". `.1%` means "format as a percentage with one decimal place". You will meet more of these grammar pieces as you need them; for now, recognise the colon as the boundary between *what value* and *how it is written*.

Fill-in scrolls are by far the most common way Python programmers build text. Almost every report, label, log line, or formatted message uses one. Make them your first reach when constructing a scroll from other values.

---

## The Scroll's Built-In Tools

Every scroll carries a small set of built-in tools — operations that read the scroll and produce a new scroll (or sometimes a stone, a switch, or a numbered row). To use a tool, write the scroll, a dot, the tool's name, and parentheses. Some tools take inputs inside the parentheses; others take none.

```python
name = "Shane"
print(name.upper())          # 'SHANE'
```

Read this as *"the `upper` tool of `name`"*. The dot is short for "the scroll's...". The parentheses say "operate now". The tool reads the scroll on the `name` shelf and produces a new scroll with every letter capitalised.

(The full machinery behind these tools — what attaches them to the scroll, how Python knows where to find them — is laid out in Lesson 16, when you meet workshop blueprints. For now, *built-in tools* is the working name.)

Here is a tour of the most useful ones.

**Changing case.**

```python
"Shane".upper()              # 'SHANE'
"SHANE".lower()              # 'shane'
"shane".capitalize()         # 'Shane'      — first character only
"shane hartley".title()      # 'Shane Hartley'   — every word
```

**Trimming whitespace from the ends.**

```python
"   hello   ".strip()        # 'hello'
"   hello   ".lstrip()       # 'hello   '   — left only
"   hello   ".rstrip()       # '   hello'   — right only
```

`strip()` is the everyday tool for cleaning up user input. Whatever the user typed, `strip()` removes any leading and trailing spaces, tabs, or newlines.

**Replacing.**

```python
"banana".replace("a", "o")   # 'bonono'
"hello world".replace(" ", "_")    # 'hello_world'
```

`replace(old, new)` produces a new scroll with every occurrence of `old` swapped for `new`.

**Splitting and joining.**

```python
"alice,bob,charlie".split(",")     # ['alice', 'bob', 'charlie']
"hello world".split()              # ['hello', 'world']   — whitespace by default

", ".join(["alice", "bob", "charlie"])   # 'alice, bob, charlie'
```

`split` cuts a scroll into a numbered row at every separator; the separator is consumed in the cut. `join` is its mirror — it takes a numbered row of scrolls and fuses them into a single scroll, with the original scroll inserted between each pair.

These two are an inseparable pair. Reading a CSV line, parsing a path, building a comma-separated list for output — `split` and `join` cover all of it.

**Asking yes/no questions.**

```python
"hello.txt".startswith("hello")    # True
"hello.txt".endswith(".txt")       # True
"hello.txt".endswith(".pdf")       # False
"hello world".isalpha()            # False  — there's a space
"hello".isalpha()                  # True
"42".isdigit()                     # True
"hello".isdigit()                  # False
```

These tools return switches. `startswith` and `endswith` are the standard tools for checking file extensions and message prefixes. The `is...` family — `isalpha`, `isdigit`, `isalnum`, `isspace`, and others — answer specific questions about the scroll's contents.

**Finding and counting.**

```python
"hello world".find("world")        # 6     — position where it starts
"hello world".find("python")       # -1    — not found
"banana".count("a")                # 3     — how many times it appears
```

`find` returns the position of the first occurrence, or `-1` if the substring is not present. `count` returns a stone with the total number of occurrences.

**This is a small selection.** Scrolls have a few dozen built-in tools in total — `zfill`, `ljust`, `rjust`, `expandtabs`, `swapcase`, and others. The set above covers the vast majority of everyday use. When you need a less common one, the official Python documentation has the full list.

---

## A Scroll Cannot Be Edited

This is the deepest rule about scrolls and the one that distinguishes them most sharply from numbered rows. Once a scroll is written, its contents cannot be changed. You cannot reach in and replace the `H` of `"Hello"` with `J`:

```python
greeting = "Hello"
greeting[0] = "J"            # TypeError — scrolls do not support assignment
```

The scroll-keeper triggers the alarm. Scrolls are sealed; the text is permanent.

What if you really do want a `"Jello"`? You produce a new scroll. Either by slicing and joining, or — much more commonly — by using a tool that does the work for you:

```python
greeting = "Hello"
new_greeting = "J" + greeting[1:]
print(new_greeting)          # 'Jello'

also = greeting.replace("H", "J")
print(also)                  # 'Jello'

print(greeting)              # 'Hello'   — original unchanged
```

Two new scrolls, one original. The original is exactly as it was. Every operation in this lesson — slicing, joining, replacing, the built-in tools — *produces a new scroll*. None of them ever edit the original.

This rule is called **immutability**. The scroll is *immutable*. It is a property the scroll shares with the stone, the vial, the switch, the sealed crate, and the vacant cubbyhole. It is *not* shared by the numbered row, the unsorted bin, or the filing cabinet — those can be modified in place. We will meet the contrast directly when you reach the numbered row in Lesson 7.

For scrolls, the practical consequence is simple: any operation that *seems* to change a scroll is actually producing a new one. If you want the change to stick, you must place the new scroll back on the original shelf:

```python
name = "  shane  "
name = name.strip()          # the original is gone; the trimmed version replaces it
print(name)                  # 'shane'
```

Picture: the scroll-maker delivers a new, trimmed scroll. You take the old one off the `name` shelf, place it on the floor (where the forklift will eventually clear it), and put the new one in its place. The card on the shelf still reads `name`. The contents have been replaced.

---

## What You Now Know

You have looked at the scroll properly for the first time. You can measure it (`len`), pick a single character off it (indexing), cut a section out of it (slicing), join two together (`+`), repeat one (`*`), and ask whether one contains another (`in`). You can write a scroll across multiple lines (triple quotes), include awkward characters via escape sequences, and build a scroll from named cubbyholes using fill-in scrolls (f-strings). You know a useful subset of the scroll's built-in tools — case changes, trimming, replacing, splitting and joining, yes/no checks, finding and counting.

Most importantly, you know the deepest rule: **scrolls cannot be edited**. Every operation produces a new scroll. The original is preserved. To make a change stick, you replace the contents of the cubbyhole — the scroll itself never mutates.

The scroll is the most heavily used item in the factory. A program that processes text — and almost every program does, somewhere — spends most of its time creating, slicing, joining, and inspecting scrolls.

---

## Quick Reference

| Python | Scroll image |
|---|---|
| `len(s)` | Measure the scroll. Returns a stone — the number of characters. |
| `s[i]` | Pick the character at position `i`. Negatives count from the end. |
| `s[a:b]` | Cut a section from `a` (inclusive) to `b` (exclusive). |
| `s[a:b:step]` | The slice with a step; `[::-1]` is the reverse. |
| `s + t` | Glue two scrolls into one new scroll. |
| `s * n` | Repeat the scroll `n` times. |
| `t in s` | Switch — does `s` contain `t`? |
| `'...'`, `"..."` | Single- or double-quoted scroll. Interchangeable. |
| `"""..."""` | Triple-quoted — multi-line scroll. |
| `\n`, `\t`, `\\`, `\"`, `\'` | Escape sequences for newline, tab, backslash, and quotes. |
| `r"..."` | Raw scroll — backslashes taken literally. |
| `f"...{name}..."` | Fill-in scroll. Each `{...}` is a window filled from the warehouse at the moment the scroll is sealed. |
| `f"{x:.2f}"` | Window with a format specifier — `.2f`, `,`, `.1%`, etc. |
| `s.upper()`, `s.lower()` | All caps / all lowercase. |
| `s.strip()` | Remove leading/trailing whitespace. |
| `s.replace(a, b)` | Replace every `a` with `b`. |
| `s.split(sep)` | Cut into a numbered row at every `sep`. |
| `sep.join(row)` | Fuse a numbered row of scrolls with `sep` between each. |
| `s.startswith(p)`, `s.endswith(p)` | Switch — does `s` begin / end with `p`? |
| `s.find(t)` | Position of first occurrence, or `-1`. |
| `s.count(t)` | How many times `t` appears in `s`. |
| `s[0] = "X"` | **TypeError.** Scrolls cannot be edited; produce a new one instead. |

---

## Try It

Open a Python prompt or any Python editor.

**Length, indexing, and slicing:**

```python
s = "Python"
print(len(s))          # 6
print(s[0], s[-1])     # P n
print(s[0:3])          # Pyt
print(s[3:])           # hon
print(s[::-1])         # nohtyP
```

Picture each operation as a physical act on the scroll — a measurement, a pick, a cut, a reversal.

**Joining and repeating:**

```python
print("Hello" + ", " + "world!")
print("ha" * 5)
print("-" * 30)
```

The third line is the standard Python way to draw a separator across a console.

**Membership:**

```python
print("py" in "python")
print("PY" in "python")        # case-sensitive
```

**Multi-line scrolls:**

```python
poem = """Roses are red,
violets are blue,
sugar is sweet,
and so are you."""
print(poem)
```

Notice the natural line breaks in the output — they came from the line breaks in the source.

**Escape sequences:**

```python
print("first\nsecond\nthird")
print("col1\tcol2\tcol3")
print("She said \"hi\".")
print("path: C:\\Users\\Shane")
```

**Fill-in scrolls (f-strings):**

```python
name = "Shane"
score = 27
print(f"Player {name} scored {score}.")

a, b = 10, 3
print(f"{a} divided by {b} is {a / b}.")
print(f"{a} divided by {b} is approximately {a / b:.2f}.")

price = 1234.5
print(f"Total: ${price:,.2f}")
```

That last line — the comma for thousands separators combined with the `.2f` for two decimal places — is the everyday format for money in console output.

**Built-in tools — try a few back to back:**

```python
greeting = "  Hello, World!  "
print(greeting.strip())
print(greeting.strip().lower())
print(greeting.strip().replace("World", "Python"))
print(greeting.strip().split(","))
```

Notice how the tools chain naturally — each one produces a new scroll, which the next tool reads. This *chaining* pattern is common in Python; you will see it everywhere.

**Immutability:**

```python
name = "Hello"
name[0] = "J"          # TypeError — try it and see the alarm
```

Now do the same change the legitimate way:

```python
name = "Hello"
new_name = "J" + name[1:]
print(new_name)        # 'Jello'
print(name)            # 'Hello'   — original unchanged
```

Two scrolls now exist, and the original is untouched.

---

## Where Next?

You have seen the most-used item in the factory in full. The next lesson is the second-most-used: the numbered row, also called a list. Many of the patterns will feel familiar — indexing, slicing, length, `in`. The biggest difference is one you have already met in name: numbered rows *can* be edited. They are not immutable.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 7 | Z3 — Warehouse | Lists — numbered rows in depth |
| Lesson 8 | Z5 — Factory Floor | Functions — the first workstation |
| Lesson 9 | Z3 — Warehouse | Scope — the locked rooms in full |
| Lesson 10 | Z3 — Warehouse | Dictionaries — filing cabinets in depth |

*See the full lesson map in **The Factory — A Complete Map**.*
