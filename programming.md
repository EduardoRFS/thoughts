# Programming

## What is good software?

## What do you get by having good code?

## Constraints

## Correctness

### Static types

Overall static types prevents the developer from commiting a large class of errors, by ensuring that no piece of code can ever fail with an error not specified by the functions being called.

### Nominal typing

Nominal typing provides a lot of really useful features, as a type is now identified by it's name instead of it's content.

Abstract types in OCaml allows you to ensure properties of a system that cannot be broken anywhere else in the codebase, by ensuring that only a limited piece of code can operate on a piece of data.

An important additional example is private fields combined with final class like in Java, of course Java type system is unsound, which is problematic.
