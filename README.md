# vivid-python

> A Python tutorial series built for visual thinkers — every concept mapped to a factory you can see in your mind.

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Main series](https://img.shields.io/badge/main%20series-25%20of%2025%20drafted-brightgreen)
![Advanced](https://img.shields.io/badge/advanced-4%20of%204%20drafted-brightgreen)
![Language](https://img.shields.io/badge/language-Python-yellow)
![Audience](https://img.shields.io/badge/audience-visual--spatial%20learners-purple)

---

## What This Is

Most programming tutorials explain how code works. vivid-python shows you where it lives.

Every concept in this series has a physical location inside a factory — a building you can picture, walk through, and return to. Variables live on shelves in the Warehouse. Functions are workstations on the Factory Floor. Error handling is a Quality Control station between the floor and the loading bay. The Records Department stores anything that needs to survive after the shift ends.

This metaphor is not decoration. It is the lesson. For learners with strongly visual-spatial cognition — particularly those with **hyperphantasia** — inhabiting a concept is faster and more durable than reading a definition of it.

---

## The Factory

Every Python program is a factory. Here is the full complex:

```
                    ┌──────────────────────────────────────────┐
                    │  Z4 — SHIFT MANAGER'S OFFICE             │
                    │  Program Structure & Orchestration        │
                    └──────────────────┬───────────────────────┘
                                       │ oversees all zones
               ┌───────────────────────┼───────────────────────────────┐
               │                       │                                │
  ┌────────────┴───┐                   ▼                   ┌───────────┴────────┐
  │ Z1             │  ┌─────────────┐  ┌─────────────┐    │ Z9                 │
  │ TOOL STORE     │  │ Z2          │  │ Z3          │    │ CCTV ROOM          │
  │                │  │ GOODS IN    │  │ WAREHOUSE   │    │                    │
  │ pip · venv     │─▶│             │─▶│             │    │ logging            │
  │ import         │  │ input()     │  │ variables   │    │                    │
  │                │  │ files       │  │ data types  │    └────────────────────┘
  └────────────────┘  │ APIs        │  │ collections │
                      └─────────────┘  └──────┬──────┘
                                              │
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
               │ TESTING LAB     │  │ RECORDS DEPT     │
               │                 │  │                  │
               │ pytest          │  │ SQLite · MySQL   │
               │ unittest        │  │ MongoDB          │
               └─────────────────┘  └──────────────────┘

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

The main production line runs left to right:

```
[Goods In] ──▶ [Warehouse] ──▶ [Factory Floor] ──▶ [Quality Control] ──▶ [Outgoings]
   Z2              Z3               Z5                    Z7                  Z11
```

Every lesson tells you which zone you are entering before you step inside.

---

## The Object Vocabulary

Objects on the warehouse shelves always look the same, across every lesson and every diagram:

| Object | Python type | Example |
|---|---|---|
| Stone | `int` | `42`, `-7` |
| Vial | `float` | `3.14`, `9.81` |
| Scroll | `str` | `"hello"`, `"Shane"` |
| Switch | `bool` | `True`, `False` |
| Numbered row | `list` | `[1, 2, 3]` |
| Sealed crate | `tuple` | `(10, 20)` |
| Unsorted bin | `set` | `{1, 2, 3}` |
| Filing cabinet | `dict` | `{"name": "Shane"}` |
| Vacant cubbyhole | `None` | `None` |

Once you know what a stone looks like, you will never mistake it for a scroll.

---

## Lessons

The full 25-lesson main series and all four advanced lessons (Testing, Logging, Threading, Async) are drafted. Each lesson now awaits a content-review pass and an alignment pass with the [Locus](https://github.com/Darian-Frey/Locus) renderer.

### Main series

| # | Zone | Topic | Status |
|---|---|---|---|
| 1 | Z3 — Warehouse | [Variables & Memory](lessons/lesson-01-variables.md) | 🟡 Drafted |
| 2 | Z11 — Outgoings | [Syntax & Output](lessons/lesson-02-output.md) | 🟡 Drafted |
| 3 | Z3 — Warehouse | [Data Types](lessons/lesson-03-data-types.md) | 🟡 Drafted |
| 4 | Z3 — Warehouse | [Numbers](lessons/lesson-04-numbers.md) | 🟡 Drafted |
| 5 | Z3 → Z5 | [Casting](lessons/lesson-05-casting.md) | 🟡 Drafted |
| 6 | Z3 — Warehouse | [Strings (incl. f-strings)](lessons/lesson-06-strings.md) | 🟡 Drafted |
| 7 | Z3 — Warehouse | [Lists](lessons/lesson-07-lists.md) | 🟡 Drafted |
| 8 | Z5 — Factory Floor | [Functions](lessons/lesson-08-functions.md) | 🟡 Drafted |
| 9 | Z3 — Warehouse | [Scope](lessons/lesson-09-scope.md) | 🟡 Drafted |
| 10 | Z3 — Warehouse | [Dictionaries](lessons/lesson-10-dictionaries.md) | 🟡 Drafted |
| 11 | Z3 — Warehouse | [Tuples and Sets](lessons/lesson-11-tuples-and-sets.md) | 🟡 Drafted |
| 12 | Z5 — Factory Floor | [Operators](lessons/lesson-12-operators.md) | 🟡 Drafted |
| 13 | Z5 — Factory Floor | [If / Else](lessons/lesson-13-conditionals.md) | 🟡 Drafted |
| 14 | Z5 — Factory Floor | [Loops](lessons/lesson-14-loops.md) | 🟡 Drafted |
| 15 | Z5 — Factory Floor | [Comprehensions](lessons/lesson-15-comprehensions.md) | 🟡 Drafted |
| 16 | Z5 — Factory Floor | [Classes](lessons/lesson-16-classes.md) | 🟡 Drafted |
| 17 | Z5 — Factory Floor | [Inheritance](lessons/lesson-17-inheritance.md) | 🟡 Drafted |
| 18 | Z5 — Factory Floor | [Iterators and Generators](lessons/lesson-18-generators.md) | 🟡 Drafted |
| 19 | Z5 — Factory Floor | [Lambda and Decorators](lessons/lesson-19-lambda-and-decorators.md) | 🟡 Drafted |
| 20 | Z1 — Tool Store | [Modules and Packages](lessons/lesson-20-modules.md) | 🟡 Drafted |
| 21 | Z2, Z8 | [File Handling](lessons/lesson-21-file-handling.md) | 🟡 Drafted |
| 22 | Z7 — Quality Control | [Error Handling](lessons/lesson-22-error-handling.md) | 🟡 Drafted |
| 23 | Z8 — Records Department | [Databases](lessons/lesson-23-databases.md) | 🟡 Drafted |
| 24 | Z2 — Goods In | [User Input in Depth](lessons/lesson-24-user-input.md) | 🟡 Drafted |
| 25 | Z4 — Shift Manager's Office | [Putting It Together](lessons/lesson-25-putting-it-together.md) | 🟡 Drafted |

### Advanced phases

| # | Zone | Topic | Status |
|---|---|---|---|
| 5a | Z6 — Testing Laboratory | [Testing — pytest, station checks](lessons/advanced-05a-testing.md) | 🟡 Drafted |
| 5b | Z9 — CCTV Room | [Logging — permanent records from every zone](lessons/advanced-05b-logging.md) | 🟡 Drafted |
| 6a | Z10 — Night Shift Wing | [Threading — two lines running in parallel](lessons/advanced-06a-threading.md) | 🟡 Drafted |
| 6b | Z10 — Night Shift Wing | [Async — one worker, no idle time](lessons/advanced-06b-async.md) | 🟡 Drafted |

🟡 **Drafted** means the lesson is on disk and readable. Each lesson will receive a final content-review pass and an alignment pass with the [Locus](https://github.com/Darian-Frey/Locus) renderer before being marked complete.

---

## Where to Start

**New to the factory:**
→ Read [The Factory — A Complete Map](factory-overview.md) first.  
→ Then begin [Lesson 1 — Variables & Memory](lessons/lesson-01-variables.md).

**Already familiar with the factory:**
→ Jump to any lesson in the table above.

**Looking for the vocabulary contract:**
→ See [VOCABULARY.md](VOCABULARY.md) — the canonical reference for all visual terms used across this series and the Locus IDE plugin.

---

## Companion Project

vivid-python is one half of a larger system.

**[Locus](https://github.com/Darian-Frey/Locus)** is a VS Code / Antigravity IDE extension that renders source code as a navigable three-dimensional spatial environment — the same factory, rendered in 3D as you write. Functions become rooms. Variables sit on shelves. Data flows visibly along connections.

Both projects share the same vocabulary contract. A stone in vivid-python is a stone in Locus.

---

## Who This Is For

vivid-python is designed primarily for learners with **hyperphantasia** — the ability to generate unusually vivid, detailed mental imagery. For these learners, the factory is not a metaphor to be held lightly: it is a place they can actually visit, navigate, and return to.

The series is also effective for anyone who learns better through spatial or visual models rather than abstract definitions.

It is not designed to replace hands-on practice. Every lesson ends with code to run. The factory gives you the model; the Python prompt gives you the proof.

---

## Vocabulary Standard

All visual terms used in this series — object names, zone names, action verbs, spatial properties — are defined in [VOCABULARY.md](VOCABULARY.md). This file is the shared contract between vivid-python and Locus. If a lesson uses a term not in VOCABULARY.md, that is a bug, not a style choice.

---

## Status

> **Status**: Active  
> **Provenance**: Claude (primary author), Shane Hartley (series director)  
> **Last reviewed**: 2026-05-19  
> **Why**: All 29 lessons (25 main + 4 advanced) drafted. Phase status across the board: 🟡 In progress — every phase awaits a final content-review pass and alignment with the Locus renderer per the "tutorial and Locus move together" rule.

---

## Licence

Creative Commons Attribution 4.0 International (CC BY 4.0) — see [LICENSE](LICENSE) for the full legal code.

You are free to share and adapt the material for any purpose, including commercially, provided you give appropriate credit and indicate any changes.
