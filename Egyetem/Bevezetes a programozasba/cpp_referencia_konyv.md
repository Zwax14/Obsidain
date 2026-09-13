---
tags:
  - cpp
  - nexusmind
  - egyetem
---

# C++ referenciakönyv — a nulláról a memóriakezelésig

Ugyanaz a formátum, mint a Python-könyvben: minden kódrészlet **önmagában futtatható**, a kimenetet mindenhol `std::cout`-tal kiírjuk, és mindenhol ott a **"miért így?"** magyarázat — nem csak azt mutatjuk meg, mit írj, hanem hogy *miért pont úgy* deklaráljuk, ahogy. A **C# összehasonlítás** végig megvan, mert a leggyorsabb út az, ha a meglévő programozói tudásodra építesz.

A C++ egyik legnagyobb különbsége minden eddig tanult nyelvhez képest (Python, C#, JavaScript), hogy **nincs automatikus szemétgyűjtő (garbage collector)** — neked kell kézzel gazdálkodnod a memóriával, vagy olyan eszközöket használnod, amik ezt helyetted, de átláthatóan intézik. Emiatt ez a könyv két nagy blokkra bomlik: az első fele (1–9. fejezet) nagyon hasonlít arra, amit már ismersz máshonnan, a második fele (10–15. fejezet) viszont valódi, C++-specifikus **új gondolkodásmódot** igényel — a mutatók, a memóriakezelés és a fordítási modell miatt. Ahol releváns, a **klasszikus C++ (nyers `new`/`delete`, nyers mutatók)** és a **modern C++ (smart pointerek, RAII)** megoldásait **egymás mellett** mutatom meg, mert mindkettővel találkozni fogsz — régebbi kódban és iskolai anyagban a klasszikus stílussal, éles/modern projektekben szinte kizárólag a modern stílussal.

---

## Tartalomjegyzék

1. [Mi a C++, és hogyan fordul le / fut le egy program](#fejezet-1)
2. [Alaptípusok, változók, operátorok](#fejezet-2)
3. [Vezérlési szerkezetek](#fejezet-3)
4. [Sztringkezelés](#fejezet-4)
5. [Standard I/O](#fejezet-5)
6. [Adatszerkezetek: tömb, vector, map, set, pair](#fejezet-6)
7. [Függvények (alprogramok)](#fejezet-7)
8. [Algoritmikus gondolkodás és alapalgoritmusok](#fejezet-8)
9. [Hibakezelés](#fejezet-9)
10. [Mutatók és referenciák](#fejezet-10)
11. [Memóriakezelés: stack, heap, RAII, smart pointerek](#fejezet-11)
12. [Objektumorientált programozás](#fejezet-12)
13. [Fordítási modell: header fájlok, namespace, linkelés](#fejezet-13)
14. [Template-ek (generikus programozás)](#fejezet-14)
15. [Move szemantika (modern C++)](#fejezet-15)
16. [Gyakorlati integráció: fájlfeldolgozás a nulláról](#fejezet-16)
17. [Automatizált tesztelés: Google Test](#fejezet-17)
18. [Megfelelési táblázat a hivatalos tematikához](#fejezet-18)


---

<a id="fejezet-1"></a>

## 1. Mi a C++, és hogyan fordul le / fut le egy program

Ez az első és talán legfontosabb fogalmi váltás Pythonhoz vagy C#-hoz képest. A C++ egy **fordított, natív gépi kódra lefordított** nyelv — nincs köztes bájtkód, nincs virtuális gép, nincs JIT. A folyamat:

```
forrás.cpp  →  [FORDÍTÓ: compiler]  →  objektumfájl (.o / .obj)  →  [LINKER]  →  futtatható program (.exe / bináris)
```

1. A **fordító (compiler)** minden `.cpp` fájlt *külön-külön* lefordít gépi kódra — ezt a lépést **fordítási egységnek (translation unit)** nevezzük. Itt derülnek ki a szintaktikai hibák és a legtöbb típushiba.
2. A **linker** ezután összefűzi ezeket a lefordított darabokat egyetlen futtatható fájllá, és feloldja a köztük lévő hivatkozásokat (pl. ha az egyik fájlban meghívsz egy függvényt, amit egy másik fájlban definiáltál).
3. Az eredmény egy **közvetlenül a processzor által futtatható bináris** — nincs mögötte semmilyen futásidejű értelmező, mint a Python PVM-je vagy a C# CLR-je.

Miért fontos ezt tudni? Mert ez magyarázza meg a C++ három legjellegzetesebb tulajdonságát:

- **Nincs futásidejű értelmezési többletköltség** — ezért olyan gyors a C++ program, mint amilyen gyorsan a processzor a gépi kódot végre tudja hajtani, semmilyen köztes réteg nem lassítja.
- **Minden hiba, ami kideríthető fordítási időben, ott is derül ki** — a C++ fordító sokkal szigorúbb típusellenőrzést végez, mint Python, és ez a program futtatása *előtt* megtörténik, nem közben.
- **Nincs automatikus szemétgyűjtő** — mivel nincs futásidejű "felügyelő réteg" (mint a Python PVM-je vagy a C# CLR-je, ami folyamatosan figyeli, mely objektumokra nincs már szükség), a memória felszabadításáról **neked kell gondoskodnod** — ez a téma a 11. fejezet gerince.

**C# összehasonlítás:** C#-ban `dotnet build` → IL kód (köztes, platformfüggetlen) → JIT fordítás futáskor natív gépi kódra, a CLR felügyelete alatt (ami a garbage collectort is futtatja). C++-ban nincs ez a köztes IL-réteg és nincs CLR — egyenesen natív gépi kód lesz belőle, fordítási időben, egyszer és mindenkorra, és senki nem "figyeli" futás közben a memóriádat.

### Hogyan fordíts és futtass egy C++ programot

```bash
g++ -std=c++20 fajlnev.cpp -o program
./program
```

Vagy CLion/Visual Studio-ban a zöld "Run" gombbal — a fordítás és a linkelés a háttérben, láthatatlanul lezajlik, mielőtt a program elindulna.

### Az első program

```cpp
#include <iostream>

int main() {
    std::cout << "Szia, NexusMind!" << std::endl;
    return 0;
}
```

Kimenet:

```
Szia, NexusMind!
```

Ellentétben Pythonnal, itt **kötelező** egy `main()` függvény — ez a program egyetlen, hivatalos belépési pontja. Ennek az az oka, hogy egy lefordított C++ programban nincs "a fájl teteje", ahonnan a végrehajtás automatikusan indulna (mint Pythonban) — a linkelt bináris futtatásakor az operációs rendszer **konkrétan a `main` nevű függvényt** keresi meg és hívja meg először, ez egy megállapodás az operációs rendszer és minden C/C++ program között. A `return 0;` azt jelzi az operációs rendszernek, hogy a program hiba nélkül fejeződött be (nem-nulla érték hibát jelezne).

Az `#include <iostream>` egy **preprocesszor-utasítás**, ami a fordítás *legelső* lépéseként, még mielőtt a tényleges fordítás elkezdődne, szó szerint bemásolja az `iostream` fejlécfájl tartalmát a te fájlodba — ez adja hozzá a `std::cout` és társai deklarációit. Ez teljesen más mechanizmus, mint a Python `import`-ja: a Python `import` futásidőben tölti be a modult, a C++ `#include` viszont **fordítási időben**, szöveges beillesztéssel dolgozik.

### Gyakorló kérdések

- Miért nincs C++-ban futásidejű bájtkód-értelmezés, mint Pythonban? *(Mert a fordító közvetlenül natív gépi kódra fordít, nincs virtuális gép.)*
- Melyik függvény a C++ program kötelező belépési pontja? *(A `main()`.)*
- Mikor és hogyan dolgozza fel a fordító az `#include`-ot? *(Fordítási időben, a preprocesszor szó szerint beilleszti a hivatkozott fájl tartalmát.)*

---

<a id="fejezet-2"></a>

## 2. Alaptípusok, változók, operátorok

### Statikus típusosság

A C++ **statikusan típusos**: minden változónak fordítási időben rögzített, változtathatatlan típusa van — ellentétben Pythonnal, ahol a típus az *értékhez* kötődik és futás közben változhat.

```cpp
#include <iostream>

int main() {
    int x = 5;
    std::cout << x << std::endl;   // 5

    x = 10;        // OK, ugyanaz a típus (int)
    // x = "szöveg";  // FORDÍTÁSI HIBA! Nem lehet stringet egy int-be tenni

    std::cout << x << std::endl;   // 10
    return 0;
}
```

Miért nem enged ez a nyelv annyi rugalmasságot, mint Python? Mert a C++ fordító a `x`-hez *fordítási időben* pontosan annyi memóriát foglal le, amennyi egy `int`-hez kell (jellemzően 4 bájtot), és ezt a memóriaterületet mindig `int`-ként értelmezi — ha megpróbálnál egy stringet beleírni, az egyszerűen nem férne bele értelmesen ugyanabba a 4 bájtos dobozba, ezért ezt a fordító már fordítási időben, a program futtatása előtt hibaként jelzi.

**C# összehasonlítás:** ez pontosan ugyanaz a modell, mint C#-ban (`int x = 5;` ott is fordítási időben rögzített típus) — ha ismered C#-ot, ez a rész semmi újat nem hoz, csak a szintaxis apró különbségeit.

### Alaptípusok

```cpp
int age = 25;              // egész szám, jellemzően 4 bájt
double price = 19.99;      // dupla pontosságú lebegőpontos szám, 8 bájt
float price_f = 19.99f;    // egyszeres pontosságú, 4 bájt — az "f" utótag jelzi a fordítónak
char grade = 'A';          // EGYETLEN karakter — figyelj, ez NEM ugyanaz, mint egy string!
bool is_active = true;
std::string name = "NexusMind";   // ez NEM alaptípus, hanem osztály — lásd 4. fejezet

std::cout << age << " " << price << " " << grade << " " << is_active << std::endl;
```

Kimenet:

```
25 19.99 A 1
```

Miért `1` és nem `true` a kiíráskor? Mert a `std::cout` a `bool` típust alapból számként jeleníti meg (`1` = igaz, `0` = hamis) — ha szövegesen akarod látni, explicit be kell kapcsolnod: `std::cout << std::boolalpha << is_active;` már `true`-t írna ki. Ez azért van így, mert a C++ történetileg a C nyelvből ered, ahol a `bool` típus eredetileg nem is létezett, csak `int`-tel szimulálták — a mai `bool` ennek örököse, és az alapértelmezett kiírási formátum ezt a hagyományt tükrözi.

⚠️ **Fontos csapda:** `char grade = 'A';` — **szimpla idézőjel** egyetlen karakterhez, `std::string name = "NexusMind";` — **dupla idézőjel** stringhez. Ez nem stílus kérdése, hanem a fordító kétféleképpen kezeli a kettőt: a `'A'` egyetlen bájtot (egy `char`-t) jelent, a `"NexusMind"` pedig egy karakterlánc-objektumot épít fel.

### Konstansok — `const` és `constexpr`

```cpp
const int MAX_RETRY_COUNT = 5;
// MAX_RETRY_COUNT = 10;   // FORDÍTÁSI HIBA! A const-ot nem lehet felülírni

constexpr double PI = 3.14159;   // fordítási időben KISZÁMÍTOTT állandó
std::cout << MAX_RETRY_COUNT << " " << PI << std::endl;
```

Ez az egyik legnagyobb különbség Pythonhoz képest: ott a "konstans" csak *konvenció* (nagybetűs név), amit a nyelv nem kényszerít ki (lásd a Python-könyv 2. fejezetét). C++-ban a `const` **valódi, fordító által kikényszerített** védelem — ha megpróbálod módosítani, a program *nem is fordul le*. A `constexpr` ennél is szigorúbb: azt is előírja, hogy az érték már *fordítási időben* ismert és kiszámítható legyen (nem függhet pl. felhasználói bemenettől) — ez lehetővé teszi, hogy a fordító ezt az értéket közvetlenül "beégesse" a gépi kódba, futásidejű számítás nélkül.

**C# összehasonlítás:** a C++ `const` a C# `readonly`/`const` kombinációjának felel meg — C#-ban is van valódi, fordító által kikényszerített védelem, ellentétben Pythonnal.

### Operátorok

| Kategória | C++ | Python | C# |
|---|---|---|---|
| Aritmetikai | `+ - * / % `| `+ - * / // %` | ugyanaz, mint C++ |
| Egész osztás | `/` (ha mindkét operandus `int`) | `//` | `/` (ha mindkét operandus `int`) |
| Hatványozás | nincs beépített operátor, `std::pow()` kell | `**` | `Math.Pow()` |
| Logikai | `&& \|\| !` | `and or not` | `&& \|\| !` |
| Növelés/csökkentés | `++ --` | nincs, `+= 1` kell | `++ --` |

```cpp
int a = 7, b = 2;
std::cout << a / b << std::endl;         // 3 — EGÉSZ osztás, mert mindkettő int!
std::cout << (double)a / b << std::endl; // 3.5 — explicit típuskényszerítés (cast) kell a lebegőpontos eredményhez
std::cout << a % b << std::endl;         // 1
```

⚠️ **Ez az egyik legveszélyesebb csapda C++-ban** — `a / b` **egész osztást** csinál, ha mindkét operandus egész szám, akkor is, ha az eredményt egy `double`-ba tárolnád:

```cpp
double result = 7 / 2;   // ELŐSZÖR kiszámolja 7/2-t EGÉSZKÉNT (=3), CSAK UTÁNA alakítja double-lé
std::cout << result << std::endl;   // 3 — NEM 3.5! Mert a hiba már megtörtént, mire a double-be került
```

Miért? Mert a C++ a `7 / 2` kifejezést a *hozzárendelés előtt*, önmagában értékeli ki — mivel `7` és `2` is `int` literál, az osztás egész osztásként fut le, az eredmény `3` lesz, és csak *ez* a `3` alakul át `double`-lé, amikor a `result`-ba kerül. A típuskényszerítést (cast) *az osztás előtt*, az egyik operandusra kell alkalmazni: `(double)7 / 2` vagy `7.0 / 2` — ekkor a fordító már az osztás *elején* lebegőpontos műveletet végez.

### Gyakorló kérdések

- Mi a fő különbség a C++ és a Python típusossága között? *(C++ statikusan típusos — a típus fordítási időben rögzített, futás közben nem változhat.)*
- Mi a különbség `const` és `constexpr` között? *(A `const` csak azt tiltja, hogy módosítsd az értéket; a `constexpr` ráadásul azt is megköveteli, hogy az érték fordítási időben kiszámítható legyen.)*
- Miért ad `7 / 2` egészet, nem 3.5-öt? *(Mert mindkét operandus `int`, ezért egész osztás történik, mielőtt bármi átalakulna `double`-lé.)*

---

<a id="fejezet-3"></a>

## 3. Vezérlési szerkezetek

### if / else if / else

```cpp
int temp = 22;

if (temp < 0) {
    std::cout << "Fagypont alatt" << std::endl;
} else if (temp < 20) {
    std::cout << "Hűvös" << std::endl;
} else {
    std::cout << "Kellemes" << std::endl;
}
```

Kimenet: `Kellemes`

Itt a `{}` határozza meg a blokkot (nem a behúzás, mint Pythonban) — a behúzás C++-ban **pusztán vizuális segítség a programozónak**, a fordítót egyáltalán nem érdekli, hány szóköz van egy sor elején. Ez azt is jelenti, hogy C++-ban *lehetséges* rosszul behúzott, de helyesen működő kódot írni (ami zavaró, de nem hibás) — Pythonban ez fordítva van: ott a behúzás *maga* a szintaxis.

### for ciklus — három formában

```cpp
// 1. klasszikus, számláló-alapú for (ez az "eredeti" C-stílusú forma)
for (int i = 0; i < 5; i++) {
    std::cout << i << " ";
}
std::cout << std::endl;   // 0 1 2 3 4

// 2. range-based for (modern C++11+, ez felel meg a Python "for item in lista"-nak)
std::vector<std::string> items = {"a", "b", "c"};
for (const std::string& item : items) {
    std::cout << item << " ";
}
std::cout << std::endl;   // a b c
```

A klasszikus `for (kezdőérték; feltétel; lépés)` három része explicit: a `int i = 0` a számláló létrehozása, a `i < 5` a feltétel, amit *minden* körben, a ciklustörzs *előtt* ellenőriz, az `i++` pedig minden kör *végén* fut le. Ez pontosan az a forma, amit a Python `range()`-dzsel szimulál, itt viszont ez a nyelv "natív", eredeti ciklusformája.

A `const std::string& item` a range-based for-ban azt jelenti: "add nekem az `item`-et **referenciaként** (`&`), amit **nem módosíthatok** (`const`)" — ez elkerüli, hogy minden egyes elemről *másolatot* készítsen a ciklus, ami nagy listáknál (sok, hosszú stringgel) komoly teljesítménykülönbséget jelenthet. A referenciák pontos működését a 10. fejezet mutatja be teljesen kifejtve.

### while ciklus

```cpp
int count = 0;
while (count < 5) {
    std::cout << count << " ";
    count++;   // itt VAN ++ operátor, ellentétben Pythonnal!
}
std::cout << std::endl;   // 0 1 2 3 4
```

### break, continue — pontosan úgy, mint Pythonban/C#-ban

```cpp
for (int i = 0; i < 10; i++) {
    if (i == 3) continue;
    if (i == 6) break;
    std::cout << i << " ";
}
std::cout << std::endl;   // 0 1 2 4 5
```

### switch — ha sok elágazást írnál egymás után

```cpp
int day = 3;
switch (day) {
    case 1:
        std::cout << "Hétfő" << std::endl;
        break;   // ⚠️ FONTOS! break nélkül a következő case is lefutna ("fallthrough")
    case 2:
        std::cout << "Kedd" << std::endl;
        break;
    case 3:
        std::cout << "Szerda" << std::endl;
        break;
    default:
        std::cout << "Ismeretlen nap" << std::endl;
}
```

Miért kell a `break` minden `case` végére? Mert a C++ `switch` alapból **"átcsorog" (fallthrough)** a következő `case`-be, ha nem állítod meg explicit módon — ez történeti örökség a C nyelvből, ahol ezt eredetileg szándékos rugalmasságnak szánták (több `case`-t egyetlen kód-blokkhoz lehet kötni), de a gyakorlatban ez a leggyakoribb kezdő-hiba forrása, mert szinte mindig `break`-et szeretnél minden ág végén.

### Gyakorló kérdések

- Mi határozza meg egy C++ kódblokk határait? *(A `{}`, nem a behúzás.)*
- Melyik ciklusforma felel meg leginkább a Python `for item in lista:`-nak? *(A range-based `for (típus& item : lista)`.)*
- Mi történik, ha kihagysz egy `break`-et egy `switch case`-ből? *(A vezérlés "átcsorog" a következő case-be, azt is végrehajtva.)*

---

<a id="fejezet-4"></a>

## 4. Sztringkezelés

C++-ban **két** string-fogalom él egymás mellett — ezt fontos tisztán látni, mert régi kódban és néha teljesítménykritikus helyeken a "régi" formával is találkozol.

### C-stílusú string (örökség a C nyelvből)

```cpp
const char* old_style = "Szia";   // egy karaktertömb kezdőcímére mutató pointer, lezárva egy '\0' karakterrel
std::cout << old_style << std::endl;
```

Ez technikailag egy `char` tömb a memóriában, aminek a *végét* egy speciális, nulla értékű `'\0'` karakter jelzi (ezért hívják "null-terminated string"-nek) — nincs benne eltárolva a hossza, azt minden művelet a `'\0'`-ig való haladással állapítja meg.

### std::string (modern, ezt használd mindig)

```cpp
#include <string>

std::string s = "  Szia NexusMind  ";

std::cout << s.length() << std::endl;              // 19 — a hossz, EL van tárolva, nem kell megszámolni
std::cout << s.substr(2, 4) << std::endl;           // "Szia" — 2. indextől, 4 karakter hosszan
std::cout << (s.find("NexusMind") != std::string::npos) << std::endl;   // 1 (true) — tartalmazás-vizsgálat
std::cout << s + "!" << std::endl;                  // összefűzés a "+" operátorral
```

Miért `!= std::string::npos` és nem egyszerűen `s.find(...)` az `if`-ben, mint Pythonban az `in`? Mert a `find()` nem `bool`-t ad vissza, hanem azt az **indexet**, ahol a keresett rész *elkezdődik* — ha nem találja, egy speciális, "nem létező index" értéket ad vissza, amit `std::string::npos` néven érhetsz el. Ennek az az oka, hogy a `find()` egyszerre két dolgot kommunikál: "megtaláltam-e" *és* "hol" — ha csak `bool`-t adna vissza, elveszne a pozíció-információ, amire gyakran szükséged van.

**C# összehasonlítás:** ez a `string.IndexOf()` C#-beli megfelelője, ahol a "nem található" esetet a `-1` jelzi — a C++ `npos` ugyanezt a szerepet tölti be, csak nem `-1`, hanem egy nagyon nagy, speciális konstans érték.

### Konkatenáció és formázás

```cpp
std::string name = "NexusMind";
int version = 1;

// C++20-tól: std::format (leginkább Python f-string-hez hasonló)
std::cout << std::format("Üdv a {} v{}-ben!", name, version) << std::endl;

// régebbi, klasszikus mód: stringstream
#include <sstream>
std::ostringstream oss;
oss << "Üdv a " << name << " v" << version << "-ben!";
std::cout << oss.str() << std::endl;
```

Kimenet mindkét esetben: `Üdv a NexusMind v1-ben!`

Miért van két különböző módszer? A `std::format` (C++20, közvetlen megfelelője a Python f-string-nek és a C# `$"..."`-nek) csak a legújabb fordítókban érhető el teljesen; a `stringstream` régebbi, minden fordítóval kompatibilis módszer, ahol a `<<` operátort (amit a `std::cout`-nál is használsz) egy string-építő objektumra alkalmazod közvetlen kiírás helyett.

### Szeletelés (substr) — a Python slicing megfelelője, de metódussal

```cpp
std::string s = "NexusMind";
std::cout << s.substr(0, 5) << std::endl;    // "Nexus" — 0. indextől, 5 karakter hosszan
std::cout << s.substr(5) << std::endl;       // "Mind"  — 5. indextől a string végéig
```

⚠️ **Fontos különbség Pythonhoz képest:** a Python `s[kezdő:vég]` a *végindexet* várja második paraméterként (a rés-modell szerint), a C++ `s.substr(kezdő, hossz)` viszont a *hosszt* — nem azt mondod meg, hol álljon meg, hanem hogy *hány karaktert* akarsz. `s.substr(0, 5)` tehát "az 5 karaktert a 0. indextől", nem "a 0. és 5. index közötti részt" (bár a végeredmény itt egybeesik).

### Gyakorló kérdések

- Miért kell a `'\0'` karakter egy C-stílusú string végén? *(Mert nincs eltárolva a hossz, a `'\0'` jelzi, hol ér véget a karaktersorozat.)*
- Mit ad vissza a `std::string::find()`, ha nem találja a keresett részt? *(A speciális `std::string::npos` értéket.)*
- Mi a különbség a Python `s[0:5]` és a C++ `s.substr(0, 5)` második paramétere között? *(Python a végindexet várja, C++ a kimásolandó karakterek számát.)*

---

<a id="fejezet-5"></a>

## 5. Standard I/O

### Kimenet

```cpp
#include <iostream>

std::cout << "Egy sor" << std::endl;
std::cout << "a" << " " << "b" << std::endl;     // "a b" — a szóközt neked kell beírnod!
std::cout << "Nincs sortörés";                    // nem ugrik új sorba
std::cout << " — folytatás ugyanabban a sorban" << std::endl;
```

⚠️ Ellentétben Python `print()`-jével, ami *automatikusan* szóközt tesz több argumentum közé, a C++ `std::cout << a << b` **semmit nem tesz közéjük** — ha szóközt akarsz, explicit ki kell írnod: `std::cout << a << " " << b`. Ennek az az oka, hogy a `<<` itt nem egy "kiíró függvény paraméterei", hanem egymás után **láncolt operátor-hívások** sorozata — mindegyik `<<` egy önálló műveletet jelent ("küldd ezt az adatot a kimeneti folyamba"), a köztük lévő tartalom kizárólag az, amit te odaírsz.

Az `std::endl` sortörést ír *és* azonnal kiüríti (flush-olja) a kimeneti puffert — ez utóbbi miatt lassabb, mint a sima `"\n"`, ezért teljesítménykritikus, sokszor futó kiírásnál inkább `"\n"`-t érdemes használni, és csak a program végén vagy fontos pontokon `std::endl`-t.

### Bemenet

```cpp
std::string name;
std::cout << "Add meg a neved: ";
std::cin >> name;                          // egyetlen szót olvas be (szóközig!)
std::cout << "Szia, " << name << "!" << std::endl;

int age;
std::cout << "Add meg az életkorod: ";
std::cin >> age;                           // KÖZVETLENÜL int-be olvas be, nem kell konvertálni!
std::cout << "Jövőre " << (age + 1) << " éves leszel." << std::endl;
```

Miért nem kell itt explicit konvertálás, mint Pythonban az `int(input(...))`-nál? Mert a `std::cin >> age` operátor *tudja*, milyen típusú változóba olvas be (ezt a fordító már fordítási időben eldönti, a `age` deklarált típusa alapján), és automatikusan megpróbálja a beolvasott szöveget azzá a típussá értelmezni — ha ez nem sikerül (pl. betűt írsz be egy `int` helyére), a `std::cin` "hiba állapotba" kerül, amit külön ellenőrizni kell (`if (std::cin.fail())`).

⚠️ **Csapda:** `std::cin >> name` csak **egy szót** olvas be, a szóközig — ha egész sort (pl. "Kovács János") akarsz beolvasni, a `std::getline(std::cin, name)`-et kell használnod:

```cpp
std::string full_name;
std::cout << "Teljes név: ";
std::getline(std::cin, full_name);   // a TELJES sort beolvassa, szóközökkel együtt
std::cout << "Szia, " << full_name << "!" << std::endl;
```

### Gyakorló kérdések

- Tesz-e automatikusan szóközt a `std::cout << a << b`? *(Nem, csak azt írja ki, amit explicit odaírsz.)*
- Miért nincs szükség explicit típuskonverzióra `std::cin >> age`-nél? *(Mert a fordító ismeri `age` fordítási időben rögzített típusát, és aszerint értelmezi a bemenetet.)*
- Mi a különbség `std::cin >> name` és `std::getline(std::cin, name)` között? *(Az első csak egy szót olvas be szóközig, a második a teljes sort.)*

---

<a id="fejezet-6"></a>

## 6. Adatszerkezetek: tömb, vector, map, set, pair

### C-stílusú tömb (fix méret, örökség a C nyelvből)

```cpp
int numbers[5] = {1, 2, 3, 4, 5};
std::cout << numbers[0] << " " << numbers[4] << std::endl;   // 1 5
std::cout << sizeof(numbers) / sizeof(numbers[0]) << std::endl;   // 5 — a méretet EZZEL a trükkel kell kiszámolni
```

Ez a legnyersebb forma: egy **fix méretű, fordítási időben rögzített** blokk a memóriában, aminek a mérete *nincs* eltárolva sehol explicit módon — ezért kell a fenti, kicsit ijesztő `sizeof(tömb) / sizeof(egy elem)` trükk a méret kiszámolásához (a teljes tömb bájtban mért mérete osztva egyetlen elem bájtban mért méretével). Ezt a formát ma már ritkán használjuk közvetlenül, inkább a `std::vector`-t.

### std::vector — a "munkaló" adatszerkezeted, ez felel meg a Python listnek

```cpp
#include <vector>

std::vector<int> numbers = {1, 2, 3, 4, 5};
numbers.push_back(6);          // hozzáadás a végéhez (a Python .append()-nek felel meg)
numbers.insert(numbers.begin(), 0);   // beszúrás az elejére — "begin()" az első pozícióra mutat
numbers.pop_back();            // utolsó elem eltávolítása

std::cout << numbers.size() << std::endl;    // 5 — hossz, MINDIG naprakészen tárolva
std::cout << numbers[0] << std::endl;        // 0 — indexelés, mint egy tömbnél

for (int n : numbers) {
    std::cout << n << " ";
}
std::cout << std::endl;   // 0 1 2 3 4
```

Miért `<int>` a `std::vector` neve után? Ez egy **template-paraméter** (bővebben 14. fejezet) — megmondod a fordítónak *milyen típusú* elemeket fog tárolni ez a konkrét vektor, és a fordító ez alapján, fordítási időben generál egy, kifejezetten `int`-ekre optimalizált vektor-kódot. Ez a C++ válasza arra a rugalmasságra, amit Pythonban a lista magától tud (bármilyen típusú elemet tárolhat egyetlen listában) — C++-ban egy `std::vector<int>` **csak** `int`-eket tárolhat, cserébe gyorsabb és típusbiztos.

A `std::vector` pontosan ugyanaz a "dinamikusan növekvő tömb" a háttérben, mint amit a Python-könyv a `list`-nél leírt — folytonos memóriablokk, ami szükség esetén nagyobbra másolja át magát. Az indexeléssel elérés (`numbers[3]`) O(1), az elejére való beszúrás O(n).

### std::pair és std::map — kulcs-érték párok

```cpp
#include <map>
#include <utility>

std::pair<std::string, int> point = {"x", 3};
std::cout << point.first << " = " << point.second << std::endl;   // x = 3

std::map<std::string, int> repo = {{"stars", 42}, {"forks", 3}};
repo["language_count"] = 2;    // új kulcs hozzáadása — pontosan úgy, mint Python dict-nél

std::cout << repo["stars"] << std::endl;   // 42

for (const auto& [key, value] : repo) {   // "structured binding" — szétbontja a párt egy lépésben
    std::cout << key << " -> " << value << std::endl;
}
```

Miért `[key, value]` a ciklusban? Ez egy modern C++17-es **structured binding** — a `std::map` minden eleme technikailag egy `std::pair<kulcs, érték>`, és e helyett, hogy `.first`/`.second`-dal kellene elérned őket, a `[key, value]` egyetlen lépésben "szétszedi" a párt két külön névre — ez pontosan az a mechanizmus, mint a Python `for key, value in repo.items():` sora, csak C++-ban ehhez explicit szintaxis kell (`auto&` + `[...]`), mert a fordítónak fordítási időben tudnia kell, milyen típusú lesz `key` és `value`.

⚠️ **Fontos:** a `std::map` — ellentétben a Python `dict`-tel — belülre **rendezett fabinárisan** tárolja a kulcsokat (a kulcs szerinti sorrendben iterálsz végig rajta), és a keresés O(log n), nem O(1). Ha téged kifejezetten a Python `dict`-hez hasonló, hash-alapú, átlagosan O(1) keresés érdekel, a `std::unordered_map`-et kell használnod.

### std::set — egyedi elemek

```cpp
#include <set>

std::set<std::string> tags = {"bug", "urgent", "bug"};   // a duplikátum eltűnik
tags.insert("reviewed");

std::cout << tags.size() << std::endl;              // 3
std::cout << (tags.count("bug") > 0) << std::endl;  // 1 (true) — count() adja vissza, hányszor van benne (0 vagy 1 egy setnél)
```

A C++ `std::set`-nek nincs `in` operátora, mint Pythonnak — helyette a `count()` (ami 0-t vagy 1-et ad vissza, mivel egyedi elemekről van szó) vagy a `find() != end()` mintát használod a tartalmazás-vizsgálathoz.

### Gyakorló kérdések

- Mire való a `<int>` a `std::vector<int>` névben? *(Template-paraméter — megmondja a fordítónak, milyen típusú elemeket tárol a vektor.)*
- Melyik C++ adatszerkezet felel meg leginkább a Python `dict`-nek, hash-alapú, O(1) kereséssel? *(A `std::unordered_map`, nem a sima `std::map`, ami rendezett és O(log n).)*
- Mit csinál egy "structured binding" (`auto& [key, value]`)? *(Egy lépésben szétbontja a `std::pair`-t/`std::map` elemét két külön névre.)*

---

<a id="fejezet-7"></a>

## 7. Függvények (alprogramok)

### Alapok, és a C++-specifikus paraméterátadási módok

```cpp
std::string greet(const std::string& name, std::string greeting = "Szia") {
    return greeting + ", " + name + "!";
}

std::cout << greet("Anna") << std::endl;                  // "Szia, Anna!" — default paraméter
std::cout << greet("Béla", "Hello") << std::endl;         // "Hello, Béla!"
```

Ez a C++ egyik legfontosabb, saját fogalma, aminek Pythonban nincs közvetlen megfelelője: **háromféleképpen** adhatsz át egy paramétert, és mindháromnak más a hatása. Nézzük meg mindhármat, ugyanazzal a példával, hogy lásd a különbséget:

```cpp
#include <iostream>
#include <vector>

// 1. ÉRTÉK SZERINT (pass by value) — MÁSOLATOT kap a függvény
void try_double_by_value(int n) {
    n = n * 2;   // ez csak a MÁSOLATOT módosítja
}

// 2. REFERENCIA SZERINT (pass by reference) — a függvény az EREDETIT módosítja
void double_by_reference(int& n) {
    n = n * 2;   // ez az EREDETI változót módosítja, mert n egy "másik neve" ugyanannak a dobozna
}

// 3. CONST REFERENCIA — nem másol, de nem is engedi módosítani (leggyakoribb objektumoknál)
void print_vector(const std::vector<int>& numbers) {
    // numbers.push_back(1);   // FORDÍTÁSI HIBA lenne, mert const!
    for (int n : numbers) std::cout << n << " ";
    std::cout << std::endl;
}

int main() {
    int a = 5;
    try_double_by_value(a);
    std::cout << a << std::endl;   // 5 — VÁLTOZATLAN! A függvény csak a másolatát duplázta

    int b = 5;
    double_by_reference(b);
    std::cout << b << std::endl;   // 10 — MÓDOSULT! A referencia az eredeti dobozra mutatott

    std::vector<int> big_list = {1, 2, 3, 4, 5};
    print_vector(big_list);   // nem másolja le az egész vektort, csak "kölcsönveszi"

    return 0;
}
```

Miért van szükség mindhárom formára? Mert mindhárom más problémát old meg:

- **Érték szerint** (`int n`): biztonságos, mert a függvény nem tudja "meglepetésszerűen" módosítani a hívó változóját — de kis, olcsó típusoknál (int, double, char) ésszerű, mert a másolás gyors. Nagy objektumoknál (pl. egy 10 000 elemű vektor) viszont **pazarló**, mert minden híváskor lemásolná a teljes tartalmat.
- **Referencia szerint** (`int& n`): akkor kell, ha a függvénynek *ténylegesen módosítania* kell a hívó eredeti változóját (ez a C++ megfelelője annak, amit C#-ban `ref` kulcsszóval jelzel — ellentétben C#-szal, ahol ezt a hívó oldalon is jelezni kell (`Method(ref x)`), C++-ban a hívó oldalon *semmi extra jel nincs*, `double_by_reference(b)`-t simán úgy hívod, mint bármelyik függvényt — ez az egyik leggyakoribb kezdő-csapda: a hívás helyén nem látszik, hogy referenciával dolgozol).
- **Const referencia** (`const std::vector<int>& numbers`): ez a **leggyakrabban használt** forma nagyobb objektumoknál, amikor a függvénynek *nem kell módosítania* a paramétert, csak *olvasnia* — így elkerülöd a másolás költségét (mint a referenciánál), de a `const` garantálja, hogy a függvény véletlenül sem módosítja a hívó adatát (mint az érték szerinti átadásnál).

**C# összehasonlítás:** C#-ban az érték- és referenciatípusok (osztály vs struct) már eleve eldöntik ezt neked jórészt; a `ref`/`out` kulcsszavak adnak explicit kontrollt, de a hívó oldalon is jelölnöd kell őket. C++-ban ez a döntés *teljesen a te kezedben van*, minden egyes paraméternél külön, és a hívó oldalon nem látszik — ezért van szükség arra, hogy a függvény *deklarációjából* (a `&` jelből) mindig lásd, mire számíthatsz.

### Túlterhelés (overloading) — nincs Pythonban, C#-ban igen

```cpp
int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }

std::cout << add(2, 3) << std::endl;         // 5 — az int verzió fut le
std::cout << add(2.5, 3.5) << std::endl;     // 6 — a double verzió fut le
```

A C++ (akárcsak C#) megengedi, hogy **ugyanaz a függvénynév** többször is szerepeljen, más paraméter-típusokkal — a fordító a hívás argumentumainak típusa alapján, *fordítási időben* dönti el, melyik verziót kell meghívni. Pythonban ez nem lehetséges (ott egy névhez mindig csak egy függvény tartozhat egy adott pillanatban — a második definíció felülírná az elsőt), mert Python futásidőben, dinamikusan dönt, és nincs "fordítási idejű" pillanat, amikor a típusok alapján választani lehetne.

### Gyakorló kérdések

- Mi a különbség érték szerinti és referencia szerinti paraméterátadás között? *(Érték szerint a függvény másolatot kap és nem tudja módosítani az eredetit; referencia szerint ugyanarra a dobozra fér hozzá, és módosíthatja azt.)*
- Miért a `const std::vector<int>&` a leggyakoribb paramétertípus nagyobb objektumoknál? *(Elkerüli a másolás költségét, de a `const` garantálja, hogy a függvény nem módosítja a bemenetet.)*
- Miért nem lehetséges function overloading Pythonban? *(Mert Python dinamikusan, futásidőben dönt, nincs fordítási idejű pillanat a típus alapú választásra.)*

---

<a id="fejezet-8"></a>

## 8. Algoritmikus gondolkodás és alapalgoritmusok

Ugyanaz a három alapminta, mint a Python-könyvben (összegzés, számlálás, keresés) — de C++-ban ezekre **beépített, könyvtári algoritmusok** is léteznek a `<algorithm>` fejlécben, amiket érdemes megismerni a kézzel írt ciklus mellett.

### 1. Összegzés

```cpp
#include <vector>
#include <numeric>   // std::accumulate-hoz

int sum_manual(const std::vector<int>& numbers) {
    int total = 0;              // akkumulátor
    for (int n : numbers) {
        total += n;
    }
    return total;
}

std::vector<int> nums = {1, 2, 3, 4, 5};
std::cout << sum_manual(nums) << std::endl;                          // 15 — kézzel írt verzió
std::cout << std::accumulate(nums.begin(), nums.end(), 0) << std::endl;   // 15 — beépített, ugyanaz az eredmény
```

A `std::accumulate(kezdet, vég, kezdőérték)` pontosan azt csinálja, amit a kézzel írt ciklusod — végigmegy a `nums.begin()`-től `nums.end()`-ig, és összeadja az elemeket a `kezdőérték`-hez képest. A `begin()`/`end()` az úgynevezett **iterátorok** — ezekről bővebben a mutatóknál (10. fejezet) lesz szó, most elég annyit tudni, hogy a `begin()` az első elemre mutat, az `end()` pedig *az utolsó elem UTÁNI* (nem létező) pozícióra — ez ugyanaz a "kizárólagos végpont" elv, amit a Python slicing-nál láttál.

### 2. Számlálás (feltételes)

```cpp
#include <algorithm>

int count_evens_manual(const std::vector<int>& numbers) {
    int count = 0;
    for (int n : numbers) {
        if (n % 2 == 0) count++;
    }
    return count;
}

int even_count = std::count_if(nums.begin(), nums.end(), [](int n) { return n % 2 == 0; });
std::cout << count_evens_manual(nums) << std::endl;   // 2
std::cout << even_count << std::endl;                 // 2 — ugyanaz beépítettel
```

A `[](int n) { return n % 2 == 0; }` egy **lambda-kifejezés** — egy névtelen, "helyben" definiált függvény, amit közvetlenül a `std::count_if` argumentumaként adsz át. A `[]` (capture lista) azt mondja meg, mely külső változókhoz férhet hozzá a lambda-törzs (itt üres, mert nincs rá szükség), a `(int n)` a paramétere, a `{...}` pedig a törzse. Ez ugyanazt a szerepet tölti be, mint a Python `lambda n: n % 2 == 0` vagy a C# `n => n % 2 == 0`.

### 3. Lineáris keresés

```cpp
int linear_search(const std::vector<int>& numbers, int target) {
    for (size_t i = 0; i < numbers.size(); i++) {
        if (numbers[i] == target) return static_cast<int>(i);
    }
    return -1;
}

std::cout << linear_search(nums, 3) << std::endl;    // 2
std::cout << linear_search(nums, 99) << std::endl;   // -1

auto it = std::find(nums.begin(), nums.end(), 3);
std::cout << (it != nums.end()) << std::endl;        // 1 (true) — megtalálta
```

Miért `size_t` az `i` típusa, nem `int`? Mert a `.size()` metódus (ami a vektor méretét adja vissza) `size_t` típust ad vissza — ez egy **előjel nélküli** egész típus, amit a nyelv kifejezetten méretek/számosságok tárolására szán (soha nem lehet negatív, ezért nincs is rá szükség), és a fordító figyelmeztetést adna, ha egy előjeles `int`-tel hasonlítanád össze egy előjel nélküli `size_t`-t.

A `std::find` visszatérési értéke egy **iterátor**, nem index — ezért kell `it != nums.end()`-del ellenőrizni a sikerességet, nem egy konkrét számmal (mint a kézzel írt `-1`-es megállapodásnál).

### Gyakorló kérdések

- Mit csinál a `std::accumulate`? *(Összegzi az elemeket egy adott kezdőértékhez képest, egy tartomány elején és végén megadott iterátorok között.)*
- Mi egy lambda-kifejezés C++-ban, és mire használjuk? *(Egy névtelen, helyben definiált függvény, amit gyakran algoritmusoknak adunk át feltételként.)*
- Miért `size_t`, nem `int` a `.size()` visszatérési típusa? *(Mert méretek/számosságok sosem lehetnek negatívak, ezért előjel nélküli típust használunk.)*

---

<a id="fejezet-9"></a>

## 9. Hibakezelés

```cpp
#include <stdexcept>

try {
    int value;
    std::cin >> value;
    if (value == 0) {
        throw std::invalid_argument("Nullával nem lehet osztani!");
    }
    int result = 10 / value;
    std::cout << "Eredmény: " << result << std::endl;
} catch (const std::invalid_argument& e) {
    std::cout << "Hiba: " << e.what() << std::endl;
} catch (const std::exception& e) {
    std::cout << "Egyéb hiba: " << e.what() << std::endl;
}
```

**C# összehasonlítás:** `try/catch` ≈ `try/catch` — szintaktikailag szinte azonos, de C++-ban nincs `finally` blokk. Ehelyett a C++ a **RAII elvet** (Resource Acquisition Is Initialization, lásd 11. fejezet) használja: az erőforrások felszabadítását nem egy explicit `finally`-blokkal, hanem az objektumok destruktorával (ami *garantáltan* lefut, amikor az objektum "kimegy a hatóköréből", akkor is, ha kivétel történt) oldja meg — ez egy teljesen más, de legalább annyira megbízható mechanizmus.

Miért `const std::exception& e` (referencia) a `catch`-ben, nem `std::exception e`? Ugyanaz az elv, mint a 7. fejezet függvényparamétereinél: ha érték szerint fognád el, a kivétel-objektum lemásolódna (feleslegesen), és ha az öröklési hierarchiában egy leszármazott kivételt fogsz el egy ősosztály típusaként (ez a "szeletelés" jelensége), elveszíthetnéd a leszármazott extra adatait. A referencia elkerüli mindkét problémát.

### Saját kivétel

```cpp
class InvalidConfigError : public std::runtime_error {
public:
    explicit InvalidConfigError(const std::string& message)
        : std::runtime_error(message) {}
};

void load_config(bool has_api_key) {
    if (!has_api_key) {
        throw InvalidConfigError("Hiányzó API kulcs");
    }
}

try {
    load_config(false);
} catch (const InvalidConfigError& e) {
    std::cout << "Konfigurációs hiba: " << e.what() << std::endl;
}
```

Miért `: public std::runtime_error(message)` a konstruktor után? Ez az **öröklés** és egyben a **szülő-konstruktor explicit meghívása** — a `InvalidConfigError` a beépített `std::runtime_error`-ból örököl (hogy kompatibilis legyen a standard kivétel-hierarchiával, és legyen `.what()` metódusa), a kettőspont utáni rész pedig előírja, hogy amikor egy `InvalidConfigError` létrejön, a `message`-et adja tovább a szülő (`runtime_error`) konstruktorának, ami eltárolja azt, amit a `.what()` majd vissza tud adni. Ez a mechanizmus közelebbről a 12. fejezetben (öröklés) kerül elő.

### Gyakorló kérdések

- Van-e `finally` blokk C++-ban? *(Nincs — helyette a RAII elv és a destruktorok garantálják az erőforrás-felszabadítást.)*
- Miért `const std::exception&` a catch paramétere, nem érték szerinti átvétel? *(Elkerüli a felesleges másolást, és megakadályozza a leszármazott kivétel adatainak elvesztését.)*

---

<a id="fejezet-10"></a>

## 10. Mutatók és referenciák

Ez a C++ **legsajátosabb**, Pythonban és C#-ban (biztonságos kódban) nem létező fogalma — ezért ezt teljesen a "gépiesen lekövethető" szintre visszük, ahogy a Python-könyv is tette a slicing-gal és a referencia-szemantikával.

### A memória, mint sorszámozott dobozok

Képzeld el a számítógép memóriáját úgy, mint egy hosszú sor **sorszámozott dobozt**. Minden doboznak van egy **címe** (egy szám, ami megmondja, hányadik doboz), és tartalmazhat egy értéket.

```cpp
int x = 42;
std::cout << x << std::endl;      // 42 — a DOBOZ TARTALMA
std::cout << &x << std::endl;     // pl. 0x7ffd... — a DOBOZ CÍME (memóriacím)
```

A `&x` (cím-operátor) nem "x-et" adja vissza, hanem *azt a helyet a memóriában*, ahol `x` lakik. Ez az az érték, amit egy **mutató (pointer)** tárolni tud:

```cpp
int x = 42;
int* ptr = &x;    // "ptr egy int-re mutató pointer, aminek értéke x CÍME"

std::cout << ptr << std::endl;    // ugyanaz a cím, mint &x volt
std::cout << *ptr << std::endl;   // 42 — a *-gal "dereferálunk": "menj el arra a címre, és hozd el, ami ott van"
```

Kövessük végig, mi történik lépésről lépésre:

1. `int x = 42;` — a Python "címke a dobozon" modelljéhez hasonlóan, itt is létrejön egy doboz a memóriában, `42`-vel feltöltve, és `x` erre a dobozra hivatkozik.
2. `int* ptr = &x;` — létrejön egy **másik** doboz (`ptr`), aminek a tartalma **nem 42, hanem `x` doboz-a a memóriacíme**. A `ptr` típusa `int*` ("int-re mutató pointer") — ez a fordítónak azt jelzi, hogy amit `ptr` tárol, azt egy `int`-ként elhelyezkedő memóriaterület címeként kell értelmezni.
3. `*ptr` — ez a **dereferencia operátor**: "menj el arra a címre, amit `ptr` tartalmaz, és add vissza, ami *ott* van" — ez lesz `42`, mert `ptr` az `x` dobozának címét tárolja.

Ha *a pointeren keresztül* módosítod az értéket, az *eredeti* doboz módosul — ez pontosan az a mechanizmus, ami a Python referencia-szemantikáját (list_a/list_b ugyanarra mutat) a háttérben lehetővé teszi, csak itt te magad, explicit módon irányítod:

```cpp
int x = 42;
int* ptr = &x;

*ptr = 100;    // "menj el a ptr által mutatott címre, és írj oda 100-at"
std::cout << x << std::endl;   // 100 — az EREDETI x is megváltozott!
```

### Null pointer — amikor egy mutató "nem mutat sehova"

```cpp
int* empty_ptr = nullptr;   // explicit "nem mutat semmire" érték

if (empty_ptr == nullptr) {
    std::cout << "A pointer üres, ne dereferáld!" << std::endl;
}

// *empty_ptr;   // ⚠️ FUTÁSIDEJŰ HIBA (segmentation fault)! Sosem szabad dereferálni egy nullptr-t
```

Ez a C++ megfelelője a Python `None`-nak vagy a C# `null`-nak — de itt **sokkal veszélyesebb**, mert ha véletlenül dereferálsz egy `nullptr`-t, a program **azonnal, kontrollálatlanul összeomlik** (segmentation fault) — nincs Python-szerű, elkapható `NameError`/`AttributeError`, amit `try/except`-tel kezelhetnél.

### Mutató vs referencia — mikor melyiket?

```cpp
int a = 5;

int& ref = a;    // REFERENCIA — "ref" a-nak egy MÁSIK NEVE, a létrehozás pillanatában el kell dönteni, mire mutat
ref = 10;
std::cout << a << std::endl;   // 10 — mert ref és a UGYANAZ a doboz

int* ptr = &a;   // MUTATÓ — ptr egy KÜLÖN doboz, ami a CÍMÉT tárolja a-nak
int b = 20;
ptr = &b;        // a mutatót ÁT LEHET ÁLLÍTANI, hogy másik dobozra mutasson
std::cout << *ptr << std::endl;   // 20 — most már b-re mutat
```

A legfontosabb fogalmi különbség: egy **referencia** olyan, mint egy második címke ugyanazon a dobozon — a létrehozásakor véglegesen el kell dönteni, melyik dobozra vonatkozik, és ez **soha többé nem változtatható meg** (nem lehet "átirányítani"). Egy **mutató** viszont maga is egy doboz, aminek a tartalma (egy cím) **bármikor módosítható**, hogy másik dobozra mutasson — ráadásul lehet `nullptr` is, azaz "sehova sem mutat", amire a referenciának nincs is lehetősége (egy referenciának *mindig* léteznie kell mögötte valós dolognak, a létrehozás pillanatától kezdve).

Ökölszabály, amit érdemes megjegyezni: **ha nem feltétlenül kell "sehova sem mutatás" lehetősége vagy az "átirányíthatóság", használj referenciát** — biztonságosabb, mert nem lehet véletlenül `nullptr`-t dereferálni. Ezért láttad a 7. fejezetben, hogy a függvényparaméterek `const std::vector<int>&` formában voltak, nem `const std::vector<int>*`-ban.

**C# összehasonlítás:** C#-ban a `ref`/`out` paraméterek fogalmilag a C++ referenciáihoz hasonlítanak (biztonságosak, nem lehetnek `null`, amíg nem explicit `Nullable`), a nyers pointerek pedig csak `unsafe` blokkban érhetők el C#-ban — C++-ban ez az *alapértelmezett*, mindennapos eszköz, nincs hozzá külön "veszélyes" jelölés a nyelvben.

### Gyakorló kérdések

- Mit ad vissza a `&x` operátor? *(A `x` változó memóriacímét, nem az értékét.)*
- Mit csinál a `*ptr` dereferencia-operátor? *(Elhozza a `ptr` által tárolt címen lévő tényleges értéket.)*
- Mi a fő különbség egy referencia és egy mutató között? *(A referencia soha nem irányítható át és nem lehet "üres"; a mutató mindkettőre képes, beleértve a `nullptr`-t is.)*

---

<a id="fejezet-11"></a>

## 11. Memóriakezelés: stack, heap, RAII, smart pointerek

Ez a fejezet a C++ legfontosabb, önálló fogalmi blokkja — itt mutatom meg **egymás mellett** a klasszikus (nyers `new`/`delete`) és a modern (smart pointer) megközelítést, mert mindkettővel találkozni fogsz.

### Stack vs heap — két külön memóriaterület

```cpp
void stack_example() {
    int x = 42;   // STACK-en jön létre — automatikusan megszűnik, amint a függvény visszatér
}   // itt x memóriája AUTOMATIKUSAN felszabadul, nincs teendőd

int* heap_example() {
    int* ptr = new int(42);   // HEAP-en jön létre — TE vagy felelős a felszabadításáért!
    return ptr;
}   // itt ptr (a mutató maga) megszűnik, de amire MUTATOTT, az a heap-en ÉL TOVÁBB
```

A **stack** egy automatikusan kezelt memóriaterület: minden lokális változó, amit "simán" deklarálsz (`int x = 42;`), ide kerül, és amint a függvény, amiben deklaráltad, visszatér, ez a memória **automatikusan, garantáltan felszabadul** — pontosan úgy, mint C#-ban vagy Pythonban minden lokális változó. A **heap** ezzel szemben egy sokkal nagyobb, de *kézzel kezelt* memóriaterület — amit a `new` kulcsszóval foglalsz le, az **addig él, amíg te explicit fel nem szabadítod** a `delete`-tel, függetlenül attól, hogy melyik függvényben vagy blokkban voltál, amikor lefoglaltad.

Miért van egyáltalán szükség a heap-re, ha a stack "ingyen" működik? Mert a stack-en lévő adat *megszűnik*, amint a függvény visszatér — ha egy objektumnak *tovább* kell élnie, mint amíg a létrehozó függvény fut (pl. vissza akarod adni egy mutatót/referenciát, vagy az objektum mérete futásidőben dől el, esetleg nagyon nagy), a heap-re van szükséged.

### A klasszikus mód: nyers `new`/`delete`

```cpp
int* ptr = new int(42);       // HEAP-en foglal helyet egy int-nek, 42-vel feltöltve
std::cout << *ptr << std::endl;   // 42

delete ptr;    // ⚠️ KÖTELEZŐ, különben MEMORY LEAK (memóriaszivárgás) történik!
// std::cout << *ptr;   // ⚠️ "DANGLING POINTER" — ptr már felszabadított memóriára mutat, ezt HASZNÁLNI TILOS

ptr = nullptr;   // jó szokás: felszabadítás UTÁN azonnal nullptr-re állítani, hogy véletlen újrahasználat kiderüljön
```

Két klasszikus, súlyos hiba leselkedik itt:

1. **Memory leak (memóriaszivárgás):** ha elfelejted meghívni a `delete`-et, az a memóriaterület *soha nem szabadul fel*, amíg a program fut — hosszan futó programoknál (pl. egy szerver) ez fokozatosan felemészti az összes elérhető memóriát, végül a program (vagy az egész gép) összeomlik.
2. **Dangling pointer (lógó mutató):** ha *használod* a mutatót *azután*, hogy már `delete`-tel felszabadítottad, azt a memóriaterületet a rendszer bármikor újra kioszthatja valami másnak — a viselkedés kiszámíthatatlan (undefined behavior): lehet, hogy régi, "szemét" adatot olvasol vissza, lehet, hogy a program azonnal összeomlik, és a legrosszabb eset, hogy *néha* jól működik, és csak véletlenszerűen, nehezen reprodukálhatóan hibázik.

Ezen kívül van egy harmadik klasszikus hiba, a **double free** (kétszer felszabadítás — ha kétszer hívod meg a `delete`-et ugyanarra a pointerre), ami szintén undefined behavior.

### A modern mód: smart pointerek — ugyanaz a probléma, automatikus megoldással

```cpp
#include <memory>

void modern_example() {
    std::unique_ptr<int> ptr = std::make_unique<int>(42);
    std::cout << *ptr << std::endl;   // 42 — ugyanúgy dereferálható, mint egy nyers pointer

}   // itt, amikor ptr "kimegy a hatóköréből" (a függvény véget ér), AUTOMATIKUSAN felszabadul a mögötte lévő memória
    // NINCS SZÜKSÉG delete-re, és NEM LEHET elfelejteni!
```

Ez a **RAII elv (Resource Acquisition Is Initialization)** gyakorlati alkalmazása, ami a C++ egyik legfontosabb, saját ötlete: az erőforrás (itt: a heap-memória) élettartamát egy **objektum élettartamához** kötjük — amikor az objektum (a `std::unique_ptr`) létrejön, lefoglalja az erőforrást; amikor az objektum megszűnik (a hatóköréből "kimegy", ami a stack-en automatikusan, garantáltan megtörténik, akkor is, ha közben kivétel repült), a **destruktora automatikusan felszabadítja** az erőforrást. Ez azt jelenti, hogy a `std::unique_ptr` maga egy kis stack-objektum, ami *belül* egy heap-re mutató nyers pointert rejt, és a destruktorában automatikusan meghívja rá a `delete`-et — te ezt sosem látod, csak élvezed az eredményét.

```cpp
std::unique_ptr<int> a = std::make_unique<int>(42);
// std::unique_ptr<int> b = a;   // FORDÍTÁSI HIBA! unique_ptr nem MÁSOLHATÓ

std::unique_ptr<int> b = std::move(a);   // csak ÁTMOZGATHATÓ (lásd 15. fejezet), a a-t "kiüríti"
std::cout << (a == nullptr) << std::endl;   // 1 (true) — a most már üres
std::cout << *b << std::endl;               // 42 — b vette át a tulajdonjogot
```

Miért nem másolható a `std::unique_ptr`? Mert a neve is jelzi: **egyedüli (unique) tulajdonjogot** képvisel egy heap-objektum felett — a nyelv tervezői szándékosan tiltják a másolást, hogy sose alakulhasson ki olyan helyzet, ahol két különböző `unique_ptr` ugyanarra a memóriára mutat, és mindkettő megpróbálná felszabadítani (ez lenne a double free). Ha meg akarod osztani a tulajdonjogot *több* hely között, a `std::shared_ptr`-t kell használnod:

```cpp
std::shared_ptr<int> s1 = std::make_shared<int>(42);
std::shared_ptr<int> s2 = s1;   // ez MEGENGEDETT — mindketten "résztulajdonosok" lesznek

std::cout << s1.use_count() << std::endl;   // 2 — két résztulajdonos van jelenleg

s2.reset();   // s2 lemond a tulajdonjogról
std::cout << s1.use_count() << std::endl;   // 1 — csak s1 maradt
// a mögöttes memória csak akkor szabadul fel, amikor az UTOLSÓ résztulajdonos is lemond róla
```

A `std::shared_ptr` belül egy **referenciaszámlálót (reference count)** tart — minden alkalommal, amikor egy újabb `shared_ptr` "csatlakozik" ugyanahhoz az objektumhoz, a számláló nő; amikor egy `shared_ptr` megszűnik vagy explicit lemond (`reset()`), a számláló csökken; amikor a számláló **eléri a nullát**, csak *akkor* szabadul fel ténylegesen a memória. Ez a mechanizmus fogalmilag hasonlít a Python vagy C# garbage collectorához (a "hányan hivatkoznak erre az objektumra" elvhez), csak C++-ban ez nem egy háttérben futó, automatikus folyamat, hanem egy konkrét, a `shared_ptr` objektum által, azonnal, minden lépésnél elvégzett számolás.

### Összefoglaló táblázat — klasszikus vs modern

| | Nyers `new`/`delete` | `std::unique_ptr` | `std::shared_ptr` |
|---|---|---|---|
| Felszabadítás | Kézzel, `delete`-tel | Automatikus (RAII) | Automatikus, referenciaszámlálóval |
| Másolható? | A pointer igen (de veszélyes) | Nem, csak mozgatható | Igen, megosztott tulajdonlással |
| Memory leak veszélye | Igen, ha elfelejted a `delete`-et | Gyakorlatilag nincs | Gyakorlatilag nincs |
| Mikor használd | Régi kódbázis, iskolai alapok megértéséhez | **Alapértelmezett választás** modern kódban, ha egy tulajdonos van | Amikor tényleg több hely osztozik egy objektumon |

**C# összehasonlítás:** C#-ban ezt a *teljes* problémakört a garbage collector oldja meg helyetted, automatikusan, futásidőben, anélkül hogy neked bármit deklarálnod kellene — a C++ smart pointerek ezt a kényelmet próbálják visszaadni, de **fordítási időben**, futásidejű felügyelő nélkül: a `unique_ptr` "ingyen" jár (nulla futásidejű többletköltség a nyers pointerhez képest), a `shared_ptr` viszont a referenciaszámláló miatt van egy kis, mérhető futásidejű költsége.

### Gyakorló kérdések

- Mi a különbség a stack és a heap memória között? *(A stack automatikusan felszabadul a függvény visszatérésekor; a heap-et kézzel kell felszabadítani, tovább élhet, mint a létrehozó függvény.)*
- Mi az a memory leak, és hogyan keletkezik? *(Amikor `new`-val foglalt memóriát elfelejtesz `delete`-elni — a memória soha nem szabadul fel.)*
- Mi a RAII elv lényege? *(Az erőforrás élettartamát egy objektum élettartamához kötjük — a destruktor automatikusan felszabadít, amikor az objektum kimegy a hatóköréből.)*
- Miért nem másolható a `std::unique_ptr`? *(Mert egyedüli tulajdonjogot képvisel — a másolás kockázatos duplikált felszabadításhoz (double free) vezethetne.)*

---

<a id="fejezet-12"></a>

## 12. Objektumorientált programozás

### Osztályok alapjai

```cpp
class GitHubNotification {
public:
    GitHubNotification(const std::string& title, const std::string& repo)
        : title_(title), repo_(repo), is_read_(false) {}   // "inicializációs lista"

    bool is_noise() const {   // a végén lévő "const" azt ígéri: EZ A METÓDUS NEM MÓDOSÍTJA az objektumot
        std::string lower_repo = repo_;
        std::transform(lower_repo.begin(), lower_repo.end(), lower_repo.begin(), ::tolower);
        return lower_repo.find("dependabot") != std::string::npos;
    }

private:
    std::string title_;
    std::string repo_;
    bool is_read_;
};

GitHubNotification n("Új PR", "user/dependabot-updates");
std::cout << n.is_noise() << std::endl;   // 1 (true)
```

Miért `: title_(title), repo_(repo), is_read_(false)` a konstruktor előtt, nem a törzsben (`title_ = title;`)? Ez az **inicializációs lista**, és fontos különbség van a kettő között: az inicializációs lista **létrehozza (konstruálja)** a mezőket a megadott értékkel *közvetlenül*, míg ha a törzsben írnád (`title_ = title;`), a mezők *előbb* egy alapértelmezett módon jönnének létre, és *utána* íródna felül az érték egy külön értékadással — ez nem mindig egyenértékű (pl. `const` mezőknél vagy referencia-mezőknél *kötelező* az inicializációs lista, mert azokat nem lehet "utólag" értékadással beállítani).

Miért `public`/`private`? A **beágyazás (encapsulation)** elve: a `private` mezőkhöz (`title_`, `repo_`, `is_read_`) kívülről *nem* lehet közvetlenül hozzáférni (`n.title_` fordítási hibát adna), csak a `public` metódusokon keresztül — ez véd attól, hogy valaki kívülről érvénytelen állapotba hozza az objektumot. A `title_` aláhúzás-utótag pusztán konvenció (jelzi: "ez egy tagváltozó"), nem nyelvi kényszer.

**C# összehasonlítás:** ez pontosan a C# `public`/`private` mezőkkel és konstruktorral megegyező elv — a fő szintaktikai különbség az inicializációs lista, aminek C#-ban nincs közvetlen megfelelője (ott a törzsben végzett értékadás az egyetlen mód).

### Getter és setter — C++-ban nincs `@property`, expliciten írjuk meg

```cpp
class Repo {
public:
    Repo(const std::string& name, int stars) : name_(name), stars_(stars) {}

    int get_stars() const {          // GETTER — a "const" jelzi, hogy ez csak OLVAS, nem módosít
        return stars_;
    }

    void set_stars(int value) {      // SETTER — validációval
        if (value < 0) {
            throw std::invalid_argument("A csillagok száma nem lehet negatív");
        }
        stars_ = value;
    }

private:
    std::string name_;
    int stars_;
};

Repo repo("NexusMind", 10);
std::cout << repo.get_stars() << std::endl;   // 10

repo.set_stars(42);
std::cout << repo.get_stars() << std::endl;   // 42

try {
    repo.set_stars(-5);
} catch (const std::invalid_argument& e) {
    std::cout << "Hiba: " << e.what() << std::endl;
}
```

Miért `get_stars()`/`set_stars()` explicit metódusnévvel, nem valami tömörebb szintaxis, mint Pythonban a `@property`? Mert a C++ nyelv nem ad beépített "álcázási" mechanizmust ahhoz, hogy egy metódushívást (`repo.stars()`) mezőelérésnek tűntess (`repo.stars`) — ehelyett a konvenció, hogy explicit `get_`/`set_` előtagú metódusokat írsz. (Van egy haladóbb, ritkábban használt C++ eszköz, az **operator overloading** — pl. az `operator[]` — ami hasonló "álcázást" tesz lehetővé bizonyos esetekre, de teljes property-szintaxis nincs a nyelvben.)

A `const` a `get_stars() const` végén **fordító által kikényszerített ígéret**: azt mondja, "ez a metódus nem módosítja az objektum állapotát" — ha mégis megpróbálnál benne egy mezőt módosítani, a fordító hibát adna. Ez azért fontos, mert egy `const Repo& repo`-n (const referencián) keresztül *csak* a `const`-nak jelölt metódusokat hívhatod meg — ez ugyanaz a garancia-lánc, amit a 7. fejezetben a `const std::vector<int>&` paramétereknél láttál.

### Öröklés és polimorfizmus (virtual függvények)

```cpp
class NotificationSource {
public:
    virtual std::vector<std::string> fetch() const = 0;   // "= 0" -> TISZTA VIRTUÁLIS, ez teszi absztrakttá az osztályt
    virtual ~NotificationSource() = default;   // virtuális destruktor — SZINTE MINDIG kell, ha van virtual metódusod
};

class GitHubSource : public NotificationSource {
public:
    std::vector<std::string> fetch() const override {   // "override" — a fordító ellenőrzi, hogy tényleg felülírunk valamit
        return {"Új issue"};
    }
};

// NotificationSource n;   // FORDÍTÁSI HIBA! Absztrakt osztályból nem lehet példányt létrehozni

NotificationSource* source = new GitHubSource();   // "felfelé castolás" — egy pointer az ŐSRE mutat, de a leszármazott van mögötte
for (const auto& item : source->fetch()) {         // a `virtual` miatt a GitHubSource::fetch() fut le, NEM az ősosztályé!
    std::cout << item << std::endl;
}
delete source;
```

Miért kell a `virtual` kulcsszó, ha a `fetch()`-t úgyis felülírja a `GitHubSource`? Mert a `virtual` nélkül a C++ **statikusan, fordítási időben** dönti el, melyik `fetch()`-et hívja meg — mivel a `source` deklarált típusa `NotificationSource*`, `virtual` nélkül *mindig* az ősosztály (esetünkben nem is létező, hiszen tiszta virtuális) verzióját próbálná hívni, függetlenül attól, milyen tényleges objektum van mögötte. A `virtual` bekapcsolja a **dinamikus kötést (dynamic dispatch)**: a fordító beépít egy futásidejű mechanizmust (egy "virtuális metódustáblát", vtable-t), ami *futás közben* nézi meg, milyen *tényleges* típusú objektumról van szó, és azt a verziót hívja meg — ez teszi lehetővé, hogy egy `NotificationSource*`-on keresztül a valódi, leszármazott viselkedés fusson le.

Az `= 0` a `fetch()` végén **tiszta virtuális függvénnyé (pure virtual)** teszi — ez azt jelenti, hogy a `NotificationSource`-nak *nincs* implementációja ehhez a metódushoz, és emiatt magából a `NotificationSource`-ból *nem lehet* példányt létrehozni (ez a C++ "interfész" fogalmának megfelelője, hasonlóan a Python `@abstractmethod`-jához).

Miért kell a `virtual ~NotificationSource() = default;` (virtuális destruktor)? Ez az egyik legveszélyesebb, könnyen elfelejthető szabály öröklésnél: ha egy ősosztálypéldányra mutató pointeren (`NotificationSource*`) hívod meg a `delete`-et, de a destruktor *nem* `virtual`, akkor **csak az ősosztály destruktora fut le**, a leszármazott (`GitHubSource`) destruktora **kimarad** — ha a leszármazottnak saját, heap-en foglalt erőforrásai lennének, azok memory leak-ké válnának. A `virtual` destruktor ugyanazt a dinamikus kötést biztosítja a felszabadításhoz, mint amit a `fetch()`-nél láttál a hívásokhoz.

### Gyakorló kérdések

- Mi a különbség egy inicializációs lista és a konstruktor törzsében történő értékadás között? *(Az inicializációs lista közvetlenül létrehozza a mezőket a megadott értékkel, nem előbb alapértelmezetten, majd felülírva.)*
- Hogyan valósítasz meg getter/setter-t C++-ban, mivel nincs `@property`? *(Explicit `get_x()`/`set_x()` metódusokkal.)*
- Mire való a `virtual` kulcsszó? *(Futásidejű, dinamikus kötést kapcsol be, hogy egy ősosztály-pointeren keresztül is a tényleges, leszármazott objektum metódusa fusson le.)*
- Miért veszélyes elfelejteni a `virtual` destruktort öröklésnél? *(Mert egy ősosztály-pointeren hívott `delete` ilyenkor kihagyja a leszármazott destruktorát, memory leaket okozva.)*

---

<a id="fejezet-13"></a>

## 13. Fordítási modell: header fájlok, namespace, linkelés

### Header (.h/.hpp) és forrásfájl (.cpp) szétválasztása

```cpp
// repo.h  — a DEKLARÁCIÓ, "mi létezik", ezt osztod meg más fájlokkal
#pragma once   // "include guard" — megakadályozza, hogy ez a fájl KÉTSZER kerüljön beillesztésre

class Repo {
public:
    Repo(const std::string& name);
    int get_stars() const;
private:
    std::string name_;
    int stars_ = 0;
};
```

```cpp
// repo.cpp — a DEFINÍCIÓ, "hogyan működik", ez csak egyszer, itt van megírva
#include "repo.h"

Repo::Repo(const std::string& name) : name_(name) {}
int Repo::get_stars() const { return stars_; }
```

```cpp
// main.cpp
#include "repo.h"   // csak a DEKLARÁCIÓT látja, a megvalósítást a linker fűzi hozzá később

int main() {
    Repo r("NexusMind");
    std::cout << r.get_stars() << std::endl;   // 0
    return 0;
}
```

Miért kell ez a szétválasztás, ha Pythonban egyetlen `.py` fájlban minden együtt lehet? Mert a C++ fordítás **fordítási egységenként** (egy `.cpp` fájl + amit `#include`-dal belemásolt) történik, *egymástól függetlenül* — a `main.cpp` fordításakor a fordítónak *nem kell* látnia a `Repo::get_stars()` tényleges kódját, elég tudnia, hogy *létezik* egy ilyen metódus, milyen paraméterekkel és visszatérési típussal (ez van a header-ben) — a tényleges kódot majd a **linker** fűzi hozzá, a `repo.cpp`-ből lefordított objektumfájlból. Ez teszi lehetővé, hogy egy nagy projekt minden `.cpp` fájlja *párhuzamosan, egymástól függetlenül* fordítható legyen, és hogy ha csak a `repo.cpp` implementációja változik (a header nem), ne kelljen az egész projektet újrafordítani, csak azt az egy fájlt és újralinkelni.

Az `#pragma once` (vagy a régebbi, hordozhatóbb `#ifndef HEADER_H / #define HEADER_H / #endif` minta) azért kell, mert ha ugyanaz a header véletlenül *kétszer* kerülne beillesztésre ugyanabba a fordítási egységbe (pl. mert két másik header is `#include`-olja), a `class Repo` deklarációja duplán jelenne meg, ami fordítási hibát okozna ("redefinition") — az include guard ezt előzi meg, azzal, hogy a második beillesztési kísérletet a preprocesszor egyszerűen figyelmen kívül hagyja.

### Namespace — a C++ "csomagolási" eszköze

```cpp
namespace nexusmind {
    class Repo {
    public:
        std::string name;
    };
}

nexusmind::Repo r;   // a "::" jelzi, melyik namespace-ből jön a Repo
r.name = "NexusMind";
```

A `namespace` célja pontosan az, mint a Python modulok vagy a C# `namespace X { }` blokkjának: elkerülni a névütközéseket nagy projektekben vagy külső könyvtárak bevonásakor — ha két különböző könyvtárban is van egy `Repo` osztály, a `namespace`-ekkel egyértelműen meg tudod különböztetni őket (`nexusmind::Repo` vs `github_sdk::Repo`), anélkül hogy át kellene nevezned bármelyiket.

**C# összehasonlítás:** ez fogalmilag közel azonos a C# `namespace`-ekkel, csak a C++ `::`-t használ a Python/C# `.` helyett a névtér-hozzáféréshez, és — ellentétben Pythonnal, ahol minden `.py` fájl automatikusan saját modul — C++-ban a namespace-t explicit deklarálnod kell.

### Gyakorló kérdések

- Miért van szükség header és forrásfájl szétválasztására C++-ban? *(Mert a fordítás fordítási egységenként történik, és a fordítónak elég a deklaráció, hogy egy másik fájlból hívott függvényt fordítani tudjon — a tényleges kódot a linker fűzi hozzá.)*
- Mire való az `#pragma once` (include guard)? *(Megakadályozza, hogy egy header véletlenül kétszer kerüljön beillesztésre ugyanabba a fordítási egységbe.)*
- Mire való a `namespace`? *(Névütközések elkerülésére nagy projektekben, hasonlóan a C# `namespace`-hez vagy a Python modulokhoz.)*

---

<a id="fejezet-14"></a>

## 14. Template-ek (generikus programozás)

```cpp
template <typename T>
T maximum(T a, T b) {
    return (a > b) ? a : b;
}

std::cout << maximum(3, 7) << std::endl;         // 7 — a fordító T=int-ként generálja le
std::cout << maximum(3.5, 2.1) << std::endl;     // 3.5 — a fordító T=double-ként EGY MÁSIK verziót generál
std::cout << maximum(std::string("alma"), std::string("banán")) << std::endl;   // "banán" — T=std::string
```

Ez a C++ válasza arra, amit Pythonban "magától" kapsz (egy függvény bármilyen típussal meghívható, mert Python futásidőben, dinamikusan dönt), és amit C#-ban a generikusok (`<T>`) oldanak meg. A `template <typename T>` azt mondja a fordítónak: "ne fordítsd le még ezt a függvényt — várd meg, amíg valaki *ténylegesen* meghívja egy konkrét típussal, és *akkor*, fordítási időben, generálj egy külön, arra a típusra szabott verziót". Ez azt jelenti, hogy a fenti három `maximum()`-hívás mögött **három teljesen külön, lefordított függvény** rejlik a végleges programban (`maximum<int>`, `maximum<double>`, `maximum<std::string>`), nem egy közös, futásidőben "kitaláló" mechanizmus — ez az oka, hogy a C++ template-ek nem lassítják a futásidőt (a döntés fordítási időben, egyszer megtörténik), szemben Python dinamikus típuskezelésével, ami minden egyes híváskor újra "kitalálja" a típust.

### Osztály-template

```cpp
template <typename T>
class Box {
public:
    Box(T value) : value_(value) {}
    T get() const { return value_; }
private:
    T value_;
};

Box<int> int_box(42);
Box<std::string> string_box("Szia");

std::cout << int_box.get() << std::endl;      // 42
std::cout << string_box.get() << std::endl;   // Szia
```

Ez pontosan az a mechanizmus, ami mögött a `std::vector<int>` vagy a `std::map<std::string, int>` áll — a `std::vector` maga is egy osztály-template, és amikor `<int>`-et írsz utána, a fordító legenerálja neked a kifejezetten `int`-ekre szabott verziót.

### Gyakorló kérdések

- Mikor generálja le a fordító egy template-függvény konkrét változatát? *(Amikor valaki ténylegesen meghívja egy konkrét típussal — fordítási időben, hívásonként/típusonként egyszer.)*
- Lassabbak-e a template-ek futásidőben, mint a Python dinamikus típuskezelése? *(Nem — a döntés fordítási időben történik, futásidőben nincs extra "kitalálás".)*

---

<a id="fejezet-15"></a>

## 15. Move szemantika (modern C++)

Ez az egyik legmodernebb (C++11-től elérhető) és leginkább C++-specifikus fogalom — sem Pythonban, sem C#-ban nincs ehhez fogható, explicit nyelvi eszköz.

### A probléma: felesleges másolás

```cpp
#include <vector>

std::vector<int> create_big_vector() {
    std::vector<int> result(1000000, 42);   // 1 millió elemű vektor
    return result;   // ⚠️ enélkül a move szemantika nélkül ez egy TELJES MÁSOLATOT jelentene!
}

std::vector<int> v = create_big_vector();
```

Régen (C++11 előtt), amikor egy függvény egy nagy objektumot adott vissza, a fordító *kénytelen* volt egy teljes másolatot készíteni, hogy a hívó megkapja az eredményt — 1 millió elem esetén ez 1 millió elem tényleges, elem-elemenkénti lemásolását jelentette, feleslegesen, hiszen a `result` úgyis megszűnne a függvény végén.

### A megoldás: rvalue referencia és `std::move`

```cpp
class Buffer {
public:
    Buffer(size_t size) : size_(size), data_(new int[size]) {
        std::cout << "Konstruktor: " << size_ << " elem lefoglalva" << std::endl;
    }

    // MOVE KONSTRUKTOR — "ELLOPJA" a másik objektum belső adatát, nem másolja
    Buffer(Buffer&& other) noexcept : size_(other.size_), data_(other.data_) {
        other.data_ = nullptr;   // a FORRÁS objektumot "kiürítjük", hogy ne szabadítsa fel ugyanazt a memóriát
        other.size_ = 0;
        std::cout << "Move konstruktor: ellopva, nincs másolás" << std::endl;
    }

    // MÁSOLÓ KONSTRUKTOR — ténylegesen új memóriát foglal és átmásolja a tartalmat
    Buffer(const Buffer& other) : size_(other.size_), data_(new int[other.size_]) {
        std::copy(other.data_, other.data_ + size_, data_);
        std::cout << "Másoló konstruktor: teljes másolás történt" << std::endl;
    }

    ~Buffer() { delete[] data_; }

private:
    size_t size_;
    int* data_;
};

Buffer a(1000000);
Buffer b = a;                    // MÁSOLÓ konstruktor fut — "a" még kell nekünk, ezért nem "lophatunk" belőle
Buffer c = std::move(a);         // MOVE konstruktor fut — jelezzük: "a"-t innentől nem használjuk, ellophatod a tartalmát
```

Kimenet:

```
Konstruktor: 1000000 elem lefoglalva
Másoló konstruktor: teljes másolás történt
Move konstruktor: ellopva, nincs másolás
```

Mi történik pontosan a `std::move(a)` sorban? A `std::move` **maga semmit nem mozgat** — ez csak egy jelzés (technikailag: `a`-t egy "rvalue-referenciává" alakítja), ami azt üzeni a fordítónak: "ezt az objektumot innentől nem fogom többé eredeti formájában használni, szabad belőle 'ellopni' az erőforrásokat". Emiatt a `Buffer c = std::move(a);` sor a **move konstruktort** hívja meg (nem a másolót) — ez a konstruktor nem foglal új memóriát és nem másolja át elemenként az adatot, hanem egyszerűen **átveszi a nyers pointert** (`data_`) `a`-tól `c`-hez, majd `a`-ban ezt a pointert `nullptr`-re állítja, hogy amikor `a` később megszűnik és lefut a destruktora, *ne* szabadítsa fel azt a memóriát, amit `c` már "átvett".

Miért fontos ez? Mert így egy nagy objektum (mint a `create_big_vector()` elején látott 1 milliós vektor) "visszaadása" egy függvényből **nem jár másolással** — csak a belső pointer(ek) cserélődnek át egyik objektumból a másikba, ami néhány gépi utasítás, függetlenül attól, hogy 10 elemről vagy 10 millió elemről van szó.

**C# összehasonlítás:** C#-ban ez a probléma jórészt fel sem merül referenciatípusoknál (osztályoknál), mert ott a "visszaadás" eleve csak egy referencia (memóriacím) átadása, sosem tényleges másolás — a C++ "érték szerinti" alapfilozófiája miatt (ahol egy `Buffer b = a;` alapból *másolatot* jelentene) volt szükség a move szemantikára, hogy megkapd a C#-ban "ingyen" járó hatékonyságot azokban az esetekben, ahol tényleg nincs szükség másolatra.

### Gyakorló kérdések

- Mit csinál ténylegesen a `std::move()`? *(Semmit nem mozgat fizikailag — csak jelzi a fordítónak, hogy az objektum erőforrásai "ellophatók", ez váltja ki a move konstruktor hívását.)*
- Miért gyorsabb a move konstruktor a másoló konstruktornál nagy objektumoknál? *(Mert csak a belső pointert veszi át, nem másolja el elemenként a teljes tartalmat.)*

---

<a id="fejezet-16"></a>

## 16. Gyakorlati integráció: fájlfeldolgozás a nulláról

Ez a fejezet összeköti a fenti fejezeteket: fájl-I/O → saját osztály (RAII-val) → algoritmus.

```cpp
#include <fstream>
#include <vector>
#include <string>
#include <sstream>
#include <numeric>

struct Issue {
    std::string title;
    int number;
};

std::vector<Issue> load_issues(const std::string& filename) {
    std::vector<Issue> issues;
    std::ifstream file(filename);   // RAII: a fájl AUTOMATIKUSAN bezáródik, amikor "file" kimegy a hatóköréből

    if (!file.is_open()) {
        throw std::runtime_error("Nem sikerült megnyitni: " + filename);
    }

    std::string line;
    while (std::getline(file, line)) {
        std::istringstream iss(line);
        std::string title;
        int number;
        if (std::getline(iss, title, ',') && (iss >> number)) {
            issues.push_back({title, number});
        }
    }

    return issues;   // "file" itt automatikusan bezáródik, NINCS explicit file.close() sehol!
}

int count_bugs(const std::vector<Issue>& issues) {
    return std::count_if(issues.begin(), issues.end(), [](const Issue& issue) {
        return issue.title.find("bug") != std::string::npos;
    });
}

// feltételezve egy "issues.csv" fájlt "bug login,1\nfeature request,2\nbug crash,3" tartalommal
auto issues = load_issues("issues.csv");
std::cout << "Talált issue-k: " << issues.size() << std::endl;
std::cout << "Ebből 'bug' a címben: " << count_bugs(issues) << std::endl;
```

Miért nincs sehol `file.close()`? Ez a **RAII elv** (11. fejezet) gyakorlati, mindennapos alkalmazása: a `std::ifstream` egy olyan osztály, aminek a destruktora *automatikusan* bezárja a fájlt, amikor az objektum kimegy a hatóköréből (a `load_issues` függvény végén) — akkor is, ha közben kivétel repült volna. Ez ugyanaz a garancia, mint amit a Python `with open(...) as f:` blokkja ad neked explicit szintaxissal — C++-ban ezt *nem* kell külön szintaxissal jelezned, mert *minden* lokális objektum automatikusan így viselkedik.

### Gyakorló kérdések

- Miért nincs szükség explicit `file.close()` hívásra? *(A RAII elv miatt — a fájlobjektum destruktora automatikusan lefut és bezárja a fájlt, amikor kimegy a hatóköréből.)*

---

<a id="fejezet-17"></a>

## 17. Automatizált tesztelés: Google Test

A **Google Test (gtest)** a C++ egyik legelterjedtebb tesztelési keretrendszere — nagyjából azt a szerepet tölti be, mint az xUnit C#-ban vagy a `pytest` Pythonban.

```cpp
#include <gtest/gtest.h>
#include "algorithms.h"

TEST(AlgorithmsTest, SumManualBasic) {
    std::vector<int> numbers = {1, 2, 3};
    EXPECT_EQ(sum_manual(numbers), 6);
}

TEST(AlgorithmsTest, CountEvens) {
    std::vector<int> numbers = {1, 2, 3, 4};
    EXPECT_EQ(count_evens_manual(numbers), 2);
}

TEST(AlgorithmsTest, LinearSearchFound) {
    std::vector<int> numbers = {10, 20, 30};
    EXPECT_EQ(linear_search(numbers, 20), 1);
}

TEST(AlgorithmsTest, LinearSearchNotFound) {
    std::vector<int> numbers = {10, 20, 30};
    EXPECT_EQ(linear_search(numbers, 99), -1);
}
```

Futtatás (CMake-integrációval): a build után a legenerált teszt-futtatható elindítja az összes `TEST(...)`-et, és riportot ad (`[  PASSED  ] 4 tests.`) — ugyanaz az élmény, mint a `pytest`-nél vagy az xUnit-nál.

**C# összehasonlítás:**

| xUnit (C#) | pytest (Python) | Google Test (C++) |
|---|---|---|
| `[Fact]` | `test_` előtag | `TEST(CsoportNév, TesztNév)` makró |
| `Assert.Equal(a, b)` | `assert a == b` | `EXPECT_EQ(a, b)` |
| `[Theory]` + `[InlineData]` | `@pytest.mark.parametrize` | `TEST_P` + `INSTANTIATE_TEST_SUITE_P` |
| Moq | `unittest.mock` | Google Mock (gmock), ugyanabból a projektből |

Miért `EXPECT_EQ(a, b)` és nem egyszerű `assert a == b`, mint Pythonban? Mert a C++-nak nincs olyan futásidejű introspekciós mechanizmusa, mint a Python `assert`-jének (ami a bukás pillanatában vissza tudja fejteni, mik voltak a konkrét értékek) — a `EXPECT_EQ` makró ezt *explicit* megoldja: mindkét oldalt átveszi külön argumentumként, és ha nem egyeznek, ő maga formázza meg a hibaüzenetet a konkrét értékekkel. Az `EXPECT_EQ` (a teszt folytatódik hiba után is) és az `ASSERT_EQ` (a teszt azonnal megszakad hiba esetén) között is választhatsz, a hiba súlyosságától függően — ennek sincs közvetlen megfelelője a Python `assert`-nél.

### Gyakorló kérdések

- Mi a Google Test megfelelője a Python `assert a == b`-jének? *(`EXPECT_EQ(a, b)` vagy `ASSERT_EQ(a, b)`.)*
- Mi a különbség `EXPECT_EQ` és `ASSERT_EQ` között? *(Az `EXPECT_EQ` hiba esetén is folytatja a tesztet, az `ASSERT_EQ` azonnal megszakítja.)*

---

<a id="fejezet-18"></a>

## 18. Megfelelési táblázat a hivatalos tematikához

| Hivatalos tematika eleme | Fejezet |
|---|---|
| Forráskód, fordítás, futtatás | 1. |
| IDE megismerése | 1. |
| Konstansok, változók | 2. |
| Típusok, operátorok | 2. |
| Standard I/O | 5. |
| Vezérlési szerkezetek | 3. |
| Sztringkezelés | 4. |
| Alprogramok (függvények) | 7. |
| Tárbeli adatszerkezetek | 6. |
| Algoritmusleíró eszköz (pszeudokód) | 8. |
| Alapvető algoritmusok | 8. |

A 10–17. fejezetek túlmutatnak egy bevezető kurzuson — ezek adják a C++-t ténylegesen megkülönböztető, saját fogalmi világot: mutatók, kézi/automatikus memóriakezelés, fordítási modell, template-ek, move szemantika.

---

## Hogyan használd ezt a könyvet

1. Olvasd át 1–9. fejezetet — ez nagyrészt ismerős lesz C#-ból, csak a szintaxis és a statikus fordítási modell az újdonság.
2. **10–11. fejezet (mutatók, memóriakezelés) — ne siess át rajta.** Ez a C++ valódi lelke, és minden későbbi fogalom (referenciák a függvényeknél, smart pointerek az OOP-nál, move szemantika) erre épül.
3. 12–14. fejezet, amikor OOP-s, típusosított kódot kezdesz írni.
4. 15–17. fejezet, amikor már ténylegesen teljesítménykritikus vagy tesztelt C++ kódot építesz.

Ha egy konkrét hibaüzenetnél (pl. `segmentation fault`, "no matching function", linker error) elakadsz, hozd ide a pontos üzenetet — a C++ hibaüzenetei elsőre ijesztően hosszúak tudnak lenni, de szinte mindig van bennük egy konkrét, megtalálható ok.
