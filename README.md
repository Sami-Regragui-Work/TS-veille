# TypeScript — Veille Technologique

> 💻 Live coding repository: [ts-livecoding](https://github.com/Sami-Regragui-Work/ts-livecoding)

A presentation built as a single HTML file, covering what TypeScript is, how it compares to JavaScript, its key features, and where you'll encounter it in the wild.

---

## 🌐 Accessing the Presentation

The presentation is hosted on GitHub Pages and can be accessed directly at:

https://sami-regragui-work.github.io/TS-veille/


To navigate, use the **arrow keys** (`←` / `→`), or the buttons at the bottom of the screen. On slides that reveal content step by step, each press of `→` shows the next element. Press `Space` to skip directly to the next slide, and `Enter` to reveal all hidden content on the current slide at once.

---

## 📖 Concepts Explained

### What is TypeScript?

TypeScript is a programming language created by **Microsoft** in 2012, designed by Anders Hejlsberg — the same engineer who created C# and Turbo Pascal. The core idea is simple: take JavaScript, and add a layer on top of it that lets you describe _what kind of data_ your variables, functions, and objects are supposed to work with. That description is called a **type system**.

The important thing to understand is that TypeScript doesn't replace JavaScript — it **extends** it. This is what "superset" means: every JavaScript file you've ever written is already valid TypeScript. You're not learning a new language from scratch; you're adding vocabulary to one you already know.

---

### Superset

When we say TypeScript is a _superset_ of JavaScript, think of it like a Venn diagram where the TypeScript circle completely contains the JavaScript circle. Anything JS can do, TS can do too — but TS has additional capabilities that plain JS doesn't. A superset relationship means **no valid JavaScript is left behind** when you switch to TypeScript. You can even rename a `.js` file to `.ts` and it'll work immediately, then gradually add types at your own pace.

---

### Static Typing vs Dynamic Typing

This is the most fundamental concept in the entire presentation, and it's worth understanding deeply.

**Dynamic typing** (JavaScript's approach) means that a variable's type is determined _at the moment the code runs_. You don't declare upfront whether a variable holds a number or a string — JavaScript figures it out on the fly. This is flexible and fast to write, but it means certain mistakes only reveal themselves when a user actually triggers that code path in a live application.

**Static typing** (TypeScript's approach) means that types are checked _before the code ever runs_, at the moment you write and save the file. You tell TypeScript what type each variable or function parameter is supposed to hold, and the compiler verifies that you respect those rules everywhere. Think of it like proofreading a document before publishing, versus discovering the typo after thousands of people have already read it.

The word "static" here refers to the fact that types are fixed and checked at rest — before anything is moving, before any user has clicked anything.

---

### Compile Time vs Runtime

These two terms describe _when_ something happens in the life of your code.

**Runtime** is when the code is actually executing — when a user opens your app in a browser and starts interacting with it. JavaScript errors that only appear at runtime are dangerous because they require a real user, doing a specific thing, at a specific moment, to even be discovered.

**Compile time** is the step that happens _before_ runtime, when your source code is translated into something the browser can run. TypeScript adds a compilation step where it reads your code, checks all the types, and either flags errors or produces clean JavaScript. If there's a type error, TypeScript stops you here — before the code ever reaches a user.

This distinction is why TypeScript is genuinely valuable in production: it moves a whole category of bugs from "discovered in production by a frustrated user" to "caught in your editor while you're still writing the code."

---

### The `tsc` Compiler

`tsc` stands for **TypeScript Compiler**. It's a command-line tool that does two things in sequence: first it checks your types, and if everything is valid, it transforms your `.ts` files into plain `.js` files that a browser or Node.js runtime can execute. If there are type errors, it tells you exactly where they are and refuses to proceed.

The compilation pipeline shown in the presentation — `.ts file → tsc → .js file → 🌐 Browser` — is the complete lifecycle of TypeScript code. By the time the browser sees anything, all the TypeScript-specific syntax has been stripped away. The browser never knows TypeScript was involved; it just sees regular JavaScript.

---

### Basic Types

TypeScript comes with a set of built-in primitive types that map directly to the kinds of values you already work with in JavaScript:

`string` covers any text value like `"Alice"` or `"hello world"`. `number` covers any numeric value whether integer or decimal (`25`, `3.14`). `boolean` is simply `true` or `false`. `number[]` (which can also be written as `Array<number>`) describes an array where every element must be a number.

The syntax to annotate a variable is a colon after the name, followed by the type: `let age: number = 25`. This tells TypeScript — _this variable is meant to hold a number, and if I ever try to assign a string to it, stop me immediately._ The annotation is your contract with the compiler.

---

### Interfaces

An interface is TypeScript's way of describing the **shape of an object** — what properties it should have, and what type each property holds. Think of it as a blueprint or a contract. You define the interface once, and then any object that claims to follow that interface must satisfy all of its requirements.

The `?` after a property name (like `email?: string`) marks it as **optional** — the object is considered valid whether or not that field is present. This maps to how real data actually behaves: not every user record has an email, not every post has a subtitle.

One important detail: interfaces don't generate any JavaScript output. They exist purely at compile time to help TypeScript verify your logic. When the compiler is done, interfaces disappear entirely from the final `.js` file — they're a development tool, not a runtime feature.

---

### Enums

An enum (short for _enumeration_) lets you define a **named set of constants**. Instead of scattering magic values like `"UP"`, `"DOWN"`, `"LEFT"`, `"RIGHT"` or `0`, `1`, `2`, `3` across your entire codebase, you group them into a single named type and reference them by name: `Direction.Up`, `Direction.Left`, and so on.

This solves a real practical problem: if you mistype `"Upp"` somewhere in JavaScript, nothing warns you until your app behaves strangely at runtime. But if you write `Direction.Upp` in TypeScript, the compiler immediately tells you that value doesn't exist in the `Direction` enum. It also makes code self-documenting — anyone reading `Direction.Left` understands exactly what it means without needing a comment to explain it.

---

### Generics

Generics are one of the more powerful — and initially confusing — features of TypeScript, so let's approach them carefully.

The problem they solve is this: what if you want to write a function that works with _any_ type, but you still want type safety? Without generics, your options are bad: either write a separate version of the function for each type (lots of repetition), or accept `any` as the type (which effectively turns off TypeScript's checks for that value entirely).

Generics solve this with a _type parameter_, written as `<T>`. The `T` is a placeholder — a variable for a type — that TypeScript fills in automatically based on how you actually call the function. When you call `first([1, 2, 3])`, TypeScript sees that the array contains numbers, substitutes `T = number`, and knows the return value will be a `number`. When you call `first(["a", "b"])`, `T` becomes `string`. Same function, different types, full safety in both cases.

A good mental model: generics are like a recipe that works with any ingredient, while still guaranteeing that what goes in and what comes out are always the same kind of thing.

---

### Utility Types

Utility types are a set of built-in TypeScript helpers that let you _transform_ existing types into new ones without rewriting them from scratch. They're generic types that TypeScript ships with, and they cover patterns that come up constantly in real projects.

`Partial<T>` takes a type and makes every one of its fields optional. This is useful when representing an object that's only partially filled in — like a form a user hasn't finished yet, or a partial update payload in an API call.

`Required<T>` does the exact opposite: it takes a type where some fields might be optional and forces them all to be present. You'd use this when you need to guarantee a fully complete object after validation.

`Pick<T, Keys>` lets you create a new type containing only a specific subset of fields from an existing type. If you only need `name` and `email` from a full `User` type for a particular component, `Pick<User, "name" | "email">` creates that slimmed-down type without you having to define it manually.

`Omit<T, Keys>` is the inverse — it gives you a type with certain fields excluded. `Omit<User, "age">` produces a User type where the age field simply doesn't exist.

What makes utility types genuinely useful is that they stay _in sync automatically_. If you add a field to `User`, every `Pick` or `Omit` type derived from it adjusts without any extra work. This is a big deal in large codebases where types evolve constantly.

---

### Where to Expect TypeScript — The Frameworks

Understanding where TypeScript shows up by default helps you anticipate when you'll need it professionally, often before you consciously decide to "learn TypeScript."

**Angular** is the most notable case — Google's framework has required TypeScript since version 2 in 2016. There is no "JavaScript mode" in Angular. If you join an Angular project, you are writing TypeScript whether you planned to or not. This is actually what most of you have already experienced firsthand.

**NestJS** is a Node.js backend framework built entirely in TypeScript, inspired by Angular's architecture. Writing a NestJS API means dealing with decorators, interfaces, and strong typing throughout — TypeScript is inescapable here.

**Next.js** (the popular React meta-framework by Vercel) has shipped with TypeScript support by default since version 13. When you scaffold a new Next.js project today, it generates `.tsx` files and a `tsconfig.json` out of the box. You can opt out, but most teams don't.

**Vite** is a build tool rather than a framework, but it matters because most modern frontend projects use it. When you run `npm create vite@latest`, TypeScript is offered as a first-class template option for React, Vue, Svelte, and others.

**Nuxt 3** (the Vue meta-framework) was rebuilt from the ground up as TypeScript-first. Its internals are written in TypeScript and it provides end-to-end type safety throughout the entire stack.

The pattern across all of these is consistent: any framework significantly updated or created in the past five years has either adopted TypeScript by default or made it the obvious recommended path. Knowing JavaScript is the foundation, but TypeScript is increasingly the professional baseline you'll encounter in real-world codebases.

---

## 📚 Further Reading

- [typescriptlang.org](https://www.typescriptlang.org) — the official docs, which also include an excellent interactive playground to experiment in the browser without any setup
