# Python for Hyperphantasic Minds
## Lesson 23 — Databases

> **Series**: Python for Hyperphantasic Minds  
> **Source roadmap**: W3Schools Python Syllabus  
> **Lesson**: 23 of 25  
> **Topic**: Databases — the permanent archive  
> **Factory zone**: Z8 — The Records Department  
> **Vocabulary standard**: VOCABULARY.md v0.2  
> **Level**: Complete beginner  
> **Delivery**: Visual narrative — image-first, notation-second

---

## Where You Are

You return to **the Records Department** — visited briefly in Lesson 21 for file writes, today for its true purpose. The Records Department is the factory's *permanent archive*. The warehouse is cleared at the end of every shift; the records survive. Anything the program needs to outlive its own run must be sent here for storage.

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
┌──────────┐         ┌─────────┐  ┌─────────────────┐
│ QUALITY  │         │ TESTING │  │ ██ RECORDS ██   │
│ CONTROL  │         │   LAB   │  │     DEPT        │
└────┬─────┘         └─────────┘  │ YOU ARE HERE    │
     │                            └─────────────────┘
     ▼
┌──────────────┐
│  OUTGOINGS   │
└──────────────┘
```

The everyday tool for permanent storage in Python is the **SQLite database** — a single file on disk that holds many structured records, accessed through a simple Python toolkit (`sqlite3`) that comes with Python's standard kit. It is the right starting point: no server to install, no separate password, no extra ordering at the Tool Store. A SQLite database is a file you can copy, version, and email like any other.

---

## The Records Department in Full

Picture the building behind the main factory: rows of filing-cabinet-like archives stretching off into the distance, each labelled with its name. Each archive holds **tables** — large ruled sheets with columns and rows. Each row of the sheet is one *record*. Each column has a name (like `score`, `name`, `created_at`) and a type (numbers, scrolls, dates).

The archive sits on disk, not in the warehouse. When a workstation needs to read or write records, it sends a clerk — formally, a **cursor** — into the archive, who navigates the tables, fetches the requested records, and brings them back. Writes are accepted only after a special command: `commit`. Until then, the changes are pending. This is what makes a record archive *durable* — the structure forces deliberate persistence rather than accidental.

---

## SQLite — A Self-Contained Archive

`sqlite3` is one of Python's standard-library toolkits. No ordering needed. Open a database with one fetch:

```python
import sqlite3

conn = sqlite3.connect("players.db")
```

`sqlite3.connect("players.db")` opens — or creates, if it doesn't yet exist — a database file named `players.db` in the current folder. The returned `conn` is the **connection** to the archive: a doorway into the building.

Through the connection, you create a **cursor** — the clerk who actually runs commands inside the archive:

```python
cursor = conn.cursor()
```

The cursor accepts **SQL** — Structured Query Language — the universal small language that record archives speak. Every database interaction is one SQL command, sent through the cursor with `cursor.execute(...)`.

SQL is itself a small sub-language — not Python. This lesson teaches the absolute minimum needed to use it from Python. For real SQL depth, learn it separately; the W3Schools SQL pages are a good starting point. The Python machinery is largely the same regardless of how complex your SQL becomes.

---

## Designing a Filing System — `CREATE TABLE`

Before you can file records, you have to design the sheet they will be filed on. The SQL command is `CREATE TABLE`:

```python
cursor.execute("""
    CREATE TABLE IF NOT EXISTS players (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        score INTEGER DEFAULT 0
    )
""")
```

This defines a sheet called `players` with three columns:

- `id` — an integer, the primary key. SQLite fills this in automatically with a unique number for each new record. (Every table should have a primary key — it is the record's permanent label.)
- `name` — text, required (`NOT NULL`).
- `score` — an integer, defaulting to 0 if not provided.

`IF NOT EXISTS` is a safety clause that does nothing if the table is already there. Without it, running the program twice would fail the second time, since the table already exists.

Column types in SQLite are slightly relaxed compared to other databases — `INTEGER`, `TEXT`, `REAL` (for vials), `BLOB` (for binary), `NULL`. Other databases (MySQL, PostgreSQL) have stricter and more varied type systems.

---

## Filing a Record — `INSERT`

To add a new row to the sheet, use `INSERT`:

```python
cursor.execute(
    "INSERT INTO players (name, score) VALUES (?, ?)",
    ("Alice", 100),
)
```

Two things:

- The SQL itself uses `?` as a placeholder where the actual values go.
- The actual values are passed as a sealed crate (or numbered row) as the *second* argument to `execute`.

This is **parameter binding**. It is the *only* correct way to insert user data into SQL. The placeholders and the values are kept separate; the database engine handles the substitution safely. Never build SQL with f-strings or string concatenation — see the security note further down.

`id` was omitted because it is auto-assigned by SQLite when omitted.

For multiple records at once, `executemany` accepts a row of sealed crates:

```python
cursor.executemany(
    "INSERT INTO players (name, score) VALUES (?, ?)",
    [("Bob", 75), ("Cara", 200), ("Dave", 50)],
)
```

---

## Reading Records — `SELECT`

Records are read with `SELECT`:

```python
cursor.execute("SELECT id, name, score FROM players")

for row in cursor:
    print(row)
# (1, 'Alice', 100)
# (2, 'Bob', 75)
# (3, 'Cara', 200)
# (4, 'Dave', 50)
```

Each row comes back as a sealed crate (a tuple) of values in the column order requested. The cursor is iterable — a `for` belt walks the result naturally, one row per pass, with no need to load the whole result into memory at once.

`SELECT *` selects every column, in the order they were defined:

```python
cursor.execute("SELECT * FROM players")
```

Filter with `WHERE`:

```python
cursor.execute("SELECT name, score FROM players WHERE score > ?", (50,))
for row in cursor:
    print(row)
# ('Alice', 100)
# ('Bob', 75)
# ('Cara', 200)
```

Note the trailing comma in `(50,)` — a one-item sealed crate (Lesson 11). Without the comma, `(50)` would be a plain stone, and `execute` would not know how to bind it.

Order with `ORDER BY`:

```python
cursor.execute("SELECT name, score FROM players ORDER BY score DESC")
for row in cursor:
    print(row)
# ('Cara', 200)
# ('Alice', 100)
# ('Bob', 75)
# ('Dave', 50)
```

A few other useful clauses:

- `LIMIT n` — return only the first `n` rows.
- `LIMIT n OFFSET k` — skip `k` rows, return the next `n`.
- `COUNT(*)`, `SUM(column)`, `AVG(column)` — aggregate workstations.

For one-shot reads when you know there is a single row, `cursor.fetchone()` returns it directly:

```python
cursor.execute("SELECT COUNT(*) FROM players")
(total,) = cursor.fetchone()
print(total)             # 4
```

`fetchall()` returns the whole result as a row of crates — useful for small results, but uses the same memory as iterating once and is rarely needed.

---

## Updating and Removing — `UPDATE` and `DELETE`

Records can be modified or removed in place.

**`UPDATE` — change existing rows.**

```python
cursor.execute(
    "UPDATE players SET score = ? WHERE name = ?",
    (150, "Alice"),
)
```

The `WHERE` clause is critical. *An `UPDATE` without a `WHERE` modifies every row in the table.* Beginners forget this and accidentally overwrite their whole dataset. Always include `WHERE` when you mean to update specific records.

**`DELETE` — remove rows.**

```python
cursor.execute("DELETE FROM players WHERE name = ?", ("Dave",))
```

Same warning: `DELETE` without `WHERE` empties the entire table. SQLite — and every other database — does what you ask without asking back.

---

## Transactions and `commit()`

Every `INSERT`, `UPDATE`, `DELETE` you have run above is *pending*. The changes exist in the connection's transaction but are not yet written to disk. If the program crashes or the connection closes without committing, the changes are lost.

To make changes durable:

```python
conn.commit()
```

`commit` writes the pending changes to disk and starts a fresh transaction. To discard pending changes instead:

```python
conn.rollback()
```

Transactions exist for *consistency*. Suppose you transfer money between two accounts: deduct from A, add to B. These are two separate `UPDATE`s, but they belong together as a single change. A transaction lets you do both, then `commit` only if both succeeded. If anything failed in the middle, `rollback` undoes both and the accounts return to their pre-transaction state.

The pattern in code:

```python
try:
    cursor.execute("UPDATE accounts SET balance = balance - ? WHERE id = ?", (100, 1))
    cursor.execute("UPDATE accounts SET balance = balance + ? WHERE id = ?", (100, 2))
    conn.commit()
except Exception:
    conn.rollback()
    raise                            # let the alarm continue propagating
```

This pattern — *do work, then commit; on alarm, rollback and re-raise* — is the canonical shape for any multi-step change in a database.

---

## Closing the Archive — and the `with` Form

A connection holds operating-system resources, just like a file. Close it when done:

```python
conn.close()
```

Python's `sqlite3` connection also doubles as a **context manager** — meaning the connection itself can be used with the `with` statement (Lesson 21). When the `with` block exits cleanly, the connection commits automatically; if an alarm fires inside, it rolls back. The connection itself is *not* closed by `with` (a small inconvenience), so you may still want an outer `try/finally` for that. The idiomatic minimum is:

```python
import sqlite3

conn = sqlite3.connect("players.db")
try:
    with conn:                       # commits on clean exit, rolls back on alarm
        cursor = conn.cursor()
        cursor.execute("INSERT INTO players (name, score) VALUES (?, ?)", ("Eve", 175))
finally:
    conn.close()
```

This is verbose for a beginner pattern. In real applications most people wrap the connection lifecycle in a workstation, or use a higher-level toolkit (like SQLAlchemy) that handles cleanup automatically. For a beginner program, the `try/finally` form is the most defensive option.

---

## The Parameter-Binding Rule — and Why It Matters

This rule deserves its own section. It is the single most important security habit in any program that talks to a database.

**Never build SQL by gluing strings together.**

```python
# 🛑 Wrong, dangerous
user_name = input("Enter your name: ")
cursor.execute(f"SELECT * FROM players WHERE name = '{user_name}'")
```

```python
# ✅ Right
user_name = input("Enter your name: ")
cursor.execute("SELECT * FROM players WHERE name = ?", (user_name,))
```

Why the first is dangerous: if the user types `'; DROP TABLE players; --`, the f-string builds an SQL command that *contains a second command*. The cursor runs both. Your entire `players` table is deleted. This is called **SQL injection**, and it is one of the most exploited security vulnerabilities in the history of the web.

The `?` placeholder form prevents this entirely. The SQL command is parsed once with placeholders; the values are then bound separately. No matter what characters the user types, the values cannot become commands.

The rule applies to *every* value you put into SQL — `WHERE` clauses, `INSERT` values, `UPDATE` settings. Use `?`. Always.

The one exception is *column and table names* in the SQL itself — these cannot be parameterised and must come from a fixed, trusted source (a small whitelist of allowed names in your code). User input must never be used to choose a column or table name dynamically.

---

## Other Archives — Briefly

`sqlite3` is the standard-kit beginner choice. Three other archive systems you will encounter in real Python work:

**MySQL** (and the closely-related **MariaDB**). A full-scale server-based database used by most web applications. Python toolkit: `mysql-connector-python` or `PyMySQL`, ordered from PyPI. The Python pattern is almost identical to `sqlite3` — `connect`, `cursor`, `execute`, `commit`. The differences are the connection arguments (host, user, password) and slight SQL dialect variations.

**PostgreSQL.** A more feature-rich open-source database used heavily in serious applications. Python toolkit: `psycopg2` (or its successor `psycopg`), again from PyPI. Same Python pattern.

**MongoDB.** A *document store* — fundamentally different from SQL databases. Records are filing cabinets (JSON-like documents), not rows on a sheet. Python toolkit: `pymongo`. The Python pattern is different: instead of SQL strings, you call workstations on a collection (`collection.find({...})`, `collection.insert_one({...})`).

For beginner-to-intermediate Python, SQLite covers an enormous range of needs. Move to a server-based database when you need concurrent access from multiple programs, or when the data size outgrows what a single file can comfortably hold.

---

## A Practical Example — Player Records

A complete worked example: a small workstation that maintains player records.

```python
import sqlite3
from pathlib import Path


DB_PATH = Path("players.db")


def init_db():
    conn = sqlite3.connect(DB_PATH)
    try:
        with conn:
            conn.execute("""
                CREATE TABLE IF NOT EXISTS players (
                    id INTEGER PRIMARY KEY,
                    name TEXT NOT NULL UNIQUE,
                    score INTEGER DEFAULT 0
                )
            """)
    finally:
        conn.close()


def add_player(name, score=0):
    conn = sqlite3.connect(DB_PATH)
    try:
        with conn:
            conn.execute(
                "INSERT INTO players (name, score) VALUES (?, ?)",
                (name, score),
            )
    finally:
        conn.close()


def update_score(name, new_score):
    conn = sqlite3.connect(DB_PATH)
    try:
        with conn:
            conn.execute(
                "UPDATE players SET score = ? WHERE name = ?",
                (new_score, name),
            )
    finally:
        conn.close()


def top_players(limit=10):
    conn = sqlite3.connect(DB_PATH)
    try:
        cursor = conn.execute(
            "SELECT name, score FROM players ORDER BY score DESC LIMIT ?",
            (limit,),
        )
        return cursor.fetchall()
    finally:
        conn.close()


# Use the workstations:
init_db()
add_player("Alice", 100)
add_player("Bob", 75)
add_player("Cara", 200)
update_score("Alice", 150)

for name, score in top_players():
    print(f"{name}: {score}")
# Cara: 200
# Alice: 150
# Bob: 75
```

Notice a few patterns:

- Every workstation opens its own connection, does its work, and closes. This is wasteful for very high-frequency operations but is safe and clear for beginner code.
- The `UNIQUE` constraint on `name` means inserting a duplicate `name` will trigger an alarm (`sqlite3.IntegrityError`). Quality Control catches it — or, more commonly, the workstation uses `INSERT OR REPLACE` to make the operation idempotent.
- `conn.execute(...)` is a convenience that returns a cursor in one step — no separate `cursor()` call needed.
- `top_players` does not commit because it only reads. Reads do not need commits.

---

## What You Now Know

You can open a SQLite database with `sqlite3.connect`, get a cursor, run SQL through `execute` with `?` placeholders for any values, commit changes with `conn.commit()`, and close the connection when done. You know `CREATE TABLE`, `INSERT`, `SELECT` (with `WHERE`, `ORDER BY`, `LIMIT`), `UPDATE`, and `DELETE` in their minimal forms — enough for any small application.

You know the parameter-binding rule and why SQL injection is the most important security habit to develop early. You know the transaction shape — do work, commit on success, rollback on alarm — and that `with conn:` provides this automatically.

You have seen the same Python pattern apply across MySQL, PostgreSQL, and (with documents instead of SQL) MongoDB. SQL itself is a separate sub-language to learn properly when your applications need it.

The next lesson moves to the front of the building — **user input in depth** at Goods In, including command-line arguments. After that, the closing lesson in the Shift Manager's Office puts the whole factory together.

---

## Quick Reference

**Python machinery**

| Python | Records image |
|---|---|
| `sqlite3.connect("file.db")` | Open (or create) the archive. |
| `conn.cursor()` | Get a clerk to run commands inside the archive. |
| `cursor.execute(sql, params)` | Run one SQL command with parameter binding. |
| `cursor.executemany(sql, rows)` | Run the same SQL for many rows of parameters. |
| `cursor.fetchone()` | Get one result row (sealed crate). |
| `cursor.fetchall()` | Get all result rows as a numbered row of crates. |
| `for row in cursor:` | Walk results one at a time — constant memory. |
| `conn.commit()` | Make pending changes durable. |
| `conn.rollback()` | Discard pending changes. |
| `with conn:` | Commit on clean exit, rollback on alarm. Does *not* close. |
| `conn.close()` | Release the connection. |

**SQL essentials**

| SQL | Meaning |
|---|---|
| `CREATE TABLE IF NOT EXISTS t (col TYPE, ...)` | Design a sheet. |
| `INSERT INTO t (col, col) VALUES (?, ?)` | Add a row. |
| `SELECT col, col FROM t WHERE ... ORDER BY ... LIMIT n` | Read rows. |
| `UPDATE t SET col = ? WHERE ...` | Modify rows. **Always include `WHERE`.** |
| `DELETE FROM t WHERE ...` | Remove rows. **Always include `WHERE`.** |
| `?` in SQL | The only safe way to inject a value. Never use f-strings. |

---

## Try It

These exercises build up a small player archive.

**Create a fresh database and table:**

```python
import sqlite3

conn = sqlite3.connect("players.db")
conn.execute("""
    CREATE TABLE IF NOT EXISTS players (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL UNIQUE,
        score INTEGER DEFAULT 0
    )
""")
conn.commit()
conn.close()
```

**Add some players:**

```python
import sqlite3

conn = sqlite3.connect("players.db")
with conn:
    conn.execute("INSERT INTO players (name, score) VALUES (?, ?)", ("Alice", 100))
    conn.execute("INSERT INTO players (name, score) VALUES (?, ?)", ("Bob", 75))
    conn.execute("INSERT INTO players (name, score) VALUES (?, ?)", ("Cara", 200))
conn.close()
```

**Read them back:**

```python
import sqlite3

conn = sqlite3.connect("players.db")
for row in conn.execute("SELECT name, score FROM players ORDER BY score DESC"):
    print(row)
conn.close()
```

**Update and remove:**

```python
import sqlite3

conn = sqlite3.connect("players.db")
with conn:
    conn.execute("UPDATE players SET score = ? WHERE name = ?", (250, "Alice"))
    conn.execute("DELETE FROM players WHERE name = ?", ("Bob",))
conn.close()
```

Read the table again to see the changes.

**Aggregate:**

```python
import sqlite3

conn = sqlite3.connect("players.db")
(total,) = conn.execute("SELECT COUNT(*) FROM players").fetchone()
(avg,) = conn.execute("SELECT AVG(score) FROM players").fetchone()
print(f"{total} players, average score {avg:.1f}")
conn.close()
```

**Try the wrong way deliberately — then fix it:**

```python
import sqlite3

conn = sqlite3.connect("players.db")
name = "Cara' OR '1'='1"          # imagine this from user input

# ⚠ Vulnerable — never do this:
# query = f"SELECT * FROM players WHERE name = '{name}'"
# print(list(conn.execute(query)))    # would expose all records

# ✅ Safe:
print(list(conn.execute("SELECT * FROM players WHERE name = ?", (name,))))
conn.close()
```

Run the safe version. It returns nothing — because no player is *literally* called `Cara' OR '1'='1`. With the vulnerable f-string form, that string would have become an injected SQL clause that matched everything.

**Build the full worked example:**

Take the four workstations from the *practical example* section above, paste them into a file, and call them. Insert some players, update some scores, fetch the top ones. The pattern scales from this small example to applications with millions of records.

---

## Where Next?

The last full lesson before the closing one moves to **user input in depth** — what arrives at Goods In from the user. Lesson 25 then visits the Shift Manager's Office for the first time and pulls the whole factory together.

| Next lesson | Zone | Topic |
|-------------|------|-------|
| Lesson 24 | Z2 — Goods In | User Input in Depth — work orders and terminals |
| Lesson 25 | Z4 — Shift Manager's Office | Putting It Together — the complete shift |
| Advanced 5a | Z6 — Testing Lab | Testing — station checks with pytest |
| Advanced 5b | Z9 — CCTV Room | Logging — permanent records from every zone |

*See the full lesson map in **The Factory — A Complete Map**.*
