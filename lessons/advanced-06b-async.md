# Python for Hyperphantasic Minds
## Advanced Phase 6b — Async

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus + standard professional Python practice  
> **Lesson**: Advanced 6b — the final lesson of the series  
> **Topic**: Async — one worker, no idle time  
> **Factory zone**: Z10 — The Night Shift Wing  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Advanced — assumes the entire main series + Phases 5 and 6a are comfortable  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Before You Enter

The same warning that opened Advanced 6a applies here. VOCABULARY.md describes Z10 with the line *"Do not enter until the main factory is running smoothly."* Async is concurrency under a different shape from threading, but it is *still concurrency* — the same class of subtle bugs, the same demand for careful reasoning.

If you are comfortable with the main 25 lessons, Phase 5, and the threading lesson — read on. If not, the rest of Python works perfectly well without async.

---

## Where You Are

You are still in the **Night Shift Wing**. Lesson 6a covered one concurrency model — threading, the parallel-production-line approach. This lesson covers the other — `async`, the one-worker-switching-rapidly-between-many-jobs approach.

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

---

## The Picture — One Worker, No Idle Time

Threading (6a) was *many workers in parallel*. Async is the opposite: **one worker, switching rapidly between many waiting jobs**.

Picture the Night Shift floor at night, with one efficient worker on duty. Many jobs are pending — fetch this URL, read that file, query that database, wait for that timer. Each job has a step where it must *wait for the outside world* — the network response, the disk read, the timer ticking down. While a job is waiting, the worker steps aside and picks up a different job. When the first job's wait is over (the response has arrived), the worker comes back to it and continues. The worker is never idle as long as *any* job has progressable work.

The canonical phrase from VOCABULARY: a **pause-able job order**. Each async job can declare *"I am waiting for something to arrive"* by writing `await`. The worker reads that declaration, steps aside, and picks up another job.

This is a single-threaded model. There is genuinely *one worker*. The GIL of 6a is not a problem here because we never had two workers in the first place — we have one worker who is very efficient at not standing around.

The practical consequence: async excels at *I/O-bound concurrency* — lots of network requests, lots of file reads, lots of waits. It does not help with CPU-bound work. (For CPU-bound work, `multiprocessing`, exactly as in 6a.)

---

## `async def` and `await` — The Syntax

A normal workstation is defined with `def`. A *pause-able* workstation is defined with `async def`:

```python
async def fetch_page(url):
    response = await http_get(url)
    return response.text
```

Two new pieces of syntax:

- **`async def`** marks the workstation as a *coroutine* — a pause-able procedure. Calling it does not run it; it produces a coroutine workshop, ready to be scheduled.
- **`await expr`** is the *pause point*. It says: *"if `expr` is not ready, step aside and let the coordinator give me back the worker later"*. Only legal inside an `async def`.

Calling a regular workstation runs it immediately. Calling an `async def` workstation **does not** run it:

```python
async def hello():
    print("hi")

# This does NOT print anything:
job = hello()
print(job)              # <coroutine object hello at 0x...>
```

You get a *coroutine workshop*, exactly like a generator workshop from Lesson 18 — frozen at the start, waiting to be pumped. The thing that pumps async workshops is the **coordinator** — `asyncio`.

---

## Starting the Coordinator — `asyncio.run`

To actually run an async workstation, you need a coordinator. The simplest way:

```python
import asyncio


async def main():
    print("Shift starting on the Night floor.")
    await asyncio.sleep(1)
    print("One second later.")


asyncio.run(main())
```

`asyncio.run(coroutine)` does three things:

1. Builds the coordinator.
2. Schedules the given coroutine as the first job.
3. Runs the coordinator until that job and every job it spawns have completed.

Inside, `asyncio.sleep(1)` is the canonical *waitable thing* — a coroutine that announces "I am waiting for one second to pass, give the worker to someone else in the meantime". Using it is how you simulate I/O in examples without actually performing any I/O.

Every async program has exactly one `asyncio.run(...)` call near the top. It is the bridge from the ordinary synchronous world (where `def main(): main()` is the usual entry point) into the async one.

---

## Running Many Jobs at Once — `asyncio.gather`

The single most useful primitive in async Python is `asyncio.gather`. It schedules many coroutines concurrently and waits for them all to finish:

```python
import asyncio


async def fake_fetch(name, delay):
    print(f"{name}: starting")
    await asyncio.sleep(delay)
    print(f"{name}: done")
    return f"{name}-result"


async def main():
    results = await asyncio.gather(
        fake_fetch("a", 2),
        fake_fetch("b", 1),
        fake_fetch("c", 3),
    )
    print(results)


asyncio.run(main())
```

Output:

```
a: starting
b: starting
c: starting
b: done
a: done
c: done
['a-result', 'b-result', 'c-result']
```

Three jobs, run *concurrently*. Sequentially they would have taken 2 + 1 + 3 = 6 seconds. With `gather`, they take 3 seconds (the longest of the three). Notice:

- All three "starting" lines print immediately — the worker visits each job in turn until each hits its `await`, then moves on.
- The "done" lines print in the order the *sleeps* finish (b first at 1s, a next at 2s, c last at 3s).
- The returned row from `gather` is in *input order*, not finish order — same as `concurrent.futures.ThreadPoolExecutor.map` from Lesson 6a.

`gather` accepts any number of coroutines; the typical pattern uses a comprehension:

```python
async def main():
    urls = [f"https://example.com/{i}" for i in range(50)]
    results = await asyncio.gather(*(fetch_page(u) for u in urls))
    return results
```

Fifty fetches running concurrently, with the same syntactic weight as a single one. This is what makes async I/O qualitatively different from synchronous I/O: scaling from one concurrent operation to fifty does not change the code much.

---

## The Wait — Only Inside `async def`

A strict syntactic rule: **`await` can only be used inside an `async def` workstation**. Try to put it anywhere else and Python triggers a `SyntaxError`:

```python
def normal():
    await asyncio.sleep(1)     # SyntaxError — await is not allowed here
```

This is structural, not a stylistic preference. The `await` keyword tells the *coordinator* to pause this job. Without an enclosing async context, there is no coordinator to pause — the keyword is meaningless.

Two related rules that follow from this:

- **You cannot mix sync and async carelessly.** Calling an async workstation from a sync one without `await` (and without an async context to await in) returns the coroutine workshop unrun. Calling a slow sync workstation from inside an async one *blocks the whole coordinator* — the entire factory pauses until the sync call returns. This is the "async-all-the-way-down" rule.
- **`asyncio.run(...)` is the boundary.** It is the one place you transition from sync (the script's top level) into async. Inside the coroutine that `asyncio.run` schedules, everything is async; outside, everything is sync.

A common beginner mistake is to call a slow library function inside an async workstation, not realising the library is synchronous. The async program looks correct but does not gain any concurrency — the slow call blocks the worker, and every other awaiting job stalls. The fix is to *use an async equivalent of the library*, or to push the slow call into a separate thread.

---

## I/O Libraries — `aiohttp`, `aiofiles`, and Friends

Python's standard library is largely synchronous. The `requests` library, the `open()` workstation, the `sqlite3` toolkit — all synchronous. To use them inside an async program *and benefit from the concurrency*, you need async-aware alternatives. These are third-party libraries, ordered from PyPI:

| Sync toolkit | Async alternative | Use |
|---|---|---|
| `requests` | `aiohttp` or `httpx` | HTTP requests |
| `open()` (file I/O) | `aiofiles` | File reads and writes |
| `sqlite3` | `aiosqlite` | SQLite |
| `psycopg2` (PostgreSQL) | `asyncpg` | PostgreSQL |
| `time.sleep()` | `asyncio.sleep()` | Pausing |

A worked example using `aiohttp`:

```python
import asyncio
import aiohttp


async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()


async def main():
    urls = [
        "https://example.com",
        "https://www.python.org",
        "https://docs.python.org",
    ]
    async with aiohttp.ClientSession() as session:
        pages = await asyncio.gather(*(fetch(session, u) for u in urls))
    return [len(p) for p in pages]


print(asyncio.run(main()))
```

`async with` is the async version of the lockable-room pattern from Lesson 21. The same guaranteed-exit behaviour applies, with async-aware setup and teardown.

If you cannot find an async-aware library for what you need — a common situation for niche tools — you can move the synchronous call onto a separate thread so it does not block the coordinator:

```python
import asyncio


def slow_sync_call(x):
    # imagine this is a slow library that has no async version
    time.sleep(2)
    return x * 2


async def main():
    result = await asyncio.to_thread(slow_sync_call, 21)
    print(result)


asyncio.run(main())
```

`asyncio.to_thread(fn, *args)` runs `fn(...)` on a separate thread and returns a coroutine you can `await`. While the slow call runs on the side thread, the coordinator continues servicing other waiting jobs. This is the standard escape hatch for "async program that has to call one synchronous library".

---

## Async vs Threading — Choosing

You now have two concurrency tools. They are not interchangeable.

| Property | Threading (6a) | Async (this lesson) |
|---|---|---|
| Number of workers | Many | One |
| Switching trigger | Operating system, anywhere | At explicit `await` points only |
| Scaling | Heavyweight — tens to low hundreds of threads | Lightweight — thousands of concurrent tasks |
| Sync code compatible | Yes — any function can run in a thread | No — must use async-aware libraries |
| Race condition risk | High — every shared shelf needs a lock | Lower — switching only at `await` points |
| GIL effect | Yes — only one bytecode-runner at a time | N/A — there was only one to begin with |
| Best fit | Legacy sync code, mixed workloads | All-async I/O-heavy programs at high scale |

A practical decision:

- Greenfield project, lots of network I/O, can choose your libraries → **async**.
- Existing program with synchronous I/O libraries → **threading**.
- CPU-bound work → **multiprocessing**, neither of the above.

Most programs need none of these. The single-threaded `for url in urls: fetch(url)` is fine when there are five URLs. It becomes worth replacing when there are five hundred.

---

## A Practical Example

A small but realistic async script — fetching a list of URLs concurrently and reporting timing:

```python
import asyncio
import time


async def fake_fetch(url, delay):
    """Simulate fetching `url` taking `delay` seconds."""
    await asyncio.sleep(delay)
    return f"{url}: {delay}s"


async def main():
    jobs = [
        fake_fetch("alpha", 1),
        fake_fetch("bravo", 3),
        fake_fetch("charlie", 2),
        fake_fetch("delta", 1),
        fake_fetch("echo", 2),
    ]

    start = time.perf_counter()
    results = await asyncio.gather(*jobs)
    elapsed = time.perf_counter() - start

    for r in results:
        print(r)
    print(f"\nTotal time: {elapsed:.2f}s (sequential would be {sum(d for _, d in [('alpha',1),('bravo',3),('charlie',2),('delta',1),('echo',2)])}s)")


asyncio.run(main())
```

Output:

```
alpha: 1s
bravo: 3s
charlie: 2s
delta: 1s
echo: 2s

Total time: 3.00s (sequential would be 9s)
```

Five jobs, one worker, total time governed by the longest job. The replacement for this with `aiohttp` and real URLs is a two-line change.

---

## What You Now Know

You have walked through both halves of the Night Shift Wing. Threading (6a) gave you parallel production lines with the GIL caveat and the race-condition hazard. Async (today) gave you the alternative — one worker, no idle time, no parallelism, and no race conditions by default because there is only one worker.

You can write a coroutine with `async def`, pause it at any `await`, schedule many at once with `asyncio.gather`, and run the whole coordinator with `asyncio.run`. You know `await` is only legal inside an `async def`, that mixing sync and async needs deliberate care (`asyncio.to_thread` is the standard escape hatch), and that I/O libraries usually have an async counterpart available from PyPI.

You can choose between threading, async, and multiprocessing based on the shape of the work — sync code with blocking I/O, all-async with many concurrent waits, or CPU-bound parallelism.

---

## End of Series

This is the final lesson of *Python for Hyperphantasic Minds*. Twenty-five main lessons plus four advanced. Every zone of the factory has been visited; every shape of Python you are likely to meet has been mapped to a physical structure you can return to.

What now?

- **Build a real project.** A tutorial gives you the shapes; a project teaches you the shapes you did not notice. Pick something small but real — a script you actually use, a tool that automates something tedious, a small web service. Finish it. Then build the next one.
- **Read other people's code.** Open-source Python is one of the largest, friendliest codebases in the world. Pick a library you use and read its source. Compare what you see there to the factory map in your head. The match — and the mismatches — will teach you faster than any further lesson.
- **Contribute to something.** Even a documentation fix counts. Submitting a real change to a real codebase is the moment you stop being a learner and start being a Python developer.
- **Keep the vocabulary.** The factory is yours now. Carry it. When something in real Python feels unfamiliar, find where in the factory it lives. Most things slot in cleanly; the few that do not are usually mistakes in the codebase, not in your map.

You are no longer a beginner. The factory is one world, and you can walk around in it.

---

## Quick Reference

| Python | Async image |
|---|---|
| `async def name(...):` | Define a pause-able workstation (a *coroutine*). |
| `await expr` | Pause point — only legal inside `async def`. |
| `name(...)` | Returns a coroutine workshop; does not run it. |
| `asyncio.run(coroutine)` | Run the coordinator on this coroutine until everything finishes. |
| `asyncio.sleep(seconds)` | The canonical waitable — wait without blocking. |
| `asyncio.gather(*coros)` | Run many coroutines concurrently; await all; results in input order. |
| `async with ctx:` | Async lockable room — works the same as `with` but for async resources. |
| `async for x in iter:` | Async iteration — for async-producing sources. |
| `asyncio.to_thread(fn, *args)` | Run a sync function on a side thread; await the result. |
| `aiohttp`, `aiofiles`, `aiosqlite` | Common async I/O libraries — order from PyPI. |
| Async vs threading | Async is one worker switching; threading is many workers running. |
| When to use async | All-async I/O-heavy code at scale. Pick async-aware libraries. |

---

## Try It

**The minimum:**

```python
import asyncio


async def main():
    print("Hello from the Night Shift.")
    await asyncio.sleep(1)
    print("...one second later.")


asyncio.run(main())
```

Notice the second print appears after a one-second pause — but the program was not *blocked*. With more jobs, the worker would have used that second on other waiting jobs.

**Several jobs, gathered:**

```python
import asyncio


async def fake_fetch(name, delay):
    print(f"{name}: starting")
    await asyncio.sleep(delay)
    print(f"{name}: done")
    return name


async def main():
    results = await asyncio.gather(
        fake_fetch("a", 2),
        fake_fetch("b", 1),
        fake_fetch("c", 3),
    )
    print("All results:", results)


asyncio.run(main())
```

Time it. Total elapsed: about 3 seconds (the longest). Sequentially: 6 seconds.

**The await-outside-async error:**

```python
import asyncio

await asyncio.sleep(1)           # SyntaxError — try once and feel it
```

This will not even parse — Python rejects the file before running anything.

**Calling an async workstation from a sync context — what goes wrong:**

```python
import asyncio


async def hello():
    print("hi from inside the coroutine")


hello()                          # builds the coroutine but never runs it
# RuntimeWarning: coroutine 'hello' was never awaited
```

Run this. You should see no `hi` — only a warning. Now do it properly:

```python
asyncio.run(hello())             # runs the coroutine
```

**A scaling demonstration:**

```python
import asyncio
import time


async def fake_fetch(delay):
    await asyncio.sleep(delay)


async def main():
    n = 100
    start = time.perf_counter()
    await asyncio.gather(*(fake_fetch(0.1) for _ in range(n)))
    elapsed = time.perf_counter() - start
    print(f"{n} jobs in {elapsed:.2f}s — sequential would be {n * 0.1}s")


asyncio.run(main())
```

One hundred jobs of one-tenth of a second each, concurrent: about 0.1s total. Sequential: 10s. This is the scaling property that makes async worth its complexity at high concurrency.

**Mixing sync and async safely:**

```python
import asyncio
import time


def slow_sync(seconds):
    time.sleep(seconds)
    return f"slept {seconds}s"


async def main():
    # Without to_thread, this would block the whole coordinator:
    result = await asyncio.to_thread(slow_sync, 1)
    print(result)


asyncio.run(main())
```

`asyncio.to_thread` runs the synchronous call on a side thread, leaving the coordinator free.

---

## Where Next?

There is no next lesson. The factory is complete; the rest is real Python work.

Three concrete next moves:

- **Pick a project you actually want.** Build it. Use whatever the factory provides; reach for the standard library and PyPI when you need to. Finish.
- **Read the source of a library you already use.** Map what you see to the factory map. Both the matches and the mismatches will teach you.
- **Make a small contribution somewhere.** A docs typo, a clearer error message, a missing test. The smallest accepted change is the moment you become a Python developer.

The factory is one world. You can walk around in it.

---

*Thank you for walking the factory.*
