# How to use the BudgetKey API?

# SEARCH API
Search API allows access to the entire recordset of BudgetKey.

The search endpoint has the following structure:

```
https://next.obudget.org/search/<kinds>/<search-term>/<from-date>/<to-date>/<page-size>/<offset>
```

The parameters:
 - `kinds` - a comma separated list of document types or the word `all` (for all document types).
   Currently we have in the database the following kinds:
   - `budget` (items from the national budget)
   - `entities` (all sorts of organizations)
   - `national-budget-changes` (monetary changes in the national budget)
   - `supports` (supports, subsidies, transfers of funds from the government to organizations)
   - `tenders` (information about tender process in which the government intends to buys stuff)
   - `contract-spending` (information about actual money transactions with actual suppliers)
 - `search-term` - whatever it is that you're searching
 - `from-date` - when searching with a date range, the lower limit of the range in the format `YYYY-MM-DD`. 
   You can pass `2000-01-01` if you don't care about the range.
 - `to-date` - when searching with a date range, the upper limit of the range in the format `YYYY-MM-DD`. 
   You can pass `2030-01-01` if you don't care about the range.
 - `page-size` - how many results to return. Usually 10 or 20 work well.
 - `offset` - if you're browsing the results, just increment the offset for getting results further down the list.
 
 This API returns its results in JSON format.
 ```
 {
  "search_counts": {
    "<kind>": {
      "total_overall": <total-documents-for-kind>
    },
    "<another-kind>": {
      "month_counts": {
        "YYYY-MM": <number-of-documents-for-this-month>,
        ...
      },
      "total_overall": <total-documents-for-all-kinds>
    }
  },
  "search_results": [
    {
      "score": 207539.89,
      "source": { <the-result> },
      },
      "type": "kind"
    },
    ...
  ]
}
```

Note that text fields in the search results might contain 'highlights' - parts of the text where the search term matched.
These highlights are marked with enclosing `<em>` tags.
