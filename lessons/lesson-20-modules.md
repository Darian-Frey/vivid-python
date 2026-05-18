# Python for Hyperphantasic Minds
## Lesson 20 — Modules and Packages

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 20 of 25  
> **Topic**: Modules and Packages — stocking and fetching  
> **Factory zone**: Z1 — The Tool Store  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

For the first time you leave the main production line and visit a side building. The **Tool Store** is on the far side of the factory complex — a small but vital building stocked with toolkits, packaged equipment, and machinery you can fetch onto the Floor when needed. Every program you write from here on will be visiting the Tool Store at the start of its shift.

```
                    ┌──────────────────────────────┐
                    │    SHIFT MANAGER'S OFFICE     │
                    └──────────────┬───────────────┘
                                   │
┌────────────────────┐             ▼        ┌─────────────────┐
│                    │  ┌──────────┐        │                 │
│ ██ TOOL STORE ██   │─▶│ GOODS IN │        │  FACTORY FLOOR  │
│                    │  │          │        │                 │
│    YOU ARE HERE    │  └────┬─────┘        └────────┬────────┘
└────────────────────┘       │                       │
                             ▼                       │
                  ┌──────────────────────┐           │
                  │      WAREHOUSE       │◀──────────┘
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

Two related but distinct operations live here. **Fetching** — bringing a toolkit that already sits on the Tool Store's shelves onto the Factory Floor where your code can use it. And **ordering** — having a new toolkit delivered to the Tool Store from the world outside, so it can be fetched later. These are different actions. Mixing them up is one of the first mistakes new Python programmers make.

---

## The Tool Store — Physical Setup

Picture a side building with high shelves lining every wall. The shelves are organised by category — mathematics, randomness, dates and times, file operations, network requests, image processing, machine learning, web frameworks. Each shelf section holds **toolkits** (which Python calls *modules*) — bundles of pre-built workstations, ready to be fetched and used.

Some shelves are stocked from the moment your factory is built. These are Python's **standard library** — `math`, `random`, `datetime`, `os`, `sys`, `json`, `re`, and dozens more. They came with the factory and are always available; you just have to fetch them when you need them.

Other shelves are *empty* by default. To stock them, you order toolkits from the outside world — from **PyPI**, the global Python package warehouse where developers worldwide publish their toolkits. Once delivered to the Tool Store, an ordered toolkit becomes fetchable like any other.

---

## Fetching a Toolkit — `import`

The canonical verb is **fetch from the Tool Store**. The Python keyword is `import`.

```python
import math

print(math.sqrt(16))        # 4.0
print(math.pi)              # 3.141592653589793
print(math.floor(3.7))      # 3
```

Read this:

- `import math` — *fetch the `math` toolkit from the Tool Store*. After this line, the toolkit sits on the Floor under the name `math`.
- `math.sqrt(16)` — *use the `sqrt` workstation that came with the toolkit*. The dot syntax is the same as for built workshops in Lesson 16: *"the `sqrt` workstation of the `math` toolkit"*.
- `math.pi` — the toolkit also exposes shelves, not just workstations. `pi` is a vial-shape constant.

The fetch happens *once*, the first time the program runs that `import` line. Subsequent imports of the same toolkit during the same shift just give you the same already-fetched copy — Python is sensible enough not to re-fetch what it already has.

Conventionally, every `import` lives at the top of your program file, in a block, before any other code. This is not enforced — you can `import` in the middle of a function if you need to — but the top-of-file convention is what every Python style guide recommends.

---

## Selective Fetching — `from ... import ...`

When you only need a few things from a toolkit, you can fetch them by name and skip the toolkit's namespace:

```python
from math import sqrt, pi

print(sqrt(16))             # 4.0     — no math. prefix needed
print(pi)                   # 3.141592653589793
```

`from math import sqrt, pi` reads: *"from the `math` toolkit, fetch `sqrt` and `pi` directly onto the Floor"*. The toolkit itself is not brought across; only the named items are.

This is most useful for things you call repeatedly — the shorter form is more readable when the same name appears many times. But it has a small cost: a reader of your code can no longer tell, at a glance, which toolkit `sqrt` came from. The plain `import math; math.sqrt(...)` form is clearer about origins; the `from ... import ...` form is shorter at the use site. Both forms are common; pick based on context.

---

## Renaming on Fetch — `as`

A toolkit's name on the Floor does not have to match its name in the Tool Store. The `as` keyword lets you rename on arrival:

```python
import numpy as np

# Now you say:
arr = np.array([1, 2, 3])     # instead of numpy.array
```

`import numpy as np` is a convention so strong that almost every Python program working with arrays writes `np` rather than `numpy`. Similarly `import pandas as pd`, `import matplotlib.pyplot as plt`. The convention exists because these toolkit names are used so heavily that even four extra characters per call would clutter the code.

`as` works in the selective form too:

```python
from math import sqrt as square_root
print(square_root(16))      # 4.0
```

Use `as` sparingly. The convention shortcuts (`np`, `pd`, `plt`) are well known and a reader will recognise them. Random renamings of your own — `from math import sqrt as my_sqrt` — just make the code harder for someone else to read.

---

## The Star Import — and Why to Avoid It

You will see this form in old tutorials and beginner books:

```python
from math import *
```

This fetches *everything* from the `math` toolkit and dumps it all onto the open aisle of the warehouse. After this line, `sqrt`, `pi`, `floor`, `cos`, `sin`, and several dozen other names are all sitting directly on the Floor.

This is rarely a good idea. Two problems:

- **Hidden shadowing.** If any of the fetched names collides with something you already have on a shelf — or with a built-in name (Lesson 9's shadow trap) — that earlier item is silently overwritten. The damage is often invisible until something breaks much later.
- **Hidden origins.** A reader looking at `sqrt(16)` somewhere in your code has no easy way to find out which toolkit it came from. Two `from x import *` lines at the top of the file make this even worse.

In practice, almost no professional Python code uses star imports. Use named imports (`from math import sqrt, pi`) or the prefixed form (`import math`). When you encounter star imports, recognise the smell.

---

## The Standard Toolkit Set

A small tour of the toolkits Python comes pre-stocked with. You will not memorise these; recognise them, and look up the rest when you need them.

| Toolkit | Use |
|---|---|
| `math` | Mathematical functions and constants (`sqrt`, `sin`, `pi`, `log`). |
| `random` | Random numbers and choices (`random.randint`, `random.choice`, `random.shuffle`). |
| `datetime` | Dates, times, durations. |
| `os` | Operating-system interactions — paths, environment variables. |
| `os.path` | File and directory path manipulation. |
| `pathlib` | A more modern alternative to `os.path`. |
| `sys` | The Python interpreter itself — command-line arguments, exit codes. |
| `json` | Encode and decode JSON for files and APIs. |
| `re` | Regular expressions for pattern matching in scrolls. |
| `csv` | Reading and writing comma-separated values. |
| `collections` | Specialised collection items (`Counter`, `defaultdict`, `deque`). |
| `itertools` | Helpers for working with iterators and generators. |
| `functools` | Function-related tools (`cache`, `wraps`, `reduce`). |
| `time` | Sleeping, measuring elapsed time. |

There are many more. The full list lives in the official Python documentation. Treat the table above as a starting awareness, not a list to memorise.

Quick example with `random`:

```python
import random

print(random.randint(1, 6))           # a fair die roll: 1..6
print(random.choice(["yes", "no"]))   # one of the two
print(random.random())                # vial between 0.0 and 1.0
```

---

## Ordering a New Toolkit — `pip install`

The Tool Store's shelves do not have everything. For specialised work — a web framework, a scientific library, a CLI tool, an image processor — you need to **order** the toolkit from PyPI, the public repository of Python packages.

This is **not** a Python operation. It happens at the *terminal*, before the program runs:

```
$ pip install requests
```

After running this command in the terminal (or `python -m pip install requests`), the `requests` toolkit is delivered to the Tool Store. From that moment forward, your Python programs can fetch it:

```python
import requests
response = requests.get("https://example.com")
```

Two operations, two completely different scopes:

- **Ordering (`pip install package`)** — a terminal command. Stocks the Tool Store with a toolkit it did not previously have. Happens once, before the program runs. Persists for the life of the Python installation.
- **Fetching (`import package`)** — a Python statement. Brings an already-stocked toolkit from the Tool Store onto the Floor for use. Happens every time the program runs.

If you try to `import` a toolkit you have not ordered, Python triggers `ModuleNotFoundError`. The fix is at the terminal, not in the source file — order the toolkit, then re-run the program.

The standard library toolkits — `math`, `random`, etc. — come pre-ordered. You never `pip install math`; it was always there.

---

## A Private Tool Store — `venv`

Imagine two factories run by the same company, both using the same Tool Store. Factory A needs version 1 of a particular toolkit; Factory B needs version 2. If they share the same Tool Store, only one version can be stocked at a time. Both factories cannot have what they need.

Python solves this with **virtual environments**. The canonical verb is **build a private Tool Store for this factory**:

```
$ python -m venv .venv
$ source .venv/bin/activate          # Linux/macOS
$ .venv\Scripts\activate             # Windows
```

The first line builds a fresh, private Tool Store inside a folder called `.venv` in your project. The second line activates it — every subsequent `pip install` stocks *this* private store, not the system-wide one. Every subsequent `python` invocation looks at *this* private store when it fetches toolkits.

Deactivate when you are done:

```
$ deactivate
```

Why this matters: every serious Python project should use its own virtual environment. Different projects can need different versions of the same toolkit; some toolkits even need different versions of *Python itself*. A shared system Tool Store cannot serve them all. A private store per project does.

A common convention is to keep a `requirements.txt` file listing the project's ordered toolkits:

```
requests==2.31.0
numpy>=1.24
flask
```

A new clone of the project can rebuild the private Tool Store from this list with one command:

```
$ pip install -r requirements.txt
```

We will not depend on virtual environments anywhere else in this series, but every program you write outside the tutorial should use one. The habit is worth forming early.

---

## Writing Your Own Toolkit

Any Python file is itself a toolkit. Save the following as `mathutil.py`:

```python
# mathutil.py
PI = 3.14159

def circle_area(r):
    return PI * r * r

def circle_circumference(r):
    return 2 * PI * r
```

From another Python file in the same folder, you can fetch your own toolkit:

```python
# main.py
import mathutil

print(mathutil.circle_area(10))           # 314.159
print(mathutil.PI)                        # 3.14159
```

Or selectively:

```python
from mathutil import circle_area, PI

print(circle_area(10))
print(PI)
```

Your own file behaves exactly like a standard-library toolkit. The name on the Tool Store's shelf is the filename (minus `.py`). Anything you would normally write inside a Python file — workstations, blueprints, constants — is available to anyone who fetches the toolkit.

A common pattern at the bottom of a module: only-run-when-run-directly code:

```python
# mathutil.py
PI = 3.14159

def circle_area(r):
    return PI * r * r

if __name__ == "__main__":
    print(circle_area(10))
```

The `if __name__ == "__main__":` block runs only when this file is executed directly (`python mathutil.py`). When the file is *fetched* as a toolkit by another file, that block does not run. The convention is universal in Python: code that *uses* the module's contents goes inside that block; the module's reusable definitions go outside it. We will see this in detail in Lesson 25.

---

## Packages — Folders Full of Toolkits

When a single file becomes too large, the toolkit can grow into a **package** — a folder containing several `.py` files. The folder name becomes the package name; the files inside are its submodules.

The classic structure:

```
mypackage/
    __init__.py
    submod_a.py
    submod_b.py
```

The `__init__.py` file (which may be empty) tells Python *"this folder is a package, not just a folder of unrelated files"*. From elsewhere:

```python
import mypackage.submod_a
from mypackage import submod_b
from mypackage.submod_a import some_workstation
```

You will see deeply nested packages in real projects — `flask.json.tag` is a submodule of a submodule of a top-level package. The fetching syntax extends naturally: dots in the import path correspond to folder levels in the package.

Writing your own packages is rarely needed at the beginner stage. Recognise the structure when you see it; use it when a single file is genuinely getting too long.

---

## What You Now Know

You can fetch toolkits from the Tool Store with `import`, fetch selectively with `from ... import ...`, and rename on fetch with `as`. You know to avoid star imports. You know the standard library has dozens of toolkits pre-stocked, and the most commonly used ones by name.

You can order new toolkits from PyPI with `pip install`, and you know that ordering and fetching are two completely different operations — one is a terminal command that stocks the Tool Store, the other is a Python statement that brings a stocked toolkit onto the Floor. You know that every serious project should use a private Tool Store (`venv`).

You can write your own toolkit (any `.py` file), use the `if __name__ == "__main__":` convention to separate library code from script code, and recognise the structure of packages.

The next lesson moves to the loading bay — **file handling**, which means receiving crates at Goods In and sending packages to the Records Department.

---

## Quick Reference

| Python or Terminal | Tool Store image |
|---|---|
| `import math` | Fetch the `math` toolkit from the Tool Store. Use as `math.sqrt`. |
| `from math import sqrt, pi` | Fetch named items directly onto the Floor. |
| `import numpy as np` | Fetch and rename on arrival. |
| `from math import sqrt as sr` | Fetch a named item and rename. |
| `from math import *` | **Avoid.** Hidden shadowing; hidden origins. |
| `$ pip install requests` | Order a new toolkit from PyPI. Terminal command, not Python. |
| `$ pip install -r requirements.txt` | Order everything listed in a requirements file. |
| `$ python -m venv .venv` | Build a private Tool Store for this project. |
| `$ source .venv/bin/activate` | Activate the private Tool Store (Linux/macOS). |
| `$ .venv\Scripts\activate` | Activate on Windows. |
| `$ deactivate` | Return to the system Tool Store. |
| Any `.py` file | Is itself a fetchable toolkit. |
| `if __name__ == "__main__":` | The script-vs-library boundary. |
| `mypackage/__init__.py` | Marks a folder as a package. |

---

## Try It

**Fetch and use the math toolkit:**

```python
import math

print(math.sqrt(16))
print(math.pi)
print(math.floor(3.7))
print(math.ceil(3.2))
```

**Selective fetch:**

```python
from math import sqrt, pi
print(sqrt(25))
print(pi)
```

**Renaming:**

```python
from math import sqrt as square_root
print(square_root(81))
```

**Try the `random` toolkit:**

```python
import random

print(random.randint(1, 6))               # a die roll
print(random.choice(["apple", "bread", "milk"]))
print(random.random())
random.shuffle([1, 2, 3, 4, 5])           # in-place shuffle
```

**Order a toolkit from PyPI** (at the terminal, not in Python):

```
$ pip install requests
```

Then in Python:

```python
import requests
# ... use it
```

If the import fails with `ModuleNotFoundError`, you have not ordered the toolkit yet. Fix at the terminal.

**Build a private Tool Store** (at the terminal):

```
$ python -m venv .venv
$ source .venv/bin/activate          # or .venv\Scripts\activate on Windows
$ pip install requests
$ deactivate
```

You can repeat this in any project folder. Each project gets its own toolkits, isolated from every other.

**Write your own toolkit:**

Create a file `mathutil.py`:

```python
# mathutil.py
PI = 3.14159

def circle_area(r):
    return PI * r * r
```

And in `main.py` (same folder):

```python
# main.py
import mathutil
print(mathutil.circle_area(10))
```

Run `python main.py`. Your own toolkit was fetched and used.

---

## Where Next?

The next lesson moves to the loading bay at the side of the building — **file handling**. Reading crates that arrive, writing crates that are sent out. The lesson visits two zones at once: Goods In (for file reads) and the Records Department (for file writes).

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 21 | Z2, Z8 — Goods In, Records Dept | File Handling — crates at the loading bay |
| Lesson 22 | Z7 — Quality Control | Error Handling — the inspection belt |
| Lesson 23 | Z8 — Records Dept | Databases — the permanent archive |
| Lesson 24 | Z2 — Goods In | User Input in Depth — work orders and terminals |

*See the full lesson map in **The Factory — A Complete Map**.*
