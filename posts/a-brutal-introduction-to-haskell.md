# A Brutal Introduction to Haskell
## Introduction
Title shamelessly lifted from [Christopher Lane Hinson](http://blog.downstairspeople.org/2010/06/14/a-brutal-introduction-to-arrows/).

Below, find a quick (< 1000 word) taste of one thing that makes [Haskell](https://en.wikipedia.org/wiki/Haskell_%28programming_language%29) different from other programming languages. I assume you to have a modest level of experience in programming. You do not need to know anything about Haskell.

The abstract mathematical nature of Haskell is at the heart of its power and beauty, but in this article, we are going to get concrete quickly. You'll need to follow along with an interpreter, so either [use one in a browser](http://ghc.io), or (if you don't already have it) install the [Haskell platform](http://www.haskell.org/platform/). Enter the Haskell interpreter by bringing up a terminal and typing `ghci`.

Haskell is most commonly described as "functional", but if we are confined to choosing a single adjective, *"typeful"* is a better one-word descriptor. Haskell's functional purity certainly sets it apart from most popular programming languages, but it is amazingly powerful and flexible type system which makes worthwhile the constraints the Haskell places on the programmer. Once you learn to leverage it, you will miss it dearly when programming in other languages. In this article I introduce Haskell very briefly, with a focus on its type system.

## Tutorial
Haskell is strongly and statically typed, meaning that every expression in a valid program must have a type known to the compiler before the program can be run. This means, among other things, that you can easily check the type of any Haskell expression within ghci.

    :t "foo"

Now let us define a function. It will have a type, as well.

    let square x = x * x

This contrived example neatly dodges many issues I can't treat in a short article. Note that while the interpreter accepts this line of code, it is not legal at the top level of a Haskell source file.

We did not specify the type signature for our function `square`, but the interpreter is capable of inferring it. (An explanation of the mechanisms behind type inference is beyond the scope of this article.) View the type of `square` by running the following command:

    :t square

ghci replies `square :: Num a => a -> a`.

This is a *type signature*. The part before the `=>` symbol states that the type `a` must be an instance of the *typeclass* `Num`. The `->` symbol means something entirely different: the part of the type signature reading `a -> a` indicates that square takes an argument of type `a` and produces a result of the same type `a`.

Above I didn't tell you what a typeclass is; nor will I below, in order to keep this article shorter than one thousand words. However, I will tell you that you can learn a little about `Num` by asking ghci for information:

    :i Num

Think of the output as describing the contract that an otherwise arbitrary type `a` must satisfy in order to be a member of the `Num` typeclass. Several types implementing this interface are listed in ghci's response to the query `:i Num`. There are other instances of `Num`, and many other typeclasses, in various Haskell libraries. You can write your own instances of standard typeclasses, as well as your own typeclasses.

When you write Haskell programs of your own, it is good practice to include type signatures in your source code, even though they are not required. However, you can frequently save time by defining the function, asking the compiler for the inferred type, and pasting the result into your code. (A good text editor will allow you to do this with a few keystrokes.)

ghci was able to infer the type of our user-defined function `square` with no hints whatsoever other than the function definition. So it's unsurprising that ghci also knows the type of standard library functions. For example:

    :t id

ghci replies `id :: a -> a`. This type signature states that the identity function takes an argument of an arbitrary type `a` and produces a result which is of the same type `a`.

`id`'s type looks quite similar to the type of our `square` function; but note the absence of any typeclass constraint. The argument to `id` can have *any* type whatsoever, in contrast to `square`, the argument of which must be a number. Recall the definition and type of `square`:

    let square x = x * x
    :t square

which yields `square :: Num a => a -> a`. Now, let us examine the type of (*):

    :t (*)

which yields `(*) :: Num a => a -> a -> a`. (It turns out that in Haskell, operators are simply functions with an infix syntax. This is yet another topic I won't explain in this article.)

As you can see, (*) requires its (two) arguments to be numbers as well. Imagine an argument to `square` with the type `String`; for concreteness `let x = "foo"`. Haskell doesn't know how to multiply `"foo" * "foo"`, because `String` is not an instance of the typeclass `Num`. Therefore, the expression `square "foo"` contains a type error, and will be caught and rejected at compile-time. Try it yourself and examine ghci's output:

    square "foo"

So the `Num` constraint on type `a` in the type signature for `square` is necessary to guarantee that multiplying `x` times itself is a legal, type-safe operation.

We need no such guarantee for `id`. Consider its definition:

    let id x = x

We perform no operation on `x`, so type inference does not impose any constraint on the type of `x`.

## Moral
1. Follow the types. Trust the types.

2. Use ghci to ask for type and other information with `:t` and `:i`. This doesn't work everywhere, but it does work in more situations than you might expect.

## Future Study
I hope this bare exposition, with its many unanswered questions, has whetted your appetite rather than frustrating you. For a fleshier introduction to Haskell, read [Learn You a Haskell for Great Good!](http://learnyouahaskell.com/chapters), or dive right into a serious project and [Write Yourself a Scheme in 48 Hours](https://en.wikibooks.org/wiki/Write_Yourself_a_Scheme_in_48_Hours).
