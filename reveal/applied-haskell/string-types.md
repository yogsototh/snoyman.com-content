## String types

* Strings (== list of `Char`s)
* `ByteString` (strict and lazy)
* `Text` (strict and lazy)

----

## Quiz: Pick the String type

* Complete works of Shakespeare
* PNG-formatted image
* HTTP headers
* Contents of an HTML file
* Contents of a JSON file

----

## Builders

* `ByteString` builder
* `Text` builder

<img src="https://i.imgflip.com/1nuiyg.jpg" height="400px">

----

## APIs

* Mostly conforming APIs
* Designed to be imported qualified
* We'll see some typeclasses later to generalize
* Some types are *monomorphic* or *constrained*
    * `Text` and `ByteString` are monomorphic
    * Unboxed and storable have constraints on values
    * `Map`, `HashMap`, `Set`, `HashSet` have constraints on keys

---

# bytestring

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import Data.Monoid ((<>))


main :: IO ()
main = do
  let fp = "somefile.txt"
  B.writeFile fp $ "Hello " <> "World!"
  contents <- B.readFile fp
  B.putStrLn $ B.takeWhile (/= 32) contents
```

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import Data.Monoid ((<>))
import Data.Word8 (_space)

main :: IO ()
main = do
  let fp = "somefile.txt"
  B.writeFile fp $ "Hello " <> "World!"
  contents <- B.readFile fp
  B.putStrLn $ B.takeWhile (/= _space) contents
```

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import qualified Data.ByteString.Char8 as B8
import Data.Monoid ((<>))

main :: IO ()
main = do
  let fp = "somefile.txt"
  B.writeFile fp $ "Hello " <> "World!"
  contents <- B.readFile fp
  B8.putStrLn $ B8.takeWhile (/= ' ') contents
```

----

## Questions

* Is our code exception safe?
* Is there any laziness in the code?
* Discuss: to `Char8` or not `Char8` for `Handle` I/O

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import qualified Data.ByteString.Char8 as B8

fibs :: [Int]
fibs = 0 : 1 : zipWith (+) fibs (tail fibs)

fibsBS :: [B.ByteString]
fibsBS = map (B8.pack . show) fibs

main :: IO ()
main = B8.putStr $ B8.unlines $ take 5 fibsBS
```

* Propose alternative implementations
* Discuss: performance and assumptions
* `putStr` vs `putStrLn`

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import qualified Data.ByteString.Builder as BB
import System.IO (stdout)
import Data.Monoid ((<>))

fibs = 0 : 1 : zipWith (+) fibs (tail fibs)

main = BB.hPutBuilder stdout $ foldr
  (\i rest -> BB.intDec i <> "\n" <> rest)
  mempty
  (take 5 fibs)
```

* What does the performance of this look like?
* `foldr` or `foldl`?

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import Data.ByteString (ByteString)
import qualified Data.ByteString.Char8 as B8

main :: IO ()
main = do
  let bs = "Non Latin characters: שלום"
  B8.putStrLn bs
  print bs
```

```
Non Latin characters: ????
"Non Latin characters: \233\220\213\221"
```

* Implicit data loss!
* Terminal doesn't like incorrect character encodings

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
import qualified Data.ByteString as B
import qualified Data.ByteString.Char8 as B8
import qualified Data.ByteString.Lazy as BL
import qualified Data.ByteString.Lazy.Char8 as BL8
import Control.Spoon

main :: IO ()
main = do
    let bomb = ['h', 'e', 'l', 'l', 'o', undefined]
    print $ spoon $ take 5 bomb
    print $ spoon $ B.take 5 $ B8.pack bomb
    print $ spoon $ BL.take 5 $ BL8.pack bomb
```

<pre class="fragment"><code>Just "hello"
Nothing
Nothing</code></pre>

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
import qualified Data.ByteString as B
import qualified Data.ByteString.Char8 as B8
import qualified Data.ByteString.Lazy as BL
import qualified Data.ByteString.Lazy.Char8 as BL8
import Control.Spoon

main :: IO ()
main = do
    let bomb = concat $ replicate 10000 "hello" ++ [undefined]
    print $ spoon $ take 5 bomb
    print $ spoon $ B.take 5 $ B8.pack bomb
    print $ spoon $ BL.take 5 $ BL8.pack bomb
```

<div class="fragment"><pre><code>Just "hello"
Nothing
Just "hello"</code></pre><p>Arbitrary chunking size, beware!</p></div>

----

## Quiz: bytestring

* FIXME

----

## Exercise: file copy

* Copy `source.txt` to `dest.txt`
* Step 1: make it work
* Step 2: make it work for large files
    * Hint: you'll want `withBinaryFile` and `hGetSome`
    * Use Stackage to search for them

----

## Solution 1

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
import qualified Data.ByteString as B

main = B.readFile "source.txt" >>= B.writeFile "dest.txt"
```

* Problem: reads entire file into memory

----

## Solution 2

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
import qualified Data.ByteString as B
import System.IO
import Data.Function (fix)
import Control.Monad (unless)
main =
  withBinaryFile "source.txt" ReadMode $ \hIn ->
  withBinaryFile "dest.txt" ReadMode $ \hOut ->
  fix $ \loop -> do
    bs <- B.hGetSome hIn 4096
    unless (B.null bs) $ do
      B.hPut hOut bs
      loop
```

* Problem: allocates lots of buffers
* We'll look at the FFI later about this

---

## text

(Kinda just like bytestring)

----

* FIXME: copy lots of examples from above

----

```haskell
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import qualified Data.Text as T
import qualified Data.Text.IO as TIO
import qualified Data.Text.Encoding as TE

main = do
  let text = "This is some text, with non-Latin chars: שלום"
      bs = TE.encodeUtf8 text
  B.writeFile "content.txt" bs
  bs2 <- B.readFile "content.txt"
  let text2 = TE.decodeUtf8 bs2
  TIO.putStrLn text2
```

* What character encoding did `TIO.putStrLn` use?

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import qualified Data.Text as T
import qualified Data.Text.IO as TIO
import qualified Data.Text.Encoding as TE

main = do
    let text = "This is some text, with non-Latin chars: שלום"
        bs = TE.encodeUtf8 text
    B.writeFile "content.txt" bs
    text2 <- TIO.readFile "content.txt"
    TIO.putStrLn text2
```

* Good luck!
* http://www.snoyman.com/blog/2016/12/beware-of-readfile

----

## The char encoding debacle

* File I/O: use bytestring
* Standard handles
    * Interacting with user: use text
    * Interacting with pipe: use bytestring
* Also: text I/O is slow, you can use the `say` package

----

## Exercise

Take a UTF-8 encoded file and generate a UTF-16 encoded file

----

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-8.12 script
{-# LANGUAGE OverloadedStrings #-}
import qualified Data.ByteString as B
import qualified Data.Text.Encoding as TE

main :: IO ()
main = do
    bsUtf8 <- B.readFile "utf8.txt"
    let text = TE.decodeUtf8 bsUtf8
        bsUtf16 = TE.encodeUtf16LE text
    B.writeFile "utf16.txt" bsUtf16
```

----

## Laziness

* Don't read files lazily
* Writing lazy data isn't actually lazy I/O
* We'll get to streaming data/conduit later
