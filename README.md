# Deep JavaScript Foundations
> **Note:** I collected some important and deep JavaScript foundations. 
Most of this information comes from Kyle Simpson's books (You Don't Know JS series) or courses that I took. 
It would help to have a deeper understanding of JavaScript.

## Table of Contents

1. [Introduction](#Introduction)
2. [Types](#Types)

## Introduction

First of all, we should know that, when we face a bug in JavaScript, the first question we need to ask ourselves is what does the specification say should happen? and then ask does my behavior that I'm seeing in code match the specification. The JavaScript specification is the source of authority, not the JavaScript engine or MDN (Mozilla Developer Network).

> The JavaScript specification ( [ECMAScript](https://www.ecma-international.org/) ) is the source of authority.

When does the bug happen? 
when we have incorrect thinking or mental model. Then it's a divergent to what we're thinking and what the computer's doing and that's when the bug enters the code.

> Whenever there is a divergence between what your brain thinks and what is actually happening in the computer, that's where bugs enter the code. 
>
> --getify's law #17

JavaScript can be divided into three cores (pillars):

* Types:

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
    * `class{}`
    * `Prototypes`
    * `OO vs. OLOO`

## Types:

Many of us have probably heard this assertion before:

> "In the JavaScript, everything is an object."

There is a reason for this statement, but this statement is a misconception, this is false. 
The reason behind why people say everything is an object, is because most of the values in JavaScript can behave as objects. But that does not make them objects.

Let's take a look at the JavaScript specification:

> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are Undefined, Null, Boolean, String, Symbol, Number, and Object. An ECMAScript language value is a value that is characterized by an ECMAScript language type.
>
> [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-ecmascript-language-types)

> The latest ECMAScript standard defines eight data types:
>
> * Seven data types that are primitives:
>    * `Boolean`
>    * `Null`
>    * `Undefined`
>    * `Number`
>    * `String`
>    * `Symbol` (added in ES6)
>    * `BigInt` (future)
> * and `Object`
>    * `Funcion`
>    * `Array`
>
> [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Data_types)


But what about functions and arrays? functions and arrays are subtype of the object type. 

> In JavaScript, variables don't have types, values do.

> In JavaScript, a primitive (primitive value, primitive data type) is data that is not an object and has no methods. There are 7 primitive data types: string, number, bigint, boolean, null, undefined, and symbol.
>
> Most of the time, a primitive value is represented directly at the lowest level of the language implementation.
>
> All primitives are immutable, i.e., they cannot be altered. It is important not to confuse a primitive itself with a variable assigned a primitive value. The variable may be reassigned a new value, but the existing value can not be changed in the ways that objects, arrays, and functions can be altered.
> 
> [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)

> ECMAScript is object-based: basic language and host facilities are provided by objects, and an ECMAScript program is a cluster of communicating objects. In ECMAScript, an object is a collection of zero or more properties each with attributes that determine how each property can be used—for example, when the Writable attribute for a property is set to **false**, any attempt by executed ECMAScript code to assign a different value to the property fails. Properties are containers that hold other objects, primitive values, or functions. A primitive value is a member of one of the following built-in types: **Undefined**, **Null**, **Boolean**, **Number**, **String**, and **Symbol**; an object is a member of the built-in type **Object**; and a function is a callable object. A function that is associated with an object via a property is called a method.
>
> [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-ecmascript-overview)