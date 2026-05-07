# VOCABULARY.md — Shared Vocabulary Contract

> **Status**: Active — v0.2
> **Provenance**: Shane Hartley (factory metaphor and tutorial content), Claude (formalisation and Locus mapping)
> **Last reviewed**: 2026-05-05
> **Why**: The canonical reference document that keeps the *Python for Hyperphantasic Minds* tutorial series and the Locus IDE plugin speaking exactly the same language. Every spatial metaphor, every type object, every zone name, and every physical action verb used in either product is defined here. When writing a new lesson, check this document first. When building a new renderer component, check this document first. If this document is silent on something, add to it before proceeding — do not invent vocabulary independently in either product.

---

## How to Use This Document

**Tutorial authors**: before introducing a new spatial metaphor in a lesson, check whether it is already defined here. If it is, use the exact vocabulary listed. If it is not, add it to this document with its Locus mapping before including it in the lesson. Consistency between lessons is what makes the factory feel like one coherent world rather than a collection of separate analogies.

**Locus developers**: before implementing a type material, zone annotation, or shelf rendering, check this document for the tutorial's established vocabulary. The visual character of every rendered object should match the physical character described in the tutorial. A learner who has internalised the tutorial should open Locus and feel recognition, not re-learning.

**Both**: if the tutorial and Locus ever describe the same concept differently, this document is the arbiter. Update this document first, then update both products to match.

---

## Part 1 — The Factory Complex

### 1.1 Zone Names and Identities

These are the canonical names for every zone in the factory. Both the tutorial and Locus use these names exactly — not synonyms, not abbreviations.

| Zone ID | Canonical name | Tutorial description | Locus annotation label | Locus detection signal |
|---|---|---|---|---|
| Z1 | **The Tool Store** | Building on the far left of the complex; floor-to-ceiling shelves of boxed equipment. Stocks the factory via `pip` and `import`. | `Tool Store` | `import` statements at module level |
| Z2 | **Goods In** | Loading bay with wide roller doors. Raw material arrives here: `input()`, file reads, `sys.argv`, API responses. | `Goods In` | Function contains `input()`, file read calls, or `sys.argv` access as primary activity |
| Z3 | **The Warehouse** | Pale grey concrete floor, high ceiling, rows of cubbyholes. All memory and variable storage. Open aisles (global) and locked rooms (local). | `Warehouse` | Data-heavy function; primarily stores and retrieves values with few control structures |
| Z4 | **The Shift Manager's Office** | Elevated office overlooking the whole complex. Program structure and orchestration. `if __name__ == "__main__"`, `main()`, `sys`. | `Shift Manager` | Entry point function; orchestrates calls to all other zones |
| Z5 | **The Factory Floor** | Main production area. Functions, loops, conditionals, classes — where transformation happens. | `Factory Floor` | General processing function; default zone when no other zone is detected |
| Z6 | **The Testing Laboratory** | Separate building adjacent to the Factory Floor. `pytest`, `unittest`, `assert`. Pre-production verification only — not Quality Control. | `Testing Lab` | Function name starts with `test_` or file is in `tests/` directory |
| Z7 | **Quality Control** | Inspection station between the Factory Floor and Outgoings. `try/except`, `finally`, validation. Handles live material in transit. | `Quality Control` | Function whose primary block structure is `try/except` |
| Z8 | **The Records Department** | Permanent archive connected to the Warehouse and Outgoings. SQLite, MySQL, MongoDB, file writes for storage. | `Records Dept` | Function contains database calls or file write operations |
| Z9 | **The CCTV Room** | High corner room with monitors. `logging` module. Records from every zone. Does not produce — only records. | `CCTV` | Function whose primary activity is `logging` calls |
| Z10 | **The Night Shift Wing** | Separate wing through heavy double doors. `async`, `threading`, `asyncio`. Do not enter until the main factory is running smoothly. | `Night Shift` | `async def` function, or function containing `threading.Thread` |
| Z11 | **Outgoings** | Dispatch bay on the right side of the main building. `print()`, file writes for output, `return` to caller. Nothing leaves the factory without passing through here. | `Outgoings` | Function whose primary output is `print()`, a file write for output, or a `return` statement |

### 1.2 The Main Production Line

The left-to-right flow through the heart of the complex:

```
[Goods In] ──▶ [Warehouse] ──▶ [Factory Floor] ──▶ [Quality Control] ──▶ [Outgoings]
   Z2              Z3               Z5                    Z7                  Z11
```

Everything else — Tool Store (Z1), Shift Manager's Office (Z4), Testing Laboratory (Z6), Records Department (Z8), CCTV Room (Z9), Night Shift Wing (Z10) — is a supporting building connected to this main line.

### 1.3 Connections Between Zones

These are the canonical descriptions of how zones connect. Use these descriptions in lessons when explaining how data moves between zones.

- **Tool Store → all zones**: via `import` at the top of the file. A worker fetches the required tools from the Tool Store at the start of the shift and carries them to their workstation.
- **Goods In → Warehouse**: raw material arrives at the loading bay, is reshaped if necessary (casting), and placed in named cubbyholes.
- **Warehouse → Factory Floor**: a worker retrieves an object from its shelf and brings it to the workstation's input slot.
- **Factory Floor → Warehouse**: a workstation sends back a result to be stored under a new or existing label.
- **Factory Floor → Quality Control**: everything the Factory Floor produces passes through the inspection station before dispatch.
- **Quality Control → Outgoings**: approved material is dispatched. Faulty material is intercepted and handled — fixed or discarded.
- **Factory Floor → Records Department**: data that must survive after the program ends is sent here before the shift closes.
- **Outgoings → Records Department**: for output that must persist, the dispatch team sends a package to the Records Department before the shift ends.
- **CCTV Room ← all zones**: the CCTV Room receives feeds from everywhere. It does not produce anything — it only records.
- **Testing Laboratory ← Factory Floor**: workstations are brought in isolation to the Testing Laboratory before the shift begins. Live production material never enters the Testing Laboratory.

---

## Part 2 — Physical Objects (Types)

Every Python type has a canonical physical object that represents it. This object is used consistently in the tutorial's prose, in the tutorial's ASCII diagrams, and in Locus's type material rendering.

The physical character of each object (shape, visual appearance, weight as described in prose) defines how Locus renders the corresponding type material on shelves and as substance on corridors.

**Note on weight in tutorial prose**: weight is a permitted descriptive property in the tutorial — it conveys importance, persistence, and complexity — but it is described as a character quality of the object, not as a physical sensation the learner is asked to feel. "A heavy manuscript" is permitted. "You feel the weight of it in your hands" is tactile and should be avoided.

### 2.1 Core Type Objects

| Python type | Canonical object | Physical character | Locus material properties | Tutorial introduced |
|---|---|---|---|---|
| `int` | **Stone** | Smooth, dense, compact. Small enough to hold in one hand. Number painted on it in white. Hard-edged. Has presence and weight. | Dark metallic. Hard-edged geometry. Small. Dense. Minimal surface detail. | Lesson 1 |
| `float` | **Vial** | Small glass vial with a stopper. The liquid inside shifts when tilted. Number etched on the glass. Precise and slightly fragile-looking. | Slightly translucent sphere/capsule. Smooth surface. Faint internal glow. Small but visually distinct from stone. | Lesson 4 |
| `bool` | **Switch** | A small physical toggle. One of two positions — up or down. No intermediate state. The click is definitive. | Binary visual — two distinct states. Minimal geometry. Small. High contrast between True and False states. | Lesson 3 |
| `str` | **Scroll** | A roll of parchment. Length proportional to content. Characters visible on the surface if you look closely. Tied with a ribbon for short strings; a full heavy roll for long ones. | Elongated ribbon geometry. Length proportional to string length (bounded). Pale, slightly translucent. Text-like surface texture. | Lesson 1 |
| `list` | **Numbered row** | A row of cubbyholes bolted together on a wheeled frame, numbered from zero. Each cubbyhole holds one object. The frame can be extended. | Grouped cluster of smaller shapes in sequence. Count indicator visible. Can grow/shrink dynamically. Colour reflects dominant element type. | Lesson 3 |
| `tuple` | **Sealed crate** | A crate that has been nailed shut. Objects packed inside are visible through gaps in the slats. Cannot be repacked. Weight is fixed at packing time. | Grouped cluster, rigid. Distinct locked/sealed visual cue. Geometry slightly heavier than list. Cannot change. | Lesson 3 |
| `set` | **Unsorted bin** | A large open bin. Objects thrown in — no order, no duplicates allowed (duplicates simply fail to land). You can check whether something is in the bin but cannot retrieve objects by position. | Irregular cluster. No sequence indicators. Objects arranged chaotically. No index markers. | Lesson 3 |
| `dict` | **Filing cabinet** | A steel cabinet with labelled drawers. Each drawer has a key on the front (the dictionary key) and holds one object (the value). Retrieved by key, not by position. Drawers are added or removed freely. | Structured object with visible key/value indicators. Heavier than list. Index markers visible on surface. Count indicator present. | Lesson 3 |
| `None` | **Vacant cubbyhole** | A shelf that has a name card but no object inside. The card is pinned but the space is empty. Not a stone marked zero — genuinely nothing there. | Visually absent. Empty container shape. Distinct visual cue of vacancy: hollow, faint outline only. | Lesson 3 |
| `bytes` | **Sealed scroll** | A scroll that has been wax-sealed. Something is inside but it cannot be read without breaking the seal (decoding). Heavier than a regular scroll for the same content. | Dark, dense ribbon. Sealed visual. Less translucent than str. | Later lesson |
| `complex` | **Crystal** | A geometric crystal with two distinct axes visible in its structure — the real and imaginary parts as different facets. Mathematical and precise. | Geometric crystal form. Two distinct visual axes. Transparent with internal refraction. | Advanced |

### 2.2 Function and Class Objects

| Python concept | Canonical object | Physical character | Locus representation | Tutorial introduced |
|---|---|---|---|---|
| `function` (def) | **Workstation** | A physical workstation on the Factory Floor. Has a name plate on the front, an input slot, and an output slot. Receives material in; sends finished product out. | Function room in Locus. | Lesson 8 |
| `function call` | **Job order** | A physical job order handed to a workstation. Carries the materials (arguments) to be processed. The workstation processes them and sends back a result. | Corridor with substance between rooms. | Lesson 8 |
| `return value` | **Finished product** | The output of a workstation — the result of processing, labelled and sent back along the internal belt. | Return substance on corridor (reverse direction). | Lesson 8 |
| `parameter` | **Input slot** | The labelled slot on the front of the workstation where incoming material is placed. Each slot is labelled with the parameter name. | Corridor entry doorway; argument types visible as incoming substance. | Lesson 8 |
| `argument` | **Delivered material** | The actual object placed into the input slot when the job order arrives. May differ in type from what the workstation expects — which triggers a TypeError. | Type substance flowing along corridor. | Lesson 8 |
| `class` | **Workshop blueprint** | A detailed technical drawing of a specialised workshop — its layout, its shelves, its built-in workstations. The blueprint itself is not a workshop; built workshops are. | Class room with distinct visual treatment. | Lesson 16 |
| `instance` | **Built workshop** | A workshop constructed from the blueprint. Has all the shelves and built-in workstations the blueprint specified, plus its own live state. | Instance room derived from class room. | Lesson 16 |
| `method` | **Built-in workstation** | A workstation that comes pre-installed when a workshop is built from the blueprint. Every instance of the class has one. | Method room within class room. | Lesson 16 |
| `lambda` | **Impromptu workstation** | A temporary workstation assembled on the spot for a single job. No name plate. | Inline lambda visual — smaller than a full workstation room. | Lesson 19 |
| `decorator` | **Workstation wrapper** | An additional layer applied around a workstation that activates automatically before and after every job — a quality check, a timer, a logger. | Overlay wrapping the function room. | Later lesson |
| `generator` | **Passing one item and waiting** | A workstation that passes one item along the belt and waits for the next request, rather than completing all processing at once. | Generator room with pause/resume visual. | Lesson 18 |

### 2.3 Control Flow Objects

| Python concept | Canonical object | Physical character | Locus block representation | Tutorial introduced |
|---|---|---|---|---|
| `if / elif / else` | **Junction with inspection gates** | A point on the factory floor where material is examined and sent down one of several routes. Each route is clearly labelled. The first gate to open takes the item; subsequent gates never see it. | Conditional block with forking floor geometry. True branch exits east. False branch exits west. | Lesson 13 |
| `for loop` | **Conveyor belt with counter** | A circular conveyor that takes a numbered row of objects and feeds them one at a time to a workstation. When the row is empty, the belt stops. | Loop block with back-edge archway in rear wall. Floor shows iteration count during execution. | Lesson 14 |
| `while loop` | **Conveyor belt with sensor** | A circular conveyor that keeps running as long as a sensor reads a condition as True. The sensor checks at each pass. | Loop block with condition text on ceiling. Back-edge archway in rear wall. | Lesson 14 |
| `break` | **Emergency stop** | A large red button that halts the conveyor immediately, regardless of what is still on it. | Distinct floor marking at break location. Execution stops visually at this point. | Lesson 14 |
| `continue` | **Skip gate** | A gate that opens partway through a conveyor pass, letting the current item fall through to the start of the next pass without completing processing. | Skip marker in floor. | Lesson 14 |
| `try / except` | **Quality control station** | A station between the workstation output and the dispatch bay. Material passes through; if it fails inspection, it is diverted to a handling procedure. | Quality Control block with distinct floor colouring (amber). | Lesson 22 |
| `finally` | **Always-runs gate** | A final gate that every item passes through regardless of whether it passed or failed quality control. Used for cleanup, closing files, releasing resources. | `finally` block floor marking. Always-executed visual path. | Lesson 22 |
| `with` statement | **Lockable room with guaranteed exit** | A locked room that you enter to use a resource. When you leave — for any reason, including an error — the room clears itself and the door unlocks automatically. | Context manager block. Distinct entry/exit visual. Guaranteed cleanup path visible. | Lesson 21 |
| `yield` | **Pass one item and wait** | A workstation that passes one item along the belt and pauses until the next request arrives. | Generator room — pause/resume state visible. | Lesson 18 |

### 2.4 Scope Objects

| Python concept | Canonical object | Physical character | Locus representation | Tutorial introduced |
|---|---|---|---|---|
| Local scope | **Locked room within the warehouse** | Accessible only to the workstation it belongs to. Cleared completely when the workstation finishes its job. Its shelves never existed, as far as the rest of the warehouse is concerned. | Interior of a function room. Shelves visible only when inside. | Lesson 1 (preview), Lesson 9 (full) |
| Global scope | **Open aisle in the warehouse** | Accessible to any workstation in the factory. Persists for the duration of the shift. | Module-level variables shown in a special pre-entry area at the architectural zoom level. | Lesson 9 |
| Enclosing scope | **Anteroom** | A room between the open aisle and a locked room. Belongs to an outer function; accessible to inner functions nested within it. | Visually between outer and inner function rooms. | Lesson 9 |
| Built-in scope | **Factory standard kit** | Tools and materials that come pre-installed in every factory — `print`, `len`, `range`, `type` etc. Always available without fetching from the Tool Store. | Pre-populated shelf visible in every room. Distinct from user-defined shelf items. | Lesson 9 |

---

## Part 3 — Physical Actions (Verbs)

The tutorial uses specific physical verbs for specific Python operations. Use these verbs consistently — they become the learner's instinctive language for thinking about code operations.

| Python operation | Canonical verb | Example in prose |
|---|---|---|
| `x = value` (assignment) | **Place** | "Place a stone marked `27` into the cubbyhole." |
| `x = new_value` (reassignment) | **Replace** | "Remove the `27` stone. Replace it with a `28` stone." |
| Passing an argument | **Hand** | "Hand the scroll to the workstation's input slot." |
| Returning a value | **Send back** | "The workstation sends back a stone marked `True`." |
| Calling a function | **Send a job order to** | "Send a job order to the `calculate_discount` workstation." |
| Importing a module | **Fetch from the Tool Store** | "Fetch the `math` toolkit from the Tool Store." |
| `pip install` | **Order** | "Order the `requests` package from PyPI." |
| `venv` creation | **Build a private Tool Store** | "Build a private Tool Store for this factory." |
| Raising an exception | **Trigger the alarm** | "The workstation triggers the alarm — it received a scroll where it expected a stone." |
| Catching an exception | **Intercept at Quality Control** | "Quality Control intercepts the faulty item before it reaches Outgoings." |
| `finally` | **Close the room regardless** | "The room is cleared regardless — this gate always closes." |
| Opening a file | **Receive a crate at the loading bay** | "A crate arrives at the loading bay, sealed and labelled." |
| Writing to a file (output) | **Pack and dispatch** | "The dispatch team packs the result into a crate and sends it to the depot." |
| Writing to a file (storage) | **Send a package to Records** | "Send the finished record to the Records Department." |
| `print()` | **Call out across the floor** | "The workstation calls out the result across the factory floor." |
| Comment (`# ...`) | **Note pinned to the shelves** | "A note pinned to the shelf is for the human reader; the machine walks past it." |
| Indentation (block syntax) | **Lane markings on the floor** | "Lane markings group lines into one run of work; every line inside the same lane belongs to the same job." |
| f-string (`f"...{name}..."`) | **Fill-in scroll** | "A fill-in scroll has named windows; at the moment the scroll is sealed, each window is filled with the current value of the named cubbyhole it points to." |
| `logging.info()` etc. | **Report to CCTV** | "The workstation reports to CCTV: shift started." |
| `append()` on a list | **Add to the numbered row** | "Add the new stone to the end of the numbered row." |
| `dict[key]` lookup | **Open the labelled drawer** | "Open the drawer labelled `player_name` and retrieve what is inside." |
| Type casting | **Press into a mould** | "Press the `27` stone into a scroll mould. The scroll reads `'27'`." |
| `None` | **Vacant cubbyhole** | "The cubbyhole has a name card but nothing inside — it is vacant." |
| `True` | **Switch up** | "The switch is in the up position." |
| `False` | **Switch down** | "The switch is in the down position." |
| Scope entry (function call) | **Enter the locked room** | "The workstation begins its job. The locked room is entered." |
| Scope exit (function return) | **The room is cleared** | "The workstation finishes. The locked room is cleared. Its shelves never existed, as far as the rest of the warehouse is concerned." |
| Program start | **The shift begins** | "The Shift Manager reads the work order and the shift begins." |
| Program end | **The shift ends** | "The forklift moves through the warehouse. The shift is over." |
| Memory cleared at program end | **Forklift clears the shelves** | "The forklift clears every shelf. The cubbyholes return to numbered blankness." |
| `for item in list` | **Each item on the conveyor belt** | "Each stone on the numbered row arrives at the workstation in turn." |
| `while condition` | **Keep running while the sensor reads True** | "The conveyor keeps running while the sensor reads True." |
| `break` | **Hit the emergency stop** | "Hit the emergency stop — the conveyor halts immediately." |
| `continue` | **Skip gate opens** | "The skip gate opens. This item falls through to the next pass." |
| `try` | **Begin quality control watch** | "Quality Control begins watching the output." |
| `except` | **Divert the faulty item** | "A faulty item is diverted. Quality Control handles it." |
| `with` | **Enter the lockable room** | "Enter the lockable room. When you leave, it will close itself." |
| `return` | **Send back the finished product** | "The workstation sends back the finished product." |
| `yield` | **Pass one item and wait** | "The workstation passes one item along the belt and waits for the next request." |
| `raise` | **Trigger the alarm manually** | "The workstation triggers the alarm manually — it has detected a problem." |
| `assert` | **Station check** | "The Testing Lab runs a station check: is this what we expected?" |
| `class` | **Draft a blueprint** | "Draft the blueprint for a new type of workshop." |
| Creating an instance | **Build a workshop from the blueprint** | "Build a new workshop from the `Vehicle` blueprint." |
| `__init__` | **Fit out the workshop** | "When a workshop is built from the blueprint, fit it out with the standard shelves and equipment." |
| `self` | **This workshop** | "This workshop — not the blueprint, not another built workshop, this one." |
| `@decorator` | **Wrap the workstation** | "Wrap this workstation with an additional layer — a quality check, a timer, a logger — that activates before and after every job." |
| `sys.argv` | **Read the work order** | "The Shift Manager reads the work order passed in at the start of the shift." |

---

## Part 4 — Spatial Properties

These are the physical properties that give each object its character. The tutorial uses them in prose descriptions. Locus uses them in rendering. They must be consistent between both products.

**Note on tactile vs visual**: this tutorial is primarily visual — it is designed for learners who build vivid mental images. Spatial properties like weight and size are used as descriptive character qualities (a heavy manuscript; a compact stone) not as physical sensations the learner is asked to feel. Tutorial authors should describe the appearance and character of objects, not simulate handling them.

### 4.1 Size

Size reflects data size and complexity:

| Object | Size convention |
|---|---|
| `bool` / `None` | Minimal — smallest objects on any shelf |
| `int` / `float` | Small — compact, one-hand sized |
| `str` (short) | Small-medium — a pamphlet-roll |
| `str` (long) | Medium — a full scroll, noticeably substantial |
| `list` / `set` (small) | Medium — a short numbered row |
| `list` / `set` (large) | Large — a long numbered row, substantial |
| `dict` (small) | Medium — a slim filing cabinet |
| `dict` (large) | Large — a full filing cabinet |
| `tuple` | Same as equivalent list, but visually sealed and fixed |
| `class instance` | Variable — depends on the blueprint's fields |

### 4.2 Weight as Character

Weight in the tutorial conveys importance, persistence, and complexity. It is a descriptive character quality, not a tactile instruction. Use it to communicate the significance of an object.

| Object | Weight character |
|---|---|
| `bool` | Negligible — almost nothing to it |
| `int` / `float` | Satisfying — has clear presence but is easily managed |
| `str` (short) | Light — a roll of paper |
| `str` (long) | Moderate — a thick manuscript |
| `list` / `dict` (populated) | Substantial — requires both hands to carry |
| `list` / `dict` (empty) | Light — the frame without the contents |
| Database record | Heavy and permanent |

### 4.3 Colour and Material in Locus

Constraints on the Locus canonical renderer. These ensure the rendered objects evoke the physical character described in the tutorial prose.

| Object | Colour | Material character |
|---|---|---|
| `int` (stone) | Dark grey to charcoal | Hard, matte, dense. No transparency. Sharp edges. |
| `float` (vial) | Clear with pale blue tint | Slightly translucent. Smooth surface. Soft internal glow. |
| `bool` (switch) | Bright green (True) / dark red (False) | Two distinct states. Minimal geometry. No gradients. |
| `str` (scroll) | Pale cream to warm white | Slightly translucent. Elongated. Soft edges. |
| `list` (numbered row) | Blue-grey cluster | Grouped shapes in sequence. Index markers visible. |
| `tuple` (sealed crate) | Darker blue-grey | Same as list but with a sealed/rigid visual. Locked indicator present. |
| `set` (unsorted bin) | Mixed, irregular | Chaotic arrangement. No index markers. |
| `dict` (filing cabinet) | Steel grey with amber label markers | Structured. Key indicators visible on surface. |
| `None` (vacant) | Hollow outline only | No fill. Ghost-like presence. Clearly empty. |
| `bytes` (sealed scroll) | Dark olive / near-black | Dense. Opaque. Sealed visual. |
| `complex` (crystal) | Clear with prismatic refraction | Two visible axes. Geometric. Transparent with internal colour split. |

### 4.4 Execution Temperature in Locus

Execution temperature is separate from type colour. Temperature overlays on top of type material to show how often a function is being called.

| Execution state | Temperature colour | Physical metaphor |
|---|---|---|
| Never executed | Deep cool blue | Cold — the workstation has not been running |
| Rarely executed | Neutral grey-blue | Ambient temperature |
| Occasionally executed | Warm neutral | A machine that has been running |
| Frequently executed | Amber | A workstation running hot |
| Hottest path | Deep amber-red | A machine at full capacity |

The tutorial refers to execution temperature in the CCTV Room zone description. The CCTV monitors show heat in real time — a function that runs constantly produces visible warmth in the renderer.

---

## Part 5 — Error Vocabulary

Errors have a consistent physical vocabulary across both the tutorial and Locus. The factory context removes the fear from errors — they are handled conditions, not disasters.

| Python error | Canonical description | Locus visual |
|---|---|---|
| `SyntaxError` | "The job order is illegible — the workstation cannot read it." | Red indicator at the relevant block before simulation begins. |
| `NameError` | "A name card has been pinned up but no cubbyhole exists with that label — the worker reaches for a shelf that isn't there." | Amber indicator on shelf where the missing variable would be. |
| `TypeError` | "A stone arrived at a slot designed for a scroll. The workstation cannot process it." | Mismatched substance indicator on the corridor carrying the incompatible argument. |
| `IndexError` | "The numbered row has seven positions. The job order asked for position twelve. There is no position twelve." | Error state on the relevant list shelf item. |
| `KeyError` | "The job order asked for drawer `player_name` but that drawer does not exist in this filing cabinet." | Error state on the relevant dict shelf item. |
| `ValueError` | "The right kind of object arrived — a scroll — but its contents cannot be processed. The scroll reads `'hello'` and the workstation was asked to convert it to a stone." | Error state at the block where the conversion was attempted. |
| `AttributeError` | "The job order asked for a built-in workstation that this workshop does not have. The blueprint never included it." | Error state on the relevant function room. |
| `ImportError` | "An order was sent to the Tool Store but the item is not on the shelves — it has not been installed." | Error state on the import entry point. |
| `ZeroDivisionError` | "The job order asked for the material to be divided, but the divisor stone is marked zero. Division by zero is not a valid factory operation." | Error state at the division operation block. |
| `FileNotFoundError` | "A crate was expected at the loading bay but no lorry arrived. The file does not exist." | Error state at the Goods In entry point. |
| `RuntimeError` | "Something went wrong during the shift that the program cannot recover from in the normal way." | Exception rupture visual on the room where it occurred. |
| Unhandled exception | "The alarm goes off and no one is at Quality Control to handle it. The shift ends immediately." | Full rupture visual. Camera navigates to the exception room. Entire shift halts. |

---

## Part 6 — Tutorial Lesson Map

This section tracks which zones and object types are introduced in each lesson. Locus's tutorial mode uses this map to correctly annotate a beginner's code based on where they are in the series.

| Lesson | Zone introduced | Objects introduced | Concepts introduced |
|---|---|---|---|
| Overview | All zones (preview) | All objects (visual preview only) | The complete factory complex |
| 1 | Z3 — Warehouse | Stone (`int`), scroll (`str`) | Named cubbyholes, assignment, reassignment, dynamic typing, naming rules, scope (locked rooms preview) |
| 2 | Z11 — Outgoings | — | `print()`, syntax, expressions, calling out across the floor |
| 3 | Z3 — Warehouse | Vial (`float`), switch (`bool`), vacant (`None`), numbered row (`list`), sealed crate (`tuple`), unsorted bin (`set`), filing cabinet (`dict`) | All core types in overview |
| 4 | Z3 — Warehouse | Stone (`int`), vial (`float`) in depth | Arithmetic operators, integer vs float, `//`, `%`, `**` |
| 5 | Z3 → Z5 — Warehouse → Factory Floor | Mould (casting operation) | `int()`, `float()`, `str()`, `bool()` — pressing objects into moulds |
| 6 | Z3 — Warehouse | Scroll (`str`) in depth | String methods, indexing, slicing, f-strings |
| 7 | Z3 — Warehouse | Numbered row (`list`) in depth | List methods, indexing, slicing, mutability |
| 8 | Z5 — Factory Floor | Workstation (function), job order (call), input slot (parameter), delivered material (argument), finished product (return) | `def`, `return`, function calls, arguments |
| 9 | Z3 — Warehouse (locked rooms in full) | Anteroom (enclosing scope), factory standard kit (built-in scope) | Scope: local, global, enclosing, built-in |
| 10 | Z3 — Warehouse | Filing cabinet (`dict`) in depth | Dict methods, iteration, nested dicts |
| 11 | Z3 — Warehouse | Sealed crate (`tuple`), unsorted bin (`set`) in depth | Tuple immutability, set operations |
| 12 | Z5 — Factory Floor | — | Operators in depth: comparison, logical, membership, identity |
| 13 | Z5 — Factory Floor | Junction with inspection gates (if/elif/else) | Conditional logic |
| 14 | Z5 — Factory Floor | Conveyor belt (for/while), emergency stop (break), skip gate (continue) | Loops |
| 15 | Z5 — Factory Floor | — | Comprehensions — compact loop expressions |
| 16 | Z5 — Factory Floor | Workshop blueprint (`class`), built workshop (instance), built-in workstation (method) | OOP: classes, instances, methods, `__init__`, `self` |
| 17 | Z5 — Factory Floor | — | Inheritance — extending a blueprint |
| 18 | Z5 — Factory Floor | Generator workstation (`yield`) | Iterators and generators |
| 19 | Z5 — Factory Floor | Impromptu workstation (`lambda`), workstation wrapper (`@decorator`) | Lambda functions, higher-order functions, decorators |
| 20 | Z1 — Tool Store | — | Modules, packages, `pip`, `venv`, `import` |
| 21 | Z2, Z8 — Goods In, Records Dept | Crate (file), lockable room (`with`) | File handling — reading and writing |
| 22 | Z7 — Quality Control | Quality control station (`try/except`), always-runs gate (`finally`) | Error handling |
| 23 | Z8 — Records Department | — | Databases: SQLite, intro to MySQL and MongoDB |
| 24 | Z2 — Goods In | Work order (`sys.argv`) | User input in depth, command-line arguments |
| 25 | Z4 — Shift Manager's Office | — | Program structure, `if __name__ == "__main__"`, `main()`, putting it all together |
| Advanced Phase 5 | Z6, Z9 — Testing Lab, CCTV Room | — | `pytest`, `unittest`, `logging` |
| Advanced Phase 6 | Z10 — Night Shift Wing | — | `async`, `threading`, `asyncio` |

---

## Part 7 — Vocabulary That Is Banned

These terms are not to be used in either the tutorial or Locus. Either they are too abstract for the audience, they conflict with the established physical vocabulary, or they are ambiguous across contexts.

| Banned term | Use instead | Reason |
|---|---|---|
| "variable" (without context) | "named cubbyhole" or "labelled shelf" | Too abstract on first introduction |
| "memory address" (in tutorial) | "the warehouse's postal number" | The learner does not need to think about this |
| "object" (in early lessons, before Lesson 8) | Use the specific object name: stone, scroll, switch | Too abstract; the specific physical object is more useful |
| "data structure" (in early lessons) | Use the specific object name: numbered row, filing cabinet | Same reason |
| "method" (before Lesson 16) | "built-in workstation" | Not yet formally introduced |
| "attribute" (before Lesson 16) | "shelf item belonging to the workshop" | Not yet formally introduced |
| "null" | "vacant cubbyhole" | `null` is not Python — it is `None`, the vacant cubbyhole |
| "undefined" | "vacant cubbyhole" or "the shelf doesn't exist" | Python does not have `undefined`; do not import the concept |
| "pass by reference / pass by value" | "hand the object" or "hand a copy of the object" | Too technical; describe the physical action instead |
| "garbage collection" | "the forklift clearing the shelves" | The physical metaphor is more vivid and equally accurate |
| "heap" / "stack" | Avoid entirely in the tutorial | Implementation details that do not serve the hyperphantasic learner |
| "compile" | "read the work order" | Python is interpreted; "compile" implies a different execution model |
| "execute" | "run" or "the shift begins" | More natural in the factory context |
| "instantiate" | "build a workshop from the blueprint" | More physical and concrete |
| "Boolean" (capital B, as a noun) | "switch" or "True/False value" | The capital-B noun is alienating; use the physical object |
| "pointer" | "a name card pointing at a shelf" | Pointer implies C-style memory pointers; Python references are different |
| "Classes and Objects" (as a section title) | "Workshop Blueprints and Built Workshops" | "object" before Lesson 8 is banned; the physical terms are richer |
| "global variable" / "local variable" | "open aisle shelf" / "locked room shelf" | The spatial terms are the vocabulary; the abstract terms are what we are replacing |

---

## Part 8 — ASCII Diagram Standards

Both the tutorial and Locus (in its tutorial mode overlay) use ASCII diagrams to show spatial structure. These conventions ensure visual consistency across all documents.

### Zone representation in the factory map

```
┌──────────────┐   — Standard zone box
│  ZONE NAME   │   — Zone name in capitals inside the box
└──────────────┘

██ ZONE NAME ██    — Highlighted zone (YOU ARE HERE)

═══════════════    — Separator for distinct wings or sections
```

### Flow arrows

```
──▶    — Data flow direction (horizontal)
 │
 ▼     — Data flow direction (vertical)

────── — Connection without directional flow
- - -  — Indirect or optional connection
```

### Object representation on shelves (tutorial diagrams)

```
[name: type      ]   — Shelf item: name and type
[age: int   = 27 ]   — Shelf item with live value (during execution)
[                ]   — Empty shelf slot
```

### Corridor representation (Locus diagrams)

```
──[arg: int]──▶    — Corridor with argument type label
◀──[ret: bool]─    — Return corridor with type label
```

---

## Part 9 — Notes for Lesson Authors

**Introducing new zones**: when a lesson first enters a new zone, begin with a visual arrival — describe approaching the building, walking through a door, taking in the space. The learner should always know exactly where they are in the complex before any technical content is introduced.

**Returning to known zones**: when a lesson returns to a zone the learner has visited before, do not re-describe the whole space. A brief anchor phrase is sufficient: "You are back in the Warehouse. The cubbyholes are familiar now." Trust the spatial memory that earlier lessons built.

**The WHERE YOU ARE diagram**: every lesson must open with a version of the factory map with the current zone highlighted as `██ ZONE NAME ██`. This is the spatial orientation device that ensures the learner always knows their position in the complex. Never omit it.

**Code comes after the image**: the physical description of what is about to happen must always precede the code example that shows how Python writes it. Never introduce code without first placing the learner in the physical scene. The notation is the label for the experience, not the experience itself.

**Visual, not tactile**: this tutorial is designed for visual thinkers. Describe the appearance and character of objects. Do not instruct the learner to feel, smell, hear, or physically experience them. "A stone marked 27" is correct. "Pick up the stone and feel its weight" is not.

**The quick reference table**: every lesson ends with a quick reference table mapping Python syntax to the warehouse/factory physical description. This table is the learner's cheat sheet and is also the source of Locus's tooltip content for tutorial mode.

**Emotional tone**: the factory is a place of calm, purposeful activity. It is not dramatic. It is not threatening. Workers are competent and the systems work as designed. Errors are handled at Quality Control — they do not mean disaster. The tone of every lesson should communicate that programming is comprehensible, physical, and learnable by anyone who can walk through a building.

**Zone ID in the lesson header**: every lesson document must include the zone ID (e.g., `Z3 — The Warehouse`) in its front matter so Locus can correctly annotate code for learners at that stage of the series.

---

*This document is the contract between the tutorial and the plugin. Update it when either product needs to introduce new vocabulary. Never let the two products develop separate vocabularies for the same concept. The factory is one world.*
