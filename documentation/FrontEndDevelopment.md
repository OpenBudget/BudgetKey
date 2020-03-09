# Budget Key Frontend Development

## The Basics

In the past, we had one single repository for the main website. This turned out to be good at first, and a big problem as things went on. The need to understand the code and make sure that nothing broke when you changed it made 'getting into the code' and making any change harder and harder.

To fight this problem, we now take a different approach: we split the website to a set of a simple standalone 'apps'. All apps share a similar structure, and none is too complicated, so it's easy to understand the code.

Development is also kept easy, as you only need to check out the bit that you're working on - so that build and dependency management never get too big to handle.

We try to keep our apps compact and concise. Each app should have one simple purpose. If you see that things get too complicated, it's probably time to split to a few distinct apps, and make the common bits an external library.

- We use **Angular** (currently on version 7) as our framework of choice. Why Angular? Because it's good, it's simple, it's common and it's maintained. 

  - Why not ReactJS / _< any-other-web-framework >_? 

    Because we want to have a common framework for all apps. We chose one and that's final.

- We keep styles (and the general look-and-feel) common across apps. We do that by encapsulating the structure and styles of the app in a separate component, [budgetkey-ng2-components](https://github.com/OpenBudget/budgetkey-ng2-components/). This component also imports Bootstrap 3 styles so you can freely use these styles without worrying.

## Quickstart

All our repositories share the same installation process and directory structure.

To install the app:
- Clone it using git from the relevant repo (there's a list in the main README page)
- run `npm install`
- run `npm run dist-serve`

The last command will build, start a webserver for local development and watch the files for changes.
This is handy for development, as you can simply edit, save and refresh page in browser.

All apps were created using Angular's CLI (`ng new ...`), so directory structure is pretty similar.
You can find most stuff under `src/app/`.

## How to work with this?

Angular's CLI tool `ng` is useful for generating new components, services etc.
e.g. `ng generate component my-new-component` (or `ng g c my-new-component` in short).

## Styling the app

Usually styles are part of the components, so try to avoid using global styles (there's some old code doing that, don't follow on its example).

In order to get the app the same look and feel as other BudgetKey apps, we wrap all of our markup in a `<budgetkey-container>…</budgetkey-container>` pair.

Doing this adds the common header and footer, as well as import Bootstrap 3 CSS framework.

For that we import `BudgetKeyCommonModule` in the main `NgModule` declaration. It's already included in all existing apps so this is more of an FYI :) .

