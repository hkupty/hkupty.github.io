---
title: "Understanding the Math behind FP: The Monads"
date: 2016-04-10 15:53
categories: [code]
tags: [functional programming, python, scala]
---

On my [previous post][previous] I talked a little about Functional Programming, hoping to introduce it to newcomers and developers with no previous FP experience.

Several people said it was not very precise and it didn't explained some concepts (such as Monads or Functors) with the required level of details.
Well, I do hope to achieve some detailing with this post. Nonetheless, I'll still try to keep it light to those who never programmed functionally before.

Together with the aforementioned post, I hope to make more people interested the concepts of FP and how they can be applied on "non-functional" languages.
I insist on detaching the functional programming concept from specific languages, as most of its concepts can (and sometimes are enforced as best practices) on any language.

I'll make a series of posts to explain the constructs, starting with Monads today, and will try to port (or implement) them in a non functional language.

I'll explain the structures in scala and write python code only as demonstration of equivalence in other languages. I do not expect the python code to be production-class, but instead to be a mean to learn FP.

Also, if you'd like to see how the concepts would look like in other languages or have ideas for the next topics, don't hesitate to write me.


Monads and their laws
---------------------

Monads, as explained before, are containers. For OO developers, the look very much like [Generics][generics]. In fact, they're a more specialized version of generics, with [specific laws][monad-laws]. This laws exist to allow you to expect always the same behavior when dealing with monads:

  * Left Identity;
  * Right Identity;
  * Associativity.

Before explaining the laws, I'll also define the set of operations a type must have to be a Monad, with a generic signature on the side, with `arguments -> return`:

  * the return `a -> M[a]`;
  * the Bind function `(M[a], a -> M[b]) -> M[b]`

While the type constructor and the return function seem pretty obvious, the bind function (called `flatMap` on the scala dialect) needs a bit of explanation:
It is a function that takes a Monad of `a` and a function that takes `a` and returns a Monad of `b` to return a Monad of `b`.

This can be written in code as the following:

```scala
val m: List[Int] = List(12, 34)
// m is our Monad, bein List a monadic type

val n = m flatMap (i => i.toString)
// toString is an operation that returns a String, which can be also read as a list of characters

n == List[Char]('1', '2', '3', '4')
```

Why? The function that receives an Int and returns a String (list of characters) conforms with the signature of `bind`, defined above. It takes a value (nth position in the list) and a function that takes this value and produces a monad (of the same type of the monad we're working with) with the result of the supplied function.

With this explained, lets move on to the laws:

### Left Identity

This law states that `(return a) bind f == f a` or `Create a monad with 'a' and bind it with 'f' is the same as calling 'f(a)'`.
We can prove this law in:

```scala
def toListString: Int => List[String] = i => List(i.toString)

val leftProperty = List(1) flatMap (toListString)
val rightProperty = toListString(1)

leftProperty == rightProperty
```

### Right Identity

This law states that `m bind return == m` or `Binding return on a monad returns the same monad`.
We can prove this law in:

```scala
val monad = List(1)

val boundValue = monad flatMap (List(_))

boundValue == monad
```

### Associativity

This law states that `m bind f bind g == m bind (i -> f(i) bind g)` or `Binding f to a monad and then binding g is the same as binding a function that produces a monad using f bidning g to the result to the same monad`.

It might sound a bit complicated, but we can prove this law in:

```scala
def double(i: Int): List[Int] = List(i * 2)
def triple(i: Int): List[Int] = List(i * 3)

val monad = List(1)

val leftProperty = monad flatMap double flatMap triple
val rightProperty = monad flatMap {i => double(i) flatMap triple}

leftProperty == rightProperty
```

Your very first Monad in Python
-------------------------------

Below I have defined a Monad class in python, exposing the laws defined above and some exemples of usage.

```python
## Monad
class Monad(object):

  """A Monadic container in python."""

  def __init__(self, containedValue):
    """init is our 'return' function here."""
    self.__value = containedValue

  def flat_map(self, f):
    return f(self.__value)

  # Overriding '==' so we can prove the laws
  def __eq__(self, o):
    return isinstance(o, type(self)) and o.__value == self.__value

  def __repr__(self):
    return "{}({})".format(self.__class__.__name__, self.__value)

## Helper functions

def to_monad(i):
  return Monad(i)

def to_doubled_monad(i):
  return Monad(i * 2)

## Proving the laws
# Left Identity
Monad(1).flat_map(to_doubled_monad) == to_doubled_monad(1)
Monad("something").flat_map(to_doubled_monad) == to_doubled_monad("something")

# Right Identity
m = Monad(1)
m == m.flat_map(lambda k: Monad(k))

m2 = to_monad({1: 2})
m2 == m2.flat_map(to_monad)

# Associativity
m = Monad(1)
left = m.flat_map(to_monad).flat_map(to_doubled_monad)
right = m.flat_map(lambda k: to_monad(k).flat_map(to_doubled_monad))

```

Your second Monad in Python
---------------------------

As depicted in the example above, the `Monad` type dosn't do anything special and acts just as a container, wrapping the real value. Some meaningful classes can be created deriving from Monad, such as the Try Monad below:

```python
class Try(Monad):

    class Success(Monad):
        pass

    class Failure(Monad):
        pass

    def flat_map(self, f):
        try:
            return super(Try, self).flat_map(f)
        except Exception as e:
            return Try.Failure(e)
```

Please note that this implementations are rudimentary and my lack type-correctness, but still are interesting enough for us to explore Monadic Laws.

Conclusions
-----------

I know we're still missing some important answers such as "why would I need a monad" and "How am I supposed to work with all this" but, again, this is the first (actually the second) of a series of articles about functional programming, with the sole purpose sheding some FP light to those who are interested but think FP is very complicated or might not be needed.

I'll write several more of those, covering other mathematical topics deeply related to FPsuch as functors, monoids and applicative.
I might delay this posts as I'm having problems with my ISP, so if you're eager to learn for more, I recommand the excellent [Learn you a Haskell for a greater good][haskell]

I hope you enjoyed this one and if you didn't like (or don't agree with) anything I wrote, feel free to tweet me or send an e-mail.

:x

[previous]:     http://hkupty.github.io/2016/Functional-Programming-Concepts-Idioms-and-Philosophy/
[generics]:     https://docs.oracle.com/javase/tutorial/extra/generics/
[monad-laws]:   https://wiki.haskell.org/Monad_laws
[haskell]:      http://learnyouahaskell.com/a-fistful-of-monads
