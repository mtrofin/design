# Feature to add after v.1

These are features that make sense in the context of the
[high-level goals](HighLevelGoals.md) of WebAssembly but are not considered part
of the [Minimum Viable Product](V1.md) or the
[essential post-v.1 feature set](EssentialPostV1Features.md) which are expected
to be standardized immediately after v.1. These will be prioritized based on
developer feedback.

## Great tooling support
This is covered in the [tooling](Tooling.md) section.

## Dynamic linking
 * [Dynamic loading](V1.md#code-loading-and-imports) is in [v.1](V1.md), but all loaded modules have
   their own [separate heaps](V1.md#heap) and cannot share [function pointers](V1.md#function-pointers).
 * Support both load-time and run-time (`dlopen`) dynamic linking of both
   WebAssembly modules and non-WebAssembly modules (e.g., on the web, ES6
   ones containing JS), sharing the heap as well as function pointers.
 * TODO

## Finer-grained control over memory
 * `mmap` of File, `madvise(MADV_DONTNEED)`, ...
 * TODO

## GC/DOM Integration
 * Access to certain kinds of GC things from variables/arguments/expressions
 * Ability to GC-allocate certain kinds of GC things
 * Initially, things with fixed structure:
   * JS strings
   * JS functions (as callable closures)
   * [ES6 typed objects](https://github.com/nikomatsakis/typed-objects-explainer/)
   * DOM objects via WebIDL
 * Perhaps a rooting API for safe reference from the linear address space
 * TODO

## Heaps bigger than 4GiB
* Allow heaps greater than 4GiB.
* Provide load/store operations that take 64-bit address operands; `int64` becomes the
  canonical pointer type.
* On a 32-bit system, heaps must still be <4GiB so all the int64 arithmetic (which will be much
  slower than 32-bit arithmetic) will be unnecessary.
  * Show we provide a uintptr_t (only 64-bit on 64-bit systems)?
    * This feature alone would not allow a C++ compiler to write size-polymorphic code since the word
      size is also baked into the code in a hundred other ways (consider `offsetof`).
    * The compiler *could* inflate all pointer types that are used in heap storage to 64-bit (so the
      uintptr_t type was only used for local variable/expression types).
      * This would imply an implicit truncation of any load of a pointer from the heap which could cause
        subtle bugs if the pointer was storing a real int64-width value.
      * This would still unnecessarily increase heap size on 32-bit; applications sensitive to OOM would
        still want a separate 32-bit build.
      * Now there are three compile targets: all-32, all-64, and this uintptr_t hybrid.
    * More discussion and experimentation needed.
      * Would the hybrid mostly Just Work?
      * Are there users who would want to ship a hybrid build instead of two 32- and 64-bit builds
        (conditionally loaded after a feature test)?

## Support for Just-in-Time Compilation
 * Finer-grained dynamic linking
 * Safe patching (of immediates, branch targets, ...)
 * TODO
 
## Proper Tail Calls
 * Expands upon Signature-restricted Proper Tail Calls.
 * TODO
 
## Asynchronous Signals
 * TODO

## Non-fixed-width SIMD
 * TODO
 
## Operations which may not be available or may not perform well on all platforms
 * Fused multiply-add.
 * Reciprocal square root approximate.
 * 16-bit floating point.
 * and more!

## Platform-independent Just-in-Time compilation
* TODO
* Potentially through dynamic loading.

## Multiprocess support
 * `vfork`.
 * Inter-process communication.
 * Inter-process `mmap`.