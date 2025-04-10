# Functional Programming in Kotlin

**Authors:** Marco Vermeulen, Rúnar Bjarnason, Paul Chiusano

## Notes

### Chapter 1

**Function has no side effects** if it has no observable effect on the execution of the program other than to compute a results given its inputs.

**Procedure** is often used to refer to a parameterized chunk of code that may have side effect.

### Chapter 2

#### Higher-order functions
**Higher-order functions** are functions that accept other functions as arguments.

#### Tail calls

[Tail calls in Kotlin](https://kotlinlang.org/docs/functions.html#tail-recursive-functions)

> You can use a recursive function instead without the risk of stack overflow. When a function is marked with the tailrec modifier and meets the required formal conditions, the compiler optimizes out the recursion, leaving behind a fast and efficient loop based version instead

#### Passing a function parameter
There are several ways of passing function parameters.
1. Passing a callable reference

```
formatResult("factorial", 7, ::factorial)
```

`::factorial` is a shortcut from `this::factorial`.

2. Passing a function literal (anonymous function or lambda)

```
formatResult("absolute", -42, fun(n: Int): Int { return if (n < 0) -n else n})
```

this might be simplified

```
formatResult("absolute", -42, { n -> if (n < 0) -n else n})
```

if lambda function has only one parameter, if can be even replaced with the implicit [convenience parameter `it`](https://kotlinlang.org/docs/lambdas.html#it-implicit-name-of-a-single-parameter)

```
formatResult("absolute", -42, { if (it < 0) -it else it})
```

#### Extension methods and properties

```
fun Int.show(): String = "The value of this Int is $this"
```

The new method is now available on all instances of `Int`.

```
1.show()
```

Properties on all instances also can be extendies

```
val Int.show: string 
    get() = "The value of this Int is $this"


1.show
```

The extension methods and properties are dispatched statically - we're not modyfying the underlying class.
An extension function is determined by the type **of the expression on which the function is invoked**, not by the type of the result evaluating that expressin at run time. 

More on extensions in the [Kotlin documentation](https://kotlinlang.org/docs/extensions.html).

### Chapter 3

#### Compation object

Any function defined within the `compation object` block can be invoked like a static method in Java.

More on companion objects in the [Kotlin documentation](https://kotlinlang.org/docs/object-declarations.html#companion-objects).

#### Variadic functions

A variadic function is a function that accepts zero or more arguments of type `A`. In no arguments are provided, it results in a `Nil` instance of a class. If arguments are provided, the method will return an object representing those values.
 d
```
fun <A> of(vararg aa: A): List<A> {
    val tail = aa.sliceArray(1 until aa.size)
    return if (aa.isEmpty()) Nil else Cons(aa[0], List.of(*tail))
}
```

The `*tail` expression (spread operator) means that an array has been passed as a variadic parameter. The spread operator passes each element of the array as individual arguments.

More on passing array arguments in the [Kotlin documentation](https://kotlinlang.org/docs/arrays.html#pass-variable-number-of-arguments-to-a-function).

### Chapter 4

#### Lifting functions
To lift a function means to transfor a function to operate within the context of a single `Option` value:

```
fun <A, B> lift(f: (A) -> B): (Option<A>) -> Option<B> =
    { oa -> oa.map(f) }
```

#### For-comprehension

[Why Do Functional Programmers Prefer For-Comprehension Over Imperative Code Block](https://edward-huang.com/scala/functional-programming/2021/11/30/why-do-functional-programmers-prefer-for-comprehension-over-imperative-code-block/)

[List comprehensions in functional programming](https://mathspp.com/blog/twitter-threads/list-comprehensions-in-functional-programming)

[Bottom of the Rabbit Hole: for-comprehensions and monads](https://murraytodd.medium.com/bottom-of-the-rabbit-hole-for-comprehensions-and-monads-7f592a53c73c)

#### Suspended Function

A blocking operation that might be suspended on the execution of a long running child process is named `suspending function`.
It's a regular Kotlin function with a `suspend` keyword.

More on `suspend` in the [Kotlin documentation](https://kotlinlang.org/docs/flow.html#suspending-functions).


### Chapter 5

#### Lazy evaluation 
*Non-strict* or *lazy* evaluation means that the value of an expression is only evaluated at the point it's referenced. 
The code is no longer greedy in performing all the calculations but instead compute on demand. 

*Non-strictness* is a property of a function. The function that is non-strict chooses if it wants to evaluate one or more of its arguments.
Strict functions are the norm in most programming languages. 
The unevaluated form of an expression (e.g. `() -> A`) is called a *thunk*. You can force a thunk to evaluate the expression. 

If you wish to calculate a thunk reult only once, you can cache the value explicitly by delegating to the `lazy` build-in function when assigning a new value:

```
fun maybeTwice2(b: Boolean, i: () -> Int) {
    val j: Int by lazy(i)
    if (b) j + j else 0
}
```

This approach defers initialization unitl `j` is referenced by the `if` statement. 

#### Smart constructors

Smart constructors provide a slightly different signature than the "real" constructor. By convention, they live in the companion objects of the base class. Their names typically lowercase the first letter of the corresponding data constructor.

```
fun <A> cons(hd: () -> A, tl: () -> Stream<A>): Stream<A> {
    val head: A by lazy(hd)
    val tail: Stream<A> by lazy(tl)
    return Cons({head}, {tail})
}
```

This is a common trick that ensures that thunk will do its work only once when forced for the first time. 

#### Description vs evaluation

Laziness lets separate the description of an expression from the evaluation of that expression. You can describe a large expression and the evaluate only a portion of it. 

#### Corecursive Function

Corecursive function, in the opposite to the recursive function that consumes data, produces data. Recursive functions terminate by recursing on smaller inputs, corecursive functions need not to terminate as long as they remain productive (they produce data).
An example of a corecursive function is `unfold`.
Corecursion is also sometimes called *guarded recursion*, and productivity is sometimes called *cotermination*.


### Chapter 6

#### Purely functional state management

You don't update the state as a side effect but simply return the new state along with the value.

```
interface RNG {
    fun nextInt(): Pair<Int, RNG>
}
```

This can be written as `(RNG) -> Pair<A, RNG>` for some type `A`. A function of this type is called `state action` or `state transition` because it transforms the `RNG` state. Such actions can be combined using `combinators`, which are higher-order functions. 

It's pretty tedious and repetetive to pass the state along by ourselves, we want combinators to automatically pass if from one action to the next. 

**The idea is simple - use a pure function that accepts a state as its argument and returns the new state alongsite its result. Us the new state to process another value.** Thanks to that, state transitions are explicit. 

#### Dealing with awkwardness in functional programming
Awkwardness is almost always a sign of some missing abstraction waiting to be discovered.
Once encountered, plow ahead and look for common patterns you can factor out. Most likely, some had already had such a problem.

### Chapter 7

> This chapter is not on how to write a library for purely functional parallelism but on how to approach the problem of designing a strictly functional library


Kotlin evaluates strict arguments from left to right. If you want to postpone execution, use lazy parameters.


In functional programming defer execution to the latest possible moment. Everything before the execution should just describe what the execution looks like.


1. implement sa imple example
1. explore the example
1. make a design choice and discover its consequences
1. learn something about the nature of the problem domain
1. improve the design
1. reiterate the process


> The overall design process is a series of small adventures. You don't need a special license to do such exploration. Just dive in and see what you can find.


**Derived combinator** (e.g. `lazyUnit`) - derives execution, as opposed to **primitive combinator** (`unit`).
Primitives are simple combinators that don't depend on others. They provide building blocks for more complex higher-order combinators.
A combinator is said to be context sensitive when it passes on state, allowing sequencing of combinators. 

### Chapter 8

Property-based testing is a software testing approach that generalizes traditional example-based tests by focusing on properties (or invariants) that should hold true for a wide range of inputs.

The main concepts of property based testing:
1. **properties** - high-level rules or behaviours that must always hold true
    1. A sorted list should always have elements in ascending order.
    1. Adding an element to a set should not create duplicates.
2. **generators** - tools that create random or systematically varied input data for tests,
3. **shrinking** -  when a test fails, shrinking reduces the input to its simplest form that still causes the failure, making debugging easier.

### Chapter 9 

Algebraic library design establishes the interface with associated laws up front and then drives implementation. 
The way users interact with a library might be more important than the actual implementation - API-driven.

Starting design with the algebra lets combinators specify information to the implementation. 

### Chapter 10

A monoid consists of the following:
1. sompe type `A`,
1. a binary operation `combine` that takes two values of type `A` and combinates them into one `combine(combine(x, y), z) == combine(x, combine(y, z))`,
1. the value `nil: A` which is an identity for that operation: `combine(x, nil) == x`.

**A monoid is an algebra and nothing more.**

Ad hoc polymorphism - you can apply polymorphic functions to arguments of different types.

A *type class* is a type system construct that can be used to implement ad hoc polymorphism. It does so by adding constraints to type variables in parametrically pholymorphic types. 
A type class `T` and a type variable `a` means `a` can only be instatantiated to any type whose members support the overloaded operations associated with `T`.

Example: `T` can represent Monoid, which takes the type parameter `A`, e.g. `String`. You instantiate a new instance of `Monoid<String>` that represents a monoid instance `a`. This monoid instance will now have the ability to combine `String` instances without being coupled to or constrained by the `String` class itself.

#### Balanced fold
Instead of 
```combine(combine(combine(a, b), c), d)```
you would do 
```combine(combine(a, b), combine(c, d))```
Inner combine calls are independent so they can easily be parallelized. 

#### Higher-kinded types
These types represent a combination of more nested types:

```
interface Foldable<F<A>> {
    // impl
}

object ListFoldable: Foldable<List<A>> {
    // impl
}
```

They allow abstraction of operations to promote code reuse across multiple implementations and are types that take other types to construct new types.

#### Semigroup
A monoid is not the smallest unit in functional programming. It consits of the `combine` method and `nil` value. The `combine` method is known as **semigroup** anc can be defined as following:
```
interface Semigroup<A> {
    fun combine(a1: A, a2: A): A
}
```

A monoid is a combination of a semigroup and with the `nil` value. 

### Chapter 11

#### Functor

A functor is a data type that implements `map`:

```
interface Functor<F> {
    fun <A, B> map(fa: Kind<F, A>, f: (A) -> B): Kind<F, B>
}
```

*Type constructor* is applied to a type to produce another type. `List` is a type constructor, not a type. There are no values of type `List`, but you can apply it to the type `Int` to produce type `List<Int>`. The `Functor` interface is parametric in the choice of `F`. 

```
val listFunctor = object : Functor<ForList> {
    override fun <A, B> map(fa: ListOf<A>, f: (A) -> B): ListOf<B> = fa.fix().map(f)
}
```

That type of a constructor like `List` is a functor, and the `Functor<F>` instance constitutes proof that `F` is a functor.

The `Functor` interface has a `map` method, a higher-order function that applies a transformation to each element of the enclosing kind.
The functor law stipulates the relationship between `map` and identity functions. It preserves the structure of the enclosing kind and is only concerned with transforming its elements.

#### Monad

`Monad<F>` is a `Functor<F>`

**A monad is an implementation of one of the minimal sets of monadic combinators, satysfying the laws of associativity and identity.**

##### Monadic Combinators
Three monimal sets of primitive monadic combinators, and instances of `Monad` will have to provide implementation of one of these sets:
1. `flatMap` and `unit`,
1. `compose` and `unit`,
1. `map`, `join`, and `unit`.

##### Associative Law

```
combine(combine(x, y), z) == combine(x, combine(y, z))
```

##### Identity Law
This is not a single law but a pair of laws, reffered to as *left identity* and *right identity*.

```
fun <A> unit(a: A): Kind<F, A>

compose(f, {a: A -> unit(a)}) == f
compose({a: A -> unit(a)}, f) == f
```

This can also be expressed in terms of `flatMap`:

```
flatMap(x) {a -> unit(a)} == x
flatMap(unit(a), f) == f(a)
```

You could say that monads provide a context for introducing and binding variables and allowing variable substitution.

##### Partial Type Application

Consider you have a `State` that indexes by `Int`:
```
typealias IntState<A> = State<Int, A>
```

`IntState` is exactly the kind of thing for which you can build a `Monad`. However, you would need several such monads - `IntState`, `DoubleState`, `StringState`, etc. It doesn't scale well. Instead you can introduce the `StateMonad` interface, which can be partially applied with a type such as `Int`, resulting in a `StateMonad<Int>`:

```
interface StateMonad<S>: Monad<StatePartialOf<S>> {
    override fun <A> unit(a: A): StateOf<S, A>

    override fun <A, B> flatMap(
        fa: StateOf<S, A>,
        f: (A) -> StateOf<S, B>
    ): StateOf<S, B>
}
```

Monads all have `flatMap` and `unit` , and each monad brings its own set of additional primitive operations specific to that monad.