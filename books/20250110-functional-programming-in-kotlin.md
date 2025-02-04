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

