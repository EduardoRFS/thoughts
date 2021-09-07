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
