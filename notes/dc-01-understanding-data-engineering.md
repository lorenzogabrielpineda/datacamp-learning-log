# Understanding Data Engineering

DataCamp, Data Engineer path — course 1 of 9. Finished 17 August 2026.

Three chapters: the data workflow and what a data engineer actually does, storing data,
then moving and processing it.

## What I took from it

### 1. Data engineer vs data scientist is a question about output, not intent

Both work on data all day, so trying to separate them by what they are *interested in*
gets you nowhere. The split is what leaves their hands. The engineer builds the
foundation; the scientist builds on top of it.

The thing I had to correct in my own head: engineers don't build only for data
scientists. They build for analysts, BI developers, applications and ML services. Most
organisations running a data engineering function have no data scientist in them at all.

### 2. A warehouse is schema-on-write; a lake is schema-on-read

This was the one that actually landed, because I worked it out from the mechanics before
I had the names for it.

A **data warehouse** requires the data to fit a model *before* it lands. The modelling
work happens on the way in. That is what makes a warehouse queryable and consistent, and
it is also what makes it expensive to change: once a model is established and things are
built on top of it, altering it is real work.

A **data lake** takes the data as it is and imposes structure at the point of *reading*.
The extract stage is the only place a shape gets fitted to it. That is why lakes are
cheap to load into and why they can hold formats a warehouse would reject outright.

Concretely: if Spotify wanted to keep raw listening events, every skip and scrub and
replay, a warehouse would demand a decision about what a row means before storing any of
it. A lake stores it first and lets each consumer decide later.

### 3. Batch has positive advantages, not just lower requirements

My first instinct was to argue that batch is fine when you don't *need* streaming, which
is an argument about streaming, not about batch. The actual case for batch stands on its
own: it is cheaper, it is far simpler to debug because you can look at a finished set of
records instead of a moving one, and it is re-runnable — if a batch is wrong you fix the
logic and run it again over the same input.

## Where this maps onto work I have already done

The vocabulary is new to me; a fair amount of the practice is not. Naming it properly
matters, because "I built this" and "I built this and can tell you what it is" are
different claims in an interview.

- My **capstone** put four star schemas and documented ETL flows under a mixed-methods
  study, across nine interconnected databases. That is schema-on-write, warehouse-shaped
  work — the model was decided before anything was loaded, which is exactly why the
  reporting on top of it was consistent.
- At **ABS-CBN**, I automated a scoring model's recalibration so the full dataset
  rescored whenever an input changed. In this course's vocabulary that is the processing
  and scheduling half of the workflow, not the analysis half.
- Also at ABS-CBN, cleaning the audit universe — removing invalid entries, divested
  entities and audits closed through another route, across roughly a thousand records —
  is ingestion and validation. I had always filed it as "the tedious part before the
  analysis." It has a name and it is a stage.

## Still working on

**What changed in the world to make ELT practical rather than just conceivable?** I can
state what ETL and ELT each do. That is not the same question, and I want to answer the
one actually being asked before I write anything down about it.
