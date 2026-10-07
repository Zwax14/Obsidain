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

**Csavaros kérdés (5.I.9, 5.I.10): mit tudunk részinformációból?**

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

## 5. Szemantikai alapfogalmak: modell, kielégíthetőség, érvényesség

### 5.1 Modell

- **Formulahalmaz modellje:** olyan interpretáció, amelyben a halmaz **minden** eleme igaz (egyszerre, ugyanabban az interpretációban!). Jelölés: |A|ρ = 1 minden A ∈ Γ-ra.
- **Formula modellje:** az egyelemű {A} halmaz modellje, vagyis olyan interpretáció, ahol A igaz.
- **Elsőrendben** a modell egy **hármas**: ⟨U, ρ, v⟩ (univerzum + szótár + értékelés). A változók miatt kell a v is.

Konyhai hasonlat: a modell egy **„jelentésválasztás”, ami mellett a recept „az” kijön**: ha a ρ jól választott, az összes állítás igaz.

### 5.2 A négy „félé” formula (angolul)

| Fogalom | Mit jelent | Angolul | Más néven |
| --- | --- | --- | --- |
| **Kielégíthető** | van modellje, **legalább egy** interpretációban igaz | satisfiable |  |
| **Kielégíthetetlen** | nincs modellje, **sehol** nem igaz | unsatisfiable | ellentmondás, contradiction |
| **Érvényes** | **minden** interpretációban igaz, jelölés: ⊨ A | valid | logikai törvény, tautológia |
| **Cáfolható** | van olyan interpretáció, ahol hamis | falsifiable |  |

**Besorolás az igazságtábla eredményoszlopa alapján:**

| Eredményoszlop | Érvényes? | Kielégíthető? | Cáfolható? | Kielégíthetetlen? |
| --- | --- | --- | --- | --- |
| csupa 1 | igen | igen | nem | nem |
| vegyes (1 is, 0 is) | nem | igen | igen | nem |
| csupa 0 | nem | nem | igen | igen |

**A ábra (ezt képzeld el):** egy nagy téglalap = minden formula. A két fél: bal felében a kielégíthetők (itt belül egy kisebb „karika” az érvényesek), jobb felében a kielégíthetetlenek. A cáfolható az, ami a kis karikán **kívül** van, tehát a vegyes és a kielégíthetetlen együtt.

**Memóriatipp:** Érvényes → kielégíthető (ez irányban igaz, fordítva nem!). Kielégíthetetlen → cáfolható (fordítva nem).

**Fontos kapcsolat a formula és a tagadott által:**

- A érvényes ⇔ ¬A kielégíthetetlen
- A kielégíthetetlen ⇔ ¬A érvényes
- A kielégíthető, de ¬A is lehet kielégíthető (p és ¬p is az)

**Formulahalmazra:** Γ kielégíthető, ha van modellje (van olyan interpretáció, ahol minden elem egyszerre igaz). **Csapda:** a {p, ¬p} halmaz minden eleme kielégíthető külön-külön, de a halmaz **nem**, mert nincs olyan ρ, ahol p is és ¬p is igaz. Az **üres halmaz kielégíthető** (minden interpretáció modellje, mert nincs elem, ami hamis lehetne).

### 5.3 Két tétel a halmazokról

**Tétel 1: kielégíthető halmaz minden részhalmaza kielégíthető** (a logikai ellentmondásmentesség szűkítéssel nem rontható el). Értelem: ha a teljes recept elkészíthető, akkor a fele is.

**Bizonyítás** (4 sor, tudd előadni): Legyen Γ kielégíthető és Δ ⊆ Γ. Γ-nak van modellje, legyen ρ. Ha A ∈ Δ, akkor A ∈ Γ, így |A|ρ = 1. Tehát ρ Δ-nak is modellje, Δ kielégíthető. ■

**Tétel 2: kielégíthetetlen halmaz minden bővítése kielégíthetetlen** (az ellentmondás bővítéssel nem szüntethető meg). Értelem: ha a recept eleve nem elkészíthető, több hozzávalóval sem lesz az.

**Bizonyítás (indirekt):** Legyen Γ kielégíthetetlen, Γ ⊆ Δ, és tegyük fel, hogy Δ kielégíthető. Akkor az 1. tétel miatt a Γ (ami Δ részhalmaza) kielégíthető volna. Ez ellentmondás. ■

**Bizonyítási sablon (indirekt), ezt fogod használni sokszor:**

1. Tegyük fel az ellenkezőjét annak, amit bizonyítani kell.
2. Ebből kapsz egy modellt (egy interpretációt, amiben bizonyos formulák igazak).
3. Ebben a modellben írd fel, mi igaz, mi hamis (|A|ρ = 1, |¬B|ρ = 1, ezért |B|ρ = 0…).
4. Mutasd meg, hogy ez ellentmond a feltevésnek. → az eredeti állítás igaz.

### 5.4 Gyakorlatok: kielégíthetőség (102–108, 7.I.13)

| Kérdés | Válasz | Indoklás / ellenpélda |
| --- | --- | --- |
| 102. Kielégíthetetlen halmazban mindig van hamis elem? | **Igen** | bármely ρ-ban van legalább egy hamis elem (különben ρ modell volna). **De ρ-nként más lehet** a hamis elem: {p, ¬p}-ben p=1-nél ¬p a hamis, p=0-nál p |
| 103. Minden kielégíthetetlen halmaznak van kielégíthető része? | **Igen** | az üres halmaz |
| 104. ... **nem üres** kielégíthető része? | **Nem** | {p ∧ ¬p}: egyetlen nem üres részhalmaza saját maga, az pedig kielégíthetetlen |
| 105. Minden kielégíthető halmaznak van kielégíthetetlen bővítése? | **Igen** | add hozzá a p ∧ ¬p-t |
| 106. Kielégíthetetlen halmaz minden része kielégíthetetlen? | **Nem** | {p, ¬p}-nek része a {p} (kielégíthető) és az ∅ |
| 107. Az üres halmaz? | **Kielégíthető** | minden interpretáció modellje |
| 108. Példa | kielégíthető: {p, q}, {p ⊃ q}; kielégíthetetlen: {p, ¬p}, {p ∧ ¬p} |  |

**7.I.1 (Mely állítás igaz?)**

| Állítás | Igaz? | Miért |
| --- | --- | --- |
| (a) kielégíthető ⇔ logikai törvény | **hamis** | p kielégíthető, de nem törvény |
| (b) csak akkor kielégíthető, ha logikai törvény | **hamis** | ugyanaz: p |
| (c) csak akkor logikai törvény, ha kielégíthető | **igaz** | törvény → kielégíthető |
| (d) pontosan akkor nem kielégíthető, ha ellentmondás | **igaz** | ez a definíció |
| (e) ha logikai törvény, akkor kielégíthető | **igaz** | (c) másik megfogalmazása |
| (f) ha nem ellentmondás, akkor nem logikai törvény | **hamis** | p ∨ ¬p nem ellentmondás, és törvény |
| (g) ha kielégíthető, akkor a negáltja is | **hamis** | p ∨ ¬p kielégíthető, a negált kielégíthetetlen |
| (h) ha logikai törvény, a negáltja nem elégíthető ki | **igaz** | a negált mindenhol hamis |
| (i) ha a negáltja kielégíthető, akkor logikai törvény | **hamis** | p negáltja ¬p kielégíthető, p nem törvény |

Jegyzetedben is ott van: „A akkor kielégíthető, ha ⊨A” szöveg három állítása közül csak a harmadik igaz: **⊨A ⇒ A kielégíthető**.

**7.I.3 (Igazoljuk, hogy kielégíthető!)** Recept: keress **egy** interpretációt, ami mellett igaz.

- ¬(X ⊃ ¬X): X=1: X ⊃ ¬X = 1 ⊃ 0 = 0, a negált 1. ✓
- (X ⊃ Y) ⊃ (Y ⊃ X): X=1, Y=0: (1 ⊃ 0) = 0, 0 ⊃ (…) = 1. ✓
- (X ⊃ Y ∧ Z) ∧ ¬(X ∨ Z ⊃ Y): a második tag igazhoz kell X ∨ Z = 1 és Y = 0. Ekkor az első tag: X ⊃ (0 ∧ Z) = X ⊃ 0 → X = 0 → Z = 1. Modell: **X=0, Y=0, Z=1**. Ellenőrzés: 0 ⊃ 0 = 1; (0 ∨ 1) ⊃ 0 = 0, negálva 1; 1 ∧ 1 = 1. ✓

**7.I.7 (Mely formulák NEM logikai törvények?)** Recept: **keress egy ellenpéldát** (olyan interpretációt, ami mellett hamis). Egy hamis ⊃ csak 1 ⊃ 0 alakú lehet.

- (a) ¬(X ⊃ Y) ⊃ ¬Y: ha az előtag 1, akkor Y = 0, és ¬Y = 1 → mindig 1 → **törvény**.
- (b) ¬(X ⊃ Y) ⊃ X: az előtag csak X=1, Y=0 mellett 1, ott X = 1 → **törvény**.
- (c) X ⊃ ¬(X ⊃ Y): X=1, Y=1: ¬(1 ⊃ 1) = 0, 1 ⊃ 0 = 0 → **nem törvény**.
- (d) ¬X ⊃ (X ⊃ Y): ha ¬X=1, X=0 → X ⊃ Y = 1 → **törvény**.
- (e) (X ⊃ Z) ⊃ ((X ⊃ Y) ⊃ (Y ⊃ Z)): X=0, Y=1, Z=0: X⊃Z = 1, X⊃Y = 1, Y⊃Z = 0 → 1 ⊃ (1 ⊃ 0) = 0 → **nem törvény**.
- (f) ¬X ∧ (Y ⊃ Z) ∧ X ⊃ ¬Y ∨ X: az előtagban ¬X ∧ X = 0 → az egész 1 → **törvény**.

Tehát nem törvény: **(c) és (e)**.

**7.I.11 (Kielégíthető-e a halmaz?)**

- {¬X, X ⊃ Y, X ∨ ¬Y}: ¬X → X=0. Ekkor X ⊃ Y = 1 bármi, X ∨ ¬Y = ¬Y → Y=0. Modell X=0, Y=0 → **kielégíthető**.
- {X ⊃ Y, X, ¬Y}: X=1 → Y=1, de ¬Y → Y=0. Ellentmondás → **kielégíthetetlen**.

## 6. Logikai következmény (⊨)

### 6.1 Definíció és a három „nyelv”, amin ugyanazt mondhatod

**Γ ⊨ A** (a Γ formulahalmaznak logikai következménye az A formula, angolul entailment/logical consequence) pontosan akkor, ha

1. a Γ ∪ {¬A} halmaz **kielégíthetetlen** (kiskáté definíció), vagy ami ugyanaz:
2. a Γ **minden modellje** modellje az A-nak is, vagy még úgy:
3. **minden olyan interpretációban, ahol Γ minden eleme igaz, A is igaz.**

Konyhai hasonlat: a premisszák az **összes hozzávaló**, a konklúzió az **elkészült étel**. A Γ ⊨ A azt mondja: „ha minden hozzávaló jó, az étel kötelezően jó”. Hibát csak akkor találnál, ha lenne olyan eset, hogy a hozzávalók jók, az étel mégis rossz.

- **A ⊨ B** (egyetlen premissza): {A} ⊨ B.
- **⊨ A** (üres bal oldal): A érvényes. Üres halmaz minden interpretációban „igaz”, így A-nak mindenhol igaznak kell lennie.
- **Γ ⊭ A:** van olyan interpretáció, ahol Γ minden eleme igaz, de A hamis (ez az **ellenpélda**).

**Hogyan látod be, hogy Γ ⊨ A? (recept igazságtáblával)**

1. Írd fel a táblát minden premisszára és a konklúzióra.
2. Keresd meg azokat a sorokat, ahol **minden premissza 1**.
3. Ha ezekben a sorokban a konklúzió is 1 → **következik**. Ha van olyan sor, ahol a premisszák 1, a konklúzió 0 → **nem következik** (ez az ellenpélda).

**Gyorsabb módszer (nincs tábla): „elve” keresed a modellt.** A premisszákat kényszerítsék kényszerítő értékekre (pl. ¬Z → Z=0, Y → Y=1), a többit számold ki, majd nézd meg a konklúziót.

**Kidolgozott (7.I.17):** premisszák: X ⊃ (Y ⊃ Z), Y, ¬Z.

- Y = 1, Z = 0 → Y ⊃ Z = 0 → X ⊃ 0 → X = 0. Egyetlen modell: **X=0, Y=1, Z=0**.
- (a) ¬X = 1 → **következik**. (b) Y ⊃ Z = 0 → nem. (c) X = 0 → nem. (d) X ∧ Y ∧ ¬Z = 0 → nem.

**Kidolgozott (7.I.18):** ¬X ⊃ ¬Y ∨ Z, Y, ¬Z.

- Y=1, Z=0 → ¬Y ∨ Z = 0 → ¬X ⊃ 0 → ¬X = 0 → X=1. Modell: **X=1, Y=1, Z=0**.
- (a) X ∧ Z = 0 → nem. (b) Z ⊃ ¬X = 0 ⊃ 0 = 1 → **igen**. (c) X ∧ Y = 1 → **igen**. (d) X ∧ Y ∧ Z = 0 → nem.

**Kidolgozott (7.I.21):**

- (a) X ⊃ ¬Y ⊨ X ⊃ (Y ⊃ Z)? A premissza modelljei: X=0 (bármi Y, Z), vagy X=1 és Y=0. X=0 esetén a konklúzió 1. X=1, Y=0 esetén Y ⊃ Z = 1, így X ⊃ 1 = 1. **Igen**, következik.
- (b) ¬X ∨ Y, Z ⊃ ¬Y ⊨ X ⊃ ¬Z? Ha X=0, a konklúzió 1. Ha X=1: ¬X ∨ Y miatt Y=1, akkor Z ⊃ ¬Y = Z ⊃ 0 miatt Z=0, ¬Z = 1, a konklúzió 1 ⊃ 1 = 1. **Igen**, következik.

### 6.2 Tulajdonságok (tételek, bizonyítással)

| Tétel | Mit mond | Bizonyítás lényege |
| --- | --- | --- |
| **Következmény és modell** | Γ ⊨ A ⇔ Γ minden modellje A modellje is | indirekt: ha lenne modell, ami A-nak nem modellje, akkor abban Γ ∪ {¬A} igaz → kielégíthető → ellentmond a definíciónak (fordítva is hasonlóan) |
| **Érvényes formula minden halmaznak következménye** | ha ⊨ A, akkor minden Γ-ra Γ ⊨ A | ⊨A azt jelenti, hogy {¬A} kielégíthetetlen. Γ ∪ {¬A} ennek bővítése, a 2. tétel szerint kielégíthetetlen |
| **Kielégíthetetlen halmazból minden következik** | ha Γ kielégíthetetlen, akkor minden A-ra Γ ⊨ A | Γ ∪ {¬A} a Γ bővítése, így kielégíthetetlen |
| **Dedukció tétel** | Γ ∪ {A} ⊨ B ⇒ Γ ⊨ A ⊃ B | indirekt: ha Γ ⊭ A ⊃ B, van modell, ahol Γ igaz és ¬(A ⊃ B) igaz, azaz A=1, B=0 → ott Γ ∪ {A} igaz és B hamis → ellentmond |
| **Dedukció megfordítása** | Γ ⊨ A ⊃ B ⇒ Γ ∪ {A} ⊨ B | indirekt: ha Γ ∪ {A} ⊭ B, van modell, ahol Γ igaz, A=1, B=0 → ott A ⊃ B hamis, ellentmond Γ ⊨ A ⊃ B-nek |
| **Következmény és implikáció** | A ⊨ B ⇔ ⊨ A ⊃ B | a dedukció tétel és megfordítása Γ = ∅-re |
| **Metszet tétel** | Γ ∪ {A} ⊨ B és Δ ⊨ A ⇒ Γ ∪ Δ ⊨ B | indirekt: ha Γ ∪ Δ ∪ {¬B} kielégíthető, abban Δ igaz → A igaz is → Γ ∪ {A} ∪ {¬B} igaz → ellentmond |
| **Logikai és materiális ekvivalencia** | A ⇔ B ⇔ ⊨ A ≡ B | lásd 7. fejezet |

Megjegyzés: ez mind **ugyanígy működik elsőrendben** is, csak ott modellként a ⟨U, ρ, v⟩ hármast kell írni a ρ helyett.

### 6.3 Helyes és helytelen következtetések

**Nevezetes helyes sémák** (mindet igazságtáblával be lehet látni):

| Név | Séma |
| --- | --- |
| leválasztás (modus ponens) | A, A ⊃ B ⊨ B |
| modus tollens | A ⊃ B, ¬B ⊨ ¬A |
| láncszabály (hipotetikus szillogizmus) | A ⊃ B, B ⊃ C ⊨ A ⊃ C |
| diszjunktív szillogizmus | A ∨ B, ¬A ⊨ B |
| esetszétválasztás | A ⊃ C, B ⊃ C, A ∨ B ⊨ C |
| indirekt bizonyítás | Γ, A ⊨ B és Γ, A ⊨ ¬B ⇒ Γ ⊨ ¬A |

**Gyakori helytelen „következtetések”:**

- A ⊃ B, B ⊭ A (a **következmény megerősítése**: „ha esik, vizes az út; vizes az út, tehát esik”. Nem: lehet, hogy locsolták.)
- A ⊃ B, ¬A ⊭ ¬B (az **előtag tagadása**)

**Recept szöveges következtetés ellenőrzéséhez (7.I.23):**

1. Adj betűt minden alapállításnak.
2. Írd le a premissákat és a konklúziót formúlaként.
3. Keress olyan interpretációt, ahol **minden premissza igaz, a konklúzió hamis**. Ha van → helytelen. Ha a premissák egyértelműen meghatározzák az értékeket és a konklúzió ebből jön → helyes.

**(a) Uszoda:** U: reggel uszodába megyek, K: korán kelek, D: sok dolgozatot kell kijavítanom éjszaka. Premissák: U ⊃ K, D ⊃ ¬K, U. Konklúzió: ¬D. U=1 → K=1 → ¬K=0 → D ⊃ 0 → D=0 → ¬D=1. **Helyes.**

**(c) Jelmez:** M: talál megfelelő jelmezt a kölcsönzőben, V: varr magának egyet, F: felveszi a tavalyit. Premissák: ¬M ⊃ (V ∨ F), ¬M, ¬F. Konklúzió: V. ¬M=1 → V ∨ F = 1, F=0 → V=1. **Helyes.**

**Parkoló:** P: ebben a parkolóban parkolok, L: az én autóm a legszebb, F: a feleségem máshol parkol. Premissák: P ⊃ L, L ⊃ F, F. Konklúzió: P. Ellenpélda: P=0, L=0, F=1 (a premissák igazak: 0⊃0=1, 0⊃1=1, F=1, de P=0). **Helytelen** (a következmény megerősítése).

**(b) Prímszám:** A: a 2 prímszám, B: a 2 a legkisebb prímszám, C: az 1 prímszám. Premissák: A ⊃ B, B ⊃ ¬C, ¬C. Konklúzió: A. Ellenpélda: A=0, B=0, C=0: 0⊃0=1, 0⊃1=1, ¬C=1, de A=0. **Helytelen.**

## 7. Logikai ekvivalencia (⇔) és az ekvivalencia jel (≡)

**Definíció:** A és B **logikailag ekvivalens** (A ⇔ B), ha A ⊨ B és B ⊨ A. Ez ugyanaz, mint: **minden interpretációban |A| = |B|**, vagyis az igazságtábláik eredményoszlopa **azonos**.

**Kapcsolat a ≡ jellel:** A ⇔ B akkor és csak akkor, ha **⊨ A ≡ B**. (A ≡ pontosan akkor 1, ha a két oldal egyezik; ha ez minden interpretációban így van, a ≡-es formula érvényes.)

**Hogyan lesz ebből ⊃-törvény (a jegyzeted nyilai):** A ⇔ B ⇒ A ⊨ B és B ⊨ A ⇒ ⊨ A ⊃ B és ⊨ B ⊃ A ⇒ ⊨ A ≡ B.

Konyhai hasonlat: a ≡ egy **„két személyes” kapcsoló a formulában**; az ⇔ egy **megjegyzés a fordítótáblanál**, hogy „a két recept ugyanazt az ételt állítja elő”.

### 7.1 Nevezetes ekvivalenciák (ezeket tanuld meg!)

| Név | Ekvivalencia | Konyhanyelven |
| --- | --- | --- |
| kettős tagadás | ¬¬A ⇔ A | kétszer megfordítod, visszakapod |
| kommutativitás | A ∧ B ⇔ B ∧ A; A ∨ B ⇔ B ∨ A; A ≡ B ⇔ B ≡ A | sorrend mindegy (⊃-nál **nem** mindegy!) |
| asszociativitás | A ∧ (B ∧ C) ⇔ (A ∧ B) ∧ C; ugyanígy ∨ és ≡ | csoportosítás mindegy |
| idempotencia | A ∧ A ⇔ A; A ∨ A ⇔ A | ugyanaz kétszer = egyszer |
| disztributívitás | A ∨ (B ∧ C) ⇔ (A ∨ B) ∧ (A ∨ C) és A ∧ (B ∨ C) ⇔ (A ∧ B) ∨ (A ∧ C) | szétszórás, mint a számoknál a szorzás |
| elnyelés | A ∧ (B ∨ A) ⇔ A; A ∨ (B ∧ A) ⇔ A | a „kisebb” elnyeli a „nagyobbat” |
| **De Morgan** | ¬(A ∧ B) ⇔ ¬A ∨ ¬B; ¬(A ∨ B) ⇔ ¬A ∧ ¬B | a tagadás „behat” a zárójelbe, és ∧ ↔ ∨ cserél |
| áthelyezési törvény | (A ∧ B) ⊃ C ⇔ A ⊃ (B ⊃ C) | „és” előtagot láncolható feltételekké bontasz |
| **kontrapozíció** | A ⊃ B ⇔ ¬B ⊃ ¬A | fordítsd meg és tagadd mindkettőt |
| öndisztributívitás | A ⊃ (B ⊃ C) ⇔ (A ⊃ B) ⊃ (A ⊃ C) |  |
| esetszétválasztás | (A ∨ B) ⊃ C ⇔ (A ⊃ C) ∧ (B ⊃ C) | ha „vagy” az előtag, mindkét esetre külön igaz |

**Kifejezhetőség (egymással kiváltható műveletek):**

| Eredeti | Helyette |
| --- | --- |
| A ⊃ B | ¬(A ∧ ¬B) ⇔ **¬A ∨ B** |
| A ∧ B | ¬(A ⊃ ¬B) ⇔ ¬(¬A ∨ ¬B) |
| A ∨ B | ¬A ⊃ B ⇔ ¬(¬A ∧ ¬B) |
| A ≡ B | (A ⊃ B) ∧ (B ⊃ A) |

Vagyis **¬ és ∧ (vagy ¬ és ⊃)** elegendő az összes többi művelet kifejezéséhez. **A nand (not and)**: A nand B = ¬(A ∧ B). Egymással minden kifejezhető: ¬A ⇔ A nand A, A ∧ B ⇔ (A nand B) nand (A nand B). (Ez van a jegyzeteden „nand = not and”.)

**Csapda:** **A ⊃ B ≠ B ⊃ A** (ez a fordított implikáció), **A ⊃ B ≠ ¬A ⊃ ¬B** (ez az inverz). Csak a kontrapozíció (¬B ⊃ ¬A) ekvivalens.

### 7.2 Hogyan igazolj ekvivalenciát vagy logikai törvényt? (recept)

**1. módszer: igazságtábla.** Írd fel mindkét oldal tábláját, és állítsd, hogy az eredményoszlopok azonosak (a jegyzetedben így csináltad: ¬(A∧B) és ¬A∨¬B oszlopa 1,1,1,0 mindkétszer; (A∨B)⊃C és (A⊃C)∧(B⊃C) oszlopa azonos).

**2. módszer: átalakítás lépésekben** (gyorsabb, de gondosan): minden lépésnél egy ismert ekvivalenciát használj, és írd mellé, melyiket.

**Aranyszabály az átalakításhoz:**

1. Cseréld ki a ⊃-t: A ⊃ B ⇔ ¬A ∨ B.
2. Tagadás befelé: De Morgan, ¬¬A ⇔ A.
3. Disztributívitással rendezd.
4. Ha egy ∨-ban ott van p és ¬p, az **érvényes** (p ∨ ¬p mindig 1).

**8.I.6 (Mutassuk meg, hogy logikai törvények)**

- (a) ¬(X ⊃ Y) ⊃ X ⇔ ¬¬(X ⊃ Y) ∨ X ⇔ (X ⊃ Y) ∨ X ⇔ (¬X ∨ Y) ∨ X ⇔ (¬X ∨ X) ∨ Y: mindig igaz (p ∨ ¬p). ■
- (b) (X ∧ Y) ⊃ (X ⊃ Y) ⇔ ¬(X ∧ Y) ∨ (¬X ∨ Y) ⇔ ¬X ∨ ¬Y ∨ ¬X ∨ Y: tartalmaz ¬Y ∨ Y-t, ezért mindig igaz. ■
- (c) ((X ⊃ Y) ⊃ X) ⊃ (X ∨ Y): először (X ⊃ Y) ⊃ X ⇔ ¬(¬X ∨ Y) ∨ X ⇔ (X ∧ ¬Y) ∨ X ⇔ X (elnyelés). Ekkor a formula X ⊃ (X ∨ Y) ⇔ ¬X ∨ X ∨ Y: mindig igaz. ■

**8.I.4 (Ekvivalens-e?)** ¬X ⊃ Y ∧ ¬Z és (¬Y ⊃ X) ∧ (Z ⊃ X)

- Bal oldal: ¬X ⊃ (Y ∧ ¬Z) ⇔ X ∨ (Y ∧ ¬Z) ⇔ (X ∨ Y) ∧ (X ∨ ¬Z) (disztributívitás).
- Jobb oldal: (¬Y ⊃ X) ⇔ (Y ∨ X), (Z ⊃ X) ⇔ (¬Z ∨ X). Együtt: (Y ∨ X) ∧ (¬Z ∨ X).
- A két oldal ugyanaz (kommutativitással). **Ekvivalensek.** ■

## 8. Szemantikus fa (a jegyzetedből): kielégíthető-e egy formula, gépiesen?

Igazságtábla helyett egy **fa**, ami „elbászik” a formulán, és menet közben megmondja, van-e modell. (A jegyzetedben: „α → és”, „β → vagy”.)

**Alapgondolat:** minden csomópont egy **formulahalmaz** („ezek mind legyenek igazak egyszerre”). Bontjuk az összetett formulákat, amíg csak literálok (p vagy ¬p) maradnak.

| Típus | Mikor? | Mit csinálsz? | Konyha |
| --- | --- | --- | --- |
| **α („és” típus)** | A ∧ B | **ugyanabban** a halmazban A és B egyszerre marad | ugyanabba az edénybe teszed mindkettőt |
| **β („vagy” típus)** | A ∨ B | **elágazol**: az egyik ágon A, a másikon B | két külön edény: bármelyik jó lehet |

**Kiegészítés (ugyanezen elven, az ekvivalenciákból levezetve):** ¬(A ∨ B) → α: ¬A, ¬B. ¬(A ⊃ B) → α: A, ¬B. ¬¬A → A. A ⊃ B → β: ¬A | B. ¬(A ∧ B) → β: ¬A | ¬B. A ≡ B → β: (A, B) | (¬A, ¬B). ¬(A ≡ B) → β: (A, ¬B) | (¬A, B). (Ezeket a ZH-n csak akkor írd, ha az előadó „tiltotta” volna; a jegyzeted az „és” és „vagy” típust tárgyalja.)

**Zárt ág:** olyan levél, amiben van egy atom **és a tagadottja** is (p és ¬p): ott ellentmondás van, az ág „elhal”. **Nem zárt (nyitott) ág:** nincs benne ilyen pár.

**Eredmény:**

- Ha **van** legalább egy **nem zárt** levél → a formula **kielégíthető**, és a levélből kiolvasható a modell (ami literál ott áll, az 1).
- Ha **minden** levél **zárt** → a formula **kielégíthetetlen**.
- Érvényesség: ⊨ A akkor, ha a ¬A fa minden levél zárt. Következmény: Γ ⊨ A akkor, ha a Γ ∪ {¬A} fa minden levél zárt (ez a definíció gépies végrehajtása).

**A jegyzeted példája:** φ = (p ∨ q) ∧ (¬p ∨ ¬q)

- **1.** {φ}
- **2.** α (∧-bontás): {p ∨ q, ¬p ∨ ¬q}
- β (a ¬p ∨ ¬q bontása): **3.** {p ∨ q, ¬p} és **4.** {p ∨ q, ¬q}
- β (a p ∨ q bontása mindkét ágon):
  - a 3.-ból: **5.** {p, ¬p} → **zárt**; **6.** {q, ¬p} → nem zárt
  - a 4.-ből: **7.** {p, ¬q} → nem zárt; **8.** {q, ¬q} → **zárt**
- Eredmény: kielégíthető. Modellek: a 6. levélből **q=1, p=0**; a 7. levélből **p=1, q=0**. (Ez a „kizáró vagy”, pontosan egy igaz.)

**Recept:** 1) írd fel a gyökérbe a formulát (vagy Γ ∪ {¬A}-t). 2) amit lehet, α-val bontsd (nincs elágazás). 3) utána β-val bontsd, ez ágaztat. 4) minden levélben nézd, van-e p és ¬p. 5) olvasd le az eredményt.

## 9. Feladatgyűjtemény kidolgozva (kiskáté 113–114)

### 9.1 Igazak-e? (113) – kielégíthetőség, következmény

Minden „hamis” válaszhoz kell egy ellenpélda, minden „igaz”hoz rövid indoklás.

|  | Állítás | Válasz | Indoklás / ellenpélda |
| --- | --- | --- | --- |
| a | A kielégíthető ⇒ ¬A kielégíthető | **hamis** | A = p ∨ ¬p: érvényes, a negáltja kielégíthetetlen |
| b | ¬A érvényes ⇒ A nem kielégíthető | **igaz** | ¬A mindenhol 1 → A mindenhol 0 |
| c | ¬A kielégíthető ⇒ A érvényes | **hamis** | A = p: ¬p kielégíthető, p nem érvényes |
| d | A érvényes ⇒ A kielégíthető | **igaz** | mindenhol 1, tehát van modell |
| e | A kielégíthető ⇒ A érvényes | **hamis** | A = p |
| f | minden A-hoz van A-tól különböző B, A ⊨ B | **igaz** | B = A ∧ A (vagy ¬¬A) |
| g | minden A-hoz van A-val **nem ekvivalens** B, A ⊨ B | **hamis** | ha A érvényes (pl. p ∨ ¬p), B-nek is mindenhol igaznak kell lennie, tehát ekvivalens vele |
| h | minden A-hoz van A-tól különböző B, B ⊨ A | **igaz** | B = A ∧ A |
| i | minden A-hoz van A-val nem ekvivalens B, B ⊨ A | **hamis** | ha A kielégíthetetlen (p ∧ ¬p), B-nek is az, ez ekvivalens |
| j | minden A, B: A ⊨ B vagy B ⊨ A | **hamis** | A = p, B = q |
| k | van olyan formula, ami minden formulának következménye | **igaz** | bármely érvényes formula, pl. p ∨ ¬p |
| l | A ⊨ B ⇒ B ⊨ A | **hamis** | A = p ∧ q, B = p |
| m | A ⊨ B ⇒ B-nek nem következménye A | **hamis** | az „A ⊨ B” épp azt jelenti, hogy B következménye A-nak (ellentmondásos kijelentés) |
| n | létezik A, B: A ⊨ B, de B-nek nem következménye A | **hamis** | ugyanaz, mint (m). (Ha „A-nak nem következménye B” volt a szándék, akkor igaz: A = p ∧ q, B = p) |
| o | Γ ⊨ A vagy Γ ⊨ B ⇒ Γ ⊨ A ∧ B | **hamis** | Γ = {p}, A = p, B = q |
| p | Γ ⊨ A és Γ ⊨ B ⇒ Γ ⊨ A ∨ B | **igaz** | ha A igaz, A ∨ B is |
| q | Γ ⊨ A ∨ B ⇒ Γ ⊨ A vagy Γ ⊨ B | **hamis** | Γ = ∅, A = p, B = ¬p |
| r | Γ ⊨ A ⊃ B, Γ ⊨ A ⊃ C ⇒ Γ, A ⊨ B ∧ C | **igaz** | ahol Γ és A igaz, ott B is C is igaz |
| s | ugyanazok ⇒ Γ, C ⊨ A ∨ B | **hamis** | Γ = {¬p}, A = p, B = q, C = r: modell p=0, q=0, r=1: Γ és C igaz, de A ∨ B = 0 |
| t | Γ ⊨ A ⊃ C, Γ ⊨ B ⊃ C ⇒ Γ, A ∨ B ⊨ C | **igaz** | A ∨ B igaz → A vagy B igaz → mindkét esetben C |
| u | ugyanazok ⇒ Γ, A ∨ C ⊨ B | **hamis** | Γ = {r}, A = p, B = q, C = r: Γ ⊨ p ⊃ r és q ⊃ r, de {r, p ∨ r} nem adja q-t |

### 9.2 Bizonyítsuk be (114)

Mindegyik **indirekt** a szabványos sablonnal: a konklúzió tagadását vedd a premissákhoz, vegyük a modellt, ellentmondás.

|  | Állítás | Bizonyítás röviden |
| --- | --- | --- |
| a | Γ ⊨ A és Γ ⊨ A ⊃ B ⇒ Γ ⊨ B | tegyük fel, Γ ∪ {¬B}-nek van ρ modellje. Γ igaz → |
| b | Γ ⊨ A és Γ ⊨ B ⇒ Γ ⊨ A ∧ B | ρ modell Γ ∪ {¬(A ∧ B)}-re. |
| c | Γ, A ⊨ C és Γ, B ⊨ C ⇒ Γ, A ∨ B ⊨ C | ρ modell Γ ∪ {A ∨ B, ¬C}-re. |
| d | Γ ⊨ A ⇒ Γ ⊨ A ∨ B | ρ modell Γ ∪ {¬(A ∨ B)}-re. |
| e | Γ, A ⊨ B és Γ, A ⊨ ¬B ⇒ Γ ⊨ ¬A | ρ modell Γ ∪ {¬¬A}-ra → Γ igaz, |
| f | Γ ⊨ A és Γ ⊨ A ≡ B ⇒ Γ ⊨ B | ρ modell Γ ∪ {¬B}-re. |

## 10. Elsőrendű logika: kvantor-törvények és a nyelv „takarítása”

### 10.1 A szemantika két alaptétele

1. **Egyértelműség:** adott formulához, interpretációhoz és értékeléshez az |A| érték **egyértelműen** meg van határozva (nincs két különböző eredmény).
2. **Csak a szabad változók számítanak:** ha két értékelés (v₁, v₂) a **szabad** változókon megegyezik, akkor |A| ugyanaz mindkettővel. Következmény: **zárt formulánál az értékelés teljesen mindegy**, ezért van a zárt formulának „önmagában” igazságértéke.

### 10.2 A kvantifikáció De Morgan törvényei

| Törvény | Konyhanyelven |
| --- | --- |
| **¬∃xA ⇔ ∀x¬A** | „nincs olyan x, hogy A” = „minden x-re nem A” |
| **¬∀xA ⇔ ∃x¬A** | „nem minden x-re A” = „van olyan x, amire nem A” |
| ∃xA ⇔ ¬∀x¬A | az ∃ kifejezhető az ∀-lal |
| ∀xA ⇔ ¬∃x¬A | az ∀ kifejezhető az ∃-vel |

**Szabály:** a ¬ „átlép” a kvantoron, és a kvantor **átfordul** (∀ ↔ ∃), az állítás pedig tagadódik.

**Bizonyítás (¬∃xA ⇔ ∀x¬A, két irány, mind indirekt):**

- ¬∃xA ⊨ ∀x¬A: tegyük fel, {¬∃xA, ¬∀x¬A} kielégíthető, modell ⟨U, ρ, v⟩. |∃xA| = 0, |∀x¬A| = 0. A ∀ szabálya szerint van u ∈ U, hogy |¬A| v\[x:u\]-val 0, azaz |A| = 1. A ∃ szabálya szerint ekkor |∃xA| = 1. Ellentmondás.
- ∀x¬A ⊨ ¬∃xA: hasonlóan, a másik irányba (a ∃ szabályból találsz u-t, ami a ∀x¬A-t csúfolása).

### 10.3 Kvantorok mozgatása (hatáskör bővítése)

**Feltétel mindig: x ∉ FreeVar(A)**, vagyis az A-ban az x **nem szerepel szabadon** (nem függ x-től).

| Bal oldal | ⇔ | Jobb oldal |
| --- | --- | --- |
| A ∧ ∀xB | ⇔ | ∀x(A ∧ B) |
| A ∧ ∃xB | ⇔ | ∃x(A ∧ B) |
| A ∨ ∀xB | ⇔ | ∀x(A ∨ B) |
| A ∨ ∃xB | ⇔ | ∃x(A ∨ B) |
| A ⊃ ∀xB | ⇔ | ∀x(A ⊃ B) |
| A ⊃ ∃xB | ⇔ | ∃x(A ⊃ B) |
| **∀xB ⊃ A** | ⇔ | **∃x(B ⊃ A)** (a kvantor **átfordul**!) |
| **∃xB ⊃ A** | ⇔ | **∀x(B ⊃ A)** (átfordul!) |

**Miért fordul át?** Mert az ⊃ bal oldala „tagadás alatt” van: ∀xB ⊃ A ⇔ ¬∀xB ∨ A ⇔ ∃x¬B ∨ A ⇔ ∃x(¬B ∨ A) ⇔ ∃x(B ⊃ A). Nem kell bemagolnod, képes vagy levezetni.

**Fiktív kvantorok:** ha x ∉ FreeVar(A), akkor ∀xA ⇔ A és ∃xA ⇔ A. (A kvantor nem köt semmit, „üres kvantor”, elhagyható.)

**Csapda (nincs a kiskátéban, de gyakran feladat):** ∀x(A ∨ B) **nem** ekvivalens ∀xA ∨ ∀xB-vel. Példa: U = {1, 2}, A(x): x = 1, B(x): x = 2. ∀x(A ∨ B) igaz, de ∀xA ∨ ∀xB hamis. (Fordítva: ∀xA ∧ ∀xB ⇔ ∀x(A ∧ B) igaz.) Hasonlóan ∃x(A ∧ B) nem ekvivalens ∃xA ∧ ∃xB-vel.

### 10.4 Behelyettesítés, átnevezés, kongruencia

**Miért kell?** Ha egy kvantor „elkapna” egy változót, amit betettünk, a formula jelentése megváltozna. Ezt akarjuk megakadályozni.

- **\[A\]ᵗₓ** (A-ban x helyett t): az x **szabad** előfordulásait cseréljük t-re (a kötöttekhez nem nyúlunk).
- **x behelyettesíthető y változóval A-ban**, ha az x egyetlen szabad előfordulása sem esik az A egy ∀y B vagy ∃y B részformulájába (az y nem „kötödne meg”).
- **x behelyettesíthető a t termusszal**, ha behelyettesíthető a t-ben előforduló **minden** változóval.

**Példa:** A = ∃yQ(x,y).

- x → z (változó): **igen**, eredmény ∃yQ(z,y).
- x → y: **nem**, mert az x szabad előfordulása a ∃y hatáskörében van → az y „beleragadna”.
- x → f(y): **nem**, a termben y van.
- x → f(z): **igen**, ∃yQ(f(z),y).

**Átnevezés** (szabályosan): ∀xA₁-ben a kvantort és a hatáskörében lévő x-eket átírjuk x₁-re, ha x behelyettesíthető x₁-gyel és x₁ **nem szabad** A₁-ben. Példa: ∀xP(x,y) → ∀zP(z,y) (jó), de ∀xP(x,y) → ∀yP(y,y) (**rossz**, az y szabad volt).

- **Kongruens formulák** (Cong(A)): amelyek A-ból szabályos átnevezések sorozatával megkaphatók. Ez ekvivalencia-reláció (reflexív, szimmetrikus, tranzitív), és **a kongruens formulák logikailag ekvivalensek**. **Szintaktikai szinonima** = ugyanaz a fogalom, B ∈ Cong(A).
- **Változóiban tiszta** formula: (1) a szabad és a kötött változók halmaza **idegen** (FreeVar ∩ BoundVar = ∅), és (2) minden kötött változót **pontosan egy** kvantor köt. Tétel: minden formulához van vele kongruens, változóiban tiszta formula.

**Példa:** ∀xP(x) ⊃ ∃xQ(x) **nem** tiszta (x-et két kvantor köti). Tiszta alak: ∀xP(x) ⊃ ∃yQ(y).

### 10.5 Prenex alak

**Prenex alak:** minden kvantor **az elején**, utána egy kvantormentes rész: Q₁x₁Q₂x₂…Qₙxₙ B (Q ∈ {∀, ∃}, az x-ek különbözőek). Tétel: minden formulához van vele ekvivalens prenex formula.

**Recept prenex alakra hozáshoz:**

1. **Változóiban tiszta** alak (nevezd át a kötött változókat, hogy mind különböző legyen).
2. Küszöböld ki a ≡-t és a ⊃-t, ha kell (⊃ → ¬A ∨ B, vagy használd az átforduló törvényeket).
3. A ¬-t tolod befelé (kvantor De Morgan).
4. A kvantorokat húzd ki a mozgatási törvényekkel (10.3), balról jobbra.

**Példa:** ∀xP(x) ⊃ ∃xQ(x)

- tiszta alak: ∀xP(x) ⊃ ∃yQ(y)
- ∀xB ⊃ A ⇔ ∃x(B ⊃ A): ∃x(P(x) ⊃ ∃yQ(y))
- A ⊃ ∃yB ⇔ ∃y(A ⊃ B): ∃x∃y(P(x) ⊃ Q(y)) ← **prenex alak**.

**Példa:** ∀xP(x) ∧ ∃xQ(x) → tiszta: ∀xP(x) ∧ ∃yQ(y) → ∀x(P(x) ∧ ∃yQ(y)) → ∀x∃y(P(x) ∧ Q(y)).

## 11. Kalkulus: levezetés (⊢) – a „gépies bizonyítás”

**Miért kell?** A ⊨ azt mondja, hogy **igaz**, de a végtelen sok interpretációt nem lehet ellenőrizni. A kalkulus megmásol egy **véges, gépi** módszert: kiindulási formulákból **szabályokkal** építjük fel a következményeket. (A rendszer arra való és úgy van megalkotva, hogy ugyanazt hozza ki, mint a ⊨.)

Konyhai hasonlat: az axiómák a **kamra alapkészlete**, a leválasztás az **egyetlen megengedett mozdulat** (összerakni két „dárab”ot egy újabb dárabbal). Egy levezetés = a recept lépései.

### 11.1 Axiómaséma, axióma, leválasztás

**Axióma-sémák** (A, B, C **bármilyen formulák**):

- **(A1)** A ⊃ (B ⊃ A)
- **(A2)** (A ⊃ (B ⊃ C)) ⊃ ((A ⊃ B) ⊃ (A ⊃ C))
- **(A3)** (¬A ⊃ ¬B) ⊃ (B ⊃ A)

**Axióma** = egy axióma-séma **szabályos behelyettesítése** (a sémában szereplő betűk helyére tetszőleges formulákat írunk, ugyanazt a betűt mindenhol ugyanazzal). Példa A1-re: p ⊃ (q ⊃ p), vagy (p ∧ q) ⊃ ((¬r) ⊃ (p ∧ q)).

**Leválasztási szabály (modus ponens):** ha megvan B, és megvan B ⊃ A, akkor megvan A.

### 11.2 Szintaktikai következmény (Γ ⊢ A), induktív definíció

- **Bázis:** ha A ∈ Γ, akkor Γ ⊢ A. Ha A axióma, akkor Γ ⊢ A.
- **Szabály (leválasztás):** ha Γ ⊢ B és Γ ⊢ B ⊃ A, akkor Γ ⊢ A.
- **⊢ A (levezethető):** ∅ ⊢ A. A ⊢ B: {A} ⊢ B.
- **Szekvencia:** a „Γ ⊢ A” jelsorozat.
- **Cns(Γ):** Γ összes szintaktikai következménye.
- **Inkonzisztens** halmaz/formula: Cns(Γ) = Form (**mindent** le lehet vezetni belőle). **Konzisztens:** nem inkonzisztens.

**Tulajdonságok:**

| Tétel | Mit mond | Bizonyítás lényege |
| --- | --- | --- |
| **Reflexivitás** | Γ, A ⊢ A | A ∈ Γ ∪ {A}, a bázis szerint |
| **Monotonitás** | Γ ⊢ A ⇒ Γ ∪ Δ ⊢ A | strukturális indukció: axiómára igaz; Γ-beli elemre igaz; leválasztással kapott formulára az indukciós feltevés miatt a nagyobb halmazban is megvan B és B ⊃ A, így A is |
| **Dedukció tétel** | Γ, A ⊢ B ⇒ Γ ⊢ A ⊃ B | strukturális indukció (lent) |
| **Dedukció megfordítása** | Γ ⊢ A ⊃ B ⇒ Γ, A ⊢ B | monotonitás: Γ, A ⊢ A ⊃ B; reflexivitás: Γ, A ⊢ A; leválasztás |
| **Metszet tétel** | Γ ⊢ A és Δ, A ⊢ B ⇒ Γ ∪ Δ ⊢ B | monotonitás + dedukció tétel: Δ ⊢ A ⊃ B → Γ ∪ Δ ⊢ A ⊃ B, és Γ ∪ Δ ⊢ A → leválasztás |

**A dedukció tétel bizonyítása (strukturális indukció, három eset az „B” szerint):**

1. **B axióma:** Γ ⊢ B. A B ⊃ (A ⊃ B) is axióma (A1), leválasztással Γ ⊢ A ⊃ B.
2. **B ∈ Γ ∪ {A}:** ha B = A, akkor a segédtétel szerint ⊢ A ⊃ A (monotonitással Γ ⊢ A ⊃ A). Ha B ∈ Γ, mint az 1. esetben (A1 + leválasztás).
3. **B leválasztással jött:** Γ, A ⊢ C és Γ, A ⊢ C ⊃ B. Az indukciós feltevés: Γ ⊢ A ⊃ C és Γ ⊢ A ⊃ (C ⊃ B). A2 axiómából: Γ ⊢ (A ⊃ (C ⊃ B)) ⊃ ((A ⊃ C) ⊃ (A ⊃ B)). Két leválasztással: Γ ⊢ A ⊃ B. ■

**Segédtétel: ⊢ A ⊃ A (levezetés, 5 lépés)**

1. ⊢ (A ⊃ ((C ⊃ A) ⊃ A)) ⊃ ((A ⊃ (C ⊃ A)) ⊃ (A ⊃ A)) (A2, a B := C ⊃ A, C := A választással)
2. ⊢ A ⊃ ((C ⊃ A) ⊃ A) (A1, B := C ⊃ A)
3. ⊢ (A ⊃ (C ⊃ A)) ⊃ (A ⊃ A) (leválasztás 1., 2.)
4. ⊢ A ⊃ (C ⊃ A) (A1, B := C)
5. ⊢ A ⊃ A (leválasztás 3., 4.) ■

**Természetes levezetés (ND)** – a kiskátében a bizonyítható szekvenciák halmaza **PrND**, ezekkel a szabályokkal:

- bázis: Γ, A ⊢ A
- strukturális: Γ ⊢ A ⇒ Γ, B ⊢ A (bővítés); azonos premissza elhagyása; sorrendcsere; metszet
- ⊃ bevezetés: Γ, A ⊢ B ⇒ Γ ⊢ A ⊃ B
- ⊃ elimináció (leválasztás): Γ ⊢ A és Γ ⊢ A ⊃ B ⇒ Γ ⊢ B
- ¬ bevezetés: Γ, A ⊢ B és Γ, A ⊢ ¬B ⇒ Γ ⊢ ¬A
- ¬ elimináció: Γ ⊢ ¬¬A ⇒ Γ ⊢ A
- ∧: Γ ⊢ A, Γ ⊢ B ⇒ Γ ⊢ A ∧ B; Γ, A, B ⊢ C ⇒ Γ, A ∧ B ⊢ C
- ∨: Γ ⊢ A ⇒ Γ ⊢ A ∨ B; Γ ⊢ B ⇒ Γ ⊢ A ∨ B; Γ, A ⊢ C és Γ, B ⊢ C ⇒ Γ, A ∨ B ⊢ C
- ≡: Γ, A ⊢ B és Γ, B ⊢ A ⇒ Γ ⊢ A ≡ B; Γ ⊢ A és Γ ⊢ A ≡ B ⇒ Γ ⊢ B (és szimmetrikusan)

**Csapda:** ⊨ és ⊢ **két külön fogalom** (⊨: igazság, ⊢: levezetés). Jegyzet: „A ⊨ B: A-ból következik B. A ⊢ B: valamilyen algoritmusban A-ból következik B.”

## 12. Definíciók gyorstár (kiskáté 1–68, egy sorban, konyhanyelven)

A ZH-n sokszor szó szerint is kérik a definíciót. Tanuld meg a **bal oszlop fogalmát** és a **jobb oszlop „kulcsszó” vezetését**, és mondd el a saját szavaiddal, pontosan a kiépített szerkezettel.

### 12.1 Szintaxis

| Fogalom | Definíció lényege |
| --- | --- |
| nulladrendű nyelv | L⁽⁰⁾ = ⟨LC, Con, Form⟩; LC = {¬, ⊃, ∧, ∨, ≡, (, )}; Con ≠ ∅ az állításparaméterek legfeljebb megszámlálhatóan végtelen halmaza; LC ∩ Con = ∅; Form induktívan: Con ⊆ Form; A ∈ Form ⇒ ¬A ∈ Form; A, B ∈ Form ⇒ (A ⊃ B), (A ∧ B), (A ∨ B), (A ≡ B) ∈ Form |
| nulladrendű atomi (prím) formula | Con elemei |
| részformula | RF(A) a legszűkebb halmaz, amelyre A ∈ RF(A), és ha egy összetett formula benne van, a részei is |
| közvetlen részformula | ¬A-nak A; kétargumentumú formulának a két oldala; ∀xA, ∃xA-nak A; atomi formulának nincs |
| szerkezeti fa | véges rendezett fa, csúcsai formulák, gyökere A, gyerekei a közvetlen részformulák, levelei atomi formulák |
| zárójel-konvenciók | legkülső zárójel elhagyható; erősorrend (∧, ∨, ⊃, ≡); ¬ erősebb; kvantorok erősebbek; azonos jeleknél balról jobbra (a kiskáté szerint) |
| literál | p vagy ¬p (p ∈ Con); p a literál **alapja** |
| elemi konjunkció | literál, vagy **különböző alapú** literálok konjunkciója |
| elemi diszjunkció | literál, vagy különböző alapú literálok diszjunkciója |
| diszjunktív normálforma (DNF) | elemi konjunkció, vagy elemi konjunkciók diszjunkciója |
| konjunktív normálforma (KNF) | elemi diszjunkció, vagy elemi diszjunkciók konjunkciója |
| elsőrendű nyelv | L⁽¹⁾ = ⟨LC, Var, Con, Term, Form⟩, ahol LC-ben ∀, ∃, = is van; Var megszámlálhatóan végtelen; Con = ⋃(F⁽ⁿ⁾ ∪ P⁽ⁿ⁾); Term és Form induktív (lásd 3.2) |
| elsőrendű atomi formula | P⁽⁰⁾ eleme; (t₁ = t₂); P(t₁, …, tₙ) |
| szabad változók halmaza FreeVar(A) | atomi: a benne előforduló változók; ¬B: FreeVar(B); kétargumentumú: FreeVar(B) ∪ FreeVar(C); ∀xB, ∃xB: FreeVar(B) ∖ {x} |
| kötött változók halmaza BoundVar(A) | atomi: ∅; ¬B: BoundVar(B); kétargumentumú: unió; ∀xB, ∃xB: BoundVar(B) ∪ {x} |
| szabad előfordulás | az x olyan előfordulása, ami nem esik az A egyetlen ∀xB / ∃xB részformulájába sem |
| kötött előfordulás | nem szabad előfordulás |
| nyitott / zárt formula | nyitott: van szabad változó-előfordulás; zárt: nincs |
| behelyettesíthetőség, behelyettesítés | lásd 10.4 |
| átnevezés, kongruens, szintaktikai szinonima | lásd 10.4 |
| változóiban tiszta | FreeVar ∩ BoundVar = ∅ és minden kötött változót pontosan egy kvantor köt |
| prenex alak | kvantormentes, vagy Q₁x₁…QₙxₙB alakú, B kvantormentes, x-ek különbözőek |

### 12.2 Szemantika

| Fogalom | Definíció lényege |
| --- | --- |
| nulladrendű interpretáció | ρ függvény, Dom(ρ) = Con, ρ(p) ∈ {0, 1} |
| szemantikai érték |  |
| elsőrendű interpretáció | ⟨U, ρ⟩: U ≠ ∅; ρ(a) ∈ U; ρ(f) : Uⁿ → U; ρ(p) ∈ {0, 1}; ρ(P) ⊆ Uⁿ |
| értékelés v | Dom(v) = Var; v(x) ∈ U |
| módosított értékelés | vx:u = u, ha y = x; v(y) különben |
| modell | formulahalmaz modellje: olyan interpretáció (elsőrendben ⟨U, ρ, v⟩), ahol minden elem igaz; formula modellje: az {A} modellje |
| kielégíthető | van modellje (halmazra és formulára) |
| kielégíthetetlen | nem kielégíthető, nincs modellje |
| logikai következmény | Γ ⊨ A, ha Γ ∪ {¬A} kielégíthetetlen; A ⊨ B: {A} ⊨ B |
| érvényesség | ⊨ A, ha ∅ ⊨ A |
| logikai ekvivalencia | A ⇔ B, ha A ⊨ B és B ⊨ A |

### 12.3 Kalkulus

| Fogalom | Definíció lényege |
| --- | --- |
| axiómaséma | A1, A2, A3 (lásd 11.1) |
| szabályos behelyettesítés | a séma betűinek tetszőleges formulával való helyettesítése |
| axióma | az axiómasémák szabályos behelyettesítései |
| szintaktikai következmény | Γ ⊢ A: induktívan: A ∈ Γ vagy A axióma; ha Γ ⊢ B és Γ ⊢ B ⊃ A, akkor Γ ⊢ A |
| inkonzisztens | Cns(Γ) = Form |
| konzisztens | nem inkonzisztens |
| levezethető | ⊢ A, ha ∅ ⊢ A |
| szekvencia | „Γ ⊢ A” jelsorozat |
| természetes levezetés | a PrND halmaz induktív definíciója (11.2) |

### 12.4 Normálformák (recept, nem csak definíció)

**Tétel (normálforma tétel):** minden A formulához van olyan B, hogy A ⇔ B, és B diszjunktív **vagy** konjunktív normálformájú.

**Recept igazságtáblából:**

- **DNF:** vedd az eredményoszlop **1**-es sorait. Minden ilyen sorból írj egy elemi konjunkciót (ha a változó a sorban 1, írd p-t, ha 0, írd ¬p-t). A kapott konjunkciókat kapcsold ∨-val.
- **KNF:** vedd a **0**-s sorokat. Minden sorból írj elemi diszjunkciót **fordítva** (ha a változó a sorban 1, írd ¬p-t, ha 0, írd p-t). Kapcsold ∧-val.

**Példa:** (p ⊃ q) táblája: 00→1, 01→1, 10→0, 11→1. DNF: (¬p ∧ ¬q) ∨ (¬p ∧ q) ∨ (p ∧ q). KNF: egyetlen 0-s sor (p=1, q=0) → (¬p ∨ q).

## 13. ZH-lapozó

### 13.1 Feladattípus → mit csinálj

| Ha ezt kérdezik… | …akkor ezt csináld | Hol van |
| --- | --- | --- |
| Formula-e? | zárójelek, két formula egymás mellett? | 3.6 |
| Szerkezeti fa | keresd a főjelet (leggyengébb), részformulák | 3.4 |
| Zárójel elhagyása | ¬, kvantor > ∧ > ∨ > ⊃ > ≡; erősebbnél elhagy | 3.5 |
| Szabad/kötött változó | húzd alá a kvantorok hatáskörét | 3.7 |
| Magyar mondat → formula | alapállítások, betűk, kötőszavak, ∀→⊃, ∃→∧ | 3.3, 4.6 |
| Igazságérték adott interpretációban | belülről kifelé számol, a ⊃ hamis csak 1→0 | 4.2, 4.3 |
| Kvantoros formula kicsi U-ban | ∀ = minden elem, ∃ = van elem; sorrend számít | 4.4 |
| Kielégíthető/érvényes/kielégíthetetlen | igazságtábla, eredményoszlop | 5.2 |
| Kielégíthető-e a formulahalmaz? | keress egy interpretációt, ahol minden elem 1 | 5.4 |
| Következik-e Γ-ból A? | premissák 1-re, nézd az A-t; vagy keress ellenpéldát | 6.1 |
| Szöveges következtetés helyes-e | betűzz, formulázz, ellenpélda | 6.3 |
| Ekvivalens-e / törvény-e | tábla vagy átalakítás (⊃ → ¬∨, De Morgan) | 7.2 |
| Kielégíthető-e gépiesen | szemantikus fa (α/β) | 8 |
| Igaz-e az állítás a halmazokról | ellenpélda ({p, ¬p}, p ∨ ¬p, p, q) | 9.1 |
| Bizonyítsd, hogy Γ ⊨ … | indirekt, modell, ellentmondás | 5.3, 9.2 |
| Kvantor-törvény | De Morgan: ¬∃ ⇔ ∀¬; mozgatás: x ∉ FreeVar(A), ⊃ bal oldalán átfordul | 10.2, 10.3 |
| Behelyettesítés | beleragad-e a változó egy kvantorba? | 10.4 |
| Prenex alak | tiszta alak, ⊃ kezelés, ¬ befelé, kvantorok előre | 10.5 |
| Kalkulus | A1–A3, leválasztás, dedukció tétel | 11 |

### 13.2 A tizenkettő leggyakoribb csapda

1. ⊃ hamis **csak** 1→0 esetén. A hamis előtag → az egész igaz.
2. ∨ megengedő (mindkettő igaz is lehet).
3. „Csak akkor B, ha A” = **B ⊃ A**.
4. ∀ → ⊃, ∃ → ∧. Fordítva szinte mindig hibás.
5. ∀y∃x ≠ ∃x∀y.
6. ¬∀ ⇔ ∃¬, ¬∃ ⇔ ∀¬ (a kvantor átfordul).
7. Kvantor hatásköre: ∀xP(x) ∨ Q(x) → a Q(x) szabad.
8. ⊨ és ⊃ különböző „szint”; ⇔ és ≡ szintún.
9. A ⊃ B ≠ B ⊃ A, A ⊃ B ≠ ¬A ⊃ ¬B. Csak A ⊃ B ⇔ ¬B ⊃ ¬A.
10. {p, ¬p} elemei külön-külön kielégíthetőek, de a halmaz nem.
11. Az üres halmaz kielégíthető; ⊨ A azt jelenti, hogy A mindig igaz.
12. Azonos jelek csoportosításánál (p ⊃ q ⊃ r) a kiskáté és az előadás eltér: **mindig zárójelezz**.

### 13.3 Önteszt (takard le a választ!)

| Kérdés | Válasz |
| --- | --- |
| Mi a különbség term és formula között? | term = név/dolog (Péter, anyja(Péter)); formula = állítás, igaz/hamis (elmegy(Péter)) |
| Mi a függvény és a predikátum különbsége? | függvény dolgot ad vissza, predikátum igaz/hamis állítást |
| Mikor szabad egy változó? | ha nem esik az éppen azon változót kötő kvantor hatáskörébe |
| Mi az interpretáció? | ⟨U, ρ⟩: univerzum + a nem logikai jelek jelentése |
| Mi a modell? | olyan interpretáció, ahol a formula(halmaz) minden eleme igaz |
| Mikor érvényes egy formula? | minden interpretációban igaz |
| Mikor kielégíthetetlen? | egyetlen interpretációban sem igaz |
| Γ ⊨ A definíciója? | Γ ∪ {¬A} kielégíthetetlen (= Γ minden modellje A-nak is modellje) |
| Mit mond a dedukció tétel? | Γ ∪ {A} ⊨ B ⇒ Γ ⊨ A ⊃ B (és fordítva) |
| Mit jelent ⊨ A? | A érvényes (∅ ⊨ A) |
| Mikor ekvivalens A és B? | A ⊨ B és B ⊨ A; ⊨ A ≡ B |
| Mi a De Morgan? | ¬(A ∧ B) ⇔ ¬A ∨ ¬B; ¬(A ∨ B) ⇔ ¬A ∧ ¬B; ¬∃xA ⇔ ∀x¬A; ¬∀xA ⇔ ∃x¬A |
| Mik az axiómák? | A ⊃ (B ⊃ A); (A ⊃ (B ⊃ C)) ⊃ ((A ⊃ B) ⊃ (A ⊃ C)); (¬A ⊃ ¬B) ⊃ (B ⊃ A) |
| Mi az egyetlen levezetési szabály? | leválasztás (modus ponens) |
| Mi a prenex alak? | minden kvantor elöl, utána kvantormentes formula |

### 13.4 Vizsga előtti utolsó óra

1. Tudj fejből: az igazságtábla (4.2), a De Morgan, a kontrapozíció, A ⊃ B ⇔ ¬A ∨ B (7.1).
2. Oldj meg **kettőt** minden feladattípusból a 9. fejezet mintájára, megoldás nélkül.
3. A bizonyításoknál mindig a **sablon**: indirekt → modell → ellentmondás.
4. Ha elakadsz egy formulán: rajzold fel a szerkezeti fát, és menj belülről kifelé.
