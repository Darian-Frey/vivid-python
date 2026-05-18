# Python for Hyperphantasic Minds
## Lesson 17 — Inheritance

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 17 of 25  
> **Topic**: Inheritance — extending a blueprint  
> **Factory zone**: Z5 — The Factory Floor  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Still on the Factory Floor. The last lesson gave you the workshop blueprint and the built workshops that come off it. This lesson shows how to take an existing blueprint and **extend** it — drawing a new blueprint that inherits all the shelves and built-in workstations of the original, then adds new ones or replaces specific ones.

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

Inheritance is the second pillar of the workshop-blueprint pattern. Once you have a blueprint that describes a *general* kind of workshop, you can draw *specific* blueprints that build on it — a `Wizard` blueprint that extends `Player`, a `LogEntry` that extends a more general `Record`, a `Mouse` that extends `Animal`. The relationship is *"is a more specific kind of"* — every wizard *is a player*; every mouse *is an animal*.

---

## The Motivation — When Blueprints Relate

Imagine your game has several kinds of player — a wizard, a warrior, a healer. Each kind shares most of its layout with every other kind: every player has a name, a score, a health value; every player can take damage and report their state. But each kind also has its own specifics — a wizard has mana; a warrior has armour; a healer has a healing pool.

You *could* write three separate blueprints, copying and pasting the common parts into each. That has two problems:

- The shared procedures (`take_damage`, `report`) would be duplicated across all three. A bug fix would have to be applied three times.
- A reader looking at one blueprint would have no easy way to tell *what is general* about a player versus *what is specific* to a wizard.

The cleaner pattern is to draw one **parent blueprint** with everything shared, then draw three **child blueprints** that each extend the parent — adding only what is specific. This is **inheritance**.

A child blueprint *inherits* the layout of the parent. Every shelf the parent defined is also on the child. Every built-in workstation the parent defined is also pre-installed on workshops built from the child. The child can then *add* new shelves and workstations, or *replace* (override) specific parent ones with more specialised versions.

---

## Drawing a Derived Blueprint — `class Child(Parent):`

To draw a child blueprint, name the parent in parentheses after the child's name:

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0
        self.health = 100

    def take_damage(self, amount):
        self.health -= amount


class Wizard(Player):           # Wizard extends Player
    pass
```

The `Wizard` blueprint is now a child of `Player`. Even though its body is just `pass`, every workshop built from `Wizard` has *all* the shelves and workstations the `Player` blueprint defines:

```python
gandalf = Wizard("Gandalf")
print(gandalf.name)             # 'Gandalf'
print(gandalf.health)           # 100
gandalf.take_damage(30)
print(gandalf.health)           # 70
```

Picture the architect copying the `Player` blueprint onto a new sheet, then setting it aside. The new sheet (`Wizard`) is currently identical to the parent. Any workshop built from it is identical to a workshop built from `Player`. The difference will appear when we add or change something inside `Wizard`'s body.

The parent is also called the **superclass** or **base class** in formal Python; the child is called the **subclass** or **derived class**. The physical picture is enough: a parent blueprint pinned above, a child blueprint pinned below, with an arrow connecting them. Built workshops can come off either.

---

## Adding to a Derived Blueprint

A child blueprint can simply *add* new shelves and workstations on top of what it inherits:

```python
class Wizard(Player):
    def __init__(self, name):
        super().__init__(name)         # do the parent's fit-out first
        self.mana = 50                  # then add the wizard-specific shelf

    def cast_spell(self):
        if self.mana >= 10:
            self.mana -= 10
            print(self.name, "casts a spell.")
        else:
            print(self.name, "is out of mana.")
```

Two new things appear inside `Wizard`:

- A new built-in workstation, `cast_spell`, that operates on a wizard-specific shelf (`self.mana`).
- An overridden `__init__` that first calls **`super().__init__(name)`** — the parent's fit-out procedure — and then adds the `self.mana` shelf afterwards.

`super()` reads as *"the parent blueprint's version of"*. In `Wizard.__init__`, `super().__init__(name)` calls `Player.__init__` with the same `self` (the wizard being fitted out) and the delivered `name`. The parent's procedure runs first — placing `name`, `score`, `health` on the workshop's shelves — and then control returns to the child's `__init__`, which adds the `mana` shelf.

```python
gandalf = Wizard("Gandalf")
print(gandalf.name, gandalf.health, gandalf.mana)   # Gandalf 100 50

gandalf.cast_spell()             # Gandalf casts a spell.
print(gandalf.mana)              # 40
```

The wizard inherits everything the player had — `name`, `health`, `take_damage`, `score_points` if defined — plus the new `mana` shelf and the new `cast_spell` workstation.

---

## Why `super().__init__`?

A common mistake when first writing a child `__init__`: forgetting `super().__init__(...)` and doing all the fit-out by hand:

```python
class Wizard(Player):
    def __init__(self, name):
        self.name = name             # ⚠ duplicating the parent's work
        self.score = 0
        self.health = 100
        self.mana = 50
```

This works, in the sense that the wizard ends up with all the right shelves. But it has two big problems:

- **Duplication.** If `Player.__init__` ever changes (a new shelf added, a default tweaked), every child has to be updated by hand.
- **Hidden dependence.** A reader looking at `Wizard.__init__` cannot tell what `Player` would have done at construction; the wizard's fit-out is a complete restatement, not an *extension*.

`super().__init__(name)` is the universally preferred pattern: *"do everything the parent does at this stage, then add my own bits"*. It keeps the wizard's blueprint focused on what makes a wizard *different* from a generic player.

---

## Overriding — Replacing a Parent's Method

The other common reason for a child blueprint is to *replace* one of the parent's procedures with a more specific version. This is **overriding**.

```python
class Warrior(Player):
    def __init__(self, name):
        super().__init__(name)
        self.armour = 20

    def take_damage(self, amount):
        reduced = max(0, amount - self.armour)      # absorb up to armour points
        self.health -= reduced
        if self.health < 0:
            self.health = 0
```

`Warrior.take_damage` replaces `Player.take_damage` entirely for warriors. A workshop built from `Warrior` uses the warrior's version; warriors absorb a portion of damage based on their armour.

```python
boromir = Warrior("Boromir")
boromir.take_damage(50)
print(boromir.health)           # 70 — 30 damage after armour absorbed 20

gandalf = Wizard("Gandalf")
gandalf.take_damage(50)
print(gandalf.health)           # 50 — wizards get the plain Player behaviour
```

The two workshops share the same parent — both *are* players — but each one runs the version of `take_damage` that belongs to its own specific blueprint. This is **polymorphism** in formal terms: the same method name behaves differently depending on which specific built workshop receives the job order.

A common override pattern is *"do everything the parent does, then add a small adjustment"* — which uses `super()` again:

```python
class Healer(Player):
    def take_damage(self, amount):
        super().take_damage(amount)             # apply the standard damage
        print(f"{self.name} winces.")           # then add a healer-specific effect
```

`super().take_damage(amount)` runs the parent's procedure on the same workshop, then control returns and the extra line runs. Use this pattern whenever a child wants to *augment* the parent's behaviour rather than replace it.

---

## A Complete Worked Example

Putting it all together — the parent and three children:

```python
class Player:
    """A player in the game."""

    def __init__(self, name):
        self.name = name
        self.score = 0
        self.health = 100

    def take_damage(self, amount):
        self.health -= amount
        if self.health < 0:
            self.health = 0

    def report(self):
        return f"{self.name}: health {self.health}"


class Wizard(Player):
    def __init__(self, name):
        super().__init__(name)
        self.mana = 50

    def cast_spell(self):
        if self.mana >= 10:
            self.mana -= 10
            return f"{self.name} casts a spell."
        return f"{self.name} is out of mana."


class Warrior(Player):
    def __init__(self, name):
        super().__init__(name)
        self.armour = 20

    def take_damage(self, amount):
        reduced = max(0, amount - self.armour)
        super().take_damage(reduced)


players = [
    Wizard("Gandalf"),
    Warrior("Boromir"),
    Player("Frodo"),
]

for p in players:
    p.take_damage(30)
    print(p.report())

# Gandalf: health 70
# Boromir: health 90        (armour absorbed 20 of the 30 damage)
# Frodo: health 70          (plain Player behaviour)
```

Notice the loop in the last block. The row `players` holds three workshops, all *different specific kinds* — but the loop walks them uniformly, sending the same job orders to each. Each workshop responds with its own specific procedure. This is the practical payoff of inheritance: write code once that operates on a parent's interface, and every child blueprint plugs in automatically.

---

## Checking What Kind of Workshop You Have — `isinstance()`

Occasionally you want code that *does* care which specific blueprint a workshop came from. Python's standard kit provides `isinstance(obj, BlueprintName)`, which returns a switch:

```python
print(isinstance(gandalf, Wizard))         # True
print(isinstance(gandalf, Player))         # True — wizards are also players
print(isinstance(gandalf, Warrior))        # False
print(isinstance(boromir, Wizard))         # False
```

Two important properties:

- A child workshop matches its own blueprint *and* every ancestor blueprint. The line `isinstance(gandalf, Player)` is `True` because every wizard *is a player*. This matches the natural English reading.
- `isinstance` accepts a sealed crate of blueprints for "is it any of these?":

```python
if isinstance(p, (Wizard, Warrior)):
    print("Caster or fighter.")
```

A close cousin is `issubclass(Child, Parent)`, which asks whether the *blueprints themselves* are in a parent-child relationship. You use this much less often than `isinstance`.

There is a competing pattern in Python — *duck typing* — that prefers to ask *"can this workshop do X?"* rather than *"is this workshop of type Y?"*. Both are valid; both come up in real code. Beginner-friendly programs lean on `isinstance` when they need it and otherwise let the methods do the talking.

---

## Multiple Inheritance — A Brief Mention

Python allows a child blueprint to extend more than one parent:

```python
class Audible:
    def speak(self):
        print(f"{self.name} says hello.")


class Visible:
    def appear(self):
        print(f"{self.name} appears in a puff of smoke.")


class Magician(Audible, Visible):
    def __init__(self, name):
        self.name = name
```

A `Magician` inherits from both `Audible` and `Visible`. It gets `speak` and `appear` together. Python resolves the order in which it looks up methods using a rule called the **method resolution order** (MRO) — for most simple cases, *child first, then each parent left to right*.

In practice, **prefer single inheritance** in beginner-to-intermediate code. Multiple inheritance creates fragile structures where small changes in one parent ripple through unexpected paths. The Python community's common practice for combining behaviours is *single inheritance plus small classes called mixins*, used carefully. We will not depend on multiple inheritance anywhere else in this series.

---

## When NOT to Use Inheritance — Composition

Inheritance is the right tool when the relationship is genuinely *"is a more specific kind of"*. It is the wrong tool when the relationship is *"has a"*.

A `Car` is not a more specific kind of `Engine`. A car *has* an engine. The cleaner pattern is **composition** — a `Car` workshop has an `Engine` workshop as one of its shelves:

```python
class Engine:
    def __init__(self, horsepower):
        self.horsepower = horsepower

    def start(self):
        print("Engine started.")


class Car:
    def __init__(self, model, horsepower):
        self.model = model
        self.engine = Engine(horsepower)        # a Car has an Engine

    def drive(self):
        self.engine.start()
        print(f"{self.model} is driving.")
```

`Car` does not inherit from `Engine`. Instead, every car's `engine` attribute is its own built workshop of `Engine`, which the car delegates work to. This is usually cleaner than inheriting from `Engine` would be — the relationship is genuine.

A rough rule of thumb: if you cannot honestly read *"a Child is a Parent"* in plain English, use composition. If you can — *"a Wizard is a Player"*, *"a Mouse is an Animal"*, *"a LogEntry is a Record"* — inheritance is the right shape.

---

## What You Now Know

You can draw a child blueprint that extends a parent with `class Child(Parent):`. The child inherits all the shelves and built-in workstations of the parent by default. Inside the child's body, you can *add* new shelves and methods, and you can *override* parent methods with more specialised versions. The `super()` workstation is how a child calls back into the parent's version — most commonly in `__init__`, where the child wants to delegate the parent's standard fit-out before adding its own extras.

You know `isinstance(obj, Blueprint)` for asking *"is this workshop of this kind?"* — and that it follows the inheritance chain naturally. You have heard of multiple inheritance and know to prefer single inheritance in your own code. You know the *"is-a vs has-a"* rule of thumb that distinguishes inheritance from composition.

The next lesson moves to a different shape on the Floor — **iterators and generators** — workstations that produce one item at a time and pause between requests. You first met one in disguise as a generator expression in Lesson 15; Lesson 18 builds it properly.

---

## Quick Reference

| Python | Workshop image |
|---|---|
| `class Child(Parent):` | Draw a child blueprint that extends `Parent`. Inherits all its shelves and workstations. |
| Child overrides a method | A method defined in `Child` replaces the parent's version for that blueprint. |
| `super().method(args)` | Call the parent's version of `method`, on the same workshop. |
| `super().__init__(args)` | The most common use: delegate the parent's fit-out first, then add child-specific shelves. |
| `isinstance(obj, Blueprint)` | Switch — is this workshop built from `Blueprint` or any descendant? |
| `isinstance(obj, (A, B))` | Switch — is it of *any* of these blueprints? |
| `issubclass(Child, Parent)` | Switch — is the blueprint a descendant of another blueprint? |
| Multiple inheritance | `class C(A, B):` — Python supports it; prefer single inheritance in your own code. |
| Composition | A workshop *has* another workshop as an attribute. Use when *"is-a"* does not honestly read. |

---

## Try It

**The minimal child blueprint:**

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.health = 100

class Wizard(Player):
    pass

gandalf = Wizard("Gandalf")
print(gandalf.name, gandalf.health)
```

`Wizard` is empty but inherits everything from `Player`.

**Adding to a child blueprint:**

```python
class Wizard(Player):
    def __init__(self, name):
        super().__init__(name)
        self.mana = 50

    def cast_spell(self):
        if self.mana >= 10:
            self.mana -= 10
            return f"{self.name} casts a spell."
        return f"{self.name} is out of mana."

gandalf = Wizard("Gandalf")
print(gandalf.name, gandalf.health, gandalf.mana)
print(gandalf.cast_spell())
print(gandalf.mana)
```

**Overriding:**

```python
class Warrior(Player):
    def __init__(self, name):
        super().__init__(name)
        self.armour = 20

    def take_damage(self, amount):
        reduced = max(0, amount - self.armour)
        super().take_damage(reduced)

class Player:
    def take_damage(self, amount):
        self.health -= amount

boromir = Warrior("Boromir")
boromir.take_damage(50)
print(boromir.health)
```

**Polymorphism — same job order, different procedures:**

```python
players = [Wizard("Gandalf"), Warrior("Boromir"), Player("Frodo")]
for p in players:
    p.take_damage(30)
    print(p.name, p.health)
```

Each workshop runs the version of `take_damage` belonging to its own blueprint.

**`isinstance` for type checks:**

```python
print(isinstance(gandalf, Wizard))
print(isinstance(gandalf, Player))         # True — wizards are players
print(isinstance(gandalf, Warrior))        # False
print(isinstance(gandalf, (Wizard, Warrior)))   # True — either of these
```

**Composition vs inheritance — compare two designs:**

```python
# Inheritance — wrong shape here
class Engine:
    def start(self):
        print("Engine started.")

class Car(Engine):                          # ⚠ A Car is NOT an Engine
    def drive(self):
        self.start()
        print("Driving.")

# Composition — better
class Car:
    def __init__(self):
        self.engine = Engine()              # A Car HAS an Engine

    def drive(self):
        self.engine.start()
        print("Driving.")

c = Car()
c.drive()
```

The composition form reads more honestly. A car owns an engine; it is not a kind of engine.

---

## Where Next?

The next lesson visits a different shape — **generators**, workstations that produce one item at a time and pause between requests. You met one in disguise in Lesson 15 (the round-bracket comprehension). Lesson 18 builds them properly. After that, lambda and decorators round out the Factory Floor.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 18 | Z5 — Factory Floor | Iterators and Generators — passing one item and waiting |
| Lesson 19 | Z5 — Factory Floor | Lambda and Decorators — impromptu workstations and wrappers |
| Lesson 20 | Z1 — Tool Store | Modules and Packages — stocking and fetching |
| Lesson 21 | Z2, Z8 | File Handling — crates at the loading bay |

*See the full lesson map in **The Factory — A Complete Map**.*
