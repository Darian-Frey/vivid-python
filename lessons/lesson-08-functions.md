# Python for Hyperphantasic Minds
## Lesson 8 — Functions

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 8 of 25  
> **Topic**: Functions — the first workstation  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You leave the Warehouse for the second time. The first trip — Lesson 5 — was a brief excursion to the Mould Workshop just inside the Factory Floor's entrance. This time you walk past the moulds and out into the Floor proper. The Floor is enormous. Workshops disappear into the distance in every direction. Today you stop at the *kind* of workshop that makes up most of the Floor — the **workstation** — and learn how the factory builds one.

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

This lesson is also the first time you write **indented** Python. In Lesson 2 you saw the floor's lane markings and read the rule that they group lines into one run of work. Here is where the lane markings finally have work to do. A workstation's procedure is one such lane.

Stay alert — the indentation matters.

---

## What a Workstation Is

A **workstation** is a small, dedicated workshop on the Factory Floor that does a specific job. Picture a self-contained room with three external features:

- A **name plate** on the front, in clear lettering. This is how the rest of the factory addresses it.
- A row of **input slots**, each labelled. Materials arrive through the slots when a job is sent.
- An **output belt** that carries the finished product back to whoever sent the job.

Inside the workstation is a fixed procedure — the steps the workstation always carries out when a job arrives. The procedure may use the materials from the input slots, may fetch other items from the Warehouse, may produce intermediate items, and ends by placing a finished product on the output belt.

When the rest of the factory needs the workstation's job done, a worker fills out a **job order** — a small slip naming the workstation and listing the **delivered material** for each slot — and sends it. The workstation receives the order, runs its internal procedure, and sends back the **finished product**. The sender receives the finished product and continues their own work.

This is the entire architecture of every Python function.

---

## Defining a Workstation — `def`

To build a new workstation, you give the factory's welders a description: the name plate, the input slots, and the procedure. Python's keyword for this is `def`.

```python
def greet(name):
    print("Hello,", name)
```

Read this line by line:

- `def greet` — *build a workstation called `greet`*. The welders mount a name plate.
- `(name)` — *with one input slot, labelled `name`*. The slot accepts whatever item is delivered to it.
- `:` — *what follows is the workstation's internal procedure*. The colon marks the start of an indented block.
- `    print("Hello,", name)` — *the procedure*. Indented by four spaces. Whatever sits in the `name` slot is read out at the dispatch bay together with the word `Hello`.

The indented lines form the **lane** of the workstation's procedure. Every line at this indentation level belongs to the workstation. The first non-indented line afterwards is back at the factory level, outside the workstation.

```python
def greet(name):
    print("Hello,", name)
    print("Welcome.")

print("This line is outside the workstation.")
```

Two indented lines belong to `greet`. The third `print` is at the left margin and runs as a separate top-level instruction.

Defining a workstation does not run its procedure. The `def` line tells the welders to *build* the workstation; running the procedure happens later, when a job order arrives.

---

## Sending a Job Order — Calling

To run the workstation's procedure, send it a job order. The syntax is the workstation's name, followed by the materials to deliver, in parentheses:

```python
def greet(name):
    print("Hello,", name)

greet("Shane")
greet("Alice")
```

Two job orders go out. Each one delivers a different scroll to the `name` slot. Each one runs the procedure once. The output is:

```
Hello, Shane
Hello, Alice
```

You have already been sending job orders since Lesson 2 — `print("hello")` is a job order to the built-in `print` workstation, and `len("hello")` is a job order to `len`. Until now you have only used built-in workstations. Today you build your own.

The canonical verb for sending a job order is **send a job order to** the named workstation. It is interchangeable with the more conversational *call*, which is the term most Python programmers use day to day.

---

## Input Slots and Delivered Material — Parameters vs Arguments

Two related but distinct ideas live at the front of every workstation:

- An **input slot** is a labelled receptacle on the workstation's front, defined when the workstation is built. The labels are listed in the parentheses of the `def` line.
- **Delivered material** is the actual item placed into a slot when a job order arrives. The materials are listed in the parentheses of the call.

```python
def greet(name):              # name is an input slot
    print("Hello,", name)

greet("Shane")                # "Shane" is the delivered material
```

The slot label `name` exists for the workstation's own internal procedure. It is how the inside of the workstation refers to whatever item arrived through that slot. The delivered material is the actual item — the scroll `"Shane"` in this case — that landed in the slot when the job order arrived.

Picture the workstation receiving the order: the scroll `"Shane"` lands in the `name` slot, and during the procedure, every reference to `name` reads the scroll out of that slot.

Different deliveries produce different runs of the same procedure:

```python
greet("Shane")     # name slot holds "Shane" — prints "Hello, Shane"
greet("Alice")     # name slot holds "Alice" — prints "Hello, Alice"
```

Same workstation, same procedure, three different jobs. This is the entire point of a workstation: do one task once, run it many times with different inputs.

The terms **parameter** and **argument** are the formal Python words for input slot and delivered material. If you read other Python documentation you will see them constantly. They are useful labels but the physical picture — slots and what arrives in them — is the deeper one.

---

## The Finished Product — `return`

Most workstations do more than print things. They produce a result that the rest of the factory can use. To send a result back to whoever sent the job order, use the `return` keyword.

```python
def add(a, b):
    return a + b
```

Now picture the workstation `add`:

- Two slots, `a` and `b`, on its front.
- A small internal procedure: take what's in the slots, calculate their sum, place the result on the output belt.
- The output belt carries the finished product back to whoever sent the job.

When you send a job order, the result comes back as the value of the call expression:

```python
result = add(3, 4)
print(result)              # 7
```

Read the second line carefully. `add(3, 4)` is sent as a job order. The workstation runs its procedure, places the stone `7` on the output belt, and sends it back. That stone is what the expression `add(3, 4)` *evaluates to*. Then the assignment places that stone in the `result` cubbyhole.

A workstation can have many lines before its `return`:

```python
def discount_price(price, percent):
    discount = price * percent / 100
    final = price - discount
    return final

print(discount_price(100, 25))     # 75.0
```

The `discount` and `final` cubbyholes are part of the workstation's own internal scratch space — its **locked room**, in the warehouse vocabulary you saw glimpsed in Lesson 1. Lesson 9 explores locked rooms in full. For now, the visual is enough: each call to a workstation has its own private workspace, and that workspace vanishes when the procedure ends.

`return` does two things at once. It places the finished product on the output belt, and it ends the workstation's procedure immediately. Any lines after a `return` (in the same lane) are never reached:

```python
def first_positive(numbers):
    for n in numbers:
        if n > 0:
            return n         # ends the moment the first positive is found
    return None              # only reached if none were positive
```

(That code uses `for` and `if`, which arrive properly in Lessons 13 and 14. Read it as a single small procedure that walks through a numbered row and stops at the first positive value. The detail is for later.)

---

## A Workstation With No `return`

A workstation does not have to send anything back. The first one you saw — `greet` — had no `return`, and that was fine.

When a workstation's procedure ends without a `return`, Python sends back a **vacant cubbyhole** (`None`) automatically. Picture the output belt rolling out of the workstation with nothing on it.

```python
def greet(name):
    print("Hello,", name)

result = greet("Shane")    # prints "Hello, Shane"
print(result)              # None
```

A workstation that exists for its side effect (printing, writing to a file, updating a row in place) typically returns nothing. A workstation that exists to compute a value typically returns it explicitly. Both patterns are common.

A common error you will see early: forgetting to return a result, then wondering why the calling code keeps getting `None`:

```python
def add(a, b):
    a + b                  # the sum is calculated but never sent back

result = add(3, 4)
print(result)              # None — !
```

The fix is always the same: add `return`.

---

## Default Slot Contents

A slot can come pre-loaded with a default item. If the job order does not deliver anything to that slot, the default is used.

```python
def greet(name, greeting="Hello"):
    print(greeting + ",", name)

greet("Shane")                          # Hello, Shane
greet("Shane", "Welcome back")          # Welcome back, Shane
```

Picture a small drawer attached below the `greeting` slot, holding a pre-filled scroll reading `"Hello"`. When a job order does not deliver anything to the `greeting` slot, the worker reaches into the drawer and uses the default scroll. When the job order *does* deliver, the delivered material is used and the drawer is left alone.

Default-loaded slots must come *after* slots without defaults. Python will refuse to build a workstation with the slots in the wrong order:

```python
def bad(name="Shane", age):     # SyntaxError — required slot follows defaulted slot
    pass
```

The reason: when a job order arrives, materials are placed in slots from left to right. If a default slot came first and you only delivered one material, Python would not know whether the material was meant for the default slot or the required slot.

**One trap that catches every Python programmer eventually.** Never use a *mutable* item as a default — a numbered row, a filing cabinet, an unsorted bin. The default item is created once, when the workstation is built, and shared between every job order that uses the default:

```python
def bad_pattern(item, basket=[]):    # ⚠ this row is shared between calls
    basket.append(item)
    return basket

print(bad_pattern("apple"))        # ['apple']
print(bad_pattern("banana"))       # ['apple', 'banana']  — !
```

The expected `['banana']` did not happen because every call without a delivery to `basket` reuses *the same* drawer-loaded row. The fix: use `None` as the default and create a fresh row inside the procedure when needed:

```python
def good_pattern(item, basket=None):
    if basket is None:
        basket = []
    basket.append(item)
    return basket
```

You will see this pattern in real Python code constantly. Recognise it, and use it whenever a default value should be a new mutable item per call.

---

## Naming the Slots — Keyword Arguments

When sending a job order, you can label the materials with their slot names instead of relying on order:

```python
def greet(name, greeting="Hello"):
    print(greeting + ",", name)

greet(name="Shane")                      # Hello, Shane
greet(name="Shane", greeting="Hi")       # Hi, Shane
greet(greeting="Hi", name="Shane")       # Hi, Shane — order does not matter when names are given
```

Materials with names attached are called **keyword arguments** in formal Python. The image: each piece of delivered material is taped with a label naming which slot it should go into. The job order routes them automatically, in any order you wrote them.

Two practical reasons to use keyword arguments:

- **Skip default slots.** If a workstation has many default-loaded slots, you can override only the ones you care about by name.
- **Readability.** When the workstation has more than two slots, named materials make the call easier to read at the call site. Compare:

```python
draw_rectangle(0, 0, 200, 100, "red", True)              # what is the True?
draw_rectangle(x=0, y=0, width=200, height=100,
               colour="red", filled=True)                # crystal-clear
```

Most Python programmers default to keyword arguments for any call with three or more arguments.

---

## Sending Back More Than One Thing

Sometimes a workstation needs to send back two or three results at once. Python handles this by packing the results into a sealed crate (a tuple), which the receiver can unpack:

```python
def stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

low, high, average = stats([3, 1, 4, 1, 5, 9, 2, 6])
print(low, high, average)              # 1 9 3.875
```

Read `return min(numbers), max(numbers), sum(numbers) / len(numbers)` as: place all three items on the output belt, packed into a single sealed crate. The receiver unpacks the crate at the moment of assignment — `low, high, average = ...` is the parallel placement pattern you saw in Lesson 1, applied to a sealed crate.

If you do not unpack at the call site, the finished product is the sealed crate itself:

```python
result = stats([3, 1, 4])
print(result)                          # (1, 4, 2.6666...)
print(type(result))                    # <class 'tuple'>
```

This is technically a single returned item — there is no "multiple return" in Python, only "a return of a sealed crate". But the syntactic convenience makes it feel like multiple values, and that is how most Python programmers think of it.

---

## The Workstation's Instruction Sheet — Docstrings

Pinned to the inside front wall of every well-built workstation is a printed instruction sheet — a plain-language description of what the workstation does, what each slot expects, and what the finished product is. In Python, this is a triple-quoted scroll placed as the very first statement inside the workstation's procedure:

```python
def add(a, b):
    """Return the sum of two numbers."""
    return a + b

def discount_price(price, percent):
    """Return the price after applying a percentage discount.

    price: the original price (a stone or vial).
    percent: the discount percentage (0 to 100).

    The result is always a vial.
    """
    return price - (price * percent / 100)
```

These triple-quoted scrolls are called **docstrings**. They are not comments — they are real scrolls, attached to the workstation, that any reader (and any tool) can fetch:

```python
print(add.__doc__)              # 'Return the sum of two numbers.'
print(help(add))                # the human-readable form
```

The dot syntax `add.__doc__` reads as *the `__doc__` of `add`* — fetching a scroll attached to the workstation. The full machinery comes in Lesson 16. For now, recognise the form and the convention: every workstation worth keeping has a docstring.

A short single-line docstring is enough for a simple workstation. Longer multi-line docstrings explain the slots and the finished product in detail, often using a standard format. There is no penalty for being thorough; future you will thank you.

---

## A Sneak Preview of Locked Rooms

Each running job inside a workstation has its own private workspace — a **locked room** at the back of the warehouse, accessible only to that running job. Cubbyholes inside the locked room belong to the job; the moment the procedure ends, the room is cleared completely, and its shelves never existed as far as the rest of the factory is concerned.

You saw this in Lesson 1, briefly. Now that workstations are real, the picture is concrete:

```python
def double(x):
    result = x * 2          # 'result' lives in this job's locked room
    return result

print(double(5))            # 10
# print(result)             # NameError — there is no 'result' shelf in the warehouse
```

The `result` cubbyhole inside `double` exists only while the procedure is running. When `double` finishes, the room is cleared, and the only thing that survived is the finished product placed on the output belt.

Lesson 9 covers locked rooms in full — when shelves are visible from where, what *open aisle* shelves are, and how nested workstations behave. Today, the takeaway is simple: **a workstation has its own scratch space, separate from the main warehouse, and that space disappears the moment the workstation is done.**

---

## What You Now Know

You have built and operated workstations. The vocabulary is now in place: a **workstation** has **input slots** that receive **delivered material** when a **job order** arrives, runs an internal procedure, and sends back a **finished product** through `return`. A workstation with no `return` sends back a vacant cubbyhole. Slots can have default contents that fill in when no material is delivered; never make those defaults mutable. Materials can be labelled with slot names, which lets you reorder the delivery and skip defaults you do not care about. A workstation can return many things at once by packing them into a sealed crate.

You also know that a workstation has its own private workspace — its locked room — which is the entire subject of Lesson 9.

This is also the first lesson where you wrote indented Python. The lane markings of Lesson 2 are no longer a forward pointer; they are part of how every workstation is defined.

The factory is now live. From this point forward, almost every program you write will be a small set of workstations being sent job orders by a top-level procedure — *the work order* read by the Shift Manager (Lesson 25). Everything else in the series builds on top of this.

---

## Quick Reference

| Python | Workstation image |
|---|---|
| `def name(slot1, slot2):` | Build a workstation called `name` with two input slots. |
| `    body` | The indented lines are the workstation's internal procedure (its lane). |
| `name(value1, value2)` | Send a job order; deliver `value1` to slot1, `value2` to slot2. |
| `return value` | Place the finished product on the output belt; end the procedure. |
| (no `return`) | The procedure ends; finished product is `None`. |
| `def name(a, b=10):` | Slot `b` has default content `10`; used if nothing is delivered. |
| `name(b=99)` | Keyword delivery — material `99` is taped to slot `b`. |
| `return a, b, c` | Send back three items packed into a sealed crate. |
| `x, y, z = name(...)` | Unpack the sealed crate at the call site. |
| `"""docstring"""` | First scroll inside the procedure; a printed instruction sheet, attached to the workstation. |
| Mutable default trap | `def f(x, basket=[])` — *never*. Use `basket=None` and build a fresh row inside. |
| Locked room | A cubbyhole created inside a workstation lives only for the duration of one job. (Lesson 9 in full.) |

---

## Try It

Open a Python prompt or any Python editor.

**The simplest workstation:**

```python
def greet(name):
    print("Hello,", name)

greet("Shane")
greet("Alice")
```

Two job orders, two runs of the same procedure, two different deliveries.

**A workstation that returns:**

```python
def add(a, b):
    return a + b

print(add(3, 4))
total = add(10, 20)
print(total)
print(add(add(1, 2), add(3, 4)))   # 10 — workstations can be nested in a single line
```

That last line: two job orders to `add` produce stones `3` and `7`, which become the deliveries to a third job order to `add`, which produces `10`.

**Default slot contents:**

```python
def greet(name, greeting="Hello"):
    print(greeting + ",", name)

greet("Shane")
greet("Shane", "Welcome")
greet("Shane", greeting="Hi")
```

**Keyword arguments — order does not matter:**

```python
def describe(name, age, city):
    print(name, "is", age, "and lives in", city)

describe("Shane", 27, "London")
describe(city="London", name="Shane", age=27)        # any order
describe("Shane", city="London", age=27)             # mix is fine
```

The only rule about mixing: positional materials must come first, named ones after.

**Returning many things:**

```python
def stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

low, high, average = stats([3, 1, 4, 1, 5, 9, 2, 6])
print(low, high, average)
```

**A workstation with a docstring:**

```python
def discount_price(price, percent):
    """Return the price after applying a percentage discount."""
    return price - (price * percent / 100)

print(discount_price(100, 25))
print(discount_price.__doc__)
help(discount_price)
```

`help(name)` is a built-in workstation that fetches and prints the docstring in a friendly format. It is invaluable when you want to remind yourself how a workstation works without leaving the Python prompt.

**The mutable-default trap, deliberately:**

```python
def bad(item, basket=[]):
    basket.append(item)
    return basket

print(bad("apple"))
print(bad("banana"))
print(bad("cherry"))
```

The growing row is the same row across all three calls. Now fix it:

```python
def good(item, basket=None):
    if basket is None:
        basket = []
    basket.append(item)
    return basket

print(good("apple"))
print(good("banana"))
print(good("cherry"))
```

A new row per call. This is the pattern you will reach for whenever a default needs to be a fresh mutable item.

---

## Where Next?

You have stayed on the Factory Floor only as long as you needed to. The next lesson takes you back into the Warehouse to look at the **locked rooms** at the back — the workspaces every running workstation owns and clears at the end of its job. Locked rooms only make sense once you have workstations to attach them to, which is why Lesson 9 follows directly from this one.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 9 | Z3 — Warehouse | Scope — the locked rooms in full |
| Lesson 10 | Z3 — Warehouse | Dictionaries — filing cabinets in depth |
| Lesson 11 | Z3 — Warehouse | Tuples and Sets — sealed crates and unsorted bins |
| Lesson 12 | Z5 — Factory Floor | Operators — the tools on the floor |

*See the full lesson map in **The Factory — A Complete Map**.*
