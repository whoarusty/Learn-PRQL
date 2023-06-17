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



## Table of Contents
- PRQL Essentials
- Common Tasks
  - Count rows
  - See sample data
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
| ----------- | -------------------------------------------------------------------|
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

``` elm
from {table_reference} # start with a from and table name
```

The `from` transform will return all columns and all rows from the table. Note: depending on your data set, this could be a very lengthy result. 

example:

``` elm
from artists # show all columns and data from the artists table
```

results:
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

As an example of how PRQL implements transformations, we can select just the name column from the artists table.

example:

``` elm
from artists # using the artists table
select name  # only display the name column
```

results:

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

As another example of how PRQL utilizes transforms, both of the PRQL queries will produce the same results:

``` elm
from artists                             # use the artists table
filter (name == 'Philip Glass Ensemble') # filter only for 'Philip Glass Ensemble'
select name                              # only display the name column based on the filter
```

``` elm
from artists                             # use the artists table
select name                              # show all results and only display the name column
filter (name == 'Philip Glass Ensemble') # display just the 'Philip Glass Ensemble' artist
```

results from either query:
```
┌───────────────────────┐
│         name          │
│        varchar        │
├───────────────────────┤
│ Philip Glass Ensemble │
└───────────────────────┘
```

**PRQL Essentials Summary:** PRQL has ten transforms that establish the foundation of queries. Depending on what you want to do with your query, the transform will produce a set of results, which then can be further modified with another transform or set of transforms. Always start your PRQL query with `from` and continue to use other transforms to produce the desired query results.

<br />


## Common Tasks

### Count rows

### See sample data

### Select a subset of columns

### List unique values

### Filter by condition

### Sort results

### Get the top _n_ rows

### Create calculated columns

## Summarize Data

## Join Columns From Multiple Tables
