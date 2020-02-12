# Deep JavaScript Foundations

> **Note:** In this note, I collected some important JavaScript foundations, that most of this information comes from Kyle Simpson's books ([YDKJS](https://github.com/getify/You-Dont-Know-JS) series), [ECMAScript](https://www.ecma-international.org/), [MDN](https://developer.mozilla.org/en-US/) (Mozilla Developer Network) or courses that I took. It did help me a lot to have a deeper understanding of JavaScript.

## Table of Contents

1. [Introduction](#Introduction)
2. [Types](#Types)
3. [Coercion](#Coercion)
4. [Equality](#Equality)
5. [Static Typing](#Static Typing)
6. [Scope](#Scope)
7. [Scope](#Scope)

## Introduction

First of all, we should know that, when we face a bug in JavaScript, the first question we need to ask ourselves is what does the specification say should happen? and then ask does my behavior that I'm seeing in code match the specification. The JavaScript specification is the source of authority, not the JavaScript engine or MDN (Mozilla Developer Network).

> The JavaScript specification ( [ECMAScript](https://www.ecma-international.org/) ) is the source of authority.

When does the bug happen?
when we have incorrect thinking or mental model. Then it's a divergent to what we're thinking and what the computer's doing and that's when the bug enters the code.

> Whenever there is a divergence between what your brain thinks and what is actually happening in the computer, that's where bugs enter the code.
>
> --getify's law #17

JavaScript can be divided into three cores (pillars):

- Types:

  - `Primitive Types`
  - `Abstract Operations`
  - `Coercion`
  - `Equality`
  - `TypeScript, Flow, etc.`

- Scope:

  - `Nested Scope`
  - `Hoisting`
  - `Closure`
  - `Modules`

- Objects (Oriented):
  - `this`
  - `class{}`
  - `Prototypes`
  - `OO vs. OLOO`

## Types

### Primitive Type:

Many of us have probably heard this assertion before:

> "In the JavaScript, everything is an object."
>
> (false)

There is a reason for this statement, but this statement is a misconception, this is false.
The reason behind why people say everything is an object, is because most of the values in JavaScript can behave as objects. But that does not make them objects.

Let's take a look at the JavaScript specification:

> ![ECMAScript](https://user-images.githubusercontent.com/37678729/71778735-c9916300-2fc6-11ea-9732-e453c03cf75b.png)
>
> [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-ecmascript-language-types)

> The latest ECMAScript standard defines eight data types:
>
> - Seven data types that are primitives:
>   - `Boolean`
>   - `Null`
>   - `Undefined`
>   - `Number`
>   - `String`
>   - `Symbol` (added in ES6)
>   - `BigInt` (future)
> - and `Object`
>   - `Function`
>   - `Array`
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

> ![ECMAScript](https://user-images.githubusercontent.com/37678729/71778767-2b51cd00-2fc7-11ea-88d6-35eec3875d6f.png)
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
typeof v;                   // "object"      OOPS!

var v = function() {};
typeof v;                   // "function"    hmmm?

var v = [1, 2, 3];
typeof v;                   // "object"      hmmm?

// coming soon!
var v = 42n;
// or: BigInt(42)
typeof v;                   // "bigint"
```

Why `typeof` null returns object? This is a historical fact from ES1 which essentially indicated to developers that if you wanted to unset a regular value, like a number, you would use undefined. But if you wanted to unset an object reference, you would use null.

Remember, function is not an official type at the top level, in the most official sense. But it has its own return value here, the typeof operator returns as function. So it's useful that it returns to us function, but arrays, it doesn't. Arrays just returns object.

> The `Array.isArray()` will tell you for sure or not whether you have an array.

The following table summarizes the possible return values of `typeof` from [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#Description).

| Type                                                    | Result      |
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
typeof 37 === "number";
typeof 3.14 === "number";
typeof(42) === "number";
typeof Math.LN2 === "number";
typeof Infinity === "number";
typeof NaN === "number"; // Despite being "Not-A-Number"
typeof Number("1") === "number";      // Number tries to parse things into numbers
typeof Number("shoe") === "number";   // including values that cannot be type coerced to a number

typeof 42n === "bigint";


// Strings
typeof "" === "string";
typeof "bla" === "string";
typeof `template literal` === "string";
typeof "1" === "string"; // note that a number within a string is still typeof string
typeof (typeof 1) === "string"; // typeof always returns a string
typeof String(1) === "string"; // String converts anything into a string, safer than toString


// Booleans
typeof true === "boolean";
typeof false === "boolean";
typeof Boolean(1) === "boolean"; // Boolean() will convert values based on if they're truthy or falsy
typeof !!(1) === "boolean"; // two calls of the ! (logical NOT) operator are equivalent to Boolean()


// Symbols
typeof Symbol() === "symbol"
typeof Symbol("foo") === "symbol"
typeof Symbol.iterator === "symbol"


// Undefined
typeof undefined === "undefined";
typeof declaredButUndefinedVariable === "undefined";
typeof undeclaredVariable === "undefined";


// Objects
typeof {a: 1} === "object";

// use Array.isArray or Object.prototype.toString.call
// to differentiate regular objects from arrays
typeof [1, 2, 4] === "object";

typeof new Date() === "object";
typeof /regex/ === "object"; // See Regular expressions section for historical results


// The following are confusing, dangerous, and wasteful. Avoid them.
typeof new Boolean(true) === "object";
typeof new Number(1) === "object";
typeof new String("abc") === "object";


// Functions
typeof function() {} === "function";
typeof class C {} === "function";
typeof Math.sin === "function";
```

### `undefined` vs. `undeclared` vs. `uninitialized`:

This is often confused between `undefined` and `undeclared` concept. And it's confused because developers, quite naturally, think of these words as sort of synonyms. They seem like they mean kind of almost the same thing. And in English, that's probably true. But in programming, it's entirely not true. In programming, and especially in JavaScript, these are two entirely different concepts.

> `undefined` and `undeclared` in JavaScript are two entirely different concepts.

When we said type of V, and we got back quote, `undefined`, V or whatever that variable is, didn't even exist. So how is it that I can get back quote `undefined` when something doesn't even exist? Well, that's another historical wart, JavaScript trying to pretend as if the absence of a declaration isn't that big a deal. It's not that big of a problem, you can work around it. In retrospect, they never should have done that. They should have just returned a string `undeclared`.

> The `undeclared` means it's never been created in any scope that we have access to.
>
> The `undefined` means there's definitely a variable, and at the moment, it has no value.
>
> the `uninitialized` or TDZ (Temporal Dead Zone) was introduced with ES6, and the best way to describe this is uninitialized. Meaning you can't touch it yet.

> The `typeof` operator is the only operator in existence that is able to reference a thing that doesn't exist and not throw an error.

The idea for `uninitialized` is that certain variables, never initially get set to undefined. When something is in an uninitialized state, it is off-limits. You're not allowed to touch it in any way, shape or form, or you'll get an error, and the error you get is the **TDZ error**.

We can have a variable that's never been initialized. We can have a variable that's been initialized that is undefined. Or we can have a variable that was never even created, and then it's undeclared. Three different concepts that we need to wrap our brains around.

### Special Values:

#### NaN:

The `NaN` is supposedly an acronym for not a number. Essentially `NaN` doesn't mean not a number essentially it means this special what we call sentinel value that indicates an invalid number. That's a much better mental model for it than referring to it is not a number, we should refer to at as an invalid number.

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
> The `Number.isNaN()` method (ES6) tells us defiantly for sure the passed value, is the `NaN` value or it's not.
>
> **Note:** This function differs from the global `isNaN` function in that it does not convert its argument to a Number before determining whether it is `NaN`.

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

trendRate.toString();         // "0"    OOPS!
trendRate === 0;              // true   OOPS!
trendRate < 0;                // false
trendRate > 0;                // false

Object.is(trendRate, -0);     // true
Object.is(trendRate, 0);      //false
```

> **Note:** When we `toString()` the `-0` we got `0`.

> **Note:** The triple equals operator ( === ) again lies to us, cuz it says the `-0` is equal to a `0`.

> **Note:** the `<` (less than sign) and the `>` (greater than sign) , also lie to us.

Until ES6 they added a utility called `Object.is()`, and it doesn't lie at all. So the best way to do this is to use this built-in checker and by the way, object.is can be used to check `NaN`s.

> The `Object.is()` method (ES6) determines whether two values are the same value. And the best way to check if `-0` and `0` are the same or not.
>
> **Note:** The `Object.is()` also can be useful to check `NaN`s.

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
> **Note:** If the number passed into Math.sign() is 0, it will return a +/- 0. Note that if the number is positive, an explicit (+) will not be returned.
>
> **Syntax:** `Math.sign(x)`
>
> **Parameters:** `x` A number. If this argument is not a `number`, it is implicitly converted to one.
>
> **Return value:** A number representing the sign of the given argument:
>
> - If the argument is positive, returns `1`.
> - If the argument is negative, returns `-1`.
> - If the argument is positive zero, returns `0`.
> - If the argument is negative zero, returns `-0`.
> - Otherwise, `NaN` is returned.
>
> [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sign)

### Fundamental Objects:

What are these fundamental objects, are they types? Sort of, but not really.

This is like the kind of bolted on object orientedness of JavaScript, the almost Java-like mutation of JavaScript where we have in all of those cases where we have primitive values, we now also have object representations with similar behaviors. Like in Java when you want to make a string and you call it new capital S string, we have things like that in JavaScript.

> Fundamental Objects aka Built-In Objects aka Native Functions.

So these are the ones where you really should absolutely use the new keyword:

- Usage of the `new` keyword:

  - `Object()`
  - `Array()`
  - `Function()`
  - `Date()`
  - `RegExp()`
  - `Error()`

If you need to construct an object of that fundamental type, then use `new Object()`, or `new Array()`, or `new Function()`. Probably the most useful one that would be use is `new Date()`.

But there are other ones that are fundamental objects which could be used with new, but you should definitely not use them with `new` keyword:

- Don't use with `new` keyword:

  - `String()`
  - `Number()`
  - `Boolean()`

They can be used with `new` keyword to construct the objects of this form. You should absolutely never do that. You should use them only as functions, not as constructors. In coercion we can see the usefulness of them. But `String()`, `Number()`, and `Boolean()` when used as a function coerce any value to that respective primitive type. That is a far more useful utility of those than the fact that they can construct this weird Frankensteiny object.

# Coercion

## Abstract Operation:

> ![ECMAScript](https://user-images.githubusercontent.com/37678729/71778794-8daacd80-2fc7-11ea-81fb-d072db0100ba.png)
>
> [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-abstract-operations)

When we think of conversion and coercion, you should really think of them as interchangeable, at least as far as JavaScript is concerned.

> In JavaScript, we refer to `Type Conversion` as `Coercion`.

> There are three variants of type conversion, so-called `“hints”`, described in the [specification](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-toprimitive):
>
> - `"string"` ((for `alert` and other operations that need a string))
> - `"number"` (for maths)
> - `"default"` (few operators)
>
> The specification describes explicitly which operator uses which hint. There are very few operators that “don’t know what to expect” and use the `"default"` hint. Usually for built-in objects `"default"` hint is handled the same way as `"number"`, so in practice the last two are often merged together.
>
> **No `"boolean"` hint:**
> Please note – there are only three hints. It’s that simple.
>
> There is no `“boolean”` hint (all objects are `true` in boolean context) or anything else. And if we treat `"default"` and `"number"` the same, like most built-ins do, then there are only two conversions.
>
> [javascript.info](https://javascript.info/object-toprimitive)

### ToPrimitive:

The first abstract operation that we have is called `ToPrimitive`. Now obviously, if we don't have a `primitive`, we need to turn it into a `primitive`. So if we have something `non-primitive`, like one of the `object` types, like an `object`, an `array`, a `function`, whatever, and we need to make it into a `primitive`, this is the abstract operation that's going to be involved in doing that.

> If we have a `non-primitive` value, and we need to make it into a `primitive`, there the `abstract operation` is going to be involved in doing that.

By the way, the **abstract operations** they're not like a function that could somehow be called. There may, in fact, be actual methods inside of a JavaScript engine or not, they're not like required to be actual things. But when we call them abstract, we mean they're a conceptual operation. So any time you have something that is not a `primitive` and it needs to become a `primitive`, conceptually, what we need to do is this set of algorithmic steps, and that's called `ToPrimitive`, as if it were a function that could be invoked.

So the `ToPrimitive` abstract operation takes an optional type hint. So in other words, it says, if you have something that is not a `primitive`, tell what kind of type you would like it to be.

> The `ToPrimitive` abstract operation takes an optional type hint.

If you're doing a numeric operation and it invokes `ToPrimitive`, guess what hint it's gonna send in? It's gonna say, I would like to have a `number`. That doesn't guarantee you a `number`, by the way. It's just a hint to say, the place that I'm using it, I would like it to be a `number`. If you're doing something `string` based, guess what hint it's going to send in? It's going to send in `string`.

Those are basically the only two hints. It can say, I would like it to be a `number`, I would like it to be a `string`, or I'm not going to tell you at all, so just give me whatever `primitive` you can.

Another thing you need to understand about the algorithms within JavaScript is that they are inherently recursive, which means that they define something, for example to `ToPrimitive`. If the return result from `ToPrimitive` is not a `primitive`, if it's another `non-primitive`, like another object, then it's gonna get evoked again, and it's gonna keep getting invoked until we can get something that's an actual `primitive`, or in some cases get an error.

> The Abstract Operation algorithms within JavaScript are inherently recursive.

So `ToPrimitive`, the way it works, essentially, boiling down the algorithm, is that there are two methods that can be available on any `non-primitive` (any `object`, `function`, `array`, whatever...). There are these two functions, The `valueOf()` function and the `toString()` function. And this algorithm says, if you've told me that the hint is `number`, then I'm going to first try to invoke the `valueOf()`, if it's there, and see what it gives me. And if it gives me a `primitive`, then we're done. If it doesn't give me a `primitive`, or it doesn't exist, then we try the `toString()`. And we either get a `primitive`or not. And if we tried both of those (functions), and we don't get a `primitive`, generally that's gonna end up resulting in an error.

That's if the hint was `number`. If the hint was `string`, they just reverse the order that they consult them in, but they still essentially consult both of them. So if the hint is `string`, we would ask for that that `non-primitives`, `toString()` method, and if it has it, use what it returns. And if it gives us legitimately a `primitive` like a `string`, which it should, then we'll just use that. And then we'll try to `valueOf()`.

> The way the `ToPrimitive` abstract operation works, is with two methods that available on any `non-primitive`.
>
> - The `valueOf()` function
> - The `toString()` function
>
> If we told this algorithm that the hint is `number`, it first invoke the `valueOf()`, if it gives us a `primitive`, then it's done, if it doesn't give us a `primitive`, then it invoke `toString()` to get `primitive`. and if it tried both of those functions, and don't get a `primitive`, then gonna end up resulting in an error.
>
> If the hint was `string`, it invoke `toString()` method on the `non-primitives`, if it gives us a `primitive` like a `string` (which it should) then we'll just use that. if it doesn't then it'll try `valueOf()`, and if we don't get a `primitive`, then we it's end up resulting in an error.

> The other object conversion function is called `valueOf()`. The job of this method is less well-defined: it is supposed to convert an `object` to a `primitive` value that represents the `object`, if any such `primitive` value exists. `Objects` are compound values, and most `objects` cannot really be represented by a single `primitive` value, so the default `valueOf()` method simply returns the `object` itself rather than returning a `primitive`. Wrapper classes define `valueOf()` methods that return the wrapped `primitive` value. `Arrays`, `functions`, and `regular expressions` simply inherit the default method. Calling `valueOf()` for instances of these types simply returns the `object` itself. The Date class defines a `valueOf()` method that returns the date in its internal representation: the number of milliseconds since January 1, 1970:
>
> ```JavaScript
> var d = new Date(2010, 0, 1);   // January 1st, 2010, (Pacific time)
> d.valueOf();                    // => 1262332800000
> ```
>
> [JavaScript: The Definitive Guide](https://www.oreilly.com/library/view/javascript-the-definitive/9781449393854/) ( Page 50 | Chapter 3: Types, Values, and Variables )

> **Description (`valueOf`):**
>
> JavaScript calls the `valueOf` method to convert an object to a primitive value. You rarely need to invoke the `valueOf` method yourself; JavaScript automatically invokes it when encountering an object where a primitive value is expected.
>
> By default, the `valueOf` method is inherited by every object descended from `Object`. Every built-in core object overrides this method to return an appropriate value. If an object has no primitive value, `valueOf` returns the object itself.
>
> You can use `valueOf` within your own code to convert a built-in object into a primitive value. When you create a custom object, you can override `Object.prototype.valueOf()` to call a custom method instead of the default `Object` method.
>
> [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf#Description)

So just keep in mind, if we're gonna use something that is not a `primitive` in some place that definitely needs `primitives`, like math or concatenation. We should realize, it is going to end up coercing it through this `ToPrimitive` algorithm, and it's gonna end up either invoking the `valueOf()` or the `toString()`.

> Briefly summarized, when converting from Object-to-String, the following steps are taken:
>
> 1. If available, execute the toString method.
>    - If the result is a primitive, return result, else go to Step 2.
> 2. If available, execute the valueOf method.
>    - If the result is a primitive, return result, else go to Step 3.
> 3. Throw TypeError.
>
> [Stackoverflow](https://stackoverflow.com/a/3945236/9540336)

### ToString:

The next abstract operation is `toString`. The `toString` abstract operation does what it sounds like. It takes any value and gives us the representation of that value in `string` form. And almost every value that you can imagine has at least some kind of representation in `string` form.

> The `toString` abstract operation takes any value and gives us the representation of that value in `string` form.

Some examples of things and what they end up producing as a string representation:

```JavaScript
         null   "null"
    undefined   "undefined"
         true   "true"
        false   "false"
      3.14159   "3.14159"
            0   "0"
           -0   "0"   // *

Abstract Operations: ToString
```

Things get a little strange when we look at the negative zero, remember that. We already saw that it lies, the `toString` operation for negative zero lies and produces a quote zero. So that's one of the corner cases.

> The `toString` operation produces a quote zero for negative zero. That's one of the corner cases.

So if we call `toString` on an `object` remember it's going to invoke the `toPrimitive` with the `string` hint. So what's that going to give us? Remember that's gonna end up calling `toString` first and if it's present and then it's going to use `valueOf`. That's the order that it does.

> If we call `toString` on an `object`, it's going to invoke the `toPrimitive` abstract operation with `string` hint. Which is going to call `toString` first and if it's present and then it's going to use `valueOf`.

> ### **ToString (object):**
>
> #### ToPrimitive (string)
>
> ##### aka: toString() / valueOf()
>
> Abstract Operations: ToString(Array/Object)

So what's gonna look like on some particular `object` like an `array`?

```JavaScript
                    []   ""
             [1, 2, 3]   "1,2,3"
     [null, undefined]   ","
    [[[], [], []], []]   ",,,"
                [,,,,]   ",,,"

Abstract Operations: ToString(Array)
```

Arrays have a default `toString`, which is strange serializes the representation of the `array`. Because they're leaving off the brackets.

> If we serialized a empty `array`, you get an empty `string`.
> The built-in `toString` on `arrays` leaves off the brackets.

> If we have an `array` with some contents in it, it'll show those contents unless they're `null` and `undefined`. So the `nulls` and `undefines`, when they show up in `arrays` just get left out.
>
> They're not represented as `nulls` and `undefines` the way `null` and `undefine` when `toString` do.

What about on an `objects`:

```JavaScript
                               {}   "[object Object]"
                           {a: 2}   "[object Object]"
    { toString() { return "X"; }}   "X"

Abstract Operations: ToString(Object)
```

> The default `toString` on the `Object.prototype` is to do that whole the square brackets thing, it does a lower case `object` and then it puts in this thing which is called the string tag.

And it turns out you can actually override the string tag for any of your own custom `objects` using an ES6 symbol.

> So that (capital O Object) `Object` is the default string tag for all default `objects`. And then the `toString` method takes that string tag and wraps this around it.

If you override the `toString` method, you can completely control what you want the stringtification of your `object` to look like. In this case, we are making it turn, return just a string X.

### ToNumber:

The `ToNumber` is a bit more interesting cuz there's a lot more corner cases involved.

> Anytime we need to do something numeric and we don't have a `number`, we're gonna invoke the `ToNumber` abstract operation.

```JavaScript
           ""   0   // The root of all evil in JavaScript. *
          "0"   0
         "-0"   -0
      " 009 "   9
    "3.14159"   3.14159
         "0."   0
         ".0"   0
          "."   NaN
       "Oxaf"   175

Abstract Operations: ToNumber
```

```JavaScript
        false   0
         true   1
         null   0   // *
    undefined   NaN

Abstract Operations: ToNumber
```

When we use `ToNumber` on a `non-primitive` (not a `string`, `undefined`, `boolean` or whatever), when we use it in an `object`, remember it invokes the `ToPrimitive` with the `number` hint. That consults first the valueOf, and then it consults the toString.

> ### **ToNumber (object):**
>
> #### ToPrimitive (number)
>
> ##### aka: valueOf() / toString()
>
> Abstract Operations: ToNumber(Array/Object)

So what does that look like?

> ##### (for [] and {} by default):
>
> ### **valueOf() { return this; }**
>
> ### **--> toString()**
>
> Abstract Operations: ToNumber(Array/Object)

> For any `array` or `object`, by default, meaning you have not overridden these, the `valueOf` method essentially just returns itself (does this, `return this`). Which has the affect of just ignoring the `valueOf` and deferring to `toString`.
>
> So it doesn’t even matter that the hint was number. It just goes directly to the toString.

You can think of the numberification of an `object` as, essentially, the stringification of it. It's that it's gonna end up producing whatever `toString` or `valueOf` produces. That's a perplexing choice, but it's the choice nonetheless, is that it's gonna actually produce a `primitive` `string`.

So then in your various operations where you were expecting a `primitive`, but you wanted a `primitive` `number`, there's actually a `primitive` `string` there. And then further coercions will kick in. So we're gonna end up deferring to the `toString` and whatever the `toString` returns.

> So with `ToNumber` we're gonna end up deferring to the `ToString` and whatever the `ToString` returns.

```JavaScript
           [""]   0   // *
          ["0"]   0
         ["-0"]   -0
         [null]   0   // *
    [undefined]   0   // *
      [1, 2, 3]   NaN
       [[[[]]]]   0   // *

Coercion: ToNumber(Array)
```

If the `array` has either `null` or `undefined`, it becomes 0. Because they first become empty `strings`, and then empty `string` becomes `0`. Remember, empty `string` is the root of all coercion evil.

And if you have an object

```JavaScript
                      { .. }   NaN   // means {} or for example {x: 5}
   { valueOf() { return 3;}}   3

Coercion: ToNumber(Object)
```

And remember what a stringification of an `object` by default is, it's that `[object Object]` thing. Which is definitely not a representation of a number, so we get `NaN`. If you override the `valueOf` for some `object`, you can return whatever thing you want.

> The stringification of `object` by default is `[object Object]`, Which is definitely not a representation of a `number`, so we get `NaN`.

### ToBoolean:

let's look at the `ToBoolean` abstract operation. And by the way, these four are what we're looking at, `toPrimitive`, `toString`, `toNumber`, and `toBoolean`. There are other abstract operations, but these are the major ones.

> Anytime we have any value that is not a `Boolean`, and it's used in a place that needs a `Boolean`, this operation occurs. Exactly the same as the other ones.

> The `ToBoolean` is less algorithmic and more lookup. So there's essentially a look up table.

There's not really anything to do, other than to say, is the value, what we call `falsy`, or not, that's really the only question here.

> If the value is one of these things (on look up table), return `false`. And otherwise just return `true`.

So it defines a very specific and short limited list of what we call `falsy` values.

> The values that will return `false` when coerced to a `Boolean`. These are the `falsy` values:
>
> - The empty `string`
> - either of the 0 (0, -0)
> - the `null`,
> - the `NaN`
> - the `false`
> - the `undefined`

| Falsy                              | Truthy                             |
| ---------------------------------- | ---------------------------------- |
| `""`                               | "foo"                              |
| `0,-0`                             | 23                                 |
| `null`                             | { a: 1 }                           |
| `NaN`                              | [1, 3]                             |
| `false`                            | `true`                             |
| `undefined`                        | function() {..}                    |
|                                    | ... (this list is infinitely long) |
| **Abstract Operations: ToBoolean** |                                    |

> If the value is not on that list, it is always truthy.

What would happen if you try to `toBoolean` an empty `array`? So is the empty `array` on the `falsy` the list? No, so it returns `truthy`

> The `ToBoolean`, it does not invoke the `toPrimitive` algorithm. Or the `toNumber`, or the `toString`, or anything, it just does a look up.

> when we're doing `toBooleans`, there's no other coercion stuff happening. It's just a straight look up, is it there or is it not.
>
> We can't override it with a `valueOf` or `toString` or anything. It's just is it on the list or is it not.

Essentially, memorize the falsy the list, and then always ask is the value on that list if so falsy, otherwise, it must be truthy.

## Cases of Coercion

> ### Converting Values:
>
> Converting a value from one type to another is often called `“type casting”`, when done `explicitly`, and `“coercion”` when done `implicitly` (forced by the rules of how a value is used).
>
> > It may not be obvious, but JavaScript coercions always result in one of the scalar primitive (see Chapter 2) values, like string, number, or boolean. There is no coercion that results in a complex value like object or function. Chapter 3 covers “boxing,” which wraps scalar primitive values in their object counterparts, but this is not really coercion in an accurate sense.
>
> Another way these terms are often distinguished is as follows: `“type casting”` (or `“type conversion”`) occurs in statically typed languages at compile-time, while `“type coercion”` is a runtime conversion for dynamically typed languages.
>
> However, in JavaScript, most people refer to all these types of `conversions` as `coercion`, so the way I prefer to distinguish is to say `“implicit coercion”` versus `“explicit coercion”`.
>
> The difference should be obvious: `“explicit coercion”` is when it is obvious from looking at the code that a type conversion is intentionally occurring, whereas `“implicit coercion”` is when the type conversion will occur as a less obvious side effect of some other intentional operation.
>
> For example, consider these two approaches to `coercion`:
>
> ```JavaScript
> var a = 42;
> var b = a + "";        // implicit coercion
> var c = String( a );   // explicit coercion
> ```
>
> [You Don't Know JS: Types & Grammar](https://github.com/getify/You-Dont-Know-JS) ( CHAPTER 4: Coercion | Page 57 )

Let's look at some examples where we're already doing coercion whether we realize it or not:

```JavaScript
// ES6 Template literals (Template strings)

var numStudents = 16;

console.log(
  `There are ${numStrudents} students.`
);
// "There are 16 students."

Coercion: number to string
```

> Overloaded operator: Defining or redefining how an operator (+, -, \*, /, etc.) acts.
>
> [Medium](https://medium.com/@julianknodt/javascript-operator-overloading-e1ebd2344b78)

> In JavaScript the `+` operator is overloaded to serve the purposes of both `number` addition and `string` concatenation.
>
> [You Don't Know JS: Types & Grammar](https://github.com/getify/You-Dont-Know-JS) ( CHAPTER 4: Coercion | Page 88 )

> The plus operator (`+`) is normally thought of, is doing numerical operation.
>
> But with plus operator (`+`) if either one of them is a `string`, the plus operator prefers `string` concatenation.
>
> Which means, if only one of them is a `string`, it's gonna call a `toString` abstract operation on it, and turn it in to a `string`.

> We could throw a value into an `array`, just the one value into an `array`, and then call `.join` on it. And that actually ends up stringify it.
>
> ```JavaScript
> console.log([16].join(""));
> // "16"
> ```
>
> Even though it does no `string` concatenation at all, `.join` first turns it into a `string`.

Because we're dealing with web applications, which means grabbing things as `strings`:

```JavaScript
function addAStudent(numStudents) {
  return numStudents + 1;
}

addAStudent(studentsInputElem.value);
// "161"   OOPS!

Coercion: string to number
```

> The plus operator (`+`) with `string` value (`+"6"` for example), invoke `toNumber` abstract operation.
>
> And if we add plus operator (`+`) to empty `string` value (+""), it end up to `0`.

```JavaScript
function addAStudent(numStudents) {
  return numStudents + 1;
}

addAStudent( + studentsInputElem.value);
// "17"

Coercion: string to number
```

If we use the minus operator (`-`) that one is only defined for `numbers`.

That it's not overloaded for `string`, it wouldn't make any sense to subtract one `string` from another.

So that minus operator (`-`), is gonna invoke that `toNumber` abstract operation.

```JavaScript
function kickStudentOut(numStudents) {
  return numStudents - 1;
}

kickStudentOut(studentsInputElem.value);
// "15"

Coercion: string to number
```

But what about `boolean`?

```JavaScript
if (studentsInputElem.value) {
  numStudents = Number(studentsInputElem.value)
}

Coercion: __ to boolean
```

If `studentsInputElem.value` as an empty `string` that's gonna be `falsy`, But what if that `string` has just a bunch of white space, now it's gonna be `truthy`.
It's not a valid `string` that you care about because it's got a bunch of white space in it but all of a sudden it's going to be `truthy`.

```JavaScript
while (newStudents.length) {
  enrollStudent(newStudents.pop());
}

Coercion: __ to boolean
```

So if my length is zero then it becomes `false` and if my length is anything non zero then it becomes `true`. Because it's not one of the zeros. But what happens when it's `NaN` it becomes `false`.

> The `Double NOT operator` or `Double negation` (`!!`) make things become a `boolean`.

> If wanna be super explicit, we can use:
>
> - `String()`
> - `Number()`
> - `Boolean()`

## Boxing:

> Because of `boxing` in JavaScript, we are able to access properties on `primitive` values.
> For example access a length on a primitive `string` or some method on a primitive `number`.
>
> The `boxing` is a form of `implicit` coercion.
> It's not called out in the same way in the **abstract operations**.

> This DOM elements value is always a `string`.

It is saying you have this thing that is not an `object` and you're trying to use it as if it is an `object`. So JavaScript is gonna be helpful and go ahead and make it into an `object` for us.
The only other option would be for the JavaScript to throw an exception that said you're trying to access a property on an `primitive` value.

But fortunetly JavaScript `implicitly` coerces these `primitives` into their `object` counterpart so that we can access properties and methods on them.

> Because of the `boxing` most of people think “everything in JavaScript is an `object`”.
>
> It turns out that things can behave as `objects`, but that doesn't make them an `object`

> ### All programming languages have `type conversions`, because it's absolutly necessary.

> Whether we call them `coercion` or we call them `conversion` every single language in existence that we've ever programmed in has to deal with `type conversions`.

## Corner Cases

> Because all languages have `type conversions`, that means all languages have `corner cases`, including JavaScript.

Here's an example of some of these corner cases:

```JavaScript
Number("");                     //   0                    OOPS!
Number("   \t\n");              //   0                    OOPS!
Number(null);                   //   0                    OOPS!
Number(undefined);              //   NaN
Number([]);                     //   0                    OOPS!
Number([1, 2, 3]);              //   NaN
Number([null]);                 //   0                    OOPS!
Number([undefined]);            //   0                    OOPS!
Number({})                      //   NaN

String(-0);                     //   "0"                  OOPS!
String(null);                   //   "null"
String(undefined);              //   "undefined"
String([null]);                 //   ""                   OOPS!
String([undefined]);            //   ""                   OOPS!

Boolean(new Boolean(false));    //   true                 OOPS!


Coercion: corner cases
```

The root of all (Coercion) Evil:

```JavaScript
studentsInput.value = "";

// ..

Number(studentsInput.value);            //   0

studentsInput.value = "   \t\n";

// ..

Number(studentsInput.value);            //   0

Coercion: corner cases
```

Not only does the empty `string` become zero, but any `string` that's full of white space also becomes zero.

Because the `toNumber` operations first strips off all leading and trailing `whitespace` before doing it’s `coercion`. So all examples of `whitespace` strings of all forms, still all end up producing that same `zero`.

There are also corner cases that are not as obvious:

```JavaScript
Number(true);              //   1
Number(false);             //   0

1 < 2;                     //   true
2 < 3;                     //   true
1 < 2 < 3;                 //   true      (but...)

(1 < 2) < 3;
(true) < 3;
1 < 3;                    //    true      (hmm...)

// *************************************

3 > 2;                     //   true
2 > 1;                     //   true
3 > 2 > 1;                 //   false      OOPS!

(3 > 2) > 1;
(true) > 1;
1 > 1;                     //   false

Coercion: corner cases
```

---

> JavaScript's dynamic typing is not a weakness, it's one of its strong qualities.
>
> The first truly `multi-paradigm` language and a big reason why it has been able to survive `multi-paradigm` is because of its `type` system.

> For `code comments`: You should not have more reliance upon `code comments` than the code.
>
> Problem with `code comments`: People write the `How` in their `code comment` but what we want is to the `code comment` tell us, `Why`.

> Implicit in JavaScript in not a magic or bad thing, but should think about implicitness as `abstraction`.
>
> In the `abstraction` we're hiding unnecessary details, because that re-focuses the reader on the important stuff.

So some of the implicit nature of JavaScript's type system is sketchy, but some of it is quite useful. For example, the `boxing`.

# Equality

## Double & Triple Equals:

> JavaScript provides three different value-comparison operations:
>
> - `===` - `Strict Equality` Comparison ("`strict equality`", "`identity`", "`triple equals`")
> - `==` - `Abstract Equality` Comparison ("`loose equality`", "`double equals`")
> - `Object.is` provides SameValue (new in `ES2015`).
>
> Which operation you choose depends on what sort of comparison you are looking to perform. Briefly:
>
> - `double equals` (`==`) will perform a `type conversion` when comparing two things, and will handle `NaN`, `-0`, and `+0` specially to conform to [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) (so `NaN != NaN`, and `-0 == +0`);
>
> - `triple equals` (`===`) will do the same comparison as `double equals` (including the special handling for `NaN`, `-0`, and `+0`) but without `type conversion`; if the types differ, `false` is returned.
>
> - `Object.is` does no type conversion and no special handling for NaN, -0, and +0 (giving it the same behavior as `===` except on those special numeric values).
>
> Note that the distinction between these all have to do with their handling of `primitives`; none of them compares whether the parameters are conceptually similar in structure. For any `non-primitive` objects `x` and `y` which have the same structure but are distinct objects themselves, all of the above forms will evaluate to `false`.
>
> [MDN](Equality_comparisons_and_sameness)

> ![ECMAScript](https://user-images.githubusercontent.com/37678729/71779325-2262fa80-2fcb-11ea-835d-debd2bccb90f.png)
>
> [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-abstract-relational-comparison)

> This is not exactly the case:~~~The difference between `double equals` and `triple equals` is that `double equals` checks the value so called `loose equality` and `triple equals` checks the `value` and the `type` so called `strict equality`.~~~
>
> The `double equals` and the `triple equals` both checked the types, it's just that one does something different with that information than the other one.

The `triple equals` is also checking the types, and if they're not the same, it's `false`. It doesn't matter what the values are, if the types are different, it doesn't do anything else. It sorta short-circuits and says, if the types are different there's no possible way that they could be equal.

> Essentially the real difference between `strict equality` and `loose equality` is whether or not we're going to to allow any coercion to occur.

> If the types are the same in `Strict Equality` (`===`), it's going to, return `false` if they're `NaNs`, because remember it's supposed to lie about `NaNs`. And it's gonna return `true` if there's a `-0` because it's supposed to lie about `-0`. But it only does the lies if the types already match.

Otherwise, it says `false` and it didn't check anything at all. They both check the types one of them stops early and one of them doesn't. Or said a different way, the difference is whether we allow `coercion`.

We have two `objects`:

```JavaScript
var workshop1 = {
  name: "Deep JS Foundations"
};

var workshop2 = {
  name: "Deep JS Foundations"
};

if (workshop1 == workshop2) {
  // Nope
}

if (workshop1 === workshop2) {
  // Nope
}

Equality: identity, not structure
```

We have two `objects`, they have the same structure and ostensibly and the same value. But they are not the same object.

The JavaScript doesn't do like a deep assertion check that the structure of one object is exactly the same as a structure of another object.

> The Equality comparisons in JavaScript does `identity comparison` not `structure comparison`.

If workshop1 and workshop 2 pointed at literally the same `object reference` then their `identity` would be the same and you'd get `true`.
So neither `==` nor `===` is gonna return a `true` because they are different objects.

> The `==` is going to allow `coercion` when the `types` were different. And the `===` is going to disallow `coercion` when the `types` are the same.

## Coercive Equality:

> In the `double equals` if the `types` are `null` or `undefined` on either side, then return `true`.
>
> The `null` value and the `undefined` value are `coercively` equal to each other and to no other values in the language.
>
> So we have the option of treating `null` and `undefined` as indistinguishable (or pretend that they're interchangeable) through coercive equality.

Here is a example that it is better to treating `null` and `undefined` as indistinguishable:

```JavaScript
var workshop1 = { topic: null };
var workshop2 {};

if (
  (workshop1.topic === null || workshop1.topic === undefined) &&
  (workshop2.topic === null || workshop2.topic === undefined) &&
) {
  // ..
}

if (
  workshop1.topic == null &&
  workshop2.topic == null
) {
  // ..
}

Coercive Equality: null == undefined
```

The reader has not gaining anything readability-wise by being `explicit` between two empty values.

The second one (`double equals`) says, whether they are `null` or `undefined`, tell us if they're empty or not. Tell us if they're one of those two empty values. And by the way, we just picked the shorter of the two cuz it's less to type.

## Double Equals Algorithm

If we're not talking about `nulls` and `undefines`, but we're talking about `strings`, `numbers` and `booleans`:

> For `strings`, `numbers` and `booleans` primitive values, the `Abstract Equality` (`double equals`) prefers to do numeric comparison (`toNumber`).

> The [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-abstract-relational-comparison) says if one of them is `number`, but the other one is `string`, then call `toNumber` on the `string`, so it can do a numeric comparison.
>
> If one of them's `boolean`, then do a `toNumber` on it, and make the comparison.

> The `double equals` prefers numeric comparison.

Here is a example of `string` and `number`:

```JavaScript
var workshopEnrollment1 = 16;
var workshopEnrollment2 = workshop2Elem.value;

if (Number(workshopEnrollment1) === Number(workshopEnrollment2)) {
  // ..
}

// Ask: what do we know about the types here?
if (workshopEnrollment1 == workshopEnrollment2) {
  // ..
}

Coercive Equality: perfers numeric comparison
```

So the abstraction of the `double equals` is helpful there.

> But if they are both `strings`, that means they are both of the same `type` (literally `identical`), it does the `triple equals` (`===`).
>
> The `double equals` is going to allow `coercion` when the `types` are different.
>
> If we're in a scenario where the `types` are not different, where the types are the same, then it's not ever going to invoke `coercion`, ever.

> if we use a `double equals` with something that's not already a `primitive`, We invoke the `toPrimitive` abstract operation.

The `double equals` only compares `primitives`. If we use it with something that's not a `primitive`, it turn it into a `primitive`. The only time it does something useful with `non-primitives` is when they're the exact same type and then it just does the same value `triple equals` comparison. Otherwise it invokes `toPrimitive` abstract operation.

This is a example where `coercion` is a very bad thing for us:

```JavaScript
var workshop1Count = 42;
var workshop2Count = [42];

// 1- if (workshop1Count == workshop2Count)
// Because workshop2Count is non-primitive, algorithm invokes toPrimitive on it.
// (because of that weird array stringification that doesn’t include the brackets.
// And only accidentally working because there is only one value in the array.)

// 2- if (42 == "4")
// Now we have two different types, we have a number and a string. There's two options here.
// We could either make the number into a string and compare the strings,
// or make the string into a number and compare the numbers. The algorithm prefers numeric comparison.
// So the string becomes the number.

// 3- if (42 === 42)
// Because the types are now the same, it does a `triple equals` comparison.

if (true) {
  // Yep (hmm...)
}

Coercive Equality: only primitives
```

> We should never use `double equals` to compares a `primitive` with `non-primitive` value.

### Summary:

This is summary of how `double equals` works:

- If the types are the same it's just gonna use `triple equals` (`===`).
- If both of them are `null` or `undefined`, they are equal.
- If there are `non-primitives` involved in the comparison, they are always gonna become `primitive` first.
- And then once we have `primitives`, prefer `toNumber`.

## Double Equals Corner Cases

How could something like an empty array somehow be coercively equal to the negation of itself?

```JavaScript
[] == ![];      // true WAT!?

// let's explain how it works. With an example:

var workshop1Students = [];
var workshop2Students = [];

1- if (workshop1Students == !workshop2Students) {
  // Yep, WAT!?

  // 1- if ([] == ![])
  // The workshop2Students is an array which is truthy. So if we negate (!) it it becomes false.

  // 2- if ([] == false)
  // Now, we have a non-primitive compared to a primitive.
  // So we need to turn that non-primitive (array) into a primitive. So it becomes the empty string.

  // 3- if ("" == false)
  // So now, We have two primitives but they are not of the same type.
  // The algorithm prefers that they both become numbers.

  // if (0 === 0)
  // if (true)
}


2- if (workshop1Students !== workshop2Students) {
  // Yep, WAT!?

  // if (!(workshop1Students == workshop2Students))
  // if (!(false))
  // if (true)
}

== Corner Cases: WAT!?
```

Compare the first one in your mind to the more appropriate comparison (the second one), which is to say, I want to check to see if they're not the same `array`.

Those might look like the same thing (first one and second one), but these are entirely different approaches. One is saying I wanna see if it is coercively `equal` to its `negation` (`!`), and the other one is saying I wanna see if it is not coercively `equal`. Those are entirely different beasts.

In the second one, since they're both `arrays`, then what we're effectively doing is asking an `identity` question. We're saying, are they not the same `identity`?

And it would work `identically` if we use the `triple equals` version of them. It's a rational thing, and it has no difference in the rational case between double equals and triple equals.

### Corner Cases Booleans:

This is another example one of those corner cases that we shouldn't do:

```JavaScript
var workshopStudents = [];

1- if (workshopStudents) {
  // Yep

  // if (Boolean(workshopStudents))
  // if (true)
}

2- if (workshopStudents == true) {
  // Nope :(

  // if ([] == true)
  // We have a non-primitive which need to became primitive, so it becomes empty string.

  // if ("" == true)
  // We have an empty string and a true. These are not the same type, so they need to both become numbers.

  // if (0 === 1)
  // One of them becomes 0, (which should have been NaN), the other one becomes 1.

  // if (false)
}

3- if (workshopStudents == false) {
  // Yep :(

  // if ("" == false)
  // if (0 === 0)
  // if (true)
== Corner Cases: booleans
}
```

If we wanna check and we want to allow the `boolean` coercion of an `array` to be `true`. In other words, we wanna say its `truthy` sort of construct.
There's one way of doing it (the first statement), which is just to do an if statement. Allow the if statement to invoke the `toBoolean` operation on the `array`, which in this case, is a lookup that says the `array` is not on the table, so, therefore, it's `true`. That's a perfectly rational implicit `toBoolean` coercion.

But if we try to get more clever and tricky with it and say, well, if it's `truthy`, then maybe what we want to do is do a `double equals` with `true`. Well, now all of a sudden it doesn't work. And by the way, it wouldn't work with `triple equals` either. There's no case where second and third statement are better. And there's a bunch of cases where they are worse, like these one.

So `implicit` is sometimes much better than `explicit`.

> Don't ever do a `double equals`, (or even in that case, `triple equals`) with `true` or a `double equals` with `false`.
>
> We don't need to `double equals` to `true`, or `double equals` to `false`, we should allow the `toBoolean` to happen `implicitly`.

Summary:

How to avoid corner cases with the `double equals`:

- Avoid the `double equals` when either side of them can be a `0`, or an `empty string`, or even one of those `strings` with only `whitespace` in it.
- Don't use it with `non-primitives`. Only use it for coercion among the `primitives`.
- Definitely don't use `double equals` to `true` or `double equals` to `false`. Essentially, just allow the `ToBoolean` to happen. And if we really can't allow that, if it really has to exactly `true` or exactly `false`, which sometimes it does, then use `triple equals`.

---

### The Case for Double Equals:

If we <u>know</u> the `type(s)` in a comparison:

- Whether the `types` match or not, `==` is the more sensible choice when we know the `types`.

If we <u>don't know</u> the `type(s)` in a comparison:

- If we can't or won't, use a style where we can know and have obvious `types`, the only thing that is sensible and reasonable is for us to use the `===`.

Making our `types` known and obvious just leads to better code. Knowing the `types` leads to better code and if the `types` are known, `==` is always best.
In every scenario `==` is best when the `types` are known and in any scenario where you can't you should fall back to `===`.

# Static Typing

## Typecript & Flow:

let's look at what `TypeScript` and `Flow` can do for us.

**Benefits of Typecript & Flow:**

1. Catch type-related mistakes.
2. Communicate type intent. (because the typing is in the code.
   And it's gonna make our code more obvious.)
3. Provide IDE feedback. (maybe one of the biggest things is it get us an amazing amount of feedback through the tooling ecosystem that can show up live directly in our IDE.)

**Caveats of TypeScript & Flow:**

1. Inferencing is best-guess, not a guarantee.
2. The annotations are optional. (Which means the developers on our team, if they don't put an annotation on a variable, `TypeScript` will default to the any type unless we have that turned off. And then we're not getting any benefit out of it.)
3. Any part of the application that isn't typed, introduces uncertainty.

## Inferencing:

So some examples of `TypeScript` and `Flow`, and these examples are actually identical between the two:

```JavaScript
var teacher = "Kyle";

// ..

teacher = { name: "Kyle"};
// Error: can't assin object to string


Type-Awarw Linting: inferencing
```

> `Type inference` refers to the automatic detection of the `data type` of an expression in a programming language.
>
> [Wikipedia](https://en.wikipedia.org/wiki/Type_inference)

If we don't do any typing at all, both `TypeScript` and `Flow` by default will do some inferencing.

So here they're doing a `static types inference`, which means my intent (that they're guessing), is that we want `teacher` (the variable), to only ever hold `strings`. And when we later try to assign it something `non-string`, it throws us an error, and says, you're doing an assignment that you shouldn't do. That's their best guess.

But that's what we refer to as `static types`, inferring that the variable has a `type` based upon the value that goes into it. JavaScript `variables` don't have `types`, but we're layering on this extra requirement.

So that's when we don't annotate the `types`, but of course we can annotate the `types`:

```JavaScript
var teacher: string = "Kyle";

// ..

teacher = { name: "Kyle"};
// Error: can't assin object to string


Type-Awarw Linting: annotating
```

We can say `teacher` is definitely a `string`. We're gonna get basically the same error, but here we're not guessing at the error. We're literally saying we intended for this thing to only ever hold `strings`, and now we're trying to put something `non-string` to it. In both cases `TypeScript` and `Flow` are gonna throw us an error and say, you're assigning something you shouldn't have.

## Custon Types:

We can define custom types like this:

```JavaScript
type student = { name: string };

function getName(studentRec: student): string {
  return studentRec.name;
}

var firstStudent: student = { name: "Farank"};

var firstStudentName: string = getName(firstStudent);


Type-Aware Linting: custom types & signatures
```

Here we're defining that an `object` of a `type` that has a property called `name` that is of type `string`, that is a `type`. And then we can pass values of that `type` as parameters. And we can receive values back as parameters. So here we are passing in `studentRec` of the type `student`. we're defining our own `type`. this program doesn't have any errors.

But most of the guarantee here is are things being assigned correctly.
A parameter to a `function` is a lot like a `variable`. If we're saying we wanna only be able to pass in `numbers`, then we're basically saying the same thing as we want this `variable` to only hold `numbers`.

Where I to uses something like `TypeScript` I probably would define many more of my parameters as say union `types`. Or I would say, you know what, I'm gonna allow `strings`, `numbers`, and `nulls`. Because it's rare that I want it to be so restrictive that it can only ever receive exactly this kind of structured `object` for example.

But nevertheless, it's able to do some very useful guarantees if the problems that you have are misassignments of types.

## Validating Operand Types

In this particular case, `TypeSript/Flow` saying we can't subtract a `string` from a `number`:

```JavaScript
var studentName: string = "Frank";

var studentCount: number = 16 - studentName;
// error: can't substract string


Type-Aware Linting: validating operand types
```

Because that particular preference is saying don't allow that `coercion`, and in this particular case, that would be a really useful help for us.

An undervalued part of what they do, Is that they are actually allowing us to check the operations that we're doing where most of our business logic is, and making sure that those operations are valid.

> It would be nice if `TypeScript` would have some mechanism by which we could allow some more `coercion` to occur.

Because there are plenty of places, where we'd like to be able to do `coercion` and other places we'd like to avoid it. It appears that `TypeScript` is kind of all or nothing. We opt into it or we don't opt into it.

## Static Typing Pros and Cons

### Pros:

- They make types more obvious in code.
- Familiarty: they look like other language's type systems.
- Extremely popular these days.
- They're <u>very</u> sophisticated and good at what they do.

### Cons:

- They use "non-JS-standard" syntax (or code comments) (they chose to use a syntax that they had to layer on top of JavaScript.)
- They require\* a build process, which raises the barrier to entry.
- Their sophistication can be intimidating to those without prior formal types experience.
- They focus more on "static types" (variables, parameters, returns, properties, etc) than <u>value types</u>.

These tools came out with a way that we can do their typing annotations using only code comments. So at least in that scenario, we haven't locked ourself into if we don't use this tool this code literally can't run. That's sort of an escape valve, and that's a good thing, but almost nobody's using the code comments. Everybody's using the inline syntax annotations.

---

**Summary:**

- JavaScript has a (`dynamic`) `type` system, which uses various forms of `coercion` for `value type` `conversion`, including `equality comparisons`.

- We simply cannot write quality `JS` programs without knowing the `types` involved in our operations.

- ...

# Scope

> `Scope` is where to look for things.

Here in this example we're looking for `identifiers`:

```JavaScript
x = 42;
console.log(y);
```

Here you see an `x` that's being assigned to, or a `y` that's being a value retrieved from.

> **Identifiers and Reserved Words**
>
> An `identifier` is simply a name. In JavaScript, `identifiers` are used to name `variables` and `functions` and to provide labels for certain loops in JavaScript code. A JavaScript `identifier` must begin with a `letter`, an `underscore` (`_`), or a `dollar sign` (`$`). Subsequent characters can be `letters`, `digits`, `underscores`, or `dollar signs`. (`Digits` are not allowed as the first character so that JavaScript can easily distinguish `identifiers` from `numbers`.) These are all legal `identifiers`:
>
> ```JavaScript
>   i
>   my_variable_name
>   v13
>   2.4 Identifiers and Reserved Words | 23
>   Core JavaScript
>   _dummy
>   $str
> ```
>
> For portability and ease of editing, it is common to use only `ASCII letters` and `digits` in `identifiers`. Note, however, that JavaScript allows `identifiers` to contain `letters` and `digits` from the entire `Unicode` character set. (Technically, the `ECMAScript` standard also allows `Unicode` characters from the obscure categories Mn, Mc, and Pc to appear in `identifiers` after the first character.) This allows programmers to use variable names from non-English languages and also to use `mathematical` symbols:
>
> ```JavaScript
>   var sí = true;
>   var π = 3.14;
> ```
>
> Like any language, JavaScript reserves certain `identifiers` for use by the language itself. These `“reserved words”` cannot be used as regular `identifiers`.
>
> [JavaScript: The Definitive Guide](https://www.oreilly.com/library/view/javascript-the-definitive/9781449393854/) ( Page 23 | 2.4 Identifiers and Reserved Words )

All `variables` are in one of those two roles in our program:

1. Receiving the assignment of some `value`
2. We are retrieving a `value` from the `variable`.

When the scope is being processed by the JavaScript engine, it's essentially asking two question, when it see this `variable`:

1. What position is it in?
2. What scope does it belong to?

In other words, this is basically like a game of matching marbles to their color-coded buckets. If you think about the way a JavaScript engine processes the code, it's going to find a `variable` and it's gonna say, hmm, this is a green marble so it goes in the green bucket. And this is a red marble, so it goes into the red bucket. So it's fundamentally a game of sorting colored marbles.

But this **processing** that we're talking about is an actual step within JavaScript. It's not simply inlined with the **execution**. It's extremely common for people to think about JavaScript as running top down, line by line, executing.

> Because when we think of **interpreted programs**, or **dynamic scripted programs**, we generally think of them as executing line by line, top down.
>
> But it turned out, JavaScript is not an **interpreted language**, but is in fact actually a **compiled language**, that may not fit with our whole mental model because we're probably used to thinking of line-by-line **JavaScript interpretation**.

> **Compiled Languages:**
>
> Compiled languages are converted directly into machine code that the processor can execute. As a result, they tend to be faster and more efficient to execute than interpreted languages. They also give the developer more control over hardware aspects, like memory management and CPU usage.
>
> Compiled languages need a “build” step - they need to be manually compiled first. You need to “rebuild” the program every time you need to make a change. In our hummus example, the entire translation is written before it gets to you. If the original author decided he wanted to use a different kind of olive oil, the entire recipe would need to be translated again and then sent to you.
>
> Examples of pure compiled languages are C, C++, Erlang, Haskell, Rust, and Go.
>
> **Interpreted Languages:**
>
> Interpreters will run through a program line by line and execute each command. Now, if the author decided he wanted to use a different kind of olive oil, he could scratch the old one out and add the new one. Your translator friend can then convey that change to you as it happens.
>
> Interpreted languages were once known to be significantly slower than compiled languages. But, with the development of just-in-time compilation, that gap is shrinking.
>
> Examples of common interpreted languages are PHP, Ruby, Python, and JavaScript.
>
> [freeCodeCamp](https://guide.freecodecamp.org/computer-science/compiled-versus-interpreted-languages/)

> So the JavaScript is, in fact, **compiled**, or at least, as we would say, it's **parsed**.
>
> There's some **processing step** that has to happen before **execution** has occurred.

So let me prove to you, if you have ever written a JavaScript, syntax error, left off a comma, had an extra parenthesis, or curly brace somewhere, and then you try to run the program, and you immediately got a syntax error.

That is, say you have a syntax error on line 10, but you immediately get that error reported to you before lines 1 through 9 have executed.

The question is: How is it possible that JavaScript knew about the syntax error on line 10 before executing lines 1 through 9, unless JavaScript actually went through a **processing step** first. As opposed to simply **executing** top down, there was some **processing step**.

What does that **processing step** look like?

**In compiler theory, there are essentially four stages to a compiler:**

(Sometimes the first two are combined into one stage, sometimes they're separate.)

1. There's **lexing** and **tokenization**.
2. There's **parsing** (which turns the stream of **tokens** into what's called an **abstract syntax tree**).
3. The last step is what's called **code generation** (taking an **abstract syntax tree** and producing some kind of other **executable** form of that program).

This is how our program gets processed from its **textual code** and our **source format** into some kind of representation that can be executed.

Now, a lot of people think, well, JavaScript can't be **compiled** because I don't run a **compiler** on my development machine and then ship off the `compiled code` to some other location.

So in other words, a lot of people think about this difference between **interpreted** and **compiled** as the distribution model for the binary. But that's not really the right axis to be thinking about. The right axis is, is the code processed before it is executed or not? We do have languages that exist which generally don't get **processing** before **execution**. for example, `Bash script`.

In a `Bash script`, if we write a malformed command on line 4, lines 1, 2, and 3 are gonna run. And when line 4 fails that might screw stuff up, because we wanna undo lines 1, 2, and 3. This is a perpetual problem in something like a true scripting language.

But in JavaScript, if you have a syntax error on line 10 nothing happens on lines 1 through 9. It has to be processed to check that it's a well-formed, correctly formed, program.

So the way that **processing** first happens before we execute, is that there is a stage where it goes through all of that **compilation**, It goes through all of that **parsing**, and it produces this **abstract syntax tree**. But it also produces, essentially, a plan for the **lexical environment**. That is, where all the **lexical scopes** are, and what marbles are gonna be in them, what `identifiers`.
It prepares this plan, and that is the `executable` code that is handed off to be executed by the other part of the JavaScript engine.

Now, some people would say, for example, `Java`, well, that's a **compiled language** because I compile it and then they distribute a bytecode. It turns out, JavaScript does almost the same thing.

There is a **parser** that parses through your JavaScript source code, and it produces, in essence, an intermediate representation not that dissimilar from a `bytecode`. And it hands it off to the **JavaScript virtual machine**, which is embedded inside of the same JavaScript engine. And it **interprets** all that stuff, and one of the things that it **interprets** is, whenever it enters a scope it creates all the marbles according to what the program told it to do.

> We have to think about JavaScript as a two-pass system rather than a single-pass.
>
> We are gonna process (**compilation** or **parsing**) the code first, and set up the scopes, and then we are gonna execute it.

> JavaScript organizes **scopes** with **`functions`** and **blocks** (ES6).

So we have **functions** and you have **blocks**, those are the units of **scope**.

## Compilation & Scope

In this exercise we try to think more like JavaScript engine:

```JavaScript
1.  var teacher = "Kyle";         // Red
2.
3.  function otherClass() {       // Red
4.    var teacher = "Suzy";       // Green
5.    console.log("Welcome!");
6.  }
7.
8.  function ask() {              // Blue
9.    var question = "Why?";      // Blue
10.   console.log(question);      // Blue (question is blue)
11. }
12.
13. otherClass();                 // Welcome!
14. ask();                        // Why?

scope
```

After we've set up all those plans
(**compilation** step which we have two actors on it, the **compiler** and **scope manager** ), then we'll come back and execute the code.

There's going to be two actors, two entities that are going to be talking. One is the **compiler**, the thing that's **processing** the JavaScript program.
The other one is the **scope manager**, that makes buckets, makes marbles, drops the marbles in. It's the **compiler** who says, hey, I have this thing, and it's the **scope manager** who says, I'm gonna make a plan for that, I'm gonna make a plan for a bucket and make a plan for a marble.

Remember, in our **processing phase** (**compilation phase**), we have a **scope manager** and we have a **compiler**.

Just for the simplicity of our discussion we use the three primary colors. (red bucket represents the `global scope`, blue and green for wherever we have inner buckets or scopes)

On line 1, the **compiler** see a `var` declaration.
That's a formal declaration. In other words, we're creating a marble. So the **compiler** asks the `global scope` (red bucket), I have a marble here, have you ever heard of this thing called `teacher`?

And the **compiler** asking this question because if the **scope manager** (here `global scope` or red bucket) already knows about an `identifier` of the name `teacher`, it doesn't need to do anything. That's a no-op (no operation).

> (In **processing phase** or **compilation phase**) there's no such thing as redeclaration.

But in this particular case, since it's the first time that the **compiler** would have asked the `global scope` about a `variable` called `teacher`, then the `global scope`'s gonna say, nope, never heard of it. But I've created now a red marble for you and, now we just dropped it into the red bucket.

> It (the `variable`) hadn't actually been created, that they don't get created for real until **execution**, but conceptually we're creating this plan from what we see in the program.
>
> Actually what we're (the **compiler**) doing is looking for these formal declarations.

Sometimes the formal declarations look like on line 1 `var teacher`, sometimes they look like line 3, `functions` or another kind of declaration. `Functions` make a marble, in this case, the marble on line 3 would be called `otherClass`. And that needs to get added to some scope.

So the compiler, on line 3, ask the **scope manager**. I found another formal declaration (`function`), in this case for a marble called `otherClass`. Have you ever heard of that `identifier` before? Nope, never heard of it, but here's another red marble because we're still in the `global scope`.

So now we have two red marbles in the red bucket. That takes care of the `identifier otherClass` as well as the `identifier teacher`.

> When the **compiler** realize a `function`, it knows `function` makes scopes (buckets). So when it sees a `function` create both marble and bucket.

Now, the **compiler** is smart enough to realize, oh... that's a special kind of thing, because a `function` makes scopes, it makes buckets.

So, the **compiler** tells the **scope manager**, we're gonna need another bucket. Now we're creating a bucket inside of a bucket, so like a giant barrel and then a tiny little bucket inside.

So **scope manager** says, sure, now we got a blue bucket, and now we are talking about the blue bucket, and let's step into that `function` and talk about it as its own scope, since that's `functions` creates scopes.

So (into that `function`) line 4 has another formal declaration, in this case for a `variable` called `teacher`. So we're gonna say, hey blue bucket (the **scope manager**) for this `otherClass` I have a formal declaration for a marble called `teacher`, ever heard of it? Now the answer to that question might surprise us because we might think, sure, we heard about it on line 1! But remember we're talking to an entire different bucket now. We're talking to the blue bucket, not the red barrel (the `global scope`).

So we're saying hey, blue bucket, do you have a marble called `teacher`? And what's the **scope manager** going to say? no so here's your blue marble.

Okay, so now there are two marbles in two separate buckets of two different colors, even though they have the same name.

> Having two `variables` with the same name at different scopes, that has a term, it's called **shadowing**.
>
> Because we have two `variables` with the same name there's no possible way that we can reference **lexically** the `variable` from out side a inner scope. It just limit us from what we can access. Because those `names`, it would match the nearest one (`identifier`).

Sounds like sort of like evil or whatever, there's nothing bad about it, **shadowing** is entirely okay. But there is an offshoot of **shadowing**, which is, now that we've created a `variable` called `teacher`, we've created a blue marble called `teacher` in that `otherClass scope`. Well, now there's no possible way that we can reference **lexically** the `variable` from line 1.

We can't reference the red marble because now there's a blue marble of the same name, because of the **shadowing**. It's totally okay, but it does limit us from what we can access. Because those `names`, it would match the nearest one, which in this case would be the blue marble.

Because we see no more formal declarations. So we're finished with the `otherClass function`. Now we step back out to the red scope (`global scope`).

But why did we skip over console.log in line 5?

We didn't skip over it from the perspective of **compilation**. The **compiler** would have certainly handled line 5 and done a bunch of **compilation**. It doesn't have any impact on our scopes. So we've narrowed our focus of **compiler** theory to preparing our `identifiers` and our scopes. Doing our marble bucket sorting thing. So we're just skipping over the uninteresting details at this point. Since line 5 doesn't create or access any `variables` within our scopes, we don't need to worry about it.

> At this stage (the stage before **executing**), the **compiler** preparing our `identifiers` and our scopes.
>
> So we're just skipping over the uninteresting details at this point. Since it doesn't create or access any `variables` within our scopes, we don't need to worry about it.

In the `global scope` we find the next formal decoration on line 8 (the formal `function` declaration for an `identifier` called `ask`). Again the **compiler** aske the `global scope` (red bucket), I have a formal declaration for an `identifier` called `ask`, ever heard of it? Nope, never heard of it. But here's red marble. There's a red marble, because we are in the `global scope`. And that's what color marble matches with the red bucket.

All right, so we just made a red marble called `ask`. And the **compiler** notice that's attached to a `function`. So **scope manager**, make a green bucket (another scope) for us, and now we step into the scope of the green bucket, the `function` labeled `ask` here.

And the **compiler** find the formal declaration (a `var` declaration) on line 9. And it asks the **scope manager** (the green bucket, or scope of `ask`) I have a formal declaration for an `identifier` called `question`, ever heard of him? Nope, but here's your green marble. So we get a green marble and we drop it in the green bucket.

Now, this is critical, because you'll notice on line 10 we have a reference to an `identifier`. This isn't creating marbles, so in this **processing step** we're not gonna worry so much about creating it. But we are going to have to understand where that marble comes from, what color marble that is, when we execute the code in the next step.

> Even if the **compiler** sees the reference to an `identifier` (not creating it) the **compiler** needs to find out where that `identifier` comes from (which scope) for when we execute the code in the next step.

So it's critical that we do this marble sorting correctly as we process the code the first time. So we're done with the `ask function`. There's no more formal declarations. And then we step back out to the `global scope`. And we don't find any more formal declarations in the `global scope`.

So that **compiler phase** of this code is finished. And what we are left with is a plan for all the buckets and all the marbles. We've accounted for all the scopes that exist and where all the `identifiers` fit into all of those. Including references to them like on line 10, we know what color marble that is.

Now it's important to note that when we execute the code, there's no more declarations for anything. All the `var`s are gone, essentially, because we don't need to declare anything anymore. We already know what that's gonna do, because we figured that stuff out at **compile-time**.

> In the **compiler phase** we create a plan for all the scopes that exist and where all the `identifiers` fit into all of those scopes. Including references `identifiers`.
>
> That is then handed over as part of the **execution** plan so that the **virtual machine** (the JavaScript engine), can run this code.
>
> And when we execute the code, there's no more declarations for anything, because we figured that stuff out at **compile-time**.

**It is very important to know:**

> In a **lexically scoped language** (like JavaScript), all of the scopes (**lexical scopes**) and `identifiers`, that's all determined at **compile-time**. It's not determined at **run-time**. It is used at **run-time**, but it is determined at **compile-time**.

And why that matters is, that allows the engine to much more efficiently optimize, because everything is known and it's fixed. Nothing during the **run-time** can determine that this marble is no longer red, now it's blue. Once we've processed through, we already know what color marble it is and we're done with that discussion.

> This means the decisions that we've made about scope are author time decisions.
> When we write a `function` or put a `variable` here, it means that `variable` is always gonna be that color marble (inside that scope).

So that allows the JavaScript engine to be much more efficient at its job. The takeaway that you should have from that is, the decisions that I've made about scope are author time decisions. When I write this `function` and we put this `variable` here, it means that `variable` is always gonna be that color marble.

## Executing Code

We have all our plan set up. There's no more declaration code, so let's then execute this code:

```JavaScript
1.  var teacher = "Kyle";
2.
3.  function otherClass() {
4.    var teacher = "Suzy";
5.    console.log("Welcome!");
6.  }
7.
8.  function ask() {
9.    var question = "Why?";
10.   console.log(question);
11. }
12.
13. otherClass();                 // Welcome!
14. ask();                        // Why?

scope
```

> So even though line 1 looks like one statement, it's actually two separate things. There's the `var teacher` part. That's what the **compiler** handles. And then there's the `teacher = "Kyle"` part, that's what the **execution engine's** gonna handle.

Now, remember there's only two roles that a `variable` can play. It can either be the **target** of an assignment, and we can see on line 1, the `teacher` is in that role, it is receiving an assignment, it's the **target**. The only other role that it can play is in the **source position**.
It's going to give up its `value`. We're going to **retrieve** the `value` from it. That's like what we see on line 10, `question` is in that position.

We used to refer to this (in an old school terminology) as a **left hand side**, an **LHS** and a **RHS**, a **right hand side**. As in left and right hand of an equals. Nowadays, you might call it an **L value**, an **R value**. Maybe just for simple purposes, let's call them **source** and **target**.

All `variables` can play two roles in our program:

1. Be the **target** of an assignment (receiving an assignment of `value`, or in an old school terminology: **left hand side** (**LHS**), as in left hand of an equals)
2. Be in the **source position** (retrieving the `value` from it, or in an old school terminology: **right hand side** (**RHS**), as in right hand of an equals).

**let's call position of `variables` simply source position and target position.**

Here on line 1, `teacher`'s in a **target position**. On line 10, `question`, is in a **source position**. That's a piece of information that the **compiler** picked up. Whenever it created that whole `abstract tree` and all that, it knew that each `identifier`, each marble, not only what color it was, but what position it was in.

> The **compiler** in **compile-time** (**processing** or **compilation** or **parsing phase**), whenever it created that whole **abstract tree** (**abstract syntax tree**), it knew that each `identifier` (each marble) not only what color it was (not only that it exist), but what position it was in.

It's critical to know that we're dealing with something that's receiving a `value`, it's a **target**, or we're gonna **retrieve** it's `value`.

So let's then execute this code the way the JavaScript engine would, with all that **execution** plan and scope stuff set up.
Each time we enter a scope, all of the plans for the buckets and the marbles will have been created, and so now we're ready to go ahead and execute.

> In **execution phase** we have two people talking, **scope manager**, cuz it's gotta hand out the `identifier` (marbles), and the **virtual machine** (it's the JavaScript engine that's executing code).

We have two people talking, again. We still have the **scope manager**, cuz he's gotta hand out the marbles. But we also now have a different person talking in the conversation.
And that's the **virtual machine**, it's the JavaScript engine that's executing code.

So that conversation on line 1, remember, there's no `var` there, so the **virtual machine** (JavaScript engine) asks the **scope manager**, I have a **target** reference for a `variable` called `teacher` (You see in line 1, it's a **target** reference, it's receiving an assignment). Have you ever heard of this marble (`identifier`) called `teacher`? We're talking to the red bucket (`global scope`), remember. And it can only answer yes or no. So the **scope manager** says: yes, why? Because at **compile-time** we formally declared it.

Had we not formally declared it, things are different. But because at **compile-time** we formally declared something called `teacher`, now we know, yes, here's your red marble. So it's fundamentally a lookup. We're saying, hey, red bucket, do you have a marble called `teacher`? If so, please give it to me. And here we get the red marble.

And what are we gonna do with that marble? Well, we're gonna assign a `value` to it. That marble represents an area in memory that needs to get assigned to, essentially. So, we're gonna take the `value` from the right hand side, in this case, the `string "Kyle"`, and assign it to that location represented by the red marble `teacher`.

> In **run-time** (when we executing our code) if there were `identifiers`, we assign the `values` to `variables` (if they were in `receiving position` or **target**), or `retrieving` the `values` from it (if they were in **source position**).
>
> And also in **run-time** the `variables`, does not have a `var` on them anymore.
>
> That `identifier`, represents an area in memory that needs to get assignment of `value` (So, we're gonna take the `value` from the right hand side, and assign it to that location represented by that `identifier`).

Line 3 through 6, and line 8 through 11, those were declarations, they're not there anymore. They've been **compiled** away. So, execution would move to line 13. Let's talk about how line 13 `executes`.

> In **compile-time**, the **compiler** declared all `identifiers` in line 3 through 6, and line 8 through 11, so in **run-time** (**execution phase**) we move to line 13.

Don't skip just to how the `function` runs inside. We first have to ask how the line 13 itself `executes`. Cuz JavaScript has gotta figure out that and execute it.

> The `otherClass identifier` (what we see in `function` call line 13), is in **source position**.
> We're not assigning something to `otherClass`. So if we're not assigning to it, we are pulling a `value` out. We wanna know what is currently in the `identifier otherClass`. Because, in just a moment, we're about to turn the `executor`. So we need to go get it.

Now the **virtual machine** (JavaScript engine), ask `global scope` (**scope manager**), I've got a **source referenc** for a marble (`identifier`) called `otherClass`. Have you ever heard of it? Yes, and it give us the marble and the **virtual machine** pulls the `value` out from that location and memory conceptual, but the question is what is that `value` at this moment? It's the `function` that `otherClass` was declared to point at.

> When we see an `identifier` for a `function` in **run-time**, the `value`
> of that `identifier` is the `function`, that `identifier` was declared to point at.
>
> At **compile-time**, the `identifier`, was associated with that `function`. That reference to that `value` is there.
>
> In the **run-time** the `identifier` that was declared to poit at `function` (`otherClass()`) with those parentheses `()`, they execute the thing that we just pulled out of that `variable`.
> And if we pulled out of that `variable` something that wasn't a `function`, like, `null`, or `undefined`, we get an error called a `TypeError`, because we would have a `value` like `null`, or `undefined`, but that is not a `value` that is legal to execute as a `function`.
>
> So we're doing something illegal with that `type`, that's call the `TypeError`, a **run-time** error, where we're doing something with a `value` that's not allowed for that `type`.

So that's a good thing, because then, on line 13, those parentheses `()` that we see there on line 13, they execute the thing that we just pulled out of that `variable`.

In this case, because we got a `function`, so we can execute it, and moves to what 4.
Line 4 does not have a `var` on it anymore but because of an assignment it does have, **target** operation.

So that conversation is gonna continue exactly like it did on line 1. Hey, scope of `otherClass` (blue bucket), I have a **traget reference** for a marble called `teacher`, ever heard of it? Yes. Here's your marble. So now we have a location in memory, that's a different place than the one from line 1. It's a different location in memory, cuz it's a different scope.
We have this marble, and then we take the `value "Suzy"`, and we assign into it, then line 4 is executed.

> The `console.log` is an `identifier` (and it's in **source position**) that we didn't created, but it is an `identifier`. The JavaScript engine has that available to us as sort of like an `auto global` (it's already in `global scope`). And when it find it (in `global scope`), and look at the `value` of it, it's an `object`, it has a `method` call `log`, so it can `invoke` it.
>
> If JavaScript engine look up for `identifier`, first asks for it in current scope if it wasn't there, it goes up one level until it gets to `global scope`, that's call the **lexical scope**.

Now in line 5, `console.log`, there a reference to `identifier`. Not one that we created, but there is an `identifier` (a marble ) on the list. Which is call `console`, that's got to exist somewhere. The JavaScript engine has that available to us as sort of like an `auto global`. Not something we created, but it's available nonetheless.

So the same process, we would basically say, hey scope of `otherClass`, have you ever heard of a marble called `console`? Cuz that's in the **source position**. Now, has `otherClass` ever defined a marble called `console`? No, so what's going to happen then, and this is the key to understanding **lexical scope**, is that if we cannot find a `variable` that is referenced within the scope, that was declared within the scope, we go up one level, in this case to the `global scope`, and we would say, hey, `global scope`, have you ever heard of an `identifier` called `console`? And even though we didn't formally declare it, it's an `auto global`, it's already there, there's already a red marble called `console`.

So what is `global scope` gonna do? Here's your red marble, and we can look on that marble. It's an `object`, it has a `method` call `log`, and so we can `invoke` it.

> When we reference a `variable` in a **source position**, we have to first look it up, and when you reference a `variable` in a **target position**, we also have to first look it up.
>
> So there is a look up process involved.

---

There is a very important takeaway that we should have:

> We discover the **source** versus **target** position at **compile-time**, but we don't use that information until **run-time**.
>
> Basically think of it like a plan (map), it's like a to do list for every scope, that's the **executable code** (what **compiler** outputs). And then when it runs is when it all actually comes into existence.
>
> Every time we execute a `function`, the environment is recreated from scratch.
>
> So the **compiler** output is not actually reserved **memory**, it's the plan for how to reserve **memory** and make `identifier` (marbles) and all that. And then every time we execute that plan is effected into **memory**.

---

We have finished executing line 5, and execution moves back. Now we're on line 14.

The JavaScript engine find a **source reference** for `identifier` called `ask` on line 14. And asks **scope manager** (here `global scope`), do you know about an `identifier` called `ask`? Yes, here's location in memory for that `identifier` called `ask`. so we go to that `identifier` (that location in memory) and it have a `function` (green bucket, the things we declared on line 8), in it.

So now we have a `function`, and that open close parenthesis **executes** the `function`. Now execution moves to line 9. Now, there's no more `var`. And now the JavaScript engine asks, green bucket (scope of `ask`), I have a **target** reference for `identifier` called `question`, ever heard of it? Yes, here your `identifier` location in memory. So we don't care what's in it right now. We go and get the `value` from the right-hand side (`"Why?"`), we assign into that location of memory and line 9 is finished.

Now, line 10. We remember how the `console` works. So the JavaScript engine doesn't find `console` on that scope, it goes to the `global scope`, finds it, finds a `method` called `log`. But before it can execute `log`, it's gotta figure out what's being passed to it. So in line 10, it's a reference to another marble. So we gotta look that up. Again it asks green bucket **scope manager** (scope of `ask`), I have a **source reference** for `identifier` called `question`, ever heard of it? Yes, here's your green marble.

And now, because we're not assigning to it, now we want the `value` that's currently in it.
So we go to that area of memory. We pull the `value` out, which happens to be the `string` (`"Why?"`) in this case. And that `argument` is assigned to the `parameter` that `console.log` is receiving. So `arguments` (the things that we pass in) get assigned to the `parameters` that receive them.

> An `argument`: is a `value` passed to a `function` when the `function` is called. Whenever any `function` is called during the **execution** of the program there are some `values` passed with the `function`. These `values` are called `arguments`. An `argument` when passed with a `function` replaces with those `variables` which were used during the `function` definition and the `function` is then executed with these `values`.
>
> A `parameter`: is a `variable` used to define a particular `value` during a `function` definition. Whenever we define a `function` we introduce our compiler with some `variables` that are being used in the running of that `function`. These `variables` are often termed as `Parameters`. The `parameters` and `arguments` mostly have the same `value` but theoretically, are different from each other.
>
> | ARGUMENT                                                                                                          | PARAMETER                                                                                                      |
> | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
> | When a `function` is called, the `values` that are passed in the call are called `arguments`.                     | The `values` which are written at the time of the `function` `prototype` and the definition of the `function`. |
> | These are used in `function` call statement to send `value` from the calling `function` to the called `function`. | These are used in `function` header of the called `function` to receive the `value` from the `arguments`.      |
> | During the time of call each `argument` is always assigned to the `parameter` in the `function` definition.       | `Parameters` are local `variables` which are assigned `value` of the `arguments` when the `function` is called |
> | They are also called `Actual Parameters`                                                                          | They are also called `Formal Parameters`                                                                       |
>
> [GeeksforGeeks](https://www.geeksforgeeks.org/argument-vs-parameter-in-java/)

> The JavaScript engine before execute method (like `log` in `console`), first it's gotta figure out what's being passed to it (if what we passed in is an `identifier`).
>
> And that `identifier` we passed in to the `console.log` is in a **source position**, so we go to that area of memory, and pull the `value` out. And that `argument` (the `value` we pull it out and passed to the `console.log()`) is assigned to the `parameter` that `console.log` is receiving.
>
> So `arguments` (the `values` that we pass in) get assigned to the `parameters` that receive them.
>
> We can think about a `parameter`, in a `function` definition, like a `variable` that is in **target position**.
> And `argument` (if we have an `identifier` to passed in to the `function` call) is in a **source position**.

We can think about a `parameter`, like if I had `function ask()` and it took a `parameter`, the `parameter` is a **target** reference (`variable`). And if we have an `identifier` in an `argument` position like in line 10, is a **source reference**.

And in the end, it becomes something like this:

![Scope](https://user-images.githubusercontent.com/37678729/72377060-50abad00-3724-11ea-9d5c-408ee5c5cd76.png)

---

### Summary:

The JavaScript is not an **interpreted language** that it goes step by step one line at a time. But rather, we should think about JavaScript as a two pass **processing**.

First pass, we could call it **compilation** (or **parsing**), we're gonna go through the entire code. And there's lots of things that happen during the **parsing** and **compilation**, but the main thing that happens is all of the scopes (all of those colored marble buckets, the plan for those all) get created, we figure out where all the scope boundaries are.

And indeed, all of the `identifier` references, those are color coded as marbles. And we'll use that information about the color of the marble and what bucket it comes from, in the second pass when we execute code.

> The first pass is **compilation**, and the second pass is **execution**. In the **compilation phase**, it's the **parser** (or **compiler**) and the **scope manager** , And in **execution phase** (**run-time**), we have **JavaScript virtual machine** (or JavaScript engine) and the **scope manager**.

> **JavaScript engine** = **execution engine** = **virtual machine** = **VM**

The **parser** in **compilation phase** looking for formal declaration (like `var` or `function`), and if the formal declaration there was before, it do nothing. And if there wasn't, the **scope manager** will create that `identifier` on that scope.

> The reason for that chaking or question is, if we'd seen a formal declaration (like `var`) of same `identifier` a couple of times in the same scope, we don't need to make multiple `identifiers` (marbles) with the same name.
>
> It only needs to be created once for the same scope, and there really is no such thing as recreating it.

> If the `identifier` pointing to a `function`, besides creating its `identifier`, it also creates a new scope.

> Whenever an `identifier` is not in a **target position** (receiving an assignment of `value`), is in **source position**.
> If it's not **target** receiving something, it must be a **source**, cuz that's the only two options.

---

### Dynamic Global Variables:

Another example with corner cases:

```JavaScript
1.  var teacher = "Kyle";
2.
3.  function otherClass() {
4.    teacher = "Suzy";
5.    topic = "React";
6.    console.log("Welcome!");
7.  }
8.
9.  otherClass();                    // Welcome!
10.
11. teacher;                         // "Suzy"
12. topic;                           // "React"

Scope
```

First Step: **compilation**

> In **compilation stage** we looking for formal declarations and create `identifiers` for them, but in **execution stage** we see that `identifiers` is in **source position** or **target position**.

So **processing** begins with the **compiler** talking to the **scope manager**. And the **compiler** says on line 1, hey **scope manager** (`global scope`), we have a formal declaration for `teacher` (in **execution** it will be a **target reference** but, during the **compilation**, we have a formal declaration), ever of it? No, so we go ahead and create red marble.

> We will heard of `teacher` in **execution**. but in **compilation stage** we'd never heard of it.

In line 3 we have the formal declaration. Hey `global scope`, ever heard of formal declaration for an `identifier` called `otherClass`? No, and here's your red marble (and goes in the red bucket).

And because it is a `function`, It needs its own bucket, so the **scope manager** creates blue bucket.
And now we step into that scope and make its plan. And because there is no `var` there, we don't make any blue marbles.

> If in **compilation stage** there is `identifier`, but without formal declaration (such as `var` or `function`), the **scope manager** didn't create `identifier` for it.
>
> But in **execution stage** the JavaScript engine, will go up one level until it gets to `global scope` and if it didn't find it there too, it will `auto global` it, which means it will create that `identifier` in `global scope`.

So from the perspective of the **compiler**, we didn't have any formal declarations, so there's no marbles to create. So we're done with the blue bucket. Effectively it exists, but effectively it has no marbles in it.

Now, we step back out to the `global scope`. And there is no more formal declaration. So we're done with **compilation**.

Second Step: **execution**

So we remember, there's no formal declarations but there are executable statements. Line 1 has an executable statement. And so the JavaScript engine says, hey, `global scope`, I have a **target reference** for `teacher`, ever heard of it? Yes, here's your red marble. So we take the `value` `"Kyle"`, we assign it into the red marble (`teacher`), and we are complete.

Now remember, `functions` are declarations, they don't really exist. At least, they're only here symbolically. So we're going to move from the `function` to the next execution line of code, which would be line 9.

> In **execution stage**, because `functions` and other kind of formal declarations aren't exist (they're only there symbolically), the JavaScript engine skip them.

So in line 9, hey `global scope` (the red bucket), I have a source reference for an identifier called `otherClass`, have you ever heard of it before? Yes, Here's your red marble. And in red marble is a reference to the `function` which we've called `otherClass` here which has attached to it this blue bucket of scope. So we find that `function`, we execute it with the parenthesis `()` on line 9, and execution moves to line 4.

Now on line 4, hey, blue bucket, I have a **target reference** (because it is receiving an assignment) for `teacher`, ever heard of it? No. So we go up one level. Hey, `global scope`, I have a **target reference** for `teacher` ever heard of it? Yes, here's your red marble (**Important to see that it's a red marble here not a blue marble**).

Even though we're inside of the blue scope, we are referencing a red marble. So we get a red marble and when we assign `"Suzy"` to it, we are assigning over the `value` that was currently there (`"Kyle"`), because it's the same marble in this case. This wasn't `shadowed` because we didn't declare `teacher` inside of the `otherClass function`.

![Scope and Corner Cases](https://user-images.githubusercontent.com/37678729/72376864-f3176080-3723-11ea-8057-f7f63011b6fc.png)

Moving on then to line 5. And, it's gonna process exactly the same, same questions. Hey scope of `otherClass`, I have a **target reference** for the `identifier` called `topic`, ever heard of it? No, so we go up one level to `global scope`. A `global scope`, I have a **target reference** for the variable called `topic`. Ever heard of it?

Here we see one of the historical bad parts of JavaScript which is in the early days to be as forgiving as possible for people that didn't understand the language, they instituted this idea of `auto global`'s. So if we try to assign to a `variable` that's never been formally declared. Once we arrive at the `global scope`, if we say hey, `global scope`, we're looking for this marble called `topic`, ever heard of it? And the `global scope` instead of saying nope, sorry error, the `global scope`'s gonna say I just created one for you. And it's gonna hand us a red marble, not a blue marble. Why do we suppose it only hands us a red marble and not a blue marble? Cuz we're talking to the `global scope` now. We've already passed up the scope where that would have been formally declared and now we're talking to the `global scope` and it's the `global scope` that gives us the `variable`.

> If we try to assign to a `variable` that's never been formally declared (on that scope). Once we go up one level and arrive at the `global scope`, if there wasn't `variable` with that `identifier`, the `global scope`'s gonna created it for us (in a **run-time** or **execution phase**), we call this idea `auto global`.
>
> Even when we wrote that `identifier` in the first place inside another scope than `global scope`, because the `global scope` created and gives us that `variable`, it is a `global scope` variable (`red marble`).
>
> And we should keep it in mind creating a declaration at **compile-time** and creating it dynamically during the **run-time**, have differences. There are performance differences and other sorts of things but mechanically they are two `global variables` at this point.
>
> Never ever under any circumstances did you intentionally `auto-create global`'s like that. Always declare the `variables` that we want to use declare them in whatever scope we need them in, but don't `auto-create` them.
>
> But we should keep it in mind, if we have `non-declared variable` without assigning to some `value`, we still get `ReferenceError` when we execute the code.

So we've created an `auto global` called `topic` which that sounds terrible. But now there's a `global variable` called `topic` and when we get that red marble back and make the assignment on line 5, there's a `global variable` now with the value `"React"` in it.

**Q:** Do the `auto global` would occur also if `topic = "react"` were in the `global scope` under `variable` of `teacher` (for example it was in line 2)?

**A:** That's true, any assignment to a `variable` that is undeclared at that moment, not declared to any scope we have access to. Any undeclared variable is going to end up creating this `auto global`.

Now, the reason why that happens is because we'll notice that this program is not running in `strict mode`. But this is running in the `non-strict mode` or sometimes called, `sloppy mode`. We should be using `strict mode`, and if we were using `strict mode`, we wouldn't see this behavior. But since this code snip it isn't, that's what happens, as we end up creating a `global variable` called `topic`.

> If we use `strict mode` instead of using `non-strict mode` (or sometimes called `sloppy mode`), we wouldn't see this `auto-create global`.

We execute `console.log` the same way as we have before. So execution is then done in the `function`. Execution moves to line 11. So hey `global scope` (red bucket), I have a **source reference** to a `variable` called `teacher`, ever heard of it? Yes, so we go get that marble and we look for its `value`. And the `value` is `"Suzy"`. Because of line 4, we've overwritten the `value` in that `variable`. It's not a separate `variable` (marble).

So line 12 then, hey `global scope`, I have a **source reference** for `topic`, ever heard of `topic`? Yes, here's your red marble. and the value of that red marble is `"React"`.

**Q:** If line 11 was actually in line 8, `teacher` would still be `"Kyle"`?

**A:** Yes, it's correct.

**Q:** What would happen if line 12 was on line 8?

**A:** We will get `ReferenceError`. There would be no `identifier`. But because there is an `identifier` on line 12 the `function otherClass` auto created it by assigning to a `non-declared variable`.

### Strict Mode:

```JavaScript
1.  "use strict";
2.
3.  var teacher = "Kyle";
4.
5.  function otherClass() {
6.    teacher = "Suzy";
7.    topic = "React";           // ReferenceError
8.    console.log("Welcome!"):
9.  }
10.
11. otherClass();

Scope
```

So now if we flip on `strict mode`, which we do by putting this `strict mode` fragment at the top of any scope, preferably at the top of our program, the top of our file, or at the top of any `function`.

If we turn on `strict mode`, all the same processing is going to happen. But when we arrive at line 7 and we say, hey scope of `otherClass`, I have this **target reference** for `topic` ever heard of it? No, so we go to the `global scope` and we say, `global scope` have you ever heard of `topic`, and the `global scope` gonna respond with `ReferenceError`, never heard of it.

> Whenever we are in `non-strict mode` (or `sloppy mode`), and we have a **target reference** for a `non-declared variable`, we will `auto create` it (`auto global`) in **run-time**.
>
> And if in `non-strict mode`, we try to get `non-declared variable` that is in a **source position**, `global scope` throw us a `ReferenceError`.

> If we have any kind of reference (**target** or **source**) for `non-declared variable` in **`strict mode`**, `global scope` throw us a `ReferenceError` (instead of `auto create` it, if `non-declared variable` was in **target position**).

That's what we would like to happen all of the time. It now happens as the result of `strict mode`, we get a `ReferenceError`. So one of the many, many reasons why it would be good for us to be using `strict mode`. It will avoid mistakes like line 7, cuz it almost certainly a mistake, it should not be something we intentionally try to create `global`.

> The difference between `TypeErrors` and `ReferenceErrors` are , `TypeErrors` are when we found the `variable` and the `value` that it is currently holding, is not legal to do whatever we're trying to do with it. Like trying to execute or access a `property` on an `undefined` or `null`, things like that, that's `TypeError`.
> A `ReferenceError` is, when JavaScript engine couldn't find that `variable` on any scope that we have access to.

**Q:** Is `strict mode` always pretty on `ES6`?

**A:** The `strict mode` is not always on. It's true that tools like `babel` and other transpilers basically always put the `strict mode` in there for us.
So if we're using transpired code, it was almost a virtual certainty that our code is running in `strict mode`. But JavaScript is not default the `strict mode` because then that would break a bunch of programs. So that were written 20 years ago or something. So because of backwards compatible, we will always have to opt into `strict mode`.
Another little caveat inside of certain kinds of mechanisms within `ES6` and going forward, it is assumed to be in `strict mode`. So we don't even type it, so inside of a `class`, or inside of `ES6 module`, inside of both of those, `strict mode` is assumed, we don't even have to put the `strict mode`, is just assume that, that code is running in `strict mode`. But as a general rule for JavaScript itself, it's not in `strict mode` unless we opt-in.

> In the `babel` and other transpilers, basically always put the `strict mode` in there for us.
>
> By default `ES6 class` and `ES6 module` are executed in the `strict mode`.

So in `stric mode`, we can't auto create `variables`, you have to declare them.

### Nested Scope:

This is an example of a nested scope:

```JavaScript
1.  var teacher = "Kyle";
2.
3.  function otherClass() {
4.    var teacher = "Suzy";
5.
6.    function ask(question) {
7.      console.log(teacher, question);
8.    }
9.
10.   ask("Why?");
11. }
12.
13. otherClass();                   // Suzy Why?
14. ask("???");                     // ReferenceError

Scope
```

We start by looking at line 1, that's a formal declaration that makes a red marble. Line 3 is a formal declaration that also creates a red marble. Line 4 and 6 both are the formal declaration that create a blue marble.

And now, we are inside of `ask`, in line 7, there is no marbles, but there are reference to marbles. So on line 7 when we reference `teacher`, it's a blue marble. And the `question` in line 7, is a green marble. Because the `question` is a `variable` inside of the scope of the `function ask`.

> The `function declarations` make their `identifier` in their enclosing scope.

> We shoud know the `parameters` are `variables` inside of the scope of that `function`.
>
> Even though it doesn't have a `var` there, a `parameter` is formally creating an `identifier` in that scope.

So we'd have a `teacher` being a blue marble and `question` being a green marble.

The `value` of the `question` is `"Why?"`, because on line 10, when we execute line 10, we have a `value` there which we might have had to look up, but in this case it's a literal. And the `"Why?"` gets assigned to the `variable question`, so that execution happens at line 10 before `ask` starts executing.
So when we then ask for the marble that we find, which is a green marble and we ask for the value of `question`, it the `"Why?"`.

> Whenever our `function` have `arguments`, the `values` of that `arguments` will be assigned to the `parameters` of that `function` first, and then the `function` itself will be executed.

What's gonna happen with line 14? How does line 14 execute?
Hey `global scope`, I have a **source reference** for an `identifier` called `ask`, ever heard of it? No, so we have a `ReferenceError`.

Even though `ask` exist within the program, it doesn't exist in any scope that we have access to at this moment. So it is therefore an `undeclared variable`. Because we're not in `strict mode`, unlike `auto globals` which go ahead and create a marble for us, if we have a **source referenced** to a `variable` that is `undeclared`, it always throws a `ReferenceError`. That's why it's critical to understand the difference between a **target reference** and a **source reference**. At least if you're in `non-strict mode`.
Once you go to `strict mode`, they both behave exactly the same, so it stops mattering as much **target** versus **reference**.

> If in `non-strict mode`, we have a **source referenced** to a `variable` that is `undeclared` (it doesn't exist in any scope that we have access to at that moment), it always throws a `ReferenceError`. And if the `variable` is in the **target reference**, the `global scope` will be auto created it (`auto global`).
>
> But in `strict mode`, it doesn't matter `undeclared variable` is in the **source position** or the **target position**, the JavaScript engine will always throw us a `Reference Error`.

**Q:** When we're passing the `"Why?"` to `ask` on line 10, is there behind the scenes, is JavaScript saying `var question = "Why?"` at some point?

**A:** Sort of, I think conceptually it works to think about `arguments` being assigned to `parameters`. It's not technically that, but it's close enough that it works for this particular narrative exercise.

![Nested Scope](https://user-images.githubusercontent.com/37678729/72377177-818be200-3724-11ea-96ed-bc31666bec80.png)

---

### Undefined vs Undeclared:

What is the difference between `undefined` and `undeclared`?

**Undefined:** means a `variable` exists but at the moment it has no `value`. It may have never had a `value` or it might have used to have a `value` and it doesn't anymore but there is no other `value` and the vacuum of space it is `undefined`.

**Undeclared:** is actually never formally declared in any scope that we have accessed to (we do not have a marble for it). And in `strict mode`, it always results in those `ReferenceErrors`.

Unfortunately, JavaScript tries to mess around with this and pretend that `undeclared` is sort of the same thing as `undefined` and some of its error messages and other outputtings and things and not is historically a really bad idea. We need to keep these two concepts separate. There's a different something being `undeclared` (doesn't exist), and being `undefined` (definitely it does exist, but doesn't have a `value`).

---

## Scope & Function Expressions:

### Function Expressions:

We've been talking about `functions` in the **compilation phase**, adding their `identifier` (as a colored marble) in the enclosing scope.

```JavaScript
1. function teacher() { /* .. */ }
2.
3. var myTeacher = function anotherTeacher() {
4.    console.log(anotherTeacher);
5. };
6.
7. console.log(teacher);
8. console.log(myTeacher);
9. console.log(anotherTeacher);                   //ReferenceError

Scope: which scope?
```

In this example `teacher` in line 1 (`function declaration`), creates a red marble but `anotherTeacher` in line 3 (`function expression`) creates a blue marble.

In line 3 `identifier` called `myTeacher` creates red marble. We do know that there's a `function` called `anotherTeacher` there, so we need to create a bucket for it at least. We need to make a blue bucket. But because that `function` is not a `declaration`, we're not gonna handle its marble color in the same way. The key difference here is that the `anotherTeacher identifier` (line 3), is going to end up as a marble at **compile-time** but it's gonna be a different colored marble than we expect. It's not a red marble, it's a blue marble. So the blue scope is where the blue marble `anotherTeacher` ends up.

> One if the key differences between `function declarations` and `function expressions`, is that `function declarations` add their name or `identifier` (they attach their marble), to the enclosing scope, whereas `function expressions` will add their name (marble) to their own scope.
>
> Another difference is `function declarations` are hoisted but `function expressions` did not hoisted.

That's why on line 4, we can reference in `anotherTeacher` because there is in fact a blue marble called `anotherTeacher`. But down on line 9, there is no `anotherTeacher`. so when we're out in the `global scope` and we asked `global scope` you ever heard of this red marble, it's gonna say nope, never heard of that red marble and what we're gonna get there, `ReferenceError`.

> So key difference, `function expressions`, put there `identifiers` into their own scope.
>
> There's a little, also, additional nuance which is not only does that blue marble show up in the blue scope but it's also read only.
> You cannot reassign `anotherTeacher` on line 4, to some other `value`.

### Named Function Expressions:

What is a named `function expression`? That means a `function expression` that's been given a name.

```JavaScript
1. var clickHandler = function() {
2.    // ...
3. };
4.
5. var keyHandler = function keyHandler() {
6.    // ...
7. };

Named Function Expressions
```

Why line 1 is a `function expression`?
Because it's not a `function declaration`.

> How do we know someting is a `function declaration`?
>
> If the word `"function"` is literally the first thing in the statement.
> So if it's not the first thing in the statement, if there's a `variable` or an operator or parentheses or anything, then it's not a `declaration`, it is an `expression`.

So on line 1, we see a `function expression`, but we see no name. So that's called a `anonymous function expression`, whereas the one on line 5 is a `named function expression`.

> We should always, 100%, zero exception, prefer the `named function expression`, over the `anonymous function expression`.

**Why we sould always, always use `named function expressions`?**

- Reliable `function` self-reference (the name produces or creates a reliable self-reference to the `function` from inside of itself).
  That is useful if we're going to make the `function` recursive, or that `function` is an event handler of some sort and it needs to reference itself to unbind itself. or it's useful if we need to access any properties on that `function object` such as its `length` or its name or other thing of that sort.
  Anytime we might need a self-reference to the `function`, the single only right answer to that question is, it needs to have a name.
- More debuggable stack traces. So automatically by naming our functions, we make our code and stack traces more debuggable.
- More self-documenting code. If we have a `function` that is anonymous, and we look at that `function` to figure out what that `anonymous function` is doing, we have to read the `function` body and we also probably have to read where it's being parsed.

In that previous example, when we had a `variable` name that was in the `global scope`, and then we had our `function expression`. Would it be more or less reliable for us to make a reference to a marble in our own scope that is read only, or to reference a marble in an enclosing scope that is not read only? If we wanted to reference ourself from inside of the `function`, and we had the choice between referencing ourself with a marble in our own scope that is read only (the name of our `function expression`) or self-reference ourself with the `variable` that our `function` had been assigned to in the outer scope, which isn't by default read only.

**Q:** Which one of those two would be the better self-reference?

**A:** The first one, the one that we own and that is read only is a more reliable self-reference. So if there's any possible chance remotely that you're gonna need a self-reference, it's best to go ahead and name it.
Even if you don't need it now, you might need it in the future, it's best to go ahead and name it.

**Q:** When we just name the `function expression` the same thing that we will name the `variable` of the scope above, is that a case of **shadowing**?

**A:** Yes, it is a case of **shadowing**.

> We should remember, the purpose of code is not to be convenient for us to type. The purpose of code is for us to communicate more clearly our intent.

**Q:** What happens if we assign an `anonymous function` to property, or to a `variable` or something, what happens to the name?

**A:** Well, it's still an `anonymous function`, it still doesn't have a **lexical self-reference** to itself. Could we reference the `variable` in the outer scope? Of course we can, but it's a little less reliable, a little less trustable, and a little less semantic.

In 99.9% of all cases where people use `anonymous functions expressions` is as callbacks.
They pass them in line directly to other `functions` (like .map, .then and etc.) and guess what? When we pass a `function expression` in a call position, there's no way to infer any name from it. So we don't get the benefit of that name inference, we have to assign it to a `variable` to get the name inference or to a property.
And if we're gonna go to the trouble to assign it to a `variable` why not just make it a `declaration`? Why write a `variable` and then have to write the name twice? So we should prefer `function declarations` with names. If we're gonna pass in a `function expression`, put a name on it just like we would have if it were a `function declaration`, give it the same name.

> Every `function` that exists in our program, every single `function` has a purpose. And if every `function` has a purpose, it means every `function` has a name.
>
> If we can not come up with a name for the `function`, it probably means that we don't understand that `function` yet. It probably means, that `function` is too complex (means this `function` is doing too much), and needs to be broken down into smaller pieces until such a time as those names are completely self obvious, and then put them there.

> The `arrow functions` are `anonymous`.
>
> So Kyle Simpson think we shouldn't use `anonymous function expressions`. And because `arrow functionds` don't have name, the only way that we can figure out what that `arrow function` is doing, is to read it's `function` body. But the name of the `funciton` can tell us the purpose of that `function`.

We can put lots of more information in there that semantically tells the reader of our code what its purpose is, that would not be obvious in the code (names like getPersonId or defaultPersonId).

So don't use `arrow functions` for this purpose. Later we'll see the one and only one exception that we have to the `arrow functions` rule, which is their **lexical** this behavior. But Kyle Simpson does not endorse or recommend or suggest using them as a general replacement for any `function`.

> `(Named) Function Declaration` > `Named Function Expression` > `Anonymous Function Expression`
>
> So the `function declaration` has some benefits over the `named function expression`. But then `named function expressions` have huge benefits over the `anonymous function expressions`.

### Lexical and Dynamic Scope:

> The idea that a compiler (a parser or a processor) is figuring out all those scopes ahead of time before being executed, that's what we mean by the concept of **lexical scopes**.

That's where that name even comes from, that **lex** shares the same root as the first stage of parsing, the **lexer**.

We have two type of **Scope** model:

- **Lexical Scope**: It's an author time decision and all the **scopes** (where the `function` and `variables` written) create in **compile-time**. So when we talk about **Lexical Scope**, we think about something that is **fixed** at author time and it's predictable, it is **not** affected by **run-time** conditions.

- **Dynamic Scope**: It creates in **run-time**. The `Bash script` is an example of **Dynamic scope** language and it makes sense because `Bach Script` is an **interpreted language** so it has no **run-time** term. So we can say the **Dynamic scope** is the opposite of the **Lexical Scope**, and it implies that the **run-time** (the dynamic) conditions of your program, are going to affect something about the **scoping**.

The vast majority of all programming languages in existence, and almost certainly all programming languages that we've ever worked with, are in fact **lexically scoped**.

The JavaScript is absolutely, definitely, 100% **lexically scoped**.

The **Dynamic Scope** is not very common at all, we generally only see this in a few old academic languages and maybe some different modes.

### Lexical Scope and Dynamic Scope in Action:

```JavaScript
1.  var teacher = "Kyle";
2.
3.    function ask(question) {
2.      console.log(teacher, question);
5.    }
6.
7.  function otherClass() {
8.    var teacher = "Suzy";
9.
10.   ask("Why?");
11. }
12.
13. otherClass();
14. // Output in Lexically Scoped languages: Kyle Why?
15. // Output in Dynamically Scoped languages: Suzy Why?

Lexical Scope and Dynamic Scope
```

We know that **Dynamic Scope** exists in some languages, but it does not exist in JavaScript. So this is a _theoretical_ example.

So in **Lexically Scoped** (JavaScript) language, when we execute the above code, we will see `Kyle Why?` in the console. Because we noticed the `function ask` (on line 3), is making reference to a `variable` called `teacher` that does not exist in its own scope. And we would normally think of it as resolving to that `teacher variable` (on line 1 which equals to `"Kyle"`) because that's how we think **lexically**.

But _theoretically_ in a **Dynamically scoped** language, it wouldn't even consult the **lexical scope** nesting, it would say, where was `ask function` called from? It was called from the scope of `otherClass` and it would end up resolving `teacher` to that `teacher variable` (on line 8 which equals to `"Suzy"`). So when the code execute we will see `Suzy Why?` in the console.

> The idea of a **Dynamic Scope** is the idea that a `function`'s references to its `variables` are depended upon where that `function` was called from. The same `function` called from 100 different places ends up giving 100 different answers to what the `variables` are.

> So in **Dynamic Scope**, it is scope that is determined based upon the conditions at **run-time**, whereas **Lexical Scope** is determined at **author-time** (or **compile-time**).
>
> In a simple way **Lexical Scope** is fixed and predictable.
> And **Dynamic Scope** is flexible.
>
> Even though JavaScript does not have **Dynamic Scope**, it does have a mechanism that gives us this same kind of flexibility.

### Usage of Function Scoping:

In the below, there is a **Name Collision**. And if for resolving the problem, we turn this code:

```JavaScript
var teacher = "Kyle";

// ..

var teacher = "Suzy";
cobsole.log(teacher);   // Suzy -- oops

// ..

cobsole.log(teacher);   // Suzy -- oops
```

Into this one:

```JavaScript
var teacher = "Kyle";

function anotherTeacher() {
  var teacher = "Suzy";
  console.log(teacher);   // Suzy
}

anotherTeacher();

cobsole.log(teacher);   // Kyle
```

We still have a **Naming Collision**, we just shifted it to a different variable name (`anotherTeacher`).

**Name Collision:** When two different entities used the **same semantic name** in the **same scope**.

There's a principle within **Software Development**, it's called the principle of **least exposure** or the principle of **least privilege**, that says this:
_You should default to keeping everything private, and only exposing the minimal necessary._

That's one of the core principles of software engineering, because it essentially sets up a defensive posture. And we can solve **three** main problems by using this defensive posture:

- If we hide something within a scope, or hide something on a namespace or do some other sort of hiding, then we reduce the surface area for **name collisions**. (so number one, we solve naming **collision problems**.)
- If we hide something, it means that somebody else can't accidentally of intentionally misuse that thing. If we expose it publicly, everybody can use it. (so number two, if we hide something, then we prevent other from accessing it.)
- If we hide something, like one of those implementation details, or like a variable name, we protect ourself for future refactoring. (so the third one, and probably the most core and important reason for this principle, it's often overlooked.)

But, right now, this usage of a `function` is not really accomplishing that task, because the problem is that now we have a `function` that has a name in that scope.
It has the name `anotherTeacher`. So we didn't really fix the **naming collision** problem at all, we just shifted it to a different variable name.

## Immediately Invoked Function Expression (IIFE):

This is true that in the previous example, we have a new scope, and we could contain any assignments of that scope, but we still have a **naming collision** problem, so we need a better way of using our knowledge of scope.

We can resolve this problem by using `function expression`:

```JavaScript
var teacher = "Kyle";

(function anotherTeacher() {
  var teacher = "Suzy";
  console.log(teacher);   // Suzy
})();

cobsole.log(teacher);   // Kyle
```

But probably we have seen this code with an `anonymous function expression` form.

Even if we put the called `function's name` inside the parentheses like this:

```JavaScript
(anotherTeacher)();
```

It may looks weird, but it still totally works. And the reason it's gonna execute is because it's gonna do the same thing as in the previous slide, which is to go get the value in that `variable` (all the values side the `anotherTeacher`) first, and then the second set the parentheses is gonna execute it.

So we create **IIFE**. That's where this comes from, using a `function expression` to create a scope, and immediately invoking it. We only need it for that moment, just so we can have a scope. So it runs once and then it kinda goes away.

**Q:** Why is this not a `function declaration`?

**A:** Because the word `function` is not the first thing in the statement.

In the below example we make `named function expression` to `anonymous function expression` and just pass in a value to a parameter instead of making an assignment.

```JavaScript
var teacher = "Kyle";

(function (teacher) {
  console.log(teacher);   // Suzy
})("Suzy");

cobsole.log(teacher);   // Kyle
```

## Block Scoping:

We can get the previous example (**function scope**) and turns it to **block scope** form.

```JavaScript
var teacher = "Kyle";

{
  let teacher = "Suzy";
  console.log(teacher);   // Suzy
};

cobsole.log(teacher);   // Kyle
```

The **Block Scoping**, it's scoping that's done with blocks (curly braces `{}`) instead of with `functions`.

When we use **Block Scoping**, same principle is gonna apply. We wanna put something inside of a block (instead of inside of the enclosing scope), because:

- We wanna hide it, so that it has fewer chances of **name collision**.
- We wanna protect a detail.
- We wanna protect future refactoring.

All the same principle applies. It's just the mechanism we use is now a **Block Scope** declaration instead of an **IIFE** (**`function` scope**), for example.

It's a lot lighter weight. It also has less side effects in the sense it doesn't redefined anything about returns or breaks or any of that other stuff, so blocks are a bit easier and lighter weight to put into places. They're not expressions, so we can't use them in places where we can use expressions.
But if we have them in a place that's a statement, it's totally okay.

We can't use `var` in this example, because of historically the difference between the **`var`** and **`let`** or **`const`**:

- The **`var`** has been attaching itself to the outer **`function` scope** or **`global scope`**.
- But with the **`let`** or **`const`** we can make a declaration inside of a block (curly braces `{}`) and it turns that block into a scope(or in other words, **`let`** and **`const`** attached themselves to their enclosing **block scope**).

> **Blocks** are not **Scopes** until they have a `let` or `const` inside of them and then that sort of implicitly makes them a **scope**.

### Choosing `let` or `var`:

This is an example that shows us where we should use `var` and where we should use `let`:

```JavaScript
function repeat(fn, n) {
    var result;

    for (let i = 0; i < n; i++) {
        result = fn(result, i);
    }

    return result;
}

Block Scoping: let + var
```

**The `var`**: When we have a variable that belongs to the entire **`function` scope** or needs to escape an unintended block, we use **`var`**. The `var` hoists and attaches itself to the **`function` scope**. The `var` can be reused within a scope, but `let` can not.

**The `let`**: When we have something that is naturally **block scoped**, we use **`let`** (for example in `for` loop). Or we want to use a `variable` for a few lines of code we create a **block scope** for it and then we only use **`let`** inside it.

### Explicit `let` block:

When we're only going to use a `variable` for a few lines of code, and we should not just throw it into the top level of the scope. What we should do is create a scope for it:

```JavaScript
function formatStr(str) {
    { let prefix, rest;
        prefix = str.slice(0, 3);
        rest = str.slice(3);
        str = prefix.toUpperCase() + rest;
    }

    if (/^FOO:/.test(str)) {
        return str;
    }

    return str.slice(4);
}

Block Scoping: explicit let block
```

Open up a curly brace pair `{}` and create a scope just for those three lines of code to use the `prefix` and the `rest` variable, and then don't let them exist for any other part of the `function`.

**Don't just say `let prefix`, and `rest` at the top of our `function`, open up a curly brace there.**

> If I have a `variable` that only needs to exist for a couple of lines, the way to do that is to make a block for it (with curly braces `{}`).

**Note:** Putting our declarations on the very same line as the opening curly brace helps to make it super clear to the reader, hey, these two are created for this block, and they exist only for the purpose of the block. As opposed to, we have to go looking for whether or not there are variables in this scope.

**Q:** Are we making this argument purely for semantics, or is there a performance benefit to doing something like that?.

**A:** It is extremely unlikely that we would ever see or be able to observe any performance difference. There are theoretical performance differences, like theoretically an offthread **garbage collector** could theoretically garbage collect it slightly earlier. Or theoretically it would reduce what was available to a **closure**. There are theoretic reasons, but in practice, you almost certainly would not see a performance difference this way.

### The `const`:

This is an example that shows us, what happens when we declare a mutable value like an `array` with `const`. Mutating the value is totally allowed on line 8:

```JavaScript
var teacher = "Suzy";
teacher = "Kyle";                      // OK

const myTeacher = teacher;
myTeacher = "Suzy";                    // TypeError1

const teachers = ["Kyle", "Suzy"];
teachers[1] = "Brian";                 // Allowed!
```

**Note:** We should know that the `const` keyword, does not carry its own weight within the language. And this big cost that comes with `const` is not even unique to JavaScript.

**The `const` does not mean that it doesn't change, it means a `variable` that can't be reassigned.**

Maybe when we think of `const`, and we think of the word constant. What we're thinking to ourselves is, a thing that doesn't change. That's not what a programming language designer means by `const`. A programming language designer means a `variable` that can't be reassigned, and those are two entirely different things.

What the `const` keyword is actually saying, from a semantic perspective is, for the rest of this block, I promise it's not gonna get reassigned.

It's better to use `const` when we're assigning a value that is already a **`primitive`** and therefore **immutable** like **`strings`**, **`booleans`**, **`numbers`**. It used as a semantic placeholder for those literals. That's what `constants` are supposed to be for. They're supposed to give semantic meaning as placeholders.

So we should default to using `var`.
Use `let` where it is helpful, use `const` sparingly only with **immutable** `primitive` values.

## Hoisting:

We should know that, the word **hoist** literally did not even appear in the JavaScript specification. Because turns out that **hoisting is not a real thing**.

**The JavaScript engine does not hoist**, it does not move things around the way it is suggested with hoisting.

**Hoisting is an english language convention that we have made up to discuss the idea of lexical scope, without thinking about lexical scope.**

This very simple piece of code, proves to us why hoisting doesn't actually exist:

```JavaScript
student;                        // undefined
teacher;                        // undefined
var student = "you";
var teacher = "Kyle";

Scope: hoisting
```

In **lexical scope** discussion we would first pass through this as the **compiler**, and then we would pass through it as an **execution**.

But people don't think about this in terms of two passes. And that's where hoisting comes from. Cuz hoisting says, well, we could explain what really happens here, if there was this magical behavior called hoisting.
Which was that any time JavaScript's execution engine entered a scope, it magically looked ahead and found all of the declarations wherever they were in that scope, and made `variables` for them. So it says the JavaScript engine goes and finds those `variable` declarations and moves them to the top of the scope before **execution**.
That's literally how hoisting is described.

**JavaScript does not actually reorganize our code by moving `variable` declarations up to the top.**

Asking this question proves to us that, that's not really what happens:
how are we going to figure out where all the `var`s are? We're gonna do some very sophisticated processing on the tokens that come later in that block until we find the end of the block.
And then any of those places where a declaration could have occurred, then we can pull those out, and we could theoretically rearrange and move those. And guess what that magical, fancy processing would be called? **That's called parsing**.

**If you wanna find the `variable` declarations further down in the block, the only way you can do that is with parsing.**

Let see what happened in `function`:

```JavaScript
function teacher() {
    return "Kyle";
}
var otherTeacher;

teacher();                      // Kyle
otherTeacher();                 // TypeError

otherTeacher = function() {
    return "Suzy";
}

Scope: hoisting
```

For the exact same reason it doesn't move `variables`, it doesn't move `functions`. It handles them during the **first pass**, and then it **executes**.

To use the form of `function` where we only assign it to properties or `variables` (It means `function expression`), we have to have all of our `functions` defined before we call them. Otherwise we get `TypeError` like above example.

**With using of `function declaration` we can simply put the executable code at the top, and put our `function declarations` at the bottom. But with `function expression` we can't do the same thing.**

> The difference between a **`function declaration`** and a **`function expression`** (including **arrow functions**) is **function declaration** hoisted but **`function expression`** did not.

**This is critical to know:**

> When we assign a `function expression` to a `variable`, the `variables` decoration itself hoisted, but the assignment is a **run-time** thing. Even we have a `function expression`, **compile-time** still handled that `function expression`. It didn't defer handling. What didn't happen is it didn't get at **compile-time**, associated with some `identifier` in the scope, but it still got compiled.
>
> All `functions` have a plan for their scope, until, and every time they get executed. So whether it was a `declaration` or an `expression`, when it gets executed is when that whole mapper plan becomes a real thing in memory, and every time it gets executed, that is the case.

### Hoisting Example:

This example can trick us if we're not used to thinking about hoisting as a two-pass system:

```JavaScript
var teacher = "Kyle";
otherTeacher();                 // undefined

function otherTeacher() {
    console.log(teacher);
    var teacher = "Suzy";
}

Scope: hoisting
```

When should use hoisting and when not:

```JavaScript
// var hoisting?
// usually bad :/
teacher = "Kyle";
var teacher;

// function hoisting?
// IMO actually pretty useful
getTeacher();                   // Kyle

function getTeacher() {
    return teacher;
}

Scope: hoisting
```

### Hoisting and `let`:

This example shows us if we use `variable` before it declared (only if it was `let` or `const`), we get a `TDZ error`:

```JavaScript
{
    teacher = "Kyle";           // TDZ error!
    let teacher;
}

Hoisting: let gotcha
```

**If we use a `variable` before it declared (with `let` or `const`), we get a `TDZ error` (Temporal Dead Zone error).**

> This statement, "`let` doesn't hoist", is wrong.

This example proves to us, that `let` (and `const`) will be hoist too:

```JavaScript
1. var = teacher = "Kyle";
2.
3. {
4.     console.log(teacher);    // TDZ error!
5.     let teacher = "Suzy";
6. }
```

In above code, if the `teacher` on line 5 did not host, then line 4 should print out `"Kyle"`. Because there is no `teacher` as of line 4 and it should go to the outer scope and find the `variable` from line 1. But this code still throws a `TDZ error`, and the reason is because **`let` and `const` still hoist**.

**There is a difference between how `var` and `let` or `const` hoist:**

- **`let` and `const` only hoist to a block, whereas `var` hoists to a `function`.**
- **When `var` creates its `variable` in the compile-time, it initialize it to `undefined`, but when `let` and `const` hoist and create their `variables`, they do NOT initialize it. It is in an `uninitialized` state.**

**`Uninitialized` being different than `undefined`. `Uninitialized` meaning you can't touch it yet.
And it doesn't get initialized until in that block you run across the `let` or the `const` declaration.**

> So `let` and `cont` absolutely do **hoist**, which is why we still get a `TDZ error`, it's just that they don't get **initialized**.

The reason `TDZ` exists is not even for `let`, `TDZ` exists because of `const`.

Imagine `const` being attached inside of a block scope and initialized itself to `undefined`.
And then on line 1 of a block, we did `console.log` of that `variable` and we saw it `undefined`, and then later we saw it with the value 42 (for example). Technically, that `const` would have had two different values at two different times, which academically violates the concept of a `const`. So they prevent us from accessing a variable earlier in the scope. So `let`'s have a `TDZ`, because `const` academically needed a `TDZ`.

> ![ECMAScript](https://user-images.githubusercontent.com/37678729/74106719-3d063180-4b7e-11ea-9791-f7fc843afd36.png)
>
> [ECMAScript](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-declarations-and-the-variable-statement)

# Closure

### Origin of Closure:

The _closure_ was introduced in JavaScript in the mid to late 90s.

In 1995, when **Brendan Eich** was hired to go to Netscape, and he was wanting to put **Scheme** in the browser. **Scheme** being an old **functional programming language**.
That JavaScript is probably, in some respects, related to something like **Scheme**, than even to Java or C++.

We could say that, the only other language at the time that was really maybe starting to become more consumer-oriented and had closure would have been **Perl**.

> So JavaScript's either the first or nearly the **first language** to move in that direction to have **closure**.

And as things stand today, 25 years later, every single language has closure because it turns out that closure is just that important.

It's **not** only functional programming that uses closure, but closure is used in lots of different places. It's used for asynchronous AJAX. It's use for all sorts of different things.

The closure as an idea is actually predating **computer science**, it actually comes to us from **lambda calculus**. It even predates the idea of a **programming language** in that sense.

# What is Closure?

As far as I'm concerned, this is the right answer to that question:

> The **Closure** is when a **`function`** is able to **"remember"** and access its **`lexical scope`** (the `variables` outside of itself, so-called **free `variables`**), even when that `function` executes in a **different scope**.

**Important Note:** So this definition has **two part** (without these two parts, we don't have a closure. We have to have both of them.):

- The `function` is able to access its **lexical scope**: We already know that, this is **lexical scope** definition in itself! **lexical scope** is that a `function` can reference `variables` outside of itself and it just goes up the **scope chain** to find it.

- The second part is the critical part. Without the second part, it's just **lexical scope**. With the second part of this definition, which is, even when the `function` is executed in a **different scope**, now we start to see something interesting. Cuz when we take a `function` and we pass it as a callback, or we take a `function` and return it back, the scope that it was originally defined in (at least conceptually), has gone away.
And we would think, normally, it's been **garbage collected**, it's done. But there's a `function` that survived that was defined within that scope. It turns out that the scope didn't go away at all. It turns out that this `function` is able to hold on to a reference to that scope, and wherever we pass the `function`, it continues to have access to that.
That doesn't happen by accident. That's not magical, that's **closure**.

The preservation, the linkage back to the original scope where it was defined, no matter where we pass that value, no matter where it executes, it retains that value. It preserves that scope. That's called **closure**.

So let's take a look at some examples of closure:

```JavaScript
function ask(question) {
    setTimout(function waitASec() {
        console.log(question);
    }, 100);
}

ask("What is closure?");   // What is colsure?

Closure
```

At the time that `waitASec function` runs, the `ask function` has already finished, and the `variable question` which it's closed over should have gone away. But it didn't go away because closure preserved it, so-called the `waitASec function` is closed over the `variable question`. That's a **closure**.

Now, academically, they think about closure on a per **`variable-basis`**, meaning only the `variables` that we're closed over, those are the only ones that are preserved. But at least the latest information that we have, JavaScript engines essentially implement closure as a linkage to the **entire scope**, **not** on a per `variable-basis`.
So it's best to assume that closure, even though academically it's per `variable`, it's best to assume closure is a **scope-based** (a per scope basis) operation.

> To create a closure don't need to do something special, just have to access a `variable` outside and then we have to pass the `function` somewhere.

> If we're in a language that has **first-class `functions`** and **lexical scope**, we're gonna have **closure**, because if we didn't have closure we would pass the `function` somewhere, and all over sudden, it would forget about all its `variables` it wouldn't make sense not to.

Another example of closure:

```JavaScript
1.  function ask(question) {
2.      return function holdYourQuestion() {
3.          console.log(question);
4.      }
5.  }
6.  
7.  var myQuestion = ask("What is closure?");
8.
9.  // ..
10.
11. myQuestion();   // What is closure

Closure
```

Here is the exact same thing. If we returned a `function` here that is closed over the `question variable`, then even though the `ask function` has finished, by line 11, we still have access to that `variable`. Not a snapshot of the `variable`, but the actual `variable` itself. That's called closure.
