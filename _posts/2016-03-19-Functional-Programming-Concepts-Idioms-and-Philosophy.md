---
title: "Functional Programming: Concepts, Idioms and Philosophy"
date: 2016-03-19 10:06
categories: [code]
tags: [functional programming, python, scala, java]
---

[Functional Programming][fp] has risen as a solution to most modern days problems, such as concurrency and scaling. For some, it is a mystique concept that applies only to Erlang, Haskell and other strange languages that are either too complicated or irrelevant. This is simply not true, and I'm going to show how to apply some functional programming concepts on non-functional languages.

I'll deconstruct the functional paradigm by first defining what 'Functional Programming' really means, then explaining common idioms and comparing the syntax.
In the end, I'll show how to make relevant changes to non-functional in order to follow the Functional Programming Philosophy.

Please note that this post is mostly intended to those who never programmed functionally before and the goal here is to present Functional Programming as a practice not strict to language features, but as a philosophy that can, to some extent, be followed in any languages, enhancing code safety and bringing some of the advantages of FP to non-functional languages.

Defining 'Functional Programming'
---------------------------------

The simplest definition of Functional Programming is [pure functions][pure-functions] (or even simpler, _deterministic functions_).

This, on the other hand, can have several meanings and have to be carefully analyzed. It also leads to several other important rules to FP, such as immutable variables and composable functions.

### Deterministic Functions

As a general rule, think of your code as if you were writing a mathematical function:

  * Should its result vary according to anything that isn't supplied as a parameter? No
  * Should it alter any of the parameters that were supplied? No
  * Should it alter anything outside it's scope? No
  * Should the result be always the same for the same supplied parameters? __Yes.__

If, as you read this, you remembered a code that doesn't conform, I'm afraid it wasn't functional. Why?

Functions have a special meaning on those languages, such that they are __first class citizens__. They can be passed around as 'variables' and can be partially applied to compose new functionalities. I'll talk more about this later when I'll describe some idioms. The thing to have in mind here is that __functions are designed to be reusable and composable.__ Any side effect or external interference will make functions unpredictable and hardly reusable.

### Immutable Values

Stateful systems are very difficult to parallelize, having to implement mutexes, locks, semaphores and other forms of __access constraints__ to make the code safer. FP simply bashes the concept of mutability; instead, when coding in a functional language, you compose functions to obtain the values you need.

This is one of the concepts that are most difficult to picture, coming from a non-functional language. The good thing about it is that immutability forces you to rethink your problems in order to have a (functionally) correct solution for the problem. This should not be a problem once you understand that Functional Programming is much more __about how you design your functions than about how you design your data__.

Values, after all, a logical abstraction of your data, while functions are a logical abstraction of your business.

Some Idioms not Quite Familiar
------------------------------

Coming from OO to FP might be complicated if you have never seen a FP source code and how it looks like. You'll probably not be seeing the `for` and `if` commands you are used to, but instead, `map`, `reduce`, `filter` and `flatten`. You'll learn of the (not so complicated) [Monads][monad], Functors and [Algebraic Data Types][algebraic-scala]. What is all that supposed to mean and why can't the old structures be used?

I'll be using the [scala][scala] names for the idioms presented here, but they are all the same in functional languages.

### Monads, Functors and Friends

Monads are [amazingly simple][simple-monad] with amazingly complicated rules. Monads are containers. Period. There are rules that regulate how Monads work and interact with other Monads and this rules also define terms such as Functors, Monoids, Applicative Functors and other extraneous terms to those who never programmed in functional languages. For the sake of simplicity, I'll group them here as simply 'Monad', taking the risk of being semantically wrong to make you understand the concept. I promise I'll disambiguate this later, ok?

Let me show you some scala code to illustrate what this means:

```scala
def someComputation(): Option[String] = ...

val myPossibleString = someComputation()
```

Imagine, for example, that you can either have a value (in this example, a `String`) or not, as a result of some computation. Instead of the good ol' `null`, which can lead to serious problems such as the not-so-good ol' `NullPointerException`, you can return a `Option[String]`. An `Option` is a Monad that scala provides which wraps your data that allows you to safely interact with it __only if it exists__.

In a java code, you'd check for `null` before doing anything to avoid a NPE:

```java
String myPossibleString = someComputation();
if(myPossibleString == null) {
  //Short circuit out
}
return myPossibleString.toUpper();
```

In a functional programming language, you can `map` over the Monad to achieve the same safety level:

```scala
myPossibleString.map(_.toUpper)
```

The only difference here is that the java code will return a `String`, while the scala code will return an `Option[String]`.

By mapping a monad, we transform the cointained value while keeping the same container. The result of this can be either:

```scala
Some("MYUPPERSTRING") // if the computation was successful

// or

None // if the computation was unsuccessful
```

Note that `_.toUpper` won't break if `None` was supplied. This allows chaining operations on a value without having to short circuit all the possible problems:

```java
String myStr = someComputation();
if(myStr == null) {
  return false;
}
myStr = newOperationOnStr(str);
if(myStr == null || myStr.length < 3) {
  return false;
} else if (!matchRegex(str)) {
  return false;
} else {
  return true;
}
```

This code can be written as the following in scala, using `Option[String]`:

```scala
someComputation()
  .map(newOperationOnStr(_))
  .filter(s => s.length >= 3 && matchRegex(s))
  .isDefined
```

I could spend several hours here writing about how monads allow you to express your code better, in a functional way, but I'll leave that up to you.

### Algebraic Data What?

There is a complete mathematical theory backing this kind of data, but I'll humbly limit myself to explaining that algebraic data types are meaningful, composite types. An algebraic data type defined _by the sum of all definitions of its forms_. Some monads are defined as algebraic data types, such as scala `Options` we've seen before. It can be either `Some` or `None`. Each of those have different behaviors when mapped, filtered and reduced.

This means that while monads provide laws to data containers, algebraic data types provide __form__ and __meaning__. `Try` and `Option` are both quite similar in terms of API and can behave quite similarly, but what differs both is that you'll want to use `Try` when the error can be meaningful or several kinds of errors can be thrown, each requiring different actions. You can also use `Either`, if you must deal with two possible outcomes for a function. The range of monadic algebraic types is immense and they help you to structure your application without heavy, imperative concepts.

In general, using this kind of abstractions lead to a cleaner, meaningful code. Personally, I consider this kind of abstraction to be much higher and more valuable than Object Oriented programming. Of course OO has its value, but its much easier to express logic algebraic data structures.

But how can I start using it?
-----------------------------

While some languages provide native idioms to allow you write code aligned with the Functional Philosophy, there are several concepts that can be ported to non-functional (or natively non-functional) languages. It is important to note that writing code in a functional language doesn't make it immediately functional. What makes a code truly functional is the usage of aforementioned concepts. The same way you can have OO code in scala, you can have functional code in python, for example.

Imagine such situation, replacing the dummy functions with actual stuff:

```python
my_value = []

def fetch_values():
  # Imagine that you're fetching a real data
  # I'll return a dummy list of dummy ints
  my_value = [8, 3, 2, 5, 1, 4]

def filter_values(even=True):
  t = []
  for i in my_value:
    if even:
      if i % 2 == 0:
        t.append(i)
    else:
      if i % 2 == 1:
        t.append(i)
  my_value = t

def process_value(x):
  # Also dummy here.
  return x * x

def process_all_values():
  for i in my_value:
    process_value(x)

def do_process():
  fetch_values()
  # We (for some reason) need to process evens before odds
  filter_values(true)
  process_all_values()
  fetch_values()
  filter_values(false)
  process_all_values()
```

I broke all possible laws to make the example easier to transport. Hopefully, you'll be able to recognize some of the anti-patterns above on real, production code. We want to take them out. Below, I'll write a python code that fixes all of the above anti-patterns, but try to find them and imagine a functional approach before actually reading the transformed version.

```python
from functools import reduce

def fetch_values():
  return [8, 3, 2, 5, 1, 4]

def partition_values(vals):
  return reduce(lambda l, v: l[v % 2].append(v) or l, vals, ([], []))

def process_value(x):
  # Processing here is a pure function.
  return x * x

def process_all_values(lst):
  # Be cautious when plumbing functions here;
  # You should only map over pure functions, to avoid
  # intermittent state or unhandled errors.
  return map(process_value, lst)

def do_process():
  id_list = fetch_values()
  evens, odds = partition_values(id_list)
  p_evens, p_odds = process_all_values(even), process_all_values(odds)

  # Python `map` is lazy, so we force evaluation
  list(p_even)
  list(p_odds)
```

While not yet optimal due to python limitations, we have already a more functional code. There are tons of enhancements we'd be able to do here, such as using monadic algebraic types to wrap our mapped computations, allowing us to safely handle possible errors that happened during `process_value`.

Also, note that, while this slice of code is functional, it is only safe to do this __as is__ if `process_value` is a pure function. Otherwise you'll have problems such as indefinite state if an exception is thrown during this process or you'll have to reprocess all the stuff.

Some other concepts help you deal with this kind of problem and are orthogonal to FP, such as idempotence. It is important to know that functional programming (or the functional programming philosophy) is only a tool that helps you write safer code.

Conclusion
----------

You'll find that code that follows the functional philosophy is easier to test, to parallelize, to reuse and to understand. This, of course, doesn't mean that FP is the silver bullet that will solve all programming problems. If that was the case, I wouldn't be advertising the usage of those practices on non-functional languages, but rather advocating the migration to FP languages. The true value here is that you have an alternative to traditional imperative code style, which can result in the aforementioned advantages

This post got longer than I initially though and is only scratching the surface of FP. I'll surely write more about FP on future posts and I'll always focus more on the philosophy and how can you think functionally, even if your current language doesn't fully implement functional idioms. There are tons of useful techniques that one can borrow from functional programming which makes code safer, cleaner and more expressive.

Feel free to write me if you agree, disagree or just want to have a beer with me. I still don't have comments on my blog and this is something I'll take care of on the next week.
:x


[fp]: https://en.wikipedia.org/wiki/Functional_programming
[pure-functions]: http://www.sitepoint.com/functional-programming-pure-functions/
[monad]: https://en.wikipedia.org/wiki/Monad_%28functional_programming%29
[simple-monad]: http://maciejpirog.github.io/fishy/
[algebraic-scala]: http://io.pellucid.com/blog/abstract-algebraic-data-type
[scala]: http://www.scala-lang.org/
[neophytes]: http://danielwestheide.com/scala/neophytes.html
