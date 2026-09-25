# Diagnosis notes, joins, and trap log

Four notes, one per drafted KPI; then the **Joins** section; then the **trap log**. Fill in every part; short is fine.

**Cite, do not paste again.** Each piece of evidence is written down once. The notebook's cells have IDs: `A1`–`A4`
(the evidence), `B1`–`B4`, `C1`, `C2` (your queries), `D` (the reconciliation table; cite a row by its name) and `E`
(the assertions). Joins get their IDs below, in the Joins section: `J1`, `J2`, …. Where the evidence is in the
notebook or the Joins section, write its ID and quote the one line that matters. **Paste only what the notebook no
longer shows**: the message the **one** assertion gave when you made it fail on purpose, in that KPI's note only.

---

## Note 1 — KPI 1

1. **Symptom.** Which two numbers should agree and did not?

2. **Cause.** Why the draft produced that number: what the join did to the grain, or which rows found no match. Not
   what you changed.

3. **Evidence.** The join IDs and the section A cell (`J…`, `A1`), with the one line that shows the cause.

4. **Change.** The section B cell (`B1`), and one sentence on what the query now does.

5. **Verification.** The row of `D` by its name, and the assertion in `E` that guards it. If this is the one note whose
   assertion you made fail on purpose, paste its message here; otherwise leave the box empty:

```text

```

## Note 2 — KPI 2

1. **Symptom.** Which two numbers should agree and did not?

2. **Cause.** Why the draft produced that number: what the join did to the grain, or which rows found no match. Not
   what you changed.

3. **Evidence.** The join IDs and the section A cell (`J…`, `A2`), with the one line that shows the cause.

4. **Change.** The section B cell (`B2`), and one sentence on what the query now does.

5. **Verification.** The row of `D` by its name, and the assertion in `E` that guards it. If this is the one note whose
   assertion you made fail on purpose, paste its message here; otherwise leave the box empty:

```text

```

## Note 3 — KPI 3

1. **Symptom.** Which two numbers should agree and did not?

2. **Cause.** Why the draft produced that number: what the join did to the grain, or which rows found no match. Not
   what you changed.

3. **Evidence.** The join IDs and the section A cell (`J…`, `A3`), with the one line that shows the cause.

4. **Change.** The section B cell (`B3`), and one sentence on what the query now does.

5. **Verification.** The row of `D` by its name, and the assertion in `E` that guards it. If this is the one note whose
   assertion you made fail on purpose, paste its message here; otherwise leave the box empty:

```text

```

## Note 4 — KPI 4

1. **Symptom.** Which two numbers should agree and did not?

2. **Cause.** Why the draft produced that number: what the join did to the grain, or which rows found no match. Not
   what you changed.

3. **Evidence.** The join IDs and the section A cell (`J…`, `A4`), with the one line that shows the cause.

4. **Change.** The section B cell (`B4`), and one sentence on what the query now does.

5. **Verification.** The row of `D` by its name, and the assertion in `E` that guards it. If this is the one note whose
   assertion you made fail on purpose, paste its message here; otherwise leave the box empty:

```text

```

---

## Joins — the three counts, written once

Every join in your submission, your colleague's drafts included, one line each: rows in the left table, rows in the
result, distinct values of the left table's key in the result. The notes and your commit messages cite the ID.

```text
ID   KPI    join (left -> right)                         left   result   distinct left key   cell
J1
```

## Trap log

One line per data issue that changed a number, even where your own query was never wrong.

-
