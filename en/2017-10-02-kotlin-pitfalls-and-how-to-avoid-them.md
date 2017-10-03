> Kotlin is all the rage lately and while I do agree that the language is well thought out it has - as everything else - its flaws.
> In this article I'll explain some of the pitfalls I encountered and try to help you avoid them.

## The mystery `null`

In Kotlin you can write your code as if `null` never existed and this can make you forget that `null` is omnipresent
but it hides. Let's look at this simple and seemingly innocent class:

```kotlin
class Foo {
    private val c: String

    init {
        bar()
        c = ""
    }

    private fun bar() {
        println(c.length)
    }
}
```

If you try to instantiate this you'll get a `NullPointerException` because `bar` tried to access the `length` of `c` before it was initialized.
Of course the application logic was flawed here but you still got an `Exception`. The worst part of this is that the IDE won't complain about this.

The takeaway here is that Kotlin *will help* you in a lot of cases (nearly all) to avoid `null`, you can't forget about it and from time to time
you'll encounter things like this.

## Handling `null`s from the JDK

Kotlin's standard library handles `null`s fine but if you use classes from the JDK you will have to handle possible `null` pointers from
library functions by hand. Most of the time the Kotlin classes are enough, but sometimes you have to use something like the `ConcurrentHashMap`.

```kotlin
val map = ConcurrentHashMap<String, String>()

map["foo"] = "bar"

val bar: String = map["foo"]!!
```

In this case you have to use the `!!` operator but in other cases the `null` safety operator (`?`) can also work.
Nevertheless if you use Java libraries extensively you'll have to litter your code with `!!`s and `?`s or write adapters for Java classes.
This is something you can't really avoid.

## The inner `it`

When a lambda has a single parameter you can omit it from your code and can use `it` instead:

> it: implicit name of a single parameter
> One other helpful convention is that if a function literal has only one parameter, its declaration may be omitted (along with the ->), and its name will be it
> -- Kotlin docs

The problem with this is when you have nested functions like in this example:

```kotlin
val list = listOf("foo.bar", "baz.qux")

list.forEach {
    it.split(".").forEach {
        println(it)
    }
}
```

It only takes a few `it`s to lose track which is which. The solution to this problem is to name the parameters explicitly:

```kotlin
list.forEach { item ->
    item.split(".").forEach { part ->
        println(part)
    }
}
```

Much better!

## The insidious `copy`

Take a look at this `data class`:

```kotlin
data class Foo(val bars: MutableList<String>)
```

Data classes give you a bunch of functions and you can also make a `copy` of them. Guess what this will print out:

```kotlin
val bars = mutableListOf("foobar", "wombar")

val foo0 = Foo(bars)

val foo1 = foo0.copy()

bars.add("oops")

println(foo1.bars.joinToString())
```

This will print `foobar, wombar, oops`. The problem is that while the name indicates that `copy` will make an *actual copy* in fact it will only
copy the references in your object. This can be insidious if you forget to write an unit test and you pass your `data class`es around as if they
were immutable [value object](https://martinfowler.com/bliki/ValueObject.html)s.

The solution to this problem is to pay attention to your `data class`es and if they should be value objects make them one:

```kotlin
data class Foo(val bars: List<String>)
```

> There is an other problem with `data class`es: you can't tell Kotlin which fields you want to put in your `equals` / `hashCode`, you can only
> `override` both and write them by hand. Keep this in mind.

## Internal leakage

Take a look at this example:

```kotlin
    class MyApi {

        fun operation0() {

        }

        internal fun hiddenOperation() {
            
        }
    }
```

If you use classes like this from other Kotlin projects the `internal` keyword will be respected. If you look at this
from a Java project however `hiddenOperation` will be `public`! To avoid this I'd suggest using `interface`s to hide
implementation details:

```kotlin
interface MyApi {

    fun operation0()
}

class MyApiImpl: MyApi {

    override fun operation0() {

    }

    internal fun hiddenOperation() {

    }
}
```

## Non-generic global extensions

The utility of extension functions is unquestionably high, but with great power comes great responsibility.
You can - for example - write extension functions to JDK classes which will be visible for the whole project.
This can be problematic when they are non-generic and represent operations which only make sense in a local context:

```kotlin
    fun String.extractCustomerName() : String {
        // ...
    }
```

Now everybody on your project will scratch their heads when they bump into this.
I'd suggest thinking twice before writing extension functions to JDK classes.

## Unit returning lambdas vs Java SAM conversion

When you have functions which accept lambdas you can omit the `return` keyword if the lambda's return type is `Unit`:

```kotlin
fun consumeText(text: String, fn: (String) -> Unit) {

}

// usage
consumeText("foo") {
    println(it)
}
```

This is fine but if you call this from Java you'll face the awkward problem of needing to return `Unit`:

```java
consumeText("foo", (text) -> {
    System.out.println(text);
    return Unit.INSTANCE;
});
```

This is very clunky from the Java side. If you try to make this work from Java you can define an `interface`:

```kotlin
interface StringConsumer {
    fun consume(text: String)
}

fun consumeText(text: String, fn: StringConsumer) {

}
```

then you can use Java's SAM conversion to make this very simple:

```java
consumeText("foo", System.out::println);
```

but then from the Kotlin side it becomes a mess:

```kotlin
consumeText("foo", object: StringConsumer {
    override fun consume(text: String) {
        println(text)
    }
})
```

The problem is that Kotlin does not support SAM conversion for Kotlin classes and it only works with Java classes. My suggestion is that
for simple cases just use Java's built in SAM interfaces like `Consumer`:

```kotlin
fun consumeText(text: String, fn: Consumer<String>) {

}

// usage from Kotlin
consumeText("foo", Consumer {
    println(it)
})

// usage from Java
consumeText("foo", System.out::println);
```

## Java interop with unmodifiable Collections

Kotlin gives you immutable variants of the JDK's collection classes:

```kotlin
fun createSet(): Set<String> = setOf("foo")

// ...
createSet().add("bar") // oops, compile error
```

This is a very nice addition but if you look at this from the Java side you'll see the JDK's `Set` api:

```java
createSet().add("bar"); // UnsupportedOperationException
```

If you try to modify this `Set` the same will happen as if you have used Java's `Collections.unmodifiableSet()` method.
I don't know whether this can be (or should be) worked around but this is something you can keep in mind when working with Kotlin collections.




