# 01. Defining Data Science

*Section 1, Introduction · read 31 Jul 2026*

## Takeaways

1. **Structured / semi-structured / unstructured is about shape, not purpose.** The axis is
   whether the data fits a consistent, predefined format a machine can read, not whether it was
   deliberately collected or how meaningful it is. A scanned copy of a signed timesheet is
   unstructured: the blank form behind it is highly structured as a *concept*, but the scan is
   pixels, and no query touches it until OCR turns it into something else. Purposeful and
   structured are independent properties.

2. **The five-stage data journey: acquire, store, process, visualise, model.** Useful as a
   checklist for locating any piece of work: most analytics in practice lives in stages 1-4 and
   never reaches 5. Worth noting that the lesson puts *statistical hypothesis testing* inside
   stage 4, alongside visualisation. Inference on data you already have is still stage 4.

3. **Machine learning is a subset of artificial intelligence, not the reverse.** ML builds models
   that predict outcomes by learning patterns from data. AI is the broader field: higher-
   complexity models aimed at mimicking human cognition, including converting unstructured input
   into structured insight. There is AI that is not ML (rule engines, search, classical planning);
   there is no ML that is not AI.

## Questions the lesson left open

### What actually separates data science from business intelligence?

The lesson defines data science carefully but never contrasts it with BI, which matters to me
because BI & Analytics is my degree and the two get used interchangeably in job ads.

The shortest version I have: **BI answers questions you already know to ask. Data science finds
the questions, and estimates answers for cases you have not seen.**

- **BI is descriptive and retrospective.** What happened, what is happening, measured against a
  target. Aggregation across known dimensions, largely over structured data. The output is a
  report, a dashboard, a KPI tracked against a goal.
- **Data science is inferential and predictive.** Why it happened, what happens next, what to do
  about it. The output is a model that generalises to records it was never trained on.

The five stages in this lesson locate the boundary precisely. Stages 1-4 are common to both
disciplines. BI teams acquire, store, process and visualise, and the lesson explicitly puts
hypothesis testing in stage 4. **Stage 5, training a predictive model, is where data science
begins and BI stops.** So statistical rigour alone does not make something data science; the
predictive model does.

The tooling overlaps almost entirely: SQL, Python, a warehouse, a visualisation layer. The
question being asked is what differs.

**Against my own work:** my undergraduate capstone was a KPI system for a university health
service unit, built on a dimensional warehouse: four star schemas, documented ETL, pre/post
hypothesis testing on patient response times. By this framing that is stages 1-4: BI with real
statistical inference in it, deliberately scoped to stop short of stage 5. Not a limit of the
data. A scoping decision, and knowing which side of the line the work sits on is the point.
