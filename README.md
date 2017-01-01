# Budget Key Frontend Development

## The Basics

In the past, we had one single repository for the main website. This turned out to be good at first, and a big problem as things went on. The need to understand the code and make sure that nothing broke when you changed it made 'getting into the code' and making any change harder and harder.

To fight this problem, we now take a different approach: we split the website to a set of a simple standalone 'apps'. All apps share a similar structure, and none is too complicated, so it's easy to understand the code.

Development is also kept easy, as you only need to check out the bit that you're working on - so that build and dependency management never get too big to handle.

We try to keep our apps compact and concise. Each app should have one simple purpose. If you see that things get too complicated, it's probably time to split to a few distinct apps, and make the common bits an external library.

- We use **Angular 2** as our framework of choice. Why Angular 2? Because it's good, it's simple, it's common and it has a future. 

  - Why not ReactJS / _< any-other-web-framework >_? 

    Because we want to have a common framework for all apps. We chose one and that's final.

- We keep styles (and the general look-and-feel) common across apps. We do that by encapsulating the structure and styles of the app in a separate component, [budgetkey-ng2-components](https://github.com/OpenBudget/budgetkey-ng2-components/). This component also imports Bootstrap 3 styles so you can freely use these styles without worrying.

## Quickstart

[This repository](https://github.com/OpenBudget/budgetkey-app-sample) is a template for a standard Budget Key app.

You can clone it (or better - use the latest [master download](https://github.com/OpenBudget/budgetkey-app-sample/archive/master.zip)), add some code and push it to a new repository called `budgetkey-app-something`.

After that, it's super easy to deploy it as a new app in the Budget Key (you just need to add a proper route in [our frontend](https://github.com/OpenBudget/open-budget-nginx-frontend)).

This repository is based on the TypeScript source code of the [angular.io quickstart](https://angular.io/docs/ts/latest/quickstart.html), with some modifications for deployment and standardisation.

## Where to find stuff

- `app/` holds your app code. You write in there in TypeScript. 
  - Component styles and templates are kept side by side with the code.
  - Create folders as necessary to prevent your code from becoming a steamy mess.
- `assets/` holds any static file you want to attach (images, fonts etc.). You should create folders per content type, e.g. `assets/img/`, `assets/fonts/` etc.
- `package.json` is where you define your dependencies and metadata about your app.

For most apps you wouldn't need to change anything else.

## How to work with this?

- `npm install` will install all necessary dependencies.
- `npm start` will fire up the development server and open a browser on `localhost:3000`. It's always watching your files, so you can simply edit and save and let the browser refresh itself.
- `npm run dist-serve` will build the bundle that will be used for serving on the server. You might need to run this if you want to debug anything, but usually not.

## Testing

_(still TBD, the following text is from the original quickstart project)_

The template app adds both karma/jasmine unit test and protractor end-to-end testing support.

These tools are configured for specific conventions described below.

*It is unwise and rarely possible to run the application, the unit tests, and the e2e tests at the same time.
We recommend that you shut down one before starting another.*

### Unit Tests
TypeScript unit-tests are usually in the `app` folder. Their filenames must end in `.spec`.

Look for the example `app/app.component.spec.ts`.
Add more `.spec.ts` files as you wish; we configured karma to find them.

Run it with `npm test`

That command first compiles the application, then simultaneously re-compiles and runs the karma test-runner.
Both the compiler and the karma watch for (different) file changes.

Shut it down manually with `Ctrl-C`.

Test-runner output appears in the terminal window.
We can update our app and our tests in real-time, keeping a weather eye on the console for broken tests.
Karma is occasionally confused and it is often necessary to shut down its browser or even shut the command down (`Ctrl-C`) and
restart it. No worries; it's pretty quick.

### End-to-end (E2E) Tests

E2E tests are in the `e2e` directory, side by side with the `app` folder.
Their filenames must end in `.e2e-spec.ts`.

Look for the example `e2e/app.e2e-spec.ts`.
Add more `.e2e-spec.js` files as you wish (although one usually suffices for small projects);
we configured protractor to find them.

Thereafter, run them with `npm run e2e`.

That command first compiles, then simultaneously starts the Http-Server at `localhost:8080`
and launches protractor.  

The pass/fail test results appear at the bottom of the terminal window.
A custom reporter (see `protractor.config.js`) generates a  `./_test-output/protractor-results.txt` file
which is easier to read; this file is excluded from source control.

Shut it down manually with `Ctrl-C`
