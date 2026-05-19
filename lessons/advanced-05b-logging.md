# Python for Hyperphantasic Minds
## Advanced Phase 5b — Logging

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus + standard professional Python practice  
> **Lesson**: Advanced 5b (after Advanced 5a)  
> **Topic**: Logging — permanent records from every zone  
> **Factory zone**: Z9 — The CCTV Room  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Post-beginner — assumes the main series and Advanced 5a are complete  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You enter **the CCTV Room** for the first time. The Room sits at the side of the factory complex, a small but always-staffed control room with a wall of monitors. Each monitor shows a feed from a different zone — Goods In, the Warehouse, the Factory Floor, Quality Control, the Records Department, Outgoings, all of them. The CCTV Room does not produce anything; it only *records*. Every notable event in the factory is reported here, written into a permanent log, and displayed on a monitor coloured by severity.

```
                    ┌──────────────────────────────┐
                    │    SHIFT MANAGER'S OFFICE     │
                    └──────────────┬───────────────┘
                                   │
┌──────────┐                       ▼        ┌─────────────────┐    ┌──────────────────┐
│          │        ┌──────────┐            │                 │    │                  │
│TOOL STORE│───────▶│ GOODS IN │            │  FACTORY FLOOR  │    │ ██ CCTV ROOM ██  │
│          │        │          │            │                 │    │                  │
└──────────┘        └────┬─────┘            └────────┬────────┘    │  YOU ARE HERE    │
                         │                           │             │                  │
                         ▼                           │             └──────────────────┘
              ┌──────────────────────┐               │                       ▲
              │      WAREHOUSE       │◀──────────────┘                       │
              └──────────┬───────────┘                                       │ feeds from
                         │                                                   │ every zone
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

The CCTV Room is Python's `logging` toolkit. Where `print()` shouts something across the Factory Floor (Lesson 2), `logging.info(...)` *reports it to CCTV* — the canonical verb. The report is stamped with a severity, the timestamp, and the zone it came from, then written to a log and shown on a colour-coded monitor.

This is the standard way that a running Python program tells the world what it is doing.

---

## Why Not Just `print`?

`print()` was the first tool the series taught (Lesson 2), and it has carried you through every example. For a single small script run once, `print` is perfectly adequate. For anything you intend to run regularly, in production, or with other people — `print` is *the wrong tool*. Three reasons:

- **One destination.** `print` always writes to the same stream (standard output). You cannot redirect debug prints to a file while keeping the user-facing messages on screen. `logging` separates these cleanly.
- **No severity.** A `print` of "User logged in" and a `print` of "Database connection lost" look identical. Anyone watching the output has to read every line carefully to spot the important ones. `logging` tags each report with a level so the important ones stand out visually.
- **No structure.** A `print` is a single scroll, dropped onto the output. `logging` adds timestamps, the source zone (which file, which workstation), the severity, automatically — so a log file from a real program is *searchable* and *aggregatable* without you writing any formatting code.

The rule of thumb: **use `print` for direct dialogue with the user; use `logging` for everything else.** Status updates, debug traces, error reports, audit trails — all of these are reports to CCTV.

---

## The Five Severity Monitors

Every report to CCTV carries a severity level. There are five canonical levels in increasing order of importance, each with a monitor colour. This is VOCABULARY.md Part 4.5; the summary:

| Level | Monitor | When to use |
|---|---|---|
| `DEBUG` | Dim grey | Internal details useful while developing — variable values, branch decisions. |
| `INFO` | Steady white | Normal operational events — shift started, file written, batch complete. |
| `WARNING` | Amber | Something noteworthy but not necessarily wrong — a retry happened, a deprecated path used. |
| `ERROR` | Red | Something went wrong on this job — an alarm triggered or a workstation failed. |
| `CRITICAL` | Flashing red | The program may not survive — data loss, resource exhaustion, fatal misconfiguration. |

The colours are not just decoration. Every modern log viewer (and every standard log handler that supports colour) tints `WARNING` amber, `ERROR` red, `CRITICAL` flashing red. The convention is universal.

When you choose a level for a report, ask: *"how alarmed should the operator be when they see this?"* `DEBUG` is "I'm not alarmed at all, this is just data". `INFO` is "everything is fine, take note if you want to". `WARNING` is "look at this". `ERROR` is "fix this". `CRITICAL` is "everything stops until this is fixed".

---

## The Minimum Working Report

The simplest possible use of `logging` is four lines:

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.info("Shift started.")
logging.warning("Cache miss — falling back to disk.")
logging.error("Could not save to %s", "results.txt")
```

Output:

```
INFO:root:Shift started.
WARNING:root:Cache miss — falling back to disk.
ERROR:root:Could not save to results.txt
```

Read this:

- `import logging` — fetch the toolkit from the Tool Store.
- `logging.basicConfig(level=logging.INFO)` — configure CCTV. The `level=` argument is the *floor* — reports below this level are filtered out and never reach a monitor. With `INFO`, the `DEBUG` reports are silently discarded; `INFO` and above are shown.
- `logging.info(...)`, `logging.warning(...)`, `logging.error(...)` — send a report to CCTV at the named severity.
- `logging.error("Could not save to %s", "results.txt")` — the `%s` placeholder and a separate value. `logging` does the substitution lazily — it only does the work if the report actually passes the level filter. This matters for `DEBUG` reports in tight loops; building the scroll up front would be wasteful.

This is the minimum-viable shape. Real programs replace `basicConfig` and `logging.info` with something more structured — see *Loggers Per Module* below.

---

## Loggers Per Module — The Standard Pattern

The `logging.info(...)` form above uses a shared *root logger* — fine for tiny scripts, awkward for anything larger. The standard pattern in real Python code is to create a logger *per module*, keyed by the module's `__name__` shelf:

```python
# in players.py

import logging

logger = logging.getLogger(__name__)


def add_player(name):
    logger.info("Adding player %s", name)
    # ...
```

`logging.getLogger(__name__)` builds (or fetches an existing) named logger. The convention `__name__` makes the logger's name match the module's name — so a report from `players.py` will be tagged with the source `players`, automatically.

The benefit: in `basicConfig` (or a richer configuration file), you can tune the level of *one specific module* independently:

```python
import logging

logging.basicConfig(level=logging.WARNING)
logging.getLogger("players").setLevel(logging.DEBUG)
# Now: WARNING and above everywhere — but DEBUG and above from players.py
```

This is invaluable in real applications. A bug somewhere in the `database` module? Turn its logger up to `DEBUG`; everything else stays quiet at `WARNING`.

You will see the `logger = logging.getLogger(__name__)` line at the top of essentially every module in essentially every professional Python codebase. It is the convention.

---

## Configuring CCTV — `basicConfig`

`basicConfig` is the simplest way to set up the CCTV Room. A more useful invocation:

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)
```

Output of subsequent reports now looks like:

```
2026-05-19 14:23:11 [INFO] players: Adding player Alice
2026-05-19 14:23:11 [WARNING] cache: Cache miss
```

Each report carries its timestamp, severity, source module, and message — the standard four-field log line that almost every production application uses.

The `format` argument uses the older `%`-style placeholders, not f-strings, because `logging` does the substitution itself. Common placeholders:

- `%(asctime)s` — the timestamp.
- `%(levelname)s` — the severity (`DEBUG`, `INFO`, etc.).
- `%(name)s` — the logger's name (`__name__` of the source module).
- `%(message)s` — the actual report text.
- `%(filename)s`, `%(lineno)d` — the source file and line.
- `%(funcName)s` — the workstation name (which function logged this).

For more advanced configurations — writing to a file, rotating logs, sending to a remote service — you move beyond `basicConfig` to **handlers** and **formatters**.

---

## Handlers and Formatters — Briefly

In the full `logging` model, a logger does not write anywhere itself. It produces a *report* and passes it to one or more **handlers** — destinations. Each handler has its own level filter and its own **formatter**, which decides how the report's fields are rendered to text.

A small example: log INFO+ to a file, and WARNING+ to the console.

```python
import logging

logger = logging.getLogger("myapp")
logger.setLevel(logging.DEBUG)

# Handler 1 — write everything INFO+ to a file
file_handler = logging.FileHandler("app.log")
file_handler.setLevel(logging.INFO)
file_handler.setFormatter(logging.Formatter(
    "%(asctime)s [%(levelname)s] %(name)s: %(message)s"
))

# Handler 2 — print only WARNING+ to the console
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.WARNING)
console_handler.setFormatter(logging.Formatter("%(levelname)s: %(message)s"))

logger.addHandler(file_handler)
logger.addHandler(console_handler)


logger.debug("low-level detail")          # filtered out everywhere
logger.info("normal event")               # goes to file only
logger.warning("something odd")           # goes to file AND console
```

Picture the logger as the *reporting clerk* who receives the message, and the handlers as separate dispatch belts — one feeding the permanent log file, one feeding the live console. Each belt has its own filter; not every report goes down every belt.

For beginner-to-intermediate work, you rarely set up handlers by hand. Most projects use a configuration file (YAML, JSON, or Python's `logging.config.dictConfig`) that describes the whole logging setup declaratively. The Python documentation has full examples; the *pattern* — logger → handler(s) → formatter — is the thing to remember.

---

## Logging Alarms — `logger.exception()`

A particularly useful idiom: inside an `except` block, calling `logger.exception(...)` logs *both* a message and the full alarm traceback at `ERROR` level automatically:

```python
import logging

logger = logging.getLogger(__name__)


def process(item):
    try:
        do_the_work(item)
    except Exception:
        logger.exception("Failed to process %s", item)
        # do not re-raise — handle gracefully or re-raise depending on need
```

Output (roughly):

```
ERROR:myapp:Failed to process item_42
Traceback (most recent call last):
  File "myapp.py", line 14, in process
    do_the_work(item)
  ...
ValueError: bad input
```

`logger.exception(msg)` is equivalent to `logger.error(msg, exc_info=True)`. The result is a log line with a full traceback attached — exactly what a developer reading the log later needs to diagnose what went wrong.

Use `logger.exception` whenever you catch an alarm and decide to handle it rather than re-raise. Without it, the traceback is lost and the cause is much harder to diagnose later.

---

## The Two Common Anti-Patterns

A short pair of warnings before the chapter closes.

**Anti-pattern 1: Logging at the wrong level.**

```python
# 🛑 Don't do this:
logger.info("Database connection refused — retrying")

# ✅ Use the right level:
logger.warning("Database connection refused — retrying")
```

The level is *information for the reader* about how alarmed to be. If you log every failed retry at `INFO`, an operator scanning the logs will see hundreds of normal-looking entries and might miss the real `WARNING` they care about. Be honest about severity.

**Anti-pattern 2: `print` mixed into a project that uses `logging`.**

In a real codebase, every module's reports should go through `logging`. Stray `print()` calls bypass the logging system entirely — they cannot be redirected to a file, cannot be filtered by level, cannot be tagged with their source. A single bare `print` in a quiet module makes a console window suddenly chatty in production.

The discipline: if your project uses `logging`, *use it everywhere*. Reserve `print` for direct user-facing output (CLIs that show information to the person running them) — and even there, send anything operational through the logger.

---

## What You Now Know

You have entered the CCTV Room — the small control room that watches every zone. You know `logging` is the right tool for any program intended to run more than once, and that `print` is the wrong tool for anything operational. You know the five severity levels and their monitor colours, and the rough question to ask when choosing one ("how alarmed should the operator be?").

You can write the minimum four-line setup (`import`, `basicConfig`, log calls), and you know the standard `logger = logging.getLogger(__name__)` pattern that every professional codebase uses. You can configure the format to include timestamps, severity, and source. You have seen handlers and formatters at a glance, and know that real applications usually load logging configuration from a file rather than building it by hand.

You can log alarms with their full traceback via `logger.exception()` — the essential pattern inside any `except` block that handles rather than re-raises.

The final two lessons — Advanced 6a and 6b — visit the Night Shift Wing, the deliberately last zone, where the factory runs more than one production line at a time.

---

## Quick Reference

| Python | CCTV image |
|---|---|
| `import logging` | Fetch the CCTV toolkit. |
| `logging.basicConfig(level=logging.INFO)` | Configure the Room — set the level floor. |
| `logging.info(msg)` etc. | Report to CCTV using the shared root logger. |
| `logger = logging.getLogger(__name__)` | The standard per-module logger pattern. |
| `logger.debug`, `.info`, `.warning`, `.error`, `.critical` | Five severity levels — dim / white / amber / red / flashing red. |
| `logger.exception(msg)` | Log a message *plus the full traceback* at ERROR level. Use inside `except`. |
| `logger.warning("Cache miss for %s", key)` | Use `%`-style placeholders, not f-strings — `logging` substitutes lazily. |
| `format="%(asctime)s [%(levelname)s] %(name)s: %(message)s"` | The standard four-field log line. |
| `FileHandler`, `StreamHandler` | Destinations — file and console. Each handler has its own level and formatter. |
| `logging.getLogger("module").setLevel(...)` | Tune one module's level independently. |
| Anti-pattern | Wrong severity; bare `print` mixed into a logging codebase. |

---

## Try It

**The minimum:**

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.info("Shift started.")
logging.warning("Something noteworthy.")
logging.error("Something went wrong.")
logging.critical("The program may not survive.")
```

Watch the output. `DEBUG` does not appear — it is below the configured level.

**Lazy formatting:**

```python
import logging

logging.basicConfig(level=logging.INFO)

name = "Alice"
score = 100
logging.info("Player %s scored %d", name, score)
```

The `%s`/`%d` substitution is done by `logging`, not by you. This is the preferred form for log messages.

**A more useful configuration:**

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)

logger = logging.getLogger("demo")
logger.info("Shift started.")
logger.warning("Cache miss.")
logger.error("Workstation failed.")
```

**Per-module logger:**

Save this as `mymodule.py`:

```python
# mymodule.py
import logging

logger = logging.getLogger(__name__)


def do_work():
    logger.info("doing work")
```

And run from another file:

```python
# main.py
import logging
import mymodule

logging.basicConfig(level=logging.INFO,
                    format="%(name)s — %(levelname)s — %(message)s")

mymodule.do_work()
```

Output:

```
mymodule — INFO — doing work
```

Notice the source `mymodule` is identified automatically.

**Logging an alarm with full traceback:**

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger("demo")


def safe_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        logger.exception("Cannot divide %d by %d", a, b)
        return None


safe_divide(10, 0)
```

Notice the full traceback appears after the log line — exactly what you would need to diagnose the issue from a log file alone.

**Two handlers — file and console:**

```python
import logging

logger = logging.getLogger("demo2")
logger.setLevel(logging.DEBUG)

file_handler = logging.FileHandler("demo.log")
file_handler.setLevel(logging.INFO)
file_handler.setFormatter(logging.Formatter(
    "%(asctime)s [%(levelname)s] %(message)s"
))

console_handler = logging.StreamHandler()
console_handler.setLevel(logging.WARNING)
console_handler.setFormatter(logging.Formatter("%(levelname)s: %(message)s"))

logger.addHandler(file_handler)
logger.addHandler(console_handler)


logger.debug("won't appear anywhere")
logger.info("appears in demo.log only")
logger.warning("appears in demo.log AND on console")
```

Open `demo.log` afterwards to see the file output; the console only shows the warning.

---

## Where Next?

Phase 6 is the deliberately-last phase — concurrency. Two lessons cover the Night Shift Wing: threading (two production lines running in parallel) and async (one worker, no idle time). Wait until the rest of the factory feels natural before opening the Night Shift door.

| Next lesson | Zone | Topic |
|---|---|---|
| Advanced 6a | Z10 — Night Shift Wing | Threading — two lines running in parallel |
| Advanced 6b | Z10 — Night Shift Wing | Async — one worker, no idle time |

*See the full lesson map in **The Factory — A Complete Map**.*
