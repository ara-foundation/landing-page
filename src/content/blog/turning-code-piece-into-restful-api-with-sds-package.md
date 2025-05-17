---
title: "How did I create a REST api for any JSON object"
meta_title: ""
description: "A blog describes our refactoring when we turned our AST tree to be REST operational"
date: 2025-05-17T00:00:00Z
categories: ["rest", "sds", "reflect", "refactoring"]
author: "Medet Ahmetson"
tags: ["refactoring", "reflect", "exciting"]
draft: false
---

## Intro
Hey, there! I am refactoring my typescript library. I wanted to describe my journey on how I used `Rest` library to write elegant solution. I'm so excited that I wanted to share with others my solution, hoping it would help you as well. 
`Rest` library that I want integrate allows to create RESTFul api for any JSON by the CSS Selectors as a querying language.

If you have a large object or deeply nested JSON. CSS Selectors allows a powerful filtering operation to find right element at right nesting. But it's not about the library, but how it became elegant and saved me headaches in a long run.

Firstly, to give a context, let me describe a library that I am refactoring. A little bit about how the library works internally. I will explain the part used in this refactoring journey. Stay with me till the end, and I hope you will also appreciate by my choice of Rest as the elegant solution. And if you will be in the same situation as I am, hopefully Rest could help you as well. :)

> Rest module is open source and released already. It's available in the `@ara-web/sds` package in [npmjs](npmjs.com).

## Refactoring
The library that I am refactoring is called Reflect package. *Not released yet*. If you are a frontend developer, then with Reflect, you could change data or UI right in your browser console instead opening up the the code editor. 

> I created it, after getting tired by adjusting CSS. Also, if I had to change the text on the page, I had to go to my code editor, type `"Lorum ipsum.Nospace after dot"` to find the file. And all of it just to fix the typo. Reflect is the CMS, and Admin panel without being CMS and Admin panel, but only is a Nodejs package that works with any framework.

How it is working internally? Reflect creates Ontological data of the files. Ontological data is stored in the JSON format. Ontology simply means the files will be linked internally.

Reflect also supports the dynamic data. For example, if a web component parameter depends on a data fetched from the database. Reflect will fetch content from the database. Then, add the result into the JSON representation of the web component.

The dynamic data of the website are called `CodePiece`.
Code pieces are component scripts. For example, if a web component depends on variable `var userName = db.getUserId(session)`, then Code Piece will be the result of database request assigned to the user name.

Code pieces are used internally, and results are in the `Reflect` memory: [CodePieceMemory](https://github.com/ara-foundation/web/blob/aab7c4c744ed6748e3ca5942820cdf259d81d090/packages/reflect/src/code-piece-memory.ts#L10) memory.

Reflect Memory keeps key-value pair of the code pieces and their identifiers. Key-Value pairs are defined in [CodePieceRecord](https://github.com/ara-foundation/web/blob/aab7c4c744ed6748e3ca5942820cdf259d81d090/packages/reflect/src/code-level/code-piece.ts#L22) type.

As code pieces also have the types. Type might be a variable
declaration, a function call, a class or a type definition etc. The potential types of code pieces is defined in [CodePieceType](https://github.com/ara-foundation/web/blob/aab7c4c744ed6748e3ca5942820cdf259d81d090/packages/reflect/src/code-level/code-piece.ts#L6) enum.

But interaction with this key-value memory isn't flexible.
If we want to receive a code by the code piece type, or do 
they have certain attribute, then generating additional
interface adds more code lines.

Luckily, creating `Rest` module for collaboration website, I found that using it for memory is also useful.
If I will need a result from memory, I could use `CodePieceMemory.get`, `CodePieceMemory.post` and other rest operations. And filtering is done by the Rest.

## What do I need to do to use Rest in my code?

Rest doesn't work with JSON straight away. Rest works with the ontological object tree. So, we need to define ontological object node, and tree builder that can convert our JSON into ontological nodes.

Ontological means, instead IDs, we use links. 
If I say, this is ontological array, then array may be, elements of array, or size of array may be linked to another data within the project. That to understand the array, you need to find elements by the link. Basically, ontological array defines what it presents. Your source codes don't need it, as they have module importing rules provided by the package managers.
Databases also don't need it, since they have `foreign-keys`.
But if we want to have inter-related JSON that is agnostic of database, or file system which means we can share it, we can use it in anywhere. Then, we need it.

So, we the first step is to design the ontology of our code piece. Which means the links. To make it work with CSS Selectors, our links should be extending Object Links. 

> 1 First step in using Rest API for any JSON:
> **Design Linking rules for objects compatible with ObjectLink**

## 1. Ontology, code piece links as object links

Object Links are based on CSS Selectors. It's compatible with the CSS, so, any CSS Parser would understand object links as well.

**Valid Object Links**
```css
div#main-content        /* main-content element in the page */
a[href="google.com"]    /* all links to google.com          */
.btn-primary            /* all primary buttons              */
```

Object Links provide four ways to build selectors:
- ID
- Tag
- Class
- Attribute

We need to think, how do we design our JSON links, to match our data to object link's four selectors.

### 1.1 Defining link selectors to our JSON object.
In Reflect, Code pieces are already stored using identifiers. So, we will match it with id selector. 
If we have a variable `let userName`; Then we could
fetch it using `#userName` link.

Code Pieces have its own attributes such as `public`, `data`,
`dataType` etc. If we want to fetch all `const varName` variables, then we could fetch using `[constant]`,
if we want to fetch all `let varName`, then we could write `[constant!=true]`.

Our code pieces have a type. Let them be the tag of code piece. To return all variable declarations we could write `var`, to return all type declarations we could write `type`.

When it comes to classes, we don't need them.

### 1.2 Writing the code for Object Link creation
So let's create Object Node builder four our code pieces. What will be the Rest operations work with? With the code pieces in a single memory. So, each module have it's own object tree from code pieces.

```typescript
import { OkResult } from "@ara-web/p-hintjens";
import { type ObjectToNodeTree, type ElementOp, ObjectNode, DOCUMENT_SELECTOR } from "@ara-web/sds";

export const moduleToObjectTree: ObjectToNodeTree<CodePiece> = (): ObjectNode<CodePiece> => {
    // Creating the root for entire source code that has one or many code pieces.
    const doc = new ObjectNode<CodePiece>(codePieceOps);
    return doc;
}
```

The `codePieceOps` is the helpful translators, that in request by Object Node, will return respectful CSS Selector.

Here is the declaration:

```typescript
export const codePieceOps: ElementOp<CodePiece> = {
	getName: getCodePieceName,
	getChildren: getCodePieceChildren,
	getAttribute: getCodePieceAttribute,
	setAttribute: setCodePieceAttribute,
}
```

Code Piece Operations define four functions that CSS Selectors use for every object node.
Before starting to write code pieces, we already designed how we will define the css selectors.

We could solve if one condition.

The element operator that returns ID selector of the `CodePiece`:

```typescript
const getCodePieceName = (_element?: CodePiece): string => {
	if (_element === undefined) {
		return DOCUMENT_SELECTOR;
	}
    return _element.identifier!;
}
```

And other code piece operations:
```typescript
const getCodePieceChildren = (el: CodePiece): CodePiece[] => {
	return el.getAllMemoryData();
}

const getCodePieceAttribute = (_element: CodePiece | undefined, attrName: string): string | undefined => {
	if (_element === undefined) {
		return undefined;
	}
    if (attrName in _element) {
        return (_element as any)[attrName]?.toString();

    }
	return undefined;
}

const setCodePieceAttribute = <AttrType>(_element: CodePiece | undefined, attrName: string, attrValue: AttrType): OkResult => {
	if (_element === undefined) {
		return OkResult.ok();
	}
	if (attrName in _element) {
		(_element as any)[attrName] = attrValue;
		return OkResult.ok();
	} else {
		return OkResult.fail(`The ${_element.identifier} has no attributes`, `Can not set ${attrName} to non attributal element`)
	}
}
```

Voila. Our `code-piece-object-tree.ts` is ready, how do we set the CodePieceTree?
So, how do we use it and replace our previous code?

### 1.3 Updating code to use REST
Let me check who is depending on the `CodePieceMemory`. What I did
is basically removed the code piece and turned it into a REST node
based on `CodePiece`.

> Offtopic:
> Luckily, I also follow the SDS rule, which makes CodePieceMemory local file and doesn't have to worry that removing it will break entire code.

The CodePieceMemory is part of `ModuleMemory` that along with code pieces keeps
it's source code, file path, and other attributes.

What we need to do now is to update `ModuleMemory` to remove CodePiece and use the REST.

---
> 1 day later
---

I did replace `class ModuleMemory<T> extends CodePieceMemory`
with the simple `ModuleMemory<T>` with the `rest()` method returning `Rest<CodePiece>`.

Now, I can simply fetch data from module memory using `memory.rest.get!('#varName') to fetch the result of the variable.

Check out the test files on [GitHub](https://github.com/ara-foundation/web/blob/27216bc54fa2cdd8764051d6da720cba30c79b4e/packages/reflect/test/type-declaration.test.ts#L37).