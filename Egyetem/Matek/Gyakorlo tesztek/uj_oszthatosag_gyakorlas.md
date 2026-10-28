---
title: Oszthatóság, kongruenciák, φ-függvény – ZH gyakorlás (füzet szerint)
tags:
  - discrete-math
  - number-theory
  - exam-prep
type: study-note
status: to-review
---

# 🔢 ZH gyakorlás – a füzet 1–8. feladata

> [!abstract] Felépítés
> A feladatok sorszáma megegyezik a füzet bal oldali számozásával. Mindegyiknél csak az van, **hogyan kell megcsinálni**.
> A válasz a lenyíló `Megoldás` dobozban van, előbb próbáld meg magad.

---

# 1️⃣ Osztók száma

> [!question] Feladat (füzet: 4410, 110)
> - Hány pozitív osztója van a $4410$-nek?
> - Ezek közül hány relatív prím $110$-hez?

**1. lépés – prímfelbontás** (oszd 2-vel, 3-mal, 5-tel, 7-tel, amíg lehet):

| Osztás | Hányados |
| --- | --- |
| $4410:2$ | $2205$ |
| $2205:3$ | $735$ |
| $735:3$ | $245$ |
| $245:5$ | $49$ |
| $49:7$ | $7$ |
| $7:7$ | $1$ |

$$4410=2^1\cdot3^2\cdot5^1\cdot7^2$$

**2. lépés – osztók száma:** minden kitevőhöz adj hozzá $1$-et, aztán szorozz:
$$(1+1)(2+1)(1+1)(2+1)=2\cdot3\cdot2\cdot3=36$$

**3. lépés – relatív prím $110$-hez:**
$$110=2\cdot5\cdot11$$
Egy osztó akkor relatív prím $110$-hez, ha **nincs benne $2$, $5$, $11$**.
- A $2$ és az $5$ kitevőjét **$0$-ra rögzítjük** (nincs választás).
- A $11$ nincs a $4410$-ben, nem számít.
- Marad a $3^a$ ($a=0,1,2$) és a $7^b$ ($b=0,1,2$):
$$(2+1)(2+1)=9$$

> [!success]- ✅ Megoldás
> **36** osztó, ebből **9** relatív prím $110$-hez.

> [!warning] Gyakori hiba
> A kitevő **nullára esik**, nem eggyel csökken. A kihúzott prím tényezője (1 lehetőség) nem szól bele a szorzatba.

---

# 2️⃣ gcd és lcm

> [!question] Feladat (füzet: 270, 396)
> Határozza meg $\gcd(270,396)$-ot és $\text{lcm}(270,396)$-ot.

**1. lépés – mindkettőt bontsd prímekre:**
$$270=2^1\cdot3^3\cdot5^1,\qquad 396=2^2\cdot3^2\cdot11^1$$
(396: $:2=198$, $:2=99$, $:3=33$, $:3=11$.)

**2. lépés:**
- **gcd:** minden prímből a **kisebb** kitevő (ha az egyik számban nincs meg, a kitevő $0$):
$$\gcd=2^1\cdot3^2=18$$
- **lcm:** minden prímből a **nagyobb** kitevő:
$$\text{lcm}=2^2\cdot3^3\cdot5\cdot11=4\cdot27\cdot55=5940$$

**Ellenőrzés:** $\gcd\cdot\text{lcm}=a\cdot b$, azaz $18\cdot5940=106920=270\cdot396$ ✅

> [!success]- ✅ Megoldás
> $\gcd=\mathbf{18}$, $\text{lcm}=\mathbf{5940}$

---

# 3️⃣ Megoldható-e? (kongruencia és diofantikus egyenlet)

> [!question] Feladat (füzet)
> Döntse el, megoldható-e (egészekben):
> - a) $16x\equiv20\pmod{28}$
> - b) $204x+372y=132$
> - c) $261x+333y=699$
> - d) $18x\equiv24\pmod{12}$

> [!info] A két szabály
> | Egyenlet | Mikor megoldható? |
> | --- | --- |
> | $ax\equiv b\pmod m$ | ha $\gcd(a,m)\mid b$ |
> | $ax+by=c$ | ha $\gcd(a,b)\mid c$ |
>
> Vagyis: számold ki a gcd-t, és nézd meg, **osztja-e a jobb oldalt**.

### a) $16x\equiv20\pmod{28}$ – gcd az **együttható és a modulus** között

Euklideszi algoritmus (mindig a **maradékkal** folytasd):

| Osztandó | Osztó | Hányados | Maradék |
| --- | --- | --- | --- |
| $28$ | $16$ | $1$ | $12$ |
| $16$ | $12$ | $1$ | $4$ |
| $12$ | $4$ | $3$ | $0$ |

$\gcd(16,28)=4$, és $4\mid20$ ✅ → **megoldható**.

### b) $204x+372y=132$ – gcd a **két együttható** között

| Osztandó | Osztó | Hányados | Maradék |
| --- | --- | --- | --- |
| $372$ | $204$ | $1$ | $168$ |
| $204$ | $168$ | $1$ | $36$ |
| $168$ | $36$ | $4$ | $24$ |
| $36$ | $24$ | $1$ | $12$ |
| $24$ | $12$ | $2$ | $0$ |

$\gcd=12$, és $132=11\cdot12$, tehát $12\mid132$ ✅ → **megoldható**.

> [!warning] A füzetben itt elcsúszott a lánc
> A füzetben $204=168\cdot1+24$ és $168=24\cdot7+12$ szerepel. Helyesen $204=168\cdot1+\mathbf{36}$, utána $168=36\cdot4+24$, $36=24+12$, $24=12\cdot2$. A végeredmény ($12$) ettől még jó, de ZH-n a láncot is nézhetik.

### c) $261x+333y=699$

| Osztandó | Osztó | Hányados | Maradék |
| --- | --- | --- | --- |
| $333$ | $261$ | $1$ | $72$ |
| $261$ | $72$ | $3$ | $45$ |
| $72$ | $45$ | $1$ | $27$ |
| $45$ | $27$ | $1$ | $18$ |
| $27$ | $18$ | $1$ | $9$ |
| $18$ | $9$ | $2$ | $0$ |

$\gcd=9$. Az oszthatóság 9-cel gyorsan ellenőrizhető a számjegyösszeggel: $6+9+9=24$, ez nem osztható $9$-cel → $9\nmid699$ ❌ → **nem megoldható**.

### d) $18x\equiv24\pmod{12}$

$\gcd(18,12)=6$, és $6\mid24$ ✅ → **megoldható**.

> [!success]- ✅ Megoldás
> a) ✅ megoldható, b) ✅ megoldható, c) ❌ **nem** megoldható, d) ✅ megoldható

> [!example] ⚡ Gyorsabb gcd
> Kis számoknál nem kell táblázat: keress közös osztót fejben. Pl. $204$ és $372$ is osztható $12$-vel ($204=12\cdot17$, $372=12\cdot31$), és $17$, $31$ már nem osztható közös számmal.

---

# 4️⃣ φ-függvény és nagy hatvány maradéka

> [!question] Feladat (füzet: 36, 55)
> - Adja meg $\varphi(36)$-ot.
> - Mennyi $55^{109}\bmod36$?
>
> (A füzetben a kitevő $109$-nek olvasható: $108+1$ látszik a levezetésben.)

### a) $\varphi(36)$

Prímfelbontás: $36=2^2\cdot3^2$ (különböző prímek: $2$ és $3$). Minden különböző prímre egy tényező:
$$\varphi(36)=36\cdot\left(1-\tfrac12\right)\left(1-\tfrac13\right)=36\cdot\tfrac12\cdot\tfrac23=12$$

### b) $55^{109}\bmod36$

**1. lépés – Euler-tétel:** ha $\gcd(a,m)=1$, akkor $a^{\varphi(m)}\equiv1\pmod m$.
$\gcd(55,36)=1$ ($55=5\cdot11$), tehát
$$55^{12}\equiv1\pmod{36}$$

**2. lépés – bontsd a kitevőt $\varphi$ többszöröse + maradék alakba:**
$$109=12\cdot9+1$$
$$55^{109}=\left(55^{12}\right)^{9}\cdot55^1\equiv1^9\cdot55\pmod{36}$$

**3. lépés – redukáld az alapot:** $55-36=19$, tehát $55\equiv19\pmod{36}$.

> [!success]- ✅ Megoldás
> $\varphi(36)=\mathbf{12}$, $55^{109}\equiv\mathbf{19}\pmod{36}$

> [!warning] Gyakori hiba
> Ne a $\varphi$-vel oszd a kitevőt úgy, hogy elfelejted a maradékot. Ha a kitevő mondjuk $104$ lenne: $104=12\cdot8+8$, akkor $55^8$ maradna, azt már külön ki kell számolni.

---

# 5️⃣ Oszthatóság 8-cal

> [!question] Feladat (füzet: $3472a6$)
> Milyen $a$ számjegy esetén osztható $3472a6$ 8-cal?

> [!info] Szabály 8-cal
> Egy szám akkor osztható $8$-cal, ha az **utolsó három számjegyéből álló szám** osztható $8$-cal. (Mert $1000=8\cdot125$, a szám többi része mindig osztható $8$-cal.)

**1. lépés:** az utolsó három számjegy: $2a6$.

**2. lépés – próbáld végig az $a$-kat** (a füzet is ezt csinálja): $2a6$ osztható-e $8$-cal?

| $2a6$ | $:8$ | Jó? |
| --- | --- | --- |
| $206$ | $25{,}75$ | ❌ |
| $216$ | $27$ | ✅ |
| $226$ | $28{,}25$ | ❌ |
| $236$ | $29{,}5$ | ❌ |
| $246$ | $30{,}75$ | ❌ |
| $256$ | $32$ | ✅ |
| $266$ | $33{,}25$ | ❌ |
| $276$ | $34{,}5$ | ❌ |
| $286$ | $35{,}75$ | ❌ |
| $296$ | $37$ | ✅ |

> [!success]- ✅ Megoldás
> $a\in\{1,\,5,\,9\}$ (a füzetben: $216:8=27$, $256:8=32$, $296:8=37$)

> [!example] ⚡ Gyorsabban, mind a tízet nem kell kipróbálni
> $2a6=200+10a+6$. A $200=8\cdot25$ osztható $8$-cal, tehát $10a+6$-nak kell oszthatónak lennie $8$-cal. $10a+6\in\{16,\,56,\,96\}$ jó ($a=1,5,9$), a többi nem. Minden $4$. lépésben jön egy megoldás: $1,5,9$.

---

# 6️⃣ Kongruenciák megoldása

> [!question] Feladat (füzet)
> - a) $x\equiv3\pmod{11}$: a megoldások felsorolása, a legkisebb a feltételnek megfelelő ($>20$) megoldás.
> - b) $8x\equiv30\pmod{13}$ megoldása.

### a) $x\equiv3\pmod{11}$

A megoldások $x=3+11k$. Adj hozzá $11$-et lépésenként:
$$3,\;14,\;25,\;36,\dots$$
A füzetben $25$-nél áll meg ($25\equiv3\pmod{11}$, mert $25-22=3$). A $20$-nál nagyobb legkisebb megoldás: **25**.

### b) $8x\equiv30\pmod{13}$

**Megoldhatóság:** $\gcd(8,13)=1$, és $1\mid30$ → mindig van megoldás, **egyetlen** maradékosztály mod $13$.

**Módszer 1 – a jobb oldalhoz adj hozzá $13$-at, amíg osztható $8$-cal** (leggyorsabb):

| Jobb oldal | Osztható 8-cal? |
| --- | --- |
| $30$ | ❌ |
| $43$ | ❌ |
| $56$ | ✅ ($56:8=7$) |

$$8x\equiv56\pmod{13}\;\Rightarrow\;x\equiv7\pmod{13}$$

**Módszer 2 – a füzet módszere (kiterjesztett Euklidesz):** a $8x-13y=30$ alakot oldod meg.

1. Euklidesz: $13=8\cdot1+5$, $8=5\cdot1+3$, $5=3\cdot1+2$, $3=2\cdot1+1$, $2=1\cdot2+0$ → $\gcd=1$.
2. Visszafelé helyettesítve, hogy $1$-et kapj $8$ és $13$ kombinációjaként:
$$1=3-2=3-(5-3)=2\cdot3-5=2(8-5)-5=2\cdot8-3\cdot5=2\cdot8-3(13-8)=5\cdot8-3\cdot13$$
3. Tehát $8\cdot5\equiv1\pmod{13}$ (a $8$ inverze $5$). Szorozd be $30$-cal: $x_0=30\cdot5=150$.
4. Általános megoldás: $x=150-13t$. A legkisebb pozitív: $150-13\cdot11=150-143=7$.

**Ellenőrzés:** $8\cdot7=56=52+4$, és $30=26+4$ → mindkettő $4$ mod $13$ ✅

> [!success]- ✅ Megoldás
> a) **25**, b) $x\equiv\mathbf{7}\pmod{13}$ (a legkisebb pozitív: $7$)

---

# 7️⃣ $\varphi(40)$

> [!question] Feladat (füzet)
> Adja meg $\varphi(40)$ értékét.

**1. lépés – prímfelbontás:** $40=2^3\cdot5$. Különböző prímek: $2$ és $5$ (a kitevő a képletben nem számít).

**2. lépés – képlet:**
$$\varphi(40)=40\cdot\left(1-\tfrac12\right)\left(1-\tfrac15\right)=40\cdot\tfrac12\cdot\tfrac45=20\cdot\tfrac45=16$$

> [!success]- ✅ Megoldás
> $\varphi(40)=\mathbf{16}$

> [!example] ⚡ Gyorsabban
> Osztás $p$-vel, szorzás $(p-1)$-gyel: $40\xrightarrow{:2\cdot1}20\xrightarrow{:5\cdot4}16$.

---

# 8️⃣ Melyik szám felel meg $a\equiv18\pmod{19}$-nek?

> [!question] Feladat (füzet)
> Melyik szám ad $18$ maradékot $19$-cel osztva? A füzetben a vizsgált számok: $-173$, $-58$, $56$, $-20$, $151$.

**Módszer – hozd $0$ és $18$ közé** (negatívnál adj hozzá $19$ többszörösét, pozitívnál vonj ki):

| Szám | Átalakítás | Maradék | Jó? |
| --- | --- | --- | --- |
| $-173$ | $-173+190$ | $17$ | ❌ |
| $-58$ | $-58+76$ | $18$ | ✅ |
| $56$ | $56-38$ | $18$ | ✅ |
| $-20$ | $-20+38$ | $18$ | ✅ |
| $151$ | $151-133$ | $18$ | ✅ |

> [!success]- ✅ Megoldás
> $-173$ **nem** jó (maradék $17$). A $-58$, $56$, $-20$, $151$ jó.

> [!example] ⚡ Gyorsabban: $18\equiv-1\pmod{19}$
> $a\equiv18$ pontosan akkor, ha **$a+1$ osztható $19$-cel**.
> $-58+1=-57=-3\cdot19$ ✅, $56+1=57=3\cdot19$ ✅, $-20+1=-19$ ✅, $151+1=152=8\cdot19$ ✅, de $-173+1=-172$, és $172:19$ nem egész ❌.

> [!warning] Gyakori hiba
> Negatív számnál ne a „szokásos" maradékot vedd fejben: **mindig adj hozzá $19$-es többszörösöket**, amíg $0$ és $18$ közé nem esik.

---

# 📌 Csalólap

| Téma | Amit tudni kell |
| --- | --- |
| Prímfelbontás | oszd 2-vel, 3-mal, 5-tel, 7-tel… |
| Osztók száma | kitevő $+1$, szorzás |
| Relatív prím osztók | a közös prímek kitevője $0$-ra rögzül, a többire szorzás |
| $\gcd$ / $\text{lcm}$ | kisebb / nagyobb kitevők, vagy Euklidesz, $\text{lcm}=\frac{ab}{\gcd}$ |
| $\varphi(n)$ | $n\cdot\prod\left(1-\frac1p\right)$, csak a **különböző** prímekre |
| Megoldható, $ax\equiv b\pmod m$ | $\gcd(a,m)\mid b$ |
| Megoldható, $ax+by=c$ | $\gcd(a,b)\mid c$ |
| Oszthatóság $3$, $9$ | számjegyösszeg |
| Oszthatóság $8$ | utolsó 3 számjegy |
| Lineáris kongruencia | add hozzá $m$-et a jobb oldalhoz, amíg osztható $a$-val |
| Nagy hatvány | Euler: $a^{\varphi(m)}\equiv1$, kitevő $=\varphi\cdot q+r$ |
| Negatív szám mod $m$ | adj hozzá $m$ többszöröseit |
