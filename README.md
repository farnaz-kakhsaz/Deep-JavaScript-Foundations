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

## Types

### Primitive Type:

Many of us have probably heard this assertion before:

> "In the JavaScript, everything is an object."
>
> (false)

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

Unlike languages like C++ and Java, in JavaScript and in other dynamically typed languages, variables do not that have types.  

> In JavaScript, variables don't have types, values do.

So let's refer to them as value types.

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

### typeof Operator

When we assign some value to a variable, and use operator like `typeof`, we're not asking what's the `typeof` the variable (for example v), we're asking what is the `typeof` the value that is currently in v.

```JavaScript
var v;
typeof v;              // "undefined"

v = "1";
typeof v;              // "string"

v = 2;
typeof v;              // "number"

v = true;
typeof v;              // "boolean"

v = {};
typeof v;              // "object"

v = Symbol();
typeof v;              // "symbol"
```

> We can think of `undefined` as basically a default value.

When there is no other value, the value that you have is called the `undefined` value. And remember, there's an `undefined` type with one and only one value in it called `undefined`. So typeof is telling us v currently is `undefined`. It got initialized and it's `undefined`. And that, by the way, does not mean it doesn't have a value yet.

> `undefined` means, does not currently have a value.

A lot of people think, `undefined` means doesn't have a value yet. The most appropriate way to think about it is, does not currently have a value. Because it's entirely plausible and possible, that you have a variable or a property that has some value and then it goes back to the state of not having a value anymore. You set it to `undefined`, you undefine it. It just takes it back to that state where the value that it's holding is `undefined`. 


> The `typeof` is an operator that guarantees that it will always return a string.

```JavaScript
typeof doesNotExist;        // "undefined"

var v = null;
typeof v;                   // "object"   OOPS!

var v = function() {};
typeof v;                   // "function"   hmmm?

var v = [1, 2, 3];
typeof v;                   // "object"   hmmm?

// coming soon!
var v = 42n;
// or: BigInt(42)
typeof v;                   // "bigint"
```

Why typeof null returns object? This is a historical fact from ES1 which essentially indicated to developers that if you wanted to unset a regular value, like a number, you would use undefined. But if you wanted to unset an object reference, you would use null.

Remember, function is not an official type at the top level, in the most official sense. But it has its own return value here, the typeof operator returns as function. So it's useful that it returns to us function, but arrays, it doesn't. Arrays just returns object.

The `Array.isArray()` will tell you for sure or not whether you have an array.

The following table summarizes the possible return values of `typeof` from [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#Description).

| Type                                                    | Result |
| ------------------------------------------------------- | ----------- |
| Undefined                                               | "undefined" |
| Null                                                    | "object"    |
| Boolean                                                 | "boolean"   |
| Number                                                  | "number"    |
| BigInt (new in ECMAScript 2020)                         | "bigint"    |
| String                                                  | "string"    |
| Symbol (new in ECMAScript 2015)                         | "symbol"    |
| Function object (implements [[Call]] in ECMA-262 terms) | "function"  |
| Any other object                                        | "object"    |

Some `typeof` examples from [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#Examples):

```JavaScript
// Numbers
typeof 37 === 'number';
typeof 3.14 === 'number';
typeof(42) === 'number';
typeof Math.LN2 === 'number';
typeof Infinity === 'number';
typeof NaN === 'number'; // Despite being "Not-A-Number"
typeof Number('1') === 'number';      // Number tries to parse things into numbers
typeof Number('shoe') === 'number';   // including values that cannot be type coerced to a number

typeof 42n === 'bigint';


// Strings
typeof '' === 'string';
typeof 'bla' === 'string';
typeof `template literal` === 'string';
typeof '1' === 'string'; // note that a number within a string is still typeof string
typeof (typeof 1) === 'string'; // typeof always returns a string
typeof String(1) === 'string'; // String converts anything into a string, safer than toString


// Booleans
typeof true === 'boolean';
typeof false === 'boolean';
typeof Boolean(1) === 'boolean'; // Boolean() will convert values based on if they're truthy or falsy
typeof !!(1) === 'boolean'; // two calls of the ! (logical NOT) operator are equivalent to Boolean()


// Symbols
typeof Symbol() === 'symbol'
typeof Symbol('foo') === 'symbol'
typeof Symbol.iterator === 'symbol'


// Undefined
typeof undefined === 'undefined';
typeof declaredButUndefinedVariable === 'undefined';
typeof undeclaredVariable === 'undefined'; 


// Objects
typeof {a: 1} === 'object';

// use Array.isArray or Object.prototype.toString.call
// to differentiate regular objects from arrays
typeof [1, 2, 4] === 'object';

typeof new Date() === 'object';
typeof /regex/ === 'object'; // See Regular expressions section for historical results


// The following are confusing, dangerous, and wasteful. Avoid them.
typeof new Boolean(true) === 'object'; 
typeof new Number(1) === 'object'; 
typeof new String('abc') === 'object';


// Functions
typeof function() {} === 'function';
typeof class C {} === 'function';
typeof Math.sin === 'function';
```
