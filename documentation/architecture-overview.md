# Budgetkey Architecture Overview

Overivew of the different Budgetkey services and how they interact

## budgetkey-data-pipelines

Outputs:
* [elasticsearch pipeline](https://github.com/OpenBudget/budgetkey-data-pipelines/blob/master/datapackage_pipelines_budgetkey/pipelines/budgetkey/elasticsearch/budgetkey.source-spec.yaml)
  * see the generated pipeline [here](https://github.com/OpenBudget/budgetkey-data-pipelines/blob/master/datapackage_pipelines_budgetkey/generator.py#L35)
  * indexes data to Elasticsearch index `budgetkey`
  * doc-type is determined by the pipeline spec root keys, e.g. `budget` / `national-budget-changes` / `entities` ..
    * each type has different data, here are some example ids for each doc type:
    * `tenders` - `523515/office/2.2011`
    * `entities` - `512893587`
  * each document is also indexed to `document` doc type, with a different id, some example ids:
    * `supports/0001010102/2010/אחר/בקשת תמיכה לפרט`
    * `tenders/530919/office/9/2012`
    * `tenders/545694/office/מצפ / 11399478`
    * `org/company/512894387`

## open-budget-search-api

Backend / API, provides the following endpoints:
* [search](https://github.com/OpenBudget/open-budget-search-api/blob/master/open_budget_search_api/main.py#L12)
  * used by frontend search app
  * https://next.obudget.org/search/budget,national-budget-changes,contract-spending,entities/mic/1992-01-01/2019-01-01/10/0
* [get](https://github.com/OpenBudget/open-budget-search-api/blob/master/open_budget_search_api/main.py#L26)
  * used by frontend generic item page
  * `https://next.obudget.org/get/org/company/513390377`
  * `https://next.obudget.org/get/tenders/545694/office/מצפ / 11399478`
  
## budgetkey-app-search
  
Frontend search app, uses the open-budget-search-api endpoints. Example urls:
* `http://next.obudget.org/app/search/`
* `http://next.obudget.org/app/search/#/search?term=בדיקה`
  
## budgetkey-app-generic-item-page

Show item page for the documents in ES. Example urls:
* `http://next.obudget.org/i/org/company/513390377`
* `http://next.obudget.org/i/tenders/520462/office/123/2010`
