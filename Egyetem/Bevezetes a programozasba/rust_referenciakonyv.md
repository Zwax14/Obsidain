---
tags:
  - rust
  - nexusmind
  - egyetem
---

# Rust referenciakönyv — a nulláról az ownership-ig és tovább

Ugyanaz a formátum, mint a Python- és a C++-könyvben: minden kódrészlet **önmagában futtatható**, a kimenetet mindenhol kiírjuk (`println!`), és mindenhol ott a **"miért így?"** magyarázat — nem csak azt mutatjuk meg, mit írj, hanem hogy *miért pont úgy* működik. A **C# összehasonlítás** végig megvan, és ahol hasznos, a **Python**, a **C++** (és a C/Java) is ott van mellette, mert a leggyorsabb út az, ha a meglévő tudásodra építesz.

A Rust a C++-hoz hasonlóan **natív gépi kódra fordul**, és **nincs szemétgyűjtő (garbage collector)** — viszont a C++-szal ellentétben a memóriabiztonságot **a fordító garantálja**, nem a programozó fegyelme. Ezt az **ownership (tulajdonjog) rendszer** teszi lehetővé, és ez az egyetlen igazán új gondolat, amit a Rustban tanulnod kell. Minden más (változók, ciklusok, függvények, struct-ok) ismerős lesz C#-ból, Pythonból vagy C++-ból, csak a szintaxis más.

Emiatt a könyv két blokkra bomlik: az első fele (1–9. fejezet) az **általános programozási alapok** Rustban, a második fele (10–17. fejezet) pedig a **Rust-specifikus gondolkodásmód**: ownership, borrowing, lifetime-ok, trait-ek, smart pointerek. A 10. fejezetet **ne siesd át**, minden későbbi fogalom arra épül.

> [!tip] A legfontosabb szokás
> A Rust fordítója **kivételesen jó hibaüzeneteket** ad. Mindig olvasd el végig: gyakran a `help:` sorban ott a megoldás is. Egy hibaüzenetet hozz ide, és közösen kibogozzuk.

---

## Tartalomjegyzék

- [[#1. Mi a Rust, és hogyan fordul le / fut le egy program|1. Mi a Rust, és hogyan fordul le / fut le egy program]]
- [[#2. Alaptípusok, változók, operátorok|2. Alaptípusok, változók, operátorok]]
- [[#3. Vezérlési szerkezetek|3. Vezérlési szerkezetek]]
- [[#4. Sztringkezelés|4. Sztringkezelés]]
- [[#5. Standard I/O|5. Standard I/O]]
- [[#6. Adatszerkezetek: tömb, Vec, tuple, HashMap, HashSet|6. Adatszerkezetek: tömb, Vec, tuple, HashMap, HashSet]]
- [[#7. Függvények (alprogramok)|7. Függvények (alprogramok)]]
- [[#8. Algoritmikus gondolkodás és alapalgoritmusok|8. Algoritmikus gondolkodás és alapalgoritmusok]]
- [[#9. Hibakezelés|9. Hibakezelés]]
- [[#10. Ownership és borrowing|10. Ownership és borrowing]]
- [[#11. Lifetime-ok|11. Lifetime-ok]]
- [[#12. Struct, enum, trait (objektumorientált programozás)|12. Struct, enum, trait (objektumorientált programozás)]]
- [[#13. Modulok és csomagkezelés (cargo)|13. Modulok és csomagkezelés (cargo)]]
- [[#14. Generikus programozás|14. Generikus programozás]]
- [[#15. Smart pointerek: Box, Rc, RefCell|15. Smart pointerek: Box, Rc, RefCell]]
- [[#16. Gyakorlati integráció: fájlfeldolgozás a nulláról|16. Gyakorlati integráció: fájlfeldolgozás a nulláról]]
- [[#17. Automatizált tesztelés: cargo test|17. Automatizált tesztelés: cargo test]]
- [[#18. Gyakorló feladatok|18. Gyakorló feladatok]]
    - [[#Feladat 1: Összeg|Feladat 1: Összeg]]
    - [[#Feladat 2: Páros szám|Feladat 2: Páros szám]]
    - [[#Feladat 3: Mi a hiba?|Feladat 3: Mi a hiba?]]
    - [[#Feladat 4: Legnagyobb elem|Feladat 4: Legnagyobb elem]]
    - [[#Feladat 5: Faktoriális (rekurzió)|Feladat 5: Faktoriális (rekurzió)]]
    - [[#Feladat 6: Palindrom|Feladat 6: Palindrom]]
    - [[#Feladat 7: FizzBuzz|Feladat 7: FizzBuzz]]
    - [[#Feladat 8: Borrow checker hiba|Feladat 8: Borrow checker hiba]]
    - [[#Feladat 9: Érvényes számok összege|Feladat 9: Érvényes számok összege]]
    - [[#Feladat 10: Bankszámla (struct és Result)|Feladat 10: Bankszámla (struct és Result)]]
- [[#19. Gyakori fordítói hibák|19. Gyakori fordítói hibák]]
- [[#20. Megfelelési táblázat a hivatalos tematikához|20. Megfelelési táblázat a hivatalos tematikához]]

---

## 1. Mi a Rust, és hogyan fordul le / fut le egy program

A Rust **fordított, natív gépi kódra lefordított** nyelv — pontosan úgy, mint a C++. Nincs bájtkód, nincs virtuális gép, nincs JIT, nincs szemétgyűjtő. A folyamat:

```
forrás.rs  →  [rustc fordító]  →  futtatható program (.exe / bináris)
```

A C++-szal ellentétben **nincs külön linker-lépés, amit neked kellene kezelned**, és nincsenek header fájlok: a `cargo` nevű build-eszköz mindent elintéz helyetted.

Miért fontos ezt tudni? Mert ez magyarázza meg a Rust három jellegzetes tulajdonságát:

- **Nincs futásidejű többletköltség** — a program olyan gyors, mint a C++, mert közvetlenül a processzor hajtja végre, semmilyen köztes réteg nincs.
- **Rengeteg hiba fordítási időben derül ki** — a típushibák mellett a **memóriahibák is** (felszabadított memória használata, adatversenyek). Ezért tűnik a Rust fordítója "szigorúnak": a Python és a C++ ugyanezeket a hibákat csak futás közben (vagy soha) jelzi.
- **Nincs szemétgyűjtő, de nem is kell kézzel `free()`-zni** — a fordító a kódod alapján *előre kiszámolja*, hol szabadítható fel az adat, és beírja a felszabadítást a gépi kódba. Ennek a mechanizmusnak a neve: **ownership** (10. fejezet).

**C# összehasonlítás:** C#-ban `dotnet build` → IL kód → JIT natív kódra futáskor, a CLR felügyelete alatt (ami a garbage collectort is futtatja). Rustban nincs IL, nincs CLR, nincs GC — egyenesen natív kód lesz belőle, egyszer és mindenkorra.

| | Rust | C++ | Python | C# |
|---|---|---|---|---|
| Futtatás módja | natív gépi kód | natív gépi kód | bájtkód + PVM | IL + JIT (CLR) |
| Memóriakezelés | ownership (fordításkor) | kézi / RAII / smart ptr | szemétgyűjtő | szemétgyűjtő |
| Típusosság | statikus | statikus | dinamikus | statikus |
| Build-eszköz | `cargo` | CMake / make | – (`pip`) | `dotnet` |

### Telepítés

**Linux / macOS:**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

**Windows:** töltsd le a telepítőt a https://rustup.rs oldalról.

```bash
rustc --version
cargo --version
```

A **rustup** feltelepíti a fordítót (`rustc`) és a csomagkezelőt/build-eszközt (`cargo`).

### Hogyan fordíts és futtass egy Rust programot

> [!tip] A legkönnyebb módszer
> **Ne használd közvetlenül a `rustc`-t.** Mindig a `cargo`-t használd:
>
> | Parancs | Mit csinál |
> |---|---|
> | `cargo new my_project` | új projekt létrehozása |
> | `cargo run` | lefordítja **és** elindítja |
> | `cargo build` | csak lefordít (debug mód) |
> | `cargo build --release` | optimalizált, gyors verzió |
> | `cargo check` | gyors hibaellenőrzés, nem készít programot |
> | `cargo test` | lefuttatja a teszteket |
> | `cargo add rand` | könyvtár (crate) hozzáadása |

Egy projekt felépítése:

```
my_project/
├── Cargo.toml      ← a projekt leírása és függőségei (mint a .csproj / requirements.txt)
└── src/
    └── main.rs     ← ide írod a kódot, innen indul a program
```

VS Code-ban a **rust-analyzer** bővítmény, vagy a JetBrains **RustRover** adja a kódkiegészítést és a hibajelzést menet közben.

### Az első program

```bash
cargo new hello_nexusmind
cd hello_nexusmind
cargo run
```

A `src/main.rs`:

```rust
fn main() {
    println!("Szia, NexusMind!");
}
```

Kimenet:

```
   Compiling hello_nexusmind v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 0.50s
     Running `target/debug/hello_nexusmind`
Szia, NexusMind!
```

Soronként:

| Kódrész | Jelentés |
|---|---|
| `fn` | függvényt definiálunk (*function*) |
| `main` | a program **belépési pontja**, itt indul minden (mint a C++ `main`, a C# `Main`) |
| `println!` | kiír egy sort. A `!` azt jelzi, hogy ez **makró**, nem sima függvény |
| `;` | az utasítás vége |

Miért makró a `println!`? Mert a Rustban nincs változó paraméterszámú függvény (mint a C `printf`-je), a makró viszont **fordítási időben** ellenőrzi a formátumot és az argumentumokat — így a C-s `printf` típusú hibák (rossz `%d`) itt nem fordulnak le.

⚠️ **Csapda:** `println` és `println!` **nem ugyanaz**. A felkiáltójel nélkül hibát kapsz.

### Gyakorló kérdések

- Mit csinál a `cargo run`? *(Lefordítja és elindítja a programot.)*
- Miért nem kell szemétgyűjtő a Rustnak? *(Mert a fordító az ownership szabályok alapján fordítási időben eldönti, mikor kell felszabadítani az adatot.)*
- Mi a különbség `cargo check` és `cargo build` között? *(A `check` csak ellenőriz, nem készít futtatható programot, ezért sokkal gyorsabb.)*

---

## 2. Alaptípusok, változók, operátorok

### Statikus típusosság és típuskövetkeztetés

A Rust **statikusan típusos**: minden változónak fordítási időben rögzített típusa van, mint C++-ban és C#-ban. A fordító viszont **kikövetkezteti** a típust, ezért nem kell mindig kiírnod (mint a C++ `auto`, a C# `var`):

```rust
fn main() {
    let x = 5;               // a fordító: i32
    let y: i64 = 5;          // explicit típus
    println!("{} {}", x, y);
}
```

Kimenet:

```
5 5
```

### Változók: alapból NEM módosíthatók

Ez az első nagy különbség. Rustban a változó **alapértelmezetten megváltoztathatatlan** (immutable):

```rust
fn main() {
    let x = 5;
    println!("{}", x);        // 5

    // x = 10;   // FORDÍTÁSI HIBA! cannot assign twice to immutable variable

    let mut counter = 0;
    counter += 1;
    counter += 1;
    println!("{}", counter);  // 2
}
```

Kimenet:

```
5
2
```

Miért így? Mert a legtöbb hiba abból ered, hogy valami **véletlenül megváltozik**. A Rust arra kényszerít, hogy ezt **kimondd**: `mut` (*mutable*). Aki a kódot olvassa, a `let mut` láttán rögtön tudja: ez a változó később módosulni fog.

**C# / C++ összehasonlítás:** C#-ban és C++-ban minden változó alapból módosítható, a `readonly` / `const` a kivétel. Rustban **fordítva**: a módosíthatóság a kivétel.

### Shadowing (árnyékolás)

```rust
fn main() {
    let spaces = "   ";
    let spaces = spaces.len();   // új változó, ugyanazzal a névvel, MÁS típussal
    println!("{}", spaces);      // 3
}
```

Kimenet:

```
3
```

A `let` újra kiadva **új változót** hoz létre, nem módosítja a régit — ezért a típus is változhat. A `mut` nem engedi a típusváltást (`let mut spaces = "   "; spaces = spaces.len();` hiba).

### Alaptípusok

```rust
fn main() {
    let age: i32 = 25;               // előjeles egész, 4 bájt (alapértelmezett egész típus)
    let price: f64 = 19.99;          // dupla pontosságú lebegőpontos (alapértelmezett)
    let price_f: f32 = 19.99;        // egyszeres pontosságú
    let grade: char = 'A';           // EGY Unicode karakter, szimpla idézőjel
    let is_active: bool = true;
    let name: &str = "NexusMind";    // szövegszelet, dupla idézőjel (lásd 4. fejezet)
    let owned_name: String = String::from("NexusMind");

    println!("{} {} {} {} {} {} {}", age, price, price_f, grade, is_active, name, owned_name);
}
```

Kimenet:

```
25 19.99 19.99 A true NexusMind NexusMind
```

Az egész típusok neve a **méretet és az előjelet** is mutatja:

| Típus | Méret | Tartomány |
|---|---|---|
| `i8` / `u8` | 1 bájt | -128…127 / 0…255 |
| `i16` / `u16` | 2 bájt | ±32 767 / 0…65 535 |
| `i32` / `u32` | 4 bájt | ≈ ±2,1 milliárd / 0…≈4,29 milliárd |
| `i64` / `u64` | 8 bájt | nagyon nagy |
| `i128` / `u128` | 16 bájt | extrém nagy |
| `isize` / `usize` | gépfüggő (64 bit gépen 8 bájt) | **indexeléshez és hosszakhoz** használjuk |

`i` = előjeles (*integer*), `u` = előjel nélküli (*unsigned*). Ez ugyanaz a rendszer, mint a C `<stdint.h>` típusai (`int32_t`, `uint8_t`), csak rövidebben, és **mindig garantált a méret** (a C `int`-jével ellentétben).

⚠️ **Fontos csapda:** a `char` Rustban **4 bájtos Unicode karakter** (nem 1 bájtos, mint C-ben/C++-ban), és `'A'` szimpla idézőjellel jelöljük. A `"A"` már szöveg (`&str`).

### Konstansok

```rust
const MAX_RETRY_COUNT: u32 = 5;     // a típus megadása KÖTELEZŐ, a név NAGYBETŰS
const SECONDS_PER_DAY: u32 = 24 * 60 * 60;   // fordítási időben kiszámolt kifejezés lehet

fn main() {
    println!("{} {}", MAX_RETRY_COUNT, SECONDS_PER_DAY);
}
```

Kimenet:

```
5 86400
```

A `const` **mindig fordítási időben kiszámolható** (hasonlóan a C++ `constexpr`-hez), és **valódi, fordító által kikényszerített** védelem — ellentétben Pythonnal, ahol a nagybetűs név csak konvenció.

### Operátorok

| Kategória | Rust | C++ | Python | C# |
|---|---|---|---|---|
| Aritmetikai | `+ - * / %` | ugyanaz | `+ - * / // %` | ugyanaz |
| Egész osztás | `/` (ha mindkettő egész) | `/` | `//` | `/` |
| Hatványozás | `.pow()` / `.powi()` / `.powf()` | `std::pow()` | `**` | `Math.Pow()` |
| Logikai | `&& \|\| !` | `&& \|\| !` | `and or not` | `&& \|\| !` |
| Növelés | `+= 1` (**nincs** `++`/`--`) | `++ --` | `+= 1` | `++ --` |
| Bitműveletek | `& \| ^ ! << >>` | `& \| ^ ~ << >>` | `& \| ^ ~ << >>` | `& \| ^ ~ << >>` |

```rust
fn main() {
    let a: i32 = 7;
    let b: i32 = 2;

    println!("{}", a / b);                // 3   — egész osztás
    println!("{}", a as f64 / b as f64);  // 3.5 — explicit típuskonverzió az `as` kulcsszóval
    println!("{}", a % b);                // 1

    println!("{}", 2_i32.pow(10));        // 1024
    println!("{}", 16.0_f64.sqrt());      // 4
}
```

Kimenet:

```
3
3.5
1
1024
4
```

Miért `4` és nem `4.0` az utolsó? Mert a `{}` (Display) a törtszámot "szépen" írja ki, a felesleges `.0`-t elhagyja. Ha a pontos alakot akarod, használd a `{:?}` (Debug) formát: `println!("{:?}", 16.0_f64.sqrt());` → `4.0`.

⚠️ **A C++ egyik legveszélyesebb csapdája itt kizárt.** C++-ban `double result = 7 / 2;` csendben `3`-at ad. Rustban:

```rust
// let a: i32 = 7;
// let b: i32 = 2;
// let result: f64 = a / b;   // FORDÍTÁSI HIBA: mismatched types (expected `f64`, found `i32`)
```

Miért? Mert a Rust **soha nem alakít át automatikusan** típusokat (sem `i32` → `i64`, sem `i32` → `f64`). Ha konvertálni akarsz, ki kell írnod: `as`. Ez egy kicsit több gépelés, de nincs "csendes" adatvesztés.

### Túlcsordulás (overflow)

```rust
fn main() {
    let max_byte: u8 = 255;

    println!("{:?}", max_byte.checked_add(1));    // None  — jelzi, hogy nem fért bele
    println!("{}", max_byte.wrapping_add(1));     // 0     — körbefordul
    println!("{}", max_byte.saturating_add(1));   // 255   — megáll a maximumnál
}
```

Kimenet:

```
None
0
255
```

Ha sima `max_byte + 1`-et írnál, **debug módban a program összeomlik** (`attempt to add with overflow`), release módban körbefordul. A fenti metódusokkal **te döntöd el**, mi történjen — nincs csendes meglepetés.

### Kiírás formázása

```rust
fn main() {
    let pi = 3.14159265;
    println!("{:.2}", pi);          // 3.14        két tizedes
    println!("{:8.3}|", pi);        //    3.142|   8 széles, 3 tizedes
    println!("{:05}", 42);          // 00042       nullákkal kitöltve
    println!("{:>6}|{:<6}|", "ab", "cd");   //     ab|cd    |
    println!("{:b} {:x}", 10, 255); // 1010 ff     kettes és tizenhatos számrendszer

    let numbers = vec![1, 2, 3];
    println!("{:?}", numbers);      // [1, 2, 3]   Debug formátum
}
```

Kimenet:

```
3.14
   3.142|
00042
    ab|cd    |
1010 ff
[1, 2, 3]
```

| Jelölés | Jelentés |
|---|---|
| `{}` | Display — "emberbarát" kiírás |
| `{:?}` | Debug — programozói kiírás (listák, struct-ok) |
| `{:#?}` | Debug, szépen tördelve |
| `{:.2}` | két tizedesjegy |
| `{:>8}` / `{:<8}` | jobbra / balra igazítás 8 szélességben |

### Gyakorló kérdések

- Mi történik, ha egy `let x = 5;` változónak később értéket adsz? *(Fordítási hiba, mert alapból nem módosítható; `let mut` kell.)*
- Mi a különbség a `mut` és a shadowing között? *(A shadowing új változót hoz létre, ezért a típus is változhat; a `mut` ugyanazt a változót módosítja, a típus marad.)*
- Miért nem fordul le a `let result: f64 = 7 / 2;`? *(A Rust nem alakít át automatikusan típust: az `i32` osztás eredménye `i32`, `f64` kellene.)*
- Mi a különbség `u8` és `i8` között? *(Az `u8` előjel nélküli: 0…255, az `i8` előjeles: -128…127.)*

---

## 3. Vezérlési szerkezetek

### if / else if / else

```rust
fn main() {
    let temp = 22;

    if temp > 30 {
        println!("hot");
    } else if temp > 15 {
        println!("pleasant");
    } else {
        println!("cold");
    }

    // az if KIFEJEZÉS: értéket ad vissza
    let label = if temp > 20 { "warm" } else { "cool" };
    println!("{}", label);
}
```

Kimenet:

```
pleasant
warm
```

Miért `pleasant`? A `22 > 30` hamis, a `22 > 15` igaz → ez az ág fut le. Az `if` **kifejezés**, ezért nincs szükség C-s `? :` ternáris operátorra: az `if ... else ...` önmagában értéket ad. A két ág típusának **meg kell egyeznie**.

⚠️ **Nincs "truthiness":** a feltételnek **kizárólag `bool`** lehet. `if 1 { }` vagy `if name { }` hibát ad — Pythonban működne, C-ben és C++-ban is. A zárójel a feltétel körül **nem kell** (és a Rust stílus szerint nem is írunk).

### for ciklus

```rust
fn main() {
    for i in 0..3 {
        println!("i = {}", i);
    }

    let evens: Vec<i32> = (0..10).step_by(3).collect();
    println!("{:?}", evens);                  // [0, 3, 6, 9]

    let countdown: Vec<i32> = (0..3).rev().collect();
    println!("{:?}", countdown);              // [2, 1, 0]

    let names = ["Anna", "Bela"];
    for (index, name) in names.iter().enumerate() {
        println!("{}: {}", index, name);
    }
}
```

Kimenet:

```
i = 0
i = 1
i = 2
[0, 3, 6, 9]
[2, 1, 0]
0: Anna
1: Bela
```

| Tartomány | Elemek |
|---|---|
| `0..5` | 0, 1, 2, 3, 4 (az 5 **nincs** benne) |
| `0..=5` | 0, 1, 2, 3, 4, 5 (az 5 **benne van**) |

Nincs C-stílusú `for (int i = 0; i < n; i++)` — csak a **bejárás** van (mint Pythonban a `for ... in`). Ez kizárja az "eggyel elcsúszott index" típusú hibákat, és a `.enumerate()` megadja az indexet is, ha kell (Python `enumerate`).

### while és loop

```rust
fn main() {
    let mut countdown = 3;
    while countdown > 0 {
        println!("{}", countdown);
        countdown -= 1;
    }
    println!("Go!");

    // a `loop` végtelen ciklus; a `break` értéket is visszaadhat
    let mut attempts = 0;
    let result = loop {
        attempts += 1;
        if attempts == 3 {
            break attempts * 10;
        }
    };
    println!("result = {}", result);
}
```

Kimenet:

```
3
2
1
Go!
result = 30
```

A `while` levezetése:

| Kör | `countdown` | `> 0`? | Kiírás | Utána |
|---|---|---|---|---|
| 1 | 3 | igen | 3 | 2 |
| 2 | 2 | igen | 2 | 1 |
| 3 | 1 | igen | 1 | 0 |
| – | 0 | nem | kilép | – |

Nincs `do-while`; a `loop` + `break` helyettesíti.

### break, continue, címkézett break

```rust
fn main() {
    for i in 0..6 {
        if i == 4 { break; }          // kilép a ciklusból
        if i % 2 == 0 { continue; }   // ugrás a következő körre
        println!("{}", i);
    }

    'outer: for i in 0..3 {
        for j in 0..3 {
            if i * j == 2 {
                println!("found {} {}", i, j);
                break 'outer;         // a KÜLSŐ ciklusból lép ki
            }
        }
    }
}
```

Kimenet:

```
1
3
found 1 2
```

### match — a `switch` erősebb változata

```rust
fn main() {
    let score = 85;

    let grade = match score {
        90..=100 => 'A',
        80..=89 => 'B',
        70..=79 => 'C',
        _ => 'F',          // a `_` mindent elkap, ami még nem szerepelt
    };
    println!("{}", grade);   // B
}
```

Kimenet:

```
B
```

A `match` három dologban jobb a `switch`-nél:

- **kimerítő (exhaustive)**: minden lehetséges esetet le kell fedned, különben a program nem fordul le;
- **nincs fall-through**: nem kell `break`, csak az egyező ág fut le;
- **kifejezés**: értéket ad vissza, és tartományokat, mintákat is használhatsz.

### Gyakorló kérdések

- Miért nem működik az `if 1 { }`? *(A feltételnek `bool`-nak kell lennie, a Rust nem alakít át számot logikai értékké.)*
- Mit jelent a `0..=5`? *(0-tól 5-ig, az 5-tel együtt.)*
- Mi történik, ha a `match`-ből kihagysz egy esetet? *(Fordítási hiba: non-exhaustive patterns.)*

---

## 4. Sztringkezelés

Ez az első hely, ahol a Rust furcsának tűnhet, mert **kétféle string típus** van:

| Típus | Mi ez | Hol van az adat | Módosítható? |
|---|---|---|---|
| `&str` | **string szelet** — egy szövegre mutató "nézet" | bárhol (pl. a programba égetett literál) | nem |
| `String` | **saját tulajdonú** szöveg | heap | igen (`mut`-tal) |

```rust
fn main() {
    let literal: &str = "hello";
    let mut owned: String = String::from("hello");
    owned.push_str(", world");
    owned.push('!');
    println!("{} | {}", literal, owned);
}
```

Kimenet:

```
hello | hello, world!
```

Miért van két típus? Mert a Rust **ownership** rendszerében külön kérdés, hogy *ki birtokolja* a szöveget (`String`), és hogy valaki csak *belenéz* (`&str`). Függvény paraméterben szinte mindig `&str`-t használj — az fogad `String`-et és literált is.

**C++ összehasonlítás:** a `String` ≈ `std::string`, a `&str` ≈ `std::string_view`. **C#/Python:** ott egyetlen `string` / `str` van, és immutable.

### UTF-8: a hossz bájtban értendő!

```rust
fn main() {
    let word = "szép";

    println!("{}", word.len());              // 5  — BÁJT
    println!("{}", word.chars().count());    // 4  — karakter

    // println!("{}", word[0]);              // FORDÍTÁSI HIBA: a string nem indexelhető
    println!("{}", &word[0..2]);             // sz — bájtok szerinti szeletelés

    let reversed: String = word.chars().rev().collect();
    println!("{}", reversed);                // pézs
}
```

Kimenet:

```
5
4
sz
pézs
```

Miért `5` a "szép" hossza? A Rust stringek **UTF-8**-ban tárolódnak: az `s`, `z`, `p` 1 bájt, az `é` 2 bájt → összesen 5 bájt. Ezért a `word[0]` nem is létezik: nem derülne ki, hogy *karaktert* vagy *bájtot* kérsz. A `&word[0..2]` bájt-tartományt szeletel, és **összeomlik**, ha egy karakter közepébe vágsz (pl. `&word[0..3]` az `é` közepébe esne).

| Művelet | Python | Rust |
|---|---|---|
| Hossz (karakter) | `len(s)` | `s.chars().count()` |
| Hossz (bájt) | `len(s.encode())` | `s.len()` |
| Szelet | `s[0:2]` | `&s[0..2]` (bájtok!) |
| Megfordítás | `s[::-1]` | `s.chars().rev().collect::<String>()` |

### Összefűzés és formázás

```rust
fn main() {
    let first = String::from("Nexus");
    let second = String::from("Mind");

    let combined = format!("{}{}", first, second);   // nem "viszi el" a bemeneteket
    println!("{} {} {}", first, second, combined);

    let joined = first + &second;   // a `first` ITT ELMOZDUL (move), többé nem használható
    println!("{}", joined);
}
```

Kimenet:

```
Nexus Mind NexusMind
NexusMind
```

A `format!` ugyanaz, mint a `println!`, csak nem kiírja, hanem `String`-ként visszaadja (Python f-string megfelelője). A `+` operátor viszont a bal oldali `String`-et **elfogyasztja** — ennek az oka a 10. fejezetben derül ki.

### Gyakori metódusok

```rust
fn main() {
    let text = "  Rust is fast  ";
    let trimmed = text.trim();

    println!("{}", trimmed.to_uppercase());           // RUST IS FAST
    println!("{}", trimmed.contains("fast"));         // true
    println!("{}", trimmed.replace("fast", "safe"));  // Rust is safe
    println!("{}", trimmed.starts_with("Rust"));      // true

    let parts: Vec<&str> = trimmed.split(' ').collect();
    println!("{:?}", parts);                          // ["Rust", "is", "fast"]

    println!("{:?}", "42".parse::<i32>());            // Ok(42)
    println!("{:?}", "abc".parse::<i32>());           // Err(ParseIntError { kind: InvalidDigit })
}
```

Kimenet:

```
RUST IS FAST
true
Rust is safe
true
["Rust", "is", "fast"]
Ok(42)
Err(ParseIntError { kind: InvalidDigit })
```

A `parse` **nem dob kivételt**, hanem `Result`-ot ad vissza: `Ok(érték)` vagy `Err(hiba)`. Ez a Rust hibakezelésének alapja (9. fejezet).

### Gyakorló kérdések

- Mi a különbség `String` és `&str` között? *(A `String` saját tulajdonú, módosítható, heap-en élő szöveg; a `&str` csak egy nézet egy szövegre.)*
- Miért 5 a `"szép".len()` értéke? *(A `len()` bájtot számol, és az `é` UTF-8-ban 2 bájt.)*
- Miért nem működik a `word[0]`? *(A Rust nem engedi a stringek indexelését, mert nem egyértelmű, hogy bájtot vagy karaktert jelent.)*

---

## 5. Standard I/O

### Kimenet

```rust
fn main() {
    println!("with newline");
    print!("no newline, ");
    print!("same line\n");
    eprintln!("this goes to stderr");   // hibakimenet (stderr)
}
```

Kimenet (stdout):

```
with newline
no newline, same line
```

### Bemenet

```rust
use std::io;

fn main() {
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("failed to read line");

    let number: i32 = input.trim().parse().expect("not a number");
    println!("{}", number * 2);
}
```

Ha a felhasználó beírja, hogy `21`, a kimenet:

```
42
```

Miért ennyi lépés?

- `String::new()` — üres, **módosítható** String, mert a `read_line` *ebbe* fogja beleírni a sort.
- `&mut input` — a `read_line` megkapja a változót **módosítható hivatkozásként** (kölcsönadjuk neki, de nem adjuk át a tulajdonjogot) — ezt a 10. fejezet magyarázza el.
- `.trim()` — a `read_line` a sorvége karaktert (`\n`) **is** beolvassa, azt le kell vágni, különben a `parse` hibát ad.
- `.parse()` → `Result`, az `.expect("...")` pedig hiba esetén üzenettel leállítja a programot.

### Több szám egy sorból

```rust
use std::io;

fn main() {
    let mut line = String::new();
    io::stdin().read_line(&mut line).expect("failed to read line");

    let numbers: Vec<i32> = line
        .split_whitespace()
        .map(|token| token.parse().expect("not a number"))
        .collect();

    let total: i32 = numbers.iter().sum();
    println!("{:?} sum = {}", numbers, total);
}
```

Bemenet `3 4 5` esetén:

```
[3, 4, 5] sum = 12
```

### Gyakorló kérdések

- Miért kell `.trim()` a `read_line` után? *(Mert a beolvasott szöveg a sorvége karaktert is tartalmazza.)*
- Mit jelent a `&mut input`? *(Módosítható hivatkozás: a függvény módosíthatja a változót, de nem veszi át a tulajdonjogot.)*

---

## 6. Adatszerkezetek: tömb, Vec, tuple, HashMap, HashSet

### Tömb (fix méret)

```rust
fn main() {
    let fixed: [i32; 3] = [10, 20, 30];
    let zeros = [0; 5];                        // öt darab 0

    println!("{} {}", fixed[0], fixed.len());  // 10 3
    println!("{:?}", zeros);                   // [0, 0, 0, 0, 0]
}
```

Kimenet:

```
10 3
[0, 0, 0, 0, 0]
```

A tömb mérete **a típus része** (`[i32; 3]`), fordítási időben rögzített, és a stack-en él. Ritkán használod; helyette a `Vec`.

### Vec — a "munkaló" adatszerkezeted

A `Vec<T>` a dinamikus tömb, a Python `list`, a C++ `std::vector`, a C# `List<T>` megfelelője:

```rust
fn main() {
    let mut numbers: Vec<i32> = Vec::new();   // vagy: vec![1, 2, 3]
    numbers.push(1);
    numbers.push(2);
    numbers.push(3);

    println!("{:?} len={}", numbers, numbers.len());   // [1, 2, 3] len=3
    println!("{}", numbers[1]);                        // 2
    println!("{:?}", numbers.get(10));                 // None
    println!("{:?}", numbers.pop());                   // Some(3)
    println!("{:?}", numbers);                         // [1, 2]
}
```

Kimenet:

```
[1, 2, 3] len=3
2
None
Some(3)
[1, 2]
```

Miért `None` a `numbers.get(10)`? Mert a `get` **biztonságos**: ha nincs ilyen elem, `None`-t ad vissza. A `numbers[10]` viszont **összeomlik** (`index out of bounds: the len is 3 but the index is 10`) — ellentétben a C-vel, ahol *undefined behavior* lenne, és a C++ `operator[]`-vel, ami szintén nem ellenőriz.

### Bejárás háromféleképpen

```rust
fn main() {
    let mut numbers = vec![1, 2, 3];

    for n in &numbers {            // 1. csak OLVASÁS: kölcsönveszi
        print!("{} ", n);
    }
    println!();

    for n in &mut numbers {        // 2. MÓDOSÍTÁS: módosítható kölcsönvétel
        *n *= 2;
    }
    println!("{:?}", numbers);

    for n in numbers {             // 3. ELFOGYASZTÁS: a Vec elmozdul, utána nem használható
        print!("{} ", n);
    }
    println!();
}
```

Kimenet:

```
1 2 3 
[2, 4, 6]
2 4 6 
```

Ez a rész előre vetíti a 10. fejezetet: a három forma a három tulajdonjogi viszony (`&`, `&mut`, érték). A `*n` a hivatkozás **dereferálása** (mint a C mutatónál).

### Iterátor-láncok (a list comprehension / LINQ megfelelője)

```rust
fn main() {
    let squares_of_evens: Vec<i32> = (1..=6)
        .filter(|n| n % 2 == 0)
        .map(|n| n * n)
        .collect();

    println!("{:?}", squares_of_evens);   // [4, 16, 36]
}
```

Kimenet:

```
[4, 16, 36]
```

| Rust | Python | C# (LINQ) |
|---|---|---|
| `.filter(\|n\| ...)` | `if ...` a comprehension végén | `.Where(n => ...)` |
| `.map(\|n\| ...)` | a kifejezés a comprehension elején | `.Select(n => ...)` |
| `.collect()` | `[...]` | `.ToList()` |

Az iterátorok **lusták** (a `collect()` hívásig nem történik semmi), és a fordító **ugyanolyan gyors gépi kódra optimalizálja**, mint a kézzel írt ciklust (*zero-cost abstraction*).

### Tuple

```rust
fn main() {
    let person: (&str, i32) = ("Anna", 25);
    let (name, age) = person;                  // szétbontás (destructuring)
    println!("{} {} {}", name, age, person.0);
}
```

Kimenet:

```
Anna 25 Anna
```

### HashMap — kulcs-érték párok

```rust
use std::collections::HashMap;

fn main() {
    let mut ages: HashMap<String, i32> = HashMap::new();
    ages.insert(String::from("Anna"), 25);
    ages.insert(String::from("Bela"), 30);

    println!("{:?}", ages.get("Anna"));         // Some(25)
    println!("{:?}", ages.get("Cili"));         // None
    println!("{}", ages.contains_key("Bela"));  // true

    let age = ages.get("Cili").copied().unwrap_or(0);   // alapértelmezett érték, ha nincs
    println!("{}", age);                                // 0
}
```

Kimenet:

```
Some(25)
None
true
0
```

A `get` mindig `Option`-t ad vissza — **nincs `KeyError`** mint Pythonban, és nincs véletlen `null` mint C#-ban. A fordító kényszerít, hogy kezeld a "nincs ilyen kulcs" esetet.

**Gyakoriság-számolás az `entry` API-val:**

```rust
use std::collections::BTreeMap;

fn main() {
    let text = "a b a c b a";
    let mut counts: BTreeMap<&str, i32> = BTreeMap::new();

    for word in text.split_whitespace() {
        *counts.entry(word).or_insert(0) += 1;
    }
    println!("{:?}", counts);
}
```

Kimenet:

```
{"a": 3, "b": 2, "c": 1}
```

Levezetés: az `entry(word)` megnézi, van-e már ilyen kulcs; az `or_insert(0)` ha nincs, beteszi 0-val, és **hivatkozást ad** az értékre; a `*... += 1` ezt növeli. A `BTreeMap` kulcs szerint rendezett — a `HashMap` bejárási sorrendje **nem determinisztikus**, ezért a kiírás sorrendje futásról futásra változhat.

### HashSet — egyedi elemek

```rust
use std::collections::HashSet;

fn main() {
    let mut unique: HashSet<i32> = HashSet::new();
    unique.insert(1);
    unique.insert(2);
    unique.insert(2);                      // már benne van, nem kerül be még egyszer

    println!("{}", unique.len());          // 2
    println!("{}", unique.contains(&1));   // true
}
```

Kimenet:

```
2
true
```

### Mátrix (2D adatszerkezet)

```rust
fn main() {
    let matrix = vec![vec![1, 2, 3], vec![4, 5, 6]];
    println!("{}", matrix[1][2]);     // 6

    let grid = vec![vec![0; 3]; 2];   // 2 sor, 3 oszlop, MINDEN sor külön másolat
    println!("{:?}", grid);           // [[0, 0, 0], [0, 0, 0]]
}
```

Kimenet:

```
6
[[0, 0, 0], [0, 0, 0]]
```

A Python-ban hírhedt `[[0] * 3] * 2` aliasing-hiba itt **nem létezik**: a `vec![vec![0; 3]; 2]` minden sorból külön másolatot készít.

### Gyakorló kérdések

- Mi a különbség `numbers[10]` és `numbers.get(10)` között? *(Az indexelés összeomlik, ha nincs ilyen elem; a `get` `None`-t ad vissza.)*
- Mi a három módja a Vec bejárásának? *(`&v` olvasás, `&mut v` módosítás, `v` elfogyasztás.)*
- Mit ad vissza a `HashMap::get`? *(`Option`-t: `Some(érték)` vagy `None`.)*

---

## 7. Függvények (alprogramok)

```rust
fn add(first: i32, second: i32) -> i32 {
    first + second        // NINCS pontosvessző: ez a visszatérési érték
}

fn greet(name: &str) {    // nincs `->`: a visszatérési típus `()` (üres, mint a void)
    println!("Hello, {}!", name);
}

fn main() {
    println!("{}", add(3, 4));
    greet("Anna");
}
```

Kimenet:

```
7
Hello, Anna!
```

Miért így?

- A paraméterek típusát **mindig ki kell írni** (nincs típuskövetkeztetés a függvény határán) — így a függvény "szerződése" a fejlécből kiolvasható.
- Az **utolsó kifejezés** a visszatérési érték, ha nincs mögötte `;`. A korai kilépéshez a `return` kell.

⚠️ **A leggyakoribb csapda: a pontosvessző.**

```rust
// fn add(first: i32, second: i32) -> i32 {
//     first + second;     // HIBA: mismatched types — a `;` eldobja az értéket, a függvény `()`-t adna vissza
// }
```

A fordító ezt szerencsére meg is mondja: *"remove this semicolon"*.

### Több érték visszaadása (tuple)

```rust
fn min_max(numbers: &[i32]) -> (i32, i32) {
    let mut min_value = numbers[0];
    let mut max_value = numbers[0];
    for &n in numbers {
        if n < min_value { min_value = n; }
        if n > max_value { max_value = n; }
    }
    (min_value, max_value)
}

fn main() {
    let (low, high) = min_max(&[3, 9, 2, 7]);
    println!("{} {}", low, high);
}
```

Kimenet:

```
2 9
```

Levezetés: kezdetben `min = max = 3`. A `9` nagyobb → `max = 9`. A `2` kisebb → `min = 2`. A `7` egyiket sem érinti. Eredmény: `(2, 9)`.

### Nincs túlterhelés és nincs alapértelmezett paraméter

A Rustban **nincs függvény-túlterhelés** (overloading) és **nincsenek alapértelmezett argumentumok** — ellentétben a C++-szal és a C#-pal (és Pythonnal, ahol az alapértelmezett paraméter van). A helyette használt eszközök: különböző nevek (`from_str`, `from_int`), `Option` paraméter, **generikusok és trait-ek** (14. fejezet).

### Closure-ök (névtelen függvények)

```rust
fn apply_twice(function: impl Fn(i32) -> i32, value: i32) -> i32 {
    function(function(value))
}

fn main() {
    let double = |x: i32| x * 2;
    println!("{}", double(21));                // 42

    let offset = 10;
    let add_offset = |x: i32| x + offset;      // a környezetből "elkapja" az `offset`-et
    println!("{}", add_offset(5));             // 15

    println!("{}", apply_twice(double, 5));    // 20
}
```

Kimenet:

```
42
15
20
```

A closure a Python `lambda`-ja, a C# lambda kifejezése, a C++ `[](int x){...}`. Levezetés az utolsó sorra: `double(5) = 10`, majd `double(10) = 20`.

### Gyakorló kérdések

- Miért hibás az `a + b;` a függvény végén? *(A pontosvessző eldobja az értéket, így a függvény `()`-t adna vissza az `i32` helyett.)*
- Hogyan adhatsz vissza több értéket? *(Tuple-lel: `-> (i32, i32)`.)*
- Van függvény-túlterhelés Rustban? *(Nincs; generikusokkal és trait-ekkel oldjuk meg.)*

---

## 8. Algoritmikus gondolkodás és alapalgoritmusok

### Pszeudokód-minta

```
ÖSSZEGZÉS(lista):
    összeg := 0
    minden elem-re a listában:
        összeg := összeg + elem
    VISSZAAD összeg
```

### A négy alapalgoritmus Rustban

```rust
fn sum_manual(numbers: &[i32]) -> i32 {
    let mut total = 0;
    for &n in numbers {
        total += n;
    }
    total
}

fn count_evens(numbers: &[i32]) -> usize {
    let mut count = 0;
    for &n in numbers {
        if n % 2 == 0 {
            count += 1;
        }
    }
    count
}

fn linear_search(numbers: &[i32], target: i32) -> Option<usize> {
    for (index, &n) in numbers.iter().enumerate() {
        if n == target {
            return Some(index);
        }
    }
    None
}

fn bubble_sort(numbers: &mut [i32]) {
    let length = numbers.len();
    for i in 0..length {
        for j in 0..length - 1 - i {
            if numbers[j] > numbers[j + 1] {
                numbers.swap(j, j + 1);
            }
        }
    }
}

fn main() {
    let numbers = vec![5, 2, 8, 1, 9, 4];

    println!("{}", sum_manual(&numbers));            // 29
    println!("{}", count_evens(&numbers));           // 3
    println!("{:?}", linear_search(&numbers, 8));    // Some(2)
    println!("{:?}", linear_search(&numbers, 99));   // None

    let mut to_sort = numbers.clone();
    bubble_sort(&mut to_sort);
    println!("{:?}", to_sort);                       // [1, 2, 4, 5, 8, 9]
    println!("{:?}", numbers);                       // [5, 2, 8, 1, 9, 4] — az eredeti érintetlen
}
```

Kimenet:

```
29
3
Some(2)
None
[1, 2, 4, 5, 8, 9]
[5, 2, 8, 1, 9, 4]
```

Levezetések:

- **Összeg:** 5+2=7, +8=15, +1=16, +9=25, +4=**29**.
- **Páros számláló:** a 2, 8 és 4 páros → **3**.
- **Keresés a 8-ra:** a 0. elem 5, az 1. elem 2, a 2. elem **8** → `Some(2)`.
- **Buborékrendezés, 1. menet:** `[5,2,8,1,9,4]` → 5>2 csere → `[2,5,8,1,9,4]` → 5<8 marad → 8>1 csere → `[2,5,1,8,9,4]` → 8<9 marad → 9>4 csere → `[2,5,1,8,4,9]`. A legnagyobb elem (9) a végére "úszott". A további menetek a maradékot rendezik.

Miért `Option<usize>` a keresés visszatérése? A C-ben és C++-ban a "nem találtam" jelzésére a `-1`-et szokás használni, amit könnyű elfelejteni ellenőrizni. Rustban a **típus mondja ki**, hogy lehet, hogy nincs eredmény, és a fordító kényszerít a kezelésére. A `usize` az indexelés természetes típusa.

Miért `&[i32]` és nem `&Vec<i32>` a paraméter? A `&[i32]` (szelet) **tömböt és Vec-et is fogad**, ezért rugalmasabb. A `&mut [i32]` módosítható szelet: a `bubble_sort` helyben rendez.

### Ugyanez iterátorokkal (idiomatikus Rust)

```rust
fn main() {
    let numbers = vec![5, 2, 8, 1, 9, 4];

    println!("{}", numbers.iter().sum::<i32>());                       // 29
    println!("{}", numbers.iter().filter(|&&n| n % 2 == 0).count());   // 3
    println!("{:?}", numbers.iter().position(|&n| n == 8));            // Some(2)
    println!("{:?}", numbers.iter().max());                            // Some(9)
    println!("{:?}", numbers.iter().min());                            // Some(1)

    let mut sorted = numbers.clone();
    sorted.sort();
    println!("{:?}", sorted);                                          // [1, 2, 4, 5, 8, 9]
}
```

Kimenet:

```
29
3
Some(2)
Some(9)
Some(1)
[1, 2, 4, 5, 8, 9]
```

Egy bevezető kurzuson **a kézi megvalósítást** kell tudnod; éles kódban az iterátor-változatot használd.

### Gyakorló kérdések

- Miért ad vissza `Option`-t a keresés? *(Mert lehet, hogy az elem nincs a listában, és a típus ezt kikényszeríti, hogy kezeld.)*
- Mit jelent a `&mut [i32]`? *(Módosítható hivatkozás egy egész-szeletre: a függvény helyben módosíthatja az elemeket.)*

---

## 9. Hibakezelés

A Rustban **nincsenek kivételek** (nincs `try/catch`, nincs `throw`). A hibákat **értékekként** kezeljük, a típusrendszeren keresztül:

| Helyzet | Rust | Python | C# / Java | C++ |
|---|---|---|---|---|
| Lehet, hogy nincs érték | `Option<T>` | `None` | `null` / `Nullable` | `std::optional` / `nullptr` |
| Várható hiba | `Result<T, E>` | `try/except` | `try/catch` | kivétel / hibakód |
| Programozói hiba (bug) | `panic!` | kivétel | kivétel | `assert` / UB |

Miért így? Mert a kivételek **láthatatlanok**: egy függvény szignatúrájából nem derül ki, hogy dobhat-e hibát. A Rust `Result` visszatérési típusa **a szignatúra része** — a hívó nem hagyhatja figyelmen kívül.

### Result

```rust
fn divide(numerator: i32, denominator: i32) -> Result<i32, String> {
    if denominator == 0 {
        Err(String::from("division by zero"))
    } else {
        Ok(numerator / denominator)
    }
}

fn main() {
    match divide(10, 2) {
        Ok(value) => println!("result = {}", value),
        Err(message) => println!("error: {}", message),
    }

    match divide(10, 0) {
        Ok(value) => println!("result = {}", value),
        Err(message) => println!("error: {}", message),
    }
}
```

Kimenet:

```
result = 5
error: division by zero
```

### A `?` operátor

```rust
use std::num::ParseIntError;

fn parse_and_double(input: &str) -> Result<i32, ParseIntError> {
    let number: i32 = input.trim().parse()?;   // hiba esetén AZONNAL visszatér a hibával
    Ok(number * 2)
}

fn main() {
    println!("{:?}", parse_and_double("21"));
    println!("{:?}", parse_and_double("abc"));
}
```

Kimenet:

```
Ok(42)
Err(ParseIntError { kind: InvalidDigit })
```

A `?` jelentése: *"ha ez `Err`, add vissza a hibát a hívónak, ha `Ok`, csomagold ki az értéket és folytasd"*. Ugyanaz, mint egy kézi `match`, csak egy karakter. Ez a "hibák felfelé terjesztése" Rust-módra — kivétel nélkül.

⚠️ A `?` csak olyan függvényben használható, ami maga is `Result`-ot (vagy `Option`-t) ad vissza.

### unwrap, expect, unwrap_or

```rust
fn main() {
    let good: Result<i32, String> = Ok(5);
    let bad: Result<i32, String> = Err(String::from("oops"));

    println!("{}", good.clone().unwrap());        // 5
    println!("{}", bad.clone().unwrap_or(0));     // 0 — alapértelmezett érték hiba esetén
    println!("{}", bad.clone().unwrap_or_default());   // 0 — az i32 alapértéke

    // bad.unwrap();   // PANIC: called `Result::unwrap()` on an `Err` value: "oops"
}
```

Kimenet:

```
5
0
0
```

Az `unwrap()` és az `expect("üzenet")` hiba esetén **leállítja a programot** (`panic`). Gyors próbákhoz és prototípusokhoz rendben van; komoly kódban használj `match`-et, `?`-t vagy `unwrap_or`-t.

### Saját hibatípus

```rust
use std::fmt;

#[derive(Debug)]
enum AgeError {
    NotANumber(String),
    OutOfRange(u32),
}

impl fmt::Display for AgeError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match self {
            AgeError::NotANumber(text) => write!(f, "'{}' is not a number", text),
            AgeError::OutOfRange(age) => write!(f, "{} is out of range", age),
        }
    }
}

fn parse_age(text: &str) -> Result<u32, AgeError> {
    let age: u32 = text
        .trim()
        .parse()
        .map_err(|_| AgeError::NotANumber(text.to_string()))?;

    if age > 150 {
        return Err(AgeError::OutOfRange(age));
    }
    Ok(age)
}

fn main() {
    for input in ["25", "abc", "200"] {
        match parse_age(input) {
            Ok(age) => println!("valid: {}", age),
            Err(error) => println!("error: {}", error),
        }
    }
}
```

Kimenet:

```
valid: 25
error: 'abc' is not a number
error: 200 is out of range
```

Levezetés: a `"25"` parse-olható és ≤ 150 → `Ok(25)`. Az `"abc"` nem szám → a `map_err` az `AgeError::NotANumber`-ré alakítja a hibát, a `?` visszatér vele. A `"200"` szám, de > 150 → `OutOfRange`. A `match` a hibatípus minden változatát kezeli (a fordító ellenőrzi, hogy egyet se hagyj ki).

### panic — amikor a program bugot talál

```rust
fn main() {
    let numbers = vec![1, 2, 3];
    let index = 5;
    println!("{}", numbers[index]);   // PANIC: index out of bounds: the len is 3 but the index is 5
}
```

A panic a program leállása (a folyamat kilépési kódja 101), nem helyreállítható hibakezelésre való. Ha részletes nyomkövetést (backtrace) akarsz: `RUST_BACKTRACE=1 cargo run`.

### Gyakorló kérdések

- Miért nincs kivétel a Rustban? *(A hibák értékek (`Result`, `Option`), amik a függvény típusában is látszanak, így a hívó nem hagyhatja őket figyelmen kívül.)*
- Mit csinál a `?` operátor? *(Hiba esetén azonnal visszatér a hibával, siker esetén kicsomagolja az értéket.)*
- Mikor használj `panic!`-ot / `unwrap()`-ot? *(Programozói hibára vagy gyors prototípusban; helyreállítható hibára `Result`-ot.)*

---

## 10. Ownership és borrowing

**Ez a könyv legfontosabb fejezete.** Ha ezt érted, a Rust többi része sima. Mielőtt a szabályokat látnád, érdemes megérteni, *mi a probléma*, amire megoldást ad.

### A probléma: ki szabadítja fel a memóriát?

Minden program kétféle memóriát használ:

| | Stack | Heap |
|---|---|---|
| Mi kerül ide | fix méretű adat (`i32`, `bool`, mutató) | dinamikus méretű adat (`String`, `Vec`) |
| Sebesség | nagyon gyors | lassabb |
| Felszabadítás | automatikus (a függvény végén) | **valakinek gondoskodnia kell róla** |

A kérdés: ki és mikor szabadítja fel a heap memóriát? A nyelvek három választ adnak:

| Megoldás | Nyelv | Hátrány |
|---|---|---|
| Kézzel (`free`, `delete`) | C, C++ | elfelejtett `free` (leak), kétszeres `free`, felszabadított adat használata |
| Szemétgyűjtő (GC) | Python, C#, Java | futásidejű költség, kiszámíthatatlan szünetek |
| **Ownership szabályok** | **Rust** | meg kell tanulni a szabályokat |

A Rust harmadik útja: a fordító **a kódod alapján eldönti**, hol kell a felszabadítás, és beírja. Futásidőben nincs többletköltség, és a hibák nem fordulnak le.

### A három szabály

> [!important]
> 1. Minden értéknek van **egy tulajdonosa** (*owner*) — egy változó.
> 2. Egyszerre **csak egy** tulajdonos lehet.
> 3. Amikor a tulajdonos **kiesik a hatókörből**, az érték **automatikusan törlődik** (`drop`).

### A 3. szabály működés közben

```rust
struct Noisy {
    name: String,
}

impl Drop for Noisy {
    fn drop(&mut self) {
        println!("dropping {}", self.name);
    }
}

fn main() {
    let _outer = Noisy { name: String::from("outer") };
    {
        let _inner = Noisy { name: String::from("inner") };
        println!("inside block");
    }   // ← itt esik ki a hatókörből az `_inner`
    println!("after block");
}   // ← itt esik ki az `_outer`
```

Kimenet:

```
inside block
dropping inner
after block
dropping outer
```

Levezetés: a belső blokk `}` jelénél a `_inner` hatóköre véget ér → a Rust **automatikusan** meghívja a `drop`-ot. Az `_outer` a `main` végén törlődik. Ez a C++ **RAII** elve — csak itt *minden* típusra automatikusan és kötelezően érvényes, és a fordító garantálja.

### Move — a tulajdonjog átadása

```rust
fn main() {
    let first = String::from("hello");
    let second = first;          // a tulajdonjog átkerül `second`-hez

    println!("{}", second);      // hello
    // println!("{}", first);    // FORDÍTÁSI HIBA: borrow of moved value: `first`
}
```

Kimenet:

```
hello
```

Miért nem másolódik egyszerűen az érték? Nézd meg, mi van a memóriában:

```
let first = String::from("hello");

  STACK                          HEAP
┌──────────────┐             ┌─────────────┐
│ first        │             │ h e l l o   │
│  ptr ────────┼────────────▶│             │
│  len = 5     │             └─────────────┘
│  capacity = 5│
└──────────────┘

let second = first;   ← csak a 3 mező másolódik, a heap-adat NEM

┌──────────────┐
│ first        │  ✗ ÉRVÉNYTELEN (a fordító letiltja)
└──────────────┘
┌──────────────┐             ┌─────────────┐
│ second       │             │ h e l l o   │
│  ptr ────────┼────────────▶│             │
│  len = 5     │             └─────────────┘
│  capacity = 5│
└──────────────┘
```

Ha mindkét változó érvényes maradna, **mindkettő megpróbálná felszabadítani** ugyanazt a heap-memóriát a hatókör végén → *double free*, a C/C++ egyik leghírhedtebb hibája. A Rust ezt úgy kerüli el, hogy a régi változót **érvénytelenné teszi**. Ez a **move** (áthelyezés).

**C++ összehasonlítás:** C++-ban az alapértelmezés a **másolás**, a move-hoz `std::move` kell. Rustban fordítva: az alapértelmezés a **move**, a másoláshoz `.clone()` kell. **C# / Python:** ott minden változó csak egy hivatkozás, a szemétgyűjtő intézi a többit.

### clone — valódi másolat

```rust
fn main() {
    let first = String::from("hello");
    let second = first.clone();     // a heap-adat is lemásolódik

    println!("{} {}", first, second);
}
```

Kimenet:

```
hello hello
```

A `.clone()` **drága lehet** (új heap-foglalás és másolás), ezért a Rust arra kényszerít, hogy *kiírd* — látszik a kódban, hol történik költséges művelet.

### Copy típusok — az egyszerű típusoknál nincs move

```rust
fn main() {
    let first = 5;
    let second = first;
    println!("{} {}", first, second);   // 5 5 — mindkettő érvényes
}
```

Kimenet:

```
5 5
```

Az `i32`, `f64`, `bool`, `char` (és az ezekből álló tuple-ök) **teljesen a stack-en élnek**, olcsón másolhatók, ezért a Rust automatikusan lemásolja őket (`Copy` trait). Csak a heap-et birtokló típusoknál (`String`, `Vec`, `Box`, saját struct-ok) van move.

### Move függvényhívásnál

```rust
fn consume(text: String) {
    println!("got: {}", text);
}   // a `text` itt kiesik a hatókörből → törlődik

fn give_back(text: String) -> String {
    println!("passing through: {}", text);
    text                          // visszaadja a tulajdonjogot
}

fn main() {
    let message = String::from("hi");
    consume(message);
    // println!("{}", message);   // FORDÍTÁSI HIBA: a `message` elmozdult

    let another = String::from("again");
    let another = give_back(another);   // visszakapjuk
    println!("{}", another);
}
```

Kimenet:

```
got: hi
passing through: again
again
```

A függvényparaméter **ugyanúgy move**, mint egy értékadás. Ha minden függvény elvenné és visszaadná az értékeket, rettenetesen kényelmetlen lenne — erre való a következő rész.

### Borrowing — kölcsönzés a `&` jellel

```rust
fn print_length(text: &String) {
    println!("length = {}", text.len());
}

fn main() {
    let message = String::from("hello");
    print_length(&message);                  // kölcsönadjuk, NEM adjuk át
    println!("still mine: {}", message);     // ✅ még a miénk
}
```

Kimenet:

```
length = 5
still mine: hello
```

A `&message` egy **hivatkozás (reference)**: a függvény "ránézhet" az adatra, de nem birtokolja, ezért nem is törli.

> [!tip] Hogyan jegyezd meg?
> - `value` — **átadom** (move), többé nem az enyém
> - `&value` — **megmutatom** (csak olvasható hivatkozás)
> - `&mut value` — **kölcsönadom módosításra**

| Rust | C++ megfelelő | Jelentés |
|---|---|---|
| `fn f(x: T)` | `void f(T x)` (copy) / `f(std::move(x))` | átvétel (move) |
| `fn f(x: &T)` | `void f(const T& x)` | csak olvasható kölcsönvétel |
| `fn f(x: &mut T)` | `void f(T& x)` | módosítható kölcsönvétel |

### Módosítható hivatkozás: `&mut`

```rust
fn add_exclamation(text: &mut String) {
    text.push_str("!");
}

fn main() {
    let mut message = String::from("hello");   // a változónak `mut`-nak kell lennie
    add_exclamation(&mut message);
    println!("{}", message);
}
```

Kimenet:

```
hello!
```

### A kölcsönzés két aranyszabálya

> [!important]
> Egy adott pillanatban **vagy**
> - **akárhány** `&` (olvasó) hivatkozás létezhet, **vagy**
> - **pontosan egy** `&mut` (író) hivatkozás.
>
> **Soha nem lehet mindkettő egyszerre.** És a hivatkozás **nem élhet tovább, mint az adat**, amire mutat.

```rust
fn main() {
    let mut message = String::from("hello");

    let first_ref = &mut message;
    // let second_ref = &mut message;   // FORDÍTÁSI HIBA: cannot borrow `message` as mutable more than once at a time
    first_ref.push('!');

    println!("{}", message);
}
```

Kimenet:

```
hello!
```

Miért ez a szabály? Mert ha ketten **írhatnának** ugyanabba a memóriába egyszerre (vagy valaki olvasna, miközben más ír), **adatverseny (data race)** és *iterátor-érvénytelenedés* keletkezne. Ezek a legnehezebben felderíthető hibák:

```rust
fn main() {
    let mut numbers = vec![1, 2, 3];

    // for n in &numbers {
    //     numbers.push(*n);   // FORDÍTÁSI HIBA: cannot borrow `numbers` as mutable because it is also borrowed as immutable
    // }

    println!("{:?}", numbers);
    numbers.push(4);
}
```

Ez a hiba Javában `ConcurrentModificationException` (futásidőben, ha szerencséd van), C++-ban *undefined behavior* (az iterátor érvénytelenné válik a `push_back`-nél). Rustban **le sem fordul**. A fordító a hibát *még a futtatás előtt* megfogja.

### A hivatkozás addig él, amíg használod (NLL)

```rust
fn main() {
    let mut text = String::from("hi");

    let reader = &text;
    println!("{}", reader);        // a `reader` UTOLSÓ használata

    let writer = &mut text;        // ✅ OK: az olvasó hivatkozásra már nincs szükség
    writer.push('!');

    println!("{}", text);
}
```

Kimenet:

```
hi
hi!
```

A fordító nem a blokk végéig, hanem **az utolsó használatig** tekinti élőnek a hivatkozást.

### Szeletek (slices) — kölcsönzött részletek

```rust
fn first_word(text: &str) -> &str {
    match text.find(' ') {
        Some(index) => &text[..index],
        None => text,
    }
}

fn main() {
    let sentence = String::from("hello brave world");
    println!("{}", first_word(&sentence));
}
```

Kimenet:

```
hello
```

A `&str` és a `&[T]` **a stringre/tömbre mutató kölcsönzött nézet**: nincs másolás. A fordító arra is vigyáz, hogy a `sentence` ne törlődjön vagy módosuljon, amíg a `first_word` eredményét használod.

### Gyakorló kérdések

- Mi a három ownership-szabály? *(Minden értéknek egy tulajdonosa van; egyszerre csak egy; a tulajdonos kiesésekor az érték törlődik.)*
- Miért hibás a `let a = String::from("x"); let b = a; println!("{}", a);`? *(Az értékadás move: a tulajdonjog `b`-hez került, `a` érvénytelen.)*
- Miért működik az `i32`-nél mégis? *(Az `i32` `Copy` típus: olcsón, automatikusan másolódik.)*
- Mi a különbség `&T` és `&mut T` között? *(Az első csak olvasható és több is lehet belőle egyszerre; a második módosíthat, de egyszerre csak egy lehet, és közben olvasó sem létezhet.)*
- Miért kizárt a Rustban az iterátor-érvénytelenedés? *(A kölcsönzési szabály nem engedi, hogy módosítsd a gyűjteményt, amíg egy hivatkozás rá mutat.)*

---

## 11. Lifetime-ok

A kölcsönzés harmadik szabálya: a hivatkozás **nem élhet tovább, mint az adat**, amire mutat. Különben **lógó mutató (dangling pointer)** keletkezne — a C/C++ egyik leggyakoribb hibája.

### A lógó hivatkozás — és hogy miért nem fordul le

```rust
// fn dangle() -> &String {          // HIBA: missing lifetime specifier
//     let s = String::from("hi");
//     &s                            // a `s` a függvény végén törlődik → a hivatkozás érvénytelen lenne
// }

fn no_dangle() -> String {
    let s = String::from("hi");
    s                                // a tulajdonjogot adjuk vissza → nincs gond
}

fn main() {
    println!("{}", no_dangle());
}
```

Kimenet:

```
hi
```

Javítás: **ne hivatkozást adj vissza, hanem magát az értéket** (move).

### Mi az a lifetime-jelölés?

Ha egy függvény hivatkozást ad vissza, a fordítónak tudnia kell, *melyik bemenethez kötődik* a visszaadott hivatkozás élettartama:

```rust
fn longest<'a>(first: &'a str, second: &'a str) -> &'a str {
    if first.len() >= second.len() {
        first
    } else {
        second
    }
}

fn main() {
    let a = String::from("long string");
    let b = String::from("short");

    let result = longest(&a, &b);
    println!("{}", result);
}
```

Kimenet:

```
long string
```

A `<'a>` azt mondja: *"a visszaadott hivatkozás legfeljebb addig él, ameddig **a két bemenet közül a rövidebb élő**"*. A `'a` nem módosít semmit futásidőben — csak a fordítónak szóló **ígéret**, amit ő ellenőriz.

Mi történne, ha rosszul használnád?

```rust
fn longest<'a>(first: &'a str, second: &'a str) -> &'a str {
    if first.len() >= second.len() { first } else { second }
}

fn main() {
    let a = String::from("long string");
    // let result;
    // {
    //     let b = String::from("short");
    //     result = longest(&a, &b);
    // }                                  // `b` itt törlődik
    // println!("{}", result);            // FORDÍTÁSI HIBA: `b` does not live long enough
    println!("{}", longest(&a, "tiny"));
}
```

Kimenet:

```
long string
```

A `result` lehetne `b`-re mutató hivatkozás, de a `b` a belső blokk végén törlődik — a fordító ezt megfogja.

### Lifetime struct-ban

Ha egy struct **hivatkozást tárol**, annak is lifetime kell:

```rust
struct Excerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me. Some years ago...");
    let first_sentence = novel.split('.').next().unwrap();

    let excerpt = Excerpt { part: first_sentence };
    println!("{}", excerpt.part);
}
```

Kimenet:

```
Call me
```

A `'a` biztosítja, hogy az `Excerpt` nem élhet tovább, mint a `novel`, amire mutat.

### Mikor nem kell kiírni?

A legtöbb esetben **egyáltalán nem kell** lifetime-ot írnod (*lifetime elision*): ha egy függvénynek egyetlen hivatkozás-paramétere van, a fordító kitalálja. Ezért működött az előző fejezetben a `fn first_word(text: &str) -> &str` jelölés nélkül. Kézzel akkor kell, ha **több hivatkozás** van, és a fordító nem tudja eldönteni, melyikhez kötődik az eredmény. A `'static` a program teljes futásáig élő adat (pl. a string literálok).

### Gyakorló kérdések

- Mi a lógó hivatkozás, és miért nem fordul le Rustban? *(Olyan hivatkozás, ami már törölt adatra mutat; a fordító lifetime-ellenőrzéssel kizárja.)*
- Mit jelent a `<'a>`? *(Lifetime-paraméter: összekapcsolja a bemenetek és a visszatérési hivatkozás élettartamát; a fordítónak szóló információ, futásidőben nem létezik.)*
- Mikor kell kézzel lifetime-ot írni? *(Ha több hivatkozás-paraméter van, vagy egy struct hivatkozást tárol.)*

---

## 12. Struct, enum, trait (objektumorientált programozás)

A Rustban **nincsenek osztályok és nincs öröklés**. Helyette három dolog van:

| Építőelem | Szerepe | Megfelelő máshol |
|---|---|---|
| `struct` | adatok csoportosítása | C++/C# `class` mezői, Python `dataclass` |
| `impl` | metódusok hozzáadása egy típushoz | C++ metódusok, Python `def` az osztályban |
| `trait` | közös viselkedés leírása | Java/C# `interface`, C++ absztrakt osztály |

### struct és impl

```rust
#[derive(Debug, Clone, PartialEq)]
struct Rectangle {
    width: f64,
    height: f64,
}

impl Rectangle {
    // "asszociált függvény" — nincs `self`, ez a konstruktor megfelelője
    fn new(width: f64, height: f64) -> Self {
        Rectangle { width, height }
    }

    // metódus, olvasó hozzáférés
    fn area(&self) -> f64 {
        self.width * self.height
    }

    // metódus, módosító hozzáférés
    fn scale(&mut self, factor: f64) {
        self.width *= factor;
        self.height *= factor;
    }
}

fn main() {
    let mut rect = Rectangle::new(3.0, 4.0);
    println!("{}", rect.area());            // 12

    rect.scale(2.0);
    println!("{:?}", rect);                 // Rectangle { width: 6.0, height: 8.0 }
    println!("{}", rect.area());            // 48

    let copy = rect.clone();
    println!("{}", copy == rect);           // true
}
```

Kimenet:

```
12
Rectangle { width: 6.0, height: 8.0 }
48
true
```

Miért így?

- **Nincs konstruktor-nyelvi elem:** a megszokott név a `new`, ami egy sima függvény, és `Típus::new(...)`-vel hívjuk (a `::` az "asszociált függvény" hívása).
- **`&self`**, **`&mut self`**, **`self`** — a metódus három fajta hozzáférése az objektumhoz (olvasás / módosítás / elfogyasztás). Ez a `&`/`&mut`/érték szabály a 10. fejezetből. A `self` a Python `self`-je és a C++/C# `this` megfelelője, csak itt **a hozzáférés módja is látszik**.
- **A módosíthatóság a változóé, nem a mezőé:** a `let rect` (mut nélkül) nem engedi a `rect.scale(...)`-t. Nincs külön "const metódus" jelölés.
- **`#[derive(...)]`** — a fordító automatikusan generálja a `Debug` (`{:?}` kiírás), `Clone` (`.clone()`) és `PartialEq` (`==`) viselkedést. Pythonban a `@dataclass` csinál hasonlót.
- A `12.0` kiírása `12`, mert a `{}` elhagyja a felesleges `.0`-t (lásd a 2. fejezetet).

### Enum — adatot hordozó felsorolás

A Rust `enum`-ja **sokkal erősebb**, mint a C-s/Java-s: a változatok **adatot is hordozhatnak**.

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

fn handle(message: &Message) -> String {
    match message {
        Message::Quit => String::from("quit"),
        Message::Move { x, y } => format!("move to {}, {}", x, y),
        Message::Write(text) => format!("write: {}", text),
    }
}

fn main() {
    println!("{}", handle(&Message::Quit));
    println!("{}", handle(&Message::Move { x: 3, y: 7 }));
    println!("{}", handle(&Message::Write(String::from("hi"))));
}
```

Kimenet:

```
quit
move to 3, 7
write: hi
```

A `match` **kimerítő**: ha új változatot adsz az enum-hoz, a fordító minden hiányzó `match`-nél hibát jelez. Az `Option` és a `Result` is csak egy-egy enum:

```rust
// enum Option<T> { Some(T), None }
// enum Result<T, E> { Ok(T), Err(E) }
```

Egyetlen változat vizsgálatához kényelmes az `if let`:

```rust
fn main() {
    let maybe_number: Option<i32> = Some(7);

    if let Some(number) = maybe_number {
        println!("got {}", number);
    }
}
```

Kimenet:

```
got 7
```

### trait — közös viselkedés

```rust
trait Shape {
    fn area(&self) -> f64;
    fn name(&self) -> String;

    // alapértelmezett implementáció (mint a Java/C# interface `default` metódusa)
    fn describe(&self) -> String {
        format!("{} with area {:.2}", self.name(), self.area())
    }
}

struct Circle {
    radius: f64,
}

struct Square {
    side: f64,
}

impl Shape for Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * self.radius * self.radius
    }
    fn name(&self) -> String {
        String::from("circle")
    }
}

impl Shape for Square {
    fn area(&self) -> f64 {
        self.side * self.side
    }
    fn name(&self) -> String {
        String::from("square")
    }
}

fn main() {
    let shapes: Vec<Box<dyn Shape>> = vec![
        Box::new(Circle { radius: 1.0 }),
        Box::new(Square { side: 3.0 }),
    ];

    for shape in &shapes {
        println!("{}", shape.describe());
    }

    let total: f64 = shapes.iter().map(|shape| shape.area()).sum();
    println!("total = {:.2}", total);
}
```

Kimenet:

```
circle with area 3.14
square with area 9.00
total = 12.14
```

Levezetés: a kör területe `π · 1² ≈ 3,14159`, a négyzeté `3² = 9`. Összeg: `12,14159` → két tizedesre `12.14`.

Miért `Vec<Box<dyn Shape>>`? A `Circle` és a `Square` **különböző méretű** típusok, nem férnek egy `Vec`-be közvetlenül. A `Box` heap-re teszi őket (15. fejezet), a `dyn Shape` pedig azt jelenti: *"bármilyen típus, ami implementálja a Shape-et"* — **futásidejű polimorfizmus** (mint a Java/C# interface-ek, a C++ `virtual` függvények).

### Láthatóság (pub)

Alapból **minden privát**, amit nem jelölsz `pub`-bal — az mezőkre, függvényekre, struct-okra egyaránt igaz (a láthatóság a **modul** határán dől el, nem az osztályén; lásd 13. fejezet).

### Összehasonlítás

| Szempont | Rust | C++ | Python | C# / Java |
|---|---|---|---|---|
| Adat + viselkedés | `struct` + `impl` | `class` | `class` | `class` |
| Konstruktor | `Típus::new()` (konvenció) | konstruktor | `__init__` | konstruktor |
| Öröklés | **nincs** (kompozíció + trait) | van, többszörös is | van | van (osztályból egy) |
| Interface | `trait` | absztrakt osztály | `ABC` / duck typing | `interface` |
| Alapértelmezett láthatóság | privát | `class`: privát | publikus | privát / package |
| Destruktor | `Drop` trait | destruktor | `__del__` | `IDisposable` / finalizer |

Miért nincs öröklés? Mert az öröklési hierarchiák törékenyek (a "gyémánt probléma", a szoros csatolás). A Rust helyette **kompozíciót** (struct a struct-ban) és **trait-eket** használ.

### Gyakorló kérdések

- Mi a különbség `&self` és `&mut self` között? *(Az első csak olvassa az objektumot, a második módosíthatja.)*
- Mi a `#[derive(Debug)]` szerepe? *(A fordító automatikusan megírja a `{:?}` kiíráshoz szükséges kódot.)*
- Mi a különbség a Rust és a Java enum között? *(A Rust enum változatai adatot is hordozhatnak, és a `match` kimerítő.)*
- Mi a trait, és mi a megfelelője más nyelvekben? *(Közös viselkedést leíró szerződés; a Java/C# `interface`-e, a C++ absztrakt osztálya.)*

---

## 13. Modulok és csomagkezelés (cargo)

### Modulok: `mod` és `pub`

```rust
mod utils {
    pub fn square(x: i32) -> i32 {
        x * x
    }

    pub struct Counter {
        count: u32,                    // privát mező: csak a `utils` modulon belül érhető el
    }

    impl Counter {
        pub fn new() -> Self {
            Counter { count: 0 }
        }
        pub fn increment(&mut self) {
            self.count += 1;
        }
        pub fn get(&self) -> u32 {
            self.count
        }
    }
}

use utils::{square, Counter};

fn main() {
    println!("{}", square(5));         // 25

    let mut counter = Counter::new();
    counter.increment();
    counter.increment();
    println!("{}", counter.get());     // 2

    // println!("{}", counter.count);  // FORDÍTÁSI HIBA: field `count` is private
}
```

Kimenet:

```
25
2
```

A `mod` egy névteret nyit (C++ `namespace`, C# `namespace`, Python modul). Minden **alapból privát**; csak a `pub`-bal jelölt elemek érhetők el kívülről. A `use` hozza be a neveket a hatókörbe (Python `from x import y`).

### Több fájl

Ha a `main.rs`-ben `mod utils;` áll (blokk nélkül), a fordító a `src/utils.rs` fájlt (vagy a `src/utils/mod.rs`-t) tölti be:

```
src/
├── main.rs      ← `mod utils;` és `use utils::square;`
└── utils.rs     ← pub fn square(x: i32) -> i32 { x * x }
```

Nincsenek header fájlok, nincs `#include`, nincs külön deklaráció/definíció, és nincs linker-hibaüzenet-pokol, mint C++-ban.

### Külső könyvtárak: crate-ek

A Cargo.toml (a `.csproj` / `requirements.txt` megfelelője):

```toml
[package]
name = "my_project"
version = "0.1.0"
edition = "2021"

[dependencies]
rand = "0.8"
serde = { version = "1", features = ["derive"] }
```

Egy sor hozzáadásához:

```bash
cargo add rand
```

Ezután a `cargo build` automatikusan letölti és lefordítja a crate-et a https://crates.io oldalról, és a `Cargo.lock` rögzíti a pontos verziókat (reprodukálható build).

| | Rust | Python | C# | C++ |
|---|---|---|---|---|
| Csomagkezelő | `cargo` | `pip` / `venv` | NuGet / `dotnet` | CMake + vcpkg / conan |
| Leíró fájl | `Cargo.toml` | `requirements.txt` | `.csproj` | `CMakeLists.txt` |
| Központi tár | crates.io | PyPI | nuget.org | – (szétszórt) |
| Névtér | `mod` / `use` | `import` | `namespace` / `using` | `namespace` / `#include` |

### Gyakorló kérdések

- Mi a `pub` szerepe? *(Nyilvánossá tesz egy elemet a moduljának határán kívül; alapból minden privát.)*
- Mi a Cargo.toml? *(A projekt leírása és függőségei: a `.csproj` / `requirements.txt` megfelelője.)*
- Kellenek header fájlok Rustban? *(Nem, a `mod` és a `use` kezeli a szerkezetet.)*

---

## 14. Generikus programozás

A generikusok lehetővé teszik, hogy **egyetlen függvényt több típusra** írj meg. A Rust `<T>` jelölése a C++ `template`-jének, a C#/Java `<T>`-jének felel meg.

```rust
fn largest<T: PartialOrd + Copy>(items: &[T]) -> T {
    let mut largest = items[0];
    for &item in items {
        if item > largest {
            largest = item;
        }
    }
    largest
}

fn main() {
    println!("{}", largest(&[3, 9, 2]));          // 9
    println!("{}", largest(&[1.5, 0.3, 2.7]));    // 2.7
    println!("{}", largest(&['a', 'z', 'm']));    // z
}
```

Kimenet:

```
9
2.7
z
```

Miért a `T: PartialOrd + Copy`? Ez a **trait bound** (megszorítás): a `T` bármilyen típus lehet, *de csak olyan, amire*

- **`PartialOrd`** — működik az összehasonlítás (`>`). Enélkül nem lehetne `item > largest`-et írni, hiszen nem tudhatjuk, hogy a `T`-t össze lehet-e hasonlítani.
- **`Copy`** — olcsón másolható. Enélkül az `items[0]` kivétele "mozgatás lenne a szeletből", amit a 10. fejezet szabályai tiltanak.

Ez a legnagyobb különbség a C++ template-hez képest: C++-ban a template-et *használatkor* ellenőrzi a fordító, ami hosszú, érthetetlen hibaüzeneteket ad. Rustban a megszorítás **a szignatúrában** áll, és a hiba már a függvény *definíciójánál* jelentkezik, érthető üzenettel.

### Generikus struct

```rust
use std::fmt::Display;

struct Pair<T> {
    first: T,
    second: T,
}

impl<T: PartialOrd + Display> Pair<T> {
    fn print_larger(&self) {
        if self.first >= self.second {
            println!("larger: {}", self.first);
        } else {
            println!("larger: {}", self.second);
        }
    }
}

fn main() {
    Pair { first: 3, second: 7 }.print_larger();
    Pair { first: "apple", second: "banana" }.print_larger();
}
```

Kimenet:

```
larger: 7
larger: banana
```

A szövegek összehasonlítása szótár szerinti: `"banana" > "apple"`.

### Hogyan működik a háttérben? (monomorphization)

A fordító minden típushoz, amivel meghívod, **külön gépi kódot generál** (`largest::<i32>`, `largest::<f64>`, `largest::<char>`). Ugyanez a C++ template-ek működése, ezért a generikus kód **pontosan olyan gyors**, mint a kézzel típusra írt változat. Ezt hívják *zero-cost abstraction*-nek.

| | Rust | C++ | Java | C# | Python |
|---|---|---|---|---|---|
| Név | generics | template | generics | generics | duck typing |
| Mikor ellenőriz | definíciónál (trait bound) | használatkor | fordításkor | fordításkor | futásidőben |
| Megvalósítás | monomorphization | monomorphization | type erasure | futásidejű reifikáció | – |

### A `where` záradék és az `impl Trait`

Hosszú megszorításoknál a `where` olvashatóbb:

```rust
use std::fmt::Display;

fn show_largest<T>(items: &[T]) -> String
where
    T: PartialOrd + Copy + Display,
{
    let mut largest = items[0];
    for &item in items {
        if item > largest {
            largest = item;
        }
    }
    format!("largest = {}", largest)
}

fn main() {
    println!("{}", show_largest(&[4, 8, 1]));
}
```

Kimenet:

```
largest = 8
```

Az `impl Trait` a paraméterben egyszerűbb jelölés egyetlen megszorításra (lásd a 7. fejezet `apply_twice` példáját).

### Gyakorló kérdések

- Mit jelent a `T: PartialOrd + Copy`? *(A `T` bármilyen típus lehet, ami összehasonlítható és olcsón másolható.)*
- Lassabb a generikus kód, mint a típusra megírt? *(Nem, a fordító minden típusra külön gépi kódot generál: monomorphization.)*
- Miben jobb a Rust generikus rendszere a C++ template-nél? *(A megszorítások a szignatúrában vannak, ezért a hibák a definíciónál jelentkeznek, érthető üzenettel.)*

---

## 15. Smart pointerek: Box, Rc, RefCell

A "sima" Rust-adatok a **stack-en** élnek, egyetlen tulajdonossal. Néha ennél többre van szükség. A **smart pointerek** ezeket az eseteket fedik le.

| Típus | Mire való | C++ megfelelő |
|---|---|---|
| `Box<T>` | egy érték **heap-en**, egyetlen tulajdonossal | `std::unique_ptr<T>` |
| `Rc<T>` | **több tulajdonos** (szálon belül), referenciaszámlálással | `std::shared_ptr<T>` |
| `Arc<T>` | mint az `Rc`, de **szálbiztos** | `std::shared_ptr<T>` (atomikus) |
| `RefCell<T>` | módosítás **megosztott** adaton, a szabályok **futásidőben** ellenőrizve | – |

### Box — rekurzív típusokhoz

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use List::{Cons, Nil};

fn sum_list(list: &List) -> i32 {
    match list {
        Cons(value, rest) => value + sum_list(rest),
        Nil => 0,
    }
}

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
    println!("{}", sum_list(&list));
}
```

Kimenet:

```
6
```

Levezetés: `1 + (2 + (3 + 0)) = 6`.

Miért kell a `Box`? Egy `List`, ami tartalmaz egy `List`-et, **végtelen méretű lenne** — a fordító nem tudná kiszámolni, mennyi helyet foglaljon. A `Box` egy **mutató** (fix méretű), ami a heap-en lévő következő elemre mutat. A `Box` kiesésekor az adat automatikusan felszabadul (RAII).

### Rc — több tulajdonos

```rust
use std::rc::Rc;

fn main() {
    let shared = Rc::new(String::from("data"));
    println!("{}", Rc::strong_count(&shared));     // 1

    let second = Rc::clone(&shared);               // NEM másolja az adatot, csak a számlálót növeli
    println!("{}", Rc::strong_count(&shared));     // 2

    {
        let third = Rc::clone(&shared);
        println!("{} {}", Rc::strong_count(&shared), third);   // 3 data
    }   // a `third` itt kiesik

    println!("{}", Rc::strong_count(&shared));     // 2
    println!("{}", second);
}
```

Kimenet:

```
1
2
3 data
2
data
```

Az `Rc::clone` **olcsó**: csak a referenciaszámlálót növeli. Az adat akkor törlődik, amikor a számláló **0-ra** esik. Ez ugyanaz, mint a Python referenciaszámlálása és a C++ `shared_ptr` — csak itt **kérésre** (nem alapértelmezetten) van.

### RefCell — módosítás megosztott adaton

Az `Rc` mögötti adat alapból csak olvasható (hiszen több tulajdonos van). Ha mégis módosítani kell, a `RefCell` a kölcsönzési szabályokat **futásidőben** ellenőrzi:

```rust
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
    let shared = Rc::new(RefCell::new(vec![1, 2]));
    let other = Rc::clone(&shared);

    other.borrow_mut().push(3);          // módosítás az egyik "tulajdonoson" keresztül
    println!("{:?}", shared.borrow());   // [1, 2, 3] — a másikon keresztül látszik
}
```

Kimenet:

```
[1, 2, 3]
```

⚠️ Ha futásidőben megszeged a szabályt (például két `borrow_mut()` egyszerre), a program **panic-kel** leáll. A `RefCell` tehát a fordítási idejű garanciát futásidejű ellenőrzésre cseréli — csak akkor használd, ha tényleg szükséges.

### Szálak közti megosztás

Szálak között `Rc` helyett **`Arc`** kell (atomikus számláló), a módosításhoz pedig `RefCell` helyett **`Mutex`**: `Arc<Mutex<T>>`. A fordító **nem engedi**, hogy egy `Rc`-t szálak közé küldj — az adatversenyek tehát fordítási időben kizártak. Ez a Rust "félelem nélküli konkurencia" ígérete.

### Gyakorló kérdések

- Miért kell `Box` a rekurzív enumhoz? *(Mert különben végtelen méretű lenne; a `Box` fix méretű mutató a heap-re.)*
- Mit csinál az `Rc::clone`? *(Nem másolja az adatot, csak növeli a referenciaszámlálót.)*
- Mi a különbség `Rc` és `Arc` között? *(Az `Arc` atomikus számlálót használ, ezért szálbiztos; az `Rc` csak egy szálon belül használható.)*
- Mi a `RefCell` ára? *(A kölcsönzési szabályokat futásidőben ellenőrzi, megszegésnél panic keletkezik.)*

---

## 16. Gyakorlati integráció: fájlfeldolgozás a nulláról

Egy komplett program, ami **fájlt ír, beolvas, feldolgoz, és hibát kezel** — az eddigi fejezetek összefoglalása:

```rust
use std::error::Error;
use std::fs;

struct Issue {
    title: String,
    number: u32,
}

fn load_issues(filename: &str) -> Result<Vec<Issue>, Box<dyn Error>> {
    let content = fs::read_to_string(filename)?;   // fájl beolvasása, hiba esetén visszatér

    let mut issues = Vec::new();
    for line in content.lines() {
        if let Some((title, number_text)) = line.split_once(',') {
            let number: u32 = number_text.trim().parse()?;
            issues.push(Issue { title: title.to_string(), number });
        }
    }
    Ok(issues)
}

fn count_bugs(issues: &[Issue]) -> usize {
    issues.iter().filter(|issue| issue.title.contains("bug")).count()
}

fn main() -> Result<(), Box<dyn Error>> {
    fs::write("issues.csv", "bug login,1\nfeature request,2\nbug crash,3")?;

    let issues = load_issues("issues.csv")?;
    println!("Issues found: {}", issues.len());
    println!("With 'bug' in title: {}", count_bugs(&issues));

    let number_sum: u32 = issues.iter().map(|issue| issue.number).sum();
    println!("Sum of numbers: {}", number_sum);

    match load_issues("missing.csv") {
        Ok(_) => println!("unexpected"),
        Err(error) => println!("Error: {}", error),
    }

    Ok(())
}
```

Kimenet:

```
Issues found: 3
With 'bug' in title: 2
Sum of numbers: 6
Error: No such file or directory (os error 2)
```

Levezetés:

- A fájl három sort tartalmaz: `bug login,1`, `feature request,2`, `bug crash,3`.
- A `split_once(',')` minden sort két részre vág az **első vesszőnél**: a cím és a szám.
- A `bug` szót két cím tartalmazza (`bug login`, `bug crash`) → **2**.
- A számok összege: `1 + 2 + 3 = 6`.
- A `missing.csv` nem létezik → a `read_to_string` `Err`-t ad, a `?` továbbítja, a `match` kiírja.

Miért nincs sehol `file.close()`? A `fs::read_to_string` belül megnyitja és **automatikusan bezárja** a fájlt. Ha kézzel nyitnál meg egy `File`-t, az is **RAII** szerint záródik, amikor kiesik a hatókörből (10. fejezet) — pontosan úgy, mint a Python `with open(...)` blokkja, csak külön szintaxis nélkül, és hiba esetén is.

Miért `Box<dyn Error>` a hibatípus? Mert a függvényben **kétféle hiba** is előfordulhat (fájl-hiba és szám-feldolgozási hiba). A `Box<dyn Error>` "bármilyen hibatípust" befogad, így a `?` mindkettőt át tudja alakítani. A `fn main() -> Result<(), Box<dyn Error>>` azt is megengedi, hogy a `main`-ben is használd a `?`-t: hiba esetén a program hibakóddal lép ki.

### Gyakorló kérdések

- Miért nincs `file.close()`? *(RAII: a fájl automatikusan bezáródik, amikor kiesik a hatókörből.)*
- Mit jelent a `Box<dyn Error>`? *(Bármilyen hibatípust befogadó, heap-en tárolt hiba.)*
- Mit csinál a `split_once(',')`? *(Az első vesszőnél kettévágja a stringet, és `Option<(&str, &str)>`-t ad vissza.)*

---

## 17. Automatizált tesztelés: cargo test

A Rustban a tesztelés **a nyelv és a cargo része**, nincs szükség külső keretrendszerre.

```rust
fn add(first: i32, second: i32) -> i32 {
    first + second
}

fn linear_search(numbers: &[i32], target: i32) -> Option<usize> {
    numbers.iter().position(|&n| n == target)
}

fn divide(numerator: i32, denominator: i32) -> i32 {
    if denominator == 0 {
        panic!("division by zero");
    }
    numerator / denominator
}

fn main() {
    println!("{}", add(2, 3));
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn add_works() {
        assert_eq!(add(2, 3), 5);
    }

    #[test]
    fn search_found() {
        assert_eq!(linear_search(&[10, 20, 30], 20), Some(1));
    }

    #[test]
    fn search_not_found() {
        assert_eq!(linear_search(&[10, 20, 30], 99), None);
    }

    #[test]
    #[should_panic(expected = "division by zero")]
    fn divide_by_zero_panics() {
        divide(1, 0);
    }
}
```

Futtatás:

```bash
cargo test
```

Kimenet:

```
running 4 tests
test tests::add_works ... ok
test tests::divide_by_zero_panics - should panic ... ok
test tests::search_found ... ok
test tests::search_not_found ... ok

test result: ok. 4 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Miért így?

- `#[cfg(test)]` — a `tests` modul **csak tesztfuttatáskor** fordul le, a végleges programba nem kerül be.
- `use super::*;` — behozza a szülő modul (a fájl többi része) függvényeit.
- `#[test]` — ezt a függvényt a cargo tesztként futtatja. Ha a függvény **panic-kel** leáll, a teszt **megbukik**.
- `assert_eq!(a, b)` — hiba esetén kiírja mindkét konkrét értéket, így látod, mi volt a gond.
- `#[should_panic]` — az olyan tesztet jelöli, aminek **panic-elnie kell**.

| xUnit (C#) | pytest (Python) | Google Test (C++) | Rust |
|---|---|---|---|
| `[Fact]` | `test_` előtag | `TEST(Suite, Name)` | `#[test]` |
| `Assert.Equal(a, b)` | `assert a == b` | `EXPECT_EQ(a, b)` | `assert_eq!(a, b)` |
| `Assert.True(x)` | `assert x` | `EXPECT_TRUE(x)` | `assert!(x)` |
| `Assert.Throws` | `pytest.raises` | `EXPECT_THROW` | `#[should_panic]` |
| külön projekt | külön fájl | külön build-célpont | **ugyanabban a fájlban**, vagy a `tests/` mappában |

Hasznos tudnivalók:

- `cargo test search` — csak azokat a teszteket futtatja, amiknek a nevében szerepel a `search`.
- `cargo test -- --nocapture` — a tesztekben lévő `println!` kimenetet is megmutatja (alapból elnyeli).
- A `tests/` mappa fájljai **integrációs tesztek**: kívülről, a crate nyilvános felületén át tesztelnek.
- A dokumentációs kommentekben (`///`) lévő kódpéldákat a `cargo test` **szintén lefuttatja**, így a dokumentáció sosem avul el.

### Gyakorló kérdések

- Mi a `#[cfg(test)]` szerepe? *(A modul csak tesztfuttatáskor fordul le, a kész programba nem kerül be.)*
- Mikor bukik meg egy teszt? *(Ha a tesztfüggvény panic-kel leáll, például egy `assert_eq!` hibája miatt.)*
- Hogyan teszteled, hogy egy függvénynek panic-elnie kell? *(`#[should_panic]` attribútummal.)*

---

## 18. Gyakorló feladatok

Itt **kódot kell írnod**, nem csak kérdésre válaszolnod. A módszer: olvasd el a feladatot, próbáld meg **magad** megoldani (`cargo run`), és csak utána nyisd ki a **Megoldás** blokkot (kattints a címére). A megoldásoknál mindig ott a **kimenet** és a **levezetés**, hogy lásd, *miért* az jön ki.

A feladatok nehézség szerint nőnek: az 1–4. az 1–8. fejezet anyaga, az 5–7. ciklusok és rekurzió, a 8–10. az ownership, a hibakezelés és a struct-ok.

### Feladat 1: Összeg

Írj függvényt `sum_to(limit: u32) -> u32` néven, ami összeadja a számokat 1-től `limit`-ig. Mi lesz a `sum_to(4)` értéke?

> [!success]- Megoldás
> ```rust
> fn sum_to(limit: u32) -> u32 {
>     let mut total = 0;
>     for number in 1..=limit {
>         total += number;
>     }
>     total
> }
>
> fn main() {
>     println!("{}", sum_to(4));
> }
> ```
>
> Kimenet:
>
> ```
> 10
> ```
>
> Levezetés (az `1..=limit` az utolsó elemet is tartalmazza):
>
> | `number` | `total` |
> |---|---|
> | 1 | 0 + 1 = 1 |
> | 2 | 1 + 2 = 3 |
> | 3 | 3 + 3 = 6 |
> | 4 | 6 + 4 = 10 |
>
> Miért `1..=limit` és nem `1..limit`? A `..` kizárja az utolsó számot, így a 4 kimaradna, és az eredmény 6 lenne.

### Feladat 2: Páros szám

Írj függvényt `is_even(number: i32) -> bool` néven. Mit ad vissza a `is_even(8)` és az `is_even(7)`?

> [!success]- Megoldás
> ```rust
> fn is_even(number: i32) -> bool {
>     number % 2 == 0
> }
>
> fn main() {
>     println!("{}", is_even(8));
>     println!("{}", is_even(7));
> }
> ```
>
> Kimenet:
>
> ```
> true
> false
> ```
>
> Levezetés: `8 % 2 = 0`, és `0 == 0` igaz → `true`. A `7 % 2 = 1`, és `1 == 0` hamis → `false`. A függvény egyetlen kifejezés **pontosvessző nélkül**, ezért ez a visszatérési érték.

### Feladat 3: Mi a hiba?

Miért nem fordul le ez a program? Javítsd ki kétféleképpen.

```rust
fn main() {
    let text = String::from("rust");
    let other = text;
    println!("{} {}", text, other);
}
```

> [!success]- Megoldás
> A hiba: `error[E0382]: borrow of moved value: text`. A `let other = text;` **move**: a tulajdonjog `other`-hez került, a `text` érvénytelen (10. fejezet).
>
> **1. javítás: `clone`** (valódi másolat, két külön heap-adat):
>
> ```rust
> fn main() {
>     let text = String::from("rust");
>     let other = text.clone();
>     println!("{} {}", text, other);
> }
> ```
>
> Kimenet:
>
> ```
> rust rust
> ```
>
> **2. javítás: kölcsönzés `&`-lel** (nincs másolás, `other` csak hivatkozás `text`-re):
>
> ```rust
> fn main() {
>     let text = String::from("rust");
>     let other = &text;
>     println!("{} {}", text, other);
> }
> ```
>
> Kimenet:
>
> ```
> rust rust
> ```
>
> Melyiket mikor? Ha **külön másolatra** van szükséged, `clone`. Ha csak **olvasni** akarod ugyanazt az adatot, a kölcsönzés olcsóbb.

### Feladat 4: Legnagyobb elem

Írj függvényt `find_max(numbers: &[i32]) -> Option<i32>` néven, ami a legnagyobb elemet adja vissza, **üres szeletnél** pedig `None`-t. Miért `Option` a visszatérési típus?

> [!success]- Megoldás
> ```rust
> fn find_max(numbers: &[i32]) -> Option<i32> {
>     if numbers.is_empty() {
>         return None;
>     }
>     let mut max_value = numbers[0];
>     for &number in numbers {
>         if number > max_value {
>             max_value = number;
>         }
>     }
>     Some(max_value)
> }
>
> fn main() {
>     println!("{:?}", find_max(&[3, 9, 2, 7]));
>     println!("{:?}", find_max(&[]));
> }
> ```
>
> Kimenet:
>
> ```
> Some(9)
> None
> ```
>
> Levezetés a `[3, 9, 2, 7]`-re:
>
> | `number` | `max_value` |
> |---|---|
> | (kezdet) | 3 |
> | 3 | 3 |
> | 9 | **9** |
> | 2 | 9 |
> | 7 | 9 |
>
> Miért `Option`? Üres szeletnél nincs legnagyobb elem, és a `numbers[0]` **összeomlana** (`index out of bounds`). A `None` a típusban mondja ki, hogy "lehet, hogy nincs eredmény", és a hívó a `match`-csel köteles kezelni.

### Feladat 5: Faktoriális (rekurzió)

Írj rekurzív függvényt `factorial(n: u64) -> u64` néven. Mennyi a `factorial(5)` és a `factorial(20)`?

> [!success]- Megoldás
> ```rust
> fn factorial(n: u64) -> u64 {
>     if n <= 1 {
>         1
>     } else {
>         n * factorial(n - 1)
>     }
> }
>
> fn main() {
>     println!("{}", factorial(5));
>     println!("{}", factorial(20));
> }
> ```
>
> Kimenet:
>
> ```
> 120
> 2432902008176640000
> ```
>
> Levezetés a `factorial(5)`-re (a hívások lefelé építkeznek, majd visszafelé szorzódnak):
>
> ```
> factorial(5) = 5 * factorial(4)
>              = 5 * (4 * factorial(3))
>              = 5 * (4 * (3 * factorial(2)))
>              = 5 * (4 * (3 * (2 * factorial(1))))
>              = 5 * 4 * 3 * 2 * 1
>              = 120
> ```
>
> A `factorial(1)` az **alapeset**: itt áll meg a rekurzió, enélkül a végtelenségig hívná magát. Miért `u64`? A `20!` már nem fér `i32`-be. A `21!` pedig a `u64`-be sem: debug módban a program `attempt to multiply with overflow` üzenettel leáll, ezért nagy bemenetnél `checked_mul` kell (2. fejezet).

### Feladat 6: Palindrom

Írj függvényt `is_palindrome(text: &str) -> bool` néven, ami figyelmen kívül hagyja a nagybetűket, a szóközöket és az írásjeleket. Az `"A man, a plan, a canal: Panama"` palindrom.

> [!success]- Megoldás
> ```rust
> fn is_palindrome(text: &str) -> bool {
>     let cleaned: Vec<char> = text
>         .chars()
>         .filter(|c| c.is_alphanumeric())
>         .map(|c| c.to_ascii_lowercase())
>         .collect();
>
>     let reversed: Vec<char> = cleaned.iter().rev().cloned().collect();
>     cleaned == reversed
> }
>
> fn main() {
>     println!("{}", is_palindrome("level"));
>     println!("{}", is_palindrome("rust"));
>     println!("{}", is_palindrome("A man, a plan, a canal: Panama"));
> }
> ```
>
> Kimenet:
>
> ```
> true
> false
> true
> ```
>
> Levezetés a harmadik példára:
>
> 1. `.filter(...)` megtartja a betűket és számokat: `AmanaplanacanalPanama`.
> 2. `.map(...)` kisbetűssé alakít: `amanaplanacanalpanama`.
> 3. Megfordítva ugyanez jön ki: `amanaplanacanalpanama`.
> 4. A két `Vec` egyenlő → `true`.
>
> A `rust` megfordítva `tsur`, ez nem egyenlő a `rust`-tal → `false`. A `Vec<char>` összehasonlítása `==`-gel elemenként történik.

### Feladat 7: FizzBuzz

Írd ki az 1-től 15-ig tartó számokat, de a 3-mal osztható szám helyett `Fizz`, az 5-tel osztható helyett `Buzz`, a mindkettővel (tehát 15-tel) osztható helyett `FizzBuzz` szerepeljen.

> [!success]- Megoldás
> ```rust
> fn main() {
>     for number in 1..=15 {
>         if number % 15 == 0 {
>             println!("FizzBuzz");
>         } else if number % 3 == 0 {
>             println!("Fizz");
>         } else if number % 5 == 0 {
>             println!("Buzz");
>         } else {
>             println!("{}", number);
>         }
>     }
> }
> ```
>
> Kimenet:
>
> ```
> 1
> 2
> Fizz
> 4
> Buzz
> Fizz
> 7
> 8
> Fizz
> Buzz
> 11
> Fizz
> 13
> 14
> FizzBuzz
> ```
>
> Miért a `% 15` az **első** feltétel? Az `if / else if` láncban **az első igaz ág fut le**, a többit a Rust már nem nézi. A 15 osztható 3-mal is és 5-tel is: ha a `% 3` állna elöl, a 15-nél `Fizz` jelenne meg, és a `FizzBuzz` ág sosem futna le. A feltételek sorrendje számít.

### Feladat 8: Borrow checker hiba

Miért nem fordul le, és hogyan javítanád?

```rust
fn main() {
    let mut scores = vec![10, 20, 30];
    let first = &scores[0];
    scores.push(40);
    println!("{}", first);
}
```

> [!success]- Megoldás
> A hiba: `error[E0502]: cannot borrow scores as mutable because it is also borrowed as immutable`.
>
> Miért veszélyes ez valójában? A `first` egy **hivatkozás** a Vec első elemére. A `push` néha **új, nagyobb memóriaterületet foglal** a Vec-nek, és átmásolja az elemeket. Ekkor a `first` a **régi, felszabadított** memóriára mutatna: lógó mutató (dangling pointer). C++-ban ez *undefined behavior* lenne, Rustban a fordító megfogja.
>
> **Javítás:** az `i32` `Copy` típus, ezért az értéket **kimásolhatod** hivatkozás helyett:
>
> ```rust
> fn main() {
>     let mut scores = vec![10, 20, 30];
>     let first = scores[0];      // érték, nem hivatkozás
>     scores.push(40);
>     println!("{}", first);
>     println!("{:?}", scores);
> }
> ```
>
> Kimenet:
>
> ```
> 10
> [10, 20, 30, 40]
> ```
>
> Másik lehetőség: **használd fel a hivatkozást a `push` előtt** (a hivatkozás az utolsó használatáig él, lásd a 10. fejezet NLL részét).

### Feladat 9: Érvényes számok összege

Írj függvényt `sum_valid(items: &[&str]) -> i32` néven, ami összeadja azokat az elemeket, amik számmá alakíthatók, a többit pedig kihagyja. Mennyi a `sum_valid(&["1", "x", "3"])`?

> [!success]- Megoldás
> ```rust
> fn sum_valid(items: &[&str]) -> i32 {
>     items
>         .iter()
>         .filter_map(|item| item.parse::<i32>().ok())
>         .sum()
> }
>
> fn main() {
>     println!("{}", sum_valid(&["1", "x", "3"]));
>     println!("{}", sum_valid(&["a", "b"]));
> }
> ```
>
> Kimenet:
>
> ```
> 4
> 0
> ```
>
> Levezetés az `["1", "x", "3"]`-ra:
>
> | elem | `parse::<i32>()` | `.ok()` | megmarad? |
> |---|---|---|---|
> | `"1"` | `Ok(1)` | `Some(1)` | igen |
> | `"x"` | `Err(...)` | `None` | **nem** |
> | `"3"` | `Ok(3)` | `Some(3)` | igen |
>
> Az `.ok()` a `Result`-ot `Option`-né alakítja (`Ok(v)` → `Some(v)`, `Err(_)` → `None`), a `filter_map` pedig csak a `Some` értékeket tartja meg és kicsomagolja. Összeg: `1 + 3 = 4`. Ha nincs érvényes elem, az üres összeg `0`.

### Feladat 10: Bankszámla (struct és Result)

Készíts `BankAccount` struct-ot `balance: u32` mezővel. Legyen `new`, `deposit(&mut self, amount)` és `withdraw(&mut self, amount) -> Result<(), String>`, ami hibát ad, ha nincs elég pénz a számlán.

> [!success]- Megoldás
> ```rust
> struct BankAccount {
>     balance: u32,
> }
>
> impl BankAccount {
>     fn new(balance: u32) -> Self {
>         BankAccount { balance }
>     }
>
>     fn deposit(&mut self, amount: u32) {
>         self.balance += amount;
>     }
>
>     fn withdraw(&mut self, amount: u32) -> Result<(), String> {
>         if amount > self.balance {
>             Err(format!(
>                 "insufficient funds: balance {}, requested {}",
>                 self.balance, amount
>             ))
>         } else {
>             self.balance -= amount;
>             Ok(())
>         }
>     }
> }
>
> fn main() {
>     let mut account = BankAccount::new(100);
>     account.deposit(50);
>
>     println!("{:?}", account.withdraw(30));
>     println!("{:?}", account.withdraw(500));
>     println!("{}", account.balance);
> }
> ```
>
> Kimenet:
>
> ```
> Ok(())
> Err("insufficient funds: balance 120, requested 500")
> 120
> ```
>
> Levezetés: kezdetben `100`, a betét után `150`. A `withdraw(30)` sikeres: `150 - 30 = 120`, ezért `Ok(())`. A `withdraw(500)`: `500 > 120`, ezért `Err(...)`, és az egyenleg **nem változik**: `120`.
>
> Miért `Result<(), String>`? A `()` azt jelenti, hogy sikernél nincs értelmes visszatérési érték, csak az, hogy sikerült. A `&mut self` kell, mert a metódus módosítja a számlát. A `u32` miatt a negatív egyenleg a típusban kizárt, de a kivonás előtti ellenőrzés így is kell: nélküle `u32` alulcsordulás (panic) lenne.

### Haladásod

- [ ] Megoldottam az 1–4. feladatot (alapok)
- [ ] Megoldottam az 5–7. feladatot (ciklus, rekurzió)
- [ ] Megoldottam a 8–10. feladatot (ownership, hibakezelés, struct)
- [ ] A hibás kódokat (3., 8.) **előbb magam javítottam ki**, csak utána néztem a megoldást

---

## 19. Gyakori fordítói hibák

A Rust fordítója a hibákat **kóddal** és **javaslattal** látja el (`error[E0382]`, `help: ...`). A leggyakoribbak:

| Hibakód / üzenet | Jelentése | Tipikus megoldás |
|---|---|---|
| `E0384` cannot assign twice to immutable variable | nem `mut` a változó | `let mut x = ...` |
| `E0382` borrow of moved value | már átadtad a tulajdonjogot | `.clone()`, vagy kölcsönzés `&`-lel |
| `E0502` cannot borrow as mutable because it is also borrowed as immutable | olvasó hivatkozás él, miközben módosítanál | rövidítsd az olvasó hivatkozás élettartamát |
| `E0499` cannot borrow as mutable more than once | két `&mut` egyszerre | csak egyet tarts egyszerre |
| `E0308` mismatched types | rossz típus, vagy felesleges `;` | `as` konverzió / távolítsd el a `;`-t |
| `E0106` missing lifetime specifier | nem derül ki a hivatkozás élettartama | lifetime-jelölés, vagy értéket adj vissza |
| `E0597` does not live long enough | a hivatkozás tovább él, mint az adat | az adat hatóköre legyen szélesebb |
| `E0004` non-exhaustive patterns | a `match` nem fed le minden esetet | add hozzá a hiányzó ágat / `_ =>` |
| `E0277` the trait bound is not satisfied | a típus nem tud valamit (pl. nincs `Display`) | `#[derive(...)]` vagy `impl Trait for Típus` |

> [!tip] Hogyan olvasd a hibaüzenetet?
> 1. Nézd meg a **hibakódot és a rövid leírást** az első sorban.
> 2. Nézd meg a **nyilakkal jelölt sort**, ott van a pontos hely.
> 3. Keresd a **`help:`** sort: gyakran szó szerint megmondja a javítást.
> 4. A teljes magyarázathoz: `rustc --explain E0382`.

---

## 20. Megfelelési táblázat a hivatalos tematikához

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

A 10–17. fejezetek túlmutatnak egy bevezető kurzuson — ezek adják a Rustot ténylegesen megkülönböztető, saját fogalmi világot: ownership, borrowing, lifetime-ok, trait-ek, smart pointerek.

### A négy nyelv egy pillantásra

| Kérdés | Rust | C++ | Python | C# |
|---|---|---|---|---|
| Változó | `let x = 5;` | `auto x = 5;` | `x = 5` | `var x = 5;` |
| Módosítható változó | `let mut x = 5;` | `int x = 5;` | `x = 5` | `var x = 5;` |
| Konstans | `const X: i32 = 5;` | `constexpr int X = 5;` | `X = 5` (konvenció) | `const int X = 5;` |
| Függvény | `fn add(a: i32) -> i32` | `int add(int a)` | `def add(a):` | `int Add(int a)` |
| Ciklus | `for i in 0..5` | `for (int i=0;i<5;i++)` | `for i in range(5):` | `for (int i=0;i<5;i++)` |
| Nincs érték | `Option<T>` | `std::optional` | `None` | `null` |
| Hiba | `Result<T, E>` | `try/catch` | `try/except` | `try/catch` |
| Lista | `Vec<T>` | `std::vector<T>` | `list` | `List<T>` |
| Szótár | `HashMap<K, V>` | `std::map` / `unordered_map` | `dict` | `Dictionary<K, V>` |
| Kiírás | `println!("{}", x);` | `std::cout << x;` | `print(x)` | `Console.WriteLine(x);` |
| Memória | ownership | RAII / `new`+`delete` | szemétgyűjtő | szemétgyűjtő |
| Öröklés | nincs (trait) | van | van | van |

---

## Hogyan használd ezt a könyvet

1. Olvasd át az 1–9. fejezetet — ez nagyrészt ismerős lesz C#-ból, Pythonból vagy C++-ból, csak a szintaxis, a `mut` és az `Option`/`Result` az újdonság.
2. **A 10–11. fejezet (ownership, borrowing, lifetime-ok) az, amit ne siess át.** Ez a Rust lelke; minden későbbi fogalom (metódusok `&self`-je, smart pointerek, trait-ek) erre épül. Az első két-három nap harcolni fogsz a fordítóval — ez teljesen normális, és gyorsan elmúlik.
3. 12–14. fejezet, amikor strukturált, újrahasznosítható kódot kezdesz írni.
4. 15–17. fejezet, amikor már komolyabb, tesztelt programot építesz.
5. **18. fejezet: a feladatokat oldd meg magad**, mielőtt a megoldást kinyitod. A programozást csak írással lehet megtanulni.

> [!success] Gyakorló tipp
> Írd be a példákat **magad** a `cargo run`-nal, és **szándékosan rontsd el őket** (vedd ki a `mut`-ot, használj egy változót move után). A fordító hibaüzenetének elolvasása a leggyorsabb tanulási mód.

Ha egy konkrét hibaüzenetnél elakadsz (`E0382`, `E0502`, "does not live long enough"), hozd ide a pontos üzenetet és a kódot — a Rust hibaüzenetei elsőre hosszúnak tűnnek, de szinte mindig van bennük egy konkrét, megtalálható ok.

**További források:**

- 📖 The Rust Book: https://doc.rust-lang.org/book/
- 🧪 Rust by Example: https://doc.rust-lang.org/rust-by-example/
- 🏋️ Rustlings (gyakorló feladatok): https://github.com/rust-lang/rustlings
- ▶️ Online kipróbálás telepítés nélkül: https://play.rust-lang.org
