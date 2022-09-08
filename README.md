# TypeScript Introduction

## Learning Goals

- Understand TypeScript at a high level
- Install TypeScript onto your machine
- Write and run TypeScript programs

## What is TypeScript

JavaScript's power comes from the fact that virtually every browser on virtually
every single user-facing computing platform has a runtime engine that knows how
to run JavaScript code. In addition to that, there are other runtime engines,
such as NodeJS, which can run JavaScript outside a browser container.

That said, JavaScript has many limitations, chief among them the inability to
specify types for variables.

But since JavaScript has a huge advantage over any other languages with the
existing installed base of JS runtime engines, one solution is to come up with a
language that addresses the shortcomings of JavaScript at development and
compile time, but that actually gets translated to JavaScript before it's
shipped, so that it can be run just like "native" JavaScript would be.

This is exactly what TypeScript is.

TypeScript is a programming language that is a superset of JavaScript. It can do
everything that JavaScript can do, but also provides some unique constructs to
overcome some limitations of pure JavaScript. Since TypeScript is ultimately
"translated" into JavaScript, however, it can only support features that can
ultimately also be supported in JavaScript.

We will look at some of the most important features of TypeScript in this
section, but first let's look at a JavaScript example that makes one of its
limitations, the lack of variable typing, very obvious. Let's create a file 
named `ambiguous-types.js` and write in the following code:

```javascript
function add(firstNumber, secondNumber) {
  return firstNumber + secondNumber;
}

console.log(add("10", "20"));
```

This `add()` function was obviously written to add 2 numbers together. However,
in JavaScript, I have no way to specify that I want "integers" to be passed into
this function. What's even worse is that I have no way to automatically generate
an error if the parameters passed in are not integers.

What results from running `node ambiguous-types.js` will look like this:

```javascript
➜  node ambiguous-types.js
1020
```

Instead of getting the number `30` back, we got the number `1020` back. What's
even worse, we got it back as a number, suggesting that the addition operation
was actually successful. Imagine this was a function within a bank's software
system, and that `10` and `20` were the amount of transactions that had taken
place on a customer's account and needed to be taken out. Now that customer
would have "1,020" dollars taken out instead of "30" dollars.

There is actually JavaScript code we could write to guard against this
particular issue. For example, we could check the type of each parameter to the
`add()` function once we get them and throw an error (or attempt to convert
them) if they are not numbers - something along the lines of:

```javascript
if (typeof firstNumber === "number" && typeof secondNumber === "number") {
  return firstNumber + secondNumber;
} else {
  // throw an error here
}
```

There are 2 main issues with this approach:

1. This is a lot of additional code to add every time we want to make sure a
   variable is of a particular type
2. Even with this code, the error is not thrown or the problem is not
   highlighted until runtime, so a developer could actually proceed with this
   code for quite some time before realizing there ever was a problem.

TypeScript protects against these types of errors because it's a typed language,
meaning that you can specify an actual type for your variables and the
TypeScript compiler will warn you if you have mismatched types at compile time.

Before we explore the features of TypeScript, let's first set ourselves up to be
able to use it.

## Install TypeScript

TypeScript is readily available as an npm package. Let's install it onto our 
machines globally in terminal with the command:

`npm install -g typescript`

This package gives us access to the `tsc` terminal command that lets us generate 
JavaScript files (`.js`) from TypeScript files (`.ts`):

To check that the `tsc` command was installed properly, try running:

`tsc --help`

If successful, you should see output that indicates how to use this command.

## Using TypeScript

Now that we're all set up with TypeScript, let's test it out. Create a file named 
`app.ts` with the following content:

```typescript
console.log("Hello World");
```

This file:

1. Has a `.ts` extension to indicate that its content is TypeScript code
2. Has a simple line of code that is just like JavaScript - remember that
   TypeScript is a superset of JavaScript, so any JavaScript code is valid
   TypeScript code as well
3. Cannot be run as is by the browser, so it needs to be compiled into
   JavaScript code to run

We compile this `.ts` into a `.js` file with the following command:

`tsc app.ts`

Running this command will add a file named `app.js` into the same directory. 
It contains the vanilla JavaScript "translated" from your TypeScript code. 

You can now run the JavaScript file with Node:

`node app.js`

And see the corresponding output in the console.

> **Note**: When using `tsc` to compile TypeScript into JavaScript, it will
> always overwrite any preexisting code in the corresponding JavaScript file.
> When using TypeScript, make sure you're always working in the `.ts` file, 
> not the `.js`, to avoid accidentally overwriting your work! 

## Implementing the `add()` function with TypeScript

Let's go back to consider our simple `add()` function in JavaScript:

```javascript
function add(firstNumber, secondNumber) {
  return firstNumber + secondNumber;
}

console.log(add("10", "20"));
```

Let's re-implement this functionality in TypeScript. Write the following code in 
`app.ts`:

```typescript
function add(firstNumber: number, secondNumber: number) {
   return firstNumber + secondNumber;
}

console.log(add('10', '20'));
```

With this code, we've introduced a key feature of TypeScript, **type annotation**.
The syntax `firstNumber: number` specifies the type of the variable `firstNumber` 
as a `number`, which means that the TypeScript compiler will warn us if anyone 
tries to call the method with parameters that are not of type `number`. 

It achieves the same functionality as our JavaScript workaround (with the `typeof` 
instruction) above, but without the additional code, and with this added advantage: 
the compiler can actually tell us about a type issue _prior_ to us running the code 
anywhere. 

For example, run the command to compile `add.ts` and you will get the following error:

```typescript
➜  tsc add.ts
add.ts:5:17 - error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.

5 console.log(add('10', '20'));
                  ~~~~


Found 1 error in add.ts:5
```

Now try updating the code to fix that error and compile again -- it should run with 
no errors!

## Conclusion 

JavaScript is a staple language of the web as we know it today, but it is not without
its limitations. TypeScript addresses many of them and has become a popular alternative 
as a result. As we saw, just adding types to our code saved debugging and development time. 
Our example was small, of course, but imagine you've built a much larger app with several 
files and many lines of code. A bug caused by calling a function with strings instead
of integers could take a while to track down or even catch to begin with. When used properly,
TypeScript can help prevent such errors. In the following lessons, we will cover more of the
basic features TypeScript has to offer. 

## Resources 

- [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
- [TypeScript Tooling in 5 Minutes](https://www.typescriptlang.org/docs/handbook/typescript-tooling-in-5-minutes.html)