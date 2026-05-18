# Python for Hyperphantasic Minds
## Lesson 16 — Classes

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 16 of 25  
> **Topic**: Classes — workshop blueprints and built workshops  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still on the Factory Floor. The lessons so far have given you workstations (Lesson 8) that take in materials and send back finished products, and conveyor belts (Lesson 14) that feed many items through the same procedure. Today the Floor gains its largest construct yet: the **workshop blueprint** — Python's word for *class* — and the **built workshops** (instances) that are built from it.

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

This lesson also introduces vocabulary that earlier lessons deferred — **methods**, **attributes**, the formal terms for "the workstations and shelves attached to a built workshop". You will recognise both: methods are the *built-in workstations* you have been calling on scrolls and rows since Lesson 6 (`name.upper()`, `row.append()`), attributes are the *shelves attached to a workshop*. The full machinery is finally here.

---

## The Workshop Blueprint

Picture an architect's office at the side of the Factory Floor. On its wall is a large pinned drawing — a **workshop blueprint** — showing every detail of a particular kind of workshop:

- The shelves it will contain, with their labels.
- The built-in workstations that will be pre-installed on its floor.
- The procedure to follow when *fitting out* a new workshop from this blueprint — what goes on which shelf at construction time.

The blueprint is not itself a workshop. You cannot send a job order to a blueprint. The blueprint is a plan. To do work, you take the blueprint, walk onto the Factory Floor, and have a small physical workshop built from it. Each workshop built from the same blueprint shares the same layout — the same shelves, the same workstations — but each one is a separate physical structure on the Floor with its own contents.

This is the entire architecture of every Python class. The class is the blueprint; an *instance* is a built workshop.

Why use them? Two reasons:

- **Bundling**. A built workshop bundles a set of related shelves *and* the workstations that operate on them. Instead of a loose row of stones and three workstations that each take the row as an argument, you have a single workshop whose internal shelves hold the stones and whose built-in workstations operate on those shelves directly.
- **Many at once**. Once a blueprint exists, building a hundred workshops from it costs only a hundred construction calls. Each workshop has its own shelves; they do not interfere with one another. This is the natural shape for *records with behaviour* — a player in a game, a connection to a server, a row in a database with attached operations.

---

## Drafting a Blueprint — `class`

To draft a blueprint, give Python the keyword `class`, a name, and the indented procedure describing the layout. The simplest possible blueprint:

```python
class Player:
    pass
```

Read this:

- `class Player:` — *draft a blueprint called `Player`*. The convention is PascalCase for blueprint names (`Player`, `BankAccount`, `LogEntry`).
- `:` — what follows is the blueprint's contents, indented.
- `    pass` — the body is empty (`pass` is Python's "do nothing" placeholder; required because Python expects *something* indented after the colon).

That is a complete blueprint. It draws a workshop with no shelves and no built-in workstations — empty, but real. Drafting it does not build any workshop; it just files the plan.

---

## Building a Workshop From the Blueprint

To build a workshop, *send a job order to the blueprint itself*. Python treats the blueprint's name as a workstation that, when called, produces a freshly built workshop on the Floor:

```python
alice = Player()
bob = Player()
```

Two workshops have been built from the `Player` blueprint. The first is on the shelf `alice`; the second on the shelf `bob`. They are two separate physical structures on the Factory Floor. Each one came off the same plan — but they are independent.

This action is called **building a workshop from the blueprint** throughout the tutorial. The Python community has its own formal vocabulary for the same act, which you will meet when you read other documentation — but the physical event is the same: a fresh workshop comes off the blueprint and onto the Floor.

You can place shelves on a built workshop directly, using a dot:

```python
alice.name = "Alice"
alice.score = 0

bob.name = "Bob"
bob.score = 50

print(alice.name, alice.score)         # Alice 0
print(bob.name, bob.score)             # Bob 50
```

Each shelf belongs to *that specific built workshop*, not to the blueprint and not to the other workshop. Setting `alice.name` does not change `bob.name`.

These shelves are called **attributes** — the formal Python word for *shelves attached to a built workshop*. The dot syntax `alice.name` reads as *"the `name` attribute of `alice`"* — exactly the pattern you have been using on scrolls and rows since Lesson 6.

Setting attributes one-by-one after building is permitted, but it is rarely how real Python code works. The natural pattern is to have the blueprint *prescribe* which shelves are installed when the workshop is built — and that is where `__init__` comes in.

---

## `__init__` — Fitting Out the Workshop

A blueprint can include a procedure named `__init__` that runs once for every new workshop, immediately after it is built. The procedure's job is to *fit out the workshop* — set its initial shelves to their starting contents.

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0
        self.health = 100
```

Picture: the workshop has just been built on the Factory Floor. The fit-out crew arrives with the blueprint and runs the procedure — placing a scroll on the `name` shelf (with the value handed in at build time), placing a `0` stone on the `score` shelf, placing a `100` stone on the `health` shelf. When the crew leaves, the workshop is ready for work.

The double underscores around `__init__` mark it as a *Python-internal* name. Python calls it automatically the moment a workshop is built. You almost never call it yourself by hand. Such names — `__init__`, `__str__`, `__repr__`, and others — are sometimes pronounced *"dunder init"*, *"dunder str"*, with the prefix *dunder* short for *double underscore*.

To build a workshop now, the call passes the materials needed by `__init__`:

```python
alice = Player("Alice")
bob = Player("Bob")

print(alice.name, alice.score, alice.health)    # Alice 0 100
print(bob.name, bob.score, bob.health)          # Bob 0 100
```

The `"Alice"` scroll was delivered as `name` material into `__init__`. The fit-out crew used it to label the workshop's `name` shelf. The other shelves were filled with their hard-coded defaults.

---

## `self` — This Workshop

The first parameter of `__init__` — and of *every* built-in workstation — is conventionally called `self`. It is not a magic Python keyword; it is just a slot label, like `name` or `score`. But the convention is universal.

`self` refers to **this workshop** — the specific built workshop currently being operated on. When the fit-out crew runs `__init__` for `alice`, `self` is `alice`. When they run `__init__` for `bob`, `self` is `bob`. The same procedure runs each time, but `self` points at a different physical workshop on each call.

The line `self.name = name` reads as: *"place the value on this workshop's `name` shelf"*. The left side (`self.name`) names the shelf inside *this* workshop; the right side (`name`) is the value delivered to the fit-out procedure's input slot.

Picture two simultaneous fit-outs happening for two different workshops:

```
Fit-out for alice:                  Fit-out for bob:
  self  is alice                       self  is bob
  name  is "Alice"                     name  is "Bob"
  self.name = name                     self.name = name
  → places "Alice" on                  → places "Bob" on
    alice's name shelf                   bob's name shelf
```

Two procedures, two workshops, two distinct shelves. The `self` slot is what keeps the two fit-outs independent.

---

## Built-In Workstations — Methods

A blueprint can also include workstations that come pre-installed on every workshop built from it. These are written as `def` blocks inside the blueprint, with `self` as their first parameter.

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0
        self.health = 100

    def score_points(self, points):
        self.score += points

    def take_damage(self, amount):
        self.health -= amount

    def is_alive(self):
        return self.health > 0
```

Three new workstations join `__init__` inside the blueprint: `score_points`, `take_damage`, `is_alive`. Each comes with its own input slot list (after `self`) and its own procedure.

These pre-installed workstations are called **methods** in formal Python vocabulary. From this lesson onwards, *method* is no longer banned in the tutorial — but the visual to keep is **built-in workstation**: a workstation that sits inside the workshop and operates on that workshop's own shelves.

To send a job order to one of these workstations, use the same dot syntax you have been using since Lesson 6:

```python
alice = Player("Alice")
alice.score_points(50)
alice.take_damage(20)

print(alice.score)              # 50
print(alice.health)             # 80
print(alice.is_alive())         # True
```

When you write `alice.score_points(50)`, two things happen:

1. Python looks at the blueprint and finds the `score_points` procedure.
2. It calls the procedure, automatically filling the `self` slot with `alice`. The `50` is delivered as `points`.

So `alice.score_points(50)` is internally the same as `Player.score_points(alice, 50)` — except the first form is the one Python programmers always use. The dot syntax makes the *workshop being operated on* the prominent thing, and pushes the `self` argument out of sight.

The `score_points` procedure reads `self.score`, adds the delivered points, and writes the new value back to `self.score`. From inside the procedure, `self` is `alice` — so `self.score` is `alice.score`. The shelves modified are the shelves of the specific built workshop receiving the job order. Calling `bob.score_points(10)` would touch only `bob`'s shelves.

---

## A Complete Worked Example

Here is the `Player` blueprint with everything you have seen so far, plus two more workshops built from it and used:

```python
class Player:
    """A player in the game."""

    def __init__(self, name):
        self.name = name
        self.score = 0
        self.health = 100

    def score_points(self, points):
        self.score += points

    def take_damage(self, amount):
        self.health -= amount
        if self.health < 0:
            self.health = 0

    def is_alive(self):
        return self.health > 0

    def report(self):
        status = "alive" if self.is_alive() else "down"
        return f"{self.name}: score {self.score}, health {self.health} ({status})"


alice = Player("Alice")
bob = Player("Bob")

alice.score_points(50)
alice.take_damage(30)

bob.score_points(20)
bob.take_damage(150)

print(alice.report())            # Alice: score 50, health 70 (alive)
print(bob.report())              # Bob: score 20, health 0 (down)
```

A few things to notice:

- The blueprint's first triple-quoted scroll is the **docstring**, same convention as Lesson 8. `help(Player)` will print it.
- `take_damage` does a small bit of bookkeeping — clamps the health at zero so it cannot go negative. Methods often add this kind of safe-guarding around the bare shelf updates.
- `report` calls another built-in workstation — `self.is_alive()` — from inside its own procedure. Methods can call methods on the same workshop. The `self` is implicit and unchanged.
- The result of `report` is a fill-in scroll (Lesson 6), built from the workshop's own shelves. This is one of the most common workshop patterns: a method that returns a presentable description of the workshop's state.

---

## How a Workshop Introduces Itself — `__str__` and `__repr__`

When you `print` a built workshop directly, Python needs to know how to write it out as a scroll:

```python
print(alice)
# <__main__.Player object at 0x7f8b1c0d1e80>
```

That default is rarely what you want. To control how the workshop is read out, define `__str__` — another dunder method that returns the scroll to be used when the workshop is printed:

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0
        self.health = 100

    def __str__(self):
        return f"Player({self.name!r}, score={self.score}, health={self.health})"


alice = Player("Alice")
print(alice)
# Player('Alice', score=0, health=100)
```

`__str__` runs whenever Python needs a *user-friendly* scroll for the workshop. There is also `__repr__`, which runs when Python needs a *developer-friendly* scroll (typically in the interactive prompt, or when the workshop appears inside another collection like a row). For most beginner classes, defining `__str__` is enough; for libraries, `__repr__` is also worth defining.

The `{self.name!r}` form is a fill-in scroll feature: the `!r` modifier tells the window to use the value's *repr* (which adds quotes around scrolls, among other things). The result is a scroll a developer can read back to understand what the workshop is.

---

## Class Attributes — Shared Shelves on the Blueprint

So far every shelf you placed lived on a specific built workshop — `alice.name`, `bob.score`. There is also a way to place a shelf *on the blueprint itself*, shared by every workshop built from it.

```python
class Player:
    species = "human"           # a class attribute — lives on the blueprint

    def __init__(self, name):
        self.name = name        # an instance attribute — lives on the built workshop
        self.score = 0


alice = Player("Alice")
bob = Player("Bob")

print(alice.species)            # human
print(bob.species)              # human
print(Player.species)           # human — readable directly from the blueprint
```

Picture: a single shelf pinned to the blueprint itself, in the architect's office. Every workshop built from the blueprint *can read* that shelf, simply because the blueprint is part of every workshop's identity.

Class attributes are useful for *defaults and shared constants* — values that all workshops of the same kind agree on. They are usually scrolls, stones, or sealed crates (immutable items).

**A trap with mutable class attributes.** If a class attribute is a *mutable* item — a numbered row, a filing cabinet — every workshop ends up sharing the same physical item, with the same surprising consequences as Lesson 8's mutable-default trap:

```python
class Player:
    items = []                  # ⚠ mutable class attribute — shared across all workshops!

    def __init__(self, name):
        self.name = name

    def pick_up(self, item):
        self.items.append(item)


alice = Player("Alice")
bob = Player("Bob")
alice.pick_up("sword")
print(bob.items)                # ['sword']  — !
```

The fix is the same as the Lesson 8 fix: give each workshop its own row by creating it in `__init__`:

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.items = []         # ✅ instance attribute — independent per workshop
```

Now each built workshop owns its own `items` row, and Alice picking up a sword has no effect on Bob.

In general: use class attributes for constants and defaults; use instance attributes (set in `__init__`) for anything per-workshop or mutable.

---

## What You Now Know

You can draft a blueprint with `class`, build workshops from it by calling the blueprint's name as a workstation, and write methods (built-in workstations) and an `__init__` (the fit-out procedure) to give each workshop its own shelves. You know that `self` is always *this workshop* — the specific built workshop a job order is currently operating on — and that the dot syntax (`workshop.attribute`, `workshop.method()`) is how the rest of the program sends job orders and reads shelves.

You have met `__str__` for controlling how a workshop is printed, and class attributes for shared constants — with the same mutable-default trap that bit you in Lesson 8 returning here in a slightly different form.

This lesson also retired two early-banned terms. *Method* and *attribute* are now formally in the tutorial's vocabulary. *Object* in the OOP sense — *"a built workshop is an object"* — also becomes usable from here forward, when paired with its physical equivalent.

You will see classes constantly in real Python code. Almost every library defines its own blueprints. The next lesson shows how to take an existing blueprint and *extend* it — drawing a new blueprint that inherits all the layout of the original and adds or changes specific details.

---

## Quick Reference

| Python | Workshop image |
|---|---|
| `class Name:` | Draft a blueprint named `Name`. |
| `class Name:` `    """docstring"""` | First scroll inside the blueprint is the docstring. |
| `obj = Name(args)` | Build a workshop from the `Name` blueprint. `__init__` fits it out. |
| `def __init__(self, ...):` | The fit-out procedure. Runs once per built workshop. |
| `self.shelf = value` | Place an item on *this workshop's* shelf (instance attribute). |
| `obj.shelf` | Read or write *that workshop's* shelf from outside. |
| `def method(self, ...):` | A built-in workstation. First slot is `self` — this workshop. |
| `obj.method(args)` | Send a job order to the built-in workstation; `self` is filled automatically. |
| `class Name:` `    constant = 10` | A class attribute — shared by every workshop. |
| Class attribute caution | Never use a mutable item (row, cabinet) as a class attribute — they will share. Put it in `__init__`. |
| `def __str__(self):` | How the workshop introduces itself when printed. |
| `def __repr__(self):` | Developer-facing form; shown in the interactive prompt. |
| PascalCase | Convention for blueprint names: `Player`, `BankAccount`, `LogEntry`. |

---

## Try It

**The simplest blueprint:**

```python
class Player:
    pass

alice = Player()
alice.name = "Alice"
alice.score = 100
print(alice.name, alice.score)
```

Two attributes placed by hand. Works, but is fragile — easy to forget one.

**With `__init__`:**

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0

alice = Player("Alice")
bob = Player("Bob")
print(alice.name, alice.score)
print(bob.name, bob.score)
```

Every workshop gets the same starting shelves automatically.

**Adding methods:**

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0

    def score_points(self, points):
        self.score += points

alice = Player("Alice")
alice.score_points(50)
alice.score_points(20)
print(alice.score)                  # 70
```

**The full worked example:**

```python
class Player:
    """A player in the game."""

    def __init__(self, name):
        self.name = name
        self.score = 0
        self.health = 100

    def score_points(self, points):
        self.score += points

    def take_damage(self, amount):
        self.health -= amount
        if self.health < 0:
            self.health = 0

    def is_alive(self):
        return self.health > 0

    def report(self):
        status = "alive" if self.is_alive() else "down"
        return f"{self.name}: score {self.score}, health {self.health} ({status})"


alice = Player("Alice")
bob = Player("Bob")
alice.score_points(50)
alice.take_damage(30)
bob.take_damage(150)

print(alice.report())
print(bob.report())
```

**`__str__` for printable output:**

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0

    def __str__(self):
        return f"Player({self.name}, score={self.score})"

alice = Player("Alice")
print(alice)                        # Player(Alice, score=0)
```

**The mutable class-attribute trap — see it, then fix it:**

```python
class Bag:
    items = []                      # ⚠ shared

    def pick_up(self, item):
        self.items.append(item)

a = Bag()
b = Bag()
a.pick_up("sword")
print(a.items)
print(b.items)                      # ['sword'] — surprising!
```

Now fix it:

```python
class Bag:
    def __init__(self):
        self.items = []             # per-workshop, independent

    def pick_up(self, item):
        self.items.append(item)

a = Bag()
b = Bag()
a.pick_up("sword")
print(a.items)                      # ['sword']
print(b.items)                      # [] — Bob's bag is independent
```

---

## Where Next?

The next lesson shows how to take an existing blueprint and extend it — drawing a *new* blueprint that inherits everything from the original and adds or changes specific details. This is **inheritance**, and it is the second pillar of the workshop-blueprint pattern.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 17 | Z5 — Factory Floor | Inheritance — extending a blueprint |
| Lesson 18 | Z5 — Factory Floor | Iterators and Generators — passing one item and waiting |
| Lesson 19 | Z5 — Factory Floor | Lambda and Decorators — impromptu workstations and wrappers |
| Lesson 20 | Z1 — Tool Store | Modules and Packages — stocking and fetching |

*See the full lesson map in **The Factory — A Complete Map**.*
