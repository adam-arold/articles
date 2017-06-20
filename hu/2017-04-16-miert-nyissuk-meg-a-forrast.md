> Ha dolgoztál már valahol nagyobb projekten, akkor jó eséllyel eszedbe jutott már, hogy a kódot amit írsz lehet megérné nyílt forráskódúvá tenni. Ebben a cikkben összefoglalom az előnyeit és a hátrányait, valamint adok pár ötletet, hogy miért lehet hasznos, ha még sosem foglalkoztál nyílt forráskódú szoftverrel.

Nyílt forráskódú (továbbiakban [OSS](https://en.wikipedia.org/wiki/Open-source_software)) szoftveren dolgozni kifejezetten szórakoztató. Te választhatod meg, hogy min dolgozz, mikor tedd azt és hogyan. Ha valami másoknak is hasznosat írsz, akkor előbb-utóbb az emberek elkezdik használni azt és itt kezdődnek az igazán érdekes dolgok.

## Pull request-ek (PR)
Legtöbbször a felhasználóidnak nem lesz idejük, vagy kedvük hozzájárulni a projektedhez, de van pár programozó, aki szeret javításokat küldeni, amiket átböngészve rájöhetsz, hogy mások hogyan használják a programodat. Ha magának a PR-nek a minősége jó (tehát javítás, vagy új feature fejlesztés, ami tesztelve van) akkor jópár dolgot tudsz tanulni belőle:
- Látni fogod __pontosan__, hogy mások hogyan használják a programodat
- Azt is látni fogod, hogy __hogyan szeretnék__ használni
- Illetve betekintést nyerhetsz a fejlesztő perspektívájába is, ami segíthet új szemszögből látni a dolgaidat

## Hiba jelentések
Azok a felhasználók, akik nem akarnak (vagy tudnak) hozzájárulni kóddal a projektedhez, de szeretnének rávilágítani egy hibára, hiba jelentést fognak küldnei neked. A legtöbb platform, ahol OSS kódot lehet host-olni rendelkezik olyan funkcióval, ahol ezt meg lehet tenni (például [GitHub](https://github.com/). Habár ezek nem annyira hasznosak, mint egy PR, tekinthetsz rájuk úgy, mint ingyen funkcionláis tesztelésre. Úgy, mint a PR-ek, a bug reportok is hasznosak abból a szempontból, hogy rávilágítanak arra, hogyan használják a szoftveredet és így rájöhetsz, hogy mikre nem gondoltál annak tervezése során.

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
- Használj [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) rendszert. Ha jól csinálod, akkor a meló javarészét delegálhatod egy programnak, és a hozzájárulások automatikusan le lesznek fordítva, sőt egyes platformok (pl. GitHub) nagyon könnyen integrálhatók build eszközökkel, mint a [Travis](https://travis-ci.org/), vagy kód elemző szoftverekkel, mint a [CodeCov](https://codecov.io/), ami kommentelni is tud PR-ekre. Egy példát [itt](https://github.com/Hexworks/hexameter/pull/24) láthatsz.

### Too much buzz around your project

You may start with something small but any project can become the second Death Star if you put in the time and the effort. Also if your project gains attention you can find yourself in the situation when you are flooded with feature requests and bug reports. While these are good for your projects you may feel like having another full-time job which can consume a lot of your spare time.

This can be worked around by letting proven contributors become committers and using a proper Kanban/Scrum board with a backlog and prioritizing tasks.

### Licensing problems

It is possible that you use a license like MIT which might be too permissive for your use case or AGPL which is too restricting. Choosing the right license is very important so I'd recommend perusing [this](https://choosealicense.com/) website for more information.

## Things to tell your boss

So now you might think that you are ready to open source parts of your project at your workplace or your own hobby project. In the former case you have a sometimes very hard task ahead of you: *convincing your boss*. Here are a few tips you can leverage to make your idea pass:

> - Open sourcing *comes at nearly no cost*
> - You don't have to open source everything just the parts which might be useful for others. This also helps with the modularization of your program.
> - You will get *free* bug reports, contributors or even core committers.
> - Programmers like to work for companies where they know that their code can get open sourced which makes their contributions more visible to the outside world.
> - Not only you but your company also gets some *positive karma*.
> - By making code visible for everyone you will think twice before adding an ugly hack since your professional reputation is on the line. This makes the code have slightly better quality.

I think that the benefits way outweigh the drawbacks so there is no reason not to try OSS out. Go for it and feel free to share your experiences in the comment section or if you think otherwise.





