# Homework 2 — Many tables and an API: the board-deck numbers

**ECBS5294 — Working with Data · due the Friday after Session 2 (16 October 2026), 23:59, on Moodle**

Budget about **6–7 hours**. If you are well past that and still stuck, post on the Moodle forum. That tells us
something useful about the assignment, and it is not a mark against you.

## Start here

| | |
|---|---|
| **The question** | What are the six numbers the board asked for, and can each one be trusted? |
| **The files** | Olist's orders, items, payments, reviews, customers, sellers, products and category names in `data/raw/`, and the ECB rates in `data/raw/api/frankfurter_eur.json`. What each KPI means: **`BRIEF.md`**. |
| **What is wrong** | Four KPIs are drafted and run without an error; their totals disagree with the headline numbers. Two KPIs are not started. |
| **What you hand in** | `hw2-submission.zip` and a 60–90 second video, on Moodle. `SUBMITTING.md` says how. |
| **First thing to do** | Read `BRIEF.md`. Then run the notebook and put each KPI's total beside the headline. |

## Get the project

In your terminal (Git Bash on Windows, Terminal on macOS), in the folder where you keep course work:

```bash
git clone https://github.com/earino/ecbs5294-hw02-joins-json.git
cd ecbs5294-hw02-joins-json
uv sync
```

Open **this folder** in VS Code (*File → Open Folder…*; trust the authors if asked), open `notebooks/report.ipynb`,
pick the `.venv` kernel, then *Restart* and *Run All*. The first code cell prints the folder it is working in: it must
be this project's folder.

## What is broken

The notebook's headline cell is right. It says there are **2,653 orders**, **2,652 paid orders**, **435,300.95 BRL**
of revenue, **3,024 items** and **364,110.54 BRL** of item sales. Your colleague's four KPIs disagree with it:

- **KPI 1** (monthly revenue): its months add up to **327,051.28 BRL** over **2,027 orders**.
- **KPI 2** (average order value by state): **120.41 BRL**, over **3,024 orders**.
- **KPI 3** (average review score by state): it counts **2,681 reviews**. `order_reviews.csv` has **2,659**
  different `review_id` values.
- **KPI 4** (item sales by category): its categories add up to **2,927 items** and **351,299.08 BRL**.

Nothing errors. Find out why for each one, before you change it.

*(If the instructor announced the three single-table questions in Lab 4, KPI 1's repair is where you write Lab 4's
monthly euro query for the first time. The brief's conversion rule is the same.)*

## The order of work

The list under *What you must submit* is what you hand in. This is the order to do it in.

**Open `DIAGNOSIS.md` before step 2.** Each piece of evidence is written down **once**, and cited everywhere else by
its ID. The notebook's cells already have IDs: `A1`–`A4` (the evidence), `B1`–`B4`, `C1`, `C2` (your queries), `D`
(the reconciliation table, one named row per KPI) and `E` (the assertions). Joins get IDs in `DIAGNOSIS.md`: `J1`, `J2`, ….

1. Read `BRIEF.md`. Run the notebook top to bottom. Write the four disagreements above into the four notes' part 1.
2. **Section A — the evidence, before any repair.** For each drafted KPI: the three counts for every join it makes,
   and the key test (`COUNT(*)` against `COUNT(DISTINCT …)`) on any table whose grain you are not sure of. Write each
   join's three counts once, in the **Joins** section of `DIAGNOSIS.md`, with an ID. Part 3 of that KPI's note cites
   the join's ID and the cell, and quotes the one line that shows the cause. Leave section A's cells as they are after
   you repair: they measure your colleague's drafts, so they stay the evidence.
3. **Section B — the repairs, one KPI at a time.** Each repair is a query that follows the brief, written into a new
   cell; your colleague's cell stays as it was. Add the three counts of every join you write to the Joins section.
   After each repair, compare its total with the headline, and **commit**: a commit that changes a join carries the
   join's ID and its three counts in one line of its message.
4. **Section C — the two new KPIs**, from the brief. Their joins go in the Joins section too.
5. **Section D — the reconciliation table**: one row per KPI, naming the identity that KPI allows, with the KPI's
   number beside a number computed independently. **Section E — the assertions**: one cell that stops, with the number
   in its message, if any identity in D fails, if any order has no rate, or if any order is counted twice. Then make
   **one** assertion fail on purpose (point it at one of your colleague's `draft_` tables, say), paste its message
   into that KPI's note, part 5, and put it back. One demonstration is the whole requirement: the other notes name
   the assertion that guards them, without a message.
6. *Restart* and *Run All*. The notebook must run top to bottom with every assertion passing.
7. Finish `DIAGNOSIS.md` (the rest of each note, the joins list, the trap log), `AI_USE.md`, and the **Results**
   section of this README (below). Commit.
8. Follow `SUBMITTING.md`: commit everything, save the Git log, make the zip. Record the video. Upload both.

## What you must submit

In the repo, committed:

1. **`notebooks/report.ipynb`**, sections A to E filled in, running clean after *Restart* and *Run All*. The six KPI
   tables have the names and columns the notebook asks for.
2. **The category repair as two queries**: the anti-join that lists every category with no English name and its item
   count, and the labelled `LEFT JOIN` that keeps every item (section B4). Changing one join keyword is not this repair.
3. **The reconciliation table** (section D), **one identity per KPI**: a sum where the KPI is a total; a ratio's
   numerator and denominator, each reconciled on the same orders; a weighted mean where the KPI is an average.
   Averages do not add up, and the average of averages is not the average: a row that sums an average is wrong.
4. **The assertions cell** (section E), encoding exactly those identities, plus *no order without a rate* and *no
   order counted twice*. **Money is compared to the cent, never with `==`:** write `abs(a - b) < 0.005`, or round
   both to two decimals first. Two correct ways of adding up the same money can differ in the last digit of a float,
   so `==` can fail on right numbers. Whole-number counts may use `==`; averages agree to six decimals.
5. **`DIAGNOSIS.md`**: one five-part note per failure you repaired (the template has four); a **Joins** section, the one
   place the three counts are written: every join in your submission, one line each, with an ID (`J1`, `J2`, …); and a
   **Trap log**: one line per data issue that changed a number, even where your own query was never wrong (for
   example: one order has no payment row, so there are 2,652 paid orders, not 2,653). The notes cite joins and cells
   by ID instead of pasting them again (see *Diagnosis note*, below).
6. **`README.md`**: replace the *Start here* and *What is broken* sections with a **Results** section for the board:
   the six KPIs' headline figures, and three to five sentences a board member can read — what the numbers show, what
   was excluded and why, and **one thing this data cannot answer**, said as a fact about the data ("Olist has no cost
   data, so these figures say nothing about margin"), not as a hedge. Give each exclusion once, in words and one
   number; do not copy join counts here. If a sentence rests on a join, cite its ID from `DIAGNOSIS.md`. Leave
   `SUBMITTING.md` alone.
7. **`AI_USE.md`**.
8. **Commits** whose messages name the cause, with the join's ID and three counts in any commit that changes a join;
   **`GIT_LOG.txt`**; and the archive, **`hw2-submission.zip`**, made as `SUBMITTING.md` says.

Upload to the Homework 2 slot on **Moodle**: `hw2-submission.zip` and the video.

## The video

**60–90 seconds.** Start by saying "Homework 2" and the repo name. Show the notebook after *Restart* and *Run All*,
with the assertions cell passing. Then take **one** failure of your choice and explain it in full: the symptom, the
cause (with the evidence that showed it — why the number was wrong, not only where), what you changed, and how you
verified it. It is scored on those four elements, for that one failure; the other three are in your notes, not the
video. Your voice is required.

## Rules

- **Never edit `data/raw/`.** Fix the queries.
- The brief is the definition. A number that matches the headline by a route the brief does not describe is not the
  fix.
- Every fix is a query: a rewritten query with its evidence and its check, not a changed keyword.
- AI may explain and suggest; you must be able to explain every line, on video, without notes. Say what you used it
  for in `AI_USE.md`.

## Hints, if stuck

1. Put each KPI's total beside the headline number it should add up to. Then, for every join in a KPI that
   disagrees, the three counts: did rows disappear, or appear?
2. For each table a KPI joins, say what one row is, then test it: `COUNT(*)` against `COUNT(DISTINCT …)` on the key
   you believe in. For the rates, count the paid orders that found no rate, by weekday.
3. Rows disappear when the other side has no match: the ECB publishes no rate on weekends and its holidays, and not
   every category has an English name, or any name. Rows appear more than once when the table you joined is finer
   than the thing you are counting: an item is not an order, and a review can cover two orders. The brief says what
   each average is over.

## Diagnosis note

`DIAGNOSIS.md` has four notes ready, one per drafted KPI, then the Joins section and the trap log. **Cite, do not paste
again**: where the evidence is in the notebook or the Joins section, write its ID and quote the one line that matters.

- **Part 3, evidence:** the join IDs and the section A cell (`J3`, `A1`), with the line that shows the cause.
- **Part 4, change:** the section B cell (`B1`), and one sentence on what the query now does.
- **Part 5, verification:** the row of `D` by its name and the assertion in `E` that guards it. **In one note only**,
  the one whose assertion you made fail on purpose, also paste **the message it gave**: once you put the assertion
  back, the notebook no longer shows it. The other three notes need no message.

## Stretch (optional, not graded)

The brief counts one review, one vote. "One order, one vote" is another defensible rule. The notebook's last cell asks
which states move by more than 0.05 under it, and whether the board's reading would change.

## Git thread

This session's habit: **the three counts go in the commit message of any commit that changes a join**, for example
"KPI 1 at the month's average rate, LEFT JOIN by month (J2): 2,652 orders in, 2,652 rows out, 2,652 distinct".
The line is written once, when you commit; it is the history's record of what the join did. Commit after each repair,
not once at the end.

## Grading

See the rubric on the course site: `assessments/hw2_rubric.md`.

## If you got lost: how to reset

Both of these **destroy work**. Read before running.

**Discard uncommitted changes (destructive)** — throw away edits and new files; keep your commits:

```bash
git restore --staged --worktree .    # every tracked file back to the last commit, staged or not
git clean -fd                        # and remove new, untracked files
```

> ⚠️ Permanently deletes uncommitted changes, staged or not, and any new untracked files.

**Full reset to the starter state (destructive)** — back to exactly what you cloned; throws away your commits too:

```bash
git reset --hard origin/main
git clean -fdx
```

> ⚠️ Discards your local commits and uncommitted changes. The `-x` also removes ignored files — the `.venv/`
> environment — so the folder matches a fresh clone. `uv sync` rebuilds the environment in a minute.
