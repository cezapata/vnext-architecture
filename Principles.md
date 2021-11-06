# Principles

The following are a set of observations and opinions I have gathered through experience that I believe should help in general towards building better software products.

## Remove friction from devs as much as possible

In software development, there is a lot of time and energy spent working on tasks that are repetitive and require very little effort to achieve. Examples of these are:

- Setting boilerplate code when working on new features
- Arguing about formatting or presentation issues on a PR
- Catching minor issues in the code on PRs that could have been caught by automation, such as static analysis (i.e. linters)

Spending the time to automate very common workflows will result in a net positive time saving in the long run, by freeing developers from focusing on those issues and instead giving them time to concentrate on solving the problem they are actually working on. There is a balancing act here, where we need to take into consideration the cost of automation over the projected time savings.

The following are a few guidelines you can use to consider whether or not automating a workflow is valuable for the team:

- Consider the size of the team and the impact of the automation. If you have a huge team that would use the automation, then the value it provides is multiplied.
- Consider how common or rare a use case you are trying to automate will be. Optimizing for cases that will be hit all the time (such as linting on committed files) makes much more sense than optimizing for very rare scenarios (like writing automation to scaffold the creation of new projects).
- Consider how hard implementing the automation will be, e.g. adding [eslint](https://eslint.org/) with [pre-established rulesets](https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/src/configs/eslint-recommended.ts) and auto-fixers is cheaper than having to define custom rules and creating custom fixers for a project.
- Consider the burden of maintenance. Anything that you build will need to be maintained over time, and it has a lifecycle of its own. This code may have bugs, may incurr in dependencies, may go stale during its lifecycle, and will surely become obsolete in a matter of time (e.g. [TSLint](https://palantir.github.io/tslint/), [Enzyme](https://github.com/enzymejs/enzyme), [AngularJS](https://angularjs.org/)). Plan for its lifecycle, and consider its value over time. Even if automation will eventually rot and be replaced, it may provide a lot of value while it lasts.

## Build as little as possible to accomplish value proposition

Focus on your value proposition. What is it that you actually need to bring value to customers? In many cases, we chase for the new shiny thing. We have had projects in the past where we tried to boil the ocean, trying to implement a lot of things without thinking whether or not they provide value to our customers. It is totally fine to think outside the box, and try to go for "building a car, not a faster horse", but these decisions should be based on data, and trying to solve actual customer needs. Furthermore, we should always evaluate the value of an idea and the potential benefits it will bring us in order to determine how to prioritize it. Following the [Pareto Principle](https://medium.com/pm101/how-you-can-apply-the-80-20-rule-in-your-life-and-work-7d094a78e136) should allow us to maximize the impact that we have on our customers while providing benefits to our development teams:

- Engineers should have better clarity about what they are building. Reducing the scope of their deliverables to targeted features will make them understand the product better, focus on things that matter, and allow them to see the value and improve the product on key areas.
- As we are targeting more customers by targeting on areas that provide higher value proposition, we will have both happier customers that find a much more useful product being delivered today, and happier developers that will feel that their work is fulfilling, and making a difference in the world.

Do not re-invent the wheel. Ensure that your team is focused on delivering the value they promised. Borrow and reuse tools that were made to make your life easier. Do you really need to create a chart library to render visuals, or can you borrow [one that does the job](https://www.highcharts.com/)? How many data grid implementations does the world really need?  Microsoft already has a team that is building and unifying on a [Design System](https://www.microsoft.com/design/fluent/#/). While it can be tempting to implement components on our own to achieve a goal, we should think about the costs that bringing in these components will be:

- Maintenance costs over the long term (similar to the automation detailed above). Code that you build is code that we will maintain over the long term. If you publish it as part of a library, then you make it even stickier as you are responsible for other consumers that decided to adopt it.
- Thinking about the complexity of bringing in custom controls, and considering whether we have the knwoledge to implement them. Sure, implementing a drop down with type-ahead search is not very expensive, but thinking about how it can be reused, themed, and implementing the proper accessibility patterns, roles, and guidelines to make it a solid component is **very** expensive.
- Use common solutions when they are good enough. As an example, most modern browsers have implemented solid date and time pickers that are native to the browser. For developers to use an input field with an appropriate type attribute is incredibly cheap. More often than not, though, I see designs where fancy widgets to select the time are drawn, because they serve as good eye candy. These widgets often fail in many aspects, like not holding up in different devices or screen sizes, not being accessible, and even having bugs in fetching or presenting data.

## Be conscious on the dependencies that you bring into the product

This may sound as a contradiction on the previous topic, but it is important to clarify the difference. Any dependency that we bring into the product brings baggage alongside it:

- It may bring bugs into the code.
- It may introduce vulnerabilities, either on its own or through the use of other dependencies it relies on.
- It implies a level of trust with an external developer. Accounts can get hacked, and malicious code can be run in its place.
- Each dependency carries its own life cycle and expiration date on it. We need to plan ahead and find a replacement or an alternative in case the dependency gets sunset.
- Each dependency bloats the size of the code. This results in longer installs, and ultimately impacts in our bundle sizes (unless we create processes to make this more efficient like adding tree-shaking, lazy loading, and adding steps to decouple the code).

These are all the drawbacks we should consider before incurring on a dependency to a new tool or process. Once this has been considered, then we should definitely try and use the dependencies that will still work for us. I would never imply that we should build our own static analysis validator when eslint is the right tool for this. Neither should I imply that we should create our own transpiler if babel or the Typescript compiler do their job.

We should, however, we wary of libraries that [bring in very little value but that are used by many developers](https://qz.com/646467/how-one-programmer-broke-the-internet-by-deleting-a-tiny-piece-of-code/) in many packages. When selecting a dependency, always think about the level of support you may get from its maintainers. A dependency coming from [us](https://github.com/microsoft) has its benefits, as we can talk to the maintainers directly. Using open source solutions that are backed by big corporations (e.g. [React](https://reactjs.org/)) also provides a level of assurance as these are used by millions of users and have a dedicated team that works on solving issues. Be aware about the drawbacks before deciding on bringing in a new dependency.

## Build iteratively, from low cost to most expensive

Developing a product is costly. Minimizing the cost of development is of utmost importance. Elimintating needless iterations is an art form that starts when designing a solution. Refining an idea can be done very cheaply on paper, and even more so at a brainstorming session. Sometimes, it is essential to see a product in order to get a better feel for it. At this stage, relying on design concepts sketched by artists and designers can give stakeholders the visual cues and insights that they need to make a decision.

Prototypes don't need to be fully functional. They just need to convey enough information to test a concept. Using a sandboxing environment like [CodeSandbox](https://www.codesandbox.io) saves time and allows for quick development and sharing of ideas. Calls to services can be mocked, allowing us to figure out the proper contracts without having to wait for the services to be available.

Ensure that commitments to develop a feature are well-founded. Implementing and re-implementing a feature is costly and saps the team's morale. Being strategic about how we work on features to reduce the chances that we end up re-implementing the code will allow us to deliver more features, and pay more attention to detail on the ones that we commit to deliver.

## Build thinking into the future; do not over-engineer

As engineers, we may want to build the biggest, boldest thing. Filling a product with features may feel like the best course of action to get customers excited. Yet, bringing the Pareto principle over again, we can soon realize that for all the functionality we think we need to add to a product, only a small subset is used by the majority of our users.

These are some negative aspects of implementing every feature we can think of on a product:

- Adding a feature means maintaining it for the long haul. It is harder to take away something once it has been given freely. Therefore, providing functionality to the world, even if it is half-baked and labeled as experimental, may commit us to delivering on it.
- Adding features adds cognitive load to your product. Users will need to learn about the features to use it properly. This also results in more complicated UX, making the product harder to use.
- Having more code makes the portal slower. There are more chunks we will need to load, more JS we will need to parse, and more external dependencies we may need to add.

Over-engineering is not only specific to adding new functionality. It also impacts the design and decisions we make on the software that we *have* to build. In many cases, we need to think hard about the requirements given to us. More senior engineers are prone to fall on the trap of thinking that we should always abstract away complexity and create solutions that apply to each and every situation. This idea is sound in principle. In practice, we need to consider that these decisions could increase the complexity of the software that we build, will take more time to implement, and may not even be applied broadly. What we **can** do is build software that is extensible. These are some of the tools in the toolbelt to ensure we can build code that will be easy to extend and work with:

- Build code that is high cohesion and is loosely coupled. High cohesion means that all its relevant parts fit together. For instance, if building a library that allows us to handle mathematical operations, putting all the APIs together, concentrating the references, dependencies and APIs in one place makes sense and allows us to understand how it is used. Making it loosely coupled allows us to plug into different parts of the codebase easily, even making it easy to migrate from one place to others.

- When writing APIs, version them. Strive to introduce as few breaking changes as possible. Following [semantic verioning](https://www.semver.org) provides a contract that developers understand, and plays well with the expectations NPM puts on its packages.

- Build to an interface, not an implementation. This means we should not write an API that is modeled about how things are done, but rather modeled on what things do. Having a sort function can be understood and used, whereas using a bubble_sort already implies knowledge about how something is built. Implementation may change in the future, but coding to an interface allows us to change the innards of an API while leaving the contract intact.

- Write tests for the code that you write. For APIs and helpers, unit tests are perfect, allowing you to test the contracts you expose to your functionality in isolation. For features, writing functional tests (call them scenario tests, integration tests, or whatever you want to call them) gives you the best bang for your buck. These tests will allow you to test a feature the way users would experience it. They also allow developers to figure out which scenarios are broken whenever the tests fail. Having tests allow developers to know whenever regressions occur, and provide them with the confidence to refactor or work on new functionality without feeling that they are walking on broken eggshells, speeding up development time.

## Optimize for your best resource: the team

Software Engineers are expensive. Their time is worth much more than hardware. Having them waste their time while waiting on tasks to complete is much more expensive on the long run than fitting them with the right tools to do their job. Skimping on training is also a bad way of managing them or their time, and may impact in their mental well-being. Creating an environment where they can thrive and their value can be maximized is of utmost importance. Understand what their motivations are, and how can they do their best work. Consider the following:

- Some developers work better at dealing with ambiguity. Have them crack problems and suggest solutions. Keep them engaged at what they are best at.
- Some developers have a hard time figuring out solutions but can soldier on and provide time, effort and muscle to power through things once they have a mental model as to how to accomplish a task. Pairing them with the developers that crack problems will allow them to build synergy and produce a better product.
- Understand motivations and align them with your team-members career paths and ambitions. Engage them in tasks that will allow them to continue growing according to the path that best suits them.