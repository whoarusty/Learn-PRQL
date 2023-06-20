<div align="center">

# Learn PRQL

Learn how to write simple, powerful, pipelined queries as an SQL replacement using
  <br>
  **P**ipelined **R**elational **Q**uery **L**anguage, pronounced "Prequel".

  <a href="https://prql-lang.org/"><img src="https://github.com/PRQL/prql-brand/blob/main/logos/PNG/prql-wordmark.png" width=500></a>

  [![Website](https://img.shields.io/badge/INTRO-WEB-blue?style=for-the-badge)](https://prql-lang.org)
  [![GitHub CI Status](https://img.shields.io/github/actions/workflow/status/PRQL/prql/pull-request.yaml?branch=main&logo=github&style=for-the-badge)](https://github.com/PRQL/prql/actions?query=branch%3Amain+workflow%3Atest-all)
  [![Stars](https://img.shields.io/github/stars/PRQL/prql?style=for-the-badge)](https://github.com/PRQL/prql/stargazers)

</div>

## Tutorial Prerequisites

* [PRQL](https://github.com/PRQL/prql)
* [DuckDB](https://github.com/duckdb/duckdb/)
* [Chinook Database](https://github.com/lerocha/chinook-database)

## Introduction

> PRQL [is] designed to serve the growing need of writing 
> analytical queries, emphasizing data transformations,
> development speed, and readability.
> &nbsp;&nbsp;&nbsp;&nbsp;*[PRQL Website](https://prql-lang.org/)*

As a query language, PRQL can be used for various data management tasks and by various professionals -- programmers, analysts, data engineers, and data scientists. For this tutorial, an **E**xtract, **L**oad, **T**ransform (ELT) methodology was used, where the [Chinook CSV data](https://github.com/whoarusty/Learn-PRQL/tree/6714e47c95d25b831a27ea9a80d473b9f03a2bc2/files/data/chinook%20data) was ingested into a [DuckDB database](https://github.com/whoarusty/Learn-PRQL/blob/6714e47c95d25b831a27ea9a80d473b9f03a2bc2/files/data/chinook%20data/chinook.ddb) and queries will be written to transform the data -- such as selecting, filtering, and performing calculations.

<div align="center">
<img src="https://raw.githubusercontent.com/whoarusty/Learn-PRQL/main/files/images/extract-load-transform.png" width=400>
<br />
</div>

This tutorial will focus on writting PRQL queries to see how easy and powerful the language is to transform data.

## Table of Contents

- PRQL Essentials
- Common Tasks
  - See sample data
  - Count rows
  - Select a subset of columns
  - List unique values
  - Filter by condition
  - Sort results
  - Get _n_ rows
  - Create calculated columns
- Summarize Data
- Join Columns From Multiple Tables

## PRQL Essentials

PRQL is a linear **pipeline of transformations**, where each line of the query takes the previous results and adjusts it in some way before passing it to the next transform keyword.

Currently, there are ten transforms:

| Transform   | Purpose                                                            |
| ----------- | ------------------------------------------------------------------ |
| `from`      | Start from a table                                                 |
| `derive`    | Compute new columns                                                |
| `select`    | Pick & compute columns                                             |
| `filter`    | Pick rows based on their values                                    |
| `sort`      | Order rows based on the values of columns                          |
| `join`      | Add columns from another table, matching rows based on a condition |
| `take`      | Pick rows based on their position                                  |
| `group`     | Partition rows into groups and applies a pipeline to each of them  |
| `aggregate` | Summarize many rows into one row                                   |
| `window`    | Apply a pipeline to overlapping segments of rows                   |

As indicated with the above table, to begin with PRQL, start with the `from` transform. The following code examples use comments that start with a `#` and don't affect the query or results.

```elm
from {table_reference} # start with a from and table name
```

The `from` transform will return all columns and all rows from the table. Note: depending on your data set, this could be a very lengthy result. 

example:

```elm
from artists # show all columns and data from the artists table
```

<details>
<summary>DuckDB results: (click to expand)</summary>

```
┌───────────┬────────────────────────────────────────────────────────────────────────────────────┐
│ artist_id │                                        name                                        │
│   int32   │                                      varchar                                       │
├───────────┼────────────────────────────────────────────────────────────────────────────────────┤
│         1 │ AC/DC                                                                              │
│         2 │ Accept                                                                             │
│         3 │ Aerosmith                                                                          │
│         4 │ Alanis Morissette                                                                  │
│         5 │ Alice In Chains                                                                    │
│         · │      ·                                                                             │
│         · │      ·                                                                             │
│         · │      ·                                                                             │
│       271 │ Mela Tenenbaum, Pro Musica Prague & Richard Kapp                                   │
│       272 │ Emerson String Quartet                                                             │
│       273 │ C. Monteverdi, Nigel Rogers - Chiaroscuro; London Baroque; London Cornett & Sackbu │
│       274 │ Nash Ensemble                                                                      │
│       275 │ Philip Glass Ensemble                                                              │
├───────────┴────────────────────────────────────────────────────────────────────────────────────┤
│ 275 rows (10 shown)                                                                  2 columns │
└────────────────────────────────────────────────────────────────────────────────────────────────┘
```

Note: this result set was shortened for brevity of the tutorial.

</details>

As an example of how PRQL implements transformations, we can select just the name column from the artists table.

example:

```elm
from artists # using the artists table
select name  # only display the name column
```

<details>
<summary>DuckDB results: (click to expand)</summary>

```
┌────────────────────────────────────────────────────────────────────────────────────┐
│                                        name                                        │
│                                      varchar                                       │
├────────────────────────────────────────────────────────────────────────────────────┤
│ AC/DC                                                                              │
│ Accept                                                                             │
│ Aerosmith                                                                          │
│ Alanis Morissette                                                                  │
│ Alice In Chains                                                                    │
│      ·                                                                             │
│      ·                                                                             │
│      ·                                                                             │
│ Mela Tenenbaum, Pro Musica Prague & Richard Kapp                                   │
│ Emerson String Quartet                                                             │
│ C. Monteverdi, Nigel Rogers - Chiaroscuro; London Baroque; London Cornett & Sackbu │
│ Nash Ensemble                                                                      │
│ Philip Glass Ensemble                                                              │
├────────────────────────────────────────────────────────────────────────────────────┤
│                                275 rows (10 shown)                                 │
└────────────────────────────────────────────────────────────────────────────────────┘
```

Note: this result set was shortened for brevity of the tutorial.

</details>

As another example of how PRQL utilizes transforms, both of the PRQL queries will produce the same results:

```elm
from artists                             # use the artists table
filter (name == 'Philip Glass Ensemble') # filter only for 'Philip Glass Ensemble'
select name                              # only display the name column based on the filter
```

```elm
from artists                             # use the artists table
select name                              # show all results and only display the name column
filter (name == 'Philip Glass Ensemble') # display just the 'Philip Glass Ensemble' artist
```

<details>
<summary>DuckDB results from either query: (click to expand)</summary>

```
┌───────────────────────┐
│         name          │
│        varchar        │
├───────────────────────┤
│ Philip Glass Ensemble │
└───────────────────────┘
```

</details>

**PRQL Essentials Summary:** PRQL has ten transforms that establish the foundation of queries. Depending on what you want to do with your query, the transform will produce a set of results, which then can be further modified with another transform or set of transforms. Always start your PRQL query with `from` and continue to use other transforms to produce the desired query results.

<br />

## Common Tasks

### See sample data

When working with data and databases, you will often need to verify data exists in your columns to make sure the ingestion process was successful. To do this with PRQL you can use the `from` transform. It is important to note that just using `from` will produce all results in the table, which could be hundreds or thousands of rows. To limit the result set you can then use the `take` transform. For example to see the columns and first five rows of the artists table, you can query the data as:

```elm
from artists
take 5
```

<details>
<summary>DuckDB results from : (click to expand)</summary>

```
┌───────────┬───────────────────┐
│ artist_id │       name        │
│   int32   │      varchar      │
├───────────┼───────────────────┤
│         1 │ AC/DC             │
│         2 │ Accept            │
│         3 │ Aerosmith         │
│         4 │ Alanis Morissette │
│         5 │ Alice In Chains   │
└───────────┴───────────────────┘
```

</details>

### Count rows

To see how many rows are in a table, you can use the aggregate count transfrom, which will summarize the results based on the previous transform. For example, to count all the rows in the artist table you can use the following transforms:

```elm
from artists      # select all rows from the artists table
aggregate count   # summarize all the rows using count
```

<details>
<summary>DuckDB results from : (click to expand)</summary>
```
┌──────────────┐
│ count_star() │
│    int64     │
├──────────────┤
│          275 │
└──────────────┘
```
</details>

Tip: the column name for the row count may not be very descriptive (depending on the database management system). To provide a more descriptive column name you can create an alias. Alias names can be descriptive like `total_artists` or simple like `ct` as a shortened `count`.

`alias_name = {expression}`

As an example, you can create an alias total_artists to represent the count results:

```elm
from artists
aggregate total_artists = count
```

<details>
<summary>DuckDB results from : (click to expand)</summary>
```
┌───────────────┐
│ total_artists │
│     int64     │
├───────────────┤
│           275 │
└───────────────┘
```
In DuckDB, the column changed from `count star()` to `total_artists`.
</details>

### Select a subset of columns

### List unique values

### Filter by condition

### Sort results

### Get the top _n_ rows

### Create calculated columns

## Summarize Data

## Join Columns From Multiple Tables
