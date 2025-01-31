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
