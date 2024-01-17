---
title: "JavaScript Notes"
date: 2023-12-08
---

Notes while I learn to be fleshed out better later.

# Pluralsight
JavaScript is a **Dynamic Programming Language** meaning it is not strongly typed (TypeScript is used because of this)

`var varName = 0` or `let varName = "string"`

JavaScript is an **Interpreted Language** meaning it is not compiled, but each instruction is interpreted by the browser or interpreter

JavaScript was created as a scripting language, but has grown beyond that, nevertheless *script* is still in the name.

## Browser JavaScript Engines
These run JavaScript in the browser.

### V8
* Chrome
* Edge

### JavaScriptCore
* Safari (a.k.a. SquirrelFish)

### SpiderMonkey
* FireFox

## Web App Frameworks
* React (Meta)
* Angular (Google)
* Vue.js (Open Source)
* Svelte (Open Source)

JSX: an extension for JavaScript, most commonly used in React, to write HTML, CSS, and JavaSript using JavaScript.

## Serverside JavaScript
Node.js started it all. It executes JavaScript code inside the V8 engine and provides an I/O library to do things you can't in the browser environment.

Node Package Managers: all use the npm registry to find useful libraries enabling magical things to happen. (owned by GitHub (owned by MS))
* npm: Node Package Manager
* yarn
* pnpm

## Next.js (A full-stack framework for JavaScript)
This combines front end and back end in the same project.

## Desktop Applications using JavaScript
* Electron (for Mac, Windows, and Linux)

## Mobile Applications using JavaScipt
Not as pretty as the Swift (iOS) or Kotlin, Java for Android, but work.
* React Native that will work on many OSes with little modification.
* Cordova (Apache) - HTML, CSS, JavaScript targeting multiple platforms with one code base.

## IoT (Internet of Things)
Tons of devices use JavaScript.

## Cloud Service Providers (AWS, GCP, and Azure) have SDKs available for JavaScript

## JavaScript Versioning
ECMA standardized JavaScript, so all versions now have a ECMA version number.

Node.js has its own governance as well.

JavaScript versioning does not work like other semantic versioning systems.  
Each release continues to support older versions.
* ES1 .. ES4 initial versions with slow implementation by browsers.
* ES5 Again slow implementation by browsers.
* ES6 (ES2015) - The modern JavaScript that addressed many complaints of developers.
* ES7 (Async / Await)

Now there a smaller incremental releases on a yearly basis.

caniuse.com - will tell you which browsers will run what versions of JavaScript (ECMAScript)  
There are similar sites for node.js.

You would need to wait to use the new JavaScript features until the browser or framework you use has implemented it in the interpreter, if it weren't for extensions.

## Extending JavaScript
Native support is going to be better, but until they support our code we write our own.

Polyfill: a piece of code, used to provide modern functionality on older browsers that do not support it.

Transpiler: translates source code another equivalent source code. (TypeScript uses the Babel transpiler)

## Development Environment
1. Install GIT
2. Install Node.js
3. Visual Studio Code
