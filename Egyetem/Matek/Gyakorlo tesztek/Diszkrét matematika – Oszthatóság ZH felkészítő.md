# Diszkrét matematika – Oszthatóság ZH felkészítő (konyhanyelven)

*Forrás: a 2. gyakorló teszt (Oszthatóság) kérdései és a téma szokásos anyaga. Cél: minden fogalom, minden jelölés, minden feladattípus lépésről lépésre, a lehető legegyszerűbben. A végén a teszt mind a 8 kérdése kidolgozva.*

## 0. Hogyan használd

1. Olvasd el a **túlélőcsomagot** (lent): a teljes téma 12 mondatban.
2. A **jelölésszótár** (1. fejezet) mindig kéznél legyen.
3. Menj sorban a fejezeteken. Mindegyik végén van **Recept** (mit csinálj lépésről lépésre) és **Csapda** (itt szokás elrontani).
4. A 9. fejezetben a teszt kérdései vannak kidolgozva. Takard le a megoldást, oldd meg magad, aztán ellenőrizd.

A teszt kérdéstípusai: beírandó szám (Answer), illetve **„Select one or more”**, vagyis több jó válasz is lehet. Ilyenkor **minden** jó válaszra rá kell kattintani, és egyetlen rosszra sem.

### A túlélőcsomag – 12 mondat

1. **a ∣ b** azt jelenti: b-t el lehet osztani a-val maradék nélkül, vagyis b = a·k valamilyen egész k-ra.
2. Minden szám felbontható **prímek szorzatára**, és csak egyféleképpen. Szinte minden feladat ebből indul.
3. **Osztók száma:** a prímfelbontásban minden kitevőhöz adj 1-et, és szorozd össze az eredményeket.
4. **lnko:** a közös prímek a **kisebb** kitevővel. **lkkt:** az összes prím a **nagyobb** kitevővel. És **lnko · lkkt = a · b**.
5. Nagy számoknál **Euklideszi algoritmus**: oszd el maradékosan, majd az osztót a maradékkal, és így tovább. Az utolsó nem nulla maradék az lnko.
6. **a ≡ b (mod m)** azt jelenti: m ∣ a − b, vagyis m-mel osztva ugyanazt a maradékot adják.
7. Kongruenciával **összeadni, kivonni, szorozni, hatványozni** szabad. **Osztani nem** (csak óvatosan).
8. **ax ≡ b (mod m)** pontosan akkor megoldható, ha **lnko(a, m) ∣ b**.
9. **ax + by = c** egész számokban pontosan akkor megoldható, ha **lnko(a, b) ∣ c**.
10. **φ(n)** = az 1, 2, …, n számok közül az n-hez **relatív prímek** száma. Képlet: n · ∏(1 − 1/p) a prímosztókra.
11. **Euler-tétel:** ha lnko(a, m) = 1, akkor a^φ(m) ≡ 1 (mod m). Nagy hatványnál ezzel kicsinyítjük a kitevőt.
12. **3-mal és 9-cel oszthatóság:** a számjegyek összege dönt.

## 1. Jelölésszótár – mi hogyan néz ki és mit jelent

| Jel | Olvasd így | Jelentése | Példa |
| --- | --- | --- | --- |
| a ∣ b | a osztja b-t | b = a·k valamilyen egész k-ra (a **osztója** b-nek, b **többszöröse** a-nak) | 6 ∣ 42, mert 42 = 6·7 |
| a ∤ b | a nem osztja b-t | nincs ilyen k | 4 ∤ 10 |
| ℤ, ℕ | egészek, természetes számok | ℤ = …, −2, −1, 0, 1, 2, … |  |
| a = q·b + r | maradékos osztás | q a hányados, r a maradék, 0 ≤ r < b | 47 = 5·9 + 2 |
| a mod m | a maradéka m-mel osztva | mindig 0 és m − 1 között | 17 mod 5 = 2 |
| a ≡ b (mod m) | a kongruens b-vel m szerint | m ∣ a − b; ugyanaz a maradékuk | 17 ≡ 2 (mod 5) |
| lnko(a, b), vagy (a, b) | legnagyobb közös osztó | angolul **gcd** (greatest common divisor) | lnko(12, 18) = 6 |
| lkkt(a, b), vagy \[a, b\] | legkisebb közös többszörös | angolul **lcm** (least common multiple) | lkkt(4, 6) = 12 |
| relatív prím | lnko(a, b) = 1 | nincs közös osztójuk 1-en kívül; angolul **coprime** | 8 és 15 |
| p prím | pontosan két pozitív osztója van: 1 és p | 2, 3, 5, 7, 11, 13, … (az 1 **nem** prím) |  |
| φ(n) | Euler-féle fi | az 1..n közül az n-hez relatív prímek száma | φ(10) = 4 (1, 3, 7, 9) |
| ∏ | szorzat (nagy pi) | „minden szóba jövő p-re szorozd össze” | ∏(1 − 1/p) |
| x₀, y₀, t | egy konkrét megoldás, illetve paraméter | az általános megoldásnál | x = x₀ + 11t |
| ⇔ | akkor és csak akkor |  |  |

**Csapda:** a **∣** (osztja) egy függőleges vonal, **nem** a törtjel (/). 6 ∣ 42 azt mondja ki, hogy „igaz”; 42/6 pedig egy szám (7).

## 2. Oszthatóság alapjai

### 2.1 Definíció és tulajdonságok

**a ∣ b**, ha van olyan **egész** k, hogy b = a·k.

Konyhai hasonlat: a b darab süteményt el tudod osztani a tálcára úgy, hogy mindegyikre pontosan ugyanannyi jut, és semmi nem marad.

| Tulajdonság | Jelentése | Példa |
| --- | --- | --- |
| 1 ∣ a, a ∣ a | az 1 és a szám maga mindig osztó |  |
| a ∣ 0 | a 0 minden szám többszöröse | 7 ∣ 0 |
| tranzitivitás | a ∣ b és b ∣ c ⇒ a ∣ c | 3 ∣ 12 és 12 ∣ 60 ⇒ 3 ∣ 60 |
| **kulcs-tulajdonság** | a ∣ b és a ∣ c ⇒ a ∣ (b·x + c·y) bármely egész x, y-ra | 5 ∣ 20 és 5 ∣ 35 ⇒ 5 ∣ (20·3 − 35·2) |
| a ∣ b és b ∣ a | ⇒ a = ±b |  |
| ha b ≠ 0 és a ∣ b | ⇒ a ≤ b (abszolút értékben) |  |

### 2.2 Maradékos osztás

Bármely a egész és b > 0 egész esetén **egyértelműen** van olyan q és r, hogy **a = q·b + r, és 0 ≤ r < b**.

**Recept:** 1) nézd meg, b hányszor fér a-ba, de **alulról** (lefelé kerekítve). 2) az r = a − q·b mindig 0 és b − 1 között lesz.

**Negatív szám esetén vigyázz!** −17 mod 5: lépj **lefelé** a következő többszörösre: −20 = (−4)·5. Ekkor −17 = (−4)·5 + 3, tehát −17 mod 5 = **3** (nem −2, és nem 2).

### 2.3 Oszthatósági szabályok (a jegyekből)

| Osztó | Mikor osztható egy szám vele? |
| --- | --- |
| 2 | az utolsó jegy páros |
| 3 | a **jegyek összege** osztható 3-mal |
| 4 | az utolsó **két** jegyből alkotott szám osztható 4-gyel |
| 5 | az utolsó jegy 0 vagy 5 |
| 6 | osztható 2-vel **és** 3-mal is |
| 8 | az utolsó **három** jegyből alkotott szám osztható 8-cal |
| 9 | a **jegyek összege** osztható 9-cel |
| 10 | az utolsó jegy 0 |
| 11 | a jegyek **váltakozó előjelű** összege osztható 11-gyel (jobbról balra: +, −, +, −, …) |

**Miért működik a 9-es szabály?** Mert 10 ≡ 1 (mod 9), ezért minden 10-hatvány is ≡ 1, és a szám ugyanazt a maradékot adja 9-cel, mint a jegyeinek összege.

**Recept hiányzó jegyes feladathoz** (pl. 2a4957 oszthatósága 9-cel): 1) add össze az ismert jegyeket: 2 + 4 + 9 + 5 + 7 = 27. 2) keresd azokat az a számjegyeket (0–9), amelyekkel az összeg osztható 9-cel: 27 + a ≡ 0 (mod 9) ⇒ a ≡ 0 (mod 9) ⇒ **a = 0 vagy a = 9**.

**Csapda:** a 6-ra **nem** a „jegyösszeg osztható 6-tal” a szabály (az 12-re igaz, de pl. a 15-re nem, 1 + 5 = 6, mégsem osztható 6-tal). A 6-hoz külön a 2 és a 3 szabályát kell ellenőrizni.

## 3. Prímek, prímfelbontás, osztók száma

### 3.1 Prímfelbontás

**Alaptétel:** minden 1-nél nagyobb egész szám **egyféleképpen** (a sorrendtől eltekintve) felírható prímek szorzataként. Például 60 = 2² · 3 · 5.

**Recept:**

1. Oszd el a számot 2-vel, ameddig tudod. Ugyanígy 3-mal, 5-tel, 7-tel, 11-gyel…
2. Írd le a kitevőket. Addig menj, amíg a maradék szám 1 nem lesz.
3. Ellenőrizd: szorozd vissza.

**Gyorstrükk:** ha a szám szorzat alakban adott, bontsd a tényezőket külön. 25200 = 252 · 100 = (2² · 3² · 7) · (2² · 5²) = **2⁴ · 3² · 5² · 7**.

### 3.2 Osztók száma

Ha n = p₁^a₁ · p₂^a₂ · … · p\_k^a\_k, akkor n pozitív osztóinak száma **(a₁ + 1) · (a₂ + 1) · … · (a\_k + 1)**.

**Miért?** Egy osztóban az első prím kitevője 0, 1, …, a₁ lehet (a₁ + 1 lehetőség), a másodiké a₂ + 1 lehetőség, és ezek egymástól függetlenül választhatók.

**Példa:** 25200 = 2⁴ · 3² · 5² · 7¹. Osztók száma: (4 + 1)(2 + 1)(2 + 1)(1 + 1) = 5 · 3 · 3 · 2 = **90**.

**Osztók összege (extra):** ∏(1 + p + p² + … + p^a). Például 12 = 2² · 3: (1 + 2 + 4)(1 + 3) = 7 · 4 = 28 (1 + 2 + 3 + 4 + 6 + 12 = 28 ✓).

### 3.3 Azok az osztók, amelyek relatív prímek egy másik számhoz

**Gondolat:** egy osztó pontosan akkor relatív prím a másik számhoz, ha **nem tartalmaz egyetlen olyan prímet sem**, ami a másik szám prímfelbontásában is szerepel.

**Recept** (25200 osztói, amelyek relatív prímek 66-tal):

1. Bontsd a 66-ot: 66 = 2 · 3 · 11.
2. Húzd ki az 25200 prímfelbontásából azokat, amik a 66-ban is vannak: a 2-t és a 3-at. A 11 nincs az 25200-ban, azzal nem kell foglalkozni.
3. Marad: 5² · 7. Ennek osztói jók. Számuk: (2 + 1)(1 + 1) = **6**.

**Csapda:** ez **nem** φ(25200)! A φ azt számolja, hány szám 1 és n között relatív prím n-hez, itt viszont az **osztók** közül kell válogatni.

## 4. Legnagyobb közös osztó és legkisebb közös többszörös

### 4.1 Prímfelbontásból

- **lnko:** minden közös prím, a **kisebbik** kitevővel.
- **lkkt:** minden szereplő prím, a **nagyobbik** kitevővel.
- **lnko(a, b) · lkkt(a, b) = a · b** (pozitív a, b-re). Ez ellenőrzésnek is jó.

**Példa:** 12 = 2² · 3 és 18 = 2 · 3². lnko = 2¹ · 3¹ = 6. lkkt = 2² · 3² = 36. Ellenőrzés: 6 · 36 = 216 = 12 · 18 ✓.

Konyhai hasonlat: az lnko az a legnagyobb „doboz”, amibe mindkét szám darabjai pontosan beleférnek. Az lkkt az a legkisebb szám, ami mindkettőnek többszöröse.

### 4.2 Euklideszi algoritmus (nagy számoknál, amikor a prímfelbontás kényelmetlen)

**Elv:** lnko(a, b) = lnko(b, a mod b). Ismételd, amíg a maradék 0 nem lesz. Az **utolsó nem nulla maradék** az lnko.

**Példa: lnko(540, 462)**

| Lépés | Osztás | Maradék |
| --- | --- | --- |
| 1 | 540 = 1 · 462 + **78** | 78 |
| 2 | 462 = 5 · 78 + **72** | 72 |
| 3 | 78 = 1 · 72 + **6** | 6 |
| 4 | 72 = 12 · 6 + **0** | 0 |

Az utolsó nem nulla maradék a 6, tehát lnko(540, 462) = **6**. Ekkor lkkt = 540 · 462 / 6 = 540 · 77 = **41580**.

**Csapda:** az lkkt-t mindig az **a·b / lnko** képlettel számold, ne próbálgasd.

### 4.3 Bővített Euklideszi algoritmus (Bézout-azonosság)

**Tétel:** minden a, b egész számhoz vannak olyan x, y egészek, hogy **a·x + b·y = lnko(a, b)**.

**Recept:** írd fel az Euklideszi algoritmus lépéseit, majd **visszafelé** helyettesítsd be a maradékokat.

**Példa:** 540 és 462, ahol lnko = 6.

- 6 = 78 − 72 (3. lépés)
- 72 = 462 − 5 · 78 (2. lépés), tehát 6 = 78 − (462 − 5 · 78) = 6 · 78 − 462
- 78 = 540 − 462 (1. lépés), tehát 6 = 6 · (540 − 462) − 462 = **6 · 540 − 7 · 462**
- Ellenőrzés: 3240 − 3234 = 6 ✓

## 5. Kongruencia

### 5.1 Mi ez?

**a ≡ b (mod m)** azt jelenti, hogy **m ∣ a − b**, vagyis a és b ugyanazt a maradékot adja m-mel osztva.

Konyhai hasonlat: a **faliórán** számolunk. 15 óra ≡ 3 óra (mod 12), mert 15 − 3 = 12, és 12 ∣ 12. Az óra körbefordul, a számok „visszaugranak”.

**Fontos átfogalmazás:** a ≡ r (mod m) ⇔ **a = m·k + r** valamilyen egész k-ra. Ezt használod a „melyik szám lehet a?” típusú feladatoknál.

### 5.2 Számolási szabályok

Ha a ≡ b és c ≡ d (mod m), akkor:

- **a + c ≡ b + d** (összeadás)
- **a − c ≡ b − d** (kivonás)
- **a · c ≡ b · d** (szorzás)
- **a^k ≡ b^k** (hatványozás)

**Osztani NEM szabad csak úgy!** Példa: 2 · 7 ≡ 2 · 1 (mod 6) (14 és 2 is 2 maradékot ad 6-tal), de 7 ≢ 1 (mod 6).

Osztás csak akkor, ha **lnko(c, m) = 1** (akkor c-vel szabad „egyszerűsíteni”), vagy ha az m-et is leosztod: **c·a ≡ c·b (mod m) ⇒ a ≡ b (mod m / lnko(c, m))**.

### 5.3 „Melyik értéket veheti fel a?” (a ≡ r mod m típus)

**Recept:**

1. Számold ki **a − r**-t.
2. Nézd meg, hogy az osztható-e m-mel. Ha igen, akkor a jó.
3. Negatív számnál is ugyanígy működik (−88 − 8 = −96 = −6 · 16 ✓).

**Gyors ellenőrzés maradékkal:** hozd a számot a 0 és m − 1 közé úgy, hogy m többszöröseit adod hozzá: −101 + 112 = 11, ez nem 8, tehát −101 ≢ 8 (mod 16).

**Csapda:** a negatív számoknál ne hagyd ki, hogy a maradék **nem negatív** (a −134 maradéka 16-tal osztva 10, nem −6).
