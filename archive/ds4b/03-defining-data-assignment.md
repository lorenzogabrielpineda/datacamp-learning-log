# Classifying Datasets

*Lesson 03, Defining Data · 1 Aug 2026*

Each scenario is classified on three independent axes: how the data is structured,
what kind of values it holds, and where it came from relative to whoever is using it.

---

**1. A company has been acquired and now has a parent company. The data scientists have received a spreadsheet of customer phone numbers from the parent company.**

- **Structure Type:** Structured. A spreadsheet of phone numbers is rows and columns with the same fields in every row.
- **Value Type:** Qualitative. Phone numbers are written as digits but they identify a person, they do not measure anything. Averaging them would be meaningless.
- **Source Type:** Secondary. The data scientists received the file, they did not collect it. The two companies now share an owner, but ownership is not the same as provenance: the team inherits the collection conditions without knowing them, and that gap is the reason the distinction exists.

**2. A smart watch has been collecting heart rate data from its wearer, and the raw data is in JSON format.**

- **Structure Type:** Semi-structured. JSON carries its own keys, so a program can find its way around the file without the data being tabular.
- **Value Type:** Quantitative. Heart rate is measured on a real scale. Averages, peaks and resting rates are all standard measures.
- **Source Type:** Primary. The watch generated the readings for its own wearer.

**3. A workplace survey of employee morale that is stored in a CSV file.**

- **Structure Type:** Semi-structured by this lesson's classification. A CSV holds no schema, no types and no rule forcing every row to carry the same fields. Worth recording that the practical view differs: in day to day work a CSV is treated as structured because it loads straight into a table. Both readings hold up depending on whether the axis is shape or guarantees, and the distance between them is where CSV loading tends to fail. A zip code of `01805` arriving as `1805` is the usual example.
- **Value Type:** Qualitative. Morale is subjective. Even where a survey captures it on a 1 to 5 scale, the numbers are ordered categories rather than measurements, since the distance between 1 and 2 is not guaranteed to match the distance between 4 and 5.
- **Source Type:** Primary. The workplace ran the survey on its own employees for its own use.

**4. Astrophysicists are accessing a database of galaxies that has been collected by a space probe. The data contains the number of planets within each galaxy.**

- **Structure Type:** Structured. A database enforces columns, types and constraints.
- **Value Type:** Quantitative. A count of planets can be summed, averaged and compared.
- **Source Type:** Secondary. The astrophysicists are accessing readings the probe mission collected. They are not the ones who gathered them.

**5. A personal finance app uses APIs to connect to a user's financial accounts in order to calculate their net worth. They can see all of their transactions in a format of rows and columns and looks similar to a spreadsheet.**

- **Structure Type:** Structured. Transactions arrive as rows and columns with consistent fields.
- **Value Type:** Quantitative. Transaction amounts are measurements that add up to a net worth figure.
- **Source Type:** Secondary. The banks generated the transaction records. The app requests them through an API and displays them. The API is the pipeline, not the origin and not the format.

---

## Two things worth separating

**Structure type and value type are independent.** The container does not tell you what is
inside it. A JSON file can hold nothing but integers, and a strict relational table can hold
nothing but free text. Scenario 2 is the clearest case: the format is loose, the values are
as numeric as anything in this set.

**Primary and secondary are relational, not properties of the file.** The same readings are
primary to whoever generated them and secondary to everyone who receives them afterwards. The
question to ask is not what the data is, but whether I caused it to exist or it arrived.
