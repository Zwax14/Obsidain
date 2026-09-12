---
tags:
  - python
  - nexusmind
  - egyetem
---

# Python referenciakönyv — a nulláról a NexusMind-ig

Ez egy **fejezetenkénti kézikönyv**, amit saját tempódban járhatsz végig, előre-hátra lapozhatsz benne, és később is visszakereshetsz belőle. Két dolgot fed le egyszerre:

1. A **"Bevezetés a programozásba"** hivatalos tematikáját (alapfogalmak → algoritmikus gondolkodás).
2. Azt a **haladó réteget**, amire a NexusMind projekt Python-moduljához (gRPC, Pydantic, async, AI API-hívások) szükséged lesz.

Minden fejezetnél ott vannak a **C# összehasonlítások**, mert ez a leggyorsabb út, hogy ne a nulláról tanulj, hanem a meglévő programozói tudásodra építs. Minden kódrészlet **önmagában futtatható és kiírja a saját eredményét** (`print()`-tel) — nem kell mást hozzáadnod, hogy lásd, mit csinál. Másold be egy `.py` fájlba, és futtasd.

A kódrészletek után mindenhol ott a **"miért így?"** magyarázat is: nem elég tudni, hogy *mit* írj, azt is érdemes érteni, *miért* pont úgy deklaráljuk, ahogy — ez az, ami hosszú távon megkülönbözteti a "kód lemásolását" a "kód megértésétől".

---

## Tartalomjegyzék

1. [Mi a Python, és hogyan fut le egy program](#1-mi-a-python-és-hogyan-fut-le-egy-program)
2. [Alaptípusok, változók, operátorok](#2-alaptípusok-változók-operátorok)
3. [Vezérlési szerkezetek](#3-vezérlési-szerkezetek)
4. [Sztringkezelés](#4-sztringkezelés)
5. [Standard I/O](#5-standard-io)
6. [Adatszerkezetek: list, tuple, dict, set](#6-adatszerkezetek-list-tuple-dict-set)
7. [Függvények (alprogramok)](#7-függvények-alprogramok)
8. [Algoritmikus gondolkodás és alapalgoritmusok](#8-algoritmikus-gondolkodás-és-alapalgoritmusok)
9. [Hibakezelés](#9-hibakezelés)
10. [Objektumorientált programozás](#10-objektumorientált-programozás)
11. [Modulok és csomagkezelés](#11-modulok-és-csomagkezelés)
12. [Típusosítás és Pydantic](#12-típusosítás-és-pydantic)
13. [Async/await](#13-asyncawait)
14. [Gyakorlati integráció: valós API-hívás a nulláról](#14-gyakorlati-integráció-valós-api-hívás-a-nulláról)
15. [Automatizált tesztelés: pytest](#15-automatizált-tesztelés-pytest)
16. [Megfelelési táblázat a hivatalos tematikához](#16-megfelelési-táblázat-a-hivatalos-tematikához)

---

## 1. Mi a Python, és hogyan fut le egy program

A Python **interpretált** nyelv — pontosabban: a forráskódot (`.py` fájl) a Python futtatókörnyezet előbb **bájtkóddá fordítja** (ezt automatikusan, láthatatlanul csinálja, `.pyc` fájlokba cache-elve, jellemzően egy `__pycache__` mappában), majd ezt a bájtkódot a **Python Virtuális Gép (PVM)** értelmezi sorról sorra, futás közben.

Miért fontos ezt tudni, és nem csak "elhinni"? Mert ez magyarázza meg a Python két jellegzetes viselkedését:

- **Nem igényel külön fordítási lépést** tőled (ellentétben C#-szal, ahol `dotnet build` kell a futtatás előtt) — írod a kódot, futtatod, kész. A bájtkóddá fordítás a háttérben, a `python3` parancs meghívásakor, automatikusan megtörténik.
- **Szintaktikai hibákat** (pl. hiányzó kettőspont, rossz zárójelezés) már a bájtkóddá fordításkor jelez, még mielőtt egyetlen sor is lefutott volna.
- **Típus- és logikai hibákat** viszont csak **futás közben**, amikor a hibás sor ténylegesen lefut — ha egy hibás sor egy `if`-ágban van, ami sosem fut le, a hiba soha nem fog előjönni. Ez a fő különbség a C#-hoz képest, ahol a fordító a *teljes* kódot statikusan ellenőrzi típushiba szempontjából, még azt is, ami esetleg sosem fut le.

**C# összehasonlítás:** C#-ban `dotnet build` → IL kód → JIT fordítás futáskor natív gépi kódra. Pythonban: `.py` → bájtkód (automatikusan) → PVM értelmezi. Egyik sem "gépi kódra fordít" közvetlenül — mindkettő egy köztes réteget használ, csak más a részletezettség és más a hibaellenőrzés időzítése.

### Hogyan futtass egy Python programot

```bash
python3 fajlnev.py
```

PyCharm-ban ez a zöld "Run" gombbal megy, és a debugger (töréspont, `F8`/`F9` léptetés) pontosan úgy működik, mint amit Rider-ből vagy Visual Studio-ból ismersz.

### Az első program

```python
print("Szia, NexusMind!")
```

Kimenet:

```
Szia, NexusMind!
```

Nincs `Main` metódus, nincs osztály-kényszer, nincs `;` a sorok végén. Ennek az az oka, hogy a Python-fájl teteje **maga a program belépési pontja** — a PVM egyszerűen sorról sorra végrehajtja, ami a fájlban van, felülről lefelé. Ha egy sorban csak egy kifejezés áll (pl. `print(...)`), az azonnal lefut, amint a PVM odaér — nincs szükség egy külön "ez itt a program kezdete" jelzésre, mint C#-ban a `static void Main()`.

### Gyakorló kérdések

- Mi történik a Python forráskóddal, mielőtt lefut? *(Bájtkóddá fordul, amit a Python Virtuális Gép (PVM) értelmez sorról sorra.)*
- Mikor derül ki egy Python programban egy típushiba? *(Csak futás közben, amikor a hibás sor ténylegesen lefut.)*
- Melyik parancs futtat egy Python fájlt a terminálból? *(`python3 fajlnev.py`)*

---

## 2. Alaptípusok, változók, operátorok

### Változók és típusok

Python **dinamikusan típusos**: a változóhoz nem típust rendelsz, hanem egy értéket — a típus az *értékből* következik, és futás közben változhat.

```python
x = 5
print(type(x), x)   # <class 'int'> 5

x = "szöveg"
print(type(x), x)   # <class 'str'> szöveg
```

Miért működik ez így? Mert Pythonban egy változónév valójában csak egy **címke (referencia)**, amit rá tudsz kötni bármelyik objektumra a memóriában — a névhez magához nincs típus-metaadat rögzítve, csak ahhoz az objektumhoz, amire éppen mutat. Amikor újra értéket adsz `x`-nek, nem "átalakítod" a régi értéket, hanem a címkét egy másik, új objektumra irányítod át.

**C# összehasonlítás:** C#-ban `int x = 5;` — a típus a *változóhoz* van kötve, örökre, a fordító ezt ellenőrzi minden hozzárendelésnél. Pythonban a típus az *értékhez* tartozik, nem a változóhoz. Ez kényelmes, de felelősséget is jelent — neked kell fejben tartanod, mi micsoda (vagy type hinteket használnod, lásd 12. fejezet).

Alaptípusok: `int`, `float`, `str`, `bool`, `None` (ez a C# `null` megfelelője, de kis kezdőbetű helyett nagybetűvel írjuk: `None`, mert Pythonban a `None` egy egyedi, beépített objektum-példány, nem egy kulcsszó-szintű "üres referencia").

```python
age = 25
price = 19.99
name = "NexusMind"
is_active = True
result = None

print(type(age), type(price), type(name), type(is_active), type(result))
```

Kimenet:

```
<class 'int'> <class 'float'> <class 'str'> <class 'bool'> <class 'NoneType'>
```

### Konstansok

Pythonnak **nincs valódi konstans-kulcsszava** (nincs `const`, mint C#-ban). Ennek nyelvi oka van: a Python filozófiája ("we're all consenting adults here") szerint a nyelv nem kényszerít ki futásidejű védelmet olyasmire, amit konvencióval is meg lehet oldani — ezért a "ne módosítsd ezt" jelzést a **csupa nagybetűs név** hordozza, amit minden Python-fejlesztő ismer és tiszteletben tart, de a nyelv maga nem ellenőriz.

```python
MAX_RETRY_COUNT = 5   # ez csak megállapodás, a nyelv nem kényszeríti ki

MAX_RETRY_COUNT = 10  # ez legális, semmilyen hibát nem dob, csak "illetlen"
print(MAX_RETRY_COUNT)
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

Fontos csapda: Python-ban `and`/`or`/`not` **szavak**, nem szimbólumok — ha `&&`-t írsz, `SyntaxError`-t kapsz. Ennek az az oka, hogy a Python olvashatóságot preferál a tömörséggel szemben — a nyelv tervezői tudatosan döntöttek úgy, hogy a logikai operátorok angol szavak legyenek.

```python
a = 7
b = 2
print(a / b)    # 3.5  — lebegőpontos osztás
print(a // b)   # 3    — egész osztás, lefelé kerekít
print(a % b)    # 1    — maradék
print(a ** 2)   # 49   — hatványozás

year = 2024
is_leap = year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)
print(is_leap)  # True — 2024 szökőév
```

### Gyakorló kérdések

- Hogyan viselkedik egy Python változó típusa futás közben? *(Dinamikusan változhat, mert a típus az értékhez, nem a változóhoz tartozik.)*
- Van-e Pythonnak valódi `const` kulcsszava? *(Nincs, csak konvenció: csupa nagybetűs név.)*
- Mi az eredménye a `7 // 2` kifejezésnek? *(3, egész osztás.)*
- Mivel helyettesítjük Pythonban a `&&` és `||` operátorokat? *(Az `and` és `or` szavakkal.)*

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

Kimenet: `Kellemes`

Nincs `{}` — a **behúzás (indentation)** határozza meg a blokkot. Ez nem esztétikai döntés: a Python tervezői szerint a kód vizuális szerkezetének (hogyan van behúzva) mindig egyeznie kellene azzal, hogyan fut le a kód — ha ezt a nyelv kikényszeríti, elkerülhetetlen, hogy a kód "úgy nézzen ki", ahogy viselkedik. Ez a leggyakoribb kezdő hiba C#-ról jőve: a behúzásnak **konzisztensnek** kell lennie (PyCharm automatikusan 4 szóközt használ egy `Tab` leütésére, ne keverd valódi tab-karakterrel, mert az `IndentationError`-t okozhat).

### for ciklus

```python
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for item in ["a", "b", "c"]:   # közvetlenül a lista elemein iterálsz
    print(item)
```

**C# összehasonlítás:** ez leginkább a C# `foreach`-nek felel meg — mindkettő közvetlenül a *elemeken* iterál, nem egy számláló-indexen. A klasszikus `for (int i=0; i<n; i++)` mintát Pythonban a `range()`-dzsel utánozod: `for i in range(n):`. A `range(5)` maga nem egy lista — egy "lusta" (lazy) generátor-szerű objektum, ami menet közben, egyesével adja ki a következő számot, ezért memóriahatékony akkor is, ha `range(1_000_000)`-et írnál.

### while ciklus

```python
count = 0
while count < 5:
    print(count)
    count += 1   # nincs count++ Python-ban!
```

⚠️ **Nincs `++` és `--` operátor Pythonban** — mindig `+= 1` / `-= 1` kell. A Python tervezői tudatosan hagyták ki, mert az `++`/`--` C-stílusú nyelvekben gyakori hibaforrás (pre- vs. post-increment félreértések), és a `+= 1` egyértelműbb, kevésbé félreérthető.

### break, continue

```python
for i in range(10):
    if i == 3:
        continue   # kihagyja a 3-at, ugrik a következő iterációra
    if i == 6:
        break      # teljesen kilép a ciklusból
    print(i)
```

Kimenet: `0 1 2 4 5` (soronként), ugyanúgy működnek, mint C#-ban.

### Gyakorló kérdések

- Mi határozza meg egy Python kódblokk határait `{}` helyett? *(A behúzás/indentation.)*
- Van-e Pythonban `++` operátor? *(Nincs, mindig `+= 1`-et kell írni.)*
- Melyik Python ciklus felel meg leginkább a C# `foreach`-nek? *(A `for item in lista:` szerkezet.)*

---

## 4. Sztringkezelés

A `str` Python-ban **immutable** (megváltoztathatatlan) — minden "módosító" metódus valójában egy **új** stringet ad vissza, az eredetit nem érinti. Ez azért fontos, mert stringeket biztonságosan lehet megosztani több hely között a programban anélkül, hogy félned kellene, hogy valaki "a hátad mögött" módosítja őket.

```python
s = "  Szia NexusMind  "

print(repr(s.strip()))      # 'Szia NexusMind' — levágja a whitespace-t
print(s.lower())            # kisbetűsít
print(s.upper())            # nagybetűsít
print(s.replace("Szia", "Hello"))
print(s.split(" "))         # lista lesz belőle
print("-".join(["a", "b", "c"]))   # "a-b-c" — lista összefűzése
print(s.startswith("  Sz")) # True
print("NexusMind" in s)     # True — tartalmazás-vizsgálat az `in`-nel

print(s)   # az eredeti s VÁLTOZATLAN maradt, mert a str immutable
```

### f-string (formázott string) — ezt fogod leggyakrabban használni

```python
name = "NexusMind"
version = 1
print(f"Üdv a {name} v{version}-ben!")
print(f"Pi kb. {3.14159:.2f}")   # 2 tizedesjegyre kerekítve: 3.14
```

Kimenet:

```
Üdv a NexusMind v1-ben!
Pi kb. 3.14
```

**C# összehasonlítás:** ez pontosan a C# `$"Üdv a {name}"` interpolált stringjének felel meg — ha ezt már ismered C#-ból, itt semmi új nincs, csak `f` betű `$` helyett. A `{érték:formátum}` szintaxis (pl. `:.2f`) is ugyanazt a szerepet tölti be, mint C#-ban a `{érték:F2}`.

### Szeletelés (slicing) — string és lista is "sorozat" típus

```python
s = "NexusMind"
print(s[0])      # "N"       — index 0-tól számolunk
print(s[0:5])    # "Nexus"   — a végpont KIZÁRÓLAGOS (5. index már nincs benne)
print(s[-4:])    # "Mind"    — negatív index a végétől számol
print(s[::-1])   # "dniMsuxeN" — a lépésköz -1, tehát végigmegy, de visszafelé
```

Miért `s[0:5]` és nem `s[0:4]`, ha "Nexus" 5 karakter? Mert a szeletelés `[kezdő:vég]` jelölése Pythonban (és sok más nyelvben is) a **kezdőindexet zárja be, a végindexet viszont kizárja** — ez lehetővé teszi, hogy `vég - kezdő` mindig pontosan megadja a kapott elemek számát, ami sok algoritmusban kényelmes tulajdonság.

### Gyakorló kérdések

- Módosítható-e egy Python string a helyén (mutable)? *(Nem, a `str` immutable — minden módosító metódus új stringet ad vissza.)*
- Mi az f-string szintaxisa? *(`f"szöveg {változó}"`.)*
- Mit ad vissza az `s[::-1]`? *(A string megfordítva.)*

---

## 5. Standard I/O

### Kimenet

```python
print("Egy sor")
print("a", "b", "c")              # "a b c" — szóközzel elválasztva
print("a", "b", sep="-")          # "a-b"
print("Nincs sortörés", end="")   # nem ugrik új sorba
print(" — folytatás ugyanabban a sorban")
```

A `print()` alapból minden argumentum közé szóközt tesz (`sep=" "` az alapértelmezés), és a végén sortörést ír (`end="\n"` az alapértelmezés) — mindkettő felülírható paraméterrel, mert a Python-tervezők a leggyakoribb esetet (szóközzel elválasztott, soronkénti kiírás) tették alapértelmezetté, de nem zárták ki a testreszabást.

### Bemenet

```python
name = input("Add meg a neved: ")   # MINDIG stringet ad vissza!
print(f"Szia, {name}!")

age_text = input("Add meg az életkorod: ")
age = int(age_text)                 # explicit konvertálni kell
print(f"Jövőre {age + 1} éves leszel.")
```

⚠️ **Fontos csapda:** az `input()` mindig `str`-t ad vissza, akkor is, ha számot gépelsz be — neked kell explicit konvertálnod (`int()`, `float()`). Ennek az az oka, hogy a billentyűzetről érkező adat technikailag mindig szöveg-karaktersorozat; a Python nem próbálja "kitalálni", hogy számot akartál-e beírni, mert ez rejtett, nehezen megjósolható viselkedéshez vezetne (pl. mi történjen, ha `"5"`-öt írsz be — number legyen vagy string?).

**C# összehasonlítás:** ez a `Console.ReadLine()` megfelelője — ott is stringet kapsz vissza, és neked kell `int.Parse()`-olnod. Ebben a két nyelv szinte azonos.

### Gyakorló kérdések

- Milyen típust ad vissza mindig az `input()`? *(Mindig `str`-t, még számbevitel esetén is.)*
- Hogyan függeszted fel a sortörést a `print()`-nél? *(Az `end=""` paraméterrel.)*

---

## 6. Adatszerkezetek: list, tuple, dict, set

### list — dinamikus tömb (ez a "munkaló" adatszerkezeted)

```python
numbers = [1, 2, 3, 4, 5]
numbers.append(6)          # hozzáadás a végéhez
numbers.insert(0, 0)       # beszúrás adott pozícióra
numbers.remove(3)          # érték alapú törlés (az ELSŐ 3-as elemet törli)
last = numbers.pop()       # utolsó elem eltávolítása ÉS visszaadása

print(numbers)             # [0, 1, 2, 4, 5]
print(last)                # 6
print(len(numbers))        # 5 — hossz
print(3 in numbers)        # False — tartalmazás-vizsgálat
```

⚠️ **Fontos felismerés (OS-tárgyhoz is kapcsolódik):** a Python `list` **nem láncolt lista**, hanem — akárcsak a C# `List<T>` — egy dinamikusan növekvő **tömb** a háttérben, ami folyamatos memóriablokkot használ, és szükség esetén nagyobb blokkra másolja át magát. Ez azt jelenti, hogy az indexeléssel elérés (`numbers[3]`) O(1), de egy elem *elejére* beszúrása (`insert(0, ...)`) O(n), mert minden utána lévő elemet el kell tolni. A klasszikus, mutatókkal összekötött láncolt lista Pythonban nincs beépítve; ha ennek elvét szeretnéd használni (gyors beszúrás elejére/végére), a `collections.deque` áll hozzá legközelebb.

### List comprehension — Python egyik legjellemzőbb idiómája

```python
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]

print(squares)
print(evens)
```

Miért érdemes ezt a formát választani egy hagyományos `for` ciklus + `append()` helyett? Mert a list comprehension egyetlen kifejezésben mondja ki, hogy "milyen az az új lista, amit akarok" (deklaratív stílus), szemben azzal, hogy lépésről lépésre elmondanád, *hogyan* építsd fel (imperatív stílus) — ez rövidebb, és a Python belsőleg is hatékonyabban tudja végrehajtani, mint egy explicit ciklust.

**C# összehasonlítás:** ez nagyjából a LINQ `.Select()`/`.Where()` párosának felel meg, csak tömörebb szintaxissal: `numbers.Select(x => x*2).Where(x => x > 5)` ≈ `[x*2 for x in numbers if x*2 > 5]`.

### tuple — megváltoztathatatlan sorozat

```python
point = (3, 4)
x, y = point   # "tuple unpacking" — szétszedés egy lépésben
print(f"x={x}, y={y}")
```

Akkor használd, ha az adat **nem fog változni** (pl. egy koordináta, egy fix méretű rekord). Az immutabilitásnak gyakorlati előnye is van: egy `tuple` — a `list`-tel ellentétben — használható `dict` kulcsként vagy `set` elemként, mert ezekhez a nyelvnek garantálnia kell, hogy a kulcs/elem nem változik meg a tárolás után (különben a hash-alapú keresés inkonzisztenssé válna).

### dict — kulcs-érték párok

```python
repo = {"name": "NexusMind", "stars": 42, "language": "Python"}

print(repo["stars"])              # 42
repo["forks"] = 3                 # új kulcs hozzáadása
print(repo.get("missing", 0))     # 0 — biztonságos lekérdezés, default értékkel

for key, value in repo.items():
    print(key, "->", value)
```

Miért van külön `.get()` metódus, ha a `repo["kulcs"]`-sal is elérhető az érték? Mert `repo["missing"]` `KeyError`-t dobna, ha a kulcs nem létezik — a `.get(kulcs, alapérték)` ezt egyetlen sorban, hibakezelés nélkül old meg, ha van egy elfogadható alapértelmezett érték.

**C# összehasonlítás:** ez a `Dictionary<string, object>` megfelelője — szintaktikailag sokkal egyszerűbb (`{}` vs `new Dictionary<...>()`), de ugyanaz az elv (hash-alapú kulcs-érték tárolás, átlagosan O(1) keresés).

### set — egyedi elemek halmaza

```python
tags = {"bug", "urgent", "bug"}   # a duplikátum eltűnik
tags.add("reviewed")

print(tags)                # {'bug', 'urgent', 'reviewed'} — sorrend nem garantált
print("bug" in tags)       # True, nagyon gyors ellenőrzés, mert hash-alapú
```

A `set` azért garantálja az egyediséget és azért gyors a `in` ellenőrzés, mert ugyanazt a hash-alapú tárolást használja belül, mint a `dict` — csak itt nincsenek "értékek" a kulcsok mellett, csak maguk a kulcsok (elemek).

### Mátrix (2D adatszerkezet)

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(matrix[1][2])   # 6 — sor, oszlop indexeléssel
```

Komolyabb numerikus munkához (ha később adatanalitikai résznél kell) a `numpy` csomag `ndarray`-je sokkal hatékonyabb, mint a nyers `list of list`, mert folytonos memóriablokkot és vektorizált (C-szintű) műveleteket használ.

### Gyakorló kérdések

- A Python `list` a háttérben láncolt lista vagy dinamikus tömb? *(Dinamikus tömb, akárcsak a C# `List<T>`.)*
- Melyik adatszerkezet garantálja az egyedi elemeket? *(A `set`.)*
- Hogyan kérdezel le biztonságosan egy `dict` kulcsot, ami esetleg nem létezik? *(A `.get(kulcs, alapérték)` metódussal.)*

---

## 7. Függvények (alprogramok)

```python
def greet(name: str, greeting: str = "Szia") -> str:
    return f"{greeting}, {name}!"

print(greet("Anna"))                    # "Szia, Anna!" — default paraméter
print(greet("Béla", greeting="Hello"))  # "Hello, Béla!" — kulcsszavas hívás
```

Miért van a `name: str` és a `-> str` a definícióban, ha Python futásidőben nem is ellenőrzi őket? Ezek **type hint**-ek (lásd bővebben 12. fejezet): nem kényszerítik ki a típust, de dokumentálják a szándékot — az IDE (PyCharm) ez alapján tud autocomplete-et adni, hibát jelezni még futtatás előtt, és bárki, aki később olvassa a kódot, azonnal látja, mit vár a függvény.

### Hatókör (scope)

```python
x = 10   # globális változó

def modify():
    x = 20   # ez egy ÚJ, lokális x — nem módosítja a globálist!
    print("belül:", x)   # 20

modify()
print("kívül:", x)   # még mindig 10
```

⚠️ Ez a leggyakoribb kezdő hiba — Pythonban egy függvényen belüli értékadás alapból **lokális** változót hoz létre, hacsak nem jelzed explicit a `global` kulcsszóval, hogy a globálisra gondolsz:

```python
counter = 0

def increment():
    global counter   # explicit jelzés: a globális "counter"-t módosítom
    counter += 1

increment()
increment()
print(counter)   # 2
```

Miért kell ezt explicit kijelölni? Mert a Python fordítási időben (mielőtt a függvény lefutna) eldönti, hogy egy adott név lokális vagy globális lesz az adott függvényben — ha bárhol a függvényben történik értékadás egy névre, azt a Python automatikusan lokálisnak veszi *az egész függvényen belül*, hacsak nem mondod meg a `global` kulcsszóval, hogy ez alól kivétel. Ez elejét veszi annak, hogy egy elgépelt vagy véletlen névismétlés csendben módosítsa a globális állapotot.

### *args és **kwargs — változó számú paraméter

```python
def summarize(*numbers):          # tetszőleges számú pozícionális paraméter
    return sum(numbers)

def build_config(**options):      # tetszőleges számú kulcsszavas paraméter
    return options

print(summarize(1, 2, 3, 4))                 # 10
print(build_config(debug=True, retries=3))   # {"debug": True, "retries": 3}
```

A `*numbers` a pozícionális extra argumentumokat egy `tuple`-be gyűjti össze, a `**options` pedig a kulcsszavas extrákat egy `dict`-be — ez teszi lehetővé, hogy a függvényed anélkül fogadjon el rugalmas számú bemenetet, hogy előre fel kellene sorolnod minden lehetséges paramétert.

### Rekurzió

```python
def factorial(n: int) -> int:
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))   # 120
```

Ugyanaz az elv, mint C#-ban — bázisest + rekurzív hívás. Python alapból kb. 1000 mélységű rekurziót enged (`sys.setrecursionlimit()`-tel állítható), mert a PVM minden rekurzív hívásnál egy új "stack frame"-et hoz létre, és ez a korlát véd a végtelen rekurzió miatti memóriakifogyástól; mélyebb rekurzióhoz inkább ciklust érdemes írni.

### Gyakorló kérdések

- Mi történik, ha egy függvényen belül értéket adsz egy globális névvel megegyező változónak, `global` nélkül? *(Egy új, lokális változó jön létre, a globális változatlan marad.)*
- Mire való a `*args` egy függvénydefinícióban? *(Tetszőleges számú pozícionális paraméter átvételére.)*
- Mi a rekurzió bázisesete? *(Az a feltétel, ami megállítja a további rekurzív hívásokat.)*

---

## 8. Algoritmikus gondolkodás és alapalgoritmusok

A hivatalos tematika kifejezetten kéri az **algoritmusleíró eszköz** (pszeudokód) használatát, és a három klasszikus alapalgoritmus-mintát. Ezek mindegyike egy **akkumulátor-változó + ciklus** kombináció: van egy változó, ami az eredményt gyűjti, és a ciklus minden lépésben frissíti.

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
    total = 0            # akkumulátor: itt gyűjtjük az eredményt
    for n in numbers:
        total += n
    return total

print(sum_manual([1, 2, 3, 4, 5]))   # 15
```

### 2. Számlálás (feltételes)

```python
def count_evens(numbers: list[int]) -> int:
    count = 0
    for n in numbers:
        if n % 2 == 0:
            count += 1
    return count

print(count_evens([1, 2, 3, 4, 5, 6]))   # 3
```

### 3. Lineáris keresés

```python
def linear_search(numbers: list[int], target: int) -> int:
    for index, n in enumerate(numbers):
        if n == target:
            return index
    return -1   # nem található

print(linear_search([10, 20, 30], 20))   # 1
print(linear_search([10, 20, 30], 99))   # -1
```

Miért `enumerate(numbers)` és nem `range(len(numbers))`? Mert az `enumerate()` egyszerre adja vissza az indexet *és* az elemet, olvashatóbban, mint ha külön kellene indexelned (`numbers[i]`) — ez a "pythonic" (a nyelv szellemének megfelelő) megoldás.

### Bővítés: egyszerű rendezés (buborékrendezés — csak a logika megértésére)

```python
def bubble_sort(numbers: list[int]) -> list[int]:
    arr = numbers.copy()   # másolat, hogy az eredeti listát ne módosítsuk
    n = len(arr)
    for i in range(n):
        for j in range(n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]   # egyszerre csere

    return arr

print(bubble_sort([5, 2, 4, 1, 3]))   # [1, 2, 3, 4, 5]
```

Miért `numbers.copy()` a legelső sorban? Mert a Python listákat **referencia szerint** adja át a függvényeknek — ha közvetlenül `arr = numbers`-t írtál volna, `arr` és `numbers` ugyanarra a listára mutatna a memóriában, és a `bubble_sort` "melléhatásként" felcserélné a hívó eredeti listáját is. A `.copy()` egy új, független listát hoz létre, hogy a függvény ne módosítsa a bemenetét — ez a "tiszta függvény" (nincs mellékhatása) elvének betartása.

A `arr[j], arr[j+1] = arr[j+1], arr[j]` sor Python-specifikus idióma — egy sorban cserél két értéket, nincs szükség ideiglenes változóra, mert a Python a jobb oldalt (`arr[j+1], arr[j]`) előbb kiértékeli egy átmeneti tuple-lé, és csak utána rendeli hozzá a bal oldali nevekhez. (C#-ban ezt `(a, b) = (b, a)` szintén meg lehet oldani tuple-lel, de a klasszikus C#-mintázat inkább `temp` változós.)

### Gyakorló kérdések

- Milyen három klasszikus alapalgoritmust tárgyal ez a fejezet? *(Összegzés, számlálás, lineáris keresés.)*
- Mit ad vissza a lineáris keresés, ha nem találja az elemet? *(-1-et, megállapodás szerint.)*
- Mi az akkumulátor-változó szerepe ezekben az algoritmusokban? *(Ciklusonként frissül, és a végén tartalmazza az eredményt, pl. összeg, darabszám.)*

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

**C# összehasonlítás:** `try/except/finally` ≈ `try/catch/finally`. A Python `else` blokk viszont C#-ban nincs — az "csak akkor fusson, ha nem volt hiba" logikát ott máshogy kellene megoldani (pl. a `try` blokk végére írva). Az `else` létezésének célja, hogy elkülönítsd azt a kódot, ami *csak sikeres esetben* fusson, attól a kódtól, ami *hibalehetőséget* rejt — így a `try` blokkban csak a ténylegesen kockázatos sorok maradnak.

### Saját kivétel

```python
class InvalidConfigError(Exception):
    pass

def load_config(data: dict) -> str:
    if "api_key" not in data:
        raise InvalidConfigError("Hiányzó API kulcs")
    return data["api_key"]

try:
    load_config({})
except InvalidConfigError as e:
    print(f"Konfigurációs hiba: {e}")
```

Miért érdemes saját kivételosztályt létrehozni ahelyett, hogy egy általános `Exception`-t dobnál? Mert egy egyedi kivételosztály (`InvalidConfigError`) lehetővé teszi, hogy a hívó kód **szelektíven** kapja el csak ezt a hibatípust (`except InvalidConfigError:`), anélkül hogy minden más, váratlan hibát is elnyelne — ez pontosabb, biztonságosabb hibakezelést tesz lehetővé. A `pass` a törzsben azt jelenti, hogy az osztály nem ad hozzá semmi újat az `Exception` viselkedéséhez, csak egy megkülönböztethető *típust* hoz létre.

### Gyakorló kérdések

- Mikor fut le a Python `try`-`except` `else` blokkja? *(Csak akkor, ha NEM történt hiba a try blokkban.)*
- Hogyan hozol létre saját kivétel osztályt Pythonban? *(Egy `Exception`-ből örökölt osztállyal.)*

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
print(n.title, n.repo, n.is_read)
```

Miért kell a `__init__` metódus, és miért pont ez a neve? A `__init__` a Python **konstruktora** — amikor `GitHubNotification(...)`-t hívsz, a Python automatikusan létrehoz egy új, üres objektumot, majd meghívja rajta az `__init__`-et, hogy feltöltse a kezdőértékekkel. A dupla aláhúzásos (`__init__`) elnevezés jelzi, hogy ez egy **"dunder" (double underscore) metódus** — a Python egy speciális, a nyelv által automatikusan meghívott metódus-családja, amit *te* implementálsz, de sosem közvetlenül te hívsz meg (`n.__init__(...)`-t nem írsz kézzel).

Miért kell a `self` minden paraméterlistában elsőként? Mert Pythonban a példánymetódusok hívása mögött (`n.is_noise()`) valójában ez történik: `GitHubNotification.is_noise(n)` — vagyis maga az objektum, amin a metódust hívod, mindig átadásra kerül első argumentumként. A `self` ennek a bejövő objektumnak a neve a metóduson belül; ha elhagynád, a metódusnak nem lenne módja elérni a saját `title`, `repo`, `is_read` attribútumait.

**C# összehasonlítás:** a `self` a C# `this`-nek felel meg, de **explicit** — minden példánymetódus első paramétere kötelezően `self`, nem hallgatólagos, mint C#-ban.

### @property — getter és setter Pythonban

Pythonban nincs külön `get`/`set` kulcsszó, mint C#-ban a property-knél — helyette a `@property` dekorátort használjuk, ami egy sima metódust úgy "álcáz", hogy kívülről attribútumként (`objektum.mező`, zárójel nélkül) lehessen elérni, mégis futtatható logika van mögötte (pl. validáció).

```python
class Repo:
    def __init__(self, name: str, stars: int):
        self.name = name
        self._stars = stars   # aláhúzásos név: "belső", ne nyúlj hozzá közvetlenül

    @property
    def stars(self) -> int:          # ez a GETTER
        return self._stars

    @stars.setter
    def stars(self, value: int) -> None:   # ez a SETTER
        if value < 0:
            raise ValueError("A csillagok száma nem lehet negatív")
        self._stars = value

repo = Repo("NexusMind", 10)
print(repo.stars)    # 10 — a getter fut le, de zárójel nélkül hívjuk

repo.stars = 42      # a setter fut le a háttérben, validál is
print(repo.stars)    # 42

try:
    repo.stars = -5  # a setter validációja miatt hibát dob
except ValueError as e:
    print(f"Hiba: {e}")
```

Miért nem elég egyszerűen nyilvános attribútumot (`self.stars = stars`) használni? Azért, mert így nem tudnál validációt vagy egyéb logikát futtatni, amikor valaki *módosítja* az értéket — a `repo.stars = -5` simán átmenne, mert semmi nem állná útját. A `@property`/`@x.setter` páros lehetővé teszi, hogy kívülről ugyanúgy nézzen ki, mint egy sima mező (`repo.stars`, nem `repo.get_stars()`), de belül futhat érvényesítő vagy számított logika. Az aláhúzásos `_stars` konvenció jelzi, hogy ez a "valódi" tárolóhely, amihez kívülről nem szabadna közvetlenül hozzányúlni — csak a `@property`-n és a setteren keresztül.

**C# összehasonlítás:** ez pontosan a C# `public int Stars { get; set; }` (vagy validációval: `get => _stars; set { ...; _stars = value; }`) megfelelője — a szintaxis más, de a cél és a működés azonos: kívülről mezőnek tűnik, belül metódus fut le.

Csak-olvasható property-t úgy kapsz, hogy nem definiálsz hozzá settert:

```python
class Circle:
    def __init__(self, radius: float):
        self.radius = radius

    @property
    def area(self) -> float:   # csak getter, nincs setter -> nem írható felül kívülről
        return 3.14159 * self.radius ** 2

c = Circle(3)
print(c.area)   # 28.27431

try:
    c.area = 100   # AttributeError, mert nincs setter definiálva
except AttributeError as e:
    print(f"Hiba: {e}")
```

### @dataclass — gyors adatosztály boilerplate nélkül

```python
from dataclasses import dataclass

@dataclass
class Repo2:
    name: str
    stars: int = 0

r = Repo2("NexusMind", 5)
print(r)                 # Repo2(name='NexusMind', stars=5) — automatikus __repr__
print(r == Repo2("NexusMind", 5))   # True — automatikus __eq__ mezőnkénti összehasonlítással
```

Ez helyettesíti azt, amit C#-ban kézzel kellene megírnod (konstruktor, `ToString()`, `Equals()`) — a `@dataclass` dekorátor az osztálydefiníció alapján automatikusan legenerálja az `__init__`-et, egy olvasható `__repr__`-t (ez adja a kiíráskor látott formátumot), és egy mezőnkénti `__eq__`-ot. Ezt akkor érdemes választani sima osztály helyett, ha az osztályod célja főleg *adat tárolása* logika nélkül.

### Statikus metódusok és osztálymetódusok

Pythonnak **nincs `static class`** fogalma úgy, ahogy C#-ban. Helyette:

```python
class Validator:
    @staticmethod
    def is_valid_email(email: str) -> bool:
        # nem fér hozzá self-hez, csak "csoportosítás" célból van itt
        return "@" in email

    @classmethod
    def describe(cls) -> str:
        # cls = maga az osztály, nem egy konkrét példány
        return f"Ez az osztály: {cls.__name__}"

print(Validator.is_valid_email("teszt@example.com"))   # True
print(Validator.describe())                             # "Ez az osztály: Validator"
```

Miért van szükség két külön dekorátorra (`@staticmethod` és `@classmethod`)? A `@staticmethod` egy olyan metódus, aminek *semmi* köze az osztály állapotához — sem `self`-t (konkrét példányt), sem `cls`-t (magát az osztályt) nem kapja meg, csak azért van az osztályon belül, mert logikailag odatartozik. A `@classmethod` viszont megkapja magát az osztályt (`cls`), ezért olyankor hasznos, amikor a metódusnak szüksége van tudnia, *melyik* osztályról van szó (pl. alternatív konstruktorokhoz, öröklés esetén is helyesen működve).

```python
class Validator2(Validator):
    pass

print(Validator2.describe())   # "Ez az osztály: Validator2" — a cls a leszármazottra mutat
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

source = GitHubSource()
print(source.fetch())   # [{'title': 'Új issue'}]

try:
    NotificationSource()   # TypeError, mert absztrakt osztályt nem lehet példányosítani
except TypeError as e:
    print(f"Hiba: {e}")
```

Miért `ABC` és `@abstractmethod`, és mi történik, ha ezeket kihagyod? Az `ABC` (Abstract Base Class) és az `@abstractmethod` dekorátor együtt kényszerítik ki, hogy (1) magából a `NotificationSource`-ból ne lehessen közvetlenül példányt létrehozni, és (2) minden leszármazott osztály *köteles* implementálni a `fetch` metódust, különben már az osztály létrehozásakor `TypeError`-t kapsz. Enélkül a Python simán hagyná, hogy létrehozz egy `NotificationSource`-t `fetch()` nélkül, és csak akkor derülne ki a hiba, amikor valaki ténylegesen meghívná a hiányzó metódust — az `ABC` ezt előrébb hozza, fordítás- (pontosabban osztálydefiníciós-) időre.

Ez lesz a NexusMind-ban a "GitHub/Spotify/Steam forrás" közös absztrakciója — tehát ez nem elméleti gyakorlat, hanem valódi tervezési minta, amit élesben is használni fogsz.

A harmadik eszköz, a **`typing.Protocol`** (strukturális interfész — nem kell explicit örökölni belőle, elég ha megvannak a metódusok) inkább haladó, statikus típusellenőrzéshez (`mypy`) hasznos — egyelőre elég, ha tudod, hogy létezik.

### Gyakorló kérdések

- Mi a `self` szerepe egy Python metódusban? *(A C# `this`-nek felel meg, de explicit paraméterként kell mindig szerepelnie.)*
- Hogyan valósítasz meg getter/setter-t Pythonban? *(A `@property` és a hozzá tartozó `@név.setter` dekorátorokkal.)*
- Van-e Pythonnak `static class` kulcsszava? *(Nincs — helyette `@staticmethod`/`@classmethod`, vagy modul-szintű függvények.)*
- Hogyan valósítasz meg "interfészt" Pythonban? *(`abc.ABC` + `@abstractmethod`, vagy `typing.Protocol`.)*

---

## 11. Modulok és csomagkezelés

### Saját modul

```python
# models.py
class Repo:
    def __init__(self, name: str):
        self.name = name

# main.py
from models import Repo

r = Repo("NexusMind")
print(r.name)
```

Egy `.py` fájl automatikusan egy **modul** — nincs szükség explicit "namespace" vagy "package" deklarációra egyetlen fájlhoz, mint C#-ban a `namespace X { }`. A fájlnév (kiterjesztés nélkül) lesz a modul neve, amivel importálod (`models.py` → `from models import ...`).

### Virtuális környezet (venv) — ezt MINDEN projektnél használd

```bash
python -m venv .venv          # létrehozás
source .venv/bin/activate     # aktiválás (Linux/Mac)
.venv\Scripts\activate        # aktiválás (Windows)
pip install httpx pydantic    # csomag telepítése CSAK ebbe a környezetbe
pip freeze > requirements.txt # függőségek listázása
```

Miért kell külön "virtuális környezet", ha C#-ban a NuGet csomagok simán projektenként települnek? Mert Pythonban alapból *egyetlen, gépszintű* Python-telepítés és annak csomagkészlete létezik — ha két projekted különböző verziójú `httpx`-et igényelne, ütköznének. A `venv` egy elszigetelt, projektenkénti másolatot hoz létre a Python-értelmezőből és a csomagtárolóból, hogy a projektek ne zavarják egymást.

**C# összehasonlítás:** ez nagyjából a NuGet-csomagok projektenkénti izolációjának felel meg, csak explicit lépésekkel — Python nem csinálja automatikusan projektenként, neked kell létrehoznod és aktiválnod a venv-et. PyCharm ezt vizuálisan is segíti (Settings → Python Interpreter).

### Gyakorló kérdések

- Mire való a Python `venv`? *(Projektenkénti izolált csomagkörnyezet létrehozására.)*
- Melyik paranccsal hozol létre virtuális környezetet? *(`python -m venv .venv`.)*

---

## 12. Típusosítás és Pydantic

### Type hintek

```python
def average(numbers: list[int]) -> float:
    return sum(numbers) / len(numbers)

age: int = 25
name: str | None = None   # lehet None is (Optional[str] régebbi szintaxissal)

print(average([2, 4, 6]))   # 4.0
print(age, name)
```

Ezek **nem kényszerítik ki** futásidőben a típust (Python futtatáskor nem ellenőrzi!) — dokumentációs célt szolgálnak, és az IDE/`mypy` tudja őket kihasználni hibakereséshez. Ez azt is jelenti, hogy a következő kód *lefut*, hiába "hazudik" a type hint:

```python
def broken_average(numbers: list[int]) -> float:
    return "ez nem is szám"   # type hint szerint float-ot kéne visszaadnia

print(broken_average([1, 2]))   # simán kiírja: ez nem is szám — nincs futásidejű ellenőrzés
```

Ezért van szükség a Pydantic-ra ott, ahol a típushelyesség *ténylegesen* számít (pl. külső API válaszok validálásánál).

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
print(issue)               # title='Bug' number=42 is_open=True
print(issue.number)        # 42, ténylegesen int, nem string

try:
    Issue(title="Rossz", number=-1)
except ValueError as e:
    print(f"Validációs hiba: {e}")
```

Miért kell a `@field_validator` fölé is `@classmethod`? Mert a Pydantic a validátor-függvényt osztályszinten hívja meg (még mielőtt egy konkrét példány létrejönne), így a `cls` (maga az osztály) az első paraméter, nem `self` — ugyanaz a logika, mint a 10. fejezet `@classmethod`-jánál.

**Miért fontos ez a projektben:** a Pydantic modellek lesznek a "nyelvtan" minden gRPC üzenethez, minden külső API-válaszhoz (GitHub, Spotify, Steam) — ezek adják a típusos határfelületet a C# oldal felé, és ellentétben a sima type hintekkel, itt a validáció **ténylegesen lefut** futásidőben.

### Gyakorló kérdések

- Kényszerítik-e ki futásidőben a type hintek a megadott típust? *(Nem, csak dokumentációs/IDE-célt szolgálnak.)*
- Mi a Pydantic `BaseModel` fő előnye a sima `@dataclass`-hoz képest? *(Automatikus validáció és JSON parse/serialize egy lépésben.)*

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
    for r in results:
        print(r["login"], "-", r.get("public_repos"), "repo")

asyncio.run(main())
```

Miért kell az `async def` és az `await`, miért nem lehet ezt sima függvényekkel megírni? Egy `async def`-fel deklarált függvény **korutint** hoz létre — ez egy olyan függvény, ami útközben, az `await` pontokon **átadhatja a vezérlést** más kódnak, amíg egy lassú művelet (pl. hálózati hívás) befejeződik, ahelyett hogy tétlenül várakozna és blokkolná a teljes programot. Az `await client.get(...)` sornál a program "beteszi várólistára" a jelenlegi hívást, és közben más korutinokat futtathat — ezért lehet a `fetch_user("torvalds")` és `fetch_user("gvanrossum")` hívásokat **egyszerre**, egymást nem blokkolva futtatni az `asyncio.gather()`-rel, ahelyett hogy egymás után, sorban várnánk meg mindkettőt.

Az `asyncio.run(main())` az a belépési pont, ami elindítja az **event loop**-ot — ez az a "karmester", ami eldönti, melyik korutin fusson éppen, amikor egy másik `await`-nál vár valamire.

### Három nyelv async-modellje egymás mellett

- **JavaScript/React:** egyszálú event loop, `Promise`, `async/await`.
- **C#:** `Task`, `async/await`, de a háttérben **valódi szálkezelés** is történhet (`Task.Run`) — összetettebb modell.
- **Python:** `asyncio`, `async/await`, **egyszálú event loop** — fogalmilag közelebb áll a JS modellhez, mint a C#-éhoz. Nincs automatikus szálváltás; egy `async` függvény csak `await`-nél adja át a vezérlést, tehát két korutin sosem fut *ténylegesen* párhuzamosan egy processzoron belül, csak "átfedve" várakozik egymásra — ez ideális I/O-kötött feladatokhoz (hálózat, fájl), de nem old meg CPU-intenzív párhuzamosítást.

Ez a hasonlóság (Python ≈ JS az async modellben) megkönnyíti a dolgodat — a React-tapasztalatod jó intuíciót ad.

### Gyakorló kérdések

- Melyik nyelv async-modellje áll közelebb a Pythonhoz: a JavaScript vagy a C#? *(A JavaScript — mindkettő egyszálú event loopot használ.)*
- Mire való az `asyncio.gather()`? *(Több async függvény párhuzamos futtatására.)*

---

## 14. Gyakorlati integráció: valós API-hívás a nulláról

Ez a fejezet összeköti a fenti 13 fejezetet egyetlen, futtatható, valódi példában: `httpx` (hálózati hívás) → Pydantic (validáció) → saját algoritmus (feldolgozás).

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

Miért `Issue(**item)` és nem egyszerűen `Issue(item)`? Mert az `Issue` konstruktora kulcsszavas paramétereket vár (`title=..., number=..., state=...`), az `item` viszont egy `dict`. A `**item` **szétbontja** a szótárat kulcs=érték párokra, mintha te írtad volna ki kézzel `Issue(title=item["title"], number=item["number"], ...)` — ez ugyanaz a `**kwargs`-mechanizmus, amit a 7. fejezetben láttál, csak most fordítva: nem összegyűjtjük, hanem szétterítjük az argumentumokat.

Ha ez lefut nálad — gratula, gyakorlatilag a NexusMind Python-modul első vertikális szeletének "csontváza" kész: valós API + async I/O + Pydantic validáció + saját algoritmus egy helyen.

### Gyakorló kérdések

- Milyen sorrendben kapcsolódnak össze ebben a fejezetben a rétegek? *(httpx (hívás) → Pydantic (validáció) → saját algoritmus (feldolgozás).)*
- Mire való a `**item` szintaxis egy függvényhívásban? *(Szétbontja a szótárat kulcsszavas argumentumokká.)*

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

Futtatás: `pytest` a terminálból, vagy PyCharm-ban közvetlenül a zöld play gombbal a teszt mellett — ugyanaz az élmény, mint a Rider-ben az xUnit tesztekkel. Sikeres futás esetén a `pytest` maga írja ki a terminálba, hány teszt futott le és hány sikerült (pl. `4 passed in 0.01s`) — ezért nincs itt szükség saját `print()`-re, a keretrendszer adja a "kimenetet".

**C# összehasonlítás:**

| xUnit / Moq / Playwright (C#) | pytest (Python) |
|---|---|
| `[Fact]` attribútum | nincs szükség dekorátorra — elég, ha a függvény neve `test_`-tel kezdődik |
| `Assert.Equal(a, b)` | egyszerű `assert a == b` — nincs külön Assert-osztály |
| `[Theory]` + `[InlineData(...)]` paraméteres teszthez | `@pytest.mark.parametrize(...)` dekorátor |
| Moq (`Mock<IService>`) | `unittest.mock` (beépített modul) vagy `pytest-mock` plugin |
| Playwright (C# binding) | Playwright Pythonban **natívan létezik** (`playwright` csomag) |
| `[Fixture]`/konstruktoros setup | `@pytest.fixture` dekorátor |

Miért elég egy sima `assert`, C#-os `Assert.Equal()`-lel szemben? Mert a `pytest` a Python `assert` kulcsszavát belülről "átírja" (introspekció): ha egy `assert` elbukik, a `pytest` nem csak azt írja ki, hogy "hamis volt", hanem a konkrét bal- és jobb oldali értékeket is megmutatja a hibaüzenetben — ezért nincs szükség dedikált `Assert`-osztályra a részletes hibaüzenethez.

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

A `@pytest.mark.parametrize` első argumentuma a paraméterek neve (string, vesszővel elválasztva), a második egy lista a konkrét esetekkel — a `pytest` ebből *külön tesztesetet* generál soronként, így egy teszt-függvényből 3 önálló, egyedileg futtatható/riportolható teszt lesz.

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

Miért `AsyncMock` és nem a sima `Mock`? Mert a `fetch_issues` egy `async` függvényt hív (`await client.get(...)`) — egy sima `Mock` egy közönséges értéket adna vissza, amit nem lehetne `await`-elni, `TypeError`-t okozva. Az `AsyncMock` úgy viselkedik, mintha egy korutin lenne, tehát `await`-elhető, ezért használható async kód mockolására.

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

Egy `@pytest.fixture` egy olyan függvény, amit a `pytest` **automatikusan meghív és befecskendez**, ha egy tesztfüggvény paraméterlistájában szerepel a fixture neve (`sample_issues`) — ez elkerüli, hogy minden tesztben újra és újra lemásold ugyanazt az előkészítő kódot, hasonlóan ahhoz, ahogy egy xUnit konstruktor vagy `[SetUp]` metódus is közös állapotot készít elő a tesztekhez.

**Miért fontos ez a NexusMind-nak:** amikor a gRPC handlerek és a Pydantic modellek elkészülnek, a `pytest` lesz az az eszköz, amivel — pont úgy, ahogy a korábbi projektjeidnél az xUnit-tal — lefeded a Python-modul üzleti logikáját unit tesztekkel, a külső API-hívásokat pedig mockolod, hogy a tesztek ne valódi hálózati hívásoktól függjenek.

### Gyakorló kérdések

- Mi a pytest megfelelője az xUnit `[Fact]` attribútumának? *(Nincs szükség dekorátorra, elég ha a függvény neve `test_`-tel kezdődik.)*
- Mivel helyettesíted Pythonban a Moq mock-objektumait? *(Az `unittest.mock` modullal, pl. `AsyncMock`.)*
- Mire való a `@pytest.fixture`? *(Újrafelhasználható, tesztek közti előkészítő logika biztosítására.)*

---

## 16. Megfelelési táblázat a hivatalos tematikához

| Hivatalos tematika eleme                              | Fejezet |
| ----------------------------------------------------- | ------- |
| Forráskód, fordítás/értelmezés, futtatás              | 1.      |
| IDE megismerése                                       | 1.      |
| Konstansok, változók                                  | 2.      |
| Típusok, operátorok                                   | 2.      |
| Standard I/O                                          | 5.      |
| Vezérlési szerkezetek                                 | 3.      |
| Sztringkezelés                                        | 4.      |
| Alprogramok (függvények)                              | 7.      |
| Tárbeli adatszerkezetek                               | 6.      |
| Algoritmusleíró eszköz (pszeudokód)                   | 8.      |
| Alapvető algoritmusok (összegzés, számlálás, keresés) | 8.      |

A 10–15. fejezetek már túlmutatnak a hivatalos tematikán — ezek a NexusMind projekthez szükséges haladó réteg (OOP, csomagkezelés, típusosítás, async, valós integráció, automatizált tesztelés).

### Gyakorló kérdések

- Melyik fejezet fedi a hivatalos tematika "algoritmusleíró eszköz" pontját? *(A 8. fejezet.)*
- Melyik fejezetek számítanak a hivatalos egyetemi tematikán túlmutató, projekt-specifikus anyagnak? *(A 10–15. fejezetek.)*

---

## Hogyan használd ezt a könyvet

Nem kell lineárisan, egyben végigolvasnod. Javasolt megközelítés:

1. Olvasd át 1–8. fejezetet, és minden fejezetnél futtasd le a kódpéldákat, csináld meg a gyakorlatokat.
2. 9–11. fejezet, amikor a "koszosabb" adatokkal/hibákkal/csomagokkal dolgozó feladatok jönnek.
3. 12–14. fejezet, amikor ténylegesen elkezded a NexusMind Python-modulját építeni.
4. 15. fejezet (pytest), amikor az első Python-modulodhoz már írnál unit teszteket — természetes folytatása annak, amit xUnit/Moq/Playwright-tal a C#-os projektjeidnél már megszoktál.

Ha bármelyik fejezetnél elakadsz, vagy egy konkrét hibaüzenetet nem értesz, hozd ide — együtt átbeszéljük, konkrét kódon keresztül.
