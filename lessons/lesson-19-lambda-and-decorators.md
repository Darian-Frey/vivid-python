# Python for Hyperphantasic Minds
## Lesson 19 — Lambda and Decorators

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 19 of 25  
> **Topic**: Lambda and Decorators — impromptu workstations and wrappers  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Last day on the Factory Floor. Lessons 12 through 18 walked you through the floor's permanent fixtures — the tools, junctions, conveyor belts, comprehensions, workshop blueprints, and pause-and-resume workstations. Today's two constructs are conveniences: short-lived workstations that need no name plate, and wrappers that apply automatically before and after every job order to an existing workstation.

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

Both constructs are common enough to recognise on sight, but most beginner Python programs use them sparingly. The point of today is to read them confidently when they appear, and to reach for them when they are the right tool.

---

## Lambda — The Impromptu Workstation

Picture an everyday workstation: a permanent fixture on the Floor, with a name plate, an input slot, and an output belt. You met them in Lesson 8 — `def name(args): body`.

Now picture something humbler: a worker carries onto the Floor a small portable bench, sets it up just where they need it for a single passing job, performs the work in one motion, and either packs the bench away or leaves it behind. No name plate is ever fixed; no permanent floor space is allocated. This is the **impromptu workstation** — Python's `lambda`.

The syntax is one line:

```python
double = lambda x: x * 2
print(double(7))                # 14
```

Read this:

- `lambda` — start of an impromptu workstation.
- `x` — its single input slot.
- `:` — separator.
- `x * 2` — its body, which must be a *single expression*. The result of the expression is automatically returned.

A lambda is a workstation, the same shape `def` produces — input slots, a body, a returned value. The only differences are *form*:

- It has no name (the assignment `double = lambda ...` *gives* it one, but the lambda itself is anonymous).
- It has no `return` keyword; the single expression *is* the returned value.
- It cannot contain statements — no `if` blocks, no `for` loops, no multi-line bodies. One expression only.
- It cannot have a docstring.

A few more shapes:

```python
add = lambda a, b: a + b
print(add(3, 4))                # 7

greet = lambda name="world": f"Hello, {name}"
print(greet())                  # 'Hello, world'
print(greet("Shane"))           # 'Hello, Shane'

is_even = lambda n: n % 2 == 0
print(is_even(4))               # True
```

All four shapes have ordinary `def` equivalents that would do the same job. The lambda form is shorter; the `def` form is more flexible. The choice is almost always about *where the workstation is used*, not what it does.

---

## Where Lambdas Earn Their Keep

The good use of a lambda is **as an argument to another workstation** — most often the `key=` slot of a workstation that needs a custom rule for *how to compare or rank* items.

```python
words = ["banana", "fig", "elderberry", "apple"]

# Sort by length, shortest first
by_length = sorted(words, key=lambda w: len(w))
print(by_length)                # ['fig', 'apple', 'banana', 'elderberry']

# Sort by last character
by_last = sorted(words, key=lambda w: w[-1])
print(by_last)                  # ['banana', 'apple', 'fig', 'elderberry']
```

`sorted` needs to know "what value should I compare these items by?". The `key` slot accepts a workstation that, given one item, returns the value to compare. A lambda fits naturally here — the workstation is tiny, single-purpose, and only used once at the call site.

The same pattern applies to `min`, `max`, and (less commonly) `filter` and `map`:

```python
people = [("Alice", 30), ("Bob", 25), ("Cara", 35)]

oldest = max(people, key=lambda p: p[1])
print(oldest)                   # ('Cara', 35)

youngest = min(people, key=lambda p: p[1])
print(youngest)                 # ('Bob', 25)
```

---

## When NOT to Use a Lambda

A lambda is the right tool when the workstation is *trivial* and *only used once*. It is the *wrong* tool when:

**It needs a name and a docstring.** If the workstation does anything worth explaining, give it a real `def`:

```python
# 🛑 hard to read — what is this lambda doing?
ranking = sorted(records, key=lambda r: (r['priority'], -r['score'], r['name']))

# ✅ same effect, much clearer
def by_rank(r):
    """Sort key: highest priority first, then highest score, then alphabetical."""
    return (r['priority'], -r['score'], r['name'])

ranking = sorted(records, key=by_rank)
```

**It is assigned to a name anyway.** If you find yourself writing `double = lambda x: x * 2`, you wanted a `def double(x): return x * 2`. The `def` form has a name plate, supports a docstring, and is what Python style guides recommend for any function that lives on a shelf:

```python
# 🛑 unusual style
double = lambda x: x * 2

# ✅ standard form
def double(x):
    return x * 2
```

**It needs more than one expression.** A multi-step body cannot fit in a lambda. Use `def`.

A reasonable rule of thumb: **if the lambda needs explaining, it should be a `def`.** Inline lambdas in `key=` slots and similar argument positions are idiomatic and clear. Assigned-to-a-name lambdas and multi-clause lambdas are usually rewritten as `def` by anyone reviewing the code.

---

## Functions Are First-Class — A Step Back

Before we can talk about decorators, the visual you need is this: **a workstation is itself an item the factory can move around**. It can be placed on a warehouse shelf, passed to another workstation as delivered material, returned as a finished product, stored in a numbered row.

```python
def greet(name):
    return f"Hello, {name}"

g = greet                       # the workstation itself, on a new shelf
print(g("Shane"))               # 'Hello, Shane'  — same workstation

workstations = [greet, str.upper, len]
for w in workstations:
    print(w("hello"))           # each one operates on "hello"
```

This is called **first-class functions** — meaning workstations have the same standing as any other item. They can be assigned, passed, returned. Python has had this from the start; you have been using it tacitly every time you handed a function name as an argument (`sorted(..., key=lambda w: len(w))` passes a workstation as material).

This property is what makes decorators possible.

---

## Decorators — The Workstation Wrapper

Picture an ordinary workstation doing its job. Now picture a *layer* fitted around it — a kind of sleeve. Every job order arrives at the sleeve first, the sleeve runs a small bit of work (logging the arrival, perhaps, or starting a timer), passes the order through to the workstation inside, receives the finished product, runs another small bit of work on it (stopping the timer, logging the result), and only then hands the finished product on to the original sender.

The sleeve does not change the workstation itself. The workstation's procedure runs unchanged. What the sleeve adds is a *layer of work that happens before and after* every job order, automatically.

In Python this layer is called a **decorator**, and the canonical verb is **wrap the workstation**.

---

## Building a Decorator Step by Step

The visual is concrete; the syntax is built up in three steps.

**Step 1: A workstation that takes a workstation in and gives a workstation out.**

```python
def announce(func):
    def wrapped(*args, **kwargs):
        print(f"Calling {func.__name__}...")
        result = func(*args, **kwargs)
        print(f"Finished {func.__name__}.")
        return result
    return wrapped
```

Read this carefully:

- `announce` takes one slot: `func`. That slot will receive a workstation.
- Inside, `announce` defines a *new* workstation called `wrapped`. This is the sleeve.
- The sleeve accepts *any* delivered material via `*args` (positional materials) and `**kwargs` (named materials) — Python's variadic forms. Use of these is *required* in a wrapper because the wrapper does not know what slots the inner workstation expects.
- The sleeve runs `func(*args, **kwargs)` — the inner workstation, with whatever materials arrived — and stores its finished product in `result`.
- The sleeve prints messages before and after.
- `announce` finally returns `wrapped` — the sleeve, the new wrapped workstation.

**Step 2: Apply the wrapper by hand.**

```python
def greet(name):
    return f"Hello, {name}"

greet = announce(greet)         # replace greet with its wrapped version

print(greet("Shane"))
# Calling greet...
# Finished greet.
# Hello, Shane
```

The line `greet = announce(greet)` replaces the `greet` shelf with the wrapped version. From now on, every job order to `greet` first runs the sleeve's pre-work, then the original procedure, then the sleeve's post-work.

**Step 3: The `@` syntax.**

Python provides a shorthand for the manual-wrap pattern. Place `@announce` immediately above the `def` line:

```python
@announce
def greet(name):
    return f"Hello, {name}"
```

This is exactly equivalent to writing the unadorned `def greet` and then `greet = announce(greet)` after it. The `@announce` shorthand is the form you will see in real Python code — almost no one writes the manual rebinding.

Read `@announce` as **"wrap this workstation with `announce`"**.

---

## A Few Useful Real Decorators

Decorators are everywhere in Python libraries. A few patterns you will see constantly:

**Timing.**

```python
import time

def timed(func):
    def wrapped(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapped


@timed
def slow_calculation():
    total = 0
    for i in range(1_000_000):
        total += i
    return total

slow_calculation()
# slow_calculation took 0.0XXs
```

The wrapper measures elapsed time around the inner procedure. The original workstation is not modified; only the wrap is.

**Caching (memoisation).**

Python's standard kit provides a caching decorator for free:

```python
from functools import cache

@cache
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(40))            # instant, despite the recursion
```

`@cache` wraps the workstation so that each job order with the same input is computed only once. Repeat calls with the same arguments return the stored finished product directly. The naive recursive `fibonacci` would be unusably slow for large `n`; with `@cache` it is instant.

**Logging.**

A decorator that logs every call's inputs and result:

```python
def logged(func):
    def wrapped(*args, **kwargs):
        print(f"{func.__name__}({args}, {kwargs}) →", end=" ")
        result = func(*args, **kwargs)
        print(result)
        return result
    return wrapped

@logged
def add(a, b):
    return a + b

add(3, 4)
# add((3, 4), {}) → 7
```

Useful for development; rarely left on in production. (Real logging uses the `logging` module, met properly in Advanced Lesson 5b.)

---

## `functools.wraps` — Keep the Original's Identity

When you wrap a workstation, the *outside* now sees the sleeve, not the original. This means the sleeve's `__name__` and `__doc__` are visible, not the wrapped workstation's:

```python
@announce
def greet(name):
    """Say hello."""
    return f"Hello, {name}"

print(greet.__name__)           # 'wrapped'        — !
print(greet.__doc__)            # None
```

This is occasionally a problem — tools that print stack traces or generate documentation see the wrapper's identity instead of the original's. The standard fix is `functools.wraps`, applied to the sleeve inside the decorator:

```python
from functools import wraps

def announce(func):
    @wraps(func)                # copy func's identity onto wrapped
    def wrapped(*args, **kwargs):
        print(f"Calling {func.__name__}...")
        result = func(*args, **kwargs)
        print(f"Finished {func.__name__}.")
        return result
    return wrapped

@announce
def greet(name):
    """Say hello."""
    return f"Hello, {name}"

print(greet.__name__)           # 'greet'           ✅
print(greet.__doc__)            # 'Say hello.'      ✅
```

`@wraps(func)` is a decorator-on-the-decorator that copies `func`'s name, docstring, and a few other identifying scrolls onto the wrapper. It is a small but worthwhile habit when writing decorators that other people will use.

---

## Stacking Decorators

A workstation can wear several sleeves at once. Multiple `@` lines stack from the *innermost wrap* outward:

```python
@announce
@timed
def slow_calculation():
    ...
```

This applies `@timed` first (closest to the `def`), then wraps the result in `@announce`. Reading top-down, the order of *application* is bottom-up — `@timed` first, then `@announce`. The order of *execution per job order* matches the order on the page from the top: `announce`'s pre-work, then `timed`'s pre-work, then the inner procedure, then `timed`'s post-work, then `announce`'s post-work.

Stacking three or more decorators becomes hard to read quickly. For beginner-to-intermediate code, two is typically the practical maximum.

---

## Decorators With Arguments

The decorators above have been plain `@name`. Sometimes you want to configure the decorator at the wrap site — for example, a `repeat(n)` decorator that runs the wrapped workstation `n` times.

The syntax `@repeat(3)` requires `repeat` to be a workstation that *returns* a decorator:

```python
def repeat(times):
    def decorator(func):
        def wrapped(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)
        return wrapped
    return decorator

@repeat(3)
def hello():
    print("Hello!")

hello()
# Hello!
# Hello!
# Hello!
```

Three levels of nested workstations. The outer one (`repeat`) takes the configuration. The middle one (`decorator`) takes the wrapped workstation. The innermost (`wrapped`) is the sleeve. This pattern feels intricate the first few times you write it; it becomes second nature with practice. Most beginner code never writes a parameterised decorator; it appears almost exclusively in libraries.

---

## What You Now Know

You can write a `lambda` as an impromptu workstation for any single-expression job — useful as the `key=` argument to `sorted`, `min`, `max`, and as a function passed into other workstations. You know when *not* to use a lambda: when it warrants a name, a docstring, or more than one expression.

You can write a decorator — a workstation wrapper applied to another workstation with `@name`. You know the three-step build pattern: a decorator workstation takes a function in, returns a wrapped version. The `*args, **kwargs` pattern in the wrapper is required so that any inner signature is supported. Common real decorators include `@timed`, `@cache`, `@logged`, and the convention of using `functools.wraps` to preserve the original's identity. You have seen stacking and parameterised decorators briefly.

This is the last lesson on the Factory Floor. Every construct you will meet from here on is in one of the supporting buildings — the Tool Store, Goods In, Records Department, Quality Control, and the Shift Manager's Office. The Floor itself is now a place you can navigate with confidence.

---

## Quick Reference

**Lambda**

| Python | Image |
|---|---|
| `lambda x: x * 2` | An impromptu workstation. Single input, single expression. |
| `lambda a, b: a + b` | Multiple inputs, separated by commas. |
| `sorted(items, key=lambda x: ...)` | The classic use — a tiny custom rule passed as the `key`. |
| Use a `def` instead when | The function needs a name, a docstring, or more than one expression. |

**Decorators**

| Python | Image |
|---|---|
| `@decorator` `def func(...):` | Wrap `func` with `decorator`. Equivalent to `func = decorator(func)`. |
| `def deco(func): def wrapped(*args, **kwargs): ... return wrapped` | The standard decorator pattern. |
| `*args, **kwargs` | The wrapper accepts any positional and any named materials. |
| `@functools.cache` | Built-in caching decorator from the Tool Store. |
| `@functools.wraps(func)` | Copy `func`'s name and docstring onto the wrapper. |
| Stacking decorators | Read bottom-up for application order; top-down for execution order. |
| `@deco(arg)` | A decorator with arguments — `deco` returns a decorator. |

---

## Try It

**Lambda — the basics:**

```python
double = lambda x: x * 2
print(double(7))

add = lambda a, b: a + b
print(add(10, 5))

is_even = lambda n: n % 2 == 0
print(is_even(4), is_even(5))
```

**Lambda — where it earns its keep:**

```python
words = ["banana", "fig", "elderberry", "apple"]
print(sorted(words, key=lambda w: len(w)))
print(sorted(words, key=lambda w: w[-1]))

people = [("Alice", 30), ("Bob", 25), ("Cara", 35)]
print(max(people, key=lambda p: p[1]))
print(min(people, key=lambda p: p[1]))
```

**Build a decorator step by step:**

```python
def announce(func):
    def wrapped(*args, **kwargs):
        print(f"Calling {func.__name__}...")
        result = func(*args, **kwargs)
        print(f"Finished {func.__name__}.")
        return result
    return wrapped


@announce
def greet(name):
    return f"Hello, {name}"

print(greet("Shane"))
```

**`@cache` for memoisation:**

```python
from functools import cache

@cache
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(50))            # instant
```

Without `@cache`, the same call would take a noticeable time at `n = 30` and become unusably slow well before `n = 50`.

**A timing decorator:**

```python
import time

def timed(func):
    def wrapped(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapped


@timed
def slow():
    total = 0
    for i in range(1_000_000):
        total += i
    return total

slow()
```

**`functools.wraps` — see the difference:**

```python
from functools import wraps

def announce(func):
    @wraps(func)
    def wrapped(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapped

@announce
def greet(name):
    """Say hello."""
    return f"Hello, {name}"

print(greet.__name__)
print(greet.__doc__)
```

Remove the `@wraps(func)` line and re-run — notice how `__name__` and `__doc__` change.

**Stacking:**

```python
def timed(func):
    def wrapped(*args, **kwargs):
        print("timed: start")
        result = func(*args, **kwargs)
        print("timed: end")
        return result
    return wrapped

def announce(func):
    def wrapped(*args, **kwargs):
        print("announce: start")
        result = func(*args, **kwargs)
        print("announce: end")
        return result
    return wrapped


@announce
@timed
def work():
    print("doing work")

work()
# announce: start
# timed: start
# doing work
# timed: end
# announce: end
```

Trace the prints — the outer decorator (announce) wraps around the inner (timed), which wraps around the actual work.

---

## Where Next?

The Factory Floor is now complete. The next four lessons visit the supporting buildings — the Tool Store, Goods In, the Records Department, and Quality Control — finishing with a putting-it-all-together lesson in the Shift Manager's Office.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 20 | Z1 — Tool Store | Modules and Packages — stocking and fetching |
| Lesson 21 | Z2, Z8 | File Handling — crates at the loading bay |
| Lesson 22 | Z7 — Quality Control | Error Handling — the inspection belt |
| Lesson 23 | Z8 — Records Dept | Databases — the permanent archive |

*See the full lesson map in **The Factory — A Complete Map**.*
