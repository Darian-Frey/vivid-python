# Python for Hyperphantasic Minds
## Advanced Phase 5a — Testing

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus + standard professional Python practice  
> **Lesson**: Advanced 5a (after the main 25 lessons)  
> **Topic**: Testing — station checks with `pytest`  
> **Factory zone**: Z6 — The Testing Laboratory  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Post-beginner — assumes the main series is complete  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You enter **the Testing Laboratory** for the first time. The Lab is a separate building next to the Factory Floor — not part of the main production line. Workstations are brought here in isolation, *before* the shift begins, to be verified against carefully prepared test inputs. Only workstations that pass their checks are signed off and allowed onto the live floor.

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
┌──────────┐    ┌─────────────────┐  ┌──────────┐
│ QUALITY  │    │ ██ TESTING ██   │  │ RECORDS  │
│ CONTROL  │    │      LAB        │  │  DEPT    │
└────┬─────┘    │ YOU ARE HERE    │  └──────────┘
     │          └─────────────────┘
     ▼
┌──────────────┐
│  OUTGOINGS   │
└──────────────┘
```

This is a deliberately separate building. The work that happens here does not affect any live production. The workstations being tested have not been signed off yet; they sit on isolated benches with controlled inputs. When the Lab passes a workstation, it earns a stamp and goes to the Factory Floor. When the Lab rejects one, the workstation goes back to its builder for repair before the shift can begin.

---

## The Lab is Not Quality Control

The single most important distinction in this lesson — and one beginners reliably get wrong — is that **the Testing Laboratory is not Quality Control**. Both deal with verification; both protect the factory from defective work. But they operate at different times, on different material, with different tools, for different reasons.

The canonical comparison is in VOCABULARY.md Part 1.4. The summary:

| Aspect | Testing Laboratory (Z6) | Quality Control (Z7) |
|---|---|---|
| When active | Before the shift begins | During the live shift |
| Material handled | Synthetic test inputs | Real production items |
| Python tools | `pytest`, `unittest`, `assert` | `try` / `except` / `finally` |
| Failure mode | Test fails; workstation rejected | Alarm caught; shift continues |
| Question asked | *"Does this workstation work?"* | *"Did this specific job go wrong?"* |
| Lesson | Advanced 5a (this lesson) | Lesson 22 |

Both are useful. Both are common. Neither replaces the other. A well-built program tests its workstations *before* deploying them (Lab), then catches alarms gracefully when something goes wrong in production (QC). Skipping the Lab means shipping bugs; skipping QC means crashing on the first surprise.

The rest of this lesson is about the Lab.

---

## The Simplest Station Check — `assert`

Python has a built-in keyword for "if this is not true, trigger the alarm immediately": `assert`.

```python
def double(n):
    return n * 2

assert double(3) == 6
assert double(0) == 0
assert double(-1) == -2
```

Each `assert` is a **station check**. The expression to the right of `assert` is evaluated; if it is truthy (Lesson 5's truthiness rule), the check passes silently. If it is falsy, Python triggers `AssertionError` and the program halts.

For one-off scripts and quick "sanity checks during development", `assert` is the right tool. For real test suites — many tests, organised, runnable as a batch — you reach for a test framework.

A small but important note: `assert` checks are *removed* when Python is run with the `-O` ("optimise") flag. Production code should not rely on assertions for runtime safety. Use them for testing and for documenting invariants; use proper `if … raise` (Lesson 22) for runtime checks that must always run.

---

## `pytest` — The Industry-Standard Test Workshop

`pytest` is the de facto standard testing toolkit for Python. It is not part of the standard library — you order it from PyPI:

```
$ pip install pytest
```

After installation, the pattern is:

1. Create a file whose name starts with `test_` (e.g. `test_double.py`).
2. Inside, write workstations whose names start with `test_`.
3. Inside each, use plain `assert` statements.
4. Run `pytest` from the terminal.

The minimum complete example:

```python
# test_double.py

def double(n):
    return n * 2


def test_double_positive():
    assert double(3) == 6


def test_double_zero():
    assert double(0) == 0


def test_double_negative():
    assert double(-1) == -2
```

From the terminal:

```
$ pytest
============================ test session starts ============================
collected 3 items

test_double.py ...                                                    [100%]

============================ 3 passed in 0.01s =============================
```

Three station checks, all passed. The three dots in `test_double.py ...` represent the three passed tests.

`pytest` finds tests automatically based on naming conventions (file `test_*.py`, function `test_*`). You do not register tests anywhere; you just create them with the right names. This is the closest Python gets to "ergonomics by convention".

If a test fails, `pytest` shows the failing assertion with rich context:

```
$ pytest
============================ test session starts ============================
collected 3 items

test_double.py ..F                                                    [100%]

================================= FAILURES =================================
___________________________ test_double_negative ___________________________

    def test_double_negative():
>       assert double(-1) == 0
E       assert -2 == 0
E        +  where -2 = double(-1)

test_double.py:14: AssertionError
========================== short test summary info ==========================
FAILED test_double_negative - assert -2 == 0
```

Notice `pytest` does not just say "assertion failed"; it shows exactly what each side of the assertion evaluated to (`assert -2 == 0  +  where -2 = double(-1)`). This is one of the reasons `pytest` is preferred over alternatives — its failure output is much easier to read than what a bare `assert` produces.

---

## Common Assertion Shapes

Most tests use one of a handful of assertion patterns:

```python
# Equality
assert add(2, 3) == 5
assert sorted([3, 1, 2]) == [1, 2, 3]

# Inequality
assert price > 0
assert len(items) <= 100

# Identity
assert result is None
assert player.weapon is not None

# Membership
assert "alice" in users
assert "banana" not in fruits

# Type
assert isinstance(value, int)

# Truthy / falsy
assert is_logged_in           # any truthy value passes
assert not is_logged_out      # falsy passes

# Approximate equality (for vials — Lesson 4 precision trap!)
from pytest import approx
assert 0.1 + 0.2 == approx(0.3)
```

That last one is worth pausing on. The Lesson 4 precision trap (`0.1 + 0.2 != 0.3` exactly) means you should never compare vials with `==` in tests. `pytest.approx` is the standard tool for "is this *approximately* what I expected?".

---

## Asserting that an Alarm Fires — `pytest.raises`

Sometimes the *correct* behaviour of a workstation is to trigger an alarm — Lesson 22's `raise` pattern. To test that an expected alarm fires, use the `pytest.raises` context manager:

```python
import pytest


def withdraw(balance, amount):
    if amount > balance:
        raise ValueError("Insufficient funds")
    return balance - amount


def test_withdraw_succeeds():
    assert withdraw(100, 30) == 70


def test_withdraw_refuses_overdraft():
    with pytest.raises(ValueError):
        withdraw(100, 200)
```

`with pytest.raises(ValueError):` declares: *"the indented code is expected to trigger a `ValueError`; the test passes if it does, fails if it does not"*. The lockable-room pattern from Lesson 21 reused — the inspection happens automatically on exit.

To inspect the alarm's message as well:

```python
def test_withdraw_message():
    with pytest.raises(ValueError, match="Insufficient funds"):
        withdraw(100, 200)
```

`match=` accepts a substring (technically a regular expression) that the alarm's message must contain. If it does not, the test fails.

---

## Fixtures — Setting Up and Tearing Down

Tests often need *setup* — building a workshop, populating a row, opening a database — before the actual check. The fixture pattern lets you write that setup once and reuse it across many tests:

```python
import pytest


class Player:
    def __init__(self, name):
        self.name = name
        self.health = 100


@pytest.fixture
def alice():
    return Player("Alice")


def test_starts_full_health(alice):
    assert alice.health == 100


def test_can_take_damage(alice):
    alice.health -= 30
    assert alice.health == 70
```

A fixture is a workstation decorated with `@pytest.fixture` (decorators from Lesson 19). Any test that needs the prepared item lists the fixture's name as a parameter. `pytest` notices, runs the fixture, and hands the result in.

Each test gets its own fresh fixture call by default — `test_can_take_damage` does not see the changes that another test would have made to its `alice`. Tests stay independent.

For setup that should be shared across many tests in the same session (slow database setup, for instance), fixtures can be given a wider scope:

```python
@pytest.fixture(scope="module")
def database():
    conn = sqlite3.connect(":memory:")
    yield conn
    conn.close()
```

Notice `yield`. A fixture that needs *both* setup and teardown uses `yield` to hand the item over and pause; when the test (or the test module) is done, the fixture resumes from after the `yield` and runs the teardown code. This is the generator pattern from Lesson 18, applied as a fixture lifecycle.

---

## Parametrise — Many Inputs, One Test

Testing the same workstation with several inputs and expected outputs is so common that `pytest` provides a dedicated decorator for it:

```python
import pytest


def double(n):
    return n * 2


@pytest.mark.parametrize("input,expected", [
    (0, 0),
    (1, 2),
    (3, 6),
    (-1, -2),
    (1000, 2000),
])
def test_double(input, expected):
    assert double(input) == expected
```

`@pytest.mark.parametrize` runs the test once per row in the table — five runs in this example. If one fails, the others still run and `pytest` reports exactly which input was the problem. This is more compact and clearer than writing five separate test workstations.

---

## `unittest` — The Older Sibling

Python's standard library has its own testing toolkit, `unittest`, dating back to the JUnit era. Tests are written as workshop blueprints inheriting from `unittest.TestCase`:

```python
import unittest


class TestDouble(unittest.TestCase):
    def test_positive(self):
        self.assertEqual(double(3), 6)

    def test_zero(self):
        self.assertEqual(double(0), 0)


if __name__ == "__main__":
    unittest.main()
```

This shape works, but it is verbose compared to `pytest`. Most modern Python codebases use `pytest`; you will encounter `unittest` in older libraries and in legacy code, and `pytest` happily runs `unittest`-style tests if it finds them. As a beginner, learn `pytest` first; recognise `unittest` when you see it.

---

## Why Test?

A small philosophical interlude.

A Python program runs once and produces an answer. *Working in one run* and *working in every run* are different things — and you cannot tell them apart by looking at the code. Tests are how you check the second.

Three practical benefits of writing tests:

- **Regression protection.** A test you write today catches a future change that breaks the workstation it was testing. Without tests, regressions ship silently and get discovered weeks later by users.
- **Faster iteration.** Once tests exist, you can refactor freely. Each refactor is followed by `pytest`; if it passes, the change preserved the workstation's behaviour. Without tests, every change is a small leap of faith.
- **Documentation by example.** A test of `discount_price(100, 25) == 75` is a more reliable description of what the workstation does than a docstring claiming it. The test is checked every time; the docstring is checked when someone reads it.

What makes a good test:

- **Isolated.** One test, one workstation under examination, no dependence on the side effects of other tests.
- **Fast.** Slow tests get skipped. A test that takes ten minutes will run rarely; a test that takes ten milliseconds will run every commit.
- **Clear.** When a test fails, a reader should be able to tell *what* failed and *why* from the test name and the assertion alone. Long, complex tests fail in confusing ways.

A test suite of a hundred fast, isolated, clear tests is a far more valuable asset than a hundred lines of docstrings explaining what the code *should* do.

---

## What You Now Know

You have entered the Testing Laboratory — the building beside the Floor where workstations are verified before going into production. You know the Lab is not Quality Control; the canonical comparison table makes the distinction concrete.

You can write quick `assert` checks for development, and proper `pytest` test files for real test suites. You know the naming conventions (`test_*.py` files, `test_*` functions), the common assertion shapes, the `pytest.raises` form for expected alarms, the `pytest.approx` form for vial comparisons, fixtures for reusable setup (with `yield` for teardown), and `@pytest.mark.parametrize` for table-driven tests. You have seen `unittest` and know it is the older sibling — recognisable but not preferred for new code.

You know *why* tests matter — regression protection, fast iteration, documentation by example — and what makes a good test (isolated, fast, clear).

The next advanced lesson visits the **CCTV Room**, which watches the whole factory in real time and writes down what it sees — Python's `logging` module.

---

## Quick Reference

| Python | Lab image |
|---|---|
| `assert expr` | Station check — passes if `expr` is truthy; otherwise `AssertionError`. |
| `pip install pytest` | Order the test toolkit from PyPI. |
| `test_*.py`, `def test_*():` | The pytest naming conventions — discovery is automatic. |
| `pytest` (terminal) | Run all tests in the current folder and below. |
| `pytest test_file.py::test_name` | Run a single specific test. |
| `pytest -v` | Verbose — print every test name. |
| `pytest.raises(ValueError)` | Context manager: passes if the block raises `ValueError`. |
| `pytest.raises(ValueError, match="...")` | …also requires the message to contain the substring. |
| `pytest.approx(value)` | Comparison for vials — handles the precision trap. |
| `@pytest.fixture` | A reusable setup workstation; `yield` for teardown. |
| `@pytest.fixture(scope="module")` | Wider scope — shared across all tests in a module. |
| `@pytest.mark.parametrize("a,b,exp", [...])` | Run the same test once per row in the table. |
| `unittest.TestCase` | The older standard-library style — recognise but prefer pytest. |

---

## Try It

These exercises assume `pytest` is installed (`pip install pytest`).

**A first test file:**

```python
# test_first.py

def add(a, b):
    return a + b


def test_add_positive():
    assert add(2, 3) == 5


def test_add_negative():
    assert add(-1, -2) == -3
```

Run it:

```
$ pytest test_first.py
```

You should see two passed tests.

**Watch a failure happen — change one of the expected values:**

```python
def test_add_negative():
    assert add(-1, -2) == 0          # wrong on purpose
```

Re-run and read the failure output carefully — notice how `pytest` shows the actual value alongside the expected.

**Test that an alarm fires:**

```python
import pytest

def withdraw(balance, amount):
    if amount > balance:
        raise ValueError("Insufficient funds")
    return balance - amount


def test_withdraw_refuses_overdraft():
    with pytest.raises(ValueError, match="Insufficient funds"):
        withdraw(100, 200)
```

**Vials — see the precision issue, then fix it:**

```python
from pytest import approx

def test_vial_naive():
    assert 0.1 + 0.2 == 0.3          # fails — Lesson 4's precision trap

def test_vial_approx():
    assert 0.1 + 0.2 == approx(0.3)  # passes
```

**A fixture:**

```python
import pytest


class Player:
    def __init__(self, name):
        self.name = name
        self.health = 100


@pytest.fixture
def alice():
    return Player("Alice")


def test_starts_full_health(alice):
    assert alice.health == 100


def test_can_take_damage(alice):
    alice.health -= 30
    assert alice.health == 70


def test_fixtures_are_independent(alice):
    # alice here is a fresh Player, not the one from the previous test
    assert alice.health == 100
```

Notice the third test passes — each test gets its own fresh `alice`.

**Parametrise:**

```python
import pytest

def double(n):
    return n * 2

@pytest.mark.parametrize("input,expected", [
    (0, 0),
    (1, 2),
    (3, 6),
    (-1, -2),
    (1000, 2000),
])
def test_double(input, expected):
    assert double(input) == expected
```

Run `pytest -v` to see all five runs listed with their parameter values.

---

## Where Next?

The other half of Phase 5 is the **CCTV Room** — Python's logging module — which is how a running program records what it is doing in real time. After that, Phase 6 visits the Night Shift Wing for threading and async.

| Next lesson | Zone | Topic |
|---|---|---|
| Advanced 5b | Z9 — CCTV Room | Logging — permanent records from every zone |
| Advanced 6a | Z10 — Night Shift Wing | Threading — two lines running in parallel |
| Advanced 6b | Z10 — Night Shift Wing | Async — one worker, no idle time |

*See the full lesson map in **The Factory — A Complete Map**.*
