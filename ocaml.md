# OCaml

## Memory Model

To do unsafe things or FFI understanding the memory model is important, the following link is quite useful.

- https://dev.realworldocaml.org/runtime-memory-layout.html

## FFI

It's REALLY important to understand how the memory model works, if you do understand, the following links are always useful.

- https://ocaml.org/manual/intfc.html

### When to use ctypes?

I truly don't know. Overall it seels less error prone, so it's probably a good idea to always use it.

- https://opam.ocaml.org/packages/ctypes

## Future - Memory Management

- https://blog.janestreet.com/building-a-lower-latency-gc/

### refcounting

> Why?

The goal is to somehow get OCaml be lower latency, but actually the things that we get more easily is fast finalizers, especially for data that is known to be short lived it can be quite useful.

But the latency improvement aspect is by hoping that we can prevent compacting the major a lot of times if we can find big holes in the major, created by operations like `List.map`, by putting the minor data inside the major hole.

This should also allow to have a refcounted only heap, which may be interesting.
