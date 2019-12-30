# Deep JavaScript Foundations
> **Note:** I collected some important and deep JavaScript foundations. 
Most of this information comes from Kyle Simpson's books (You Don't Know JS series), MDN (Mozilla Developer Network) or courses that I took. 
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

> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are `Undefined`, `Null`, `Boolean`, `String`, `Symbol`, `Number`, and `Object`. An ECMAScript language value is a value that is characterized by an ECMAScript language type.
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

> In JavaScript, a primitive (primitive value, primitive data type) is data that is not an object and has no methods. There are 7 primitive data types: `string`, `number`, `bigint`, `boolean`, `null`, `undefined`, and `symbol`.
>
> Most of the time, a primitive value is represented directly at the lowest level of the language implementation.
>
> All primitives are immutable, i.e., they cannot be altered. It is important not to confuse a primitive itself with a variable assigned a primitive value. The variable may be reassigned a new value, but the existing value can not be changed in the ways that objects, arrays, and functions can be altered.
> 
> [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)

> ECMAScript is object-based: basic language and host facilities are provided by objects, and an ECMAScript program is a cluster of communicating objects. In ECMAScript, an object is a collection of zero or more properties each with attributes that determine how each property can be used—for example, when the Writable attribute for a property is set to **false**, any attempt by executed ECMAScript code to assign a different value to the property fails. Properties are containers that hold other objects, primitive values, or functions. A primitive value is a member of one of the following built-in types: **Undefined**, **Null**, **Boolean**, **Number**, **String**, and **Symbol**; an object is a member of the built-in type **Object**; and a function is a callable object. A function that is associated with an object via a property is called a method.
>
> [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-ecmascript-overview)

### typeof Operator:

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

> The `undefined` means, does not currently have a value.

A lot of people think, `undefined` means doesn't have a value yet. The most appropriate way to think about it is, does not currently have a value. Because it's entirely plausible and possible, that you have a variable or a property that has some value and then it goes back to the state of not having a value anymore. You set it to `undefined`, you undefine it. It just takes it back to that state where the value that it's holding is `undefined`. 


> The `typeof` is an operator that guarantees that it will always return a `string`.

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

Why `typeof` null returns object? This is a historical fact from ES1 which essentially indicated to developers that if you wanted to unset a regular value, like a number, you would use undefined. But if you wanted to unset an object reference, you would use null.

Remember, function is not an official type at the top level, in the most official sense. But it has its own return value here, the typeof operator returns as function. So it's useful that it returns to us function, but arrays, it doesn't. Arrays just returns object.

> The `Array.isArray()` will tell you for sure or not whether you have an array.

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

### `undefined` vs. `undeclared` vs. `uninitialized`:

This is often confused between `undefined` and `undeclared` concept. And it's confused because developers, quite naturally, think of these words as sort of synonyms. They seem like they mean kind of almost the same thing. And in English, that's probably true. But in programming, it's entirely not true. In programming, and especially in JavaScript, these are two entirely different concepts.

> `undefined` and `undeclared` in JavaScript are two entirely different concepts.

When we said type of V, and we got back quote, `undefined`, V or whatever that variable is, didn't even exist. So how is it that I can get back quote `undefined` when something doesn't even exist? Well, that's another historical wart, JavaScript trying to pretend as if the absence of a declaration isn't that big a deal. It's not that big of a problem, you can work around it. In retrospect, they never should have done that. They should have just returned a string `undeclared`. 

> The `undeclared` means it's never been created in any scope that we have access to.
>
> The `undefined` means there's definitely a variable, and at the moment, it has no value.
>
> the `uninitialized` or TDZ (Temporal Dead Zone) was introduced with ES6, and the best way to describe this is uninitialized. 

> The `typeof` operator is the only operator in existence that is able to reference a thing that doesn't exist and not throw an error.

The idea for `uninitialized` is that certain variables, never initially get set to undefined. When something is in an uninitialized state, it is off-limits. You're not allowed to touch it in any way, shape or form, or you'll get an error, and the error you get is the TDZ error.

We can have a variable that's never been initialized. We can have a variable that's been initialized that is undefined. Or we can have a variable that was never even created, and then it's undeclared. Three different concepts that we need to wrap our brains around.

### Special Values:

#### NaN:

The `NaN` is supposedly an acronym for not a number. Essentially `NaN` doesn't mean not a number essentially it means this special what we call sentinel value that indicates an invalid number. That's a much better mental model for it than referring to it is not a number, we should refer to at as an invalid number, 

> The `NaN` is an invalid number.

```JavaScript
var myAge = Number("0o46");       // 38
var myNextAge = Number("39");     // 39
var myCatsAge = Number("n/a");    // NaN
myAge - "my son's age";           // NaN

myCatsAge === myCatsAge;          // false   OOPS!

isNaN(myAge);                     // false
isNaN(myCatsAge);                 // true
isNaN("my son's age");            // true    OOPS!

Number.isNaN(myCatsAge);          // true
Number.isNaN("my son's age");     // false
```

It's not appropriate in any way shape or form to think of 0 as being the place holder for absence of numeric value. Doesn't make sense mathematically and doesn't make sense programatically.

> The `number` 0 is not the way you indicate the absence of valid numeric value.

There's nothing that you can ever do (mathematically) that includes a `NaN` and results in anything other than a `NaN`. So `NaN` sort of propagates all the way out. 

> The `NaN` with any other mathematical operation is always `NaN`, cuz it's invalid.

The `NaN` is the only value in existence, at least in JavaScript, that does not have what we call the identity property, meaning it is not equal to itself.

> The `NaN` is the only value in JavaScript, that is not equal to itself. Even when we use the triple equals operator ( === ).

The reason that two `NaN` are not equal two each other, is because [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) specification said `NaN`s are not equal to each other. So in this case the triple equals operator lies to us.

So we need some way to determine if the value is in fact `NaN`. Because triple equals lies to us.

And JavaScript shipped originally with a utility called `isNaN`, which when we pass a thing like a number to it we correctly get false. And when we pass a thing that actually, legitimately is `NaN`, we get `true`. Seems great until we pass in something that is definitely not a number, it's the `string` and we get `true`. The historical reason for getting true, is because for some reason, the isNaN utility coerces values to numbers before it checks for them to be `NaN`. So, it's gonna coerce the `string` to a number and guess what number it's gonna coerce it to? The `NaN` value, so of course it's gonna back `true`. And because it's gonna coerce the `string` to a number and guess what number it's gonna coerce it to? The `NaN` value, so of course it's gonna back `true` (in `isNaN("my son's age")` example). 

> The `isNaN()` function determines whether a value is `NaN` or not. But for The historical reason the `isNaN` utility coerces values to numbers before it checks for them to be `NaN`.
>
> The `Number.isNaN()` method (ES6) tells us defiantly for sure  the passed value, is the `NaN` value or it's not.
>
> **NOTE:** This function differs from the global `isNaN` function in that it does not convert its argument to a Number before determining whether it is `NaN`.

So now we can answer that, what is `NaN`? Well, if we do a numeric operation and I get back a value what type am I expecting back from every single numeric operation? I'm expecting a number, right? And remember I said `NaN`s are part of the IEEE 754 spec which is a numeric representation specification.

So perplexingly, the type of a `NaN` is number. It's just an invalid number. Which is why it's not appropriate to think of it as not a number because then you get into that weirdness the wording of type of not a number is number? That's because it's wrong to think of it as not a number, it's better to think of it as an invalid number.

> The type of a `NaN` is number. It's just an invalid number.

And of course the type of and invalid number is definitely number. I have people suggest to me no no no they should have never done `NaN`, nevermind what I EEE 754 said. They never should have done `NaN`, we should have returned something else. Really? What should we return from a numeric operation that's gonna be not of the type number?

### Negative Zero:

If you ask a mathematician, they'll say that's made up, that doesn't exist, that's not a real thing. It definitely exists in programming, it definitely exists in [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754). Just like nan exists in [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754), but a mathematician doesn't know what nan means. Then it definitely also requires, there to be a negative zero.

> The `-0` (Negative Zero) is essentially the zero value, but with the sign bit on. So it is the negative representation of a zero.

Now, early days of JavaScript felt that JavaScript developers would never want a negative zero. And so they actually went to extreme lengths to try to pretend as if it didn't exist which we're gonna see a whole bunch of weirdness. For example:

```JavaScript
var trendRate = -0;
trendRate === -0;             // true

trendRate.toString();         // "0"   OOPS!
trendRate === 0;              // true   OOPS!
trendRate < 0;                // false
trendRate > 0;                // false

Object.is(trendRate, -0);     // true
Object.is(trendRate, 0);      //false
```

> **NOTE:** When we `toString()` the `-0` we got `0`.

> **NOTE:** The triple equals operator ( === ) again lies to us, cuz it says the `-0` is equal to a `0`.

> **NOTE:** the `<` (less than sign) and the `>` (greater than sign) , also lie to us.

Until ES6 they added a utility called `Object.is()`, and it doesn't lie at all. So the best way to do this is to use this built-in checker and by the way, object.is can be used to check `NaN`s.

> The `Object.is()` method (ES6) determines whether two values are the same value. And the best way to check if `-0` and `0` are the same or not.
>
> **NOTE:** The `Object.is()` also can be useful to check `NaN`s.

```JavaScript
Math.sign(-3);         // -1
Math.sign(3);          // 1
Math.sign(-0);         // -0   WTF?
Math.sign(0);          //  0   WTF?

// "fix" Math.sign(..)
function sign(v) {
    return v !== 0 ? Math.sign(v) : Object.is(v, -0) ? -1 : 1;
}

sign(-3);              // -1
sign(3);               // 1
sign(-0);              // -1
sign(0);               // 1
```
> If we had a `-0` or `0`. Unfortunately the `Math.sign()` return negative zero and zero instead of negative one and one. 

> The `Math.sign()` function returns either a positive or negative +/- 1, indicating the sign of a number passed into the argument. 
>
> **NOTE:** If the number passed into Math.sign() is 0, it will return a +/- 0. Note that if the number is positive, an explicit (+) will not be returned.
>
> **Syntax:** `Math.sign(x)`
>
> **Parameters:** `x` A number. If this argument is not a `number`, it is implicitly converted to one.
>
> **Return value:** A number representing the sign of the given argument:
> * If the argument is positive, returns `1`.
> * If the argument is negative, returns `-1`.
> * If the argument is positive zero, returns `0`.
> * If the argument is negative zero, returns `-0`.
> * Otherwise, `NaN` is returned.
>
> [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sign)