<div id="tldr">
If you are a Java developer for a while now you might be wondering what to learn next.
There are a bunch of languages out there which worth a look, like <a href="https://clojure.org/">Clojure</a>,
 <a href="https://www.rust-lang.org/en-US/">Rust</a> or <a href="https://www.haskell.org/">Haskell</a>.
But what if you want to learn something with which you can pay the bills but it is not a pain to use?
Kotlin is in the sweet spot just where Java used to be and in this article my goal is to explain why.
</div>

## So what is Kotlin?
- A home-grown programming language by [JetBrains](https://www.jetbrains.com/) who are the masterminds behind the acclaimed [IDEA](https://www.jetbrains.com/idea/) IDE and a bunch of other stuff.
- A simple and flexible alternative to Java
- Which interoperates well with existing Java code
- Compiles to Java bytecode
- Runs on the JVM
- And also compiles to javascript

If you read the docs you can see a bunch of stuff going for it:
- It lets you achieve more with less code
- Solve a lot of problems in Java
- Helps you keep using the Java ecosystem
- Lets you write front-end and back-end code in the same language
- Gives you 100% Java interoperability
- It does well compared to the alternatives (Clojure, Scala)
- Adds only a thin layer of complexity over Java

Sounds cool, right? Let's just not drink the Kool-Aid too soon and see some examples how well it fares compared to Java.

## Value objects vs data classes
What you see here is a POJO with all the boilerplate:

{% gist 0c86e6a2c6bb91f44983c5d9fd342117 %}

Creating value objects is really cumbersome even with the usage of libraries like Lombok (Lombok needs you to install a plugin to your IDE in order for it to work which might not be an option for all IDEs. It can be worked around with tools like Delombok but it is a hack at best. Read more [here](https://projectlombok.org/features/delombok)) At least IDEA (or Eclipse) gives you a little help with generating a lot of these methods but adding a field and forgetting to modify the `equals` method will lead to nasty surprises. Let's look at the Kotlin equivalent:

{% gist 306e9222179ef822fa7ce852ac2c4d9b %}

Whoa! That's quite less typing compared to the Java version. Data classes in Kotlin give you
- `equals` + `hashCode` and
- `toString` in addition to
- getters and setters.
You can also `copy` them which effectively creates a new object with some fields overwritten. See [here](https://kotlinlang.org/docs/reference/data-classes.html) for more information on this topic.

## String interpolation
String manipulation in Java is painful. It can be alleviated by using `String.format` but
it will still remain ugly.

{% gist 12b7888d78ddda93144aebddac1d779e %}

Kotlin works around this by adding [String interpolation](https://stackoverflow.com/questions/37442198/how-does-string-interpolation-work-in-kotlin) to the mix with which it is a bit simpler to
use variables in String literals. You can even call methods from one!

{% gist eeb7dd12e75d1f4013f76b4dd00529f4 %}

## Extension functions
Writing decorators in Java can be tricky and they are not perfect. If you want to write a decorator which
can be used with all classes implementing `List` you can't simply use it in your decorator because it would need you to
implement a lot of other methods so you have to extend `AbstractList`.

{% gist 6e2b6932521ca83ae8e156ce016d9a3a %}

If you need to decorate something which does not provide useful base classes like `AbstractList`
or is a `final` class then you are out of luck. Extension methods come to the rescue!

{% gist 96322e2c30883b24c3f46a53777ca568 %}

This method acts as a decorator for all `List`s. Compared to the Java alternative this one-liner is much simpler and it will
also work for `final` classes. Just try not to [abuse](https://www.philosophicalhacker.com/post/how-to-abuse-kotlin-extension-functions/) them.

## Null safety
Checking for `null` values involves a lot of boolean expressions and a lot of boilerplate. With the advent of Java 8
you can finally work around this with the `Optional` class but what if the reference to an `Optional` is `null`?
Yes, you'll get a `NullPointerException` and after 20 years of Java we still don't know **what** was null.
Take the following example:

{% gist 074849246682406ac6faaac78461bae9 %}

With Kotlin you have several options. If you have to interop with Java projects you can use the null safety operator (`?`):

{% gist a4ffb5d187a893ac59af7b349ef399b3 %}

The code will only run after a `?` if its left operand is not `null`.
The `let` function creates a local binding for the object it was called upon
so here `it` will point to `it.city`.
If you don't have to interop with Java I would suggest doing away with `null`s completely:

{% gist 7c3530e7257e161d475755dd4bc0c504 %}

If there are no `null`s involved (no `?`s present) it all becomes a lot more simpler.

## Type Inference
Kotlin supports type inference which means that it can derive types from the context in which they are present.
This is like the Java diamond notation `<>` but on steroids! Take the following example:

{% gist 5416e0d1aa9134b7036ff2af09e207a1 %}

This looks almost the same in Kotlin:

{% gist d81a7681415e4988c72fd23fd923efcd %}

Until you let Kotlin figure out the types of your variables:

{% gist 58c106ff475e9c261ac39cfd76b602ae %}

or even methods:

{% gist a83328555586f45efeb523c440fc17d3 %}

## No checked exceptions
You must have seen this piece of code at least a million times:

{% gist d51b2eb08d5be1908e93c2daf69b78a4 %}

Old school IO in Java. Note the try with resources block!
The same would look like this in Kotlin:

{% gist ec2e31718bbe0343dd17ca3037ebe3b8 %}

There are a couple of things going on here. First Kotlin does away with checked exceptions. Secondly Kotlin adds
`use` to any `Closeable` object which basically:

> Executes the given [block] function on this resource and then closes it down correctly whether an exception is thrown or not.
> (Taken from Kotlin's documentation)

What you can also see here is that an extension function (`readLines`) is added to the `File` class. This pattern is visible throughout Kotlin's rather small standard library. If you have ever used Guava, Apache Commons or something similar, chances are that you will see common functionality from them added to a JDK class as an extension function. Needless to say this will be good for your health (nerves at least).

## Lambda support
Let's look at the lambda support in Java:

{% gist 9276ed18fec50fa4bdf030139e813572 %}

Since there is no syntax for method parameter types we have to create an interface for it.
Note that we could use `Function<String, Boolean>` here but it only works for functions with one parameter!
There are some interfaces in the JDK to solve this problem but if someone looks at the code they might be puzzled what
a `BiFunction` is useful for?
Kotlin improves on this a bit:

{% gist 392370158e68247cddf7585cee92575b %}

Kotlin adds a syntax for passing functions as parameters: `(ParamType1, ...ParamTypeN) -> ReturnType`.
And with Kotlin you have method and field references and you can also refer to a method from a concrete object!
Using the example above I can refer to the `filterBy` function on a concrete instance like this:

{% gist 08584eff41be9c03e84ebd1bc1532ea8 %}

## Functional programming
Functional programming is all the buzz nowadays and with Java 8 they have released Oracle's take on the topic: the Stream API.
It works like this:

{% gist 71b953cf1610fcd80584d738e8025cd1 %}

The Kotlin equivalent is rather similar, but subtly different:

{% gist 09491cc6059659f270b2820f1a817cd6 %}

There is no explicit conversion to streams since all Kotlin collections support it out of the box.
Not having to pass a lambda to `flatMap` here is a direct consequence of this.
Collecting the result is also automatic (no need for `Collectors.to*` method calls).
We only had to use `toSet` here because we want to return a `Set`. Otherwise `.toSet()` can be omitted.

## Kotlin-java interoperation
Well this can be a dealbreaker for most people but JetBrains got this right:

{% gist 085d8a371f87eaca5cf28cab1aef5ee5 %}

{% gist cf6b7c6eb5ff4c9e20476a8a3d7b112c %}

The interop is seamless and painless. Java and Kotlin can live together in the same project and Kotlin supplies a set of annotations (like `@JvmStatic` here) so Kotlin code can be called from Java without any fuss. Check [here](https://kotlinlang.org/docs/reference/java-interop.html) for further information on this topic.

As you might have seen from these examples Kotlin takes the good ideas from Java, improves upon them and tries to keep the WTF/minute counter to a minimum. The recent news about Google making Kotlin one of the supported languages on Android also underpins this.

## Things to tell your boss
So if you want to give it a try here are some pointers which will help you when negotiating with your boss and teammates:

- Kotlin comes from industry, not academia. It solves problems faced by working programmers today.
- It is free and Open Source
- It comes with a useful Java to Kotlin converter tool
- You can mix Kotlin and Java with __0 effort__
- You can use *all* existing java tools/frameworks
- Kotlin is supported by the best IDE on the market (with free version)
- It is easy to read, even non-Kotlin programmers can review your code
- **You don’t need to commit** your project to Kotlin: *you can start by writing your tests in it*
- JetBrains is not likely to abandon Kotlin because it drives their sales
- Kotlin has a vibrant community and even you can easily contribute to Kotlin and suggest new features using [KEEP](https://github.com/Kotlin/KEEP)

So does it live up to the hype? Only you can tell.
Here are a few pointers where you can start:

- [Kotlin Tutorials](https://kotlinlang.org/docs/tutorials/)
- [The Kotlin Reddit](https://www.reddit.com/r/Kotlin/)
- [Kotlin Koans](https://kotlinlang.org/docs/tutorials/koans.html)
- [Kotlin Blog](https://blog.jetbrains.com/kotlin/) <-- this will keep you up to date
- [Awesome Kotlin](https://kotlin.link/) <-- Curated list of Kotlin resources and libraries
- You can also check the source of the examples presented in this article [here](https://github.com/AppCraft-Projects/java-to-kotlin-examples).
