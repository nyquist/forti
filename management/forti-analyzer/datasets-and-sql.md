# Datasets and SQL

## SQL SELECT

A dataset is an SQL SELECT query.

`SELECT column [as new_name], function`

`FROM table`

Log types: $log-attack, $log-dlp, $log-event, $log-netscan, $log-app-ctrl, $log-emailfilter, $log-traffic, $log-virus, $log-webfilter, $log-traffic, $log-virus, $log-webfilter

`WHERE expression`

$filter = current dropdown filter of the view

`GROUP BY`

`ORDER BY column {asc|desc}`

`LIMIT number`

`OFFSET number`

## SQL Functions

* **normal**: operate on each element of the column
  * `NULLIF(expression1, expression2)` =&#x20;
    * NULL,  if expression1==expression2
    * expression 1, otherwise
  * `COALESCE(expression1, expression2,…)` = returns first non-NULL expression among it’s arguments. If all arguments are NULL, then it returns NULL.&#x20;
    * E.g: `COALESCE(catdesc, ‘unknown’)` would be used to replace a NULL catdesc with ‘unknown’
* **Aggregate**: use the entire column of data as input and produces a single output
  * `AVG(expression)`
  * `COUNT(expression)`
  * `COUNT(*)`
  * `FIRST(expression)`
  * `LAST(expression)`
  * `MAX(expression)`
  * `MIN(expression)`
  * `SUM(expression)`

### Built-in FortiAnaylyzer functions:

* `root_domain(hostname)` = gets the root domain of a hostname
* `nullifna(expression)` = `NULLIF(NULLIF(expression, ‘N/A’), ‘N/A’)` = the inverse of COALESCE. Eg: `COALESCE (nullifna(‘user’), ‘srcip’)`
* `email_user(expression)` = returns everything before @
* `email_domain(expression)` = returns everything after @
* `from_dtime(bigint)` = returns device timestamp without TZ
* `from_itime(bigtime)` = returns FAZ timestamp without TZ

### Builtin FortiAnalyzer macros:

| Macro            | Format               | Example                                                              |
| ---------------- | -------------------- | -------------------------------------------------------------------- |
| `$hour_of_day`   | _HH24:00_            | **14:00**                                                            |
| `$HOUR_OF_DAY`   | _YYYY-MM-DD HH24:00_ | **2023-10-20 14:00**                                                 |
| `$day_of_week`   | _WDAY D-Day_         | <p><strong>WDAY 1-Sun</strong></p><p><strong>WDAY 2-Mon</strong></p> |
| `$DAY_OF_WEEK`   |                      |                                                                      |
| `$day_of_month`  | _DD_                 | **20**                                                               |
| `$DAY_OF_MONTH`  | _YYYY-MM-DD_         |  **2023-10-20**                                                      |
| `$month_of_year` | _YYYY-MM_            | **2023-10**                                                          |
| `$MONTH_OF_YEAR` |                      |                                                                      |

## Operators:

* Arithmetic:`+, -, *, /, % (modulus)`
* Comparison: `=, >, >= or !<, <, <= or !>, <> or !=`
* Logical: `ALL, AND, ANY, BETWEEN, EXISTS, IN, LIKE, NOT, OR, SOME`



