# Deep JavaScript Foundations

> **Note:** I collected some important and deep JavaScript foundations.
> Most of this information comes from Kyle Simpson's books ( [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) series ), [ECMAScript](https://www.ecma-international.org/), [MDN](https://developer.mozilla.org/en-US/) (Mozilla Developer Network) or courses that I took.
> It would help to have a deeper understanding of JavaScript.

## Table of Contents

1. [Introduction](#Introduction)
2. [Types](#Types)
3. [Coercion](#Coercion)
4. [Equality](#Equality)
5. [Static Typing](#Static Typing)

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

> ![Untitled2](https://user-images.githubusercontent.com/37678729/71778735-c9916300-2fc6-11ea-9732-e453c03cf75b.png)
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
>   - `Funcion`
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

> ![Untitled3](https://user-images.githubusercontent.com/37678729/71778767-2b51cd00-2fc7-11ea-88d6-35eec3875d6f.png)
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

> ![Untitled4](https://user-images.githubusercontent.com/37678729/71778794-8daacd80-2fc7-11ea-81fb-d072db0100ba.png)
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

By the way, the `abstract operations` they're not like a function that could somehow be called. There may, in fact, be actual methods inside of a JavaScript engine or not, they're not like required to be actual things. But when we call them abstract, we mean they're a conceptual operation. So any time you have something that is not a `primitive` and it needs to become a `primitive`, conceptually, what we need to do is this set of algorithmic steps, and that's called `ToPrimitive`, as if it were a function that could be invoked.

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
>     It may not be obvious, but JavaScript coercions always result in one of the scalar primitive (see Chapter 2) values, like string, number, or boolean. There is no coercion that results in a complex value like object or function. Chapter 3 covers “boxing,” which wraps scalar primitive values in their object counterparts, but this is not really coercion in an accurate sense.
>
> Another way these terms are often distinguished is as follows: `“type casting”` (or `“type conversion”`) occurs in statically typed languages at compile time, while `“type coercion”` is a runtime conversion for dynamically typed languages.
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

> We could throw a value into an array, just the one value into an array, and then call `.join` on it. And that actually ends up stringify it.
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
> It's not called out in the same way in the `abstract operations`.

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

> ![Untitled](https://user-images.githubusercontent.com/37678729/71779325-2262fa80-2fcb-11ea-835d-debd2bccb90f.png)
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
// Because workshop2Count is non-primitive, algorithm invokes toPrimitive on it. (because of that weird array stringification that doesn’t include the brackets. And only accidentally working because there is only one value in the array.)

// 2- if (42 == "4")
// Now we have two different types, we have a number and a string. There's two options here. We could either make the number into a string and compare the strings, or make the string into a number and compare the numbers. The algorithm prefers numeric comparison. So the string becomes the number.

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
  // Now, we have a non-primitive compared to a primitive. So we need to turn that non-primitive (array) into a primitive. So it becomes the empty string.

  // 3- if ("" == false)
  // So now, We have two primitives but they are not of the same type. The algorithm prefers that they both become numbers.

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

- They use "non-JS-standard" syntax (or code comments) (they chose to use a syntax that they had to layer on top of `JavaScript`.)
- They require\* a build process, which raises the barrier to entry.
- Their sophistication can be intimidating to those without prior formal types experience.
- They focus more on "static types" (variables, parameters, returns, properties, etc) than <u>value types</u>.

These tools came out with a way that we can do their typing annotations using only code comments. So at least in that scenario, we haven't locked ourself into if we don't use this tool this code literally can't run. That's sort of an escape valve, and that's a good thing, but almost nobody's using the code comments. Everybody's using the inline syntax annotations.
