# Budget Key Frontend Development

## The Basics

All our frontend is in the OpenBudget/budgetkey-app repository.

- We use **Angular** (currently on version 16) as our framework of choice. Why Angular? Because it's good, it's simple, it's common and it's maintained. 

  - Why not ReactJS / _< any-other-web-framework >_? 

    Because we want to have a common framework for all apps. We chose one and that's final.

## Quickstart

To install the app:
- Clone it using git from the relevant repo (there's a list in the main README page)
- run `npm install`
- run `ng serve`

The last command will build, start a webserver for local development and watch the files for changes.
This is handy for development, as you can simply edit, save and refresh page in browser.

All apps were created using Angular's CLI (`ng new ...`), so directory structure is pretty similar.
You can find most stuff under `project/budgetkey/src/app/`.

## How to work with this?

Angular's CLI tool `ng` is useful for generating new components, services etc.
e.g. `ng generate component my-new-component` (or `ng g c my-new-component` in short).

## Styling the app

Usually styles are part of the components, so avoid using global styles (there are some useful macros and constants in `common.less`).
