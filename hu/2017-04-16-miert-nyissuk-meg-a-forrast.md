> Ha dolgoztál már valahol nagyobb projekten, akkor jó eséllyel eszedbe jutott már, hogy a kódot amit írsz lehet megérné nyílt forráskódúvá tenni. Ebben a cikkben összefoglalom az előnyeit és a hátrányait, valamint adok pár ötletet, hogy miért lehet hasznos, ha még sosem foglalkoztál nyílt forráskódú szoftverrel.

Nyílt forráskódú (továbbiakban [OSS](https://en.wikipedia.org/wiki/Open-source_software)) szoftveren dolgozni kifejezetten szórakoztató. Te választhatod meg, hogy min dolgozz, mikor tedd azt és hogyan. Ha valami másoknak is hasznosat írsz, akkor előbb-utóbb az emberek elkezdik használni azt és itt kezdődnek az igazán érdekes dolgok.

## Pull request-ek (PR)
Legtöbbször a felhasználóidnak nem lesz idejük, vagy kedvük hozzájárulni a projektedhez, de van pár programozó, aki szeret javításokat küldeni, amiket átböngészve rájöhetsz, hogy mások hogyan használják a programodat. Ha magának a PR-nek a minősége jó (tehát javítás, vagy új feature fejlesztés, ami tesztelve van) akkor jópár dolgot tudsz tanulni belőle:
- Látni fogod __pontosan__, hogy mások hogyan használják a programodat
- Azt is látni fogod, hogy __hogyan szeretnék__ használni
- Illetve betekintést nyerhetsz a fejlesztő perspektívájába is, ami segíthet új szemszögből látni a dolgaidat

## Hiba jelentések
Azok a felhasználók, akik nem akarnak (vagy tudnak) hozzájárulni kóddal a projektedhez, de szeretnének rávilágítani egy hibára, hiba jelentést fognak küldeni neked. A legtöbb platform, ahol OSS kódot lehet host-olni rendelkezik olyan funkcióval, ahol ezt meg lehet tenni (például [GitHub](https://github.com/). Habár ezek nem annyira hasznosak, mint egy PR, tekinthetsz rájuk úgy, mint ingyen funkcionális tesztelésre. Úgy, mint a PR-ek, a bug reportok is hasznosak abból a szempontból, hogy rávilágítanak arra, hogyan használják a szoftveredet és így rájöhetsz, hogy mikre nem gondoltál annak tervezése során.

## Feature request-ek
Néha-néha jönni fog pár ötlet a külvilágból, feature request-ek formájában. Ezek elég hasznosak, mert általában *valódi üzleti igényeket* oldalnak meg, illetve ha esetleg tanácstalan lennél, hogy merre haladj tovább, akkor segíthetnek kijelölni a projekted irányát.

## Hozzájárulók és társak
Előfordulhat, hogy valakit annyira érdekel a projekted, hogy többször is hozzájárul, vagy akár teljes értékű csapattaggá is válhat, ha rájöttök, hogy jól tudtok együtt dolgozni. Mondanom sem kell, hogy ez talán a legjobb dolog, ami történhet veled és annak is egy nyilvánvaló jele, hogy jó irányba haladsz a projekteddel.

## Hátulütők
Annak ellenére, hogy az OSS fejlesztés sok előnnyel jár, vannak hátulütői is, amikbe előbb-utóbb bele fogsz futni. Ezeknek a többségét ki lehet küszöbölni, de jó, ha előre felkészülsz rájuk, ha nem akarod, hogy a projekted túlságosan időrablóvá váljon.

## Alacsony színvonalú hozzájárulások

Csak idő kérdése, hogy mikor fogod megkapni az első PR-edet a pokolból. Az is lehet, hogy törni fogja a build-et, vagy csak simán nincsenek hozzá tesztek. Láttam olyan PR-t is, amiben a fél projektet refaktorálták, csak hogy egy generikus típus paramétert betehessenek, aminek nem volt valódi előnye sem. Ezeknek az elkerülésére az alábbi módokon tudsz felkészülni:

- Csapj hozzá egy COC ([Code of Conduct](https://en.wikipedia.org/wiki/Code_of_conduct))-ot a projektedhez. Egy jó példa [itt](http://contributor-covenant.org/version/1/1/0/) található. Ez azért fontos, mert nem akarsz majd hálátlannak tűnni, amikor valaki az idejét a projektedre áldozza, de ha van egy doksi, ahova összeírod, hogy hogyan szeretnéd, hogy mások hozzájáruljanak, akkor ez elkerülhető.
- Tegyél fel egy CLA ([Contributor License Agreement](https://en.wikipedia.org/wiki/Contributor_License_Agreement))-t. Ez segít abban, hogy másoknak is egyértelmű legyen, hogy kié a projekt szellemi tulajdona ([Intellectual Property](https://en.wikipedia.org/wiki/Intellectual_property)). Ha ezt nem teszed meg, akkor belefuthatsz kötözködő emberekbe, akikkel vitázhattok ítéletnapig. Egy jó példa található [itt](https://github.com/ReactiveX/RxJava/blob/2.x/CONTRIBUTING.md).
- Használj [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) rendszert. Ha jól csinálod, akkor a meló java részét delegálhatod egy programnak, és a hozzájárulások automatikusan le lesznek fordítva, sőt egyes platformok (pl. GitHub) nagyon könnyen integrálhatók build eszközökkel, mint a [Travis](https://travis-ci.org/), vagy kód elemző szoftverekkel, mint a [CodeCov](https://codecov.io/), ami kommentelni is tud PR-ekre. Egy példát [itt](https://github.com/Hexworks/hexameter/pull/24) láthatsz.

### Túl sok hűhó a projekted körül
Lehet, hogy kicsiben kezded, de ha elég időt beleraksz, akkor bármelyik projekt a következő Halál Csillaggá válhat. Az sem segít, ha népszerű lesz a programod és elárasztanak a PR-ek és a hiba jelentések. Habár ezek alapvetően jók a projekt szempontjából, előfordulhat, hogy azon kapod magad, hogy lett egy második főállásod.

Erre a megoldás az lehet, ha bevonsz másokat is társként a projektbe, illetve ha nekilátsz SCRUM-ot, vagy Kanban-t használni a projekt menedzseléséhez.

### Licenszelési problémák

Előfordulhat, hogy egy túl megengedő licensz mellett döntöttél a projektedhez (mint pl. a MIT), vagy éppen fordítva, rájöttél, hogy a feltételek túl szigorúak (pl. AGPL). Ahhoz, hogy ezt elkerüld érdemes [itt](https://choosealicense.com/) körülnézni.

## Mit mondhatsz a főnöködnek?

Ha esetleg úgy gondolnád, hogy van valamid, aminek érdemes megnyitni a forrását, vagy éppen a munkahelyeden vannak olyan projektek, amik erre alkalmasak, akkor egy elég nehéz feladat áll előtted: *meggyőzni a főnöködet*. Összegyűjtöttem pár tippet, ami segíthet ebben:

> - A forrás megnyitása *szinte semmilyen költséggel nem jár*
> - Nem kell minden forrást megnyitni, elég csak azokat a részeket, melyek mások számára is hasznosak lehetnek. Ez abban is segít, hogy elgondolkodjatok azon, hogy érdemes modularizálni a kódot
> - *Ingyen* hibajelentéseket, PR-eket, vagy hibajavításokat fogtok kapni
> - A fejlesztők szeretnek olyan helyen dolgozni, ahol tudják, hogy a kód, amit írnak nyílt és így a hozzájárulásuk jobban látható a külvilág számára
> - Nem csak te, de a cég is kap némi *pozitív karmát*
> - Ha megnyitod a forrást, akkor kétszer is meggondolod, hogy hozzáadod-e azt a hekket, amit kitaláltál, mivel a saját megítélésedet is kockáztatod ezzel. Ettől a kód némileg jobb minőségű lesz.

Véleményem szerint a forrás megnyitása sokkal több előnnyel jár, mint hátránnyal, így érdemes próbát tenni vele. Ha esetleg van már tapasztalatod, vagy más véleményen vagy, akkor kíváncsian várom a hozzászólásodat a komment szekcióban...



