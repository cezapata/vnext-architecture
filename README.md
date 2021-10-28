# Portal Architecture

## Objective
To define a mechanism that allows us to deliver a great developer experience for portal development.

## Development environment

- VS Code
  - Suggested Extensions
- ESLint
  - define consistent set of rules
  - treat all warnings as errors
- Husky
- Prettier
- Integration with cloud-based environment (think of Web-based sandbox ready to hack)
  - Ability to switch branches
  - Give us a runtime to see code in execution on the browser
  - Live sync / pair-programming sessions
  - Save / history tracking / fork changes
  - Ability to install / unininstall / manage external packages and dependencies
  - Ability to run tests

## PRs
- Ownership
  - Add directory/file owners for all the codebase
  - Set up policies for commiting code
  - Set oup a website that allows you to easily see when / if you need to review code
- Bots
  - RenovateBOT
  - BOT for clarification / action items on common PR blunders
  - BOT for entry chunk size
  - BOT for letting you know on outstanding PR feedback

## DevOps
- ADO
  - Better reporting
- YAML pipelines
- Parallelization
- Error reporting
- Artifacts
  - Packages
  - Feeds

## Frameworks
- React
- react-i18next
- Apollo client

## Hosting
- Docker
- Docker-compose
- JAM-stack
- CDN-hosted web app
- NGINX
- Getting rid of razzle

## Server-side rendering
- Next.js
- Suspense API, React concurrent mode

## Workers
- Web workers
- Service workers (if we want a PWA)


## Accessibility

- Accessibility Insights
- Accessibility linters (static analysis)
- Accessibility runs on functional tests
- Accessibility runs on E2E tests

## Testing


### Utilities
- Shared test utilities (problem area)
- Mocking (when needed)
  - Use Jest mocking properly (automock, mock packages, spies)
  - Mocking APIs
  - Apollo mocking (just a separate callout, may not need it)
- Guidelines on how to write proper tests
  - Writing DRYer tests
  - No conditionals on your tests
  - Writing useful tests (that give you coverage and tell you what is wrong/broken)
  - How to organize tests

### Concepts
- Unit tests
- Functional tests
- E2E tests
- Visual regression testing (Chromatic)
- Performance testing
- Lighthouse

### Tools
- Jest
- testing-library
- Cypress / Playwright
- Applitools

## Telemetry
- Scenario-based telemetry
- BI / Interaction telemetry
- Operational telemetry (e.g. performance, availability)

## Portability
- React portals << requires more inv to see if it can save us pain in the future
- Web components
- Monorepos (Lerna, Rush, Yarn workspaces)
- Micro-frontends
- Shared-context (e.g. feature-flags, storage)

## State management
- Context API
- react-query
- Apollo Client

## Design systems
- Fluent UI
- Figma
- Storybook

## Utilities
- Date time utilities (kill Moment.js and replace it with ??? ask Matthew)
- Lodash (specify how to use it properly)
- Codemods for autofixers / tools for maintaining the code
