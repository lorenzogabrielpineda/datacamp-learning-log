# 03. Defining Data

*Section 1, Introduction · read 1 Aug 2026*

## Takeaways

1. **Structure and value type are independent axes.** How data is organised says nothing about
   what kind of values it holds. A JSON file with no tabular shape can contain nothing but
   measurements; a strict relational table can contain nothing but free text. The mistake to
   avoid is letting the first classification colour the second, which is easy to do because
   loose formats feel like they should hold loose values.

2. **Primary and secondary describe a relationship, not the data.** Nothing inside a file makes
   it one or the other. The same weather readings are primary to the station that recorded them
   and secondary to a researcher who downloads them a year later. The useful question is whether
   I caused this data to exist or whether it arrived. Corporate ownership does not settle it
   either: a parent company handing over a subsidiary's customer file is still secondary to the
   team receiving it, because they inherit the collection conditions without being able to see
   them.

3. **Raw and structured are separate properties.** Raw means nothing has been done to the data
   since it left its source. Structured means it carries a consistent, machine-readable format.
   A database export that nobody has touched is both at once. The lesson's phrasing runs the two
   ideas together in one paragraph, which invites the reading that raw data is unorganised data,
   but a raw file can be as rigidly structured as any other.

## Questions the lesson left open

### Is a CSV structured or semi-structured?

The lesson lists CSV as semi-structured, alongside HTML and JSON. Most working practice calls it
structured. Both positions hold up, and which one is right depends on the axis being used.

- **Shape:** a CSV is rows and columns with consistent fields. It loads directly into a table or
  a dataframe with no intermediate step. By shape it is structured.
- **Guarantees:** a CSV declares no schema, enforces no types and does not require row 400 to
  carry the same fields as row 399. By guarantees it is semi-structured.

The difference is not academic. A CSV has the shape of structured data without the promises of
it, which is why loading one is where pipelines break. A zip code of `01805` comes in as the
number `1805` because nothing in the file said the column was text. A comma inside an address
field shifts every value after it by one column. A relational database would have refused both
on the way in.

The answer I would give: shaped like structured data, guaranteed like semi-structured data.
