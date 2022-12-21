# How to use the BudgetKey API?

## Query API
Query the database with any SQL based query.

It has the following structure:

```
https://next.obudget.org/api/query?query=<SQL Query>
```

The parameters:
 - `SQL Query` - any query that you can run on http://data.obudget.org can also be run using the query API.

Note that this API will return at most 100 rows from each query (to prevent abuse etc.)

This API returns its results in JSON format:
```
{
  "rows": [
    {
      <column-name>: value
      ...
    },
    ...
  ],
  "total": <total-number-of-rows>
}
```


## Search API
Search API allows access to the entire recordset of BudgetKey.

We use the [apies](https://github.com/dataspot/apies) library to create the API, detailed information on the different parameters can be found there. The library's endpoint is locatated at 

```
https://next.obudget.org/search/<doc-type>
```


The main data types that we expose are:
   - `budget` (items from the national budget)
   - `entities` (all sorts of organizations)
   - `national-budget-changes` (monetary changes in the national budget)
   - `supports` (supports, subsidies, transfers of funds from the government to organizations)
   - `tenders` (information about tender process in which the government intends to buys stuff)
   - `contract-spending` (information about actual money transactions with actual suppliers)
   
(use `all` to fetch documents from all types)
