# Deep JavaScript Foundations

> **Note:** I collected some important and deep JavaScript foundations.
> Most of this information comes from Kyle Simpson's books (You Don't Know JS series), MDN (Mozilla Developer Network) or courses that I took.
> It would help to have a deeper understanding of JavaScript.

## Table of Contents

1. [Introduction](#Introduction)
2. [Types](#Types)
3. [Coercion](#Coercion)

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

> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are `Undefined`, `Null`, `Boolean`, `String`, `Symbol`, `Number`, and `Object`. An ECMAScript language value is a value that is characterized by an ECMAScript language type.
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

Earlier, we see `toNumber()`, actually that's an abstract operation.

> **Abstract Operations**
>
> These operations are not a part of the ECMAScript language; they are defined here to solely to aid the specification of the semantics of the ECMAScript language. Other, more specialized `abstract operations` are defined throughout this specification.
>
> **_Type Conversion:_**
>
> The ECMAScript language implicitly performs automatic type conversion as needed. To clarify the semantics of certain constructs it is useful to define a set of conversion `abstract operations`. The conversion `abstract operations` are polymorphic; they can accept a value of any ECMAScript language type. But no other specification types are used with these operations.
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
           -0   "0"

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
