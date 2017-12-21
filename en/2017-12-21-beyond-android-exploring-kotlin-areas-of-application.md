<div id="tldr">
If you have written something in Kotlin chances are that you wrote it for Android. Kotlin, however, has other areas where it shines.
In the following series, we'll explore what other areas exist where Kotlin can be useful and discuss how you can take advantage of them.
</div>

## Branching out
While Kotlin started out as a language for the JVM, its creators have started to give us options to use the language on other platforms as well. The [Kotlin Frontend Plugin](https://github.com/Kotlin/kotlin-frontend-plugin) is nothing new, but now we can [go native](https://github.com/JetBrains/kotlin-native/) or create [multiplatform projects](https://kotlinlang.org/docs/reference/multiplatform.html).

What is also interesting is that on the JVM you can also use Kotlin on the backend with great effect. Using Spring with Kotlin is becoming easier with the advent of Spring 5 which has [built-in support](https://spring.io/blog/2017/01/04/introducing-kotlin-support-in-spring-framework-5-0) for Kotlin, but you can choose from a variety of technologies, like [vert.x](http://vertx.io/docs/vertx-core/kotlin/), [RxKotlin](https://github.com/ReactiveX/RxKotlin) or even some native tools like [Hexagon](https://github.com/hexagonkt/hexagon).

## Kotlin on the backend
As [I have written about this before](http://the-cogitator.com/2017/05/19/kotlin-is-the-new-java.html) I think that the interop between Java and Kotlin is quite seamless. This also means that using Kotlin in place of Java on the backend is rather easy. Apart from a few nuisances, you can pretty much start writing your new features in Kotlin within your Java project or if you just want to try it out you can start by writing your tests with it.

If you look around it seems that companies with a big slice of the backend pie also has the same thought: the new version of Spring has some features [dedicated to Kotlin](https://tech.io/playgrounds/8594/spring-5---dedicated-kotlin-features) and you can even use Kotlin to write your Gradle scripts using the [kotlin-dsl](https://github.com/gradle/kotlin-dsl).

What is interesting to note here is that **you don't need Kotlin support for any of these libraries**, because the Java interop features of Kotlin are so good. In the next article in this series, we'll explore the ways you can write backend code with and without libraries written in Kotlin and we'll also look into some ways you can tinker with your existing programs written in Java.

## Compiling Kotlin to Javascript
When trying to compile Kotlin to Javascript you have two options: the *kotlin2js* plugin and the *kotlin-frontend-plugin*.

The former is a simple way to turn your code to JS without the hassle of managing external dependencies. It just works out of the box and results in a `.js` file which you can copy to your static assets folder.

The [latter](https://github.com/Kotlin/kotlin-frontend-plugin) is a little more involved but it lets you use both Maven and npm dependencies.

With the use of these, you can easily go full stack but they do not let you share code between your backend and your frontend project. Check out [this](https://github.com/AppCraft-Projects/todomvc-kotlin) TodoMVC implementation which I have written if you are interested in how this works.

## Going native

Have you ever tried running Kotlin in an embedded environment or compile it into a single binary? Enter Kotlin Native which lets you do just that:

> Kotlin/Native is an LLVM backend for the Kotlin compiler, runtime implementation, and native code generation facility using the LLVM toolchain.

While this is still in a pre-release version (`0.4` at the time of writing) Kotlin Native is a promising development which tries to fill in the holes which are currently present and let you use Kotlin in some areas where it was not feasible to do so, like:

- iOS applications (reusing code with Android)
- Embedded systems/IoT (e.g., Arduino and beyond)
- Data analysis and Scientific Computing
- Server-side and Microservices (low-footprint executables, utilizing the power of coroutines)
- Game Development

## Multiplatform projects

While it is all well and good that you can now use Kotlin on a multitude of platforms what good does it do if you can't wire your different codebases together? With [the release of Kotlin 1.2](https://blog.jetbrains.com/kotlin/2017/11/kotlin-1-2-released/) now you can share code between platforms reliably.

This works by dividing your codebase into *common* and *platform* modules and by using the *expect + actual* API which lets you define classes and functions which will be implemented on each platform. You can take a look at [this video](https://www.youtube.com/watch?v=afc5PUs_EPE) by Dmitry Jemerov who is the Kotlin IDE Team Lead at JetBrains to get a better understanding of this topic.

## Conclusion

We've explored some areas where Kotlin can shine and the way you can glue together your multiplatform projects. In the following articles we'll look into each of these options in a little more detail.
