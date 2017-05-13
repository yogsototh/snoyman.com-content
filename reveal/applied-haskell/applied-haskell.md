* FIXME split into separate files

---

* Strictness annotations
    * `data Foo = Foo !Int` vs `newtype Foo = Foo Int`
    * Unpack
    * Primitive types
    * Primitive `IO`

* Foldable, MonoFoldable

* classy-prelude

* Monad transformers, MonadIO

* Logging: trace vs monad-logger

* Exceptions

* Mutable variables
    * IORef
    * MVar
    * TVar, and STM in general

* async
    * Write a concurrent program that prints the numbers 1 to 100 in
      one thread, pausing one second between number, and printing
      "Hello World" every two seconds in another thread

* typed-process

* hspec

* Profiling
    * Bad average
    * Analyzing GHC core

* conduit
    * Good average
    * When to use conduit
    * Take a file and, for each line, print out the number of bytes in
      the line (try using bytestring directly and then conduit)
    * http://stackoverflow.com/questions/42675764/read-large-lines-in-huge-file-without-buffering/42676477#42676477

* http-conduit
    * Make an HTTP request and print the returned HTML to the console

* aeson and yaml

* Web services: WAI vs servant vs Yesod

* FFI
    * Unsafe bytestring operations
    * Efficient file copy
