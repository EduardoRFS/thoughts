# Programming

## What is good software?

## What do you get by having good code?

## Constraints

## Correctness

The property of your program to behave as expected.

### Static types

Overall static types prevents the developer from commiting a large class of errors, by ensuring that no piece of code can ever fail with an error not specified by the functions being called.

### Nominal typing

Nominal typing provides a lot of really useful features, as a type is now identified by it's name instead of it's content.

Abstract types in OCaml allows you to ensure properties of a system that cannot be broken anywhere else in the codebase, by ensuring that only a limited piece of code can operate on a piece of data.

An important additional example is private fields combined with final class like in Java, of course Java type system is unsound, which is problematic.

### Tests

While tests cannot proof that a piece of code works it can ensure that it works for a specific case, which is important for you and your users, this on itself is already a good sign of correctness.

### Defensive coding

This is the idea of you should expect that your function can be called with the wrong inputs, normally in dynamic languages that would be ensuring the type of a function, but besides of that you can instrument it to ensure that a number is in a specific range or that a function before it was called.

A good trick is to add this assertions through a macro or a function call that can be removed by the compiler in production, for something like JS you can think about webpack / babel / typescript. This allows you to go crazy on assertions without affecting performance at all.

### Type Level Programming

This idea is one of writing code to satisfy the type checker, this code will be executed only on checking time, so it is "type time" code.

Whenever someone refers to be doing programming on the type level, they're probably trying to enforce some properties on the type checker.

### Dependent Types

Types that can depend on values. This allows to be more specific about values and enforce properties on them. This is also one of the most convenient properties to do type level programming.

In most languages with dependent types, there is no separation between term and type level, making so that you can proof properties about your code.

```rust
// aka, if pred is true then x will be an int, otherwise it will be a string
(pred : Bool) => (x : pred | true => Int | false => String) => x
```
