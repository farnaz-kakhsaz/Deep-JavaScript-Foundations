# Deep JavaScript Foundations
> **Note:** I collected some important and deep JavaScript foundations. 
Most of this information comes from Kyle Simpson's books (You Don't Know JS series) or courses that I took.
It would help to have a deeper understanding of JavaScript.

## Table of Contents

1. [Introduction](##Introduction)

## Introduction

First of all, we should know that, when we face a bug in JavaScript, the first question we need to ask ourselves is what does the specification say should happen? And then ask does my behavior that I'm seeing in code match the specification. The JavaScript specification is the source of authority, not the JavaScript engine or MDN (Mozilla Developer Network).
> The JavaScript specification ( [ECMAScript](https://www.ecma-international.org/) ) is the source of authority.

When does the bug happen? When we have incorrect thinking or mental model. then it's a divergent to what we're thinking and what the computer's doing and that's when the bug enters the code.
> Whenever there is a divergence between what your brain thinks and what is actually happening in the computer, that's where bugs enter the code. --getify's law #17

JavaScript can be divided into three cores (pillars):

* Type:

    * `Primitive Types`
    * `Abstract Operations`
    * `Coercion`
    * `Equality`
    * `TypeScript, Flow, etc.`

* Scope:

    * `Nested Scope`
    * `Hoisting`
    * `Closure`
    * `Modules`

* Objects (Oriented):
    * `this`
    * `class {}`
    * `Prototypes`
    * `OO vs. OLOO`
