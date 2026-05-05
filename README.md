# vivid-python

> A Python tutorial series built for visual thinkers — every concept mapped to a factory you can see in your mind.

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Lessons](https://img.shields.io/badge/lessons-1%20of%2025-blue)
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

| # | Zone | Topic | Status |
|---|---|---|---|
| 1 | Z3 — Warehouse | Variables & Memory | ✅ Available |
| 2 | Z11 — Outgoings | Syntax & Output | 🔜 Coming soon |
| 3 | Z3 — Warehouse | Data Types | 🔜 Coming soon |
| 4 | Z3 — Warehouse | Numbers | 🔜 Coming soon |
| 5 | Z3 → Z5 | Casting | 🔜 Coming soon |
| 6 | Z3 — Warehouse | Strings | 🔜 Coming soon |
| 7 | Z3 — Warehouse | Booleans | 🔜 Coming soon |
| 8 | Z5 — Factory Floor | Operators | 🔜 Coming soon |
| 9 | Z3 — Warehouse | Lists | 🔜 Coming soon |
| 10 | Z3 — Warehouse | Tuples | 🔜 Coming soon |
| 11 | Z3 — Warehouse | Sets | 🔜 Coming soon |
| 12 | Z3 — Warehouse | Dictionaries | 🔜 Coming soon |
| 13 | Z5 — Factory Floor | If / Else — Junctions | 🔜 Coming soon |
| 14 | Z5 — Factory Floor | While Loops — Sensor Belts | 🔜 Coming soon |
| 15 | Z5 — Factory Floor | For Loops — Batch Belts | 🔜 Coming soon |
| 16 | Z5 — Factory Floor | Functions — Workstations | 🔜 Coming soon |
| 17 | Z3 — Warehouse | Scope — Locked Rooms | 🔜 Coming soon |
| 18 | Z5 — Factory Floor | Classes — Workshop Blueprints | 🔜 Coming soon |
| 19 | Z5 — Factory Floor | Inheritance — Extended Blueprints | 🔜 Coming soon |
| 20 | Z1 — Tool Store | Modules & Packages | 🔜 Coming soon |
| 21 | Z2 / Z8 | File Handling | 🔜 Coming soon |
| 22 | Z7 — Quality Control | Try / Except | 🔜 Coming soon |
| 23 | Z11 — Outgoings | String Formatting | 🔜 Coming soon |
| 24 | Z2 — Goods In | User Input | 🔜 Coming soon |
| 25 | All zones | Putting It Together | 🔜 Coming soon |

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
> **Last reviewed**: 2026-05-05  
> **Why**: Active development — foundation documents complete, lesson pipeline in progress.

---

## Licence

MIT — see [LICENSE](LICENSE) for details.
