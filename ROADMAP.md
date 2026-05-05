# ROADMAP.md — vivid-python Development Roadmap

> **Status**: Active  
> **Provenance**: Shane Hartley (series director), Claude (formalisation)  
> **Last reviewed**: 2026-05-05  
> **Why**: Defines the phase structure, completion criteria, and Locus alignment points for the vivid-python tutorial series. A lesson is not done when it is written — it is done when it is vocabulary-compliant, reviewed, and its corresponding Locus zone is renderable.

---

## Guiding Principles

**The factory is one world.** Every lesson adds to the same building. A learner who completes Phase 1 should be able to describe the Warehouse from memory. A learner who completes Phase 4 should be able to navigate the entire complex.

**Image first, notation second.** No lesson ships without its physical scene established before its first code block. The notation is the label for the experience, not the experience itself.

**vivid-python and Locus move together.** The tutorial tells the learner what the factory looks like. Locus shows it to them in their code. A lesson that introduces a zone Locus cannot yet render is a lesson that breaks the promise of the series. Each phase defines which Locus components must be ready before that phase is considered complete.

**Vocabulary compliance is not optional.** Every lesson is checked against VOCABULARY.md before it ships. A lesson with a banned term is not a draft — it is a bug.

---

## Phase Overview

| Phase | Name | Lessons | Zone focus | Status |
|---|---|---|---|---|
| 0 | **Foundation** | Overview, Lesson 1 | Z3 — Warehouse | 🟡 In progress |
| 1 | **First Words** | Lessons 2–4 | Z11, Z3 | 🔲 Not started |
| 2 | **The Full Warehouse** | Lessons 5–11 | Z3 in depth | 🔲 Not started |
| 3 | **The Factory Floor** | Lessons 12–19 | Z5 | 🔲 Not started |
| 4 | **The Supporting Buildings** | Lessons 20–25 | Z1, Z2, Z4, Z7, Z8 | 🔲 Not started |
| 5 | **Advanced Systems** | Advanced Phase 5 | Z6, Z9 | 🔲 Not started |
| 6 | **Concurrency** | Advanced Phase 6 | Z10 | 🔲 Not started |

---

## Phase 0 — Foundation

**Purpose**: establish the factory as a coherent world and the warehouse as the learner's first room. Define the vocabulary, the visual style, and the lesson format that all subsequent lessons will follow. This phase is the contract with the reader.

**Lessons in this phase:**

| Document | Status | Notes |
|---|---|---|
| `Python-factory-overview.md` | ✅ Complete | Full 11-zone map, vocabulary-compliant v0.2 |
| `VOCABULARY.md` | ✅ Complete | v0.2 — full contract between tutorial and Locus |
| `README.md` | ✅ Complete | Repo introduction with factory diagram and lesson table |
| `lesson-01-variables.md` | 🟡 Drafted | v0.2 vocabulary pass complete; full content review pending after tutorial outline is in place |

**Phase 0 exit criteria:**

- [ ] All four documents are vocabulary-compliant against VOCABULARY.md v0.2
- [ ] Lesson 1 uses "named cubbyhole" on first introduction, not "variable"
- [ ] Lesson 1 has no banned terms from Part 7 of VOCABULARY.md
- [ ] The WHERE YOU ARE diagram is present and correct in Lesson 1
- [ ] The repo structure is in place: `README.md`, `VOCABULARY.md`, `ROADMAP.md`, `factory-overview.md`, `lessons/lesson-01-variables.md`

**Locus requirements for Phase 0:**

- [ ] Z3 (Warehouse) zone annotation label implemented
- [ ] Shelf rendering for `int` (stone) and `str` (scroll) implemented
- [ ] Basic name card rendering on cubbyholes
- [ ] Tutorial mode can display the WHERE YOU ARE overlay for Z3

---

## Phase 1 — First Words

**Purpose**: give the learner the minimum viable factory experience. By the end of Phase 1, the learner can write a complete, working Python program — however simple — and locate every part of it in the factory. They can receive input, store it, and produce output.

**Lessons in this phase:**

| Lesson | Zone | Topic | Status |
|---|---|---|---|
| Lesson 2 | Z11 — Outgoings | Syntax & Output — calling out across the floor | 🔲 Not started |
| Lesson 3 | Z3 — Warehouse | Data Types — the full range of objects on the shelves | 🔲 Not started |
| Lesson 4 | Z3 — Warehouse | Numbers — stones and vials in depth | 🔲 Not started |

**What the learner can do after Phase 1:**

- Write `print()` statements and understand what Outgoings is
- Identify all core Python types by their canonical physical object
- Perform arithmetic with stones (`int`) and vials (`float`)
- Write a simple three-line program and place every line in the factory

**Phase 1 exit criteria:**

- [ ] All three lessons are written and vocabulary-compliant
- [ ] Each lesson has a WHERE YOU ARE diagram with the correct zone highlighted
- [ ] Each lesson ends with a Try It section and a Quick Reference table
- [ ] Lesson 3 introduces all nine canonical type objects from VOCABULARY.md Part 2.1
- [ ] Lesson 4 uses "vial" consistently for `float` — not "decimal number" or "floating point"

**Locus requirements for Phase 1:**

- [ ] Z11 (Outgoings) zone annotation label implemented
- [ ] Shelf rendering for all nine core type objects implemented
- [ ] Type material colour and character matches VOCABULARY.md Part 4.3
- [ ] `print()` call-out visual implemented (brief highlight on Outgoings annotation)

---

## Phase 2 — The Full Warehouse

**Purpose**: complete the learner's understanding of the Warehouse. By the end of Phase 2, the learner understands every kind of object that can sit on a shelf, how to reshape objects between types, and why some parts of the warehouse are locked.

**Lessons in this phase:**

| Lesson | Zone | Topic | Status |
|---|---|---|---|
| Lesson 5 | Z3 → Z5 | Casting — pressing objects into moulds | 🔲 Not started |
| Lesson 6 | Z3 — Warehouse | Strings — scrolls in depth | 🔲 Not started |
| Lesson 7 | Z3 — Warehouse | Lists — numbered rows in depth | 🔲 Not started |
| Lesson 8 | Z5 — Factory Floor | Functions — the first workstation | 🔲 Not started |
| Lesson 9 | Z3 — Warehouse | Scope — the locked rooms in full | 🔲 Not started |
| Lesson 10 | Z3 — Warehouse | Dictionaries — filing cabinets in depth | 🔲 Not started |
| Lesson 11 | Z3 — Warehouse | Tuples and Sets — sealed crates and unsorted bins | 🔲 Not started |

**Note on Lesson 8**: Functions technically belong to Z5 (Factory Floor) but are introduced here because the Warehouse cannot be fully understood without knowing what a workstation is — locked rooms only make sense in the context of a workstation that uses them. Lesson 8 is a brief excursion to the Factory Floor entrance before returning to complete the Warehouse.

**What the learner can do after Phase 2:**

- Convert between types using the mould metaphor
- Use all string methods and understand indexing and slicing on scrolls
- Build and manipulate numbered rows (lists)
- Write a basic workstation (function) and understand its input slots and output
- Navigate the locked room / open aisle distinction fluently
- Use filing cabinets (dicts), sealed crates (tuples), and unsorted bins (sets) correctly

**Phase 2 exit criteria:**

- [ ] All seven lessons written and vocabulary-compliant
- [ ] Lesson 5 uses "press into a mould" for casting — not "convert" or "cast" without physical context
- [ ] Lesson 8 introduces "workstation", "job order", "input slot", "delivered material", "finished product" — all from VOCABULARY.md Part 2.2
- [ ] Lesson 9 introduces all four scope levels: locked room (local), open aisle (global), anteroom (enclosing), factory standard kit (built-in)
- [ ] No lesson uses "method" before Lesson 8, "attribute" before Lesson 16

**Locus requirements for Phase 2:**

- [ ] Casting operation visual implemented (mould animation between type materials)
- [ ] String indexing and slicing overlay on scroll objects
- [ ] List/dict/tuple/set shelf rendering with count indicators
- [ ] Function room (workstation) implemented in Locus
- [ ] Scope visualisation: locked room interior visible only when cursor is inside function; open aisle visible at module level
- [ ] Corridor rendering with argument type substance

---

## Phase 3 — The Factory Floor

**Purpose**: move the learner into the largest zone. By the end of Phase 3, the learner can build complex programs using the full suite of Factory Floor tools — conditionals, loops, functions in depth, classes, and inheritance.

**Lessons in this phase:**

| Lesson | Zone | Topic | Status |
|---|---|---|---|
| Lesson 12 | Z5 — Factory Floor | Operators — the tools on the floor | 🔲 Not started |
| Lesson 13 | Z5 — Factory Floor | If / Else — junctions and inspection gates | 🔲 Not started |
| Lesson 14 | Z5 — Factory Floor | Loops — conveyor belts, emergency stops, skip gates | 🔲 Not started |
| Lesson 15 | Z5 — Factory Floor | Comprehensions — compact belt expressions | 🔲 Not started |
| Lesson 16 | Z5 — Factory Floor | Classes — workshop blueprints and built workshops | 🔲 Not started |
| Lesson 17 | Z5 — Factory Floor | Inheritance — extending a blueprint | 🔲 Not started |
| Lesson 18 | Z5 — Factory Floor | Iterators and Generators — passing one item and waiting | 🔲 Not started |
| Lesson 19 | Z5 — Factory Floor | Lambda and Decorators — impromptu workstations and wrappers | 🔲 Not started |

**What the learner can do after Phase 3:**

- Use all comparison and logical operators as factory floor tools
- Build junctions (if/elif/else) and route material correctly
- Build `for` and `while` conveyor belts and control them with emergency stops and skip gates
- Write list and dict comprehensions
- Design workshop blueprints (classes), build workshops from them (instances), and extend blueprints (inheritance)
- Use generators and lambda functions appropriately

**Phase 3 exit criteria:**

- [ ] All eight lessons written and vocabulary-compliant
- [ ] Lesson 13 uses "junction with inspection gates" — never "conditional" without physical context
- [ ] Lesson 14 uses "conveyor belt with counter" (for), "conveyor belt with sensor" (while), "emergency stop" (break), "skip gate" (continue) consistently
- [ ] Lesson 16 uses "workshop blueprint" for class and "built workshop" for instance throughout — "object" is still banned until used in context with its canonical name
- [ ] Lesson 16 uses "fit out the workshop" for `__init__` and "this workshop" for `self`
- [ ] No lesson uses "instantiate" — always "build a workshop from the blueprint"

**Locus requirements for Phase 3:**

- [ ] Junction block with forking floor geometry implemented
- [ ] Loop block with back-edge archway and iteration counter implemented
- [ ] Emergency stop and skip gate floor markings implemented
- [ ] Class room with distinct visual treatment implemented
- [ ] Instance room derived from class room implemented
- [ ] Blueprint visual distinct from built workshop visual
- [ ] Inheritance relationship shown as extended blueprint visual
- [ ] Generator room with pause/resume state implemented

---

## Phase 4 — The Supporting Buildings

**Purpose**: complete the factory complex. By the end of Phase 4, the learner understands every zone and can write a complete, structured Python application — with proper input handling, file I/O, error handling, database interaction, and program entry point structure.

**Lessons in this phase:**

| Lesson | Zone | Topic | Status |
|---|---|---|---|
| Lesson 20 | Z1 — Tool Store | Modules and Packages — stocking and fetching | 🔲 Not started |
| Lesson 21 | Z2, Z8 — Goods In, Records Dept | File Handling — crates at the loading bay | 🔲 Not started |
| Lesson 22 | Z7 — Quality Control | Error Handling — the inspection belt | 🔲 Not started |
| Lesson 23 | Z8 — Records Department | Databases — the permanent archive | 🔲 Not started |
| Lesson 24 | Z2 — Goods In | User Input in Depth — work orders and terminals | 🔲 Not started |
| Lesson 25 | Z4 — Shift Manager's Office | Putting It Together — the complete shift | 🔲 Not started |

**What the learner can do after Phase 4:**

- Install and import third-party packages using pip and virtual environments
- Read and write files using the lockable room (`with`) pattern
- Handle errors gracefully at Quality Control with `try/except/finally`
- Read and write to a SQLite database
- Accept and validate user input from the terminal and from `sys.argv`
- Structure a complete Python program with a `main()` function and a proper entry point

**Phase 4 exit criteria:**

- [ ] All six lessons written and vocabulary-compliant
- [ ] Lesson 20 distinguishes "ordering" (`pip install`) from "fetching" (`import`) clearly
- [ ] Lesson 21 uses "receive a crate at the loading bay" for file open and "lockable room" for `with` consistently
- [ ] Lesson 22 uses the full error vocabulary from VOCABULARY.md Part 5 for all examples
- [ ] Lesson 25 introduces Z4 (Shift Manager's Office) as the final zone — the learner has now visited every building in the complex
- [ ] Lesson 25 ends with a complete worked example that touches all five main production line zones

**Locus requirements for Phase 4:**

- [ ] Z1 (Tool Store) zone annotation for `import` statements implemented
- [ ] Z2 (Goods In) zone annotation for `input()` and file reads implemented
- [ ] Z4 (Shift Manager's Office) annotation for entry point implemented
- [ ] Z7 (Quality Control) block with amber floor colouring implemented
- [ ] `finally` always-runs gate visual implemented
- [ ] Z8 (Records Department) annotation for database and file write calls implemented
- [ ] Full error vocabulary visual indicators from VOCABULARY.md Part 5 implemented

---

## Phase 5 — Advanced Systems

**Purpose**: introduce the two remaining supporting buildings that operate outside normal beginner scope — the Testing Laboratory and the CCTV Room. By the end of Phase 5, the learner writes tested, logged code as a matter of habit.

**Lessons in this phase:**

| Lesson | Zone | Topic | Status |
|---|---|---|---|
| Advanced 5a | Z6 — Testing Laboratory | Testing — station checks with pytest | 🔲 Not started |
| Advanced 5b | Z9 — CCTV Room | Logging — permanent records from every zone | 🔲 Not started |

**What the learner can do after Phase 5:**

- Write `pytest` test functions and run a full test suite
- Use `assert` as a station check correctly
- Understand the difference between Testing Lab (pre-production) and Quality Control (live production)
- Replace `print()` debugging with structured `logging` calls
- Set appropriate log levels and write to a log file

**Phase 5 exit criteria:**

- [ ] Both lessons written and vocabulary-compliant
- [ ] Lesson 5a clearly distinguishes the Testing Laboratory from Quality Control using the table in VOCABULARY.md
- [ ] Lesson 5b uses "report to CCTV" for `logging` calls and distinguishes all five severity levels by their monitor colour

**Locus requirements for Phase 5:**

- [ ] Z6 (Testing Lab) annotation for `test_` functions implemented
- [ ] Station check (`assert`) visual indicator implemented
- [ ] Z9 (CCTV Room) annotation for `logging` calls implemented
- [ ] Log severity level colour indicators implemented
- [ ] Execution temperature overlay implemented (Part 4.4 of VOCABULARY.md)

---

## Phase 6 — Concurrency

**Purpose**: introduce the Night Shift Wing. This phase is deliberately last — concurrency is complex and should only be approached once the main factory is fully understood. The learner who reaches Phase 6 is no longer a beginner.

**Lessons in this phase:**

| Lesson | Zone | Topic | Status |
|---|---|---|---|
| Advanced 6a | Z10 — Night Shift Wing | Threading — two lines running in parallel | 🔲 Not started |
| Advanced 6b | Z10 — Night Shift Wing | Async — one worker, no idle time | 🔲 Not started |

**What the learner can do after Phase 6:**

- Write basic `threading.Thread` programs and understand the parallel lines metaphor
- Write basic `asyncio` programs and understand the no-idle-worker metaphor
- Understand when to use threading vs async vs neither
- Understand the risks of concurrent access to shared warehouse shelves (race conditions)

**Phase 6 exit criteria:**

- [ ] Both lessons written and vocabulary-compliant
- [ ] Lesson 6a introduces race conditions as "two lines reaching for the same shelf simultaneously"
- [ ] Both lessons open with the explicit warning from VOCABULARY.md: do not enter this wing until the main factory is running smoothly

**Locus requirements for Phase 6:**

- [ ] Z10 (Night Shift Wing) annotation for `async def` and `threading.Thread` implemented
- [ ] Parallel execution visual — two rooms active simultaneously
- [ ] `await` pause/resume visual implemented
- [ ] Race condition warning indicator on shared shelf items

---

## Milestone Summary

| Milestone | Description | Marker |
|---|---|---|
| **M0** | Foundation complete | All four Phase 0 documents vocabulary-compliant and pushed to repo |
| **M1** | First working program | Learner can write input → store → output after Phase 1 |
| **M2** | Warehouse complete | Learner understands every shelf object and scope after Phase 2 |
| **M3** | Factory Floor complete | Learner can write any procedural or OOP program after Phase 3 |
| **M4** | Full factory | All 25 lessons complete — learner has visited every zone after Phase 4 |
| **M5** | Professional habits | Learner writes tested and logged code after Phase 5 |
| **M6** | Advanced capability | Learner can write concurrent programs after Phase 6 |

---

## Locus Alignment

vivid-python and Locus share VOCABULARY.md as their contract. The following table shows the minimum Locus renderer state required before each phase's lessons are published. Publishing lessons for zones that Locus cannot render breaks the promise of the series.

| Phase | Minimum Locus state before publishing |
|---|---|
| Phase 0 | Z3 shelf rendering with `int` and `str` type materials |
| Phase 1 | All nine core type materials; Z11 annotation |
| Phase 2 | Function room (workstation); scope visualisation; corridor substance |
| Phase 3 | Junction, loop, class room, instance room, blueprint visual |
| Phase 4 | Z1, Z2, Z4, Z7, Z8 annotations; full error vocabulary visuals |
| Phase 5 | Z6, Z9 annotations; execution temperature overlay |
| Phase 6 | Z10 annotation; parallel execution visual |

---

## Document Structure

The repo should reach this structure by the end of Phase 4:

```
vivid-python/
├── README.md
├── VOCABULARY.md
├── ROADMAP.md
├── CONTRIBUTING.md
├── LICENSE
├── factory-overview.md
└── lessons/
    ├── lesson-01-variables.md
    ├── lesson-02-output.md
    ├── lesson-03-data-types.md
    ├── lesson-04-numbers.md
    ├── lesson-05-casting.md
    ├── lesson-06-strings.md
    ├── lesson-07-lists.md
    ├── lesson-08-functions.md
    ├── lesson-09-scope.md
    ├── lesson-10-dictionaries.md
    ├── lesson-11-tuples-and-sets.md
    ├── lesson-12-operators.md
    ├── lesson-13-conditionals.md
    ├── lesson-14-loops.md
    ├── lesson-15-comprehensions.md
    ├── lesson-16-classes.md
    ├── lesson-17-inheritance.md
    ├── lesson-18-generators.md
    ├── lesson-19-lambda-and-decorators.md
    ├── lesson-20-modules.md
    ├── lesson-21-file-handling.md
    ├── lesson-22-error-handling.md
    ├── lesson-23-databases.md
    ├── lesson-24-user-input.md
    ├── lesson-25-putting-it-together.md
    ├── advanced-05a-testing.md
    ├── advanced-05b-logging.md
    ├── advanced-06a-threading.md
    └── advanced-06b-async.md
```

---

*The factory is one world. Build it one room at a time, and build each room before you open the door.*
