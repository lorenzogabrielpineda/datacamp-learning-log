# 02. Data Science Ethics

*Section 1, Introduction · read 5 Aug 2026*

## Takeaways

1. **Unfairness comes in two shapes, and only one of them is usually illegal.** An allocation
   harm withholds something: a loan, a tenancy, a job. A quality-of-service harm withholds
   nothing; the system simply works worse for one group than another, like speech recognition
   that transcribes one accent at a markedly lower accuracy than another. The same underlying
   bias can produce either. Regulation mostly reaches the first, which is exactly why the second
   is the one that goes unaudited, and it is still a harm when it does.

2. **Dataset bias and data quality fail in different places.** Bias is a property of who is in
   the sample: a commuter survey run only at the main terminal between 6 and 9am describes
   peak-hour terminal users and quietly presents them as commuters. Quality is a property of the
   records themselves: an income field filled in pesos by some respondents and in thousands by
   others, with nothing marking which is which. A perfectly collected dataset can still be
   unrepresentative, and a perfectly representative one can still be unusable. Constrain the
   field to fix the second; you cannot constrain your way out of the first.

3. **Dropping a protected attribute does not remove it from the model.** It is reconstructable
   from whatever correlates with it: occupation, spending categories, income trajectory, gaps
   in a credit file. Deleting the column removes the ability to see what the model is doing with
   the signal, not the signal. This produces the awkward but correct conclusion that fairness
   auditing needs the protected attribute even though the model must not use it: to demonstrate
   that outcomes do not differ by group, outcomes have to be measured by group.

## Questions the lesson left open

### If a company has a code of conduct, is ethics already handled?

Not on the lesson's own definitions, and the gap between them is where the case studies happen.

- **Ethics** is voluntary, shared values, no enforcement mechanism, no law required for it to
  exist. It is the only one of the three that stands on its own.
- **Compliance** is following the law where the law has been written. It requires a statute to
  exist first, so it is always behind the technology.
- **Governance** is the machinery an organisation builds to enforce the first and satisfy the
  second: review boards, release checklists, sign-offs.

A code of conduct is governance. It is genuinely useful and it is not the same thing as ethics,
because it can only encode the harms someone already thought of. The 2019 Apple Card credit
limit case is the illustration: the model did not take gender as an input, which satisfied the
policy as written, and the outcomes still differed by gender. Nobody broke a rule. The rule did
not cover it.

The practical version: a checklist tells you whether you did the things on the list. It cannot
tell you whether the list was right.
