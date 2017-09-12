> Ha már Java fejlesztőként dolgozol egy ideje, akkor talán már elgondolkoztál azon, hogy mit tanulj meg legközelebb.
> Van jópár nyelv, amikre érdemes ránézni, például a  [Clojure](https://clojure.org/), a [Rust](https://www.rust-lang.org/en-US/) vagy a [Haskell](https://www.haskell.org/), de mi van akkor, ha olyan nyelvet szeretnél tanulni, amivel pénzt is lehet keresni és emellett kellemes vele kódot írni?
> A Kotlin pont egy ilyen nyelv és ebben a cikkben szeretném bemutatni, hogy miért.

## Szóval mi is a Kotlin?

- A [JetBrains](https://www.jetbrains.com/) saját fejlesztésű programozási nyelve, azé a cégé, akik az [IDEA](https://www.jetbrains.com/idea/) fejlesztői környezet mögött is állnak.
- Egy egyszerű, és sokoldalú Java alternatíva
- Ami kifogástalan együttműködésre képes a Java-val
- Java bájtkódra fordul
- A JVM-en fut
- Továbbá képes Javascript-re is fordulni

Ha elolvassuk a dokumentációt, akkor találhatunk még pár ígéretes dolgot:
- Többet elérhetünk kevesebb kód írásával
- Megold sok problémát a Java-val
- Továbbra is használhatjuk a Java ökoszisztémát vele
- Frontend és backend kódot is tudunk írni ugyanazon a nyelven
- 100%-ban együttműködő a Java-val
- Jobban teljesít e tekintetben, mint más alternatívák (Clojure, Scala)
- És csak egy vékony réteg komplexitást ad hozzá a Java-hoz

Jól hangzik, nem? Mielőtt rátenyerelünk a Letöltés gombra azért még tekintsük át, hogy mit is jelent ez a gyakorlatban.

## Érték- és adatosztályok
Amit itt láthatunk, az egy jó öreg POJO a szokásos boilerplate kóddal:

```java
/**
 * A plain old Java object with all the boilerplate.
 */
public class HexagonValueObject {

    private final int x;
    private final int y;
    private final int z;

    public HexagonValueObject(int x, int y, int z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    public int getZ() {
        return z;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        HexagonValueObject hexagon = (HexagonValueObject) o;
        return getX() == hexagon.getX() &&
                getY() == hexagon.getY() &&
                getZ() == hexagon.getZ();
    }

    @Override
    public int hashCode() {
        return Objects.hash(getX(), getY(), getZ());
    }

    @Override
    public String toString() {
        return "HexagonValueObject{" +
                "x=" + x +
                ", y=" + y +
                ", z=" + z +
                '}';
    }
}
```

Az érték osztályok készítése elég nehézkes még akkor is, ha Lombokot használunk (a Lombok használatához telepíteni kell egy beépülő modult a fejlesztői környezetünkbe, ami nem minden esetben megoldható). Habár ehhez segítséget nyújt az IDEA (illetve az Eclipse), különbféle kódgeneráló eszközökkel, ezek nem igazán megbízhatóak (gondoljunk csak arra, amikor hozzáadunk egy új mezőt az osztályhoz és elfelejtjük újragenerálni az `equals` metódust.

Nézzük meg hogy néz ki ugyanez Kotlinban:

```kotlin
data class HexagonDataClass(val x: Int, val y: Int, val z: Int)
```

Hoppá! Ehhez lényegesen kevesebbet kellett gépelni a Java verzióhoz képest! A Kotlin adat osztályok a következőket adják:
- `equals` + `hashCode`,
- `toString` és
- getterek + setterek
Ezeken kívül megkapjuk a `copy` metódust, amivel másolatokat tudunk készíteni már meglévő adat osztály példányokról. Erről [itt](https://kotlinlang.org/docs/reference/data-classes.html) lehet többet olvasni.




## String interpoláció
A `String`-ek manipulálása Java-ban nem éppen kényelmes művelet. Ezen segíthet a `String.format`, de nem túl sokat.

```java
public class JavaUser {
    private final String name;
    private final int age;

    public String toHumanReadableFormat() {
        return "JavaUser{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public String toHumanReadableFormatWithStringFormat() {
        return String.format("JavaUser{name='%s', age=%s}", name, age);
    }
}
```
A Kotlin ennek a problémának a megoldására a [String interpolációt](https://stackoverflow.com/questions/37442198/how-does-string-interpolation-work-in-kotlin) nyújtja, amivel kényelmesebben tudunk kezelni szövegeket:

```kotlin
class KotlinUser(val name: String, val age: Int) {

    fun toHumanReadableFormat() = "JavaUser{name='$name', age=$age}"

    fun toHumanReadableFormatWithMethodCall()
     = "JavaUser{name='${name.capitalize()}', age=$age}"
}
```

## Kiegészítő funkciók

Dekorátorokat írni Java-ban trükkös feladat és nem mindig sikerül tökéletesen.


Writing decorators in Java can be tricky and they are not perfect. If you want to write a decorator which
can be used with all classes implementing `List` you can't simply use it in your decorator because it would need you to
implement a lot of other methods so you have to extend `AbstractList`.

```java
public class ListPresenterDecorator<T> extends AbstractList<T> {

    private List<T> list;

    public ListPresenterDecorator(List<T> list) {
        this.list = list;
    }

    public String present() {
        return list.stream()
                .map(Object::toString)
                .collect(Collectors.joining(", "));
    }

    @Override
    public T get(int index) {
        return list.get(index);
    }

    @Override
    public int size() {
        return list.size();
    }
}
```

If you need to decorate something which does not provide useful base classes like `AbstractList`
or is a `final` class then you are out of luck. Extension methods come to the rescue!

```kotlin
fun <T> List<T>.present() = this.joinToString(", ")
```

This method acts as a decorator for all `List`s. Compared to the Java alternative this one-liner is much simpler and it will
also work for `final` classes. Just try not to [abuse](https://www.philosophicalhacker.com/post/how-to-abuse-kotlin-extension-functions/) them.

## Null safety
Checking for `null` values involves a lot of boolean expressions and a lot of boilerplate. With the advent of Java 8
you can finally work around this with the `Optional` class but what if the reference to an `Optional` is `null`?
Yes, you'll get a `NullPointerException` and after 20 years of Java we still don't know **what** was null.
Take the following example:

```java
public class JavaUser {

    static class Address {
        String city;
    }

    private final String firstName;
    private final String lastName;
    private final List<Address> addresses;

    /**
     * If you want to make sure nothing is `null` you have to check everything.
     */
    public static String getFirstCity(JavaUser user) {
        if(user != null && user.addresses != null && !user.addresses.isEmpty()) {
            for(Address address : user.addresses) {
                if(address.city != null) {
                    return address.city;
                }
            }
        }
        throw new IllegalArgumentException("This User has no cities!");
    }
}
```

With Kotlin you have several options. If you have to interop with Java projects you can use the null safety operator (`?`):

```kotlin
data class KotlinUserWithNulls(val firstName: String?,
                               // String? means that it is either a String object or a null
                               val lastName: String?,
                               val addresses: List<Address> = listOf()) {

    data class Address(val city: String?)

    companion object {
        fun fetchFirstCity(user: KotlinUserWithNulls?): String? {
            user?.addresses?.forEach { it.city?.let { return it } }
            return null
        }
    }
}
```

The code will only run after a `?` if its left operand is not `null`.
The `let` function creates a local binding for the object it was called upon
so here `it` will point to `it.city`.
If you don't have to interop with Java I would suggest doing away with `null`s completely:

```kotlin
data class KotlinUserWithoutNulls(val firstName: String,
                                  // this parameter can't be null
                                  val lastName: String,
                                  val addresses: List<Address> = listOf()) {

    data class Address(val city: String)

    companion object {
        fun fetchFirstCity(user: KotlinUserWithNulls)
         = user.addresses.first().city
    }
}
```

If there are no `null`s involved (no `?`s present) it all becomes a lot more simpler.

![can't explain that]({{ site.url }}/assets/articles/cant_explain_nulls.jpg)

## Type Inference
Kotlin supports type inference which means that it can derive types from the context in which they are present.
This is like the Java diamond notation `<>` but on steroids! Take the following example:

```java
public class JavaUser {

    // ...

    private final String firstName;
    private final String lastName;
    private final List<Address> addresses;

    public Address getFirstAddress() {
        Address firstAddress = addresses.get(0);
        return firstAddress;
    }
}
```

This looks almost the same in Kotlin:

```kotlin
data class KotlinUser(val firstName: String,
                      val lastName: String,
                      val addresses: List<Address> = listOf()) {

    data class Address(val city: String)

    /**
     * This is the same as in `JavaUser`.
     */
    fun getFirstAddressNoInference(): Address {
        val firstAddress: Address = addresses.first()
        return firstAddress
    }
}
```

Until you let Kotlin figure out the types of your variables:

```kotlin
/**
 * Here the type of `firstAddress` is inferred from the context.
 */
fun getFirstAddressInferred(): Address {
    val firstAddress = addresses.first()
    return firstAddress
}
```

or even methods:

```kotlin
/**
 * Here the return type is inferred. Note that
 * if a method consists of only one statement
 * you can omit the curly braces.
 */
fun getFirstAddress() = addresses.first()
```

## No checked exceptions
You must have seen this piece of code at least a million times:

```java
public class JavaLineLoader {

    public List<String> loadLines(String path) {
        List<String> lines  = new ArrayList<>();
        try(BufferedReader br = new BufferedReader(new FileReader(path))) {
            String line;
            while((line = br.readLine()) != null) {
                lines.add(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        return lines;
    }
}
```

Old school IO in Java. Note the try with resources block!
The same would look like this in Kotlin:

```kotlin
class KotlinLineLoader {

    fun loadLines(path: String) = File(path).readLines()
}
```

There are a couple of things going on here. First Kotlin does away with checked exceptions. Secondly Kotlin adds
`use` to any `Closeable` object which basically:

> Executes the given [block] function on this resource and then closes it down correctly whether an exception is thrown or not.
> (Taken from Kotlin's documentation)

What you can also see here is that an extension function (`readLines`) is added to the `File` class. This pattern is visible throughout Kotlin's rather small standard library. If you have ever used Guava, Apache Commons or something similar, chances are that you will see common functionality from them added to a JDK class as an extension function. Needless to say this will be good for your health (nerves at least).

## Lambda support
Let's look at the lambda support in Java:

```java
public class JavaFilterOperation {

    private List<String> items;

    @FunctionalInterface
    interface FilterOperation {
        Boolean filter(String element);
    }

    private List<String> filterBy(FilterOperation fn) {
        return items.stream()
                .filter(fn::filter) // applying the function
                .collect(Collectors.toList());
    }

    public void doFilter() {
        filterBy((element) -> {
            return element.length() > 0;
            // calling the function with an actual lambda
        });
    }
}
```

Since there is no syntax for method parameter types we have to create an interface for it.
Note that we could use `Function<String, Boolean>` here but it only works for functions with one parameter!
There are some interfaces in the JDK to solve this problem but if someone looks at the code they might be puzzled what
a `BiFunction` is useful for?
Kotlin improves on this a bit:

```kotlin
class KotlinFilterOperation {

    private val items = listOf<String>()

    fun filterBy(fn: (String) -> Boolean) = items.filter(fn)

    fun doFilter() {
        filterBy(String::isNotEmpty)
        // note the exension function `isNotEmpty` added to `String`!
    }
}
```

Kotlin adds a syntax for passing functions as parameters: `(ParamType1, ...ParamTypeN) -> ReturnType`.
And with Kotlin you have method and field references and you can also refer to a method from a concrete object!
Using the example above I can refer to the `filterBy` function on a concrete instance like this:

```kotlin
val reference = KotlinFilterOperation()::filterBy
```

## Functional programming
Functional programming is all the buzz nowadays and with Java 8 they have released Oracle's take on the topic: the Stream API.
It works like this:

```java
public class JavaUser {
 
    // ...
    
    public static Set<String> fetchCitiesOfUsers(List<JavaUser> users) {
        return users.stream()
                .flatMap(user -> user.addresses.stream())
                .map(JavaUser.Address::getCity)
                .collect(Collectors.toSet());
    }
}
```

The Kotlin equivalent is rather similar, but subtly different:

```kotlin
    fun fetchCitiesOfUsers(users: List<KotlinUser>) = users
            .flatMap(KotlinUser::addresses)
            .map(Address::city)
            .toSet()
```

There is no explicit conversion to streams since all Kotlin collections support it out of the box.
Not having to pass a lambda to `flatMap` here is a direct consequence of this.
Collecting the result is also automatic (no need for `Collectors.to*` method calls).
We only had to use `toSet` here because we want to return a `Set`. Otherwise `.toSet()` can be omitted.

## Kotlin-java interoperation
Well this can be a dealbreaker for most people but JetBrains got this right:

```java
public class KotlinInterop {

    public void helloJava() {
        System.out.println("Hello from Java!");
    }

    public void helloKotlin() {
        JavaInterop.createInstance().helloKotlin();
    }
}
```


```kotlin
class JavaInterop {

    fun helloJava() {
        KotlinInterop().helloJava()
    }

    fun helloKotlin() {
        println("Hello from Kotlin!")
    }

    companion object {

        @JvmStatic
        fun createInstance() = JavaInterop()
    }
}
```

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
