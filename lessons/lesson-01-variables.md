# Python for Hyperphantasic Minds
## Lesson 1 — Variables & Memory

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 1 of 25  
> **Topic**: Variables & Memory  
> **Factory zone**: Z3 — The Warehouse  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Every Python program is a factory. Before this lesson, read **The Factory — A Complete Map** to see the full complex. Today you are entering one specific building within it.

Here is the factory from above. Your destination is highlighted:

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

The Warehouse sits at the heart of the complex. Everything that arrives through Goods In gets stored here. Everything the Factory Floor produces gets stored here. Nothing moves through this factory without passing through the Warehouse first.

This is the right place to start.

---

## The Warehouse

You pass through a set of wide double doors and step inside.

The floor stretches away in pale grey concrete, faintly reflective under long rows of fluorescent lights that run to the far wall. The ceiling is high — high enough that the topmost shelves disappear into a soft haze of light. The shelving units stand in precise rows, each one subdivided into thousands of small cubbyholes, identical in size, stretching off into the distance.

It is quiet in the way a well-run warehouse is quiet. Order without stillness. Things are in their places, but the building is alive — somewhere, a name card is being pinned; somewhere, a stone is being lifted out; somewhere, a forklift is preparing to clear an aisle.

This building is your computer's **memory**. Every value your program creates, receives, or calculates will be stored somewhere in here — labelled, filed, and ready to be retrieved.

---

## The Shelves

Row upon row of shelving, each shelf subdivided into cubbyholes. Each cubbyhole holds exactly one item — never more, never less.

Every cubbyhole has a number stencilled on it in black paint: **0, 1, 2, 3, 4...** running into the millions. These numbers are **the warehouse's postal numbers** — the machine uses them internally to find things. You, as a programmer, almost never will. Postal numbers are how the warehouse files its own paperwork; they are not how *you* will work in here.

Instead, you are going to give your cubbyholes *names*.

---

## Your First Named Cubbyhole

Picture a small white card with the word **`age`** written on it in clear black ink.

You walk to an empty cubbyhole — postal number 4096, though you won't need to remember that — and pin your card to the front of it. The cubbyhole now has two identities: its postal number (which the machine uses) and its name (which you use).

Now picture a small stone with the number **`27`** painted on it in white.

You place it in the cubbyhole.

That's it. That's a **named cubbyhole** — what programmers call a *variable*. In Python:

```python
age = 27
```

You *named* a location in memory. You *placed* a value inside it. The name `age` now points — like a finger extended toward that specific shelf — at that exact spot. Whenever your program needs to know the value of `age`, it walks to that shelf, reads the stone, and finds **27**.

Notice what the `=` sign does here. It is not a statement of equality, the way it is in mathematics. It is a *placement instruction*. Read left to right: **take the value on the right, and place it in the named cubbyhole on the left.** The arrow goes one way only.

---

## Replacing What's Inside

Picture the `27` stone being lifted out of the cubbyhole and set on the floor. A new stone — this one marked **`28`** — is placed inside.

The card on the front still reads `age`. The postal number is still 4096. Only the stone inside has changed. The named cubbyhole has been **replaced**. In Python:

```python
age = 28
```

What did *not* change: the location, the name, the postal number. A named cubbyhole is not the value — it is the *named place that holds the value*. The shelf exists independently of whatever is currently sitting on it.

This distinction trips up beginners constantly. They start to think of `age` as "the number 28". It is not. It is the *shelf labelled `age`, which currently happens to hold a stone reading 28*. Tomorrow it might hold a different stone. The shelf endures; the contents come and go.

---

## Two Named Cubbyholes Side by Side

Walk a little further down the aisle. Pin a second card — **`score`** — to another cubbyhole. Place a stone marked **`0`** inside it.

Now picture this sequence as a short film: you look at the `age` shelf (stone reads **`28`**), produce a new stone also marked **`28`**, and place it into the `score` cubbyhole.

```python
score = age
```

Both cubbyholes now hold stones marked `28`. They are equal in value — but they are not the same stone. See them side by side: two shelves, two stones, the same number painted on each. If you replace one, the other is unaffected. This is one of the most important visual anchors in the whole warehouse: *separate shelves, separate stones*.

```python
age = 100
# score is still 28 — the score stone never changed
```

The `score` shelf was never connected to the `age` shelf. The act of `score = age` was a one-time read-and-place. After that moment, the two shelves live their own lives.

---

## Many Cubbyholes at Once

The warehouse will sometimes accept several placement instructions on a single line. Two patterns are worth seeing now.

**The same value into several cubbyholes:**

```python
x = y = z = 0
```

Picture a worker producing three identical stones, each marked `0`, and placing one in each of three cubbyholes — `x`, `y`, `z` — in a single sweep. Three separate shelves, three separate stones, all reading the same value.

**Different values into several cubbyholes, in parallel:**

```python
a, b, c = 1, 2, 3
```

Three cards are pinned simultaneously — `a`, `b`, `c` — and three different stones (`1`, `2`, `3`) are placed in the matching cubbyholes in the same order. The first stone goes into the first shelf, the second into the second, and so on.

These are conveniences, not new ideas. Underneath, both patterns are doing exactly what `age = 27` did — naming a location and placing a value inside it. They simply do it for several shelves at once.

---

## The Shape of the Cubbyhole — Python's Secret

Look at the shelves again. Now notice something: **the cubbyholes have no fixed shape**. There is no section for stones on one side and scrolls on the other. Python's cubbyholes are universal — any one of them can hold anything.

You don't decide what kind of thing goes on a shelf when you pin up your name card. You simply place something there, and Python looks at what is currently inside to figure out what it is.

```python
age = 27              # a stone — a whole number (Python calls this an int)
age = "twenty-seven"  # now a scroll — a string of characters (Python calls this a str)
```

Both are perfectly legal. The same shelf. The card still reads `age`. First a stone, then a scroll. Python looks at whatever is currently inside and adapts.

Picture the cubbyhole quietly reshaping itself around each new arrival. This is called **dynamic typing** — the *type* of a named cubbyhole is determined by whatever is currently sitting inside it, not by the shelf itself.

This differs from languages like C++ or Java, where you must declare the shape of the cubbyhole before you put anything in it. There, the shelf says "stones only" or "scrolls only", and a mismatched item is rejected at the door. In Python, the shelf is open. Each thing placed inside carries its own identity with it.

However — just because the shelf is flexible doesn't mean types don't matter. The stone knows it is a stone. The scroll knows it is a scroll. If you try to add a stone and a scroll together, Python will stop and tell you those things cannot be combined. Imagine trying to do arithmetic on the word "twenty-seven" — the image tells you immediately why it fails.

```python
age = 27
greeting = "I am "
print(greeting + age)        # Python will stop — stone + scroll cannot be added
print(greeting + str(age))   # This works — the stone has been pressed into a scroll
```

That last line — `str(age)` — is pressing the `27` stone into a mould that produces a scroll reading `"27"`. The original stone is unchanged; you now have a scroll version alongside it. We'll explore this reshaping — called **casting** — fully in Lesson 5.

---

## The Warehouse Has Locked Rooms

Before you leave this building, notice something at the back. Not all of this warehouse is open aisle.

Deep inside, behind doors that require a keycard, are locked rooms. A locked room belongs to a specific workstation on the Factory Floor. The shelves inside it are visible only to that workstation — nothing else in the factory can reach them. The moment the workstation finishes its job, the locked room is cleared completely. Its shelves never existed, as far as the rest of the warehouse is concerned.

The open aisles hold shelves that any part of the factory can reach. The locked rooms hold shelves that belong to one workstation and disappear when it finishes. This system is called **scope** — you will visit it fully in Lesson 9, but picture it now so the building feels complete.

---

## Naming Your Shelves Well

You can pin any name you like onto a cubbyhole, with a few rules. In Python, named cubbyholes:

- Can contain letters, numbers, and underscores
- **Cannot** start with a number
- **Cannot** contain spaces (use `_` instead)
- **Cannot** be one of Python's reserved words (`if`, `for`, `class`, `return`, `def`, `while`, and a handful of others — the factory uses these for its own job-order instructions)
- **Are case-sensitive** — `Age`, `age`, and `AGE` are three different cubbyholes

```python
age = 27               # valid
first_name = "Shane"   # valid — underscore instead of a space
_count = 0             # valid — underscores are allowed at the start
2fast = True           # INVALID — starts with a number
my age = 27            # INVALID — contains a space
class = "year 9"       # INVALID — class is a reserved word
```

Picture each invalid name as a card the warehouse's labelling system refuses to accept — it slides off the shelf every time. The rules exist so Python can always tell a cubbyhole name apart from a number or an instruction at a glance.

### The Warehouse's House Style

Beyond the strict rules, Python has a *house style* for naming. These are conventions, not laws — Python won't reject a card written in another style — but reading code becomes much easier when everyone labels their shelves the same way.

- **`snake_case`** — lowercase words joined by underscores. This is the Pythonic style for ordinary named cubbyholes: `first_name`, `player_score`, `total_count`.
- **`UPPER_CASE`** — all capitals, words joined by underscores. This is a *signal* to the reader: "this shelf holds a value that should not be replaced". Python will not actually stop you from replacing it — there is no padlock — but the all-caps card is a sign every Python programmer recognises: **constant, leave alone**. Examples: `MAX_PLAYERS`, `PI`, `DEFAULT_TIMEOUT`.
- **`camelCase`** and **`PascalCase`** — common in other languages, occasionally seen in Python. PascalCase is reserved for workshop blueprints (classes), which we will meet in Lesson 16. For ordinary shelves, prefer `snake_case`.

The most important rule is one Python cannot enforce: **choose names that mean something**. A cubbyhole called `x` is an unlabelled shelf in a vast warehouse. A cubbyhole called `player_score` is immediately readable from the aisle. You are labelling your own warehouse — future you will need to navigate it.

---

## Taking a Card Down

Sometimes you want to undo a name — take a card off a shelf entirely. Python provides one keyword for this: `del`.

```python
age = 27
del age
```

Picture the `age` card being lifted off the cubbyhole. The shelf is anonymous again. If you now ask Python for `age`, it will report that there is no such shelf — the name has been forgotten.

You will not need `del` often in beginner Python. The forklift (described in the next section) handles most clearing automatically. But it is worth knowing the operation exists, and worth picturing what it does to the shelf.

---

## What Happens When the Shift Ends

When your program finishes running, picture a forklift moving silently through the warehouse. It clears every shelf your program was using — stones lifted out, scrolls removed, name cards taken down. The cubbyholes return to their blank, numbered state, ready for the next program.

Memory is borrowed, not owned. What your program calls `age` is a temporary relationship between a name and a location. When the shift ends, the name dissolves. The shelf remains, anonymous and waiting.

The forklift also makes smaller passes during the program itself. Whenever the last name pointing to a particular stone is taken down — or replaced with something else — the forklift quietly comes through and clears that stone away. Nothing in the warehouse outlives the names that point to it.

If data must survive after the program closes, it must be sent to the **Records Department** before the shift ends — written to a file or a database. The warehouse does not keep its contents overnight. The Records Department does.

---

## What You Now Know

You have walked through the Warehouse. You have seen the rows of cubbyholes, watched name cards being pinned, seen stones and scrolls being placed and replaced. You have seen that Python's shelves accept anything — but that whatever sits on the shelf still knows what it is. You have seen several cubbyholes named in a single sweep. You have learned the warehouse's house style for labelling. You have glimpsed the locked rooms at the back, and watched the forklift make its quiet rounds.

That visual model is not a shortcut to understanding variables. It *is* the understanding. The notation (`age = 27`) is just the shorthand programmers write so they don't have to describe the warehouse every time.

When you see a named cubbyhole in code, picture the shelf. When you see `=`, picture the stone being placed. When Python stops you from combining two things, picture someone trying to do arithmetic on a scroll — the image tells you immediately why it fails.

The warehouse is always there. You just learned to walk around in it.

---

## Quick Reference

| Python | Warehouse image |
|--------|----------------|
| `age = 27` | Pin a card saying `age` to a shelf. Place a stone marked `27` inside. |
| `age = 28` | Remove the `27` stone. Replace it with a `28` stone. Card unchanged. |
| `score = age` | Read the `age` stone. Produce an identical stone. Place it in the `score` shelf. |
| `age = "twenty-seven"` | Remove the stone. Replace it with a scroll. Python adapts. |
| `x = y = z = 0` | Three cubbyholes, three identical stones, placed in one sweep. |
| `a, b, c = 1, 2, 3` | Three cubbyholes, three different stones, placed in parallel. |
| `del age` | Take the `age` card off the shelf. The cubbyhole becomes anonymous. |
| `str(age)` | Press the `age` stone into a mould; receive a scroll version. (Lesson 5.) |
| Naming rules | Cards must start with a letter or `_`, contain no spaces, avoid reserved words; case-sensitive. |
| `snake_case` | The Pythonic house style for ordinary named cubbyholes. |
| `UPPER_CASE` | A signal to readers: "this shelf is a constant — do not replace". |
| Shift ends | Forklift clears all shelves. Records Dept is the only permanent storage. |
| Open aisle shelf | A shelf any part of the factory can reach. (Lesson 9.) |
| Locked room shelf | A shelf belonging to one workstation; cleared the moment it finishes. (Lesson 9.) |

---

## Try It

Open a Python prompt or any Python editor and type these lines one at a time. After each one, type the cubbyhole name and press Enter to see what is currently on that shelf:

```python
age = 27
age
age = 28
age
name = "your name here"
name
score = age
score
age = 100
score   # Does score change? Why not?
```

Pause on that last one. Picture the two shelves. Picture the two separate stones.

Now try the multi-placement patterns:

```python
x = y = z = 0
x
y
z
a, b, c = 1, 2, 3
a
b
c
```

Picture each line as a single sweep across multiple shelves.

Finally, picture a card coming down:

```python
greeting = "hello"
greeting
del greeting
greeting   # Python will stop — there is no shelf with that name
```

If the last line surprises you, hold the image: the card was taken off, the shelf became anonymous, and asking for a name that isn't on any shelf is asking for nothing.

---

## Where Next?

You are still inside the Warehouse. The next lessons continue here — there is more to learn about what kinds of things can sit on these shelves before moving to other parts of the complex.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 2 | Z11 — Outgoings | Syntax & Output — calling out across the floor |
| Lesson 3 | Z3 — Warehouse | Data Types — the full range of objects on the shelves |
| Lesson 4 | Z3 — Warehouse | Numbers — stones and vials in depth |
| Lesson 5 | Z3 → Z5 | Casting — pressing objects into moulds |

*See the full lesson map in **The Factory — A Complete Map**.*
