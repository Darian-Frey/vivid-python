# Python for Hyperphantasic Minds
## Advanced Phase 6a — Threading

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus + standard professional Python practice  
> **Lesson**: Advanced 6a (after Phase 5)  
> **Topic**: Threading — two lines running in parallel  
> **Factory zone**: Z10 — The Night Shift Wing  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Advanced — assumes the entire main series + Phase 5 are comfortable  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Before You Enter

VOCABULARY.md describes Z10 with a one-line warning: *"Do not enter until the main factory is running smoothly."* This lesson begins by repeating it.

The Night Shift Wing is not optional in the same way that the rest of Phase 5 is optional. Concurrency is *genuinely harder* than every other topic in this series. The bugs are subtler. The debugging is slower. The mistakes you make here cost more time than the mistakes you made in the main series.

If you are not already comfortable with workstations, junctions, conveyor belts, classes, error handling, and the rest of the main 25 lessons — close this file and come back later. There is no penalty for doing so. The factory works perfectly well without concurrency for almost every program a beginner-to-intermediate Python developer writes.

If you are comfortable with all of the above, read on.

---

## Where You Are

```
                    ┌──────────────────────────────┐
                    │    SHIFT MANAGER'S OFFICE     │
                    └──────────────┬───────────────┘
                                   │
┌──────────┐                       ▼      ┌─────────────────┐
│          │        ┌──────────┐          │                 │
│TOOL STORE│───────▶│ GOODS IN │          │  FACTORY FLOOR  │
│          │        │          │          │                 │
└──────────┘        └────┬─────┘          └────────┬────────┘
                         │                         │
                         ▼                         │
              ┌──────────────────────┐             │
              │      WAREHOUSE       │◀────────────┘
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

  ═══════════════════════════════════════════════════════════
                ██ Z10 — NIGHT SHIFT WING ██
                       YOU ARE HERE
  ═══════════════════════════════════════════════════════════
```

The Night Shift Wing is a separate building through heavy double doors at the far end of the complex. It is not on any production line. Inside, the factory runs *more than one line at a time*, sharing a single warehouse between them.

This lesson covers **threading** — multiple production lines running in parallel, each with its own worker, sharing the warehouse. Lesson 6b covers async — one worker switching rapidly between many waiting jobs.

---

## The Picture — Two Lines, One Warehouse

Picture the main factory you have walked through. Now picture *another complete factory* — a second copy of every workstation, every conveyor belt — running alongside the first, with both factories drawing from the *same* warehouse.

This is what a **thread** is. The canonical phrase is **a parallel production line**. Each thread is a worker, plus a procedure, plus a private stack of local shelves (just like a workstation's locked room). What threads share is the open-aisle warehouse — the program's top-level shelves are visible to *every* line at once.

The naive expectation is that two lines running in parallel are twice as fast as one. Sometimes that is true. Often, especially in Python, it is *not* — for reasons we will come to in *The Global Interpreter Lock* below. For now, the visual is what matters: **two workers, sharing shelves, doing work at the same time.**

---

## Starting a Parallel Line — `threading.Thread`

The standard-library toolkit is `threading`. The minimum-viable parallel program:

```python
import threading
import time


def worker(name):
    for i in range(3):
        print(f"{name}: step {i}")
        time.sleep(0.5)


# Build a second production line, with its worker running the `worker` procedure
second_line = threading.Thread(target=worker, args=("Line-B",))
second_line.start()

# Meanwhile, the main line keeps running:
worker("Line-A")

# Wait for the second line to finish before the shift ends:
second_line.join()
print("Both lines finished.")
```

Output (approximately — the exact interleaving will vary):

```
Line-A: step 0
Line-B: step 0
Line-A: step 1
Line-B: step 1
Line-A: step 2
Line-B: step 2
Both lines finished.
```

Four critical methods on the parallel-line item:

- **`threading.Thread(target=fn, args=...)`** — *build* a parallel line, but do not start it. `target` is the workstation it will run; `args` is a sealed crate of materials to hand to that workstation.
- **`.start()`** — *open the doors* and let the parallel line begin running. The main line continues immediately past this call.
- **`.join()`** — *wait at the door* for the parallel line to finish. The main line pauses here until the second line is done. Without `.join()`, the main program may end while the second line is still working — sometimes that is fine, often it is not.
- **`.is_alive()`** — switch — *is the parallel line currently running?*

A subtle but important point: **the line does not start when you build it.** `threading.Thread(...)` only constructs the line; `.start()` opens its doors. This is intentional — you might want to build several lines, then start them all at once, then join them all at the end.

---

## The Daemon Switch

A small detail worth knowing. By default, a parallel line keeps running even when the main line finishes — the Python program will not exit until *every* line has joined back. If you have a background line that should die when the main program ends, mark it as a **daemon**:

```python
worker_line = threading.Thread(target=background_job, daemon=True)
worker_line.start()
```

A daemon line runs while the main program runs, and stops abruptly when the main program ends. This is right for background-monitoring lines, scheduled tickers, and anything you do not want to wait for. It is wrong for any line whose work *must* complete (saving data, finalising a file). For "must complete" work, leave `daemon=False` (the default) and `.join()` the line before the shift ends.

---

## The Race Condition — Two Lines Reaching For the Same Shelf

Here is the central hazard of threading.

When two parallel lines touch the *same* warehouse shelf at the same time, the outcome depends on the *exact* moment each worker reaches the shelf. The canonical visualisation: **two workers arriving at the same cubbyhole in the same instant — both read the value, both compute a new value, both write back, and one of the two updates is silently lost.**

The classic demonstration is a shared counter. Imagine two lines each incrementing the same `count` shelf one hundred thousand times:

```python
import threading

count = 0


def increment_many_times():
    global count
    for _ in range(100_000):
        count += 1


line_a = threading.Thread(target=increment_many_times)
line_b = threading.Thread(target=increment_many_times)

line_a.start()
line_b.start()
line_a.join()
line_b.join()

print(count)
```

You would expect `count` to be exactly `200_000`. Run the program a few times. You may see `200000` (correct, by luck) or `184217` or `196033` or any other number below `200_000`. The program *loses updates*.

Picture what happened. `count += 1` is not one action; it is three:

1. Read the current value of the `count` shelf into a worker-local stone.
2. Add 1 to that worker-local stone.
3. Write the new stone back to the `count` shelf.

If Line A is between steps 2 and 3, and Line B reads the shelf at that moment, Line B sees the *old* value. When Line A finally writes back, then Line B writes back its own (also-old) computation, A's update is silently overwritten.

This is a **race condition** — the canonical phrase *"two lines reaching for the same shelf simultaneously"*. The bug is not in any single line; it is in the *interaction* between two lines that each, on their own, would have been correct.

Race conditions are insidious because they are *non-deterministic*. The same program run twice may produce different results — or the right result one time and the wrong one the next. Testing alone (Phase 5a) cannot catch a race condition reliably; you have to reason about *every possible interleaving* of the lines.

The defensive habit: **whenever two lines share a mutable shelf, the access must be protected.**

---

## The Shelf Key — `threading.Lock`

The standard protection is a **lock** — a small key in a holder beside the shared shelf. Only the worker holding the key may access the shelf. Others wait at the holder until the key is returned.

```python
import threading

count = 0
count_key = threading.Lock()


def increment_many_times():
    global count
    for _ in range(100_000):
        with count_key:                  # take the key; release on block exit
            count += 1


line_a = threading.Thread(target=increment_many_times)
line_b = threading.Thread(target=increment_many_times)

line_a.start()
line_b.start()
line_a.join()
line_b.join()

print(count)                             # 200000 — every time
```

`with count_key:` is the lockable-room pattern from Lesson 21, applied to a key rather than a file. Inside the block, the holder has the key and no other line can enter. The moment the block ends (normally or via alarm), the key is released and the next waiting line can take it.

The result: the read-modify-write pattern is now *atomic* — no other line can interleave between the read and the write. The race is gone. The output is `200000` every time.

The cost: lines that contended for the same key spend time *waiting*. Locks serialise access — exactly the opposite of what threading is supposed to provide. The art of writing fast concurrent code is to share as little as possible, and lock around what little you must share for as short a time as possible.

You can also acquire and release the lock by hand:

```python
count_key.acquire()
try:
    count += 1
finally:
    count_key.release()
```

The `with` form is equivalent and shorter. Prefer it.

---

## The Global Interpreter Lock — Honest Caveat

A property of Python that genuinely surprises people: in the standard interpreter (CPython), only *one thread is executing Python bytecode at any given moment*, no matter how many cores your machine has. This is the **Global Interpreter Lock**, or *GIL*.

The factory metaphor: there is a single supervisor on the Night Shift floor, and only the line whose worker has the supervisor's attention can actually execute Python instructions. The supervisor swaps attention rapidly between lines — but never grants attention to two lines at once.

What this means in practice:

- **For CPU-bound work** — long calculations, image processing, anything that runs hot on the processor — threads in Python do *not* give you a speed-up. Two lines pinning the CPU just contend for the same supervisor and the program runs at roughly the same speed as one line, sometimes slower (because of the swap overhead).
- **For I/O-bound work** — waiting on a network request, reading a slow file, querying a database — threads *do* give a real speed-up. When one line is *waiting*, the supervisor goes to another line. Many slow-but-mostly-idle jobs run in parallel beautifully.

The rule of thumb: **use threads for I/O-bound work; use multiprocessing (separate Python interpreters with separate GILs) for CPU-bound work.** If you find yourself reaching for threads to make a computation faster, you usually want `multiprocessing` instead.

(A long-standing Python project has been working on removing the GIL — a "free-threaded" Python is now an experimental option. For the purposes of this lesson and any production code you write today, assume the GIL is present.)

---

## The Modern Pattern — `concurrent.futures.ThreadPoolExecutor`

Building threads by hand with `threading.Thread(...).start()` / `.join()` works fine for one or two parallel lines. For many — fetching a hundred URLs, processing a hundred files — the manual pattern becomes tedious. The standard-library toolkit `concurrent.futures` offers a much cleaner one:

```python
from concurrent.futures import ThreadPoolExecutor
import time


def fetch(url):
    print(f"fetching {url}...")
    time.sleep(1)                        # pretend this is network latency
    return f"{url}: 200 OK"


urls = [f"https://example.com/{i}" for i in range(10)]

with ThreadPoolExecutor(max_workers=5) as pool:
    results = list(pool.map(fetch, urls))

for r in results:
    print(r)
```

Read this:

- `ThreadPoolExecutor(max_workers=5)` builds a *pool of five parallel lines*, ready to receive jobs.
- `pool.map(fetch, urls)` distributes the URLs across the pool — five at a time being processed in parallel, four more queued, and so on.
- The `with` block (Lesson 21's lockable room again) ensures the pool is properly shut down at the end.
- `list(...)` materialises the results (which arrive in the original order even though they finished in arbitrary order).

For ten URLs at one second each, the manual sequential form would take ten seconds. The pooled form takes about two — five in the first batch, five in the second.

This is the pattern you will see in real Python code far more often than raw `threading.Thread`. Use the pool when you can; reach for raw threads only when you genuinely need their full control.

---

## When NOT to Use Threading

A short discipline before the lesson closes.

- **Almost never use threads in a beginner-to-intermediate project.** The vast majority of programs are fast enough single-threaded. Threading adds complexity that is rarely justified by the speed gain.
- **Never use threads for parallel computation in standard Python.** The GIL kills the speed-up. Use `multiprocessing`.
- **Be very cautious about shared mutable state.** If two lines need to coordinate, prefer message passing (a `queue.Queue`) over shared shelves and locks. Locks are correct but error-prone; queues are correct and harder to misuse.
- **Test concurrent code under load.** A race condition that happens 1% of the time will not appear in a casual test — but will appear in production, repeatedly. Stress-test by running the operation thousands of times in a tight loop and check the results are consistent.

The cleanest concurrency model is the one your problem already has. If your work is naturally parallel (many independent jobs to do), use a `ThreadPoolExecutor` or `multiprocessing.Pool`. If your work is naturally sequential, leave it sequential.

---

## What You Now Know

You have walked into the Night Shift Wing — slowly, with the warning explicit. You can build a parallel production line with `threading.Thread`, start it with `.start()`, wait for it with `.join()`, mark it as daemon for background work. You know the central hazard — the race condition, *"two lines reaching for the same shelf simultaneously"* — and you have seen the unprotected counter lose updates non-deterministically. You can protect a shared shelf with a `threading.Lock` used in the `with` pattern; you know it is the lockable-room idiom from Lesson 21 applied to a key rather than a file.

You know the GIL exists, that it makes threading the wrong tool for CPU-bound work, and that `multiprocessing` is the right tool there. You know `ThreadPoolExecutor` is the cleaner modern pattern for many parallel jobs at once. And you know the discipline — share as little as possible, lock briefly, prefer message-passing, stress-test under load.

The final lesson of the series — Advanced 6b — covers `async`: a different concurrency model where one worker switches rapidly between many waiting jobs, instead of running many workers in parallel.

---

## Quick Reference

| Python | Night Shift image |
|---|---|
| `threading.Thread(target=fn, args=(...))` | Build a parallel production line; not yet started. |
| `t.start()` | Open the line's doors — the parallel work begins. |
| `t.join()` | Wait at the door for the line to finish. |
| `t.is_alive()` | Switch — is the line still running? |
| `threading.Thread(..., daemon=True)` | Background line; dies when the main program ends. |
| `threading.Lock()` | A shelf key in a holder. |
| `with lock:` | Take the key; release on block exit (normal or alarm). |
| `lock.acquire()` / `lock.release()` | Manual key handling. Prefer `with`. |
| Race condition | Two lines reaching for the same shelf simultaneously. |
| GIL | Only one line executes Python bytecode at a time, even on many cores. |
| `concurrent.futures.ThreadPoolExecutor` | A pool of parallel lines, ready to receive jobs. |
| `pool.map(fn, items)` | Distribute jobs across the pool; return results in input order. |
| Use threads for | I/O-bound work — network, files, databases. |
| Don't use threads for | CPU-bound work; use `multiprocessing` instead. |

---

## Try It

**A first parallel line:**

```python
import threading
import time


def worker(name):
    for i in range(3):
        print(f"{name}: step {i}")
        time.sleep(0.5)


line = threading.Thread(target=worker, args=("Line-B",))
line.start()
worker("Line-A")
line.join()
print("Both done.")
```

Run it a few times. The exact interleaving of "Line-A" and "Line-B" lines will vary — that is the nature of parallel execution.

**The race-condition demonstration — run it several times:**

```python
import threading

count = 0


def bump():
    global count
    for _ in range(100_000):
        count += 1


a = threading.Thread(target=bump)
b = threading.Thread(target=bump)
a.start(); b.start()
a.join(); b.join()
print(count)
```

Expected: `200000`. Likely seen: a smaller, *different-on-each-run* number.

**Now protect it with a key:**

```python
import threading

count = 0
key = threading.Lock()


def bump():
    global count
    for _ in range(100_000):
        with key:
            count += 1


a = threading.Thread(target=bump)
b = threading.Thread(target=bump)
a.start(); b.start()
a.join(); b.join()
print(count)                             # 200000, every time
```

**A pool of lines — many small jobs:**

```python
from concurrent.futures import ThreadPoolExecutor
import time


def fetch(url):
    print(f"fetching {url}")
    time.sleep(0.5)
    return f"{url}: ok"


urls = [f"item-{i}" for i in range(10)]

with ThreadPoolExecutor(max_workers=5) as pool:
    results = list(pool.map(fetch, urls))

for r in results:
    print(r)
```

Time the run. Ten jobs at half a second each, five at a time, takes about one second total instead of five.

**See the GIL in action — CPU-bound, no speed-up:**

```python
import threading
import time


def heavy():
    # busy work — no I/O
    total = 0
    for i in range(10_000_000):
        total += i


# Sequential:
start = time.perf_counter()
heavy(); heavy()
print(f"Sequential: {time.perf_counter() - start:.2f}s")

# Threaded:
start = time.perf_counter()
a = threading.Thread(target=heavy)
b = threading.Thread(target=heavy)
a.start(); b.start()
a.join(); b.join()
print(f"Threaded:   {time.perf_counter() - start:.2f}s")
```

The threaded version will be roughly the same speed as the sequential one — often slightly slower because of GIL swap overhead. This is the GIL effect; threads do not help CPU-bound work in standard Python.

---

## Where Next?

The final lesson of the series covers a different concurrency shape — `async` — one worker switching rapidly between many waiting jobs, without the parallel-lines machinery.

| Next lesson | Zone | Topic |
|---|---|---|
| Advanced 6b | Z10 — Night Shift Wing | Async — one worker, no idle time |

*See the full lesson map in **The Factory — A Complete Map**.*
