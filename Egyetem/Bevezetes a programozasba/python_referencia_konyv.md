---
python_ch1: false
python_ch2: false
python_ch3: false
python_ch4: false
python_ch5: false
python_ch6: false
python_ch7: false
python_ch8: false
python_ch9: false
python_ch10: false
python_ch11: false
python_ch12: false
python_ch13: false
python_ch14: false
python_ch15: false
python_ch16: false
tags: [python, nexusmind, egyetem, flashcards]
---

# Python referenciakönyv — a nulláról a NexusMind-ig

Ez nem naphoz kötött terv, hanem egy **fejezetenkénti kézikönyv**, amit saját tempódban járhatsz végig, előre-hátra lapozhatsz benne, és később is visszakereshetsz belőle. Két dolgot fed le egyszerre:

1. A **"Bevezetés a programozásba"** hivatalos tematikáját (alapfogalmak → algoritmikus gondolkodás).
2. Azt a **haladó réteget**, amire a NexusMind projekt Python-moduljához (gRPC, Pydantic, async, AI API-hívások) szükséged lesz.

Minden fejezetnél ott vannak a **C# összehasonlítások**, mert ez a leggyorsabb út, hogy ne a nulláról tanulj, hanem a meglévő programozói tudásodra építs. Kódrészletek mindenhol futtathatók — másold be PyCharm-ba, próbáld ki.

> **Ez a fájl Obsidian-hoz van igazítva**, a nálad telepített pluginokkal: a pipálható négyzetek a **Meta Bind** pluginnal vannak a jegyzet frontmatter-jéhez kötve (tehát a haladásod ténylegesen elmentődik, nem csak vizuális), a link-navigáció Obsidian belső `[[#fejezet]]` hivatkozásokat használ (ez minden Obsidian-verzióban működik, ellentétben a sima markdown-horgonyokkal), a haladás-összesítő pedig **Dataview** (DataviewJS) segítségével számolódik ki automatikusan.
>
> **Egyszeri beállítás:** a Dataview beépülő beállításaiban (Settings → Community plugins → Dataview) kapcsold be az **"Enable JavaScript Queries"** opciót, különben a lenti haladás-kijelző nem fog lefutni (ez alapból biztonsági okból ki van kapcsolva).
>
> **Flashcardok:** minden fejezet végén található egy `🗂️ Flashcardok` blokk, `Kérdés::Válasz` formátumban — ezeket a **Spaced Repetition** plugin automatikusan felismeri. A bal oldali ikonsávban a kártya-ikonra kattintva (vagy a parancspalettából: "Spaced Repetition: Review flashcards") indíthatod az áttekintést; a plugin optimalizált időzítéssel fogja újra visszakérdezni őket.

---

## Tartalomjegyzék és haladáskövetés

- `INPUT[toggle:python_ch1]` [[#1. Mi a Python, és hogyan fut le egy program|1. Mi a Python, és hogyan fut le egy program]]
- `INPUT[toggle:python_ch2]` [[#2. Alaptípusok, változók, operátorok|2. Alaptípusok, változók, operátorok]]
- `INPUT[toggle:python_ch3]` [[#3. Vezérlési szerkezetek|3. Vezérlési szerkezetek]]
- `INPUT[toggle:python_ch4]` [[#4. Sztringkezelés|4. Sztringkezelés]]
- `INPUT[toggle:python_ch5]` [[#5. Standard I/O|5. Standard I/O]]
- `INPUT[toggle:python_ch6]` [[#6. Adatszerkezetek: list, tuple, dict, set|6. Adatszerkezetek: list, tuple, dict, set]]
- `INPUT[toggle:python_ch7]` [[#7. Függvények (alprogramok)|7. Függvények (alprogramok)]]
- `INPUT[toggle:python_ch8]` [[#8. Algoritmikus gondolkodás és alapalgoritmusok|8. Algoritmikus gondolkodás és alapalgoritmusok]]
- `INPUT[toggle:python_ch9]` [[#9. Hibakezelés|9. Hibakezelés]]
- `INPUT[toggle:python_ch10]` [[#10. Objektumorientált programozás|10. Objektumorientált programozás]]
- `INPUT[toggle:python_ch11]` [[#11. Modulok és csomagkezelés|11. Modulok és csomagkezelés]]
- `INPUT[toggle:python_ch12]` [[#12. Típusosítás és Pydantic|12. Típusosítás és Pydantic]]
- `INPUT[toggle:python_ch13]` [[#13. Async/await|13. Async/await]]
- `INPUT[toggle:python_ch14]` [[#14. Gyakorlati integráció: valós API-hívás a nulláról|14. Gyakorlati integráció: valós API-hívás a nulláról]]
- `INPUT[toggle:python_ch15]` [[#15. Automatizált tesztelés: pytest|15. Automatizált tesztelés: pytest]]
- `INPUT[toggle:python_ch16]` [[#16. Megfelelési táblázat a hivatalos tematikához|16. Megfelelési táblázat a hivatalos tematikához]]

```dataviewjs
const p = dv.current();
const total = 16;
let done = 0;
for (let i = 1; i <= total; i++) {
    if (p[`python_ch${i}`]) done++;
}
const percent = Math.round((done / total) * 100);
const barLength = 20;
const filled = Math.round((percent / 100) * barLength);
const bar = "█".repeat(filled) + "░".repeat(barLength - filled);
dv.paragraph(`**Haladás:** ${bar} ${done}/${total} fejezet (${percent}%)`);
```

---

## Ajánlott további Obsidian pluginok a tanuláshoz

A meglévő pluginjaid (Dataview, Git, JS Engine, Meta Bind, Minimal Theme Settings, Omnisearch, Templater) mellett ezek segíthetnek konkrétan a matek/logika/programozás tárgyaknál — mindegyik a Community Plugins böngészőből telepíthető (Settings → Community plugins → Browse):

| Plugin | Mire jó | Miért releváns neked |
|---|---|---|
| **Execute Code** | Kódblokkok **közvetlen futtatása** a jegyzeten belül (Python, C, C++, C# is támogatott, ha a megfelelő fordító/interpreter telepítve van a gépeden) | Ez a legnagyobb nyerés — a referenciakönyv Python-kódrészleteit **közvetlenül Obsidian-ban futtathatod**, nem kell PyCharm-ba átmásolni mindent. Az OS-tárgy C-kódjaihoz és a leendő C++-hoz is használható. |
| **LaTeX Suite** | Gyors LaTeX-képletbeírás rövidítésekkel/snippetekkel (Obsidian natívan is tud `$...$` LaTeX-et, ez csak felgyorsítja a gépelést) | Közvetlenül hasznos a **Kalkulus** és **Diszkrét matematika** tárgyaidhoz — matematikai jegyzeteket sokkal gyorsabban tudsz írni. |
| **Excalidraw** | Kézzel rajzolható vázlatok, folyamatábrák, diagramok a jegyzeten belül | Ideális az **algoritmusleíró eszköz** (pszeudokód/folyamatábra, 8. fejezet) gyakorlásához, és az OS-tárgy folyamatkezelés/memória-diagramjaihoz is. |
| **Spaced Repetition** | Kártya-alapú (flashcard) ismétlő rendszer, ami a jegyzeteidből generál kérdés-válasz párokat, és optimalizált időzítéssel kérdez rá újra | Jó a szintaxis/fogalmak (pl. a fenti C# vs Python táblázatok) hosszútávú megjegyzéséhez — nem csak egyszer olvasod el, hanem rendszeresen visszakérdezi. |
| **Advanced Tables** | Markdown-táblázatok kényelmes szerkesztése (Tab-bal ugrás cellák közt, automatikus formázás) | Ebben a dokumentumban rengeteg összehasonlító táblázat van — ez megkönnyíti, ha te magad is bővítenéd őket. |
| **Kanban** | Tábla-nézet (oszlopok: Tervezett / Folyamatban / Kész) | Hasznos lehet a NexusMind projekt **fázisainak/mérföldköveinek** követésére, amikor odaérünk a teljes specifikációhoz. |

A már meglévő pluginjaid ebben a dokumentumban: a **Meta Bind** adja a fenti pipálható checkboxokat, a **Dataview** a haladás-kijelzőt, a **Templater**-hez lent találsz egy külön gyakorló-sablont, az **Omnisearch** pedig automatikusan jó szolgálatot tesz, ha a `#python` taggel ellátott jegyzeteid közt keresnél. A frontmatterben lévő `tags: [python, nexusmind, egyetem, flashcards]` mind ezt, mind a Spaced Repetition-t támogatja — utóbbi alapból csak a `flashcards` taggel ellátott jegyzeteket vizsgálja.

---

## 1. Mi a Python, és hogyan fut le egy program

A Python **interpretált** nyelv — pontosabban: a forráskódot (`.py` fájl) a Python futtatókörnyezet előbb **bájtkóddá fordítja** (ezt automatikusan, láthatatlanul csinálja, `.pyc` fájlokba cache-elve), majd ezt a bájtkódot a **Python Virtuális Gép (PVM)** értelmezi sorról sorra, futás közben. Ez a "köztes" modell:

- **Nem igényel külön fordítási lépést** tőled (ellentétben C#-szal, ahol `dotnet build` kell a futtatás előtt) — írod a kódot, futtatod, kész.
- **Szintaktikai hibákat** már a bájtkóddá fordításkor jelez, futtatás előtt.
- **Típus- és logikai hibákat** viszont csak **futás közben**, amikor a hibás sor ténylegesen lefut — ez a fő különbség a C#-hoz képest, ahol a fordító sok ilyet előre elkap.

**C# összehasonlítás:** C#-ban `dotnet build` → IL kód → JIT fordítás futáskor natív gépi kódra. Pythonban: `.py` → bájtkód (automatikusan) → PVM értelmezi. Egyik sem "gépi kódra fordít" közvetlenül — mindkettő egy köztes réteget használ, csak más a részletezettség.

### Hogyan futtass egy Python programot

```bash
python3 fajlnev.py
```

PyCharm-ban ez a zöld "Run" gombbal megy, és a debugger (töréspont, `F8`/`F9` léptetés) pontosan úgy működik, mint amit Rider-ből vagy Visual Studio-ból ismersz.

### Az első program

```python
print("Szia, NexusMind!")
```

Nincs `Main` metódus, nincs osztály-kényszer, nincs `;` — egy sima szkript-fájl teteje bármilyen kód lehet, ami közvetlenül lefut, amikor a fájlt futtatod.

#### 🗂️ Flashcardok (Spaced Repetition)

Mi történik a Python forráskóddal, mielőtt lefut?::Bájtkóddá fordul, amit a Python Virtuális Gép (PVM) értelmez sorról sorra.
Mikor derül ki egy Python programban egy típushiba?::Csak futás közben, amikor a hibás sor ténylegesen lefut.
Melyik parancs futtat egy Python fájlt a terminálból?::`python3 fajlnev.py`

---

## 2. Alaptípusok, változók, operátorok

### Változók és típusok

Python **dinamikusan típusos**: a változóhoz nem típust rendelsz, hanem egy értéket — a típus az értékből következik, és futás közben változhat.

```python
x = 5           # int
x = "szöveg"    # most már str — ez teljesen legális
```

**C# összehasonlítás:** C#-ban `int x = 5;` — a típus a változóhoz van kötve, örökre. Pythonban a típus az *értékhez* tartozik, nem a változóhoz. Ez kényelmes, de felelősséget is jelent — neked kell fejben tartanod, mi micsoda (vagy type hinteket használnod, lásd 12. fejezet).

Alaptípusok: `int`, `float`, `str`, `bool`, `None` (ez a C# `null` megfelelője, de kis kezdőbetű helyett nagybetűvel írjuk: `None`).

### Konstansok

Pythonnak **nincs valódi konstans-kulcsszava** (nincs `const`, mint C#-ban). Konvencióból csupa nagybetűvel jelöljük, ami "ne módosítsd":

```python
MAX_RETRY_COUNT = 5   # ez csak megállapodás, a nyelv nem kényszeríti ki
```

### Operátorok

| Kategória | Python | C# |
|---|---|---|
| Aritmetikai | `+ - * / // % **` | `+ - * / % Math.Pow` |
| Egész osztás | `//` (pl. `7 // 2 == 3`) | nincs külön jel, `int/int` automatikusan egész |
| Hatványozás | `**` (pl. `2 ** 10`) | `Math.Pow(2, 10)` |
| Logikai | `and`, `or`, `not` | `&&`, `\|\|`, `!` |
| Összehasonlító | `== != < > <= >=` | ugyanaz |
| Értékadó | `= += -= *= /=` | ugyanaz |

Fontos csapda: Python-ban `and`/`or`/`not` **szavak**, nem szimbólumok — ha `&&`-t írsz, hibát kapsz.

**Gyakorlat:** írj egy kifejezést, ami eldönti, hogy egy évszám szökőév-e (`year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)`).

#### 🗂️ Flashcardok (Spaced Repetition)

Hogyan viselkedik egy Python változó típusa futás közben?::Dinamikusan változhat, mert a típus az értékhez, nem a változóhoz tartozik.
Van-e Pythonnak valódi `const` kulcsszava?::Nincs, csak konvenció (csupa nagybetűs névvel jelöljük).
Mi az eredménye a `7 // 2` kifejezésnek?::3 (egész osztás)
Mivel helyettesítjük Pythonban a `&&` és `||` operátorokat?::Az `and` és `or` szavakkal.

---

## 3. Vezérlési szerkezetek

### if / elif / else

```python
temp = 22
if temp < 0:
    print("Fagypont alatt")
elif temp < 20:
    print("Hűvös")
else:
    print("Kellemes")
```

Nincs `{}` — a **behúzás (indentation)** határozza meg a blokkot. Ez a leggyakoribb kezdő hiba C#-ról jőve: a behúzásnak **konzisztensnek** kell lennie (PyCharm automatikusan 4 szóközt használ, ne keverd tabbal).

### for ciklus

```python
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for item in ["a", "b", "c"]:   # közvetlenül a lista elemein iterálsz
    print(item)
```

**C# összehasonlítás:** ez leginkább a C# `foreach`-nek felel meg. A klasszikus `for (int i=0; i<n; i++)` mintát Pythonban a `range()`-dzsel utánozod: `for i in range(n):`.

### while ciklus

```python
count = 0
while count < 5:
    print(count)
    count += 1   # nincs count++ Python-ban!
```

⚠️ **Nincs `++` és `--` operátor Pythonban** — mindig `+= 1` / `-= 1` kell.

### break, continue

Ugyanúgy működnek, mint C#-ban.

#### 🗂️ Flashcardok (Spaced Repetition)

Mi határozza meg egy Python kódblokk határait `{}` helyett?::A behúzás (indentation).
Van-e Pythonban `++` operátor?::Nincs, mindig `+= 1`-et kell írni.
Melyik Python ciklus felel meg leginkább a C# `foreach`-nek?::A `for item in lista:` szerkezet.

---

## 4. Sztringkezelés

A `str` Python-ban **immutable** (megváltoztathatatlan) — minden "módosító" metódus valójában egy **új** stringet ad vissza.

```python
s = "  Szia NexusMind  "
s.strip()          # "Szia NexusMind" — levágja a whitespace-t
s.lower()          # kisbetűsít
s.upper()          # nagybetűsít
s.replace("Szia", "Hello")
s.split(" ")       # ["", "", "Szia", "NexusMind", "", ""] — lista lesz belőle
"-".join(["a", "b", "c"])   # "a-b-c" — lista összefűzése
s.startswith("  Sz")
"NexusMind" in s   # True — tartalmazás-vizsgálat az `in`-nel
```

### f-string (formázott string) — ezt fogod leggyakrabban használni

```python
name = "NexusMind"
version = 1
print(f"Üdv a {name} v{version}-ben!")
print(f"Pi kb. {3.14159:.2f}")   # 2 tizedesjegyre kerekítve: 3.14
```

**C# összehasonlítás:** ez pontosan a C# `$"Üdv a {name}"` interpolált stringjének felel meg — ha ezt már ismered C#-ból, itt semmi új nincs, csak `f` betű `$` helyett.

### Szeletelés (slicing) — string és lista is "sorozat" típus

```python
s = "NexusMind"
s[0]        # "N"
s[0:5]      # "Nexus"
s[-4:]      # "Mind" — negatív index a végétől számol
s[::-1]     # "dniMsuxeN" — megfordítja a stringet
```

**Gyakorlat:** írj egy függvényt, ami egy mondatból (pl. egy GitHub commit-üzenetből) kiszedi a szavakat (`.split()`), megszámolja őket, és visszaadja a leghosszabb szót.

#### 🗂️ Flashcardok (Spaced Repetition)

Módosítható-e egy Python string a helyén (mutable)?::Nem, a `str` immutable — minden módosító metódus új stringet ad vissza.
Mi az f-string szintaxisa?::`f"szöveg {változó}"`
Mit ad vissza az `s[::-1]`?::A string megfordítva.

---

## 5. Standard I/O

### Kimenet

```python
print("Egy sor")
print("a", "b", "c")              # "a b c" — szóközzel elválasztva
print("a", "b", sep="-")          # "a-b"
print("Nincs sortörés", end="")   # nem ugrik új sorba
```

### Bemenet

```python
name = input("Add meg a neved: ")   # MINDIG stringet ad vissza!
age = int(input("Add meg az életkorod: "))  # explicit konvertálni kell
```

⚠️ **Fontos csapda:** az `input()` mindig `str`-t ad vissza, akkor is, ha számot gépelsz be — neked kell explicit konvertálnod (`int()`, `float()`).

**C# összehasonlítás:** ez a `Console.ReadLine()` megfelelője — ott is stringet kapsz vissza, és neked kell `int.Parse()`-olnod. Ebben a két nyelv szinte azonos.

#### 🗂️ Flashcardok (Spaced Repetition)

Milyen típust ad vissza mindig az `input()`?::Mindig `str`-t, még számbevitel esetén is.
Hogyan függeszted fel a sortörést a `print()`-nél?::Az `end=""` paraméterrel.

---

## 6. Adatszerkezetek: list, tuple, dict, set

### list — dinamikus tömb (ez a "munkaló" adatszerkezeted)

```python
numbers = [1, 2, 3, 4, 5]
numbers.append(6)          # hozzáadás a végéhez
numbers.insert(0, 0)       # beszúrás adott pozícióra
numbers.remove(3)          # érték alapú törlés
numbers.pop()              # utolsó elem eltávolítása és visszaadása
len(numbers)                # hossz
3 in numbers                # tartalmazás-vizsgálat
```

⚠️ **Fontos felismerés (OS-tárgyhoz is kapcsolódik):** a Python `list` **nem láncolt lista**, hanem — akárcsak a C# `List<T>` — egy dinamikusan növekvő **tömb** a háttérben. A klasszikus, mutatókkal összekötött láncolt lista Pythonban nincs beépítve; ha ennek elvét szeretnéd látni, a `collections.deque` áll hozzá legközelebb (dupla láncolt lista, hatékony elején/végén beszúrásra).

### List comprehension — Python egyik legjellemzőbb idiómája

```python
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
```

**C# összehasonlítás:** ez nagyjából a LINQ `.Select()`/`.Where()` párosának felel meg, csak tömörebb szintaxissal: `numbers.Select(x => x*2).Where(x => x > 5)` ≈ `[x*2 for x in numbers if x*2 > 5]`.

### tuple — megváltoztathatatlan sorozat

```python
point = (3, 4)
x, y = point   # "tuple unpacking" — szétszedés egy lépésben
```

Akkor használd, ha az adat **nem fog változni** (pl. egy koordináta, egy fix méretű rekord).

### dict — kulcs-érték párok

```python
repo = {"name": "NexusMind", "stars": 42, "language": "Python"}
repo["stars"]              # 42
repo["forks"] = 3          # új kulcs hozzáadása
repo.get("missing", 0)     # biztonságos lekérdezés, default értékkel
for key, value in repo.items():
    print(key, value)
```

**C# összehasonlítás:** ez a `Dictionary<string, object>` megfelelője — szintaktikailag sokkal egyszerűbb (`{}` vs `new Dictionary<...>()`), de ugyanaz az elv (hash-alapú kulcs-érték tárolás).

### set — egyedi elemek halmaza

```python
tags = {"bug", "urgent", "bug"}   # {"bug", "urgent"} — a duplikátum eltűnik
tags.add("reviewed")
"bug" in tags   # nagyon gyors ellenőrzés, mert hash-alapú
```

### Mátrix (2D adatszerkezet)

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
matrix[1][2]   # 6 — sor, oszlop indexeléssel
```

Komolyabb numerikus munkához (ha később adatanalitikai résznél kell) a `numpy` csomag `ndarray`-je sokkal hatékonyabb, mint a nyers `list of list`.

#### 🗂️ Flashcardok (Spaced Repetition)

A Python `list` a háttérben láncolt lista vagy dinamikus tömb?::Dinamikus tömb (akárcsak a C# `List<T>`).
Melyik adatszerkezet garantálja az egyedi elemeket?::A `set`.
Hogyan kérdezel le biztonságosan egy `dict` kulcsot, ami esetleg nem létezik?::A `.get(kulcs, alapérték)` metódussal.

---

## 7. Függvények (alprogramok)

```python
def greet(name: str, greeting: str = "Szia") -> str:
    return f"{greeting}, {name}!"

greet("Anna")                    # "Szia, Anna!" — default paraméter
greet("Béla", greeting="Hello")  # "Hello, Béla!" — kulcsszavas hívás
```

### Hatókör (scope)

```python
x = 10   # globális változó

def modify():
    x = 20   # ez egy ÚJ, lokális x — nem módosítja a globálist!
    print(x)  # 20

modify()
print(x)  # még mindig 10
```

⚠️ Ez a leggyakoribb kezdő hiba — Pythonban egy függvényen belüli értékadás alapból **lokális** változót hoz létre, hacsak nem jelzed explicit a `global` kulcsszóval, hogy a globálisra gondolsz. C#-ban ez máshogy működik (ott a blokk-hatókör más logikájú), úgyhogy érdemes ezt tudatosan megjegyezni.

### *args és **kwargs — változó számú paraméter

```python
def summarize(*numbers):          # tetszőleges számú pozícionális paraméter
    return sum(numbers)

def build_config(**options):      # tetszőleges számú kulcsszavas paraméter
    return options

summarize(1, 2, 3, 4)             # 10
build_config(debug=True, retries=3)  # {"debug": True, "retries": 3}
```

### Rekurzió

```python
def factorial(n: int) -> int:
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

Ugyanaz az elv, mint C#-ban — bázisest + rekurzív hívás. Python alapból kb. 1000 mélységű rekurziót enged (`sys.setrecursionlimit()`-tel állítható, de mélyebb rekurzióhoz inkább ciklust érdemes írni).

#### 🗂️ Flashcardok (Spaced Repetition)

Mi történik, ha egy függvényen belül értéket adsz egy globális névvel megegyező változónak, `global` nélkül?::Egy új, lokális változó jön létre, a globális változatlan marad.
Mire való a `*args` egy függvénydefinícióban?::Tetszőleges számú pozícionális paraméter átvételére.
Mi a rekurzió bázisesete?::Az a feltétel, ami megállítja a további rekurzív hívásokat.

---

## 8. Algoritmikus gondolkodás és alapalgoritmusok

A hivatalos tematika kifejezetten kéri az **algoritmusleíró eszköz** (pszeudokód) használatát, és a három klasszikus alapalgoritmus-mintát. Ezek mindegyike egy **akkumulátor-változó + ciklus** kombináció:

### Pszeudokód-minta

```
ALGORITMUS Összegzés(lista)
    összeg ← 0
    MINDEN elem ∈ lista ESETÉN
        összeg ← összeg + elem
    VISSZA összeg
```

### 1. Összegzés

```python
def sum_manual(numbers: list[int]) -> int:
    total = 0
    for n in numbers:
        total += n
    return total
```

### 2. Számlálás (feltételes)

```python
def count_evens(numbers: list[int]) -> int:
    count = 0
    for n in numbers:
        if n % 2 == 0:
            count += 1
    return count
```

### 3. Lineáris keresés

```python
def linear_search(numbers: list[int], target: int) -> int:
    for index, n in enumerate(numbers):
        if n == target:
            return index
    return -1   # nem található
```

### Bővítés: egyszerű rendezés (buborékrendezés — csak a logika megértésére)

```python
def bubble_sort(numbers: list[int]) -> list[int]:
    arr = numbers.copy()
    n = len(arr)
    for i in range(n):
        for j in range(n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]   # egyszerre csere, tuple-swap
    return arr
```

Ez a `arr[j], arr[j+1] = arr[j+1], arr[j]` sor Python-specifikus idióma — egy sorban cserél két értéket, nincs szükség ideiglenes változóra (amit C#-ban `(a, b) = (b, a)` szintén tud, ha tuple-t használsz, de a klasszikus C#-mintázat inkább `temp` változós).

**Gyakorlat:** valósítsd meg mindhárom alapalgoritmust **előbb pszeudokódban**, aztán Pythonban, egy önmagad választotta listán (pl. GitHub issue-k száma naponta).

#### 🗂️ Flashcardok (Spaced Repetition)

Milyen három klasszikus alapalgoritmust tárgyal ez a fejezet?::Összegzés, számlálás, lineáris keresés.
Mit ad vissza a lineáris keresés, ha nem találja az elemet?::-1-et (megállapodás szerint).
Mi az akkumulátor-változó szerepe ezekben az algoritmusokban?::Ciklusonként frissül, és a végén tartalmazza az eredményt (pl. összeg, darabszám).

---

## 9. Hibakezelés

```python
try:
    value = int(input("Szám: "))
    result = 10 / value
except ValueError:
    print("Ez nem szám volt!")
except ZeroDivisionError:
    print("Nullával nem lehet osztani!")
else:
    print(f"Eredmény: {result}")   # csak akkor fut, ha NEM volt hiba
finally:
    print("Ez mindig lefut")
```

**C# összehasonlítás:** `try/except/finally` ≈ `try/catch/finally`. A Python `else` blokk viszont C#-ban nincs — az "csak akkor fusson, ha nem volt hiba" logikát ott máshogy kellene megoldani.

### Saját kivétel

```python
class InvalidConfigError(Exception):
    pass

raise InvalidConfigError("Hiányzó API kulcs")
```

#### 🗂️ Flashcardok (Spaced Repetition)

Mikor fut le a Python `try`-`except` `else` blokkja?::Csak akkor, ha NEM történt hiba a try blokkban.
Hogyan hozol létre saját kivétel osztályt Pythonban?::Egy `Exception`-ből örökölt osztállyal (`class SajatHiba(Exception): pass`).

---

## 10. Objektumorientált programozás

### Osztályok alapjai

```python
class GitHubNotification:
    def __init__(self, title: str, repo: str, is_read: bool = False):
        self.title = title
        self.repo = repo
        self.is_read = is_read

    def is_noise(self) -> bool:
        return "dependabot" in self.repo.lower()

n = GitHubNotification(title="Új PR", repo="user/dependabot-updates")
print(n.is_noise())   # True
```

**C# összehasonlítás:** a `self` a C# `this`-nek felel meg, de **explicit** — minden példánymetódus első paramétere kötelezően `self`, nem hallgatólagos, mint C#-ban.

### @dataclass — gyors adatosztály boilerplate nélkül

```python
from dataclasses import dataclass

@dataclass
class Repo:
    name: str
    stars: int = 0
```

Ez helyettesíti azt, amit C#-ban kézzel kellene megírnod (konstruktor, `ToString()`, `Equals()`) — a `@dataclass` mindezt automatikusan legenerálja.

### Statikus metódusok és osztálymetódusok

Pythonnak **nincs `static class`** fogalma úgy, ahogy C#-ban. Helyette:

```python
class Validator:
    @staticmethod
    def is_valid_email(email: str) -> bool:
        return "@" in email   # nem fér hozzá self-hez, csak "csoportosítás" célból van itt

    @classmethod
    def from_dict(cls, data: dict) -> "Validator":
        return cls(**data)   # alternatív konstruktor, cls = maga az osztály
```

### Öröklés és interfészek

Pythonnak nincs `interface` kulcsszava. Három rokon eszköze van:

```python
from abc import ABC, abstractmethod

class NotificationSource(ABC):        # "interfész"-szerű absztrakt osztály
    @abstractmethod
    def fetch(self) -> list[dict]:
        ...

class GitHubSource(NotificationSource):
    def fetch(self) -> list[dict]:
        return [{"title": "Új issue"}]   # muszáj implementálni, különben hibát kapsz
```

Ez lesz a NexusMind-ban a "GitHub/Spotify/Steam forrás" közös absztrakciója — tehát ez nem elméleti gyakorlat, hanem valódi tervezési minta, amit élesben is használni fogsz.

A harmadik eszköz, a **`typing.Protocol`** (strukturális interfész — nem kell explicit örökölni belőle, elég ha megvannak a metódusok) inkább haladó, statikus típusellenőrzéshez (`mypy`) hasznos — egyelőre elég, ha tudod, hogy létezik.

#### 🗂️ Flashcardok (Spaced Repetition)

Mi a `self` szerepe egy Python metódusban?::A C# `this`-nek felel meg, de explicit paraméterként kell mindig szerepelnie.
Van-e Pythonnak `static class` kulcsszava?::Nincs — helyette `@staticmethod`/`@classmethod` vagy modul-szintű függvények.
Hogyan valósítasz meg "interfészt" Pythonban?::`abc.ABC` + `@abstractmethod`, vagy `typing.Protocol`.

---

## 11. Modulok és csomagkezelés

### Saját modul

```python
# models.py
class Repo:
    ...

# main.py
from models import Repo
```

### Virtuális környezet (venv) — ezt MINDEN projektnél használd

```bash
python -m venv .venv          # létrehozás
source .venv/bin/activate     # aktiválás (Linux/Mac)
.venv\Scripts\activate        # aktiválás (Windows)
pip install httpx pydantic    # csomag telepítése CSAK ebbe a környezetbe
pip freeze > requirements.txt # függőségek listázása
```

**C# összehasonlítás:** ez nagyjából a NuGet-csomagok projektenkénti izolációjának felel meg, csak explicit lépésekkel — Python nem csinálja automatikusan projektenként, neked kell létrehoznod és aktiválnod a venv-et. PyCharm ezt vizuálisan is segíti (Settings → Python Interpreter).

#### 🗂️ Flashcardok (Spaced Repetition)

Mire való a Python `venv`?::Projektenkénti izolált csomagkörnyezet létrehozására.
Melyik paranccsal hozol létre virtuális környezetet?::`python -m venv .venv`

---

## 12. Típusosítás és Pydantic

### Type hintek

```python
def average(numbers: list[int]) -> float:
    ...

age: int = 25
name: str | None = None   # lehet None is (Optional[str] régebbi szintaxissal)
```

Ezek **nem kényszerítik ki** futásidőben a típust (Python futtatáskor nem ellenőrzi!) — dokumentációs célt szolgálnak, és az IDE/`mypy` tudja őket kihasználni hibakereséshez.

### Pydantic — validált adatmodellek

```python
from pydantic import BaseModel, field_validator

class Issue(BaseModel):
    title: str
    number: int
    is_open: bool = True

    @field_validator("number")
    @classmethod
    def number_must_be_positive(cls, v):
        if v <= 0:
            raise ValueError("Az issue száma pozitív kell legyen")
        return v

data = '{"title": "Bug", "number": 42}'
issue = Issue.model_validate_json(data)   # validál ÉS parse-ol egy lépésben
```

**Miért fontos ez a projektben:** a Pydantic modellek lesznek a "nyelvtan" minden gRPC üzenethez, minden külső API-válaszhoz (GitHub, Spotify, Steam) — ezek adják a típusos határfelületet a C# oldal felé.

#### 🗂️ Flashcardok (Spaced Repetition)

Kényszerítik-e ki futásidőben a type hintek a megadott típust?::Nem, csak dokumentációs/IDE-célt szolgálnak — Python futáskor nem ellenőrzi.
Mi a Pydantic `BaseModel` fő előnye a sima `@dataclass`-hoz képest?::Automatikus validáció és JSON parse/serialize egy lépésben.

---

## 13. Async/await

```python
import asyncio
import httpx

async def fetch_user(username: str) -> dict:
    async with httpx.AsyncClient() as client:
        response = await client.get(f"https://api.github.com/users/{username}")
        return response.json()

async def main():
    results = await asyncio.gather(
        fetch_user("torvalds"),
        fetch_user("gvanrossum"),
    )   # PÁRHUZAMOSAN fut le mindkét hívás
    print(results)

asyncio.run(main())
```

### Három nyelv async-modellje egymás mellett

- **JavaScript/React:** egyszálú event loop, `Promise`, `async/await` — ezt ismered a React-es API-hívásaidból.
- **C#:** `Task`, `async/await`, de a háttérben **valódi szálkezelés** is történhet (`Task.Run`) — összetettebb modell.
- **Python:** `asyncio`, `async/await`, **egyszálú event loop** — fogalmilag közelebb áll a JS modellhez, mint a C#-éhoz. Nincs automatikus szálváltás; egy `async` függvény csak `await`-nél adja át a vezérlést.

Ez a hasonlóság (Python ≈ JS az async modellben) megkönnyíti a dolgodat — a React-tapasztalatod jó intuíciót ad.

#### 🗂️ Flashcardok (Spaced Repetition)

Melyik nyelv async-modellje áll közelebb a Pythonhoz: a JavaScript vagy a C#?::A JavaScript (mindkettő egyszálú event loopot használ).
Mire való az `asyncio.gather()`?::Több async függvény párhuzamos futtatására.

---

## 14. Gyakorlati integráció: valós API-hívás a nulláról

Ez a fejezet összeköti a fenti 13 fejezetet egyetlen, futtatható, valódi példában:

```python
import asyncio
import httpx
from pydantic import BaseModel

class Issue(BaseModel):
    title: str
    number: int
    state: str

async def fetch_issues(owner: str, repo: str) -> list[Issue]:
    url = f"https://api.github.com/repos/{owner}/{repo}/issues"
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
        raw_data = response.json()
        return [Issue(**item) for item in raw_data]   # dict -> Pydantic modell

def count_bugs(issues: list[Issue]) -> int:            # alapalgoritmus: számlálás
    count = 0
    for issue in issues:
        if "bug" in issue.title.lower():
            count += 1
    return count

async def main():
    issues = await fetch_issues("python", "cpython")
    print(f"Talált issue-k: {len(issues)}")
    print(f"Ebből 'bug' a címben: {count_bugs(issues)}")

asyncio.run(main())
```

Ha ez lefut nálad — gratula, gyakorlatilag a NexusMind Python-modul első vertikális szeletének "csontváza" kész: valós API + async I/O + Pydantic validáció + saját algoritmus egy helyen.

#### 🗂️ Flashcardok (Spaced Repetition)

Milyen sorrendben kapcsolódnak össze ebben a fejezetben a rétegek?::httpx (hívás) → Pydantic (validáció) → saját algoritmus (feldolgozás).

---

## 15. Automatizált tesztelés: pytest

A `pytest` a Python-ökoszisztéma de facto standard tesztelési keretrendszere — nagyjából azt a szerepet tölti be, mint nálad az **xUnit** a C#-ban, csak jelentősen kevesebb boilerplate-tel.

### Alapstruktúra

```python
# test_algorithms.py — a pytest a "test_" előtaggal ellátott fájlokat/függvényeket keresi meg automatikusan
from algorithms import sum_manual, count_evens, linear_search

def test_sum_manual():
    assert sum_manual([1, 2, 3]) == 6

def test_count_evens():
    assert count_evens([1, 2, 3, 4]) == 2

def test_linear_search_found():
    assert linear_search([10, 20, 30], 20) == 1

def test_linear_search_not_found():
    assert linear_search([10, 20, 30], 99) == -1
```

Futtatás: `pytest` a terminálból, vagy PyCharm-ban közvetlenül a zöld play gombbal a teszt mellett — ugyanaz az élmény, mint a Rider-ben az xUnit tesztekkel.

**C# összehasonlítás:**

| xUnit / Moq / Playwright (C#) | pytest (Python) |
|---|---|
| `[Fact]` attribútum | nincs szükség dekorátorra — elég, ha a függvény neve `test_`-tel kezdődik |
| `Assert.Equal(a, b)` | egyszerű `assert a == b` — nincs külön Assert-osztály |
| `[Theory]` + `[InlineData(...)]` paraméteres teszthez | `@pytest.mark.parametrize(...)` dekorátor |
| Moq (`Mock<IService>`) | `unittest.mock` (beépített modul) vagy `pytest-mock` plugin |
| Playwright (C# binding) | Playwright Pythonban **natívan létezik** (`playwright` csomag), gyakorlatilag ugyanazokkal a fogalmakkal (page, locator, expect) |
| `[Fixture]`/konstruktoros setup | `@pytest.fixture` dekorátor — újrafelhasználható előkészítő logika |

### Paraméteres teszt (a Theory/InlineData megfelelője)

```python
import pytest

@pytest.mark.parametrize("numbers, expected", [
    ([1, 2, 3], 6),
    ([], 0),
    ([-1, 1], 0),
])
def test_sum_manual_parametrized(numbers, expected):
    assert sum_manual(numbers) == expected
```

### Mockolás (a Moq megfelelője)

```python
from unittest.mock import AsyncMock

async def test_fetch_issues_with_mock():
    mock_client = AsyncMock()
    mock_client.get.return_value.json.return_value = [
        {"title": "Bug", "number": 1, "state": "open"}
    ]
    # itt a valódi httpx.AsyncClient helyett a mock_client-et adnád át a fetch_issues függvénynek
```

### Fixture (újrafelhasználható előkészítés)

```python
@pytest.fixture
def sample_issues():
    return [
        {"title": "Bug in login", "number": 1, "state": "open"},
        {"title": "Feature request", "number": 2, "state": "closed"},
    ]

def test_count_bugs(sample_issues):
    issues = [Issue(**item) for item in sample_issues]
    assert count_bugs(issues) == 1
```

**Miért fontos ez a NexusMind-nak:** amikor a gRPC handlerek és a Pydantic modellek elkészülnek, a `pytest` lesz az az eszköz, amivel — pont úgy, ahogy a korábbi projektjeidnél az xUnit-tal — lefeded a Python-modul üzleti logikáját unit tesztekkel, a külső API-hívásokat pedig mockolod, hogy a tesztek ne valódi hálózati hívásoktól függjenek.

Ez a fejezet, a 9–14.-hez hasonlóan, túlmutat a hivatalos egyetemi tematikán — ez már kifejezetten a NexusMind-projekthez és a nálad meglévő tesztelési szemlélethez kapcsolódó, szerves következő lépés.

#### 🗂️ Flashcardok (Spaced Repetition)

Mi a pytest megfelelője az xUnit `[Fact]` attribútumának?::Nincs szükség dekorátorra, elég ha a függvény neve `test_`-tel kezdődik.
Mivel helyettesíted Pythonban a Moq mock-objektumait?::Az `unittest.mock` modullal (pl. `AsyncMock`).
Mire való a `@pytest.fixture`?::Újrafelhasználható, tesztek közti előkészítő logika biztosítására.

---

## 16. Megfelelési táblázat a hivatalos tematikához

| Hivatalos tematika eleme | Fejezet |
|---|---|
| Forráskód, fordítás/értelmezés, futtatás | 1. |
| IDE megismerése | 1. |
| Konstansok, változók | 2. |
| Típusok, operátorok | 2. |
| Standard I/O | 5. |
| Vezérlési szerkezetek | 3. |
| Sztringkezelés | 4. |
| Alprogramok (függvények) | 7. |
| Tárbeli adatszerkezetek | 6. |
| Algoritmusleíró eszköz (pszeudokód) | 8. |
| Alapvető algoritmusok (összegzés, számlálás, keresés) | 8. |

A 10–15. fejezetek már túlmutatnak a hivatalos tematikán — ezek a NexusMind projekthez szükséges haladó réteg (OOP, csomagkezelés, típusosítás, async, valós integráció, automatizált tesztelés).

#### 🗂️ Flashcardok (Spaced Repetition)

Melyik fejezet fedi a hivatalos tematika "algoritmusleíró eszköz" pontját?::A 8. fejezet.
Melyik fejezetek számítanak a hivatalos egyetemi tematikán túlmutató, projekt-specifikus anyagnak?::A 10–15. fejezetek.

---

## Hogyan használd ezt a könyvet

Nem kell lineárisan, egyben végigolvasnod. Javasolt megközelítés:
1. Olvasd át 1–8. fejezetet, és minden fejezetnél futtasd le a kódpéldákat, csináld meg a gyakorlatokat.
2. 9–11. fejezet, amikor a "koszosabb" adatokkal/hibákkal/csomagokkal dolgozó feladatok jönnek.
3. 12–14. fejezet, amikor ténylegesen elkezded a NexusMind Python-modulját építeni.
4. 15. fejezet (pytest), amikor az első Python-modulodhoz már írnál unit teszteket — természetes folytatása annak, amit xUnit/Moq/Playwright-tal a C#-os projektjeidnél már megszoktál.

Ha bármelyik fejezetnél elakadsz, vagy egy konkrét hibaüzenetet nem értesz, hozd ide — együtt átbeszéljük, konkrét kódon keresztül.
