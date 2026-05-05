# Python for Hyperphantasic Minds
## The Factory — A Complete Map of Every Python Program

> **Series**: Python for Hyperphantasic Minds  
> **Document type**: Series overview — read this before any lesson  
> **Vocabulary standard**: VOCABULARY.md v0.1  
> **Purpose**: Every program you will ever write follows the same flow through the same building. This document is the architect's blueprint. Return to it whenever you need to locate yourself.

---

## Before You Enter

Pull back. Far back.

You are hovering above an entire industrial complex in the early morning light. It is larger than you first thought — not a single building but a campus of connected structures, each one with a distinct purpose, each one linked to the others by corridors, conveyor belts, and pneumatic tubes.

At the centre runs the main production line, left to right. But around it, connected and essential, stand supporting buildings that the main line could not function without.

This is every Python program you will ever write.

---

## The Full Complex

```
                    ┌──────────────────────────────────────────┐
                    │  Z4 — SHIFT MANAGER'S OFFICE             │
                    │  Program Structure & Orchestration        │
                    └──────────────────┬───────────────────────┘
                                       │ oversees all zones
               ┌───────────────────────┼────────────────────────────────┐
               │                       │                                 │
  ┌────────────┴───┐                   ▼                    ┌───────────┴────────┐
  │ Z1             │  ┌─────────────┐  ┌─────────────┐     │ Z9                 │
  │ TOOL STORE     │  │ Z2          │  │ Z3          │     │ CCTV ROOM          │
  │                │  │ GOODS IN    │  │ WAREHOUSE   │     │                    │
  │ pip · venv     │─▶│             │─▶│             │     │ logging            │
  │ import         │  │ input()     │  │ variables   │     │                    │
  │                │  │ files       │  │ data types  │     └────────────────────┘
  └────────────────┘  │ APIs        │  │ collections │          records all zones
    supplies all      └─────────────┘  └──────┬──────┘
    zones via import                          │
                                              ▼
                                   ┌──────────────────┐
                                   │ Z5               │
                                   │ FACTORY FLOOR    │
                                   │                  │
                                   │ functions        │
                                   │ loops            │
                                   │ if / else        │
                                   │ blueprints       │
                                   └────────┬─────────┘
                          ┌─────────────────┤
                          │                 │
               ┌──────────┴──────┐  ┌───────┴──────────┐
               │ Z6              │  │ Z8               │
               │ TESTING         │  │ RECORDS DEPT     │
               │ LABORATORY      │  │                  │
               │                 │  │ SQLite           │
               │ pytest          │  │ MySQL            │
               │ unittest        │  │ MongoDB          │
               └─────────────────┘  └──────────────────┘
                    pre-production        permanent storage

                                   ┌──────────────────┐
                                   │ Z7               │
                                   │ QUALITY CONTROL  │
                                   │                  │
                                   │ try / except     │
                                   │ validation       │
                                   └────────┬─────────┘
                                            │
                                            ▼
                                   ┌──────────────────┐
                                   │ Z11              │
                                   │ OUTGOINGS        │
                                   │                  │
                                   │ print()          │
                                   │ file writes      │
                                   │ return values    │
                                   └──────────────────┘

          ══════════════════════════════════════════════════════
                        Z10 — NIGHT SHIFT WING
                     async · threading · concurrency
          ══════════════════════════════════════════════════════
```

### The Main Production Line

The left-to-right flow through the heart of the complex:

```
[Goods In] ──▶ [Warehouse] ──▶ [Factory Floor] ──▶ [Quality Control] ──▶ [Outgoings]
   Z2              Z3               Z5                    Z7                  Z11
```

Everything else — Tool Store, Records Department, CCTV Room, Testing Laboratory, Night Shift Wing — is a supporting building connected to this main line.

---

## Z1 — The Tool Store

Before the factory opens for its first shift, someone has to stock it.

Picture a building on the far left of the complex, slightly set back from the main line. Its shelves run floor to ceiling, loaded with boxed equipment — tools, machines, specialist instruments — still in their packaging.

This is the **Tool Store**. In Python, it is `pip` — the Package Installer for Python — and the virtual environment system that keeps each factory's tool inventory separate from every other factory on the same site.

**Ordering Tools — pip install**

When you need a tool that didn't come with Python's standard kit, you order it from an enormous central catalogue called **PyPI** (the Python Package Index) — hundreds of thousands of items, all available on demand.

```bash
pip install requests
pip install numpy
pip install pygame
```

Each command is an order form. The tool arrives, is unpacked, and placed on the Tool Store shelf. It waits there until the factory needs it.

**Keeping Factories Separate — Virtual Environments**

If every factory on the site shares the same Tool Store, one factory installing version 1.0 of a tool will break another factory that needs version 2.0. The solution is a **virtual environment** — a private Tool Store built just for this factory.

```bash
python -m venv my_env        # build a private Tool Store for this factory
source my_env/bin/activate   # open for business — all installs go here
```

From this point, every `pip install` goes onto this factory's private shelves, not the shared ones.

**Fetching Tools to the Floor — import**

Tools sit in the Tool Store until a worker fetches them. That fetch happens at the top of every Python file:

```python
import math
import requests
from datetime import datetime
```

Picture a worker walking to the Tool Store at the start of the shift, collecting the required equipment, and carrying it back to their workstation. The tool is now available on the factory floor for the rest of the shift.

**The key rule of the Tool Store:** you cannot use a tool you haven't fetched. Forgetting to `import` a module is like leaving the equipment on the shelf and then wondering why the workstation can't find it.

---

## Z2 — Goods In

The loading bay on the left side of the main building. Wide roller doors, always open. This is where raw material arrives.

**Direct Delivery — input()**

A worker sits at a terminal beside the loading bay. When your program calls `input()`, a question appears on their screen. They type a response and press Enter. A scroll drops onto the intake conveyor and begins its journey into the factory.

```python
name = input("What is your name? ")
```

**Bulk Deliveries — Files**

Lorries back up to the dock carrying pre-packed crates. These are files — text documents, spreadsheets, CSV data — loaded from disk and brought in through the bay doors.

```python
file = open("data.txt", "r")
```

A crate arrives from the lorry and moves to the intake belt. The factory didn't create this material — it arrived from somewhere else, already assembled.

**Work Orders — sys.argv**

Sometimes the Shift Manager receives instructions before the shift even begins — command-line arguments passed directly when the program is launched. These arrive as a numbered row of scrolls at the loading bay door.

```python
import sys
filename = sys.argv[1]   # the first scroll in the row
```

**The key rule of Goods In:** raw material almost never arrives in the shape you need. A number typed at the keyboard arrives as a scroll of text, not a stone. One of the first jobs of every program is to take what arrives at the dock and reshape it for the warehouse. That reshaping — casting — happens at the warehouse door.

---

## Z3 — The Warehouse

Row upon row of cubbyholes, each labelled with a name card, each holding a single object. Everything is stored here between the moment it arrives and the moment it is needed on the factory floor.

```python
name = "Shane"
age = 27
score = 0
```

**What Lives on the Shelves**

Every object on a warehouse shelf has a specific physical form. These forms are consistent across every lesson and every diagram in this series:

| Physical object | Python type | Example values |
|---|---|---|
| **Stone** | `int` | `42`, `-7`, `1000000` |
| **Vial** | `float` | `3.14`, `-0.5`, `9.81` |
| **Scroll** | `str` | `"hello"`, `"Shane"`, `"Game Over"` |
| **Switch** | `bool` | `True`, `False` |
| **Numbered row** | `list` | `[1, 2, 3]`, `["a", "b", "c"]` |
| **Sealed crate** | `tuple` | `(10, 20)`, `("red", "blue")` |
| **Unsorted bin** | `set` | `{1, 2, 3}` |
| **Filing cabinet** | `dict` | `{"name": "Shane", "score": 27}` |
| **Vacant cubbyhole** | `None` | `None` |

**The Open Aisles and the Locked Rooms — Scope**

Not all of the warehouse is open aisle. Walk to the back of the building and you will find locked rooms — rooms that only certain workstations on the factory floor can enter.

The **open aisles** hold cubbyholes that any part of the factory can reach. These are accessible everywhere — for the duration of the shift.

The **locked rooms** hold cubbyholes that belong to a specific workstation. The moment that workstation finishes its job, the locked room is cleared completely. The cubbyholes that were inside it never existed, as far as the rest of the warehouse is concerned.

This system — which rooms are open and which are locked, and who holds the key — is called **scope**. It is not a limitation. It is what keeps a factory with ten thousand workstations from descending into chaos.

**The key rule of the Warehouse:** it is temporary. When the shift ends, the forklift clears every shelf. Stones lifted out, scrolls removed, name cards taken down. If data must survive after the program closes, it must be sent to the Records Department before the shift ends.

---

## Z4 — The Shift Manager's Office

Above the factory floor, behind a window that looks out over the entire complex, sits the Shift Manager.

The Shift Manager does not do any manufacturing work directly. Their job is **orchestration** — deciding what happens, in what order, when the shift begins, and when it ends. Without a Shift Manager, the factory has tools, materials, and workstations but no one directing the flow.

In Python, the Shift Manager's primary instruction is:

```python
if __name__ == "__main__":
    main()
```

Picture this as the Shift Manager's morning brief. When Python runs a file directly — when *this* factory is the one being opened for the day — the Shift Manager calls `main()` and the shift begins. If this file is being fetched by another factory as a tool, the Shift Manager stays silent. The brief only fires when this is the primary site.

**The Shift Manager also decides:**

- Which workstations run first, second, and last
- Whether this file is a complete program or a tool to be fetched by others
- How the overall program is structured across multiple files
- Entry points, work orders, and startup configuration

```python
import sys

def main():
    if len(sys.argv) < 2:
        print("Usage: program.py <filename>")
        return
    run(sys.argv[1])

if __name__ == "__main__":
    main()
```

Picture the Shift Manager reading the day's work order before briefing the floor. If the work order is missing required information, the Shift Manager calls a halt immediately rather than sending an incomplete order into production.

**The key rule of the Shift Manager's Office:** every Python program needs one point of control. Without it, your code is a collection of workstations with no production line connecting them.

---

## Z5 — The Factory Floor

The largest zone. Walk through the double doors and the scale of it becomes clear — machines running, conveyor belts moving, workstations arranged in precise configurations along the production line.

This is where your data gets *transformed*.

**Workstations — Functions**

A workstation is a sealed booth with an input slot on the front and an output slot on the side. Material is handed in; a finished product is sent back.

```python
def calculate_discount(price, percentage):
    return price - (price * percentage / 100)
```

You build a workstation once and send job orders to it a thousand times. The workstation doesn't care what shift it is or who sent the order — it performs the same transformation every time, on whatever arrives at the input slot.

**Conveyor Belts — Loops**

Some workstations don't process one item at a time — they process a whole numbered row, item by item, as the conveyor belt brings each one forward.

```python
for item in shopping_list:
    process(item)
```

A `for` loop is a belt that runs until the numbered row is empty. A `while` loop is a belt governed by a sensor — it keeps running while the sensor reads `True`:

```python
while stock > 0:
    dispatch_one_item()
    stock -= 1
```

**Junctions — If / Else**

A junction is a point on the factory floor where incoming material is examined and sent down one of several routes. Each route is clearly labelled.

```python
if age >= 18:
    grant_access()
elif age >= 16:
    restricted_access()
else:
    deny_access()
```

Picture a series of inspection gates in sequence. Each one reads the incoming item and either passes it through or diverts it. The first gate to open takes the item; subsequent gates never see it.

**Workshop Blueprints and Built Workshops — Classes**

Deep inside the factory, entire self-contained workshops — each with their own internal shelves, their own specialist workstations, and their own blueprint.

```python
class Vehicle:
    def __init__(self, make, speed):
        self.make = make
        self.speed = speed

    def accelerate(self):
        self.speed += 10
```

The **class** is the blueprint — a detailed technical drawing of how a workshop should be laid out, what shelves it has, and what built-in workstations it contains. The blueprint itself is not a workshop.

Each time you call `Vehicle()`, a new **built workshop** is constructed from that blueprint — its own shelves, its own readings, its own state. You can build a hundred workshops from one blueprint; each one is fully independent.

**Extended Blueprints — Inheritance**

A blueprint can be built on top of another blueprint, inheriting everything from the original while adding new shelves and workstations.

```python
class ElectricVehicle(Vehicle):
    def __init__(self, make, speed, battery):
        super().__init__(make, speed)
        self.battery = battery

    def charge(self):
        self.battery = 100
```

The `ElectricVehicle` blueprint extends the `Vehicle` blueprint. Every built workshop created from `ElectricVehicle` has everything a `Vehicle` workshop has — plus a battery shelf and a charging workstation. The original `Vehicle` blueprint is untouched.

**The key rule of the Factory Floor:** nothing here creates data from nothing. Every workstation transforms what the warehouse provides and sends results back to labelled shelves — or passes them directly to the next station on the line.

---

## Z6 — The Testing Laboratory

Off to the side of the factory floor, connected by a corridor but deliberately separated, is the Testing Laboratory.

Picture a clean room. White walls. Workstations identical to the ones on the factory floor, but isolated — no live production material passes through here. Instead, testers bring copies of workstations in here and run controlled station checks against them.

**What the Testing Laboratory Is Not**

The Testing Laboratory is not Quality Control. Quality Control runs during production — it handles defects in live material as it moves through the line. The Testing Laboratory runs *before* production — it verifies that each workstation does exactly what it is supposed to do, under every condition, before a single real item is processed.

**How It Works**

A tester takes a workstation and runs it against a series of known inputs with known expected outputs. If every station check passes, the workstation is cleared for the factory floor. If any check fails, it goes back to the engineers.

```python
def test_calculate_discount():
    assert calculate_discount(100, 10) == 90
    assert calculate_discount(200, 50) == 100
    assert calculate_discount(0, 10) == 0
```

Running the full set of station checks before the shift begins:

```bash
pytest
```

Every workstation that passes gets a green tick on the board. Any that fail are pulled aside before they can cause problems in production.

| | Testing Laboratory | Quality Control |
|---|---|---|
| **When** | Before the shift begins | During the shift |
| **What** | Workstations in isolation | Live material in transit |
| **Tools** | `pytest`, `assert` | `try/except`, validation |
| **Purpose** | Prove the workstation works | Handle what goes wrong anyway |

**The key rule of the Testing Laboratory:** a workstation you haven't tested is a workstation you don't fully understand. A station check proves it works — or proves it doesn't, before it causes damage in production.

---

## Z7 — Quality Control

Between the Factory Floor and Outgoings sits Quality Control — a bright inspection station that every finished item passes through before dispatch.

**The Inspection Belt — try / except**

```python
try:
    result = int(input("Enter a number: "))
    print(10 / result)
except ValueError:
    print("That wasn't a number.")
except ZeroDivisionError:
    print("Can't divide by zero.")
finally:
    print("Inspection complete.")
```

The `try` block is the main inspection belt. Python watches it run. If anything fails — a scroll arrives where a stone was expected, a division by zero is attempted, a file is missing — Quality Control intercepts the faulty item before it reaches Outgoings. The `except` block is the handling procedure. The `finally` block is the always-runs gate that every item passes through regardless of whether it passed or failed inspection.

**The Checkpoint — Validation**

Before material even reaches the Factory Floor, a technician at the warehouse door can run a quick check against the incoming item. Is it in the expected range? Is it the right kind of object?

```python
age = int(input("Enter your age: "))
if age < 0 or age > 150:
    print("That doesn't look like a valid age.")
```

Stopping bad material at the door is always cheaper than letting it cause problems deep inside the production line.

**The key rule of Quality Control:** a program that never crashes is not necessarily a good program. A good program fails *safely* — Quality Control intercepts the alarm and issues a clear, visible response rather than bringing the entire shift to a halt.

---

## Z8 — The Records Department

Set apart from the main production line, connected to the Warehouse and Outgoings by dedicated corridors, stands the Records Department.

Picture a vast archive. Floor-to-ceiling shelving — but unlike the warehouse, these shelves are not cleared at the end of the shift. The material here survives indefinitely. It is indexed, searchable, and retrievable — not a row of unlabelled cubbyholes but a catalogued library with a librarian who can find any record in milliseconds.

**Why the Warehouse Is Not Enough**

The warehouse clears at the end of every shift. If your program records ten thousand scores and then closes, those scores are gone. The Records Department stores them permanently and retrieves them instantly on demand.

**The Structured Archive — SQL Databases**

Everything stored in tables: rows and columns, like an enormous grid, with a query system that can locate any record instantly.

```python
import sqlite3

conn = sqlite3.connect("factory.db")
cursor = conn.cursor()
cursor.execute("CREATE TABLE IF NOT EXISTS scores (name TEXT, score INTEGER)")
cursor.execute("INSERT INTO scores VALUES (?, ?)", ("Shane", 150))
conn.commit()
```

A filing clerk receives a record, writes it onto a standardised form, and places it in the correct drawer. The `commit()` is the clerk closing and locking the drawer — the record is now permanent.

**The Unstructured Archive — Document Stores**

Not all records fit neatly into rows and columns. Some are irregular — one record might have five fields, the next might have fifty. A document store keeps each record as a self-contained package, filed without requiring every package to be the same shape.

```python
from pymongo import MongoClient

client = MongoClient()
db = client["factory"]
db.scores.insert_one({"name": "Shane", "score": 150, "level": 7})
```

**The key rule of the Records Department:** the warehouse is memory; the Records Department is history. Use the warehouse for data that lives and dies within a single shift. Use the Records Department for anything that must outlive the program.

---

## Z9 — The CCTV Room

High up in the corner of the complex, behind a door that most workers never open, is a room filled with monitors. Every camera in the complex feeds into it. Every event, every decision, every alarm, every successful dispatch — all of it recorded, time-stamped, and stored.

This is the `logging` module.

**CCTV vs Outgoings**

Calling out across the factory floor via `print()` is immediate and gone — if no one is listening, the words vanish. The CCTV Room writes everything to a permanent record that survives after the shift ends, that a manager can review tomorrow, that can be filtered by severity and by zone.

```python
import logging

logging.basicConfig(
    filename="factory.log",
    level=logging.DEBUG,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logging.info("Shift started.")
logging.warning("Stock running low.")
logging.error("Workstation calculate_discount received None — alarm triggered.")
```

Each message carries a severity level, visible as a colour on the monitors:

| Level | Monitor colour | Meaning |
|---|---|---|
| `DEBUG` | Grey | Detailed internal activity — for engineers only |
| `INFO` | Green | Normal operations — shift started, item processed |
| `WARNING` | Amber | Something unexpected but not critical |
| `ERROR` | Red | A workstation failed — production affected |
| `CRITICAL` | Flashing red | The factory is in serious trouble |

The CCTV Room receives feeds from every zone. It does not produce anything — it only records.

**The key rule of the CCTV Room:** `print()` is for development — for seeing what is happening right now. `logging` is for production — for knowing what happened at 3am when the program failed and no one was watching.

---

## Z10 — The Night Shift Wing

Walk to the far end of the complex. Through a set of heavy double doors is a separate wing — same machinery, same layout, but running on a completely different schedule.

This is the **Night Shift Wing**. It handles concurrency — multiple production lines running at the same time.

**The Problem**

The main factory runs one task at a time. Start a job, finish a job, start the next. This works well for most programs. But some jobs spend a lot of time waiting — waiting for a file to load, waiting for a web server to respond, waiting for a user to act. During that wait, the main factory floor stands idle.

**Threading — Multiple Lines Running in Parallel**

Picture two production lines running simultaneously, each with its own workers, each processing different material at the same time.

```python
import threading

def run_line_a():
    pass  # production line A

def run_line_b():
    pass  # production line B

thread_a = threading.Thread(target=run_line_a)
thread_b = threading.Thread(target=run_line_b)

thread_a.start()
thread_b.start()
```

Both lines run at the same time. Line A does not wait for Line B to finish before it begins.

**Async — One Worker, No Idle Time**

Threading gives you multiple workers. Async gives you one highly efficient worker who never stands idle — the moment one task is waiting for a response, the worker immediately picks up the next job on the bench.

```python
import asyncio

async def fetch_data():
    await asyncio.sleep(1)  # waiting for a response
    return "data"

asyncio.run(fetch_data())
```

**The key rule of the Night Shift Wing:** do not enter this wing until the main factory is running smoothly. Concurrency adds significant complexity. A fault in a single-line factory is hard enough to trace. A fault in a multi-line factory can be invisible, intermittent, and deeply disorienting.

---

## Z11 — Outgoings

The right-hand side of the complex. A clean dispatch bay where finished goods are packaged, labelled, and sent out into the world.

This is where your program delivers its results. Every item that makes it through Quality Control arrives here. Nothing leaves the factory without passing through this zone.

**Calling Out Across the Floor — print()**

The simplest dispatch. A worker reads the label on a finished item and calls it out clearly.

```python
print("Hello, world!")
print(f"Welcome, {name}. Your score is {score}.")
```

`print()` is immediate and visible — the words appear on the screen and the moment passes. They do not persist after the program closes. If you need the output to survive, it must be written to a file or sent to the Records Department.

**Packing for the Depot — File Writes**

For output that must survive after the shift ends, the dispatch team writes to a file — packs the finished item into a crate, labels it, and sends it to the depot.

```python
with open("results.txt", "w") as f:
    f.write(f"{name}: {score}\n")
```

Picture the packing team sealing a crate and sending it to long-term storage. Long after the factory shift ends, the crate is still there.

**Internal Dispatch — return**

Not all finished products leave the building. Some are sent back along the internal belt to whoever placed the job order — another workstation waiting for the result.

```python
def square(number):
    return number * number

result = square(5)
```

The `square` workstation receives a stone marked `5`, processes it, and sends back a finished product — a stone marked `25` — along the internal belt to the `result` shelf in the warehouse. It does not go to the screen. It does not go to the depot. It goes directly to whoever placed the job order.

**The key rule of Outgoings:** if your program produces no output, the world sees nothing. A factory that manufactures goods and never dispatches them has done invisible work. `print()`, file writes, and sent-back products are the three dispatch mechanisms. Without at least one of them, your program runs, produces a result, and the result disappears when the shift ends.

---

## The Complete Map

| Zone ID | Zone | Python concepts | Lessons |
|---|---|---|---|
| Z1 | **Tool Store** | `pip`, `venv`, `import` | Lesson 20 |
| Z2 | **Goods In** | `input()`, file reads, `sys.argv` | Lessons 21, 24 |
| Z3 | **Warehouse** | Variables, data types, collections, scope | Lessons 1–11 |
| Z4 | **Shift Manager's Office** | `if __name__ == "__main__"`, `main()`, `sys` | Lesson 25 |
| Z5 | **Factory Floor** | Functions, loops, if/else, blueprints, operators | Lessons 8, 12–19 |
| Z6 | **Testing Laboratory** | `pytest`, `unittest`, `assert` | Advanced Phase 5 |
| Z7 | **Quality Control** | `try/except`, `finally`, validation | Lesson 22 |
| Z8 | **Records Department** | SQLite, MySQL, MongoDB, file writes | Lessons 21, 23 |
| Z9 | **CCTV Room** | `logging` module | Advanced Phase 5 |
| Z10 | **Night Shift Wing** | `threading`, `asyncio` | Advanced Phase 6 |
| Z11 | **Outgoings** | `print()`, file writes, `return` | Lessons 2, 8 |

---

## The Flow, Fully Drawn

A complete Python program — not a toy script but a real application — touches most of these zones every time the shift begins:

1. The **Tool Store** (Z1) is stocked before the shift. Tools are fetched via `import` at the start of every file.
2. The **Shift Manager** (Z4) reads the work order and calls `main()`. The shift begins.
3. **Goods In** (Z2) receives raw material — scrolls from the keyboard, crates from files, work orders from the command line.
4. The **Warehouse** (Z3) logs and stores everything under named labels on the shelves.
5. The **Factory Floor** (Z5) transforms the material — workstations, conveyor belts, junctions, workshop blueprints.
6. The **Testing Laboratory** (Z6) has already verified, before this shift ever began, that every workstation works correctly.
7. **Quality Control** (Z7) watches the live line and intercepts anything that goes wrong before it reaches dispatch.
8. The **Records Department** (Z8) stores anything that needs to outlive this shift — permanently, indexed, retrievable.
9. The **CCTV Room** (Z9) records everything from every zone — for the engineers who will review the logs later.
10. **Outgoings** (Z11) dispatches the finished result — called out to screen, packed into a file, or sent back along the internal belt.
11. The **Night Shift Wing** (Z10) runs parallel lines when the program needs to do more than one thing at once.

---

*You now have the complete map. Every lesson in this series will show you which zone you are entering before you step inside. The factory is always the same building. You are simply learning, one zone at a time, what each part of it does.*

*Start at Lesson 1 — The Warehouse (Z3) — if you haven't already.*
