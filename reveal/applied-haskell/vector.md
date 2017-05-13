# vector

* Just like lists, but not
* Packed representation
* Spine strict (sometimes value strict)

----

## The Three Types

* __Boxed__ Array of pointers
    * `Data.Vector`
* __Storable__ Binary rep, *pinned*
    * `Data.Vector.Storable`
* __Unboxed__ Binary rep, *unpinned*
    * `Data.Vector.Unboxed`
* Also has a generic interface unifying the others (more on that later)
* Ignore `Primitive`, use `Unboxed`

----

## Pinned vs unpinned

* Pinned
    * `malloc`/`free`
    * GC cannot move it around
    * Safe to pass to FFI
    * Can lead to memory fragmentation
* Unpinned
    * Fully managed by GC/RTS
    * Can be compacted by GC
    * Cannot be passed directly to FFI

----

## Compared to lists

* Less pointer indirection
* Unboxed and storable: no pointer indirection
* Less memory used for holding pointers
* Consing is more expensive
* No laziness/infinite data structures
* Advice: most cases, vectors are what you want

----

## Mutable vs immutable

* Both mutable and immutable interfaces
* Mutable actions from inside `IO` or `ST`
* Freeze a mutable to an immutable
* Thaw an immutable to a mutable
* Immutable API: similar to lists
* Mutable API: similar to C arrays

----

## What do I use?

* Unless you know otherwise: immutable
* If unboxed is possible, use it
* Otherwise, if storable is possible, use it
* Otherwise, use boxed
* Generic algorithm? Use `Generic`
* Polymorphic container? Stick with boxed

----

## Stream fusion

* Avoid intermediate data structures
* Leverages stream representation and rewrite rules
* Only applies to immutable API
* When it works, gives great speedups

---

## Vector basics

* FIXME examples

----

## Unboxed

* FIXME example

----

## Generic API

* FIXME example

----

* FIXME examples of putting undefined into vectors

---

## Mutable vectors

* FIXME

----

## vector-algorithms

* FIXME
