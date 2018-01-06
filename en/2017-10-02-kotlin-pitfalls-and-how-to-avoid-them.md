<div id="tldr">
Kotlin is all the rage lately and while I do agree that the language is well thought out it has - as everything else - its flaws.
In this article I'll explain some of the pitfalls I encountered and try to help you avoid them.
</div>

## The mystery `null`

In Kotlin you can write your code as if `null` never existed and this can make you forget that `null` is omnipresent
but it hides. Let's look at this simple and seemingly innocent class:

{% gist 2dc709969c3267b247e8536c8442d752 %}

If you try to instantiate this you'll get a `NullPointerException` because `bar` tried to access the `length` of `c` before it was initialized.
Of course the application logic was flawed here but you still got an `Exception`. The worst part of this is that the IDE won't complain about this.

The takeaway here is that Kotlin *will help* you in a lot of cases (nearly all) to avoid `null`, but you can't forget about it and from time to time you'll encounter things like this.

## Handling `null`s from the JDK

Kotlin's standard library handles `null`s fine but if you use classes from the JDK you will have to handle possible `null` pointers from
library functions by hand. Most of the time the Kotlin classes are enough, but sometimes you have to use something like the `ConcurrentHashMap`.

{% gist 5ec14315e8d9b4d8b6da153bdba6be7f %}

In this case you have to use the `!!` operator but in other cases the `null` safety operator (`?`) can also work.
Nevertheless if you use Java libraries extensively you'll have to litter your code with `!!`s and `?`s or write adapters for Java classes.
This is something you can't really avoid.

There is another more hideous problem you might bump into: when using methods on JDK classes which can return `null` and does not have syntactic sugar like `Map` access above. Consider the following example:

{% gist 2ca42017e2fb5656f650b28982c081a2 %}

In this case you use `peek` which in fact can return `null` but the Kotlin compiler won't complain so you can get a `NullPointerException` if your `Queue` was empty. The problem here is that we used `Queue` which is a JDK interface and if you look at the implementation of `peek`:

{% gist b3c6554ef525152756a24cc39053be91 %}

it says that `peek` will return `E` which will lead Kotlin to believe that `E` is not nullable. This might be worked around in a future version of Kotlin but right now it *is important to keep this in mind* in your projects and use these interfaces like this:

{% gist 9a9d2418c08af889b3c9ea5543401799 %}

## The inner `it`

When a lambda has a single parameter you can omit it from your code and can use `it` instead:

> it: implicit name of a single parameter
> One other helpful convention is that if a function literal has only one parameter, its declaration may be omitted (along with the ->), and its name will be it
> -- Kotlin docs

The problem with this is when you have nested functions like in this example:

{% gist a9d7927f3251546ac119feb9922f4dac %}

It only takes a few `it`s to lose track which is which. The solution to this problem is to name the parameters explicitly:

{% gist 8961dbfa691d908e494d0a9e05ee64a4 %}

Much better!

## The insidious `copy`

Take a look at this `data class`:

{% gist 314b2a5d21b6648ec9865d3a2f6dc15e %}

Data classes give you a bunch of functions and you can also make a `copy` of them. Guess what this will print out:

{% gist 9ef77890c45095f14cdeab78bb581ee5 %}

This will print `foobar, wombar, oops`. The problem is that while the name indicates that `copy` will make an *actual copy* in fact it will only
copy the references in your object. This can be insidious if you forget to write an unit test and you pass your `data class`es around as if they
were immutable [value object](https://martinfowler.com/bliki/ValueObject.html)s.

The solution to this problem is to pay attention to your `data class`es and if they should be value objects make them one:

{% gist 1c82249ba8e5db62320dfaba992c7280 %}

> There is an other problem with `data class`es: you can't tell Kotlin which fields you want to put in your `equals` / `hashCode`, you can only
> `override` both and write them by hand. Keep this in mind.

## Internal leakage

Take a look at this example:

{% gist f000b31080af7c359ba68aa6361e7187 %}

If you use classes like this from other Kotlin projects the `internal` keyword will be respected. If you look at this
from a Java project however `hiddenOperation` will be `public`! To avoid this I'd suggest using `interface`s to hide
implementation details:

{% gist 8bb83aec2237484fe447ac5b1195efac %}

## Non-generic global extensions

The utility of extension functions is unquestionably high, but with great power comes great responsibility.
You can - for example - write extension functions to JDK classes which will be visible for the whole project.
This can be problematic when they are non-generic and represent operations which only make sense in a local context:

{% gist 9c01f38fbe3948f21685a56978247b5c %}

Now everybody on your project will scratch their heads when they bump into this.
So I think it is good if you think twice before you write extension functions but they can be really powerful. Here are some examples which might be useful:

{% gist 0d0a902d2ebc030282679689302b82b6 %}

## Unit returning lambdas vs Java SAM conversion

When you have functions which accept lambdas you can omit the `return` keyword if the lambda's return type is `Unit`:

{% gist 34d90d33f03b691c5943e730577fe7dc %}

This is fine but if you call this from Java you'll face the awkward problem of needing to return `Unit`:

{% gist 33a37c7936fbdb72438d079c2d97a2bb %}

This is very clunky from the Java side. If you try to make this work from Java you can define an `interface`:

{% gist f6d21abc257a565cc75830f03f54cf43 %}

Then you can use Java's SAM conversion to make this very simple:

{% gist cde3b9b41f487ab3c4f53347455c9677 %}

But then from the Kotlin side it becomes a mess:

{% gist af23dfe2fb2bea872f2d6a52a4bf99bd %}

The problem is that Kotlin does not support SAM conversion for Kotlin classes and it only works with Java classes. My suggestion is that
for simple cases just use Java's built in SAM interfaces like `Consumer`:

{% gist 6457779ac7347140257c6b647c831a97 %}

## Java interop with unmodifiable Collections

Kotlin gives you immutable variants of the JDK's collection classes:

{% gist 4924f4f84b596477bf0319a570bcaec7 %}

This is a very nice addition but if you look at this from the Java side you'll see the JDK's `Set` api:

{% gist 5769498609f71411355e0caffe92a363 %}

If you try to modify this `Set` the same will happen as if you have used Java's `Collections.unmodifiableSet()` method.
I don't know whether this can be (or should be) worked around but this is something you can keep in mind when working with Kotlin's immutable versions of Java collections.

## No overloads in interfaces

This is only an issue from an interop perspective, but Kotlin does not support the `@JvmOverloads` annotation in an `interface` and you can't use it on `override`s either:

{% gist 2c7fbbfb6adbbad0d0f74b5f3c561e7d %}

Currently the only thing you can do to have overloads is to define them by hand:

{% gist b4657c2275d8d4c44a328b212e7227de %}

Keep it in mind though that you can suggest improvements to Kotlin itself using [KEEP](https://github.com/Kotlin/KEEP) (Kotlin Evolution and Enhancement Process). KEEP is something like JEP in Java, but of course KEEP has much less red tape compared to JEP.

## Conclusion

Kotlin is a very popular language right now and I **do agree** that it is a *Turbo* Java, but you should take any hype with a grain of salt. As we have seen above Kotlin has its own pitfalls which you should be aware of if you plan to use it.

All in all I think that the problems mentioned above can either be worked around easily or not critical and they don't limit the usability of the language itself.