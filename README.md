# Homework 2 — Many tables and an API: the board-deck numbers

**ECBS5294 — Working with Data · due the Saturday after Session 2 (17 October 2026), 23:59, on Moodle**

Budget about **6–7 hours**. If you are well past that and still stuck, post on the Moodle forum. That tells us
something useful about the assignment, and it is not a mark against you.

## Start here

| | |
|---|---|
| **The question** | What are the six numbers the board asked for, and can each one be trusted? |
| **The files** | Olist's orders, items, payments, reviews, customers, sellers, products and category names in `data/raw/`, and the ECB rates in `data/raw/api/frankfurter_eur.json`. What each KPI means: **`BRIEF.md`**. |
| **What is wrong** | Four KPIs are drafted and run without an error; their totals disagree with the headline numbers. Two KPIs are not started. |
| **What you hand in** | `hw2-submission.zip`, on Moodle. `SUBMITTING.md` says how. |
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

Each piece of evidence is written down **once**, and cited everywhere else by its ID. The notebook's cells have
IDs: `A1`–`A4` (the evidence), `B1`–`B4`, `C1`, `C2` (your queries), `J` (the joins: the three counts, written once,
each with an ID `J1`, `J2`, …), `D` (the reconciliation table, one named row per KPI), `E` (the assertions) and `F`
(the one assertion you made fail).

**Every KPI you repair or build has five parts**, in the markdown cell under its heading (the notebook explains them
once, at the top): the sentence, in plain English — what the number measures, which rows it comes from, which rows
the query left out; the rows you expect, with their source ("the brief fixes it", a section A cell, or "neither fixes
this"); the query and the number; the check, in the kind the heading names (a sum, a ratio, a ranked list, or a join
with its three counts in `J`); and one line, **how would I know if this were wrong?** Write that line **before you
fix the draft**, while the wrong number is still on screen: it is the evidence, and the repair erases the number.

1. Read `BRIEF.md`. Run the notebook top to bottom. Put each drafted KPI's total beside the headline.
2. **Section A — the evidence, before any repair.** For each drafted KPI: the three counts for every join it makes,
   and the key test (`COUNT(*)` against `COUNT(DISTINCT …)`) on any table whose grain you are not sure of. Write each
   join's three counts once, in section `J`, with an ID. Leave section A's cells as they are after you repair: they
   measure your colleague's drafts, so they stay the evidence.
3. **Section B — the repairs, one KPI at a time.** Write the five parts first, the how-would-I-know line from what
   section A showed you; then the repair, a query that follows the brief, in the cell under it; your colleague's cell
   stays as it was. Add the three counts of every join you write to `J`. After each repair, compare its total with
   the headline, and **commit**: a commit that changes a join names the cause and cites the join's ID.
4. **Section C — the two new KPIs**, from the brief, the same five parts. Their joins go in `J` too.
5. **Section D — the reconciliation table**: one row per KPI, naming the identity that KPI allows, with the KPI's
   number beside a number computed independently. **Section E — the assertions**: one cell that stops, with the number
   in its message, if any identity in D fails, if any order has no rate, or if any order is counted twice. Then make
   **one** assertion fail on purpose (point it at one of your colleague's `draft_` tables, say), paste its message
   under `F`, and put the assertion back. One demonstration is the whole requirement.
6. *Restart* and *Run All*. The notebook must run top to bottom with every assertion passing.
7. Finish the five parts of every KPI, `AI_USE.md`, and the **Results** section of this README (below). Commit.
8. Follow `SUBMITTING.md`: commit everything, save the Git log, make the zip. Upload it.

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
5. **The five parts under every repaired and new KPI** (B1–B4, C1, C2), and **section `J`**, the one place the
   three counts are written: every join in your submission, one line each, with an ID (`J1`, `J2`, …). The checks
   and the how-would-I-know lines cite joins and cells by ID instead of pasting them again.
6. **`README.md`**: replace the *Start here* and *What is broken* sections with a **Results** section for the board:
   the six KPIs' headline figures, and three to five sentences a board member can read — what the numbers show, what
   was excluded and why, and **one thing this data cannot answer**, said as a fact about the data ("Olist has no cost
   data, so these figures say nothing about margin"), not as a hedge. Give each exclusion once, in words and one
   number; do not copy join counts here. If a sentence rests on a join, cite its ID from section `J`. Leave
   `SUBMITTING.md` alone.
7. **`AI_USE.md`**.
8. **Commits** whose messages name the cause, citing the join's ID in any commit that changes a join;
   **`GIT_LOG.txt`**; and the archive, **`hw2-submission.zip`**, made as `SUBMITTING.md` says.

Upload to the Homework 2 slot on **Moodle**: `hw2-submission.zip`.

## Rules

- **Never edit `data/raw/`.** Fix the queries.
- The brief is the definition. A number that matches the headline by a route the brief does not describe is not the
  fix.
- Every fix is a query: a rewritten query with its evidence and its check, not a changed keyword.
- AI may explain and suggest; you must be able to explain every line, in your own words, without notes. Say what you
  used it for in `AI_USE.md`.

## Hints, if stuck

1. Put each KPI's total beside the headline number it should add up to. Then, for every join in a KPI that
   disagrees, the three counts: did rows disappear, or appear?
2. For each table a KPI joins, say what one row is, then test it: `COUNT(*)` against `COUNT(DISTINCT …)` on the key
   you believe in. For the rates, count the paid orders that found no rate, by weekday.
3. Rows disappear when the other side has no match: the ECB publishes no rate on weekends and its holidays, and not
   every category has an English name, or any name. Rows appear more than once when the table you joined is finer
   than the thing you are counting: an item is not an order, and a review can cover two orders. The brief says what
   each average is over.

## The five parts, and what each one cites

**Cite, do not paste again**: where the evidence is in the notebook or in `J`, write its ID and quote the one line
that matters.

- **The sentence** says it back from `BRIEF.md`, with the rows named: what is measured, which rows, which rows left out.
- **The rows you expect** name their source: the brief, a section A cell by ID, or "neither fixes this".
- **The check** names its kind and its place: the row of `D` by its name, and the join IDs in `J` (`J3`, `A1`).
- **How would I know if this were wrong?** names the number that would move, which way, and the cell or row of `D`
  that would show it — the cause as what the join did to the grain, or which rows found no match, never as what you
  changed. Written before the repair.
- **`F`** holds the one assertion you made fail on purpose, with its message: once you put the assertion back, the
  notebook no longer shows it. The other KPIs name their assertion in their check line, with no message.

## Stretch (optional, not graded)

The brief counts one review, one vote. "One order, one vote" is another defensible rule. The notebook's last cell asks
which states move by more than 0.05 under it, and whether the board's reading would change.

## Git thread

This session's habit: **a commit that changes a join names the cause and cites the join's ID**, for example
"KPI 1 at the month's average rate, LEFT JOIN by month (J2)". The three counts live once, in section `J` of the
notebook; the ID in the message points the history's reader there. Commit after each repair, not once at the end.

## Grading

The rubric is on the course site. Correct fixes and the two new KPIs 30 · the checks and the how-would-I-know lines,
with the three counts for every join 30 · the sentences and the sourced estimates, and the Results section 25 · clean
submission and Git 10 · AI use 5. Late: one day at −10%; nothing after Sunday 23:59.

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
