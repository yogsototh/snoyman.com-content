# Applied Haskell

* Michael Snoyman
* LambdaConf 2017

---

## Overview

* Haskell is a "revolutionary" language
* We don't care about that today very much
* How do I get things done in Haskell?
* Focus:
    * Familiarity with best-in-class libraries
    * Get better at picking up Haskell idioms
    * Learn tooling a bit more
    * Pick up runtime system idiosyncracies

----

## What we won't do

* Focus on advanced features of the language
    * If they come up, we'll cover them, but they're not the focus
* Learn math or category theory
    * We just want to do enough to be proficient with common libraries
* Teach basic Haskell syntax
    * Assuming such basic familiarity as a prereq

----

## Learning Resources

* https://www.fpcomplete.com/haskell-syllabus
* https://haskell-lang.org/documentation
* https://haskell-lang.org/libraries
* https://www.stackage.org/ (and its Hoogle search)
* http://www.yesodweb.com/book
* https://www.schoolofhaskell.com

---

## Tooling overview

* __GHC__ Compiler
    * GHCJS, JHC, others...
    * Almost everyone uses GHC
* __Stack__ build tool
    * Downloads GHC for you
    * Installs packages
* __Emacs__ editor
    * intero-mode works pretty well
    * And many others
* __hlint__ linter, __hoogle__ search

----

## Cabal packages

* Have a name and version number
* 0-1 libraries
* 0 or more executables
* 0 or more test suites
* 0 or more benchmarks
* Configured via `projectname.cabal`
* Or if you're cool: `package.yaml` and `hpack`
* Can be released as open source to Hackage

----

## hpack example

* FIXME

----

## Stackage snapshot

* Specifies a specific GHC version
* Specifies a collection of Cabal packages from Hackage
* Specifies build flags
* Nightly: one per day, try to take the latest version from Hackage
* LTS (Long Term Support): weekly, tries to maintain backwards
  compatibility based on PVP
* Guarantee: we got these packages to compile together and pass tests

----

## Stack projects

* Configured via `stack.yaml` file
* 0 or more local Cabal packages
* Specifies a *resolver*
    * Stackage snapshot (`nightly-2017-01-01`, `lts-8.9`)
    * GHC version (no extra packages)
* Can add extra deps, flag overrides, much more
* Can pull in packages from a Git repo

----

## stack.yaml example

* FIXME

----

## Quiz: package or stack.yaml

* FIXME

----

## Stack script interpreter

* Useful for single-file code
* Nicely portable, just needs Stack
* We'll use it a lot today

----

## Stack script example

* FIXME

----

## GHCi

* REPL
* Access with `stack ghci` or `stack exec ghci`
* I'm personally GHCi-challenged, we won't use it much (if at all)
  here

---

## Generic data structures

* Lists
* `Vector`s (boxed, unboxed, and storable)
* `Map`, `HashMap`, and `IntMap`
* `Set`, `HashSet`, and `IntSet`

----

## Quick: Pick the data structure

* FIXME
