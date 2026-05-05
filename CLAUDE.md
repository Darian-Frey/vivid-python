# CLAUDE.md — Handoff Document for Claude Code

> **Project**: vivid-python  
> **Repo**: github.com/Darian-Frey/vivid-python  
> **Handoff date**: 2026-05-05  
> **Prepared by**: Claude (claude.ai chat session), Shane Hartley (series director)  
> **Purpose**: Orient Claude Code to the vivid-python project — what it is, how it works, what the rules are, and what needs doing next.

---

## What This Project Is

**vivid-python** is a Python tutorial series built specifically for learners with **hyperphantasia** — the ability to generate unusually vivid, detailed mental imagery. Rather than teaching Python through abstract definitions and syntax tables, it teaches through a single coherent visual metaphor: a factory.

Every Python concept has a permanent physical location inside that factory. Variables live on shelves in the Warehouse. Functions are workstations on the Factory Floor. Error handling is a Quality Control station. The Records Department stores anything that needs to survive after the shift ends.

The factory is not decoration. It is the lesson. The notation (`age = 27`) is the shorthand; the factory is the understanding.

**This tutorial has a companion project**: [Locus](https://github.com/Darian-Frey/Locus), a VS Code / Antigravity IDE plugin that renders source code as a navigable spatial environment — the same factory, in 3D, live in the editor. Both projects share a single vocabulary contract: `VOCABULARY.md`.

---

## What Has Been Built So Far

The repository currently contains these files:

| File | Purpose | Status |
|---|---|---|
| `README.md` | Public-facing repo introduction | ✅ Complete |
| `VOCABULARY.md` | Shared vocabulary contract v0.2 | ✅ Complete |
| `ROADMAP.md` | Phase structure and completion criteria | ✅ Complete |
| `factory-overview.md` | Full 11-zone factory map | ✅ Complete — needs one minor update (see below) |
| `lessons/lesson-01-variables.md` | Lesson 1 — Variables & Memory | 🟡 Drafted — needs vocabulary compliance pass |

All other lessons (2–25 plus Advanced 5a, 5b, 6a, 6b) are not yet written.

---

## The Single Most Important Rule

**Read `VOCABULARY.md` before writing or editing anything.**

VOCABULARY.md is the contract between this tutorial and the Locus plugin. Every visual term, every zone name, every physical action verb, every banned term is defined there. Violating it is not a style issue — it is a bug that breaks the learner's mental model and breaks the Locus renderer's ability to annotate their code correctly.

The short version of the rules is:

- Use the **canonical zone names** exactly: "The Warehouse", "The Factory Floor", "Goods In", etc. — not synonyms
- Use the **canonical object names** for Python types: stone (`int`), scroll (`str`), vial (`float`), switch (`bool`), numbered row (`list`), sealed crate (`tuple`), unsorted bin (`set`), filing cabinet (`dict`), vacant cubbyhole (`None`)
- Use the **canonical verbs**: "place" for assignment, "replace" for reassignment, "send a job order to" for calling a function, "press into a mould" for casting, "the forklift clears the shelves" for garbage collection
- **Never use banned terms** — the full list is in VOCABULARY.md Part 7. Key ones: "variable" on first introduction (use "named cubbyhole"), "object" in early lessons (use the specific object name), "instantiate" (use "build a workshop from the blueprint"), "garbage collection" (use "the forklift clears the shelves"), "global variable / local variable" (use "open aisle shelf / locked room shelf")
- **Visual, not tactile** — this tutorial is for visual thinkers. Describe how things look; do not instruct the learner to feel, smell, or physically sense objects. "A stone marked 27" is correct. "Pick up the stone and feel its weight" is not.

---

## The Factory — 11 Zones

The full factory has 11 zones. Each lesson declares which zone it is in.

| Zone ID | Name | Python concepts |
|---|---|---|
| Z1 | The Tool Store | `pip`, `venv`, `import` |
| Z2 | Goods In | `input()`, file reads, `sys.argv` |
| Z3 | The Warehouse | Variables, data types, collections, scope |
| Z4 | The Shift Manager's Office | `if __name__ == "__main__"`, `main()`, program structure |
| Z5 | The Factory Floor | Functions, loops, if/else, classes, operators |
| Z6 | The Testing Laboratory | `pytest`, `unittest`, `assert` |
| Z7 | Quality Control | `try/except`, `finally`, validation |
| Z8 | The Records Department | SQLite, MySQL, MongoDB, file writes |
| Z9 | The CCTV Room | `logging` module |
| Z10 | The Night Shift Wing | `async`, `threading`, `asyncio` |
| Z11 | Outgoings | `print()`, file writes for output, `return` |

The main production line runs left to right:

```
[Goods In] ──▶ [Warehouse] ──▶ [Factory Floor] ──▶ [Quality Control] ──▶ [Outgoings]
   Z2              Z3               Z5                    Z7                  Z11
```

---

## Lesson Structure — Every Lesson Must Have These Elements

Every lesson document follows this exact structure. Do not deviate from it.

### 1. Front matter block

```markdown
> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: N of 25  
> **Topic**: [topic name]  
> **Factory zone**: [Zone ID] — [Zone Name]  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second
```

### 2. WHERE YOU ARE diagram

Every lesson opens with an ASCII map of the full factory complex with the current zone highlighted using `██ ZONE NAME ██`. The learner must always know exactly where they are before any content begins. Never omit this.

The highlighted zone format is:
```
██ WAREHOUSE ██
```

Use the standard factory ASCII layout from `factory-overview.md` as the base, then highlight the relevant zone.

### 3. Physical arrival

The first prose section describes entering the zone — walking through a door, approaching a building, taking in the space visually. This is the spatial anchor. If the lesson is returning to a known zone, a brief anchor phrase is sufficient: "You are back in the Warehouse. The cubbyholes are familiar."

### 4. Content sections

Each concept is introduced in this order:
1. Physical description of what is about to happen
2. Code example showing how Python writes it
3. Physical explanation of what the code just did

**Code never comes before the physical scene.** The notation is the label for the experience, not the experience itself.

### 5. Quick Reference table

Every lesson ends with a table mapping Python syntax to the physical warehouse/factory description. Two columns: `Python` and `Warehouse/Factory image`. This table is also the source of Locus tooltip content for tutorial mode.

### 6. Try It section

A short set of code snippets for the learner to run in a Python prompt or editor. Ends with a question that makes the learner pause and visualise what just happened.

### 7. Where Next section

A small table showing the next 3–4 lessons with their zone and topic, so the learner can see where they are going in the factory.

---

## The Lesson Roadmap — 6 Phases

Full details in `ROADMAP.md`. Summary:

| Phase | Lessons | Focus | Status |
|---|---|---|---|
| 0 — Foundation | Overview, Lesson 1 | Z3 Warehouse basics | 🟡 In progress |
| 1 — First Words | Lessons 2–4 | Z11, Z3 | 🔲 Not started |
| 2 — Full Warehouse | Lessons 5–11 | Z3 in depth | 🔲 Not started |
| 3 — Factory Floor | Lessons 12–19 | Z5 | 🔲 Not started |
| 4 — Supporting Buildings | Lessons 20–25 | Z1, Z2, Z4, Z7, Z8 | 🔲 Not started |
| 5 — Advanced Systems | Advanced 5a, 5b | Z6, Z9 | 🔲 Not started |
| 6 — Concurrency | Advanced 6a, 6b | Z10 | 🔲 Not started |

**Lesson 8 (Functions) belongs to Phase 2, not Phase 3.** Functions must be introduced before scope can be fully taught. It is a brief excursion to the Factory Floor entrance from within the Warehouse phase — see `ROADMAP.md` for the reasoning.

---

## What Needs Doing Next — Immediate Tasks

### Task 1: Vocabulary compliance pass on `factory-overview.md`

The factory overview was written before VOCABULARY.md was finalised at v0.2. It needs one update:

- The front matter references `VOCABULARY.md v0.1` — update to `v0.2`
- Scan for any use of "object" in early sections without a canonical name alongside it — replace with the specific object name (stone, scroll, etc.)
- Scan for any use of "global variable" or "local variable" — replace with "open aisle shelf" and "locked room shelf"

### Task 2: Vocabulary compliance pass on `lessons/lesson-01-variables.md`

Lesson 1 was drafted before VOCABULARY.md v0.2 was complete. Run through it against these specific checks:

- First introduction of a variable must use "named cubbyhole" or "labelled shelf" — not just "variable"
- "memory address" must be replaced with "the warehouse's postal number"
- Check all verbs against VOCABULARY.md Part 3 — particularly assignment (should be "place"), reassignment (should be "replace")
- Confirm the WHERE YOU ARE diagram is present and Z3 is highlighted correctly
- Confirm the Quick Reference table maps every code example to its physical description
- Confirm there is no tactile language — no instructions to feel, touch, or physically handle objects

### Task 3: Write Lesson 2 — Syntax & Output

**Zone**: Z11 — Outgoings  
**W3Schools reference**: https://www.w3schools.com/python/python_syntax.asp and https://www.w3schools.com/python/ref_func_print.asp  
**Vocabulary introduced**: calling out across the floor (`print()`), syntax rules  
**Objects introduced**: none new — no new type objects in this lesson  
**Key image**: the dispatch bay on the right side of the complex; a worker at the loading door reading labels aloud  
**Key concepts to cover**:
- Python syntax — indentation as the factory's lane markings
- `print()` — calling out across the factory floor
- String literals, number literals, expressions in `print()`
- f-strings as pre-labelled dispatch tags
- Comments as notes pinned to the shelves — visible to the reader, invisible to the machine

**Lesson 2 must not introduce**: functions, variables beyond what Lesson 1 covered, any Z3 concepts not already established

### Task 4: Write Lesson 3 — Data Types

**Zone**: Z3 — The Warehouse  
**W3Schools reference**: https://www.w3schools.com/python/python_datatypes.asp  
**Vocabulary introduced**: all nine canonical type objects (see VOCABULARY.md Part 2.1)  
**Key image**: a full tour of the warehouse — seeing all the different kinds of objects that can sit on the shelves for the first time  
**Key concepts to cover**:
- All nine core types introduced visually: stone, vial, scroll, switch, numbered row, sealed crate, unsorted bin, filing cabinet, vacant cubbyhole
- `type()` as the warehouse inspector — holds an object up to the light and reads what it is
- Dynamic typing reminder — the cubbyhole accepts any object; the object carries its own identity
- Brief visual overview of each; deep dives come in later lessons

---

## Writing Rules — Summary

These apply to every document in this repo:

1. **Vocabulary first** — check VOCABULARY.md before writing any spatial term
2. **Image before code** — physical scene always precedes the code example
3. **Visual, not tactile** — describe appearance and character; do not instruct the learner to feel things
4. **Canonical verbs** — use Part 3 of VOCABULARY.md for every Python operation
5. **Banned terms** — check Part 7 before every draft; if in doubt, it is probably banned
6. **Zone ID in front matter** — every lesson declares its zone ID
7. **WHERE YOU ARE diagram** — mandatory in every lesson, never omit
8. **Quick Reference table** — mandatory at the end of every lesson
9. **Emotional tone** — calm, purposeful, competent. The factory works. Errors are handled. Programming is learnable.
10. **No dates in ROADMAP.md** — this is a solo project; false deadlines are worse than no deadlines

---

## Repo Structure

Current state:

```
vivid-python/
├── README.md                         ✅
├── VOCABULARY.md                     ✅
├── ROADMAP.md                        ✅
├── CLAUDE.md                         ✅ (this file)
├── factory-overview.md               ✅ (needs minor v0.2 update)
└── lessons/
    └── lesson-01-variables.md        🟡 (needs compliance pass)
```

Target structure at end of Phase 4:

```
vivid-python/
├── README.md
├── VOCABULARY.md
├── ROADMAP.md
├── CLAUDE.md
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

## Relationship with Locus

Locus (github.com/Darian-Frey/Locus) is the companion IDE plugin. It shares VOCABULARY.md as a contract. The critical rule for lesson publishing:

**Do not publish lessons for a zone until Locus can render that zone.**

The Locus alignment requirements for each phase are in `ROADMAP.md` under each phase's "Locus requirements" section. Check those before marking any phase complete.

If VOCABULARY.md needs updating, update it in vivid-python first, then propagate the change to the Locus repo. The canonical version of VOCABULARY.md always lives here.

---

## What This Project Hopes to Achieve

In descending order of ambition:

1. **A complete, vocabulary-compliant 25-lesson Python tutorial** that teaches beginner Python through visual spatial metaphor, usable by any visual-spatial learner regardless of whether they have hyperphantasia.

2. **A companion resource for the Locus IDE plugin** — lessons that map directly to Locus's zone annotations, so a learner using both products experiences the same factory in prose and in their editor simultaneously.

3. **A model for hyperphantasia-informed technical education** — demonstrating that spatial metaphor, consistently applied and rigorously maintained, is a legitimate and effective alternative to definition-first teaching for visual-spatial learners.

4. **A public resource** on GitHub that other educators can fork, adapt, and extend for other languages and domains — with VOCABULARY.md as the pattern for building and maintaining a consistent spatial vocabulary contract across a curriculum.

---

*The factory is one world. Build it one room at a time, and build each room before you open the door.*
