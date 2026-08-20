# Introduction to SQL

DataCamp, Associate Data Engineer in SQL. Course 2 of 9, finished 20 August 2026.

Two chapters: relational databases, then querying. Chapter 1 ran across two sittings on
18 and 19 August. Chapter 2 took a single 43 minute sitting on the 20th.

## What I took from it

### 1. Row limiting is one of the places SQL flavours diverge

The keyword depends on the database, and so does where it goes in the statement.

PostgreSQL, which this course teaches, uses `LIMIT` at the **end** of the statement:

```sql
SELECT case_id, patient_response_time
FROM lc1_clinic_cases
LIMIT 10;
```

SQL Server uses `TOP`, which goes **immediately after `SELECT`**, before the column list:

```sql
SELECT TOP (10) case_id, patient_response_time
FROM lc1_clinic_cases;
```

Most flavours stay close to each other because they follow the same ISO standard, so what
tends to vary is the keywords rather than the shape of the language. A positional cue that
helps with `TOP`: it sits where `DISTINCT` sits, immediately after `SELECT`, rather than
anywhere in the column list.

These are easy to mix up, and the two mistakes compound: reaching for the other flavour's
keyword, then putting it where neither flavour accepts it. SQL reads similarly
across engines that you can be fluent at reading it and still write a statement that will
not run. Nothing warns you until an engine rejects it.

### 2. `LIMIT` answers "how many", never "which ones"

`LIMIT 10` returns ten rows. It does not return the top ten of anything. Without an
`ORDER BY`, the rows that come back are whatever the engine reached first, and that
ordering is not guaranteed to be stable between runs.

So if the question is "which ten cases had the fastest response times," the query has to
say so:

```sql
SELECT case_id, patient_response_time
FROM lc1_clinic_cases
ORDER BY patient_response_time
LIMIT 10;
```

It is a natural assumption that a limit implies a ranking, because that is how the request
is usually phrased out loud. The query does not infer it. This is the error shape I most
want to catch in my own work: not a query that breaks, but a query that runs, returns
something plausible, and answers a different question than the one asked.

### 3. Keys are what let two tables be used together

A **primary key** uniquely identifies a row within its own table. A **foreign key** is a
column holding another table's primary key. That is the mechanism that lets two tables be
connected: the same value appears in both places, so a row on one side can be matched to a
row on the other.

A primary key does not have to be a single column. A **composite key** is two or more
columns that are unique only in combination, and either column on its own repeats.
This is not an edge case. In the PSA Family Income and Expenditure Survey data I am
building a dashboard on, the household identifier is only unique within a single survey
wave, so the key has to be `(wave, household_id)`. Treating the household ID alone as the
key would quietly merge three waves of different households into one.

On the word itself: "relational" is usually assumed to mean the relationships between
tables, since those are the memorable part. It comes from *relation*, the formal
term for a table. The keys are still what make the connections work, so the practical
picture is unaffected either way.

### 4. A data type decides how a column sorts, and the wrong one raises no error

If `patient_response_time` holds 9, 45 and 100, but the column is typed as text rather than
as a number, then `ORDER BY patient_response_time` returns them in this order:

```
100
45
9
```

Text sorts character by character, so "1" comes before "4" comes before "9", and how long
the number is never enters into it. It is a fair assumption that a wrong type would throw an
error, because that is what a wrong type does in most languages. Here nothing breaks. The
query runs, returns something that looks like an ordering, and is wrong, which is the same
failure shape as section 2 arriving from a different direction.

### 5. Keywords upper case, identifiers lower case

The engine does not care about either. The convention is keywords in capitals (`SELECT`,
`FROM`, `ORDER BY`) and table and field names in lower case, and the point is that it lets
you see the shape of a statement without reading it: the capitals are the structure, the
lower case is the data. Consistent table naming does the same job across a schema.

## Where this maps onto work I have already done

- The first query I reached for unprompted was against **clinic case records** rather than
  a course dataset: cases and their response times, cut down to a handful of rows. The
  tables I reached for by instinct were ones whose shape I already knew, which is
  why the query came out quickly.
- The point in section 2 is not a new idea to me, only a new place to find it. In the
  **ABS-CBN** work, cutting the audit universe down to a valid set was the whole job before
  any analysis could begin, and a wrong cut would still have produced numbers that looked
  reasonable. `ORDER BY` and `LIMIT` are where that same failure lives in SQL specifically.

## Still working on

**Recall versus recognition.** I know what each keyword does when I see it. Producing the
syntax cold, without the exercise scaffolding in front of me, is a different skill and I am
not there yet. Guided exercises do not test it, so it is not something the course would
have surfaced on its own.

**Everything here is one table.** This course queries a single table at a time. Any
question needing two tables joined, such as patients appearing at more than one clinic, or
case records matched against demographics held separately, I have no way to answer yet.
That is the next course. It is also the exact problem waiting in the FIES dashboard, where
the household ID is only unique within a single survey wave.

**Data types.** I want more practice here than this course gave. The sorting behaviour in
section 4 is the kind of thing I would rather know cold than reason out each time, and it
comes back properly in Introduction to Relational Databases in SQL.

**Carried forward from course 1:** what changed in the world to make ELT practical rather
than just conceivable. Still unanswered, and still mine to answer.
