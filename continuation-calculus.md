# Continuation Calculus

## References

- Paper https://arxiv.org/pdf/1309.1257.pdf
- Type system https://arxiv.org/pdf/1409.3313.pdf
- Master thesis https://pure.tue.nl/ws/portalfiles/portal/46941433/760931-1.pdf
- CBV and CBN lambda calculus http://www.cs.ru.nl/~herman/PUBS/CBN_CBV_iteration.pdf

## Goal

Use it as an abstract machine where most operations are simply defined and allows fast compilation, simple optimizations and no implicit allocation.

The continuation calculus seems to allow that as most of it's terms are essentially variables or applications and don't have an implicit stack when doing evaluation, which leads to no implicit allocation.

Sadly the continuation calculus has implicit allocations in the form of data terms, that can require the creation of closures, my hypothesis is that this can be fixed by rejecting any data term.

## Data terms

As I would like to reject implicit allocations, having closures being allocated is probablematic, but it seems like all data terms inevitably lead to a term in form of `n.a.b -> a` which means it's a term that don't call a continuation just returns it.

If that's true by rejecting any term that doesn't call something the calculus is not turing complete anymore but also doesn't have any implicit allocation.

Those can be solved through extensions, like having explicit closures.

> TODO: Aren't data terms explicit enough?

## Advantages

### Optimizations

Because the only model of computation in the CC is the continuation that ensures that any optimization made in terms of continuations will have effect on all sorts of code.

And because a lot of imperative style code can be really well modeled in terms of continuations that leads to optimizations here being valid for not only functional code, but also imperative code.

As an example, direct recursion and loops can be compiled to the exact same assembly, not mattering if you're using mutation on the language level or not.

### Termination

In CC some terminations are really easy to check, essentially if there is no rule cycle then no loop can ever happen, unlike the lambda calculus where you can construct fixed point combinators or even the y-combinator.

## Compiling

### Stack Trace

As we're using always continuations the stack trace is now useless, to recover from that one idea is for functions where the depth is small and defined it's possible to emit call instead of jmp.

My first idea was to return a ret callback as continuation should be provided, this can also be faster for some functions as the continuation will always be the same global callback and the branch predictor will be happy with it.

But if the function itself already has bounded stack size maybe it could emit `ret` instead, being the job of the calleer to call the continuation later if needed, this should be even faster as there is no need to use an additional register or load the callback address

### Closures

The full continuation calculus can be compiled easily to closures, those closures can be described as requiring the closure pointer to be always in the same register, and the pointer itself needs to be dereference to access the function pointer, this reduce the need for having helper functions like `caml_apply`.

In x86_64 this looks particularly nice as it can be done as `jmp [rax]`.

Because of this encoding a trick possible is that functions that are just global symbols can have a pointer acquired directly from the `GOTPCREL` so that when the pointer it's called it will dereference the global symbol anyway, avoiding an additional address for them.

### Lambda Lifting

Because of the existance of data terms not every term can be lifted, so high order functions need to support calling closures.

I believe that this can be worked around by making so that only leaf function handle closures, which in the presence of whole program transformation is IO functions and external functions, but external functions can also have explicit signature saying marking if it returns a closure or not.

### call + jmp

I'm not sure if this works, but another interesting approach which may be possible is to do a convention of call + jmp where the first call will always only move registers around and return a pointer to jmp, this allows the encoding of data in a simple style while not requiring closures, the first call will never mess with the stack so the stack can be used.

```cc
f.a.b.r -> a.(r.a).(r.b)
```
