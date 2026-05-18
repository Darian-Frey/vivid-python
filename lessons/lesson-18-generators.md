# Python for Hyperphantasic Minds
## Lesson 18 — Iterators and Generators

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 18 of 25  
> **Topic**: Iterators and Generators — passing one item and waiting  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still on the Factory Floor. Every workstation you have built so far runs its procedure to completion, sends back its finished product, and shuts down. Today's construct is different: a workstation that **passes one item and waits**. It runs until it has one item ready, hands that item over, and freezes in place. When pumped again, it resumes exactly where it left off, runs until the next item is ready, hands it over, and freezes again. This continues until the procedure runs out of items.

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

You met this kind of workstation briefly in Lesson 15 — the round-bracket form `(expr for x in coll)` is a generator expression. Today it gets its full machinery.

---

## The Motivation — Building vs Streaming

Suppose you want to process the first ten squared natural numbers. The familiar way is a list comprehension (Lesson 15):

```python
squares = [n * n for n in range(1, 11)]
for s in squares:
    print(s)
```

A wheeled frame is built holding all ten stones, then walked. For ten items this is fine. For ten million items, the wheeled frame would have to hold all ten million stones at once — in the warehouse, in memory.

Now consider a different shape: instead of a workstation that builds the whole row in advance, picture a workstation that *produces one stone at a time*, on demand. The first request makes it run a few internal steps and hand over a stone. The second request makes it run a few more steps and hand over the next stone. No intermediate row exists. The warehouse only ever holds one stone of output at a time.

This is the **generator workstation**, and the keyword that builds one is `yield`.

---

## The Pause-and-Resume Workstation

A regular workstation runs its procedure once, returns one finished product, and ends. A generator workstation is the same shape with one critical change: it can pause partway through, hand over an interim result, and resume when asked.

```python
def count_up_to(n):
    i = 1
    while i <= n:
        yield i
        i += 1
```

The blueprint looks almost like an ordinary workstation. The crucial detail is `yield`. The presence of `yield` anywhere inside the procedure turns this into a generator workstation.

What happens when you "call" a generator workstation is also different. Sending a job order to it does **not** run the procedure:

```python
gen = count_up_to(3)
print(gen)             # <generator object count_up_to at 0x...>
```

The result is a **generator workshop** — a frozen workstation, ready to run. Nothing inside has executed yet. The `i` cubbyhole has not been placed; the loop has not started.

To get the procedure to run, you must **pump** the generator. The pump is the built-in workstation `next()`:

```python
print(next(gen))       # 1
```

Here is what happens internally:

1. `next(gen)` is sent.
2. The frozen workstation runs its procedure for the first time. The `i` cubbyhole is placed with the value `1`. The loop condition `i <= n` is checked (True). Control reaches `yield i`.
3. `yield i` hands the stone `1` back to whoever pumped, then freezes the workstation in place — *with `i` still on its shelf, with the loop in mid-pass*.

A second pump:

```python
print(next(gen))       # 2
```

1. `next(gen)` is sent.
2. The workstation resumes from exactly where it paused: just after the previous `yield`. The next line is `i += 1` — `i` becomes `2`. The loop condition is re-checked (True). Control reaches `yield i` again, now with `i = 2`.
3. `yield i` hands the stone `2` back, and freezes again.

A third pump produces `3`. A fourth pump:

```python
print(next(gen))       # 3
print(next(gen))       # StopIteration
```

The fourth pump resumes after the previous yield. `i` becomes `4`. The loop condition `i <= n` is now `False`. The loop exits. The procedure runs out of code. The workstation does not have another item to hand over — instead, it signals it is **exhausted** by raising `StopIteration`.

After that, the workstation cannot be pumped again. Each generator workshop is a one-shot stream of items.

---

## For Loops Walk Generators Naturally

You almost never call `next()` by hand. The natural consumer of a generator is a `for` belt — the same `for` you have been using since Lesson 14.

```python
for x in count_up_to(3):
    print(x)
# 1
# 2
# 3
```

What is the `for` belt doing internally? Exactly what you would do by hand: it pumps with `next()` until it sees `StopIteration`, then it stops cleanly. The exception is part of the protocol, not an error.

This is also why all the collection items in the warehouse work with `for`. Each one knows how to produce a fresh iterator on demand — a one-shot pumpable workstation that walks its contents. The `for` belt does not care whether it is walking a row, a scroll, a generator, or anything else — as long as the thing supports the pump-with-`next` protocol.

---

## Generator Expressions Revisited

You met this form in Lesson 15:

```python
squares = (n * n for n in range(1, 11))
```

This is *exactly* a generator workstation, written in the compact-belt form. The round brackets are the syntactic difference from a list comprehension; the runtime difference is enormous. A list comprehension builds the whole row in advance. A generator expression builds a generator workshop that produces one item at a time when pumped.

```python
# List comprehension — builds the whole row
squares_row = [n * n for n in range(1, 11)]
print(squares_row)      # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

# Generator expression — builds a one-item-at-a-time workstation
squares_gen = (n * n for n in range(1, 11))
print(squares_gen)      # <generator object ...>

# Both can be walked with a for belt
for s in squares_gen:
    print(s)
```

When the consumer is a workstation that accepts a sequence — `sum`, `max`, `min`, `any`, `all` — and the generator is the sole argument, you can drop the outer parentheses entirely:

```python
total = sum(n * n for n in range(1, 11))
print(total)            # 385
```

This is the most common use of generator expressions in real Python code: feeding a one-pass aggregate directly, with no intermediate row built.

---

## Lazy Evaluation — The Practical Payoff

Generators are **lazy**. Nothing inside them runs until something asks for the next item. This unlocks three concrete advantages.

**1. Constant memory regardless of size.**

```python
def squares(stop):
    n = 1
    while n <= stop:
        yield n * n
        n += 1

# Works for 10 million items just as easily as for 10:
total = sum(squares(10_000_000))
print(total)            # ... a very large number
```

The whole row of ten million squares is never built. At any moment the program holds one squared stone and a small amount of generator state.

**2. Short-circuit consumers stop early.**

```python
gen = squares(10_000_000)
for s in gen:
    if s > 100:
        print("First square over 100:", s)
        break
```

The generator runs only as far as needed to satisfy the consumer. Once `break` fires, no further items are pumped. If you had used a list comprehension, the entire ten-million-item row would have been built first — wasted work.

**3. Infinite sequences.**

A generator can model a sequence that never ends. Used carefully with a consumer that *does* stop, this is perfectly safe:

```python
def naturals():
    n = 1
    while True:
        yield n
        n += 1

for n in naturals():
    if n > 5:
        break
    print(n)
# 1 2 3 4 5
```

A list of "all natural numbers" cannot be built — the warehouse is finite. A generator of all natural numbers is a few lines of code, and only ever produces as many as you ask for.

---

## Generators as Pipelines — Chaining

Generators compose naturally. One generator can pull from another, transforming items along the way, yielding the transformed items downstream. The result is a *pipeline* — a chain of workstations, each pausing between items, with a single pump at the end driving the whole chain.

```python
def naturals():
    n = 1
    while True:
        yield n
        n += 1

def squared(source):
    for n in source:
        yield n * n

def take(source, count):
    for _ in range(count):
        yield next(source)


pipeline = take(squared(naturals()), 5)

for x in pipeline:
    print(x)
# 1 4 9 16 25
```

Trace the flow of a single pump on `pipeline`:

1. The `for` belt calls `next(pipeline)`.
2. `take` resumes; it calls `next` on its source, which is `squared(naturals())`.
3. `squared` resumes; it calls `next` on its source, which is `naturals()`.
4. `naturals` resumes; it yields `1` and freezes.
5. `squared` receives `1`, computes `1`, yields it, freezes.
6. `take` receives `1`, increments its counter, yields it, freezes.
7. The `for` belt receives `1` and runs its body.

A single pump cascaded through three workstations. The next pump cascades through all three again. Each workstation holds a small amount of state, never more. Memory stays tiny no matter how long the pipeline runs.

This is the model behind much of Python's standard data-processing patterns — and the entire reason the `itertools` module (which we will not study formally) exists. You will see generator pipelines everywhere in real Python code.

---

## A Brief Word on Iterators

The full Python word for "anything you can walk with `for`" is **iterable**. A row, a scroll, a cabinet's keys, a generator — all iterables. When the `for` belt walks one, it first calls `iter(thing)` to produce an **iterator** — a one-shot pumpable workstation that yields the iterable's items.

A generator is one kind of iterator (the kind produced by `yield`). You can also write a custom iterator by defining `__iter__` and `__next__` on a class — the same dunder pattern you saw with `__init__` and `__str__` in Lesson 16. We will not write one here; the generator form is almost always cleaner. Recognise the names when you see them in other code.

```python
nums = [10, 20, 30]
it = iter(nums)               # build an iterator from the row
print(next(it))               # 10
print(next(it))               # 20
print(next(it))               # 30
# next(it) again would raise StopIteration
```

This is what every `for x in nums:` is doing internally. You can drive it by hand if you ever need the fine-grained control.

---

## When to Use a Generator

Three patterns where the generator form is the right choice:

- **The source is large or infinite.** Anywhere a list comprehension would build a row that is too big to hold all at once, a generator does the same work with constant memory.
- **The consumer might stop early.** If only the first match matters, or the first `n` items, a generator runs only as far as needed.
- **A pipeline of transformations.** When several operations apply in sequence to a stream, chaining generators is cleaner than building intermediate rows at each stage.

When the result is small, will definitely be used in full, and needs to be walked more than once — a row (list comprehension) is the right shape. Generators can only be walked **once**; after the first pass they are exhausted.

A common beginner trap is reaching the end of a generator and then trying to use it again:

```python
gen = (n * n for n in range(5))
print(list(gen))            # [0, 1, 4, 9, 16]
print(list(gen))            # []   — already exhausted!
```

After the first `list(gen)` walked it to completion, the generator was done. The second pass had nothing left. If you need to walk the same data twice, materialise it into a row.

---

## What You Now Know

You can write a generator workstation using `yield` — a procedure that pauses partway, hands over one item, and waits for the next request. You know that calling a generator function builds a frozen workshop, and that `next()` pumps it. You know `for` loops drive generators automatically by following the pump-until-`StopIteration` protocol that every iterable supports.

You can choose between list comprehensions and generator expressions based on the shape of the data — eager-row when the result is small and needed in full, lazy-generator when the source is large, the consumer might stop early, or you are building a pipeline. You have seen how generators chain into pipelines that hold constant memory regardless of the length of the stream they process.

You also know the once-only rule: a generator can be walked one time. After exhaustion, it is done.

This is the last *form* on the Factory Floor that you will meet in this series. The remaining floor lesson covers two convenience patterns built on what you already have: anonymous workstations (lambda) and workstation wrappers (decorators).

---

## Quick Reference

| Python | Pause-and-resume image |
|---|---|
| `def gen():` `    yield item` | A generator workstation. Presence of `yield` makes it one. |
| `g = gen()` | Build the workshop, frozen at the start. No procedure code runs yet. |
| `next(g)` | Pump — run until the next `yield`, hand over the item, freeze. |
| `StopIteration` | Raised when the procedure has run out of items. |
| `for x in g:` | The natural consumer — pumps until exhausted, then stops cleanly. |
| `(expr for x in coll)` | Generator expression — compact form of a generator. |
| `sum(expr for x in coll)` | Drop the outer parens when the generator is the sole argument. |
| `iter(iterable)` | Build an iterator from any iterable (rows, scrolls, cabinets…). |
| Lazy evaluation | Nothing runs until something asks for the next item. |
| One-shot | Each generator walks exactly once; afterwards it is exhausted. |
| Infinite sequences | Allowed — the consumer is responsible for stopping. |

---

## Try It

**Your first generator:**

```python
def count_up_to(n):
    i = 1
    while i <= n:
        yield i
        i += 1

for x in count_up_to(5):
    print(x)
```

**Pumping by hand:**

```python
gen = count_up_to(3)
print(next(gen))
print(next(gen))
print(next(gen))
try:
    print(next(gen))
except StopIteration:
    print("exhausted")
```

**Generator expression — the compact form:**

```python
squares_gen = (n * n for n in range(1, 6))
print(list(squares_gen))
print(list(squares_gen))        # empty — already exhausted
```

**Aggregate with a generator expression:**

```python
print(sum(n * n for n in range(1, 11)))
print(max(len(w) for w in ["alpha", "beta", "elephant"]))
```

**An infinite generator with a stopping consumer:**

```python
def naturals():
    n = 1
    while True:
        yield n
        n += 1

count = 0
for n in naturals():
    count += 1
    if count > 5:
        break
    print(n)
```

**A small pipeline:**

```python
def naturals():
    n = 1
    while True:
        yield n
        n += 1

def squared(source):
    for n in source:
        yield n * n

def take(source, count):
    for _ in range(count):
        yield next(source)


for x in take(squared(naturals()), 5):
    print(x)
```

Three generators chained. A single pump on the outer one cascades through all three.

**The exhaustion trap, deliberately:**

```python
gen = (n * n for n in range(5))
print("first pass:", list(gen))
print("second pass:", list(gen))    # [] — already done
```

If you need a second pass, materialise into a row first:

```python
gen = (n * n for n in range(5))
materialised = list(gen)
print("first:", materialised)
print("second:", materialised)
```

---

## Where Next?

The final Factory Floor lesson covers two more constructs — **anonymous workstations** (built on the spot for a single job, no name plate) and **workstation wrappers** (a layer applied around an existing workstation that runs before and after every job order). After that, the series moves into the supporting buildings.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 19 | Z5 — Factory Floor | Lambda and Decorators — impromptu workstations and wrappers |
| Lesson 20 | Z1 — Tool Store | Modules and Packages — stocking and fetching |
| Lesson 21 | Z2, Z8 | File Handling — crates at the loading bay |
| Lesson 22 | Z7 — Quality Control | Error Handling — the inspection belt |

*See the full lesson map in **The Factory — A Complete Map**.*
