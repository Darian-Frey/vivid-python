# Python for Hyperphantasic Minds
## Lesson 24 — User Input in Depth

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 24 of 25  
> **Topic**: User Input in Depth — work orders and terminals  
> **Factory zone**: Z2 — Goods In  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

Back at **Goods In** — the loading bay at the front of the building. Lesson 21 visited briefly for file reads; today is about the *other* main source of material: input from a human at a terminal, and *the work order* that arrived with the lorry at the start of the shift.

```
                    ┌──────────────────────────────┐
                    │    SHIFT MANAGER'S OFFICE     │
                    └──────────────┬───────────────┘
                                   │
┌──────────┐                       ▼      ┌─────────────────┐
│          │     ┌─────────────────┐      │                 │
│TOOL STORE│────▶│ ██ GOODS IN ██  │      │  FACTORY FLOOR  │
│          │     │ YOU ARE HERE    │      │                 │
└──────────┘     └────────┬────────┘      └────────┬────────┘
                          │                        │
                          ▼                        │
               ┌──────────────────────┐            │
               │      WAREHOUSE       │◀───────────┘
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

Three sources of input arrive at this bay:

- **Interactive input** — a human typing into the terminal while the program is running.
- **The work order** — command-line arguments passed in when the program is started.
- **Environment variables** — facts about the environment the factory is operating in, set by the operating system or by the person running the program.

This lesson covers all three. Each one has its own canonical pattern; together they cover almost every way a Python program receives input outside of files (Lesson 21) and databases (Lesson 23).

---

## Interactive Input — `input()`

You first met `input()` in Lesson 5. Its behaviour is simple: pause the program, wait for the user to type something and press Enter, return whatever they typed as a scroll.

```python
name = input("What is your name? ")
print(f"Hello, {name}.")
```

The prompt scroll is optional; without it, `input()` just waits silently. The returned scroll never includes the trailing newline.

**Always a scroll.** This is the most important fact about `input()`. Whatever the user types, you receive a scroll. If you want a number, you must press it through a mould:

```python
age_text = input("Age: ")
age = int(age_text)         # ValueError if the user typed letters
```

Most programs combine the two lines:

```python
age = int(input("Age: "))
```

This is concise but raises a `ValueError` immediately if the user types anything that is not a whole number. For a robust program, you almost never want to crash on a typing mistake — which leads to the validation loop.

---

## The Validation Loop

Reading user input and being prepared for any kind of typing mistake is a pattern you will write many times. The canonical shape is a `while` belt that keeps asking until the input passes a check:

```python
while True:
    age_text = input("Age (whole number, 0–150): ")
    try:
        age = int(age_text)
    except ValueError:
        print("That isn't a whole number. Try again.")
        continue
    if age < 0 or age > 150:
        print("Out of range. Try again.")
        continue
    break

print(f"Got age {age}.")
```

The `while True` belt would run forever; each `continue` skips back to the prompt; only `break` exits. Picture: a worker at the loading door politely refusing every wrong delivery and asking again, until something acceptable arrives.

This pattern factored into a workstation is even better:

```python
def ask_int(prompt, low, high):
    while True:
        text = input(prompt)
        try:
            value = int(text)
        except ValueError:
            print("That isn't a whole number. Try again.")
            continue
        if value < low or value > high:
            print(f"Out of range ({low}–{high}). Try again.")
            continue
        return value


age = ask_int("Age (0–150): ", 0, 150)
year = ask_int("Year (1900–2100): ", 1900, 2100)
```

Notice the `return` inside the loop replaces the `break` — same physical effect (exit the loop), with the additional act of sending back the validated value.

---

## End-of-Input

If the user reaches the end of the input stream (often by pressing Ctrl-D on Linux/macOS or Ctrl-Z then Enter on Windows), `input()` triggers `EOFError` rather than returning a scroll. For programs read in scripts or pipelines, this matters. The defensive shape:

```python
try:
    name = input("Name: ")
except EOFError:
    print("\nNo input — exiting.")
    sys.exit(0)
```

Most everyday programs ignore this; the alarm only fires when input is being piped in and runs out unexpectedly.

---

## The Work Order — `sys.argv`

The other main way to give a program input is at the moment you start it — on the command line:

```
$ python greet.py Alice --loud
```

Everything after the program name is the **work order** — a row of scrolls that the program reads at the start of its shift. The canonical verb is **read the work order**; the Python standard-kit shelf is `sys.argv`:

```python
# greet.py
import sys

print(sys.argv)
```

Running `python greet.py Alice --loud` would print:

```
['greet.py', 'Alice', '--loud']
```

Three things:

- `sys.argv[0]` is *always* the program name itself. The real arguments start at `sys.argv[1]`.
- Every item is a scroll — even arguments that look numeric (`42` arrives as `"42"`).
- The arguments are tokenised by the *shell*, not by Python. A quoted argument like `"hello world"` arrives as a single item.

Simple use:

```python
import sys

name = sys.argv[1] if len(sys.argv) > 1 else "world"
print(f"Hello, {name}.")
```

For tiny scripts, this is fine. The moment you need more than one argument, optional flags, default values, or `--help` output, you reach for `argparse`.

---

## The Modern Way — `argparse`

Python's standard-kit toolkit `argparse` turns command-line arguments into a structured form with names, defaults, types, and help. The pattern:

```python
# greet.py
import argparse

parser = argparse.ArgumentParser(description="Greet someone by name.")
parser.add_argument("name", help="Who to greet.")
parser.add_argument("--loud", action="store_true", help="Shout the greeting.")
parser.add_argument("--repeat", type=int, default=1, help="How many times.")

args = parser.parse_args()

greeting = f"Hello, {args.name}"
if args.loud:
    greeting = greeting.upper() + "!"

for _ in range(args.repeat):
    print(greeting)
```

Run it:

```
$ python greet.py Alice
Hello, Alice

$ python greet.py Alice --loud
HELLO, ALICE!

$ python greet.py Alice --repeat 3
Hello, Alice
Hello, Alice
Hello, Alice
```

What `argparse` does for you:

- **Reads the work order automatically.** No `sys.argv` slicing needed.
- **Converts types.** `--repeat 3` arrives as a stone marked `3`, not as a scroll `"3"`. The `type=int` parameter handles this.
- **Provides `--help` for free.** Run `python greet.py --help` and see a generated help message with every option.
- **Validates.** If the user types a flag your parser doesn't know, or forgets a required argument, `argparse` prints a useful error and exits cleanly.

The three argument shapes in the example:

- **Positional** — `name`. The user must provide it; no flag needed.
- **Flag (boolean)** — `--loud`. Present or absent; `action="store_true"` makes it a switch.
- **Optional with value** — `--repeat`. Has a default; the user can override it.

`argparse` supports much more — subcommands, mutually-exclusive groups, custom validators, file arguments — but the three forms above cover the great majority of beginner-to-intermediate use.

For everything but the smallest one-off scripts, prefer `argparse` over `sys.argv`. The structure is worth the extra few lines.

---

## Hidden Input — `getpass`

When a user types a password, you do not want it shown on screen as they type. The standard-kit toolkit `getpass` handles this:

```python
from getpass import getpass

password = getpass("Password: ")
# password holds what the user typed; nothing was shown on screen
```

`getpass` behaves exactly like `input()` except that the typed characters do not appear. It is the right tool for any sensitive input — passwords, API keys, tokens — that should not be left on the user's terminal scrollback.

Use `getpass` only for sensitive input. For ordinary prompts, `input()` is the natural choice.

---

## Environment Variables — `os.environ` and `os.getenv()`

The factory operates in an *environment* — the operating system, the shell, the configuration of the host machine. **Environment variables** are facts about that environment, set by the operating system or by the person running the program. They are how configuration is most often passed to a Python program in production: API keys, database URLs, feature flags, log levels.

Python reads them from the `os.environ` cabinet — a filing cabinet on the open aisle, pre-loaded at the start of the shift:

```python
import os

# Read with a default if not set
api_key = os.getenv("MY_API_KEY", "no-key")

# Read with a strict requirement (KeyError if not set)
api_key = os.environ["MY_API_KEY"]
```

`os.getenv(name)` returns `None` if the variable is not set; `os.getenv(name, default)` returns the default. `os.environ[name]` reads a *required* variable and triggers `KeyError` if it is missing — useful when the program genuinely cannot run without it.

Setting environment variables happens outside Python — typically at the shell:

```
$ MY_API_KEY=abc123 python myscript.py
```

The variable exists only for that one invocation. For a permanent setting, you would `export` it in your shell config or set it in your operating system's environment management.

**Why this matters in real programs:**

- Environment variables are the standard way to keep secrets *out of* the source code. A program reads its API key from the environment rather than embedding it in a file that might be committed to version control.
- Different deployments (development, staging, production) often need different values for the same setting. Environment variables let the same code run in each, with different external configuration.

For a beginner program, you may never need environment variables. As soon as your program has any secret value — a password, an API key, a database connection string — environment variables become the right tool.

---

## A Practical Example — A Small CLI

A complete worked example pulling together interactive input, command-line arguments, and environment variables:

```python
# logger.py
import argparse
import os
import sys
from datetime import datetime


def log_to_file(path, level, message):
    timestamp = datetime.now().isoformat(timespec="seconds")
    line = f"[{timestamp}] [{level}] {message}\n"
    with open(path, "a") as crate:
        crate.write(line)


def main():
    parser = argparse.ArgumentParser(description="Log a message.")
    parser.add_argument("message", nargs="?", help="The message to log (or omit for interactive input).")
    parser.add_argument("--level", default=os.getenv("LOG_LEVEL", "INFO"),
                        help="Log level (default: $LOG_LEVEL or INFO).")
    parser.add_argument("--file", default=os.getenv("LOG_FILE", "app.log"),
                        help="Path to the log file (default: $LOG_FILE or app.log).")
    args = parser.parse_args()

    if args.message is None:
        try:
            args.message = input("Message: ")
        except EOFError:
            print("No message — aborting.", file=sys.stderr)
            sys.exit(1)

    log_to_file(args.file, args.level, args.message)
    print(f"Logged to {args.file}.")


if __name__ == "__main__":
    main()
```

Things to notice:

- **`nargs="?"`** on `message` makes it optional even though it is positional — falls back to interactive input if missing.
- **`default=os.getenv(...)`** chains an environment-variable default behind a command-line override. Common production pattern.
- **`sys.stderr`** in the error print sends the message to the error stream rather than standard output. Real CLI tools separate normal output from error messages this way.
- **`sys.exit(1)`** ends the program with a non-zero status, signalling failure to whatever ran it.
- **`if __name__ == "__main__":`** is the script-vs-library boundary first met in Lesson 20. Lesson 25 explains this convention fully.

Run it:

```
$ python logger.py "Server started"
Logged to app.log.

$ python logger.py "Something went wrong" --level ERROR
Logged to app.log.

$ LOG_LEVEL=DEBUG LOG_FILE=debug.log python logger.py "Detailed trace"
Logged to debug.log.

$ python logger.py
Message: User logged out
Logged to app.log.
```

Four invocations, three input methods, one program. This is the shape of a small but realistic CLI tool.

---

## What You Now Know

You can read interactive input with `input()`, knowing it always returns a scroll and that conversion to a stone needs a mould plus a validation strategy. You can write a robust validation loop, and you know the `EOFError` alarm for when the input stream ends.

You can read the work order from `sys.argv` for tiny scripts, and `argparse` for any tool that grows beyond two arguments. You know that `argparse` provides type conversion, defaults, `--help` for free, and clean validation.

You can read environment variables with `os.getenv` (with default) or `os.environ[name]` (required), and you know why they are the standard way to pass configuration and secrets to a real program.

The next lesson — the final lesson of the main series — visits **the Shift Manager's Office** for the first time and pulls every zone together. The `if __name__ == "__main__":` boundary, the `main()` workstation, program structure, and a complete worked example that touches every part of the factory.

---

## Quick Reference

| Python | Goods In image |
|---|---|
| `input(prompt)` | Pause; wait for user to type a line; return as a scroll. |
| `int(input(prompt))` | Combine input and casting — fragile without validation. |
| `while True: ... except ValueError: continue ... break` | The validation loop pattern. |
| `EOFError` | Triggered when the input stream ends unexpectedly. |
| `sys.argv` | The work order — a row of scrolls. `sys.argv[0]` is the program name. |
| `argparse.ArgumentParser` | Structured command-line parsing with help, defaults, types. |
| `add_argument("name")` | Required positional. |
| `add_argument("--flag", action="store_true")` | Boolean flag. |
| `add_argument("--n", type=int, default=1)` | Optional with type and default. |
| `getpass.getpass(prompt)` | Like `input` but does not show what was typed. |
| `os.getenv("NAME", default)` | Read an environment variable safely. |
| `os.environ["NAME"]` | Read a required environment variable — KeyError if missing. |
| `sys.exit(code)` | End the program with the given exit code. |
| `print(..., file=sys.stderr)` | Print to the error stream rather than standard output. |

---

## Try It

**Basic interactive input:**

```python
name = input("Name: ")
print(f"Hello, {name}.")
```

**Casting input — see the failure:**

```python
age = int(input("Age: "))      # type "abc" and feel the ValueError
print(age + 1)
```

**Validation loop:**

```python
def ask_int(prompt, low, high):
    while True:
        text = input(prompt)
        try:
            value = int(text)
        except ValueError:
            print("Not a whole number. Try again.")
            continue
        if value < low or value > high:
            print(f"Out of range ({low}–{high}). Try again.")
            continue
        return value

age = ask_int("Age (0–150): ", 0, 150)
print(f"Got {age}.")
```

**Inspect `sys.argv`** — save this as `args.py` and run it:

```python
# args.py
import sys
print(sys.argv)
```

Then in a terminal:

```
$ python args.py Alice --loud --repeat 3
['args.py', 'Alice', '--loud', '--repeat', '3']
```

Notice that the arguments are tokenised by the shell and arrive as scrolls.

**`argparse` for a small CLI** — save as `greet.py`:

```python
# greet.py
import argparse

parser = argparse.ArgumentParser(description="Greet someone.")
parser.add_argument("name", help="Who to greet.")
parser.add_argument("--loud", action="store_true", help="Shout the greeting.")
parser.add_argument("--repeat", type=int, default=1, help="How many times.")
args = parser.parse_args()

greeting = f"Hello, {args.name}"
if args.loud:
    greeting = greeting.upper() + "!"
for _ in range(args.repeat):
    print(greeting)
```

```
$ python greet.py Alice
$ python greet.py Alice --loud
$ python greet.py Alice --repeat 5
$ python greet.py --help
```

The `--help` output is generated automatically.

**Environment variables:**

```python
import os

api_key = os.getenv("MY_API_KEY", "default-key")
print("Using key:", api_key)
```

Try it without the variable set, then with:

```
$ python myscript.py
$ MY_API_KEY=abc123 python myscript.py
```

**`getpass` for hidden input:**

```python
from getpass import getpass

password = getpass("Password: ")
print("You typed", len(password), "characters.")
```

Notice that nothing appears on screen as you type.

---

## Where Next?

The last lesson of the main series visits **the Shift Manager's Office** — the one zone you have not yet entered. Program structure, the `main()` workstation, the script-vs-library boundary, and a worked example that touches every building you have visited.

| Next lesson | Zone | Topic |
|---|---|---|
| Lesson 25 | Z4 — Shift Manager's Office | Putting It Together — the complete shift |
| Advanced 5a | Z6 — Testing Lab | Testing — station checks with pytest |
| Advanced 5b | Z9 — CCTV Room | Logging — permanent records from every zone |
| Advanced 6a | Z10 — Night Shift Wing | Threading — two lines running in parallel |

*See the full lesson map in **The Factory — A Complete Map**.*
