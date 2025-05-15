---
title: "Ara Roadmap"
meta_title: "Ara roadmap"
description: "A roadmap of Ara's journey!"
draft: false
---

Our development progress and the current state of Ara's development pipeline are highlighted on this page. Each project on the list may be utilized for a purpose so not to be considered as an Ara component.

#### SDS

> Status: Released v0.0.1

[SDS](https://github.com/ara-foundation/web/blob/main/packages/p-hintjens/SDS.md) is a  library to write modular and scalable apps. It aimed to build apps fast
without worrying about technical debt in the future. SDS rules aims
to focus on the project, while keeping it modular with minimal architectural planning.

###### Use case
- If you want to create Plug-n-play apps
- If you want to allow people build plugins for your programs

###### Implementations
- Typescript module implementation `@ara-web/p-hintjens/sds`.
- An Eslint plugin as `@ara-web/reflect-eslint-proxy`

###### Whats needed
- Expand SDS's functionality.
- Increase the number of implementations for more programming languages.
- Include additional linting plugins

#### Reflect

> Status: Released v.0.01, in development towards v.1.0.0

[Reflect](https://github.com/ara-foundation/web/tree/main/packages/reflect) is a NodeJS package that creates ontological data of your website. In basic words, the ontological data is interrelated JSON reflecting website's files structure and code.
Reflect strives for RESTful website operations. Its goal is to transform browser-based developer tools into a coding environment, particularly powerful if combined with AI agents.

###### Use case
- Enable admin panels to edit the websites directly on browser.
- Generate UI components right on your browser using AI agents (e.g. to add advanced CSS for animation)
- Look at the website's models in the developer tools.

###### Implementations
- Typescript module implementation `@ara-web/reflect`.
- Reflect Astro Framework based apps `@ara-web/reflect-astro-ext`

###### Whats needed
- Add converting JSON into a file
- Add more frameworks
- Add more programming languages
- Use Atomic Data.

> Want to help improving Reflect? Join our Telegram channel.

#### Links module

> Status: Released v.1.0.0 (undocumented)

Instead ID for any data or a file, we provide Links modules that
turns the objects addressable across web using links.

The links have not yet been documented. To find out when it's launched, follow our [Telegram News Channel](t.me/arafoundation).

#### Soyer AI

> Status: Planned, not released

An AI agent tool called Soyer keeps track on your computer activities.
It gives you additional customization options by connecting to the services and gaining access to the web apps' ontologies.

##### Soyer AI Pattern matching

> Status: Delayed, not released

The labeled data produced by everyone is called a world pattern. 
 AI might use it to offer better tools that other people are already using.

##### Whats needed
- Implementation (probably in ten years).

#### Pak Unity

> Status: In development (release in Summer 2025)

Pak Unity is an online collaboration platform where all project's data is stored in a single place as a JSON file.
It's modular, allowing users to create various access to a certain object for strangers.

###### Use cases
- If you want to work on a project with the strangers but need a strong social contract. Pak Unity allows authorization using tokens.
- If you want to hire freelancers, then instead the rating, we use a token deposit mechanism to avoid malicious freelancers from adversary behaviour.

#### Implemented
- Lungta &ndash; a development model that includes from ideation to project release.

#### Ara Network
A blockchain based payment network that runs on the [Hyperpayment protocol](https://docs.google.com/document/d/1d3TgpcZGwnmzO3SXthvqX_hG5cji6Yj3oZUMvrwDm08/edit?usp=sharing).
It tracks all software's SBOM (Software bills of materials) and distributes fees to all open-source contributors.

##### Implemented
- Hyperpayment specification designed
- Ara Blockchain's [whitepaper](https://docs.google.com/document/d/1aNMg13er5YQGj7EzKcgLmshhfuB1t5D34a58aNPPbAk/edit?usp=sharing)

##### Whats needed
- Coding and release.

---

Would you like to join us? Join our Telegram group and say "hi" to us! :)