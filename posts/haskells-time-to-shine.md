First of all: a big shoutout to r/programmingcirclejerk (pcj). I hope
you guys get some good enjoyment out of this post. I'll try to include
some over the top lines for your benefit.

I started using Haskell about ten years ago. I'd been trying out
different languages, looking for one that I felt comfortable building
production systems in. My biggest desire was a safe language, one that
would help me avoid common pitfalls. I decided then on Haskell, and
was rewarded with (IMO) a language that also provided high levels of
productivity and good performance.

But if I'm being totally honest: the Michael of today would not have
picked Haskell ten years ago. Like most other developers (I believe),
I've grown more conservative in my choice of technologies over the
years. And while I still believe that Haskell the language was the
best language on the block ten years ago, the overall picture did not
measure up (more on this in a bit).

I'm glad ten-years-ago Michael was a bit naive. I'm glad I chose
Haskell and stuck with it. The language and ecosystem have improved a
lot since then. I obviously have some high levels of bias, but I
believe that, if you add up all the strengths and weaknesses, Haskell
today _is_ the best all around choice for many programming
problems. And as a community, we should continue to improve the areas
where it's weak (which I'll also discuss).

Perhaps more interesting is that the _world_ has changed in a
significant way in the past ten years. Back then, pitching a
statically typed, functional programming language with garbage
collection for native compilation was a hard sale. Functional
programming was considered academic; garbage collection was for
managed VM languages; static typing triggered Java
`AbstractBeanFactory` PTSD. Today, none of that is true:

* Functional programming has gone fully mainstream. Many of the most
  common idioms from Haskell (recursion, folds, maps, lambdas) are
  considered good practice in a wide range of languages. Even some of
  the scary bits from category theory (like burritos) are now
  considered weird instead of terrifying.
* The world has realized that static typing doesn't have to be painful
  ala old-school C++ and Java. Type inference is recognized as a great
  tool to reduce the overhead. And more languages are embracing
  stronger type systems which give more benefits than the way we used
  to write C++ and Java.
* The runtime model of Haskell is now more commonplace. Native code
  with garbage collection and green threads is embraced by at least
  two other popular languages (Go and Erlang).

Putting this all together, we come to the title of this blog post:
with the improvements in Haskell we've seen, the changes in industry's
perception, and a willingness on our part to keep pushing the
envelope, now is the time for Haskell to shine.

*pcj quote* Haskell is the greatest language ever and will literally
cure cancer.

## Great strengths

I'm not going to spend as much time on this section as the others,
since most readers probably recognize these already.

* I'm going to count _concurrency_ as one of the best strengths in
  Haskell, because it implies so many other things. Feel free to
  disagree here of course, it's just like, my opinion, man. But
  Haskell's insistence on immutable-by-default, non-side-effecting
  code, and lightweight green threads makes many hard problems
  trivial. We've certainly improved in this area in the past ten
  years. The introduction of the
  [async library](https://haskell-lang.org/library/async) and
  wide-spread use of Software Transactional Memory (STM) have made
  things even better.
* I wish I had more than anecdotes to back it up. But from my own
  (highly biased) personal experiences, and the (highly biased)
  experiences of people I interact with: Haskell results in lower bug
  counts in production. This is achieved by pushing more of the logic
  into the type system, and preventing entire classes of bugs. (pcj
  quote: If it compiles, it always runs perfectly, with zero runtime
  bugs even possible.) Obviously errors can still ship (I've done
  plenty of that myself). But I'm more confident with Haskell than any
  other language writing software that I believe will run correctly
  for years. Moreover, and perhaps far more importantly: Haskell's
  type system lets me modify and refactor my code in major ways,
  something I'd be terrified to do in other languages.
* The Haskell language is consistent. It was designed from the ground
  up to be lazy, pure, functional, immutable, and a few other words
  that fit in there too. Those features fit together nicely, because
  they were designed to do so. While other languages continue to add
  features like immutability and functional paradigms, they often lack
  the naturalness of a language like Haskell, designed to offer them
  from the ground up.
* There are many advanced type level features in Haskell. I'll make a
  perhaps absurd claim: the vast majority of benefit we get in Haskell
  from the type system comes from Algebraic Data Types (ADTs). Plain
  old Haskell data types make generic programming trivial and
  lightweight, encourage thinking about your program logic with
  well-designed sum types (a.k.a. tagged unions), and with various
  features like deriving and `newtype`s lower the barrier
  significantly to creating helper data types throughout your
  codebase.

Feel free to yell at me in the comments for the features I left
out. Haskell's got a lot going for it, and I honestly just picked the
first four things that came to mind.

pcj quote: Haskal is literally the only language with immutable data
structures, allowing monadic contravariant extensions.

## No longer existent problems

But like I said, ten years ago choosing Haskell was a naive
decision. What things have improved since then that make me say it's
no longer naive, but instead well informed and your best choice for
many problem domains?

* Haskell is no longer very difficult to learn (but see learning curve
  comments in next section). When I got started, there was no book
  well recommended to get off the ground. (Perhaps such a book _did_
  exist; if I didn't hear about it then, it points to another issue:
  bad community communication.) I spent a lot of time fumbling around
  with half baked tutorials, including the infamous string of Monads
  Are Like Burritos tutorials. We've shaped up significantly since
  then. There are [wonderful books](http://haskellbook.com/),
  [curated library tutorials](https://haskell-lang.org/libraries), and
  a culture of documenting libraries&mdash;both with API docs and a
  cookbook or tutorial&mdash;has taken root. We can always improve,
  but the situation is completely manageable today. I would feel
  comfortable taking a developer without a functional programming
  background, giving them a book and a few links, and expect they
  could come up to speed on how to write Haskell code.
* Ten years ago, there were not many high quality, real world
  libraries available. Today, there are over 10,000 open source
  Haskell libraries and over 2,000 in the curated Stackage package
  set. For most problems, be it network programming, data structures,
  concurrency, system interaction, or numerics, there is at least one
  high quality library available. For most software projects, I
  believe that you will be able to pick up a set of open source
  libraries and immediately get to work. The situation today is orders
  of magnitude better than it was ten years ago.
* I may have my timeline on this off a bit. When I started seriously
  writing Haskell code, Hackage and cabal-install were just coming
  together. The first time I tried using a library, I manually
  downloaded the tarball, `tar xf`d it, and then ran `runghc Setup.hs
  configure && runghc Setup.hs build && runghc Setup.hs copy && runghc
  Setup.hs register`. Or something like that. Since then, Hackage has
  acquired (as mentioned above) huge numbers of libraries,
  cabal-install made it a standard workflow to depend on upstream
  libraries, and we've introduced a plethora of additional tooling:
  Stackage, Nix, Stack, Intero, ghc-mod, and editor integration all
  over the place. The tooling ecosystem is continuing to improve, but
  there's no doubt that with just about _any_ toolset you choose
  today, you'll be up and running with Haskell development, with as
  many libraries as you want, in no time.
* Lots of us do web development on a regular basis. Even projects that
  are not primarily web dev (e.g., distributed computation frameworks)
  often require some kind of a web frontend for interaction. I
  remember fiddling around with the `cgi` library when I got started,
  and giving up when its `README` casually mentioned "oh, you'll want
  to use monad transformers for databases" without further
  explanation. The situation is completely different today. There are
  multiple high-quality server side options for web development, and
  even frontend development is well supported with GHCJS or related
  languages like PureScript. For anyone familiar with my work, I spent
  a lot of time trying to bring Haskell up to the level of other
  languages in web development. Today, someone wanted to create a web
  site, a RESTful service, or a Single Page Application can get
  started immediately.
* Performance in Haskell has improved across the board. Personally, I
  think performance usually gets overhyped in language comparisons,
  since it's one of the few things we can point to with "objective"
  numbers to back it up. Of course, skewed benchmarks are hardly
  "objective." Nonetheless, at least one aspect of Haskell's
  performance improvements stands out as worth mentioning: improved
  concurrency. The runtime system has gone through multiple
  iterations, and today we're at the point where highly concurrent
  Haskell web servers like Mighttpd are competitive (or even improve
  upon) established C codebases like Nginx, while retaining high
  level, readable code.

pcj quote: Haskell has literally accomplished 100 years of progress in
the past 10. That's because Haskell is a 10xer language.

## Remaining problems

I want to acknowledge that Haskell still has some weaknesses, which
may prevent its adoption in some cases. As a community, it's important
to recognize these so that:

1. Newcomers have realistic expectations, and know where they may want
   to consider holding off on Haskell
2. If we acknowledge these points (or other points that others feel
   more important than my list), it gives us some direction on how to
   fix things

* Despite no longer being very difficult to learn, Haskell still has a
  learning curve. I'd argue this is _not_ because Haskell is
  inherently difficult, but because it is inherently
  _different_. Jumping from Python to Ruby necessarily requires less
  cognitive overhead than Python to Haskell. We can't tell newcomers
  that they'll be up and running as expert Haskellers in two weeks
  time, it's not fair to them. I _would_ say that you can write useful
  Haskell code in your first few weeks learning the language, and with
  dedication and practice you can be an advanced intermediate
  (whatever _that_ means) in a few months. Short of getting purely
  functional programming taught earlier in programmers' careers, I
  don't see a solution here.
* There is a double edged sword: companies complain that they can't
  find Haskellers to work for them, and Haskellers complain that they
  can't find jobs. One solution is to convince companies to embrace
  remote hiring: you can get very bright programmers interested in
  doing Haskell work if you're willing to hire outside your
  city/country. Here's another one I've been pitching: as Haskellers,
  we can step outside our comfort zones and start Haskell-based
  companies. You'll do less engineering, but if you (like me) believe
  that Haskell is a better tool, you'll have a massive market
  advantage. (pcj quote: Haskell will totally destroy all Java based
  companies in a few years, just like Java wiped out all COBOL code
  bases.)
* While tooling is better, it's not perfect. I think the two biggest
  complaints I hear about are slow compile times and lack of IDE
  support. Unfortunately, there's unlikely to be major improvements in
  either of these. While GHC is trying to improve performance, Haskell
  _is_ inherently a more complicated language than, say, Go or
  Python. And optimization steps in GHC are costly. We will hopefully
  get constant factor improvements. but I doubt we'll ever get, say,
  2-3 times faster than today. On the IDE front: Haskell has attracted
  a group of people that tend to prefer editors like emacs and Vim
  versus full-blown IDEs, making it unlikely for them to invest
  heavily in creating IDEs for those requesting them. It may happen,
  but if you want to use Haskell and like IDEs, you're best off
  getting comfortable with an editor like Atom or VSCode and lowering
  your expectations.
* Want to build a real time application? Build a desktop GUI? Probably
  not a good choice for your first Haskell project. People have done
  both, but Haskell's GC introduces latency, and the desktop GUI
  bindings are not very polished yet. Both of these can improve, but
  from the subset of Haskell work I've seen, most people are building
  network services, command line tools, and other such things where
  pauses and lack of GUI don't matter. That said, many projects have
  provided very nice user interfaces via using Haskell's very strong
  web libraries.
* Going along with the above. we still have some gaps in our library
  ecosystem. Data science is not yet on the level of Python. I would
  _love_ to work with a team of data scientists to create the
  equivalent of Pandas, Numpy, and Scipy in Haskell (or better: have
  someone else do it instead :) ). Another weak point: our database
  libraries are slow. Work is already going on to improve this, but
  it's not complete yet. Finally, Haskell is still considered a
  second-tier language, and many companies do _not_ provide easy
  Haskell API bindings like they do for Java or Ruby. We're in
  catch-up mode on those sometimes. That said, projects like amazonka
  demonstrate that the Haskell communicate has both the will and the
  way to catch up in style.

pcj quote: Teh Haskell is no flaws at all, anyone who says you
shouldn't use it is simply ignoring the inherent beauty and universal
truth of its profunctor-based implementation.

Is Haskell perfect? Not by a long shot. It has strong points and weak
points, like any language. But its ability to let you write high level
code, get great performance from it, avoid bugs at compile time,
refactor (pcj: ) _fearlessly_, leverage its wonderful package
ecosystem, and overall just _get stuff done_ makes it, in my opinion,
the next language you should add to your toolchain, and the first
language you should consider for any new projects.
