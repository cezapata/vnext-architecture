# V-Next Portal strategy

## Objective

This document lays out an opinionated vision on how to set up a development environment that leverages the latest technologies for front-end development in a successful manner. The goal with this is to establish a pattern that will unblock developers, increase their productivity, and allow them to focus on delivering high value with minimal effort.

## Tl;Dr;

1. Try to implement as little as possible. There are patterns and [guidelines](https://epicreact.dev/) to set up a successful Milestone Zero codebase. Use the resources as much as possible.
2. Reuse the tools that have been battle tested and work for the community (e.g. Eslint, Typescript, Cypress, React, Storybook) rather than building your own. Do thorough research before jumping into one, but don't be afraid to change as they evolve over time (Tslint was good to have but was eventually deprecated in favor of Eslint, and that is ok).
3. Realize the value of tests, and write them not just because you have to but because you need to.
4. House rules and patterns specific to a project should be enforced through tooling. Do not let PRs devolve into discussions about the form of a product. Agree on a rule, and automate it for catching errors or formatting it.
5. Strive to have a frictionless development environment. Set up for a new hire should not be complicated, and should boil down to two steps: pulling source code from a repo, and running a script to set up the runtime.


## Development environment

Having a uniform / unified development environment is key for a team to operate successfully. Standardizing on the tools used is one way to remove friction across the board. My first suggestion is to use [Visual Studio Code](https://code.visualstudio.com/) as a standard IDE for the team. These are a few reasons why:

1. It allows us to standardize on scripts, tools, and plugins to make our lives easier.
2. It runs on all major Operating Systems, and even runs well on the browser.
3. It has a very good integration with both ADO and GitHub, as it is all owned by Microsoft.
4. It has a very healthy ecosystem of extensions, which amplify our productivity.

As far as extensions go, rely on the [recommendations](https://tattoocoder.com/recommending-vscode-extensions-within-your-open-source-projects/) engine built into VS Code. These recommendations may be unique depending on the project, but having an easy way to deploy the recommendations is a must.

Among the recommended extensions I would always suggest:

1. Use [prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) for code formatting.
2. Use a code scaffolding tool such as [this one](https://marketplace.visualstudio.com/items?itemName=Huuums.vscode-fast-folder-structure).
3. Keep a clean consistent set of scripts for running the projects consistently: npm run { start | build | test | setup }

## Repository structure

Try to keep all the project code within one place (like a single ADO account), and potentially in one repo, if possible. Monorepos tend to work well for big teams like MS Teams, and there are tools that allow you to work successfully within them:
- [Lerna](https://lerna.js.org/)
- [Yarn workspaces](https://yarnpkg.com/features/workspaces)
- [Rush](https://rushjs.io/)

Having your code split into multiple packages allows you for reusability. It requires some diligence in updating dependencies, but also allows us to make commits to multiple entities within the scope of a single PR.

One consideration is to use a well-defined commit log structure for your commits. Especially with monorepos, it would be useful to have a pattern where it is easy to identify what package you are targeting. Keep in mind that there are also tools that can create and manage Changelog files and automate them based on your commit history. This is more relevant for library maintainers but you may find it valuable to use the feature for the team.

In terms of merge strategy, [gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) has worked well for the team in the past. Having dedicated branches to trigger our CI / CD workloads allows for on-calls to easily pick up and manage ongoing deployment trains successfully.

## CI / CD

One suggestion here would be to use YAML files to carry out your configuration alongside the code. This applies on whether you are on Azure Pipelines, or using GitHub actions. This will give some portability to your code, at least in terms of creating the build and release configurations (you may still need to install plugins / extensions to make it work if you are migrating accounts).

Ideally, there should be a dedicated infra team that can handle deployments for the full team. Build issues tend to be complex and sap away a lot of time for developers to work on. The issues tend to repeat on multiple projects as well. Having a dedicated team also ensures you can provide more time and effort in following best practices, being compliant, and spending time creating automation to make the developer lives easier, plus it encapsulates developers from having to delve into the innards of the pipes (e.g. debugging a web app, learning about certificate expiration, or figuring out ACLing permissions on a Key Vault). This would be one break from the model we have where each team has traditionally maintained their services vertically, and where there is typically one or two key people that are subject-matter experts and fix all the issues.

Packaging the web application and creating a container image  has worked well in the past and provides an easy way to perform roll backs and keep track of the different versions of the code. It also allows us to port the code and make it agnostic of where it is run. Deployment takes seconds, and allows us to standardize on the processes it uses in order to build. The main problem we have with this approach is that it differs from the same code path we execute when we are working on a local debugging environment (in which case we traditionally run a web server locally on top of Node.js).

## JAMstack

JAMstack stands for Javascript, APIs, and Markup. Applications that run in this model expect all the code to be packaged into Markup and scripts that get loaded and executed at runtime. It results in very portable code, that can be run from very simple web applications. There are very popular static site generators like [Hugo](https://gohugo.io/) and [Gatsby](https://www.gatsbyjs.com/) that are excellent for running content operating on the JAMstack. What it would take for a team to implement this is ensuring that access to all the APIs is handled throught the client app (via XHRs), and have a mechanism to perform authentication from the client app's perspective, which MSAL allows us to do.

One huge advantage of having this approach is that now we can push our JS bundles generated from a build into a CDN. This will make downloading our assets faster, allow for caching, and optimized for reusability (i.e. one asset bundle that did not change won't change its hash identifier and therefore will remain valid for further use). This solution also allows us to handle versioning of the portal and even load different versions at runtime, thus making it possible for a single web application to host multiple versions of the portal (we could even control these by using a query parameter). This approach is widely used by other teams within Microsoft such as MS Teams, and Outlook WebApp.

Onel last benefit about keeping our code aligned with the JAMstack is portability. Code that we write in this way can be moved around and integrated into new products. We could potentially use the same build processes (especially if we align on a build team that provides a consistent build tooling experience) that will allow us to collaborate and produce the same output.

## Testing

Testing is key for the success of a product, but it can produce burnout and negative experiences on developers that do it badly and / or do not understand the need for it. There are a few rules that we can follow to try to make testing as good as possible:

1. Unit tests should serve to test functionality that we use as a black box. For instance, testing a library API makes a lot of sense to test in isolation to cover all the use cases that the library offers. Hence the `.spec` moniker that we use, which is supposed to output a set of rules or spec that an API is supposed to complete in order to ensure that it fulfills its goals. Because unit tests are meant to be tested as a unit, you must ensure that external dependencies are mocked when writing for these types of tests.

2. Integration tests (or functional tests), are for testing areas of the product as a user would. These tests are meant to cover the use of a product or feature the way a customer would normally use it. If we are testing a wizard that aims to build a configuration file and in the end submit it to an API, then we can write an integration test that tests for the happy path of this. These types of tests would go through several steps, and in the end would perform an action that can be compared against a baseline. Mocking here is applied to the criteria of the test author, but my personal bias is to mock external API calls (to ensure we hit the proper paths according to a step), while keeping internal references to the UI and other components intact. This will give us coverage, and make our tests as close to how a user performs the test as possible. When writing these types of tests, think about the scenario that you are running, and not about how the implementation details are specified. Assuming that we have selected the right scenarios to cover the usage of the product will give us the coverage numbers that we also crave. Alternatively, if there are very convoluted steps to get to 100% coverage, we may question whether or not we need to handle those cases as a special test.

3. E2E tests shouls always run on a real web server and against real APIs. These can be used for gating the system, and determining its overall health. They need to be rock-solid, and we should not use these as a kitchen sync to determine validity of the tests (we have the other tiers of tests to do that). At this point, I would recommend the team to use a tried and true system such as Cypress to run the E2E test suite, because:

    - It has broad support from the community, with questions answered in forums and enough users to find answers to common problems.
    - A good solid support for plugins that allow us to perform non-trivial tasks like Visual Regression tests
    - A standard set of steps and recipes to perform common tasks like authorization, or connecting data reports / output to CI/CD systems

4. Enforce code coverage if the team is not ready to embrace tests. Once the team has learned about its value and performs good testing strategies, think about relaxing the rules or even removing them. Threshold values to tests are arbitrary and in some cases devolve into developers trying to force unnatural test cases for the sake of coverage numbers. It is better to promote an environment where we value the tests and add them because they are needed.

5. Lastly, tests become better over time. Whenever you uncover a bug, add test coverage to prevent it from happening again. Little by little, your test suite will improve and become very robust.

## APIs

First and foremost, I would double-down on the use of GraphQL as our mechanism for ingesting data. It allows us to query for multiple sources at one shot. It also serves as a unifier in terms of the expectations for a backend. Adding new resources under it gives us a consistent level of quality in our APIs, like:

- Having a consistent format for handling data, getting values, interacting with it, performing CRUD, or even for parsing errors.
- It allows us to have a mechanism that will give us push notifications via subscriptions
- It provides an endpoint that allows us to discover new endpoints, and provides documentation on its underlying documents.
- Unifies the way in which we can handle our Auth-Z model, centralizing it and creating one more layer of abstraction to benefit from.
