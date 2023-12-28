# Budget Key Project

[![Discord](https://img.shields.io/discord/758246813781983252.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/S9JBJSE)

הי כולם!

ברוכים הבאים לגיטהאב של מפתח התקציב.

נעבור לאנגלית עכשיו

### BudgetKey Contributor Quickstart

The main channel for discussing issues with the community or with Adam (DM @akariv) is either Slack or Gitter (https://gitter.im/OpenBudget/BudgetKey)

A few links:
- The starting point on GitHub is [OpenBudget/BudgetKey](https://github.com/OpenBudget/Budgetkey).
  Here you can find a quick README, links to our 'ready to work on' issues, and some tutorials.
  In short, we have to main kinds of tasks:
  - Data Wrangling (i.e. getting new data sources into our DB)
  - Frontend Development (we have an Angular frontend that always needs some TLC)

  [See the project's board here](https://github.com/orgs/OpenBudget/projects/3)

  You can find more info in the rest of this README VVVV

- The pipelines dashboard is at https://pipelines.obudget.org - you can see the status of your favorite pipeline there :)

- Access to all of our data is free and easy:
  - Via SQL use the redash app at https://data.obudget.org (use any Google Account to login)
  - Via file download at https://next.obudget.org/datapackages

Enjoy and don't hesitate to ping me directly or in the channel!

Adam (@akariv)

## What is this about?

The Budget Key project aims at being the most up to date and comprehensive non-governmental database of Israeli fiscal data.

It has user-facing website (currently available at [obudget.org](http://obudget.org)), using multiple API servers which are running of meticulously collected data from various sources, manually improved and cleaned.

The data can also be queried using a relational query interface, available at [data.obudget.org](http://data.obudget.org) (use any Google account to log in after clicking 'Login with Google').

## Where to start?

- Find an interesting task to start with
  - Some of our starter issues are marked with a 'help wanted' label. 
    See here a [list of all these isssues](https://github.com/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+user%3AOpenBudget+label%3A%22help+wanted%22+label%3Aready+)
  - You can ping us on Slack (preferably), on gitter.im (https://gitter.im/OpenBudget/BudgetKey) or on Telegram (ask to join)
  - Consult with Adam, Noam or Mary to find the best match for you.
- Read a little about our [workflow](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/Workflow.md)
- See the HowTos below to understand our development environment.
- Read the documentation, if you need to dive deep in existing code

## HowTos

- [Front End Development](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/FrontEndDevelopment.md) (Angular2 / HTML / CSS / Node ... )
- [Data Wrangling](https://github.com/OpenBudget/budgetkey-data-pipelines/blob/master/README.md) (Python3)
- [Using the API](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/UsingTheAPI.md)
- [Fetching Raw Data](https://github.com/OpenBudget/BudgetKey/blob/master/documentation/RawData.md)

# Repositories

This is the main repository, where documentation should come.

Issues and pull requests should be made on the individual repositories:
 - Frontend apps: https://github.com/OpenBudget/budgetkey-app
 - Backend services:
   - APIs: https://github.com/OpenBudget/budgetkey-api
   - Nginx Frontend: https://github.com/OpenBudget/open-budget-nginx-frontend
   - ElasticSearch service: https://github.com/OpenBudget/open-budget-elasticsearch
 - Frontend Libraries:
   - A Specialized Sankey Component: https://github.com/OpenBudget/mushonkey
 - Data pipeline:
   - https://github.com/OpenBudget/budgetkey-data-pipelines
 - DevOps:
   - Kubernetes Infrastructure: https://github.com/OpenBudget/budgetkey-k8s
 - BudgetKey portals:
   - SocialMap: https://github.com/OpenBudget/socialmap-app-main-page
   - OpenProcure: https://github.com/OpenBudget/openprocure-app-main-page
