# Budget Key Project

[![Tasks Ready to be worked on](https://img.shields.io/waffle/label/OpenBudget/BudgetKey/ready.svg?style=flat-square)![Tasks Good for Newcomers](https://img.shields.io/waffle/label/OpenBudget/BudgetKey/help%20wanted.svg?style=flat-square)](http://waffle.io/OpenBudget/BudgetKey)

## What is this about?

The Budget Key project aims at being the most up to date and comprehensive non-governmental database of Israeli fiscal data.

It has user-facing website (currently available at [obudget.org]()), using multiple API servers which are running of maticulously collected data from various sources, manually improved and cleaned.

The data can also be queried using a relational query interface, available at [data.obudget.org]() (use any Google account to log in after clicking 'Login with Google').

## Where to start?

- Find an interesting task to start with
  - Some of our starter issues are marked with a 'help wanted' label. 
    See here a [list of all these isssues](https://github.com/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+user%3AOpenBudget+label%3A%22help+wanted%22+label%3Aready+)
  - You can ping us on Slack (preferrably), on gitter.im (https://gitter.im/OpenBudget) or on Telegram (ask to join)
  - Consult with Adam, Mushon, Saar or Mary to find the best match for you.
- Read a little about our [workflow](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/Workflow.md)
- See the HowTos below to understand our development environment.
- Read the documentation, if you need to dive deep in existing code

## HowTos

- [Front End Development](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/FrontEndDevelopment.md)
- [Data Wrangling](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/DataWrangling.md) _coming up!_

## Documentation of components

- [Data API Server](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/DataAPIServer.md) _coming up!_

- [Search API Server](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/SearchAPIServer.md) _coming up!_

- [Deployment](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/Deployment,md) _coming up!_

- [Ingestion Pipeline](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/IngestionPipeline.md) _coming up!_

# Repositories

This is the main repository, where documentation should come.

Issues and pull requests should be made on the individual repositories:
 - Frontend apps:
   - Search page: https://github.com/OpenBudget/budgetkey-app-search
   - Item page: https://github.com/OpenBudget/budgetkey-app-budget-item   
   - Main page: https://github.com/OpenBudget/budgetkey-app-main-page   
 - Backend services:
   - Search API: https://github.com/OpenBudget/open-budget-search-api
   - Data API: https://github.com/OpenBudget/open-budget-data-api
   - Nginx Frontend: https://github.com/OpenBudget/open-budget-nginx-frontend
   - ElasticSearch service: https://github.com/OpenBudget/open-budget-elasticsearch
 - Data pipeline:
   - https://github.com/OpenBudget/budgetkey-data-pipelines
