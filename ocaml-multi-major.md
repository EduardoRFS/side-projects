# OCaml Multi Major

## Why

OCaml Multicore allows to have single OCaml using multiple threads in a process, this is great if your application requires sharing a lot of data between multiple threads.

But it also means that the GC latency increases, and it still has a big stop the world. An alternative would be to run multiple OCaml each one with it's own old / major heap.

This allows real time applications just by moving your workload around and also to scale past the limits of OCaml Multicore GC.

## How

For Multicore Caml_state was introduced and it's a global variable per thread that points to it's own Caml_state, which includes it's own heap, but currently the major heap is not part of Caml_state.

It should be a matter of lifting the major heap state to Caml_state and calling caml_startup on another thread, possibly also providing a messaging API for that.

## Performance

I don't think there is any regression as in PIC code reading from the Caml_state should be as fast or faster than reading from global variables.
