---
title: "Hyperpayment protocol"
meta_title: ""
description: "A blog post that describes use-case of the apps"
date: 2024-04-17T00:00:00Z
categories: ["hyperpayment"]
author: "Medet Ahmetson"
tags: ["hyperpayment", "ara", "maydan"]
draft: false
---

# Hyperpayment protocol

- Keywords: Ara, Hyperpayment
- Status: early-version

The Hyperpayment protocol defines an agreement on how the payment occurred between multiple parties who don’t know each other.

Using the hyperpayment protocol create a specification for a specific domain. Then implement it.

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
Hyperpayment is created to define a decentralized payment system involving multiple parties, yet the participants don’t know each other without permission, affecting more than two parties. 

It will lead to the decentralization of the Internet.

It aims to:
- Be the internet’s payment protocol.
- Be minimalistic in terms of syntax.
- It should be trivial to make it understandable by humans and computers.
- Be expandable
- Reduce the risk of human error to as close to zero as possible.

The use cases for Hyperpayment include:
- Revenue of the open source library from the proprietary software.
- Data ownership of the user and receiving money if data analysts and AI model trainers used it.

## Specification
Hyperpayment defines a text format of how the particular parties in a domain on the internet interact. Each domain must specify the protocol that follows the hyperpayment protocol.
The protocol must specify the
- Category of the parties.
- Building that defines how participants are categorized into the categories and the hierarchy of the categories.
- The flow of money between categories is the vectors and splines. The first flow must be defined as the initial point of which category sends what. The flow section must describe the junction of the flow vectors.
- The source defines all the money. The money may be deposited, withdrawn, or transferred.
- The resources denoting the money shall be described as category and $ prefix. The flow must be based on the resources.
- Junctions must be defined using the hooks “before,” “after,” and “along.”
- Contract of each category that shall define how the money is received and how it is forwarded

The category must not have the keywords whose lemmas are:
“Ara,” “hyperpayment,” “user,” “category,” and “love.” 

This means, “usering,” “categories,” “categorize,” “ara,” “a-ra” must not be used.

The category must be defined as a single word.

Any hyperpayment protocol of any domain must 
be expandable. 

The resources that are going through the flow must be retained. The sum of all money that every user in all categories receives must equal the initial resource.

The expanded protocol 
- Must include the web link to the protocol that it expands in the intro. 
- The expanding protocol may add new categories. 
- The expanding protocol must not change the category list defined by the parent protocol.
The expanding protocol must not change the contract.

Here is the template that the protocols may follow:
- Title: Opensource
- Implements: Hyperpayment protocol (Link to this document)
- Authors: John Doe (to@example.com)
- Intro: the open source library; if it’s used in the proprietary software, then the user must also pay for the library.
- Categories:
    - Client
    - Dependency
    - Business
- Flow:
    - Resources: $client
    - Flow#1 - $client from Client to Business.
    - Flow#2 - $client from Client to Dependency.
- Junctions
    - Flow#2 is executed before the flow#1. The flow 2 receives the remaining amount from the flow 2.


- Contracts:
    - Client: is defined by the businesses. It’s the user who uses the client.
    - Business: is defined by the software developer.
    The business owner defines dependency by exposing the open-source code of their software.
Implementations and Further Reading

[Open-source hyperpayment specification](https://ara.foundation/blog/opensource-hyperpayment-specification).

[Ara hyperpayment specification](https://ara.foundation/blog/ara-hyperpayment-specification).

Todo: define the interface for the contracts and types of the contracts.


