---
title: "Opensource Hyperpayment specification"
meta_title: ""
description: "A blog post that describes use-case of the apps"
date: 2024-04-17T00:00:00Z
categories: ["hyperpayment", "opensource", "specification"]
author: "Medet Ahmetson"
tags: ["hyperpayment", "ara", "opensource"]
draft: false
---

# Opensource hyperpayment specification

- Keywords: Ara, Opensource, Hyperpayment
- Status: draft

This payment specification describes how much money the open-source libraries and programming language organizations will receive as revenue from internet businesses created using the tools above.

## License
Copyright (c) 2024 Medet Ahmetson, Sergey Pak, and contributors

This Specification is free software; you can redistribute it and modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License or (at your option) any later version.
This Specification is distributed in the hope that it will be useful but WITHOUT ANY WARRANTY, INCLUDING THE IMPLIED WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License and this program; if not, see http://www.gnu.org/licenses.

## Change Process
This Specification is a free and open standard governed by the Ara Foundation. Any changes must be approved by consensus with the main editor.
## Language
The keywords “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119 (see “Key words for use in RFCs to Indicate Requirement Levels").


## Goals
The modern open-source or freeware ecosystem isn’t economically sustainable, which leads to license changes to more strict sources. Recent examples are MongoDB's license change to a Server-Side public license and Unity's license change to charge more money, as described in Unity's [license change](https://gamerant.com/unity-licensing-fee-backlash-change/).

This specification defines the revenue model for open source software licensed under the BSD license family, such as “BSD,” “MIT,” or “Apache License,” or under the GPL, such as GNU GPL, AGPL, or Mozilla Public License.

Under this specification, the software maintainers don’t have to explicitly ask for the use of this specification for the payments. This means the current internet won’t have to change in order to adapt.

Indeed, this model is optional, regardless of whether the open-source libraries get or don’t get the money set up by the software that uses the open-source libraries.

The goal is to provide a version of the intro with examples of the use cases.

It aims to:
- Be optional and one of the sources of income.
- Paid by the users, not by the software developers.
- Paid to all dependencies, including dependencies of the dependencies.
- Incentives the open source economy to make open source compete with the proprietary software regarding the issue solving speed.

The use cases include:
- A marketplace where users want to use the software but don’t have money to pay; instead, users will pay for them.
- A blockchain cryptocurrency that implements this specification.


## Categories:
There are three categories of the software:

- **environment** category is used for the programming languages in which the software was written.
- **dep** category defines the open-source standalone software and libraries.
- **business** category defines the software.
- **customer** category defines the user of the software under the **business** category.

## Categorization
The following section describes how to list the users. Then, how to classify them.
- **customer** — Any user who uses the software for any task other than software development; thus, it’s automatically defined.
- **business** — Any software if the customer directly uses it. 
    - **business** — software shall be managed using Git or other similar version control systems. 
    - **business** — software shall be hosted on the repository platform.
    - **business** — it is recommended that it be a public repository.
- **environment** — the users of the **environment** category are defined using https://github.com/github-linguist/linguist/. The users under the **environment** category are the programming, configuration, template, and markup languages list.
- **environment** - the environment is exported as the JSON file using [Github Linguist](https://github.com/github-linguist/linguist/) and named the `environment.json` in the root folder of the business software repository.
- **dep** — is the package used for software development. For *node.js*, it is the packages listed in the `package.json` and fetched from the npm. For the *Go* language, the packages are listed in the `go.mod` file. For *Python*, the `requirements.txt` listed packages managed via *PyPI*.
- **dep** — list of users under the **dep** category must be fetched using the **business** software’s Software Bill of Materials (Bom). 
- **dep** — The dependencies must be listed as `bom.json` in the root directory of the business software repository. The bom files may be generated using either [CycloneDX](https://github.com/CycloneDX) or [SPDX](https://spdx.dev/). 
- **business** — for a private repository, the `environment.json` and `bom.json` must be defined in a public repository and linked from the business software.
- **dep** — the generated bom will list the dependencies as a tree that we categorize internally in the levels starting from 1. The primary dependencies imported by the business software are level 1. The dependencies that level 1 depends on are level 2. And so on.

Example of `environment.json` generated as https://github.com/github-linguist/linguist?tab=readme-ov-file#--json

This file must be stored in the repository’s root directory.:

```json
{  
"Toml":{
    "size":1212,
    "percentage": "0.5"
  },
  "Python":{
    "size":25999,
    "perc,e": "95.5"
  }
}
```

Example of bom.json for the project written in Python:
https://github.com/ara-foundation/blockchain-simulator/blob/deployment/bom.json


This Bom file is in the CycloneDX format. It was generated using the CylconeDX Python library.

## Flow
### Resources
- **$customer** is the money that is taken from the customer. 
- The **business** defines the amount of $ per customer.
- **$business** is the 20% of the $customer.
- **$environment** is the 0.5% of the **$business**.
- **$dep** depends on the level of the dependency.
### Splines
- **Flow 1** - **$customer** from **customer** to **business**
- **Flow 2** - **$environment** from **business** to **environment**
- **Flow 3** - **$business** from **business** to **dep**
- **Flow 4** - **$dep** from **dep** to **dep**

### Junctions
- Flow 3 comes before Flow 1. 
- Flow 2 comes after Flow 3. 
- Flow 4 comes before Flow 3.

## Contracts
Contracts define how the money is calculated for the category and the junctions. It also determines how the money is distributed within the category elements.

**customer** doesn’t have any contract, as only the receiving parties can define it.

**business** contract defines the contract by which it will receive the payments. Examples of business contracts are *“subscription,”* “one-time purchase,” “per-API call,” etc. The contract defines the amount per $customer. There must be only one element in the business category.

**Each environment has a fixed amount of money, defined as $environment**, distributed equally among all environments.

**dep** contract says that the first-level dependencies distribute all the **$business** money equally. If there is level 2 of the particular dependency at level 1, then the level 1 dependency gets 80%, while the remaining 20% is defined as **$dep**. 

The dependencies at level 2 share equally the **$dep**. If a dependency at level 2 has dependencies at level 3, then this particular dependency gets 80%, while the remaining 20% is turned into **$dep**.

## Implementations and Further Reading

[Ara hyperpayment specification](/blog/ara-hyperpayment-specification) is the extension of this specification for the blockchains.

