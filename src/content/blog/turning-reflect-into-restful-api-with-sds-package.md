---
title: "Composing SDSRest with other SDSRest to compose"
meta_title: ""
description: "A blog describes our refactoring journey when we turned our rest to be REST operational"
date: 2025-05-21T00:00:00Z
categories: ["rest", "sds", "reflect", "refactoring"]
author: "Medet Ahmetson"
tags: ["refactoring", "reflect", "exciting"]
draft: false
---

# Intro
Currently, there was only way to work with the [Reflect](https://github.com/ara-foundation/web/tree/main/packages/reflect) package:

```typescript
modules = await reflect.get('node_modules');
```

Reflect doesn't have other get options. It doesn't have any other operation to update the data.

Today's blog is about my journey how I implemented the advanced data manipulation [SDSRest](https://www.npmjs.com/package/@ara-web/sds). Instead, creating each manipulation method, I turned the Reflected ontology JSON into a `SDSRest` file format. The result? I can use CSS Selectors for the Reflect:

```typescript
// get all node modules
reflect.getAll!('.node_modules')
// find `type URL {}` in the website scripts
reflect.getAll!('type#URL') 
// update the `let varName = ''` value if its occurs in the first file.
reflect.put!('memop > module:nth-child(1) > var#navName', json)
// set actionButton as disabled.
reflect.patch!('#actionButton[disabled]', true)
```

The `Rest` is a module that creates a RESTFul wrapper for any JSON object. And allows to use CSS Selectors for querying the JSON objects.
If you have to manipulate JSON with the deeply nested objects, or without knowing exact structure, then REST might be helpful. And tell me did this blog post also helped you to understand is it worth to use SDSRest?

Earlier I already wrote my journey in the [How did I create a REST api for any JSON object](https://www.ara.foundation/blog/turning-code-piece-into-restful-api-with-sds-package)  article. I opened it on a browser, and follow it as a guide as my own documentation.

> I highly recommend to read the first article as here I will simply describe my throught processes without explaining what is REST and how to think in REST way.

Let's start from the steps to apply Rest API for our JSON.

# Creating ontology
Before coding, I have to design the ontological structure that Rest package requires.
In order to do that, I have to read the code and build a mental model of the Reflect, what will be in the REST and what's not.

Quite often, trying to read the code to have a compherensive architectural view is the hardest thing. So hard, that in the past, I would procrastinate. God damn, so hard to return back to the code you wrote few months ago and try to understand it.

I did it. So here is the plan. Reflect rest wrapper will be to all reflected website. Ontologically, each reflect's will be tagged as `memop`.

> memop = extension

Each file will be tagged as `module`.
And if the rest selector accesses to the Code Piece tags, then rest it will work with the code pieces.

Which means, if I will use the code peace object tags such as `var`, `class`, `type` then Rest understands we are asking code pieces.
If we are using `memop` then reflect should understand we are asking to return the extension.
If we are using `module` then reflect should understand we are asking to find a file.

Planning and writing yourself the architecture before coding also gives you a task list and their priorities. After all, knowing the architecture also allows to indetify the dependencies between the tasks.

Let's code!

Passed four hours
---

## Reflect Object Tree
Rest doesn't work with the JSON directly, instead it needs an intermediary data type that Rest operates with. This intermediary data type converts the JSON object into a parent-child related graph tree.

We don't create the object tree, instead create two methods that we pass to our Rest along with the JSON, and let the JSON to convert the data when its needed.

Those two methods are 
`ObjectToNodeTree`, `ElementOps` provided by `@ara-web/sds` package.

The defined reflect object is in `packages/src/reflect-object-tree.ts`.

Another three hours passed
---

## CSS Selector for Reflect
Creating a CSS Selector for the modules and module operators took me less few hours. The code itself took about twenty minutes, and most of the time I had to deal by updating the test files.

Next step is to integrate code pieces into the Reflect's rest, so that using `Reflect.rest.get('content selector')` I could fetch data from the reflect file content.

One day passed
---
But adding the Code Pieces (a.k.a file contents) took me few days, eventually discovering
over-engineered parts of Rest. The bug that didn't allow me to merge various rests together was hard to debug due to over-engineering. Eventually, updated the Rest as [@ara-web/sds@0.1.3](https://www.npmjs.com/package/@ara-web/sds/v/0.1.3).

After fixing the Object Tree Node creation, the Reflect's modules worked automatically.

I created the [reflect-object-tree](https://github.com/ara-foundation/web/blob/main/packages/reflect/src/reflect-object-tree.ts) which is creating the object tree from the reflected data.

The method `reflectElementToObjectTree` converts any reflected element (CodePiece, Module, Module Operator) into an
Object Node. 

> [Source Code on Github](https://github.com/ara-foundation/web/blob/fa1130f5cde820ad3a7d11081d6efa0c531d87ec/packages/reflect/src/reflect-object-tree.ts#L29)

The method `reflectElementOps` is a collection of methods that returns Object Node parameter for any JSON nodes, such as what is the id, tag, or its children.

> [Source Code on Github](https://github.com/ara-foundation/web/blob/fa1130f5cde820ad3a7d11081d6efa0c531d87ec/packages/reflect/src/reflect-object-tree.ts#L166)

Three days passed
---

## Replace Reflect.get with Reflect.rest
Once we had the reflect rest api, it's time to replace the 
Reflect `get` method with the `Reflect.rest`.

But Reflect is plug-and-play module. Reflect's `get` method calls the extension hooks such as `beforeGet` and `afterGet` (source on [github](https://github.com/ara-foundation/web/blob/fa1130f5cde820ad3a7d11081d6efa0c531d87ec/packages/reflect/src/extension-interface.ts#L106)).

Luckily, Rest module also implemented in SDS framework. So instead directly using Rest in Reflect, let's simply create a Rest Proxy that will call the Reflect hooks.

What we will do is, we will create `RestReflectHookProxy`.
Additionally, we will add all other rest related hooks before and after calling the rest operators.

It took more time, as I found out a bug in object duplication.
Simply I had to duplicate the object copy using `Object.create(obj)` instead using spread operator `{...obj, ...obj.hidedMethods}

Another time took to test that rest proxies are working properly.

Passed the three days. 
It's done, and tests are working [Test](https://github.com/ara-foundation/web/blob/f3d2424b5040caf8ec9191cf2d30d65c73fc2a1c/packages/reflect/test/6-reflect.test.ts#L37) files to see how it's implemented.

Today
---

Before releasing it, finally I upgraded the reflect extensions
to know what methods to export and what not. After seeing that reflect works correctly, I pushed the code to the github.

## What's next?
- Reflect Astro Extension's rest operator must include Astro's `page`, `component` and other astro website contents.
- Add a rest extension that writes back the updated ontology into the file or database data.

Then, reflect is ready to be used as a toolbar and an AI assistant. :)