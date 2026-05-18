# Python for Hyperphantasic Minds
## Lesson 22 — Error Handling

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 22 of 25  
> **Topic**: Error Handling — the inspection belt  
> **Factory zone**: Z7 — Quality Control  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Today you visit **Quality Control** — the building between the Factory Floor and Outgoings. Every alarm you have triggered throughout the series ends up here, sooner or later. Until now, when an alarm went off in your code, the program halted, the alarm message was printed, and the shift ended in mid-pass. This lesson teaches the factory's permanent procedure for *handling* alarms gracefully — catching them at the inspection belt, deciding what to do, and either fixing the problem or shutting down cleanly.

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
   ┌─────────────────────┼─────────────┐
   ▼                     ▼             ▼
┌─────────────────┐ ┌─────────┐  ┌──────────┐
│ ██ QUALITY ██   │ │ TESTING │  │ RECORDS  │
│   CONTROL       │ │   LAB   │  │  DEPT    │
│ YOU ARE HERE    │ └─────────┘  └──────────┘
└────────┬────────┘
         │
         ▼
  ┌──────────────┐
  │  OUTGOINGS   │
  └──────────────┘
```

The shift's emotional tone is calm. Alarms are not disasters. Alarms are *signals* — well-defined, named, and useful. Quality Control was designed precisely to receive them.

---

## The Inspection Belt

Picture a section of the production line, painted amber, between the Factory Floor and the dispatch bay. Material approaches the inspection belt; a sensor checks it for problems. If the material is clean, the belt forwards it on towards Outgoings. If an alarm has been triggered — somewhere upstream, by any workstation, at any depth of the call chain — the faulty item is **diverted** off the main belt into a handling station, where you decide what to do.

Three things happen in the canonical handler:

1. The alarm is **caught** — the program does not halt.
2. The faulty item is examined; you can read its details (which alarm, what message, which line).
3. A handling decision is made — repair, log, try again, give up cleanly, or re-raise to a higher handler.

The Python keywords are `try` (begin watching) and `except` (divert and handle).

---

## What an Alarm Actually Is

When a workstation cannot complete its job — because a scroll arrived in a stone slot, because a row was asked for position 12 when it had only 4, because a crate did not exist at the loading bay — it does not return silently. It **triggers an alarm**.

An alarm is a real Python item — a built workshop, in fact (Lesson 16). The alarm contains its kind (`TypeError`, `IndexError`, etc.), a human-readable message, and a *traceback* — a numbered row of every workstation that was active when the alarm fired, showing exactly where the procedure was halted.

If no handler catches the alarm, it propagates all the way up to the Shift Manager's Office, which has standing orders to print the traceback and end the shift. This is the bare default that you have been seeing whenever something has gone wrong.

The point of Quality Control is that *you* can choose to handle the alarm before it reaches the Shift Manager.

---

## Catching an Alarm — `try / except`

The minimal handler:

```python
try:
    value = int("not a number")
except ValueError:
    print("Could not convert that to a stone.")

print("Shift continues.")
```

Read this:

- `try:` — *begin watching the output of this block*. Indented lines are the watched region.
- `value = int("not a number")` — triggers `ValueError`. Without a handler, this would halt the program.
- `except ValueError:` — *divert any `ValueError` alarms that fire in the `try` block, and run this handler*.
- The indented block under `except` is the handling procedure.

Output:

```
Could not convert that to a stone.
Shift continues.
```

The shift was not halted. The alarm was caught, the message was printed, and the program continued past the entire `try/except` block. This is the *whole* point of Quality Control.

Two important rules:

- **The handler only fires for matching alarms.** `except ValueError:` will not catch a `TypeError`, an `IndexError`, or a `FileNotFoundError`. Each alarm kind has to be named explicitly (unless you catch a parent class — see below).
- **The handler runs immediately after the alarm fires.** The remaining lines in the `try` block are *skipped*. If the alarm fires on line 1 of the `try` block, lines 2 through N never run.

---

## The Error Vocabulary

The factory has a fixed set of named alarms, each with a canonical meaning. You have triggered most of them already during this series — usually as a deliberate "see it once and feel the alarm" exercise. Here is the full set worth knowing:

| Alarm | Canonical description |
|---|---|
| `SyntaxError` | The job order is illegible — the workstation cannot read it. |
| `NameError` | A name card has been pinned up but no cubbyhole exists with that label. |
| `TypeError` | A stone arrived at a slot designed for a scroll. The workstation cannot process it. |
| `IndexError` | The numbered row has seven positions. The job order asked for position twelve. |
| `KeyError` | The job order asked for drawer `player_name` but that drawer does not exist in the cabinet. |
| `ValueError` | The right kind of item arrived — a scroll — but its contents cannot be processed. |
| `AttributeError` | The job order asked for a built-in workstation that this workshop does not have. |
| `ImportError` | An order was sent to the Tool Store but the item is not on the shelves. |
| `ZeroDivisionError` | The divisor stone is marked zero. Division by zero is not a valid factory operation. |
| `FileNotFoundError` | A crate was expected at the loading bay but no lorry arrived. The file does not exist. |
| `RuntimeError` | Something went wrong during the shift that the program cannot recover from in the normal way. |

`SyntaxError` is special — the workstation could not read your job order at all, so the shift never starts. You almost never catch a `SyntaxError`; you fix it in the source. The other alarms can be caught and handled.

There are dozens of other named alarms in Python's standard library. Most of them are subclasses of the ones in the table — `FileNotFoundError` is a subclass of `OSError`, for example. The rule of thumb: if you do not know which specific alarm a workstation might trigger, look at its documentation. The docstring usually says.

---

## Catching Several Kinds of Alarm

Most real `try` blocks can be tripped by more than one kind of alarm. The handler can list several `except` clauses, in order:

```python
def safe_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        print("Cannot divide by zero.")
        return None
    except TypeError:
        print("Both inputs must be numbers.")
        return None


print(safe_divide(10, 2))           # 5.0
print(safe_divide(10, 0))           # Cannot divide by zero. → None
print(safe_divide(10, "x"))         # Both inputs must be numbers. → None
```

The handler clauses are checked *in order*. The first matching one fires; the rest are skipped — the same first-match-wins rule as the multi-gate junction in Lesson 13.

You can also catch multiple kinds in a single `except` clause using a sealed crate of alarm names:

```python
try:
    risky_operation()
except (ValueError, TypeError):
    print("Either kind of alarm — handled the same way.")
```

This is the right shape when several different alarms should be handled identically.

---

## Reading the Alarm's Details — `as`

Sometimes you want to see the alarm's message, not just respond to its kind. The `as` keyword captures the alarm item itself onto a shelf:

```python
try:
    crate = open("missing.txt", "r")
except FileNotFoundError as alarm:
    print("Could not open the crate.")
    print("Details:", alarm)
```

Output:

```
Could not open the crate.
Details: [Errno 2] No such file or directory: 'missing.txt'
```

`alarm` is the built workshop representing the alarm. Reading it as a scroll (which is what `print` does) returns its human-readable message. The shelf only exists inside the `except` block; after the block ends, the alarm shelf is cleared.

You will see the convention `except SomeError as e:` constantly in real code — `e` for *error*, or `exc` for *exception*. Both are universal abbreviations.

---

## `else` — Ran Only When No Alarm Fired

A `try` block can have an `else` clause that runs only if the watched region *finished without any alarms*. It is for code that should only run on the successful path:

```python
try:
    crate = open("notes.txt", "r")
except FileNotFoundError:
    print("File not found.")
else:
    contents = crate.read()
    crate.close()
    print(contents)
```

If the open fails, the `except` runs and the `else` is skipped. If the open succeeds, the `else` runs.

Why not just put `contents = crate.read()` inside the `try` block? Because then a *different* alarm fired by `crate.read()` would be caught by the same `except FileNotFoundError:` clause — and it shouldn't be. Putting the successful-path code in `else` keeps the `try` block narrow, watching only the operation you specifically want to catch.

This is one of Python's more subtle features. You will see it less often than `try/except`, but it has a place.

---

## `finally` — The Always-Runs Gate

The canonical visual: a small gate at the end of the inspection belt that *every item passes through*, whether it was approved or diverted. The `finally` clause is Python's always-runs gate:

```python
try:
    crate = open("notes.txt", "r")
    contents = crate.read()
except FileNotFoundError:
    print("File missing.")
finally:
    print("This always runs.")
```

The `finally` block runs whether the `try` block completed normally, whether an alarm was caught, or even whether an alarm was *not* caught (in which case `finally` runs *before* the alarm continues propagating upward). It is the right place for *cleanup* — releasing resources, closing connections, restoring state.

For file handling specifically, the `with` lockable room (Lesson 21) makes `finally` unnecessary in the everyday case — the lockable room already provides the always-runs cleanup. But `finally` remains the underlying primitive, and you will see it any time the cleanup logic does not fit into a `with`.

---

## Triggering an Alarm Manually — `raise`

The other side of Quality Control: *making* an alarm fire deliberately. The canonical verb is **trigger the alarm manually**; the Python keyword is `raise`:

```python
def withdraw(account, amount):
    if amount > account.balance:
        raise ValueError(f"Insufficient funds: balance is {account.balance}.")
    account.balance -= amount
```

Read `raise ValueError("...")` as: *"build a `ValueError` alarm with this message, and trigger it now"*. From the call site, the workstation has produced *not* a finished product, but a triggered alarm.

The caller can catch it like any other:

```python
try:
    withdraw(account, 1_000_000)
except ValueError as e:
    print("Withdrawal refused:", e)
```

`raise` is the right tool when your workstation cannot do its job and *the situation has a name*. Pick the alarm kind that best matches the failure — `ValueError` for "I got a value I cannot process", `TypeError` for "I got the wrong kind of item", `KeyError` for "asked for something not in a cabinet". The canonical names in the error vocabulary table above are the standard targets.

If none of the standard alarms fit your specific failure, you can build a custom one.

---

## Custom Alarms — Subclassing `Exception`

Every named alarm in Python is itself a workshop blueprint, descending ultimately from a root blueprint called `Exception`. You can extend it with a child blueprint of your own (Lesson 17), giving the new alarm a domain-specific name:

```python
class GameOverError(Exception):
    pass

def take_damage(player, amount):
    player.health -= amount
    if player.health <= 0:
        raise GameOverError(f"{player.name} has been defeated.")
```

Now any handler can catch it specifically:

```python
try:
    take_damage(alice, 200)
except GameOverError as e:
    print("Game over:", e)
```

The empty body (`pass`) is fine for most custom alarms — the name itself carries all the meaning. If you want to attach extra fields to the alarm, override `__init__` and call `super().__init__(message)`:

```python
class GameOverError(Exception):
    def __init__(self, player, message):
        super().__init__(message)
        self.player = player

try:
    raise GameOverError(alice, "Defeated.")
except GameOverError as e:
    print(e.player.name, "is out:", e)
```

Custom alarms are most useful in libraries — a `requests.ConnectionError`, a `sqlite3.OperationalError` — where the alarm is part of the library's public surface and naming it precisely helps users handle it.

---

## The Bare `except` Anti-Pattern

A tempting but bad pattern:

```python
try:
    risky_operation()
except:                          # ⚠ catches EVERYTHING
    print("Something went wrong.")
```

A bare `except:` (or the slightly less bad `except Exception:`) catches *every* alarm — including ones you did not expect. The two specific problems:

- **It swallows the diagnosis.** If a `NameError` happened because you misspelled a variable, the bare handler hides that fact. You see "Something went wrong" and have no idea what.
- **It catches alarms that should propagate.** `KeyboardInterrupt` (the user pressing Ctrl-C) is technically an alarm. A bare except will catch it, refusing to let your program quit when the user asks. So is `SystemExit`. So is *any* alarm a newer library raises that you have never heard of.

The right pattern is to catch *specific* alarms you know how to handle. If you really need to log every alarm before re-raising it, the form is:

```python
try:
    risky_operation()
except Exception as e:
    log(f"Operation failed: {e}")
    raise                        # re-raise the same alarm
```

`raise` with no argument inside an `except` block re-raises the alarm currently being handled. The handler logs the message and lets the alarm continue propagating upwards. This is the only common legitimate use of a catch-everything pattern.

---

## A Practical Example — Robust Reading

Putting most of this together — a workstation that reads a JSON crate, with full handling for the realistic failures:

```python
import json
from pathlib import Path


class ConfigError(Exception):
    """Raised when a configuration file cannot be loaded."""


def load_config(path):
    crate_path = Path(path)
    try:
        with crate_path.open("r") as crate:
            data = json.load(crate)
    except FileNotFoundError:
        raise ConfigError(f"Config not found at {path}")
    except json.JSONDecodeError as e:
        raise ConfigError(f"Config at {path} is not valid JSON: {e}")
    except PermissionError:
        raise ConfigError(f"No permission to read {path}")

    if "version" not in data:
        raise ConfigError(f"Config at {path} has no 'version' field")

    return data


try:
    config = load_config("settings.json")
    print("Loaded:", config)
except ConfigError as e:
    print("Could not load config:", e)
```

Three points worth noticing:

- `load_config` catches *each specific kind* of failure separately, converting them into a single domain-specific `ConfigError`. The caller does not need to know whether the underlying problem was a missing file, malformed JSON, or a permission issue — only that the config could not be loaded.
- The validation check (`if "version" not in data`) raises a `ConfigError` directly. *Validation failures are themselves alarms*, raised at the boundary of your code.
- The caller's `try/except` handles only `ConfigError`. Other alarms — a `MemoryError`, a `KeyboardInterrupt` — propagate freely. This is the *layered handler* pattern: each layer catches what it can handle and lets the rest pass.

---

## What You Now Know

You can wrap risky code in a `try` block and catch named alarms with one or more `except` clauses. You know `as` to read the alarm's details, `else` for the no-alarm-fired path, and `finally` for the always-runs gate that handles cleanup. You can trigger alarms manually with `raise`, and you know how to build custom alarms as subclasses of `Exception`.

You have seen the bare-`except` anti-pattern and know to catch specific alarms instead. You have seen the layered-handler pattern — each layer catches what it can, the rest passes through.

Most importantly, you know the alarms by name. Each one has a canonical physical description; when you see one fire, you can read it as a physical event rather than as cryptic technical text.

The factory's *emotional* tone around alarms is calm. They are signals. They are how the program tells you what went wrong. Quality Control is built specifically to handle them.

---

## Quick Reference

**The structure**

| Python | Quality Control image |
|---|---|
| `try:` `    body` | Begin watching the body for alarms. |
| `except SomeError:` | Divert items where `SomeError` fired; run this handler. |
| `except (A, B):` | Catch any of several alarm kinds with the same handler. |
| `except SomeError as e:` | Capture the alarm item onto a shelf for inspection. |
| `else:` | Runs only if the `try` body completed without any alarm. |
| `finally:` | Always-runs gate — runs whether the body succeeded, failed, or even propagated. |
| `raise ValueError(msg)` | Trigger the named alarm manually. |
| `raise` (in `except`) | Re-raise the alarm currently being handled. |
| `class MyError(Exception):` | Custom alarm — a child of `Exception`. |
| Bare `except:` | **Anti-pattern.** Catch specific alarms instead. |

**The alarms** — canonical descriptions

| Alarm | Meaning |
|---|---|
| `TypeError` | A stone arrived at a slot designed for a scroll. |
| `ValueError` | Right kind of item, wrong contents. |
| `IndexError` | Asked for position 12 of a 7-position row. |
| `KeyError` | Asked for a drawer not in the cabinet. |
| `NameError` | Reached for a shelf that does not exist. |
| `AttributeError` | Asked for a workstation the workshop does not have. |
| `ImportError` | Ordered from the Tool Store; not on the shelf. |
| `ZeroDivisionError` | Divisor stone is marked zero. |
| `FileNotFoundError` | Crate expected; no lorry arrived. |
| `RuntimeError` | Something else went wrong during the shift. |
| `SyntaxError` | Job order illegible. Fix the source — do not catch. |

---

## Try It

**Catch a single alarm:**

```python
try:
    value = int("not a number")
except ValueError:
    print("Could not convert.")
print("Continuing.")
```

**Two kinds in one handler:**

```python
def safe_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        print("Cannot divide by zero.")
    except TypeError:
        print("Both inputs must be numbers.")

safe_divide(10, 0)
safe_divide(10, "x")
```

**Reading the alarm's details:**

```python
try:
    open("nope.txt", "r")
except FileNotFoundError as e:
    print("Alarm fired.")
    print("Message:", e)
```

**`else` for the no-alarm path:**

```python
try:
    n = int("42")
except ValueError:
    print("Not a number.")
else:
    print("Successfully converted:", n)
```

**`finally` — always runs:**

```python
try:
    print("Working...")
    raise ValueError("intentional")
except ValueError as e:
    print("Caught:", e)
finally:
    print("Cleaning up — always runs.")
```

**Trigger an alarm manually:**

```python
def withdraw(balance, amount):
    if amount > balance:
        raise ValueError(f"Insufficient funds — balance is {balance}.")
    return balance - amount

try:
    new_balance = withdraw(50, 200)
except ValueError as e:
    print("Refused:", e)
```

**Custom alarm:**

```python
class GameOverError(Exception):
    pass

try:
    raise GameOverError("Hero defeated.")
except GameOverError as e:
    print("Game over:", e)
```

**The anti-pattern — see why it's a problem:**

```python
try:
    print(missing_shelf)        # triggers NameError
except:                              # bare except catches everything
    print("Something went wrong.")   # but tells you nothing useful
```

Now do it right:

```python
try:
    print(missing_shelf)
except NameError as e:
    print("Name not found:", e)
```

The named handler gives you the diagnostic. The bare one buries it.

---

## Where Next?

Quality Control is now part of your toolkit. Two of the supporting buildings remain — the Records Department for permanent storage with databases, and Goods In in depth for user input — before the closing lesson in the Shift Manager's Office.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 23 | Z8 — Records Dept | Databases — the permanent archive |
| Lesson 24 | Z2 — Goods In | User Input in Depth — work orders and terminals |
| Lesson 25 | Z4 — Shift Manager's Office | Putting It Together — the complete shift |
| Advanced 5a | Z6 — Testing Lab | Testing — station checks with pytest |

*See the full lesson map in **The Factory — A Complete Map**.*
