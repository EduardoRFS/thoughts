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

## offloading GC

Goal is to offload considerable computing of major heap collections to a different thread.

### Memory is mutable

As memory is mutable, any scanning in parallel with the worker thread can only detect things that were live recently, but never be a proof that something is dead.

**Solution**: segregate heaps in mutable and immutable, immutable memory can be scanned in parallel safely and as OCaml memory is mostly immutable this should be a considerable help.

### mutable -> immutable

Pointers from the mutable to immutable memory can not be detected in parallel which makes makes so that scanning the immutable memory ensures you that some values are definitely alive but not having a reference doesn't mean the value is necessarily dead.

**Solution**: registers + reference table = roots, use minor to get registers

Can also add a tag of externally referenced in the header of a block, and mark it during caml_modify or initialize.

Both will affect performance of mutable blocks, by making initialization and / or mutation slightly slower.

This means that initializing fields on

- major work during minor collection
- segregated heap's(immutable and mutable)
- tagging

### modules are mutable

Can the parallel GC runs during the initialization of the module? Root_initialization

### comballoc

We cannot combine two allocations for mutable and immutable, but ideally we should be able to combine alloc -> alloc(mut) -> alloc

### TRMC

How does this immutability interacts with TRMC? Can we treat TRMC data somehow as immutable? Maybe by first cleaning it?

## multiple major heap

As Caml_state already exists with a bit of work we could lift the major heap to it so that in the same process multiple threads can be running it's own OCaml with independent GCs.

Also great on Windows, as Unix.fork is really slow there.

## frametable on caml_call_gc

OCaml emit's an additional label for frametable on GC, my hypothesis is that this could be removed so that the label always points one instruction before the jmp, that would lead to slightly better compaction and also one label less.
