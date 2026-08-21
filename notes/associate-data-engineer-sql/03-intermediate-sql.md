# Intermediate SQL

DataCamp, Associate Data Engineer in SQL. Course 3 of 9, finished 21 August 2026.

Four chapters: selecting data, filtering records, aggregate functions, then sorting and
grouping. Done in two sittings on the same day, about seven hours in total including the
write up.

## What I took from it

### 1. `WHERE` and `HAVING` split on *when* they run, not on how much they look at

Both filter rows out, so the question is which one a given condition belongs in. The test
is not how many fields the condition touches. It is whether a single row, on its own, holds
enough information to answer it.

`WHERE` runs before any grouping happens, so it filters individual rows:

```sql
SELECT AVG(resolution_time)
FROM lc1_clinic_cases
WHERE severity = 'Mild';
```

One row is enough to know whether its severity is mild, so this belongs in `WHERE`. The
filter happens first and the average is then taken over what survives.

`HAVING` runs after grouping, so it filters groups on their aggregate values:

```sql
SELECT severity, AVG(resolution_time)
FROM lc1_clinic_cases
GROUP BY severity
HAVING AVG(resolution_time) > 5;
```

No single row can tell you what a group's average is, so this condition has nowhere to live
except `HAVING`.

The two are easy to swap, and the reasoning that leads there is sound right up until the
last step: `WHERE` does run before aggregation, and that is true, but it is the reason
`WHERE` works for row conditions rather than a reason to avoid it. Early is the feature.
There is also a cost to putting a row condition in `HAVING` even where the result comes out
the same, because `WHERE` discards those rows before the grouping work is done at all.

### 2. Clauses do not run in the order they are written

```
FROM  ->  WHERE  ->  GROUP BY  ->  HAVING  ->  SELECT  ->  ORDER BY  ->  LIMIT
```

`SELECT` is written first and runs sixth. That single fact explains a class of errors that
otherwise looks arbitrary:

```sql
SELECT price * quantity AS total
FROM sales
WHERE total > 1000;
```

This fails. When `WHERE` is evaluated, `SELECT` has not run yet, so the alias `total` does
not exist. The fix is to repeat the expression, since `WHERE` cannot see aliases but
evaluates expressions on columns without complaint:

```sql
SELECT price * quantity AS total
FROM sales
WHERE price * quantity > 1000;
```

Or push it down one level, which avoids writing the expression twice:

```sql
SELECT total
FROM (SELECT price * quantity AS total FROM sales) s
WHERE total > 1000;
```

The useful half of this is the asymmetry. `ORDER BY` runs *after* `SELECT`, so aliases do
work there: `ORDER BY total` is fine in the same query where `WHERE total` is not. Knowing
the order turns that from an inconsistency to be memorised into something you can derive.

### 3. `ROUND()` takes a negative second argument, and it rounds to nearest

The second argument is how many decimal places, and it runs in both directions from the
decimal point.

| Call | Result | |
|---|---|---|
| `ROUND(1234.56, 1)` | `1234.6` | one place right |
| `ROUND(1234.56, 0)` | `1235` | nearest whole number |
| `ROUND(1234.56, -1)` | `1230` | nearest ten |
| `ROUND(1234.56, -2)` | `1200` | nearest hundred |
| `ROUND(1234.56, -3)` | `1000` | nearest thousand |

Positive moves right, negative moves left. The negative direction is the one worth having,
because reporting figures to the nearest thousand is a real request and doing it in the
query beats doing it afterwards.

Worth stating separately: `ROUND` goes to the *nearest* value, not upward. `ROUND(1234.56, -2)`
returns 1200 rather than 1300, because 1234.56 is closer to 1200. The name suggests upward
motion more strongly than the behaviour warrants, which is the sort of thing that only shows
up when a reported total is off by a hundred.

### 4. `COUNT(*)` and `COUNT(column)` count different things

`COUNT(*)` counts rows. Every row, including one where every column is missing.
`COUNT(column_name)` counts non-null values in that column.

The gap between the two numbers is exactly the number of nulls:

> 100 patient records, 12 of them with no discharge date recorded.
> `COUNT(*)` returns 100. `COUNT(discharge_date)` returns 88.

`COUNT(DISTINCT column_name)` gives a third number again, counting distinct non-null values.
Three counts over the same table, all correct, all answering different questions. Which one
belongs in a report depends on whether "how many patients" means rows, or records with the
field filled in.

### 5. `NULL` is unknown, and it changes results without saying so

`NULL` is not zero and not an empty string. An empty string is a known value that happens to
be empty. `NULL` is the absence of a value, and SQL treats it as unknown rather than as
anything in particular. Four consequences, all quiet:

- Aggregates skip nulls, so `AVG` over 100 rows where 20 are null divides by 80. The result
  is a reasonable looking number computed on a different denominator than the one intended.
- Comparing to null returns null rather than true or false, so `WHERE x = NULL` never matches
  anything. `IS NULL` and `IS NOT NULL` exist because of this.
- Null propagates through arithmetic. `price * quantity` is null if either side is, so a
  row's total goes missing rather than raising an error.
- The intuition around "not equal" breaks as well. `WHERE status != 'Closed'` silently drops
  rows whose status is null, because `NULL != 'Closed'` evaluates to null rather than to
  true. Rows that a reader would expect in the result are not in it.

None of these announce themselves. They are the same failure shape as row limiting without
an order, from course 2: the query runs, returns something plausible, and answers a slightly
different question.

### 6. What guided exercises optimise for, and what they do not

Adding a field that the exercise did not ask for gets marked wrong, because the checker
compares output exactly. That is a reasonable way to grade, and it does train you to answer
the literal question asked.

It is worth naming the difference anyway, because the habit does not transfer. In the
`ORDER BY` example below, returning durations alone satisfies the stated question and is
close to useless in practice, since you get ten numbers with no way to tell which cases they
belong to. Analysis usually wants the identifier alongside the measure. The exercise
environment has one right answer and real questions do not, which is the same reason a
finished course is not a portfolio piece.

## Where this maps onto work I have already done

- The `ORDER BY` gap from course 2 is closed. Note 02 recorded that `LIMIT` answers "how
  many" and never "which ones". Asked cold for the ten longest running cases in
  `lc1_clinic_cases`, the query came out unaided:

  ```sql
  SELECT case_id, duration
  FROM lc1_clinic_cases
  ORDER BY duration DESC
  LIMIT 10;
  ```

  `ORDER BY` decides which ten. `LIMIT` only decides how many.

- The capstone KPI cards were group by questions answered without a database. One of them
  was average case resolution time for mild cases. The aggregation for those cards was
  done in Notion, so the logic existed as a set of filtered views rather than as queries.
  Everything in this course is the language those cards were already speaking: a filter on
  severity, an average over what survives it, and a grouping when the same card had to be
  cut several ways. Being able to write that as one statement is a different thing from
  having built the card, and it is the part that transfers to a job.

- Reporting to the nearest thousand came up in the ABS-CBN work, which involved figures at a
  scale where full precision was noise. `ROUND` with a negative second argument does in the query what
  was being done downstream.

- Null handling is waiting in the FIES data. Survey microdata carries missing responses
  as a matter of course. Section 5 says that an average over a column with nulls silently
  changes its own denominator, and household survey figures are exactly where that would
  produce a plausible and wrong national number.

## Still working on

**Applying the `WHERE` and `HAVING` test under pressure.** The rule in section 1 is clear
when written out. Reaching for the right one while composing a query, rather than checking
afterwards, is the part that needs repetition. The next course joins tables, which adds a
third place a condition can live.

**Null mechanics rather than null instinct.** That nulls can distort an aggregate is
something I already assumed. The specific behaviours in section 5, particularly the "not
equal" case, are the ones I want to know cold instead of deriving each time.

**Answering the question that was asked, first.** Explaining how a function works and
returning the value it produces are two different responses, and the second one is what a
question like "what does this return" is asking for. The explanation is worth having, but it
belongs after the answer rather than instead of it.

**Carried forward from course 1:** what changed in the world to make ELT practical rather
than just conceivable. Still unanswered, and still mine to answer.
