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

Dekorátorokat írni Java-ban trükkös feladat és nem mindig sikerül tökéletesen. Például, ha egy olyan dekorátort szeretnénk készíteni, ami a `List` interfész összes implementációjával képes működni, akkor nem szerencsés a `List`-et kiterjesztenünk, mert úgy elég sok metódust kellene implementálnunk, ehelyett érdemesebb az `AbstractList`-ből kiindulnunk:

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

Ha viszont valami olyasmit akarunk dekorálni, ami nem ad az `AbstractList`-hez hasonló könnyítéseket, vagy akár az általunk dekorálandó osztály `final`, akkor gondban vagyunk. Ebben a problémában tudnak segítséget nyújtani a kiegészítő funkciók:

```kotlin
fun <T> List<T>.present() = this.joinToString(", ")
```

Ez a funkció dekorátorként viselkedik minden `List` példányon. A Java-s alternatívával összehasonlítva ez az egy soros lényegesen egyszerűbb és képes `final` osztályokon is működni. Természetesen [ez sem mindenre megoldás](https://www.philosophicalhacker.com/post/how-to-abuse-kotlin-extension-functions/).

## Null biztonság

A `null`-ok ellenőrzése általában sok logikai művelettel és boilerplate-tel jár. A Java 8 megjelenése óta ezen már tudunk segíteni az `Optional` használatával, de mi történik akkor, ha egy `Optional` referencia a `null`? Bizony, ilyenkor megkapjuk a szokásos `NullPointerException`-t, ami a Java 20 éves fennállása óta még mindig nem képes megmondani, hogy **mi** volt a `null`.
Nézzük meg a következő példát:

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

A Kotlin használatával több opciónk is van. Ha együtt akarunk működni Java projektekkel, vagy egy már meglévő Java projekten használunk Kotlint is, akkor lehet a `null` biztonsági operátort (`?`) használni:

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

A `?` jobb oldalán lévő kód csak akkor fog futni, ha a bal oldalán lévő kifejezés nem `null`. A `let` funkció létrehoz egy lokális scope-ot azzal az objektummal, amin meg lett hívva, így itt az `it` változó az `it.city`-re fog mutatni a visszatéréskor. 

Ha viszont nem kell Java kóddal együttműködni, akkor jobban járunk, ha egyáltalán nem használunk `null`-t a projektünkön:

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

Ha nincs a kódbázisunkban `null` (nics `?` sehol), akkor lényegesen egyszerűbb lesz az egész.

![can't explain that]({{ site.url }}/assets/articles/cant_explain_nulls.jpg)

## Típus kikövetkeztetés

A Kotlin támogatja a típusok kikövetkeztetését, ami azt jelenti, hogy sok esetben a kontextusból ki tudja deríteni a fordító a típusok megjelölése nélkül is azt, hogy egy-egy referencia milyen típussal rendelkezik. Ez kicsit olyan, mint a gyémánt jelölés Java-ban, csak sokkal sokoldalúbb! Nézzük meg a következő példát:

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

Ez Kotlinban alapból hasonlóan néz ki:

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

Amíg rá nem hagyjuk a Kotlin fordítóra, hogy kikövetkeztesse a változóink típusait:

```kotlin
/**
 * Here the type of `firstAddress` is inferred from the context.
 */
fun getFirstAddressInferred(): Address {
    val firstAddress = addresses.first()
    return firstAddress
}
```

vagy akár a funkciók visszatérési értékeit:

```kotlin
/**
 * Here the return type is inferred. Note that
 * if a method consists of only one statement
 * you can omit the curly braces.
 */
fun getFirstAddress() = addresses.first()
```

## Nincsenek ellenőrzött kivételek
Az alábbi kódot valószínűleg már milliószor láthattad:

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
Régimódi Java IO. Ugyanez Kotlinban így néz ki:

```kotlin
class KotlinLineLoader {

    fun loadLines(path: String) = File(path).readLines()
}
```

Itt több dolog is történik egyszerre. Először is a Kotlin-ban nincsenek ellenőrzött kivételek, így itt az IO végrehajtása során nem kell elkapnunk az esetlegesen keletkező kivételeket. Másodszor a Kotlinban az összes `Closeable` osztáyhoz hozzáadja az `use` műveletet, ami a dokumentáció szerint az alábbiakat végzi el:

> Executes the given [block] function on this resource and then closes it down correctly whether an exception is thrown or not.
> (Kivonat a Kotlin dokumentációból)

Ezeken kívül a `File` osztályhoz hozzáadott `readLines` funkciót is láthatjuk működés közben. Ez a minta a Kotlin által adott függvénykönytárban sokszor előforul. Ha használtál már Guava-t, vagy esetleg Apache Commons-t, akkor az azokban található függvények sokszor visszaköszönnek, mint Kotlin kiegészítő funkciók hozzáadva a JDK osztályokhoz. Ezeket használva sok kellemetlenségtől megkímélhetjük magunkat.

## Lambda támogatás
Nézzünk meg egy példát egy Java-s lambdára:

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

Mivel a Java-ban nincsen külön szintaxis, amivel metódusok szignatúráját írhatnánk le, általában egy interfészt kell létrehozni nekik. Az alábbi példában ugyan használhatnánk a `Function<String, Boolean>` típust, de ez csak olyan függvényekre működik, amiknek egy paraméterük van. Erre vannak részmegoldások a JDK-ban, de ha valaki ránéz a kódunkra, lehet, hogy nem lesz egyértelmű számára, hogy mire is való egy `BiFunction`. Ezen egy kicsit javít a Kotlin:

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

A Kotlin szintaxist ad funkciók leírására az alábbi módon: `(ParamType1, ...ParamTypeN) -> ReturnType`.
Mivel a Kotlin-ban vannak függvény- és mező referenciák is, ezért lehet hivatkozni egy már létező objektum egy függvényére is!
Az alábbi példában egy konkrét példány `filterBy` műveletére hivatkozunk így:

```kotlin
val reference = KotlinFilterOperation()::filterBy
```

## Funkcionális programozás
Mostanában sokat hallani a funkcionális programozásról és a Java 8 megjelenésével már használhatjuk az Oracle által megálmodott eszköztárat erre a célra: A Stream API-t, ami így működik:

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

Ennek a Kotlin megfelelője nagyon hasonlít, de némileg különböző:

```kotlin
    fun fetchCitiesOfUsers(users: List<KotlinUser>) = users
            .flatMap(KotlinUser::addresses)
            .map(Address::city)
            .toSet()
```

A lényeges különbség az, hogy nincs exlicit konverzió stream-ekre, mivel az összes Kotlin collection támogatja őket alapból.
Ennek egyenes következménye az is, hogy a fenti példában nem kell lambda-t átadnunk a `flatMap` függvénynek.
Az eredmények összegyűjtése is automatikus (nincs szükség a `Collectors.to*` metódusok használatára).
Ebben a példában csak azért kellett a `toSet` függvényt használnunk, mert `Set`-et akarunk visszaadni. Ha ez nem lenne elvárás, akkor elhagyhatnánk a `.toSet()` hívást.

## Együttműködés Kotlin és Java között
Ha az együttműködés a két nyelv között nem működik kielégítően az sokakat elrettenthet (és el is rettent más nyelvekben), de a JetBrains itt kiköszörülte a csorbát:

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

Az együttműködés akadálymentes és fájdalommentes a két kódbázis között. A Java és Kotlin osztályok projekten belül is vegyíthetők és a Kotlin ad pár annotációt (például a `@JvmStatic` a fenti példában), amivel a Kotlin osztályokat lehet felokosítani úgy, hogy Java oldalról könnyű legyen a használatuk. Erről [itt](https://kotlinlang.org/docs/reference/java-interop.html) lehet többet olvasni.

Ha megnézzük a fenti példákat, akkor jól láthatóvá válik egy séma: a Kotlin megőrzi a Java jó ötleteit, azokat feljavítja, míg a Java hibáit próbálja kiküszöbölni. Az, hogy a Google az Android egyik hivatalos nyelvévé tette a Kotlint szintén ezt támasztja alá.

## Mit mondjak a főnökömnek?
Ha esetleg meggyőztek a fentiek és kipróbálnád a Kotlint a munkahelyeden, de nincs ötleted, hogy mit mondj a főnöködnek és a csapattársaidnak, akkor adok pár tippet, amik talán segítenek majd:

- A Kotlin az iparból érkezett, nem egy egyetem falai közül. Valós problémákat old meg, amikkel programozók nap mint nap találkoznak
- Ingyenes, és nyílt forráskódú
- Van hozzá egy könnyen használható Java -> Kotlin konvertáló eszköz
- Gyakorlatilag __0__ energiabefektetéssel keverhető a Java és a Kotlin kód
- *Minden* már létező Java eszközt és keretrendszert lehet használni Kotlin-ból is
- A Kotlin-hoz a piac legjobb integrált fejlesztői környezete ad támogatást (aminek van ingyenes verziója)
- Könnyen olvasható, még a nem-Kotlin fejlesztők is át tudják nézni a kódodat
- **Nem kell elköteleződni** a Kotlin mellett a projekteden: *elég az is, ha csak a teszteket írod az elején Kotlinban*
- A JetBrains nem valószínű, hogy abbahagyja a nyelv fejlesztését egyhamar, mivel bevallottan is az a céljuk, hogy a bevételeiket növeljék vele
- A Kotlin közösség meglehetősen aktív és a [KEEP](https://github.com/Kotlin/KEEP)-en akár te is tehetsz javaslatokat a nyelv irányát illetően

A kérdést, hogy több-e a Kotlin, mint egy egyszeri hype, viszont csak Te tudod eldönteni.
Ha ki akarod próbálni a nyelvet, akkor itt van pár tipp, hogy hol kezdd:

- [Kotlin Tutorial-ok](https://kotlinlang.org/docs/tutorials/)
- [A Kotlin Reddit](https://www.reddit.com/r/Kotlin/)
- [Kotlin Koan-ok](https://kotlinlang.org/docs/tutorials/koans.html)
- [Kotlin Blog](https://blog.jetbrains.com/kotlin/) <-- napi hírek
- [Awesome Kotlin](https://kotlin.link/) <-- Válogatott függvénykönyvtárak és források
- Ezen kívül [itt](https://github.com/AppCraft-Projects/java-to-kotlin-examples) megtekintheted a cikkben látható példákat
