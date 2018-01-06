<div id="series">
This article is part of a <a href="http://the-cogitator.com/2017/12/21/beyond-android-exploring-kotlin-areas-of-application.html">series</a>.
</div>
<div id="tldr">
While most developers use Kotlin on Android it is also a viable option on other platforms. In this article we'll look at how it works on the backend.
</div>

As [I have written about this before](http://the-cogitator.com/2017/05/19/kotlin-is-the-new-java.html) I think that the interop between Java and Kotlin is quite seamless. This also means that using Kotlin in place of Java on the backend is rather easy. Apart from a few nuisances, you can pretty much start writing your new features in Kotlin within your Java project or if you just want to try it out you can start by writing your tests with it.


If you look around it seems that companies with a big slice of the backend pie also have the same thought: the new version of Spring has some features [dedicated to Kotlin](https://tech.io/playgrounds/8594/spring-5---dedicated-kotlin-features) and you can even use Kotlin to write your Gradle scripts using the [kotlin-dsl](https://github.com/gradle/kotlin-dsl).


What is interesting to note here is that **you don't need Kotlin support for any of these libraries**, because the Java interop features of Kotlin are so good.

## Decisions

When you start to work with Kotlin on the backend you have several options at your disposal.

If you choose to use a library written in Kotlin you get some major advantages: there will be no compatibility issues related to the language you use since everything is written in Kotlin. Another thing worth mentioning is that there are some things which are only present in Kotlin such as Coroutines and reified generics. The trade-off is that most of these libraries are not very old and they might have the typical problems with new projects: lack of documentation and eventual bugs or design issues.

If you need something which is battle-tested you can't go wrong with tools like [Spring](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/) or [Mockito](http://site.mockito.org/). What you get with these is that most issues are well-documented and you'll get answers for your questions quite fast. What you lose though is some of the nice things of Kotlin: they don't have [Kotlin DSL](https://kotlinlang.org/docs/reference/type-safe-builders.html)s and the code will be quite Java-ish.


Another option is to pick a library which has built-in support for Kotlin. [RxKotlin](https://github.com/ReactiveX/RxKotlin) and [vert.x](https://github.com/vert-x3/vertx-lang-kotlin) are good examples of this. I'll call these projects as *hybrids* from now on. With them you get the best of both worlds: you will usually have a good API which is idiomatic from a Kotlin perspective and behind it there will be something which is well-known and battle-hardened.

Let's look at some libraries which you might want to try out.

> Note that the following are just my opinions and as such they are subjective.

### Pure Kotlin tools

#### Ktor
**[Ktor](http://ktor.io/)** is one of the newer web frameworks written in Kotlin. It comes with embedded [Netty](http://netty.io/wiki/user-guide-for-5.x.html) and a nice DSL to boot. What is interesting in this one is that it takes advantage of Kotlin's [Coroutine](https://kotlinlang.org/docs/reference/coroutines.html) support. This is how it looks like in practice:

{% gist 5162b5a65035fc0659b55c0d42d31484 %}

What my concerns were when I tried it out is that the documentation is [quite lacking](http://ktor.io/servers/structure.html) and when you bump into a problem you are more likely to get stuck. It also [does not perform well](https://www.techempower.com/benchmarks/#section=data-r14&hw=ph&test=plaintext) and interop with Java is a bit sketchy.

#### Javalin
[Javalin](https://javalin.io/) is an other web framework which has a very simple, fluent and readable API. It can also work with multiple embedded web servers like Netty or Undertow.
What I liked most is that it strikes a balance between the minimalistic approach of [Spark](http://sparkjava.com/) (not to be confused with [Apache Spark](https://spark.apache.org/)) and the low-level nature of [vert.x](http://vertx.io/).
The [documentation](https://javalin.io/documentation) is also very good so you can get started in no time:

{% gist 869e073c96cf9d72092c7f8736411645 %}

What I like here is that the API is a little more Java-ish than Ktor's so if you come from a Java background it might be easier to get started with.

#### Hexagon
[Hexagon](http://hexagonkt.com/) is an interesting choice for writing web applications. The name choice is not random: it encourages using the [hexagonal architecture](http://alistair.cockburn.us/Hexagonal+architecture)  (more commonly known as [clean architecture](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)) and it is also [more performant](https://www.techempower.com/benchmarks/#section=data-r14&hw=ph&test=plaintext) than ktor or even the Spring framework! The DSL which you get is also pretty descriptive:

{% gist f6a55afc38fd48bdf2f116c61a810198 %}

#### Off the web
It is worth noting that there are a [lot of tools](https://kotlin.link/) written in Kotlin which you can pick from.
Kotlin has a very useful [specification framework](https://github.com/spekframework/spek) and if you don't like the fact that there are no good HTTP clients for Java you can now take advantage of [Fuel](https://github.com/kittinunf/Fuel) which is a very handy tool for interfacing with REST endpoints and beyond.
You even have tools for [writing games](https://github.com/Hexworks/zircon) in Kotlin.


### Java frameworks

#### vert.x
[vert.x](http://vertx.io/) is a multi-module web framework akin to [Spring](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/). What is important to note here is that there is a [documentation section](http://vertx.io/docs/vertx-core/kotlin/) dedicated to Kotlin. vert.x might be your choice for writing web applications if performance is paramount to you: vert.x has [very good benchmark scores](https://www.techempower.com/benchmarks/#section=data-r14&hw=ph&test=plaintext). This should not be a surprise since it implements the [multi reactor pattern](http://vertx.io/docs/vertx-core/java/#_reactor_and_multi_reactor) (the reactor pattern might be familiar to you from [node.js](https://www.packtpub.com/mapt/book/web_development/9781783287314/1/ch01lvl1sec09/the-reactor-pattern)). It is worth noting that there are very goood [Kotlin examples](https://github.com/vert-x3/vertx-examples/tree/master/kotlin-examples) for vert.x. You'll need them because it has some concepts which are not present elsewhere and setting it up is also a bit more involved:

{% gist be23bd8864a8e79655f659fdc5f46a85 %}

#### Spring
For a lot of Java developers [Spring](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/) is the de facto tool for writing Java applications. The good news is that since *5.0* Spring has [built-in support](https://spring.io/blog/2017/01/04/introducing-kotlin-support-in-spring-framework-5-0) for Kotlin. There is a whole [plethora of Spring projects](https://spring.io/projects) which you can pick from. If you are interested there is a simple [tutorial](https://kotlinlang.org/docs/tutorials/spring-boot-restful.html) which will get you started with using Spring with Kotlin. Here is how Hello World looks like with it:

{% gist 23aa700db7acf7247d5dd19b3c21dc93 %}

#### Sparkjava

[Sparkjava](http://sparkjava.com/) is a minimalistic (micro)web framework. You can get started with it in practically zero time investment and if you come from node.js it is also a very good choice:

> Lately, a lot of server-side web development has been taken over by NodeJS, but a growing number of NodeJS developers are using TypeScript and other statically typed languages that compile to JavaScript. Why not go all the way and use a language that was actually designed with types, and intended to run on the server-side? You also get all the benefits of running your application on the JVM, where libraries aren’t deprecated every day. If you’re coming from ExpressJS, then Spark’s syntax will feel very familiar, and unlike a lot of JavaScript web frameworks, Spark won’t be deprecated tomorrow.
> 
> -- from the Sparkjava website

You also can't get more minimal than this:

{% gist ba295d07cd9970e605f1ab6792ddc4a3 %}

### Hybrid options

While interfacing with Java tools is usually pretty convenient some of the tools above have tooling support written for Kotlin:

**[Spark-kotlin](https://github.com/perwendel/spark-kotlin)** provides a more streamlined experience for Kotlin users and it is also worth noting that it is written by [perwendel](https://github.com/perwendel), the original author of Sparkjava!

**[vertx-lang-kotlin](https://github.com/vert-x3/vertx-lang-kotlin)** provide useful Kotlin-specific options for vert.x like coroutines, one-shot workers or reactive streams.


While using Mockito from Kotlin is mostly pleasant **[mockito-kotlin](https://github.com/nhaarman/mockito-kotlin)** improves upon that by giving you a nice DSL and taking care of some issues.

**[Hamkrest](https://github.com/npryce/hamkrest)** is a re-implementation of [Hamcrest](http://hamcrest.org/) with syntactic sugars, extensibility and more.

**[RxKotlin](https://github.com/ReactiveX/RxKotlin)** adds convenient [extension functions](https://kotlinlang.org/docs/reference/extensions.html), SAM helpers and more to [RxJava](https://github.com/ReactiveX/RxJava). The only problem is that the documentation is behind a [paywall](https://www.packtpub.com/application-development/learning-rxjava).

## A working example

Now let's look at a step-by-step example! We'll use Spring Boot with [Spring Initializr](https://start.spring.io/).

> The source code of this tutorial can be found [here](https://github.com/AppCraft-Projects/spring-boot-kotlin-demo).

### Getting started

Spring Boot comes with [Spring Initializr](https://start.spring.io/) which is a handy tool with which you can quickly kick off your project. I recommend selecting *Gradle Project* with *Kotlin* and Spring Boot *2.0.0*  for this example since with **2.0.0** you'll get the features of *Spring 5.0*.
 
 > If you click *"Switch to full version"* you'll also be able to piece together a fine-grained skeleton. There is a cornucopia of topics which you can pick tools from ranging from Web to AWS and more.
 > For this exercise I picked *Web* only.
 
 After setting up Initializr click "Generate Project" and open it in your IDE. You might notice that compared to a simple Java project you don't have to add much to your `build.gradle` which is Kotlin-specific. I've extracted them in this example:
 
{% gist 84ec0ce55be1fab38a69a2af92f8b69d %}

### Adding a simple Controller
 
 If you look at the entry point of the application it is rather minimalistic:
 
{% gist 506fb9faa3980233865bb64d64874b38 %}

Now the only thing we need for a working Hello World is to add a `RestController` to our project:

{% gist 51ec7fd732707e74fcf40f4d2c7ae73f %}

and **Bam!** you are done! You can go and check out the result at `http://localhost:8080/`:

{% gist 0d679a659201e96bdfbac0c6ebec4ef9 %}

You can learn more about how this works [in this tutorial](https://spring.io/guides/gs/spring-boot/) or if you want to see how the Kotlin APIs and the functional way looks like there is a nice tutorial [here](https://spring.io/blog/2017/08/01/spring-framework-5-kotlin-apis-the-functional-way).

## Wrapping up

In this article we have explored some of the more known options for backend development with Kotlin. We have also seen that prominent actors on the market have *embraced Kotlin* and backend development can be a lot simpler with it compared to pure Java.

In the next article we'll explore how Kotlin can be used in your project instead of *Javascript*!