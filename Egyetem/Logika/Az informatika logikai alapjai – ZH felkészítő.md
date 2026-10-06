# Az informatika logikai alapjai – ZH felkészítő (konyhanyelven)

*Forrás: a kiskáté, a 2–4. előadás, a 2–4. gyakorlat és a saját jegyzeteid. Cél: minden jelölés, minden fogalom, minden feladattípus – lépésről lépésre, a lehető legegyszerűbben.*

## 0. Hogyan használd

1. Először olvasd el a **túlélőcsomagot** (lent). Ez a teljes tantárgy 12 mondatban.
2. Utána jön a **jelölésszótár** (1. fejezet): ha egy jelnél elakadsz, ide lapozz vissza.
3. Menj sorban a fejezeteken. Mindegyik végén van **Recept** (mit csinálj lépésről lépésre) és **Csapda** (itt szokás elrontani).
4. A végén van a **ZH-lapozó** és a kidolgozott feladatok gyűjteménye. Takard le a megoldást, oldd meg magad, aztán ellenőrizd.

Jelmagyarázat: **Konyhai hasonlat** = hogy megértsd; **Recept** = lépések; **Csapda** = tipikus hiba; **Angolul** = a szakszó angol párja.

### A túlélőcsomag – 12 mondat

1. A logika két dolgot tanít: mikor **igaz** egy állítás, és mikor **következik** valami valamiből.
2. Három szint van: **szintaxis** (hogyan írjuk le helyesen – ez a nyelvtan), **szemantika** (mikor igaz – ez a jelentés), **kalkulus** (hogyan bizonyítunk gépiesen, szabályokkal).
3. Mindent két építőkockából rakunk össze: **nevek/termek** (dolgokra mutatnak: Péter, anyja(Juli)) és **formulák** (állítások, amik igazak vagy hamisak: elmegy(Juli)).
4. A formulákat **logikai jelekkel** kötjük össze: ¬ (nem), ∧ (és), ∨ (vagy), ⊃ (ha…akkor), ≡ (akkor és csak akkor), ∀ (minden), ∃ (van olyan).
5. Egy formula önmagában nem igaz vagy hamis. Kell hozzá egy **interpretáció**: megmondjuk, kikről beszélünk (univerzum), és melyik jel mit jelent.
6. Az igazságérték mindig **0 (hamis) vagy 1 (igaz)**.
7. **Modell** = olyan interpretáció, amelyben a formula (vagy a formulahalmaz minden eleme) igaz.
8. **Kielégíthető** = van modellje. **Érvényes** (logikai törvény, tautológia) = minden interpretációban igaz. **Kielégíthetetlen** = nincs modellje, sehol nem igaz.
9. **Γ ⊨ A** (a Γ-ból következik A) = ahol Γ minden eleme igaz, ott A is igaz. Másképp: a Γ ∪ {¬A} halmaz kielégíthetetlen.
10. **A ⇔ B** (logikailag ekvivalens) = minden interpretációban ugyanaz az értékük.
11. Kvantoroknál: **∀ mellé ⊃ való, ∃ mellé ∧**. „Minden egyetemista sportol” = ∀x(E(x) ⊃ S(x)); „van sportoló egyetemista” = ∃x(E(x) ∧ S(x)).
12. A bizonyítások nagy része **indirekt**: tegyük fel az ellenkezőjét, vegyünk hozzá egy modellt, és fussunk bele egy ellentmondásba.

## 1. Jelölésszótár – mi hogyan néz ki és mit jelent

### 1.1 Logikai jelek (ezek mindig ugyanazt jelentik)

| Jel | Neve | Olvasd így | Mikor igaz? | Konyhai hasonlat |
| --- | --- | --- | --- | --- |
| ¬A | negáció (tagadás) | nem A | ha A hamis | villanykapcsoló: átbillenti |
| A ∧ B | konjunkció | A és B | csak ha **mindkettő** igaz | két kulcs kell a páncélszekrényhez |
| A ∨ B | diszjunkció | A vagy B (megengedő!) | ha **legalább az egyik** igaz; csak a 0-0 esetben hamis | „kávé vagy tea”, de mindkettőt is lehet kérni |
| A ⊃ B | implikáció, kondicionális (más jelölés: →, ⇒) | ha A, akkor B | **csak az 1→0 esetben hamis**, minden másban igaz | ígéret: „ha esik, viszek esernyőt”. Csak akkor hazudtál, ha esik és nem vittél |
| A ≡ B | ekvivalencia (más jelölés: ↔) | A akkor és csak akkor, ha B | ha A és B értéke **egyezik** | két kapcsoló, ami mindig együtt áll |
| ∀x A | univerzális kvantor | minden x-re A | ha **minden** elemre igaz | „mindenki hozott tollat” |
| ∃x A | egzisztenciális kvantor | van olyan x, hogy A | ha **legalább egy** elemre igaz | „van, aki hozott tollat” |
| ( ) | zárójel |  |  | megmondja, mi mit fog össze |
| t₁ = t₂ | egyenlőség (csak a kiskátéban, elsőrendűben) | t₁ egyenlő t₂-vel | ha a két név ugyanarra a dologra mutat | két név ugyanarra az emberre |

### 1.2 Nem logikai jelek (ezeket mi találjuk ki a feladathoz)

| Jel | Mi ez? | Példa |
| --- | --- | --- |
| p, q, r, X, Y, Z | állításjel: egy egész, tovább nem bontott állítás | p = „havazik” |
| a, c, u, Péter, Éva | névjel (F⁽⁰⁾): egy konkrét dolog neve | J = Juli |
| f(·), anyja(·) | függvényjel (F⁽ⁿ⁾): ad egy **dolgot** vissza | anyja(Juli) = Juli anyja |
| P(·), E(·), S(·) | predikátumjel (P⁽ⁿ⁾): egy tulajdonság, belőle **állítás** lesz | E(x): x egyetemista |
| Q(·,·), R(·,·) | kétargumentumú predikátum: kapcsolat két dolog között | szereti(x,y) |
| x, y, z | változó: „üres hely”, amibe valamilyen dolgot képzelünk | ∀x P(x) |

**Fontos különbség:** a függvény **dolgot** ad vissza (anyja(Juli) egy ember), a predikátum **igaz/hamis** állítás lesz (elmegy(Juli)). Konyhán: a függvény egy gép, ami kiad egy hozzávalót; a predikátum egy kérdés, amire igen/nem a válasz.

### 1.3 Halmazok és kapcsolódó jelek

| Jel | Jelentése | Példa |
| --- | --- | --- |
| ∈ | eleme | A ∈ Γ: az A formula benne van a Γ halmazban |
| ⊆ | részhalmaza | Δ ⊆ Γ |
| ∅ | üres halmaz |  |
| ∪ | unió (egyesítés) | Γ ∪ {A} = Γ plusz az A. Gyakran csak így írják: Γ, A |
| ∩ | metszet (közös rész) | LC ∩ Con = ∅ |
| ∖ | különbség | FreeVar(B) ∖ {x}: vedd ki az x-et |
| { } | halmaz | {p, ¬q} |
| Γ, Δ | formulahalmazok (nagy gamma, nagy delta) |  |
| U | univerzum: a világ, amiről beszélünk | U = {u₁, u₂} |
| Uⁿ | n-esek az univerzumból | U² = az összes pár |
| Dom(ρ) | értelmezési tartomány: amire ρ meg van adva | Dom(ρ) = Con |
| ⟨ , ⟩ | rendezett pár/hármas | ⟨U, ρ⟩ |

### 1.4 A nyelv részei

| Jel | Jelentése |
| --- | --- |
| L⁽⁰⁾, L⁽¹⁾ | nulladrendű, illetve elsőrendű nyelv |
| LC | logikai konstansok halmaza (Logical Constants): ¬, ⊃, ∧, ∨, ≡, ∀, ∃, zárójelek |
| Var | változók halmaza (Variables) |
| Con | nem logikai konstansok halmaza (Constants) |
| F⁽⁰⁾, F⁽ⁿ⁾ | névjelek, n-argumentumú függvényjelek |
| P⁽⁰⁾, P⁽ⁿ⁾ | állításjelek, n-argumentumú predikátumjelek |
| Term | termek (összetett nevek) halmaza |
| Form | formulák (összetett állítások) halmaza |
| AtForm | atomi formulák halmaza |
| RF(A) | az A részformuláinak halmaza (Subformulas) |
| FreeVar(A), BoundVar(A) | A szabad, illetve kötött változói |
| Cong(A) | az A-val kongruens formulák halmaza |

### 1.5 Szemantika jelei

| Jel | Jelentése | Példa |
| --- | --- | --- |
| ρ (ró) | értelmezés: megmondja, melyik jel mit jelent | ρ(p) = 1, ρ(Péter) = lakó1 |
| ⟨U, ρ⟩ | interpretáció (nulladrendűben elég a ρ; a feladatsorokban ℐ-vel is jelölik) |  |
| \|A\|ρ vagy \|A\|^⟨U,ρ⟩ | az A formula igazságértéke (0 vagy 1) az adott interpretációban | \|p ∧ q\|ρ |
| v | értékelés: minden változóhoz egy U-beli elemet rendel | v(x) = lakó3 |
| v\[x:u\] | ugyanaz a v, de az x értéke most u | „x legyen u” |
| 0 / 1 | hamis / igaz (más jelölés: F / T, h / i) |  |

### 1.6 Kapcsolat-jelek (formulák között)

| Jel | Jelentése | Hogyan olvasd |
| --- | --- | --- |
| Γ ⊨ A | szemantikai (logikai) következmény | ahol Γ minden eleme igaz, ott A is igaz |
| Γ ⊭ A | nem következménye |  |
| ⊨ A | A érvényes | a bal oldal üres: ∅ ⊨ A. Vagyis A mindig igaz |
| Γ ⊢ A | szintaktikai következmény (levezethető) | a szabályokkal le lehet vezetni |
| Cns(Γ) | Γ összes szintaktikai következménye |  |
| A ⇔ B | logikai ekvivalencia | mindig ugyanaz az értékük |
| ⇒, ⟺ | magyarázó nyilak | a mi mondatainkban: „ha…akkor”, „akkor és csak akkor” (nem a formulában!) |
| \[A\]ᵗₓ | behelyettesítés | A-ban x helyére t |
| Γ, A ⊢ B | Γ ∪ {A} ⊢ B |  |
| A nand B | ¬(A ∧ B) | „not and”: tagadott és |

### 1.7 A legnagyobb csapda: négy „ha…akkor”

| Mi ez? | Hol él? | Mit jelent |
| --- | --- | --- |
| ⊃ | **a formulán belül**, egy logikai jel | az összetett formula része, igazságtáblája van |
| ≡ | a formulán belül | ugyanígy |
| ⊨ | **formulákról beszélünk vele** (metanyelv) | állítás arról, hogy A-ból következik B |
| ⇔ | formulákról beszélünk vele | állítás arról, hogy két formula mindig egyforma |
| ⊢ | formulákról beszélünk | levezethető |

Kapcsolat: **A ⊨ B akkor és csak akkor, ha ⊨ A ⊃ B**, és **A ⇔ B akkor és csak akkor, ha ⊨ A ≡ B**. (Ezt a 4. előadás tárgyalja, bizonyítás lent.)

## 2. A nagy kép

| Kérdés | Melyik szint | Eszköz |
| --- | --- | --- |
| Helyesen van leírva a formula? | **szintaxis** (Syntax) | zárójelek, szerkezeti fa |
| Igaz-e? Következik-e? | **szemantika** (Semantics) | interpretáció, igazságtábla, ⊨ |
| Gépiesen bizonyítható-e? | **kalkulus** (Calculus) | axiómák, leválasztás, ⊢ |

Konyhai hasonlat: a szintaxis a **recept helyesírása** (jó sorrendben, jó mennyiségekkel írtad le?), a szemantika a **megkóstolás** (finom-e, vagyis igaz-e?), a kalkulus a **receptkönyv szabályai** (csak engedélyezett lépéseket használhatsz).

## 3. Szintaxis – hogyan írunk le helyesen valamit

### 3.1 Két nyelv: nulladrendű és elsőrendű

**Nulladrendű** (angolul: propositional logic, ítéletlogika): L⁽⁰⁾ = ⟨LC, Con, Form⟩.

- LC = {¬, ⊃, ∧, ∨, ≡, (, )}
- Con = állításjelek (p, q, r, X, Y, Z…), legfeljebb megszámlálhatóan végtelen sok
- Nincs változó, nincs kvantor, nincs név.

**Elsőrendű** (angolul: first-order / predicate logic): L⁽¹⁾ = ⟨LC, Var, Con, Term, Form⟩. Ide jön még: változók, nevek, függvények, predikátumok, kvantorok.

Konyhai hasonlat: a nulladrendű olyan, mint egy halom cetli, amire csak annyit írunk, hogy „igaz” vagy „hamis”. Az elsőrendű olyan, mint egy recept, ahol hozzávalókra hivatkozunk és tulajdonságokat állítunk róluk.

**Formulák nulladrendben** (induktív definíció, vagyis „építőszabályok”):

1. Minden állításjel formula (Con ⊆ Form).
2. Ha A formula, akkor ¬A is formula.
3. Ha A és B formula, akkor (A ⊃ B), (A ∧ B), (A ∨ B), (A ≡ B) is formula.
4. Más semmi nem formula.

**Atomi formula** (prímformula) nulladrendben = az állításjel. Ezt már nem lehet tovább bontani.

### 3.2 Elsőrendű: nevek (termek) és állítások (formulák)

Con = F⁽⁰⁾ ∪ F⁽¹⁾ ∪ F⁽²⁾ ∪ … ∪ P⁽⁰⁾ ∪ P⁽¹⁾ ∪ P⁽²⁾ ∪ … , ahol

- F⁽⁰⁾ = névjelek (Péter, c, u)
- F⁽ⁿ⁾ = n argumentumú függvényjelek (anyja(·), f(·), g(·,·))
- P⁽⁰⁾ = állításjelek (havazik, p)
- P⁽ⁿ⁾ = n argumentumú predikátumjelek (özvegy(·), Q(·,·))
- A fenti halmazok páronként idegenek: egy jel nem lehet egyszerre kétféle.

**Term (összetett név) építőszabályai:**

1. Minden változó és minden névjel term (Var ∪ F⁽⁰⁾ ⊆ Term). Példa: x, Péter, u.
2. Ha f egy n-argumentumú függvényjel (n ≥ 1) és t₁, …, tₙ termek, akkor f(t₁, …, tₙ) is term. Példa: anyja(anyja(Péter)), f(x).

**Formula (összetett állítás) építőszabályai:**

1. Minden állításjel formula: P⁽⁰⁾ ⊆ Form. Példa: havazik, p.
2. Ha t₁, t₂ termek, akkor (t₁ = t₂) formula.
3. Ha P n-argumentumú predikátumjel és t₁, …, tₙ termek, akkor P(t₁, …, tₙ) formula. Példa: özvegy(Péter), P(x, c).
4. Ha A formula, akkor ¬A is.
5. Ha A, B formula, akkor (A ⊃ B), (A ∧ B), (A ∨ B), (A ≡ B) is.
6. Ha A formula és x változó, akkor ∀xA és ∃xA is.

**Atomi formulák** = az 1., 2., 3. pont szerintiek: állításjel, egyenlőség, predikátum termekkel. Ezek a legkisebb, tovább nem bontható állítások. Minden más **összetett formula**.

**Gyors teszt – mi micsoda?**

| Kifejezés | Mi ez? | Miért? |
| --- | --- | --- |
| Péter | term | névjel |
| anyja(Péter) | term | függvény + term |
| anyja | semmi | hiányzik az argumentum |
| elmegy(Péter) | formula (atomi) | predikátum + term |
| elmegy(anyja(Péter)) | formula (atomi) | predikátum + összetett term |
| anyja(elmegy(Péter)) | **hibás** | függvénynek csak term lehet az argumentuma, nem állítás |
| elmegy(szereti(Péter, Éva)) | **hibás** | predikátumnak csak term lehet az argumentuma |
| ∀x elmegy(x) | formula | kvantor + formula |

**Csapda:** függvény és predikátum argumentuma mindig **term**, soha nem formula. Állítást állítással csak ¬, ∧, ∨, ⊃, ≡ köt össze.

### 3.3 Fordítás: magyar mondat → formula

**Recept:**

1. Keresd meg az **alapállításokat** (amik tovább nem bonthatók).
2. Nevezd el a **neveket** (Juli = J, Éva = É) és a **tulajdonságokat/kapcsolatokat** (E(·) = elmegy, V(·) = visszajön).
3. A kötőszavakat cseréld logikai jelre (lásd táblázat).
4. Végül zárójelezz a mondat szerkezete szerint.

| Magyarul | Jel |
| --- | --- |
| nem, nincs, sem | ¬ |
| és, de, viszont, valamint, is | ∧ |
| vagy | ∨ |
| ha A, akkor B | A ⊃ B |
| csak akkor B, ha A | **B ⊃ A** (fordítva!) |
| A akkor és csak akkor, ha B | A ≡ B |
| sem A, sem B | ¬A ∧ ¬B |
| nem mindkettő | ¬(A ∧ B) |
| minden, mindenki | ∀ |
| van olyan, létezik, bizonyos | ∃ |

**Példa az előadásról:** „Juli anyja elmegy, és Éva itt marad, vagy mindketten elmennek, és Juli anyja vissza sem jön, de Éva vagy visszajön, vagy nem.”

- Nevek: J = Juli, É = Éva, a(·) = valakinek az anyja
- Predikátumok: E(·) = elmegy, IM(·) = itt marad, V(·) = visszajön
- Formula: **(E(a(J)) ∧ IM(É)) ∨ (((E(a(J)) ∧ E(É)) ∧ ¬V(a(J))) ∧ (V(É) ∨ ¬V(É)))**

### 3.4 Részformula, közvetlen részformula, szerkezeti fa

- **Közvetlen részformula:** a „közvetlenül alatta lévő” darab.
  - atomi formulának nincs
  - ¬A-nak az A
  - (A ∧ B), (A ∨ B), (A ⊃ B), (A ≡ B) esetén az A és a B
  - ∀xA-nak és ∃xA-nak az A
- **Részformula:** a formula önmaga, és minden közvetlen részformulájának részformulái (a legszűkebb ilyen halmaz). Jelölés: RF(A).
- **Szerkezeti fa** (Parse tree): a gyökér maga a formula; a gyerekek a közvetlen részformulák; a levelek atomi formulák.

**Recept a fa megrajzolásához:**

1. Keresd meg a **főjelet**: azt a jelet, amit **utoljára** hajtanánk végre (a leggyengébb, legkülső).
2. Ez lesz a gyökér. Alá a két (vagy egy) közvetlen részformula.
3. Ismételd, amíg atomi formulákhoz nem érsz.

**Példa:** ¬∃x(P(f(x)) ∨ Q(x, y))

- ¬∃x(P(f(x)) ∨ Q(x, y))
  - ∃x(P(f(x)) ∨ Q(x, y))
    - P(f(x)) ∨ Q(x, y)
      - P(f(x))
      - Q(x, y)

**Példa:** ((p ⊃ q) ≡ (¬p ⊃ ¬q))

- ≡ (főjel)
  - p ⊃ q → levelek: p, q
  - ¬p ⊃ ¬q → gyerekek: ¬p (alatta p), ¬q (alatta q)

**Csapda:** zárójelek nélkül a fa nem egyértelmű! A (p ⊃ q) ≡ (¬p ⊃ ¬q) és a p ⊃ (q ≡ ¬(p ⊃ ¬q)) teljesen más fa. Ezért kell a zárójel, vagy az erősorrend.

### 3.5 Zárójelek elhagyása – erősorrend (precedencia)

**Erősorrend (legerősebb → leggyengébb):**

| Rang | Jel | Megjegyzés |
| --- | --- | --- |
| 1. | ∀x, ∃x, ¬ | egyenrangúak; mindig a **közvetlenül utánuk álló legkisebb egységre** vonatkoznak |
| 2. | ∧ |  |
| 3. | ∨ |  |
| 4. | ⊃ |  |
| 5. | ≡ | a leggyengébb |

Konyhai hasonlat: ∧ olyan, mint a szorzás, ∨ mint az összeadás. Az a + b · c azt jelenti, hogy a + (b · c). Így p ∨ q ∧ r = p ∨ (q ∧ r). A ⊃ és ≡ még az összeadásnál is gyengébb.

**Szabályok (kiskáté 19. és 47.):**

1. A legkülső zárójelpár mindig elhagyható.
2. Az erősebb jel „magához húzza” a szomszédját: ha egy részformula főjele erősebb, mint a körülötte lévő jel, a zárójele elhagyható.
3. A negáció erősebb bármely kétargumentumú jelnél.
4. A kvantorok erősebbek bármely állításlogikai műveletnél. Az ∀ és az ∃ egyenrangú.
5. Azonos jelek egymás között: **itt eltér a két forrás!** A kiskáté azt írja, hogy balról jobbra csoportosítunk: p ⊃ q ⊃ r = (p ⊃ q) ⊃ r. Az előadás jegyzeted viszont úgy értelmezi, hogy p ⊃ q ⊃ r = p ⊃ (q ⊃ r) (a ∧-nál és ∨-nál mindegy, mert asszociatívak). **Tipp:** ha azonos jelek követik egymást, és számít a csoportosítás (főleg ⊃), **mindig tegyél zárójelet**, így nem lehet belekötni.

**Recept zárójel-elhagyáshoz:**

1. Töröld a legkülső zárójelpárt.
2. Nézd meg minden belső zárójelpárnál: a benne lévő főjel **erősebb**-e, mint a környező jel? Ha igen, elhagyható. Ha egyenlő vagy gyengébb, **marad**.
3. A negáció és a kvantor után zárójel marad, ha utána összetett kétargumentumú formula áll, amire vonatkozik.

**Csapda a kvantornál:** ∀x P(x) ∨ Q(x) azt jelenti, hogy (∀x P(x)) ∨ Q(x), tehát a Q(x)-beli x **szabad**! Ha a kvantor az egészre vonatkozik, ∀x(P(x) ∨ Q(x)).

Példák a gyakorlatból (1.I.5):

| Teljesen zárójelezve | Rövidítve |
| --- | --- |
| ((X ∨ Y) ⊃ Z) | X ∨ Y ⊃ Z |
| (¬(X ∨ Y) ⊃ Z) | ¬(X ∨ Y) ⊃ Z |
| ¬((X ∨ Y) ⊃ Z) | ¬(X ∨ Y ⊃ Z) |
| (((X ⊃ Y) ∧ (Y ⊃ Z)) ⊃ (¬X ∨ Z)) | (X ⊃ Y) ∧ (Y ⊃ Z) ⊃ ¬X ∨ Z |
| ((X ⊃ Y) ≡ (¬X ∨ Y)) | X ⊃ Y ≡ ¬X ∨ Y |
| (((X ∨ Y) ⊃ ¬Z) ≡ (X ∧ ¬Z)) | X ∨ Y ⊃ ¬Z ≡ X ∧ ¬Z |
| ¬(((X ⊃ Y) ⊃ (Y ∨ Z)) ⊃ (¬X ∨ Z)) | ¬(((X ⊃ Y) ⊃ Y ∨ Z) ⊃ ¬X ∨ Z) |

### 3.6 Formula-e? (1.I.3 feladat)

**Recept:** 1) minden kétargumentumú jel körül pontosan egy zárójelpár van-e? 2) a zárójelek párosak-e? 3) nincs-e két formula „egymás mellé dobva” jel nélkül?

| Sorozat | Formula? | Miért |
| --- | --- | --- |
| (X ∧ Y)¬Z | **nem** | két formula egymás mellett, nincs köztük jel |
| ((X ∧ Y) ⊃ Z) | **igen** | főjel ⊃, bal gyerek (X ∧ Y), jobb gyerek Z |
| (Z ∨ X ∧ ¬Y) | szigorúan **nem** | két kétargumentumú jel egy zárójelben, nem tudni, mi fog mit. Erősorrenddel (∧ erősebb) a (Z ∨ (X ∧ ¬Y)) rövidítése |
| (((¬X) ⊃ Y) ⊃ ¬(X ∨ Z)) | szigorúan **nem** (a (¬X) felesleges zárójele miatt) | a ¬X már önmagában formula, zárójel csak kétargumentumú jelhez jár. Hibátlan alak: ((¬X ⊃ Y) ⊃ ¬(X ∨ Z)) |
| ¬(X ∨ Y ⊃ ¬¬Z) | szigorúan **nem**, rövidítéssel igen | a ¬((X ∨ Y) ⊃ ¬¬Z) rövidítése |
| ¬(X ∨ Y ∧ ¬Z | **nem** | hiányzik a záró zárójel |

**Csapda:** az oktató szigorúságától függ, hogy a rövidítést elfogadja-e. Ha „formula-e?” a kérdés, válaszold meg a szigorú alapon, és írd mellé, hogy az erősorrenddel mi lenne.

**Szerkezeti fa példák (1.I.15):**

- ¬¬¬(X ⊃ Y): ¬ → ¬ → ¬ → ⊃ → (X, Y), egy lánc.
- (X ⊃ Y) ∨ (X ⊃ Y ∧ Z): főjel ∨; bal gyerek ⊃(X, Y); jobb gyerek ⊃(X, ∧(Y, Z)).
- X ∨ Y ⊃ ¬Z ∧ ¬(X ⊃ ¬Z): főjel ⊃; bal ∨(X, Y); jobb ∧(¬Z, ¬(⊃(X, ¬Z))).

### 3.7 Szabad és kötött változók

- **Hatáskör** (scope): a ∀x vagy ∃x után közvetlenül álló legkisebb formula.
- Egy változó egy előfordulása **kötött** (bound), ha beleesik egy ∀x B vagy ∃x B alakú részformulába (ugyanazzal a változóval). Ha nem, **szabad** (free).
- **Nyitott (nyilt) formula:** van benne legalább egy szabad előfordulás. **Zárt formula:** nincs benne szabad előfordulás. Csak a zárt formulának van „önmagában” igazságértéke, a nyitott értéke attól is függ, mit értünk a szabad változón.
- **FreeVar(A)** a szabad változók halmaza, **BoundVar(A)** a kötötteké. Szabályok: atomi formulában minden változó szabad, FreeVar(¬B) = FreeVar(B); kétargumentumú jelnél a két oldal uniója; FreeVar(∀xB) = FreeVar(B) ∖ {x}; BoundVar(∀xB) = BoundVar(B) ∪ {x}.

Konyhai hasonlat: a kvantor egy **keret**: a bele eső változó a kereten belül „megkötött”, ott értelmetlen kikérdezni, hogy ki az x. Ami a keretnél kívül van, az szabad, ott x egy konkrét, még meg nem adott dolog.

**Recept:** 1) húzd alá minden kvantor hatáskörét. 2) nézd végig az x-ek előfordulásait: kerete van, és a keret **ugyanazt a betűt** köti → kötött, különben szabad.

**Csapda:** ugyanaz a betű lehet egy formulán belül egyszer szabad, másszor kötött: ∃xP(x) ∧ P(x): az első x kötött, a második szabad.

**Kidolgozott példák (3.P.2)**

| Formula | Szabad előfordulások | FreeVar | Nyitott/zárt |
| --- | --- | --- | --- |
| ∃xP(x) ∧ P(x) | a második x | {x} | nyitott |
| ∀xQ(x,y) ⊃ ∀yQ(x,y) | bal oldali y, jobb oldali x | {x, y} | nyitott |
| ∃x∃y(Q(x,y) ∧ P(z)) | z | {z} | nyitott |
| ∀x(Q(x,c) ⊃ ∃yQ(x,y)) | nincs (c névjel, nem változó) | ∅ | **zárt** |
| ∀x(∀yR(x,y,z) ⊃ Q(x,y)) | z az R-ben; y a Q-ban (a ∀y csak az R-re vonatkozik) | {y, z} | nyitott |
| ∀y∃z(R(x,g(x,y),z) ⊃ ∃xQ(z,x)) | x az R-ben (a ∃x csak a Q-ra vonatkozik) | {x} | nyitott |
| ∃x∀y(P(x) ∨ Q(x,f(y)) ⊃ ∀yQ(x,y)) | nincs | ∅ | **zárt** |

A nem logikai konstansok (a „paraméterek”) pl. az utolsó formulában: P, Q, f.

**Fiktív kvantor:** ha az x nincs szabadon az A-ban, akkor ∀xA ⇔ A és ∃xA ⇔ A, mert a kvantor nem köt semmit (lásd később).

## 4. Szemantika – mikor igaz egy formula?

### 4.1 Interpretáció: „mi micsoda?”

A formula csak egy jelsor. Hogy igaz-e, azt csak akkor lehet eldönteni, ha megmondjuk, **mit jelentenek a jelek**.

**Nulladrendben** egyszerű: az interpretáció minden állításjelhez egy 0-t vagy 1-et rendel. Dom(ρ) = Con, ρ(p) ∈ {0, 1}. Példa: ρ(p) = 1, ρ(q) = 0 azt jelenti: „p igaz, q hamis”. Ha n állításjel van, 2ⁿ interpretáció létezik (ez az igazságtábla sorainak száma).

**Elsőrendben** az interpretáció egy pár, ⟨U, ρ⟩:

1. **U** az univerzum, **nem üres** halmaz: akikről/amikről beszélünk.
2. ρ a „sztár”, amely
   - minden névjelhez egy U-beli elemet rendel: ρ(a) ∈ U;
   - minden n-argumentumú függvényjelhez egy Uⁿ → U függvényt: ρ(f) : Uⁿ → U;
   - minden állításjelhez 0-t vagy 1-et: ρ(p) ∈ {0, 1};
   - minden n-argumentumú predikátumhoz egy relációt, azaz Uⁿ egy részhalmazát: ρ(P) ⊆ Uⁿ. Ez azoknak az n-eseknek a listája, amikre a predikátum **igaz**.

Konyhai hasonlat: az univerzum a **vendégek listája** egy bulin. A ρ a **névsoró**: megmondja, ki kicsoda („Péter = az 1. vendég”), és minden tulajdonsághoz kitölt egy listát arról, kikre igaz („tanul: az 1. és a 2. vendég”).

**Kidolgozott példa (előadás):** U = {lakó1, lakó2, lakó3, lakó4, …}. ρ(Péter) = lakó1, ρ(Juli néni) = lakó13, ρ(Mari néni) = lakó4. ρ(édesanyja): lakó2 → lakó4, lakó1 → lakó3. ρ(tanul) = {lakó1, lakó2}, ρ(dolgozik) = {lakó4}, ρ(munkatársak) = {(lakó2, lakó5), (lakó5, lakó2), (lakó3, lakó4), (lakó4, lakó3)}.

- tanul(Péter) → ρ(Péter) = lakó1 ∈ {lakó1, lakó2} → **1**
- dolgozik(Mari néni) → lakó4 ∈ {lakó4} → **1**
- tanul(édesanyja(Péter)) → édesanyja(lakó1) = lakó3 ∉ {lakó1, lakó2} → **0**
- munkatársak(Juli néni, Mari néni) → (lakó13, lakó4) nincs a listában → **0**

### 4.2 Szemantikai szabályok

**Nulladrendben** (A, B tetszőleges formulák):

- |p|ρ = ρ(p)
- |¬A|ρ = 1 − |A|ρ
- |A ∧ B|ρ = 1 pontosan akkor, ha mindkettő 1
- |A ∨ B|ρ = 0 pontosan akkor, ha mindkettő 0
- |A ⊃ B|ρ = 0 pontosan akkor, ha |A|ρ = 1 és |B|ρ = 0
- |A ≡ B|ρ = 1 pontosan akkor, ha |A|ρ = |B|ρ

**Az igazságtábla (ezt tudni kell fejből):**

| A | B | ¬A | A ⊃ B | A ∧ B | A ∨ B | A ≡ B |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | 1 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 1 | 0 | 1 | 0 |
| 1 | 0 | 0 | **0** | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 | 1 | 1 | 1 |

**Fejben tartási trükkök:**

- ∧: csak az 1-1 ad 1-et. Egyetlen 0 „belérontja”.
- ∨: csak a 0-0 ad 0-t. Egyetlen 1 „megmenti”.
- ⊃: **egyetlen** hamis sor van: 1 ⊃ 0. Ha az előtag 0 vagy az utótag 1, az egész biztosan 1.
- ≡: 1, ha a két oldal egyforma.
- ¬: megfordít.

**Miért igaz a 0 ⊃ valami?** Az ígéret-hasonlat: „ha esik, viszek esernyőt”. Ha nem esik, nem hazudtál, függetlenül attól, viszel-e esernyőt. A logikai ⊃ azt jelenti: **„nem lehet, hogy A igaz és B hamis”**, vagyis (A ⊃ B) ⇔ ¬(A ∧ ¬B) (ezt az előadáson definícióként fogadtuk el).

**Csapda:** a magyar „ha…akkor” gépi ⊃-t jelent: a hamis előtag mindig „igaz” eredményt ad. Ne akarj mellé ok-okozati kapcsolatot képzelni.

### 4.3 Hogyan tölts ki egy igazságtáblát (recept)

1. Számold meg az állításjeleket (n db). A sorok száma 2ⁿ.
2. Az oszlopok a **változók és a részformulák**, belülről kifelé (először a legbelső zárójelek).
3. Az eredmény oszlop a **főjel** alá kerül (a jegyzetedben is így csináltad: minden jel alá írod az értékét, a végén a legkülső jel oszlopa a válasz).
4. Az eredményoszlopból olvasd le:
   - csupa 1 → **érvényes** (logikai törvény)
   - csupa 0 → **kielégíthetetlen** (ellentmondás)
   - van 1 → **kielégíthető**; van 0 → **cáfolható**.

**Példa:** (X ⊃ Y) ⊃ (Y ⊃ X)

| X | Y | X ⊃ Y | Y ⊃ X | eredmény |
| --- | --- | --- | --- | --- |
| 0 | 0 | 1 | 1 | 1 |
| 0 | 1 | 1 | 0 | **0** |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 1 |

Van 1 → kielégíthető (pl. X=1, Y=0). Van 0 → **nem** érvényes (X=0, Y=1 cáfolja).

**Példa:** A ⊃ (¬A ⊃ B) (a jegyzetedből): minden sor 1 → érvényes. Tanulság: hamis előtagból bármi következik („robbanás elve”: ellentmondásból minden következik).

**Gyorsabb módszer, ha csak egy interpretációt kérdeznek** (5.I.6, 5.I.8): helyettesítsd be a 0/1 értékeket, és számolj belülről kifelé.

- X ∨ ¬Y ⊃ Z, ha X=1, Y=1, Z=0: (1 ∨ 0) = 1, 1 ⊃ 0 = **0**.
- ugyanez, ha X=0, Y=0, Z=1: (0 ∨ 1) = 1, 1 ⊃ 1 = **1**.
- X=0, Y=1 mellett: X ⊃ (Y ⊃ X) = 0 ⊃ (…) = **1**; ¬(Y ⊃ X) ∧ (X ∨ ¬Y) = ¬(1 ⊃ 0) ∧ (0 ∨ 0) = 1 ∧ 0 = **0**; ((¬X ⊃ Y) ⊃ X) ⊃ ¬Y = ((1 ⊃ 1) ⊃ 0) ⊃ 0 = (1 ⊃ 0) ⊃ 0 = 0 ⊃ 0 = **1**.

**Csünya kérdés (5.I.9, 5.I.10): mit tudunk részinformációból?**

- ¬X ⊃ (¬Y ∧ X) ∨ Z, ha Z=1: a jobb oldal ( ∨ Z) biztosan 1 → az egész ⊃ **igaz**.
- (X ⊃ Y) ⊃ (¬Y ⊃ ¬X), ha Y=1: ¬Y = 0, ezért ¬Y ⊃ ¬X = 1 → az egész **igaz**.
- ¬X ∧ Y ⊃ X ∨ Y, ha |X ⊃ Y| = 1: ha X=1, a jobb oldal 1. Ha X=0, a bal Y, a jobb Y → Y ⊃ Y = 1. **Igaz** (valójában a formula érvényes).

**Ugyanaz kódban** (ha értelmesebb neked, mint a szabályok): az értékelés egy rekurzív függvény, ami a szerkezeti fa levelektől a gyökér felé halad:

```python
def evaluate(formula, universe, rho, v):
    kind = formula['kind']
    if kind == 'atom':
        args = tuple(v.get(a, rho.get(a)) for a in formula['args'])
        return 1 if args in rho[formula['name']] else 0
    if kind == 'not':
        return 1 - evaluate(formula['sub'], universe, rho, v)
    if kind in ('forall', 'exists'):
        values = [evaluate(formula['sub'], universe, rho, {**v, formula['var']: u})
                  for u in universe]          # {**v, x: u} is v[x:u]
        return 1 if (all(values) if kind == 'forall' else any(values)) else 0
    left = evaluate(formula['left'], universe, rho, v)
    right = evaluate(formula['right'], universe, rho, v)
    if kind == 'and':     return 1 if left == 1 and right == 1 else 0
    if kind == 'or':      return 0 if left == 0 and right == 0 else 1
    if kind == 'implies': return 0 if left == 1 and right == 0 else 1
    if kind == 'equiv':   return 1 if left == right else 0
```

### 4.4 Elsőrendű szabályok: értékelés, kvantorok

- **Értékelés** (v): minden változóhoz hozzárendel egy U-beli elemet („x most ez a vendég”). A változók értékét **nem ρ** adja, hanem **v**.
- **Módosított értékelés v\[x:u\]:** ugyanaz, mint v, de az x értéke u (a többi változóé változatlan).
- |a| = ρ(a) névjelre; |x| = v(x) változóra; |f(t₁,…,tₙ)| = ρ(f)(|t₁|, …, |tₙ|).
- |P(t₁,…,tₙ)| = 1, ha a (|t₁|, …, |tₙ|) n-es **benne van** a ρ(P) listában, különben 0.
- |t₁ = t₂| = 1, ha |t₁| és |t₂| ugyanaz az elem.
- ¬, ∧, ∨, ⊃, ≡: ugyanúgy, mint nulladrendben.
- **|∀xA| = 0**, ha **van** olyan u ∈ U, hogy |A| a v\[x:u\] értékeléssel 0. Különben 1. (Vagyis ∀ = minden elemre igaz kell legyen; egy ellenpélda elrontja.)
- **|∃xA| = 1**, ha **van** olyan u ∈ U, hogy |A| a v\[x:u\] értékeléssel 1. Különben 0. (∃ = elég egy jó elem.)

Konyhai hasonlat: ∀ = **teljes étterem-ellenőrzés**: minden asztalt végignézünk, egyetlen rossz asztal bármire „hamis”. ∃ = **keresünk egy asztalt**, ahol jó: amint találtunk, „igaz”.

**Recept kvantoros formulához (kicsi univerzumban):**

1. Írd ki az univerzum elemeit.
2. ∀x A(x): írd ki A(u₁) ∧ A(u₂) ∧ … (mindegyiket ellenőrizd). ∃x A(x): A(u₁) ∨ A(u₂) ∨ …
3. Több kvantor: kívülről befelé bontsd.
4. Ahol a kvantor „kész”, állítsd elő: |A(x)| x:u → |A(u)|.

**Kidolgozott példa a jegyzetedből:** U = {u₁, u₂}, A(u₁, u₂) = A(u₂, u₁) = 1, A(u₁, u₁) = A(u₂, u₂) = 0 (azaz ρ(A) = {(u₁, u₂), (u₂, u₁)}).

- **∀y∃x A(x,y) = 1.** y = u₁-re x = u₂ jó (A(u₂, u₁) = 1). y = u₂-re x = u₁ jó (A(u₁, u₂) = 1). Minden y-hoz találtunk x-et.
- **∃x∀y A(x,y) = 0.** x = u₁-re az ∀y A(u₁, y) hamis, mert A(u₁, u₁) = 0. x = u₂-re szintún, mert A(u₂, u₂) = 0. Nincs olyan x, ami mindenkivel „jó”.

**Csapda: a kvantorok sorrendje számít!** ∀y∃x ≠ ∃x∀y. „Mindenkit szeret valaki” (∀y∃x) más, mint „van, aki mindenkit szeret” (∃x∀y). Az ∃x∀y az erősebb állítás.

**Másik példa:** U = {u₁, u₂}, ρ(A) = {u₁}, ρ(B) = {u₂}. ∀x(A(x) ∨ B(x)): u₁-re A igaz, u₂-re B igaz → mindenre van egy igaz tag → **1**.

**Példa feladat (5.P.5):** U = {1,2,3,4}, ρ(R) = {(u,v) : u osztja v-t}.

- ¬∃x¬R(x,x): minden szám osztója önmagának → nincs rossz x → **igaz**.
- ∀y∃xR(x,y): x = 1 minden y-t oszt → **igaz**.
- ∃x∀yR(x,y): x = 1 mindent oszt → **igaz**.
- ∃x∃y(R(x,y) ∧ ¬R(y,x)): x = 1, y = 2: 1 | 2, de 2 ∤ 1 → **igaz**.
- ∀x∀y(R(x,y) ∨ ¬R(y,x)): x = 2, y = 1: R(2,1) = 0, ¬R(1,2) = ¬1 = 0 → a vagy 0 → **hamis**.

### 4.5 Olvasd fel természetes nyelven (2.P.7, 5.P.1)

Ha egy formulát „ki kell mondani”: kívülről befelé olvass, a kvantorokat fordítsd (∀ = minden, ∃ = van olyan), a predikátumokat az interpretáció szerint.

**2.P.7:** f(x) = x + 1, P(x): x páros, Q(x,y): x ≤ y, U = természetes számok.

| Formula | Olvasd így |
| --- | --- |
| ∃xP(x) | van páros szám |
| ¬∀xP(x) | nem minden szám páros |
| ∃x¬P(x) | van nem páros szám (ez ugyanaz, mint az előző) |
| ¬¬∃xP(x) | nem igaz, hogy nincs páros szám, azaz van páros |
| ∀xQ(x,x) | minden szám ≤ önmagánál (igaz) |
| ∀x(P(x) ∨ P(f(x))) | minden számra vagy ő maga, vagy a rákövetkezője páros (igaz) |
| ∀x(P(x) ⊃ P(f(f(x)))) | ha páros, akkor x + 2 is páros (igaz) |
| ¬∃x¬Q(x,f(x)) | nincs olyan szám, ami nagyobb a rákövetkezőjénél, azaz minden x ≤ x + 1 (igaz) |
| ¬∃xQ(f(x),x) | nincs olyan szám, amelynek rákövetkezője ≤ őnála (igaz) |

**5.P.1:** ∀x(P(x,c) ⊃ ∃yQ(f(y),x)):

- (a) U = ℝ, c = 0, f = négyzetre emelés, P = „nagyobb”, Q = egyenlőség: „minden x-re, ha x > 0, akkor van olyan y, hogy y² = x”. **Igaz** (minden pozitív valós számnak van négyzetgyöke).
- (b) U = a rendezvény résztvevői, c = Cilike, f(x) = az, aki x-et meghívta, P = barátok, Q = ugyanaz a személy: „Cilike minden barátja meghívott valakit a rendezvényre” (mert f(y) = x azt jelenti, hogy x az, aki y-t meghívta).

### 4.6 Fordítás kvantorokkal: magyar → elsőrendű formula

**Általános szabályok:**

- „Minden A B” → **∀x(A(x) ⊃ B(x))**
- „Van olyan A, ami B” → **∃x(A(x) ∧ B(x))**
- „Nincs olyan A, ami B” → **¬∃x(A(x) ∧ B(x))** ⇔ ∀x(A(x) ⊃ ¬B(x))
- „Nem minden A B” → ¬∀x(A(x) ⊃ B(x)) ⇔ ∃x(A(x) ∧ ¬B(x))

**Csapdák (nagyon gyakoriak!):**

- ∀x(E(x) ∧ S(x)) azt jelenti: **mindenki** egyetemista és sportol. Ez nem az, hogy „minden egyetemista sportol”.
- ∃x(E(x) ⊃ S(x)) szinte mindig igaz (bármelyik nem-egyetemista már igazzá teszi), ez nem az, hogy „van sportoló egyetemista”.

**Kidolgozott (2.P.8):** v = Veronika, a = Ákos, E(x): x egyetemista, S(x): x sportol.

| Mondat | Formula |
| --- | --- |
| Veronika egyetemi hallgató. | E(v) |
| Ha Veronika egyetemi hallgató, akkor Ákos is az. | E(v) ⊃ E(a) |
| Vannak egyetemi hallgatók. | ∃xE(x) |
| Nem mindenki egyetemi hallgató. | ¬∀xE(x) |
| Bizonyos egyetemi hallgatók sportolnak. | ∃x(E(x) ∧ S(x)) |
| Nincs olyan egyetemi hallgató, aki nem sportol. | ¬∃x(E(x) ∧ ¬S(x)) ⇔ ∀x(E(x) ⊃ S(x)) |

**Kidolgozott (2.P.6)** szeret(x,y): x szereti y-t. Mindig kérdezd: **ki a „határozó” (az első argumentum) és ki a „cel” (a második)?**

| Mondat | Formula |
| --- | --- |
| Mindenki szeret valakit. | ∀x∃y szeret(x,y) |
| Mindenkit szeret valaki. | ∀y∃x szeret(x,y) |
| Mindenki szeret mindenkit. | ∀x∀y szeret(x,y) |
| Mindenki szereti önmagát. | ∀x szeret(x,x) |
| Van, aki mindenkit szeret. | ∃x∀y szeret(x,y) |
| Van, akit mindenki szeret. | ∃y∀x szeret(x,y) |
| Van, aki szereti önmagát. | ∃x szeret(x,x) |
| Senki sem szeret mindenkit. | ¬∃x∀y szeret(x,y) |
| Senkit sem szeret mindenki. | ¬∃y∀x szeret(x,y) |
| Van, aki senkit sem szeret. | ∃x∀y ¬szeret(x,y) |
| Van, akit senki sem szeret. | ∃y∀x ¬szeret(x,y) |

**Recept:** 1) határozd meg a fő kvantort a mondat elejéről („Mindenki…” → ∀x). 2) a „végző” rész („szeret valakit”) → ∃y szeret(x,y). 3) a „senki” = ¬∃, a „nem mindenki” = ¬∀. 4) Írd vissza magyarra, hogy ellenőrizd.
