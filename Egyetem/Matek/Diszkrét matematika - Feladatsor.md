---
tags: [diszkrét-matematika, feladatsor, gyakorlat]
---

# Diszkrét matematika — Gyakorlati feladatsor

> [!info] Használat
> Minden feladat elé egy pipálható checkbox kerül (`- [ ]`), így Obsidianban követheted, mit oldottál már meg. A képletek megjelenítéséhez kapcsold be: `Settings → Editor → Show math`. Az elméleti hátteret lásd: [[Diszkrét matematika - Elmélet]].

---

## 1. Halmazok és függvények

- [ ] **1.1** Legyen $A = \{x \in \mathbb{N} \mid x \text{ páros}\}$, $B = \{x \in \mathbb{N} \mid x > 4\}$, valamint $C = \{x \in \mathbb{N} \mid x < 6\}$. Mivel egyenlőek az alábbi halmazok? (Ábrázoljuk is Venn-diagram segítségével!)
  $$B \setminus C,\quad A \setminus (B \cap C),\quad B \triangle C,\quad (B \cup C)\setminus A,\quad \overline{C}\ (\text{alaphalmaz: } \mathbb{N}),\quad A \setminus C$$

	- [ ] **1.2** Tekintsük az alábbi halmazokat: $A = \mathbb{Z}$, $B = \{x \in \mathbb{Z} \mid x \text{ páros}\}$,$C = \{0,1,2,3,4\}$. Mivel egyenlőek az alábbiak?
  $$A\setminus B,\quad B\setminus A,\quad A\cap B,\quad B\triangle A,\quad \overline{B}\ (\text{alaphalmaz: } \mathbb{Z}),\quad C\setminus B,\quad B\triangle C$$

- [ ] **1.3** Írjuk fel az $A=\{a,b,c\}$ halmaz hatványhalmazát!

- [ ] **1.4** Egyenlő-e az alábbi két halmaz?
  - (a) Az 5-nél nagyobb egész, valamint a 6-nál nem kisebb természetes számok halmaza;
  - (b) $A = \{x\in\mathbb{R} \mid 0<x<1 \text{ és } x<x^2\}$ és $B=\{x\in\mathbb{R} \mid x<0 \text{ és } x=|x|\}$;
  - (c) $A=\mathbb{R}$ és $B=\{x\in\mathbb{R} \mid \sqrt{x^2}=x\}$.

- [ ] **1.5** Legyenek $A, B, C$ a $H$ alaphalmaz tetszőleges részhalmazai. Írjuk fel halmazműveletekkel $H$ azon elemeinek halmazát, amelyek:
  - (a) csak a $B$ halmaznak elemei ($A,B,C$ közül);
  - (b) pontosan két halmaznak elemei;
  - (c) nem elemei mindhárom halmaznak;
  - (d) legfeljebb egy halmaznak elemei;
  - (e) legalább két halmaznak elemei;
  - (f) legalább egy halmaznak elemei.

- [ ] **1.6** Jelölje $X$ a 2025. szeptember 1-jén 20–22 óra között a "Vak késdobáló" nevű vendéglátóegységben megjelent vendégek halmazát. Részhalmazai: $N$ = nők, $T$ = törzsvendégek (heti ≥4 alkalom), $A$ = alkalmi vendégek (≤2 alkalom eddig), $S$ = sört ivók, $B$ = bort ivók. Fogalmazzuk meg halmazelméleti műveletekkel/relációkkal:
  - (a) A bort ivó alkalmi vendégek között nincs nő.
  - (b) A férfi törzsvendégek sört és bort is isznak.
  - (c) Nincs olyan sörivó nő, aki törzsvendég.
  - (d) Aki vagy csak sört, vagy csak bort iszik, az alkalmi vendég és nő.
  - (e) Minden alkalmi vendég sörivó.

- [ ] **1.7** Legyen $A$ egy $m$ elemű, $B$ egy $n$ elemű halmaz. Adjuk meg $m,n$ függvényében, legalább ill. legfeljebb hány eleme lehet az alábbi halmazoknak!
  $$A\cup B,\quad A\cap B,\quad A\setminus B,\quad A\triangle B,\quad A\times B$$

- [ ] **1.8** Egy társaságban 27-en beszélnek angolul, 23-an németül, 12-en mindkét nyelvet, 8-an egyiket sem. Hány tagú a társaság?

### Függvényábrázolás (1.9–1.11)

- [ ] **1.9** Ábrázoljuk az alábbi függvényeket! (Értelmezési tartomány: $\mathbb{R}$ legbővebb lehetséges részhalmaza.)
  - (a) $f(x)=2x+3$
  - (b) $f(x)=-\dfrac{x}{2}$
  - (c) $f(x)=6$
  - (d) Az $f(x)$ elsőfokú függvényt, ha $f$ grafikonja illeszkedik a $(-3,1)$ és $(2,-2)$ pontokra. Mivel egyenlő $f(x)$?
  - (e) $f(x)=2x^2+8x-10$
  - (f) $f(t)=t^2-6t+9$
  - (g) $f(x)=-x^2-4$

- [ ] **1.10** Ábrázoljuk az alábbi függvényeket!
  - (a) $f(x)=8-2x$
  - (b) $f(x)=\dfrac{x}{4}+1$
  - (c) Az $f(x)$ elsőfokú függvényt, ha $f$ grafikonja illeszkedik a $(-2,0)$ és $(3,4)$ pontokra. Mivel egyenlő $f(x)$?
  - (d) $f(x)=-2x^2-5x+3$
  - (e) $f(x)=x^2-2x+4$
  - (f) $f(x)=x^2-4x+4$
  - (g) $f(x)=0{,}7^x$
  - (h) $f(x)=3^x$
  - (i) $f(x)=\log_4 x$
  - (j) $f(x)=\log_{0,5} x$

- [ ] **1.11** Tekintsük az $f:\mathbb{R}\to\mathbb{R}$, $f(x)=x^3-x=x(x-1)(x+1)$ függvényt. Ábrázoljuk $f(x)$-et, majd az alábbi transzformáltjait!
  - (a) $f(x-5)$
  - (b) $f(2x)+3$
  - (c) $3f(-x)-4$
  - (d) $\tfrac12 f(-x+4)-2$
  - (e) $-\tfrac12 f(x+3)$
  - (f) $2f(2x-4)+1$
  - (g) $-f(-x+3)$
  - (h) $f(-2x)-5$

- [ ] **1.12** Döntsük el, hogy az alábbi függvények közül melyik injektív, szürjektív, ill. bijektív! (Ha mást nem mondunk: értelmezési tartomány $\mathbb{R}$ legbővebb lehetséges részhalmaza, képtér $\mathbb{R}$.)
  - (a) $f(x)=x^2-6x+9$
  - (b) $f(x)=x^3$
  - (c) $f(x)=x^3-x$
  - (d) $f(x)=ax+b$
  - (e) $f(x)=2^x$
  - (f) $f(x)=\log_{1/3} x$
  - (g) $g(x)=\sin x$
  - (h) $h(x)=\sin x\big|_{[-\pi/2,\ \pi/2]}$ — mi a helyzet, ha a képtér $[-1,1]$?

---

## 2. Teljes indukció

*Bizonyítsuk be teljes indukcióval az alábbi állításokat!*

- [ ] **2.1** $1+2+\cdots+n=\dfrac{n(n+1)}{2}\quad \forall n\in\mathbb{N}$
- [ ] **2.2** $1^2+2^2+\cdots+n^2=\dfrac{n(n+1)(2n+1)}{6}\quad \forall n\in\mathbb{N}$
- [ ] **2.3** $1^3+2^3+\cdots+n^3=\left(\dfrac{n(n+1)}{2}\right)^2\quad \forall n\in\mathbb{N}$
- [ ] **2.4** $1+3+5+\cdots+(2n-1)=n^2\quad \forall n\in\mathbb{N}$
- [ ] **2.5** $1^2+3^2+5^2+\cdots+(2n-1)^2=\dfrac{n(4n^2-1)}{3}\quad \forall n\in\mathbb{N}$
- [ ] **2.6** $1\cdot2+2\cdot3+\cdots+n(n+1)=\dfrac{n(n+1)(n+2)}{3}\quad \forall n\in\mathbb{N}$
- [ ] **2.7** $\dfrac{1}{1\cdot2}+\dfrac{1}{2\cdot3}+\cdots+\dfrac{1}{n(n+1)}=\dfrac{n}{n+1}\quad \forall n\in\mathbb{N}$
- [ ] **2.8** $1\cdot4+2\cdot7+\cdots+n(3n+1)=n(n+1)^2\quad \forall n\in\mathbb{N}$
- [ ] **2.9** $\left(1-\dfrac14\right)\left(1-\dfrac19\right)\cdots\left(1-\dfrac{1}{(n+1)^2}\right)=\dfrac{n+2}{2n+2}\quad \forall n\in\mathbb{N}$
- [ ] **2.10** $1\cdot1!+2\cdot2!+\cdots+n\cdot n!=(n+1)!-1\quad \forall n\in\mathbb{N}$
- [ ] **2.11** $6\mid(n^3-n)\quad \forall n\in\mathbb{N}$
- [ ] **2.12** $6\mid(n^3+5n)\quad \forall n\in\mathbb{N}$
- [ ] **2.13** $5\mid(2^{4n+1}+3)\quad \forall n\in\mathbb{N}$
- [ ] **2.14** $3\mid(n^3+5n+6)\quad \forall n\in\mathbb{N}$
- [ ] **2.15** $9\mid(10^n+3\cdot4^{n+2}+5)\quad \forall n\in\mathbb{N}$
- [ ] **2.16** $4\mid(7^n+10n-5)\quad \forall n\in\mathbb{N}$
- [ ] **2.17** $\dfrac{n^3}{3}+\dfrac{n^5}{5}+\dfrac{7n}{15}$ egész szám minden $n\in\mathbb{N}$ esetén
- [ ] **2.18** $(n+1)! > 2^{n+3}$, ha $n\ge5$, $n\in\mathbb{N}$
- [ ] **2.19** Az elsős PTI évfolyam $n\ge4$ hallgatójának mindegyike tud egy pletykát (mindenki különbözőt). Ha két hallgató felhívja egymást, elmondják egymásnak az összes addig tudott pletykát. Bizonyítsuk be teljes indukcióval, hogy $2n-4$ telefonhívással elérhető, hogy mindenki ismerje az összes pletykát!

---

## 3. Oszthatóság

- [ ] **3.1** Igazoljuk az alábbi oszthatóságokat!
  - (a) $9\mid(10^{19}+53)$
  - (b) $36\mid(10^{17}-64)$
  - (c) $6\mid(10^7-88)$
  - (d) $12\mid(10^{16}+44)$

- [ ] **3.2** Milyen számjegyeket írhatunk $a$ és $b$ helyére, hogy teljesüljön az oszthatóság?
  - (a) $33\mid\overline{52ab71}$
  - (b) $36\mid\overline{762a4b}$
  - (c) $45\mid\overline{61a24b}$
  - (d) $72\mid\overline{44a21b}$

- [ ] **3.3** Igazoljuk az alábbi oszthatóságokat!
  - (a) $200\mid(199^3-199)$
  - (b) $7\mid(11^9-4^9)$
  - (c) $200\mid(101^3+99^3)$
  - (d) $99\mid(11^{22}-22^{11})$

- [ ] **3.4** Számítsuk ki, hogy $100!$ hány nullára végződik!

- [ ] **3.5** Létezik-e olyan $n$ egész szám, hogy $n!$ pontosan 5 nullára végződik?

- [ ] **3.6** Igazoljuk, hogy négy egymást követő egész szám szorzata mindig osztható 24-gyel!

- [ ] **3.7** Igazoljuk, hogy ha egy tetszőleges pozitív háromjegyű számot kétszer egymás után leírunk, a kapott hatjegyű szám osztható 13-mal!

- [ ] **3.8** Euklideszi algoritmussal számítsuk ki az alábbi számok legnagyobb közös osztóját!
  - (a) 672 és 360 (b) 455 és $-312$ (c) 1225 és 216 (d) 680 és $-845$ (e) 783 és 1160 (f) 3751 és 1240

- [ ] **3.9** Mutassuk meg, hogy nem léteznek olyan $a,b$ pozitív egészek, hogy $a^2=5b^2$.

- [ ] **3.10** Bizonyítsuk be, hogy az alábbi egyenletek nem oldhatók meg pozitív egész számok körében!
  - (a) $n^{k+1}=(n+1)^k$
  - (b) $a^6+25a=7425$
  - (c) $k(k^4+1)=3267$

- [ ] **3.11** Miért nem lehet két prímszám összege 2017?

- [ ] **3.12** Lehet-e 2007 egymást követő egész szám összege prímszám?

- [ ] **3.13** Van-e olyan $n$ egész szám, melyre $2^n-1$ és $2^n+1$ is prímszám?

- [ ] **3.14** Lehetnek-e az $n-12$, $n+3$ és $n+5$ számok egyszerre prímszámok, ahol $n\in\mathbb{Z}$?

- [ ] **3.15** Legyen $p>3$ prímszám. Mutassuk meg, hogy $24\mid(p^2-1)$.

- [ ] **3.16** Határozzuk meg az alábbi számok legnagyobb közös osztóját és legkisebb közös többszörösét!
  - (a) 450, 420 (b) 539, 364 (c) 1260, 14850 (d) 663, 308 (e) 495, 300 (f) 990, 420

- [ ] **3.17** Bizonyítsuk be, hogy négy egymást követő természetes szám között mindig van egy, amely a másik háromhoz relatív prím!

- [ ] **3.18** Hány pozitív osztója van az alábbi számoknak? (a) 252 (b) 600 (c) 528

- [ ] **3.19** Melyik a legkisebb természetes szám, amelynek pontosan 12 pozitív osztója van?

- [ ] **3.20** Hány olyan pozitív osztója van 7560-nak, amely a 15-höz relatív prím?

- [ ] **3.21*** Tudjuk, hogy valamely pozitív egész $n$-re a $110n^3$ számnak pontosan 110 pozitív osztója van. Hány pozitív osztója van a $81n^4$ számnak?

- [ ] **3.22** Oldjuk meg (ha lehetséges) az alábbi lineáris diofantikus egyenleteket!
  - (a) $14x-18y=6$
  - (b) $15x+28y=12$
  - (c) $12x-15y=26$
  - (d) $21x-15y=12$
  - (e) $495x+300y=15$
  - (f) $18x+28y=10$

- [ ] **3.23** Gombóc Artúrnak 1420 Ft-ja van, amit csokoládéra költ. Lyukas csoki: 35 Ft/db, kerek csoki: 40 Ft/db. Hogyan választhat?

- [ ] **3.24*** Péter egy 20 szálas csokrot vett 1430 Ft-ért, háromféle virágból (50, 70, 80 Ft/szál), egyik fajtából sincs 10-nél több. Hány szál van az egyes fajtákból?

- [ ] **3.25** Oldjuk meg (ha lehetséges) az alábbi lineáris kongruenciákat!
  - (a) $3x\equiv5\pmod7$
  - (b) $12x\equiv8\pmod{16}$
  - (c) $9x\equiv15\pmod{12}$
  - (d) $5x\equiv4\pmod{11}$
  - (e) $5x\equiv24\pmod{13}$
  - (f) $14x\equiv8\pmod{21}$
  - (g) $11x\equiv12\pmod{18}$
  - (h) $30x\equiv48\pmod{58}$

- [ ] **3.26** Mennyi maradékot ad
  - (a) $39^{28}$, ha 29-cel osztjuk
  - (b) $17^{40}$, ha 25-tel osztjuk
  - (c) $23^{81}$, ha 50-nel osztjuk
  - (d) $17^{18}$, ha 40-nel osztjuk
  - (e*) $5^{4\,5^{56}}$, ha 13-mal osztjuk
  - (f*) $3^{8\,3^{940}}$, ha 11-gyel osztjuk

- [ ] **3.27** Mi az utolsó két számjegye $19^{81}$-nek?

---

## 4. Komplex számok

- [ ] **4.1** Oldjuk meg az alábbi egyenleteket a komplex számok halmazán! Ábrázoljuk a megoldásokat a komplex számsíkon!
  - (a) $x^2+4=0$
  - (b) $x^2+2x+2=0$
  - (c) $x^2-x+1=0$
  - (d) $x^3-6x^2+13x=0$

- [ ] **4.2** Írjuk fel az alábbi komplex számok algebrai alakját!
  - (a) $(3+i)(2+3i)$, $(-2+3i)(5-2i)$, $i(1+2i)$, $(-1+i)(1-2i)(1+2i)$
  - (b) $5-2i$, $(3+4i)(2+i)$
  - (c) $(2-i)^3$, $i^6+3i^5-2i^3+i^2-1$, $i^{2017}$, $i^{100}$
  - (d) $\dfrac{5+3i}{i}$, $\dfrac{1-i}{2+i}$, $\dfrac{1-2i}{1-3i}$, $\dfrac{2-i}{(3-2i)(2+5i)}$

- [ ] **4.3** Oldjuk meg az alábbi egyenleteket a komplex számok halmazán!
  - (a) $z+2\overline{z}=9+2i$
  - (b) $z+|z|^2=31-i$
  - (c) $i^3\cdot z=-3-2i$
  - (d) $z^2+|z|^2=2-6i$

- [ ] **4.4** Adjuk meg az alábbi komplex számok trigonometrikus alakját!
  - (a) $2$ (b) $i$ (c) $-i$ (d) $1-i$ (e) $1+i$ (f) $-\tfrac12+\tfrac{\sqrt3}{2}i$ (g) $-3-3\sqrt3 i$ (h) $\tfrac{1}{\sqrt2}+\tfrac{1}{\sqrt2}i$

- [ ] **4.5** Legyen $x=3\left(\cos\tfrac{\pi}{9}+i\sin\tfrac{\pi}{9}\right)$, $y=2\left(\cos\tfrac{\pi}{5}+i\sin\tfrac{\pi}{5}\right)$. Határozzuk meg:
  - (a) $x\cdot y$ (b) $x/y$ (c) $x^3$ (d) $y^5$ (e) $1/x$ (f) $x^2/y$

- [ ] **4.6** Trigonometrikus alak segítségével határozzuk meg:
  - (a) $\sqrt{i}$ (b) $\sqrt[3]{i}$ (c) $(1+i)^{2008}$ (d) $(1+\sqrt3 i)^{301}$

- [ ] **4.7** Számítsuk ki $z=81\left(\cos\tfrac{\pi}{5}+i\sin\tfrac{\pi}{5}\right)$ második, harmadik, negyedik gyökeit! Ábrázoljuk a komplex számsíkon!

- [ ] **4.8** Írjuk fel és ábrázoljuk a harmadik, negyedik, ötödik és hatodik egységgyököket!

- [ ] **4.9** Oldjuk meg az alábbi egyenleteket a komplex számok halmazán!
  - (a) $z^2-3iz+4=0$
  - (b) $z^3+z^2+z=0$
  - (c) $z^5-z=0$
  - (d) $z^2+(2+4i)z-3+3i=0$

---

## 5. Polinomok

- [ ] **5.1** Írjuk fel az alábbi polinomok gyöktényezős felbontását $\mathbb{R}$ felett!
  - (a) $2x^2-2x-12$
  - (b) $-x^3-3x^2+4x$
  - (c) $4x^2-4x-3$
  - (d) $x^3+3x^2+3x+1$
  - (e) $x^3-6x^2+12x-8$
  - (f) $x^3-3x^2-x+3$
  - (g) $x^4-16$
  - (h) $x^4-2x^2+1$

- [ ] **5.2** Írjunk fel minimális fokszámú valós együtthatós polinomot, melynek
  - (a) a $-3$ kétszeres, az $1$ egyszeres gyöke;
  - (b) az $5$ és az $i$ gyöke;
  - (c) a $2$ és a $\tfrac{1}{\sqrt2}+\tfrac{1}{\sqrt2}i$ gyöke;
  - (d) az $i$ kétszeres gyöke.

- [ ] **5.3** Adjuk meg azt a minimális fokszámú $p$ polinomot, melynek $-1$, $2$, $3$ gyöke, és $p(0)=4$.

- [ ] **5.4** Végezzük el a kijelölt osztásokat!
  - (a) $(2x^3-x^2-5x-2):(x-2)$
  - (b) $(2x^4-5x^3-7x^2+14x-6):(x-3)$
  - (c) $(-3x^3+48x):(x-4)$
  - (d) $(6x^3+9x^2-5x-4):(2x+1)$
  - (e) $(6x^4-7x^3+5x^2-2x):(2x^2-x+1)$

- [ ] **5.5** Végezzük el az alábbi polinomokon a maradékos osztást!
  - (a) $2x^4-3x^3+4x^2-5x+6,\ \ x^2-3x+1$
  - (b) $2x^5-5x^3-8x,\ \ x+3$
  - (c) $x^5-3x^4+1,\ \ x^2+x+1$
  - (d) $x^4+x^3-3x^2-4x-1,\ \ x^3+x^2-x-1$
  - (e) $x^4-10x^2+1,\ \ x^4-4\sqrt2x^3+6x^2+4\sqrt2x+1$

- [ ] **5.6** Horner-algoritmussal határozzuk meg a helyettesítési értékeket!
  - (a) $p(x)=2x^5-3x^4+x^3-5x^2+2x-4$: $p(2)=?$, $p(-1)=?$
  - (b) $p(x)=2x^6-3x^4+x^2+2x+1$: $p(-2)=?$
  - (c) $p(x)=x^5-3x^4+4x^2+3x+2$: $p(-2)=?$, $p(3)=?$
  - (d) $p(x)=6x^3+9x^2-5x-4$: $p(-\tfrac12)=?$

- [ ] **5.7** Hányszoros gyöke $x_0=-1$ a $p(x)=2x^5+8x^4+11x^3+5x^2-x-1$ polinomnak?

---

## 6. Kombinatorika

- [ ] **6.1** Hány különböző 5 hosszúságú karaktersorozat készíthető az $a,b,c,d,e$ karakterekből, ha minden karakter csak egyszer szerepelhet?
- [ ] **6.2** Melyikből van több: csupa különböző számjegyből álló tízjegyű, vagy kilencjegyű számból?
- [ ] **6.3** Hányféleképpen rakhatunk sorba 12 könyvet, ha 3 bizonyos könyvet egymás mellé akarunk rakni, és (a) a három könyv sorrendje nem számít? (b) számít?
- [ ] **6.4** Hányféleképpen helyezhető el a sakktáblán 8 egyforma bástya úgy, hogy egyik se üsse a másikat? És ha a 8 bástya megkülönböztethető?
- [ ] **6.5** Hányféleképpen ültethetünk kerek asztal köré 10 embert (forgatással azonos ülésrendek egyformák)?
- [ ] **6.6** Hányféleképpen ültethetünk kerek asztal köré 5 házaspárt úgy, hogy a házastársak egymás mellett üljenek?
- [ ] **6.7** Hányféleképpen ültethetünk kerek asztal köré 5 férfit és 5 nőt úgy, hogy se két férfi, se két nő ne legyen egymás mellett?
- [ ] **6.8** Hány olyan hatjegyű szám van, melyben három 1-es, két 2-es és egy 3-as szerepel?
- [ ] **6.9** Egy urnában 5 piros, 7 fehér, 3 zöld golyó van. Egyesével húzva, sorrendben feljegyezve, hányféle színsorozat lehetséges?
- [ ] **6.10** 10 diákot hányféleképpen helyezhetünk el egy két-, egy három- és egy ötágyas szobában?
- [ ] **6.11** Hányféleképpen osztható szét 12 gyerek között 15 könyv, ha nem kötelező mindenkinek adni?
- [ ] **6.12** 6 ajánlott levelet 3 postás közt hányféleképpen oszthatunk szét?
- [ ] **6.13** Egy francia kártyacsomagból (4 szín, 13 lap/szín) hányféleképpen választható 4 páronként különböző színű lap? És ha az is kell, hogy ne legyen két azonos értékű?
- [ ] **6.14** 10 különböző korú embert hányféleképpen ültethetünk egymás mellé úgy, hogy a legidősebb és a legfiatalabb ne üljön egymás mellett?
- [ ] **6.15** A számegyenesen origóból indulva 10 lépést teszünk (±1 egység lépésenként). Hányféleképpen érhetünk vissza az origóba?
- [ ] **6.16** Síkbeli rácson origóból $(4,3)$-ba hányféleképpen juthatunk el, ha csak jobbra vagy felfelé léphetünk?
- [ ] **6.17** Térbeli rácson origóból $(4,3,2)$-be hányféleképpen juthatunk el, ha $x$, $y$, vagy $z$ irányban léphetünk pozitív irányba egységet?
- [ ] **6.18** Hányféleképpen olvasható ki a "KOMBINATORIKA" szó a megadott betűtáblázatból?
- [ ] **6.19** 5 piros és 8 fehér golyót hányféleképpen rakhatunk sorba úgy, hogy ne legyen két piros egymás mellett?
- [ ] **6.20** $n$ darab nullát és $k$ darab egyest ($k\le n+1$) hányféleképpen rakhatunk sorba úgy, hogy két egyes ne legyen egymás mellett?
- [ ] **6.21** 4 fiút és 6 lányt hányféleképpen állíthatunk sorba úgy, hogy két fiú ne álljon egymás mellett?
- [ ] **6.22** 6 piros, 5 fehér, 4 kék golyót hányféleképpen rakhatunk sorba úgy, hogy 2 piros ne kerüljön egymás mellé?
- [ ] **6.23** 6 fiúból és 8 lányból hányféleképpen választható ki 4 táncoló pár (párok tagjai különböző neműek)?
- [ ] **6.24** 15 férfi és 16 nő közül hányféleképpen választható ki 7 fő úgy, hogy pontosan 4 férfi legyen köztük?
- [ ] **6.25** Egy 11 fős társaságból hányféleképpen választható ki két négyfős csapat?
- [ ] **6.26** Egy adott héten hányféleképpen fordulhat elő pontosan három találat az ötöslottón (90-ből húznak 5-öt)?
- [ ] **6.27** Egy szelvénnyel hányféleképpen fordulhat elő legalább három találat az ötöslottón?
- [ ] **6.28** Egy francia kártyacsomagból kihúzunk 10 lapot. Hány esetben lesz köztük (a) ász? (b) pontosan egy ász? (c) legfeljebb egy ász? (d) pontosan két ász? (e) legalább két ász?
- [ ] **6.29** 4 gyerek között hányféleképpen osztható szét 7 alma és 9 körte?
- [ ] **6.30** $(2+x)^8$ kifejtésében mi az $x^3$ tag együtthatója?
- [ ] **6.31** $(1+a)^{17}$ kifejtésében mi az $a^{10}$ tag együtthatója?
- [ ] **6.32** Igazoljuk: $\dbinom{n}{k}=\dbinom{n-1}{k-1}+\dbinom{n-1}{k}$
- [ ] **6.33** Igazoljuk:
  $$\binom{n}{0}+\binom{n}{1}+\cdots+\binom{n}{n}=2^n \qquad \binom{n}{0}-\binom{n}{1}+\binom{n}{2}-\cdots+(-1)^n\binom{n}{n}=0$$

---

## 7. Vektorterek

- [ ] **7.1** Legyen $a=\begin{pmatrix}-1\\2\end{pmatrix}$, $b=\begin{pmatrix}4\\-3\end{pmatrix}$, $c=\begin{pmatrix}-5\\-2\end{pmatrix}$, $d=\begin{pmatrix}0\\-1\end{pmatrix}$. Számítsuk ki: $a+b$, $-5c$, $12a+4b-3c+d$.

- [ ] **7.2** Legyen $a=\begin{pmatrix}0\\-1\\1\end{pmatrix}$, $b=\begin{pmatrix}2\\-2\\-3\end{pmatrix}$, $c=\begin{pmatrix}-4\\-2\\1\end{pmatrix}$. Számítsuk ki: $a+b$, $-3c$, $2a+4b-c$.

- [ ] **7.3** Alteret alkot-e...
  - (a) $\mathbb{R}^2$-ben $\{(0,a)\mid a\in\mathbb{R}\}$
  - (b) $\mathbb{R}^2$-ben $\{(1,a)\mid a\in\mathbb{R}\}$
  - (c) $\mathbb{R}^3$-ban $\{(x,0,z)\mid x,z\in\mathbb{R}\}$
  - (d) $\mathbb{R}^2$-ben $\{(x,y)\in\mathbb{R}^2\mid x+y=1\}$
  - (e) $\mathbb{R}^2$-ben $\{(x,y)\in\mathbb{R}^2\mid x+y=0\}$
  - (f) $\mathbb{R}^4$-ben $\{(x_1,x_2,x_3,x_4)\mid x_2=x_3\}$
  - (g) $\mathbb{R}^3$-ban $\{(x_1,x_2,x_3)\mid x_1=x_2=x_3\}$
  - (h) $\mathbb{R}^3$-ban $\{(x_1,x_2,x_3)\mid x_1+x_2=x_3\}$
  - (i) $\mathbb{R}^2$-ben $\{(x_1,x_2)\mid x_1,x_2\ge0\}$
  - (j) legfeljebb $n$-edfokú polinomok közt: $\{p \mid p(0)=0\}$
  - (k) legfeljebb $n$-edfokú polinomok közt: $\{p \mid p(0)=1\}$
  - (l) legfeljebb $n$-edfokú polinomok közt: páros fokszámúak
  - (m) valós számsorozatok közt: számtani sorozatok

- [ ] **7.4** Mutassuk meg, hogy $\begin{pmatrix}1\\2\end{pmatrix}$ és $\begin{pmatrix}-3\\2\end{pmatrix}$ lineárisan függetlenek $\mathbb{R}^2$-ben!

- [ ] **7.5** Lineárisan függetlenek-e $\mathbb{R}^3$-ban:
  - (a) $(1,0,1), (1,1,1)$
  - (b) $(1,0,0), (0,1,0), (1,1,1)$
  - (c) $(1,1,2), (-2,-2,-4)$
  - (d) $(1,2,2), (-1,-1,2), (2,3,0)$

- [ ] **7.6** Határozzuk meg az alábbi vektorok által generált alteret $\mathbb{R}^2$-ben!
  - (a) $(1,0),(0,1)$ (b) $(-2,0),(0,1)$ (c) $(1,-2),(2,-4)$ (d) $(3,2)$ (e) $(1,2),(3,1)$ (f) $(1,0),(2,-4),(0,2)$

- [ ] **7.7** Határozzuk meg az alábbi vektorok által generált alteret $\mathbb{R}^3$-ban!
  - (a) $(1,0,0),(0,1,0),(0,0,1)$
  - (b) $(1,0,0),(0,1,0)$
  - (c) $(1,1,0),(0,1,0)$
  - (d) $(0,0,1)$
  - (e) $(1,1,1),(0,0,1),(0,1,0)$
  - (f) $(1,0,0),(2,2,0),(0,-1,0)$

- [ ] **7.8** Írjuk fel $(1,2)$ koordinátáit $\mathbb{R}^2$ alábbi bázisaira:
  - (a) $(1,0),(0,1)$ (b) $(-1,2),(1,1)$ (c) $(3,1),(1,2)$ (d) $(1,-1),(1,3)$

- [ ] **7.9** Írjuk fel $(1,-1,2)$ koordinátáit $\mathbb{R}^3$ alábbi bázisaira:
  - (a) $(1,0,0),(0,1,0),(0,0,1)$
  - (b) $(1,0,0),(0,1,0),(1,1,1)$
  - (c) $(-1,2,1),(2,-3,1),(1,1,2)$
  - (d) $(1,2,2),(-1,-1,3),(2,3,0)$

---

## 8. Mátrixok

- [ ] **8.1** $A=\begin{pmatrix}4&-1&0\\1&5&-3\end{pmatrix}$, $B=\begin{pmatrix}0&3&-2\\1&1&4\end{pmatrix}$, $C=\begin{pmatrix}2&-1&1\\-1&3&10\\4&0&4\end{pmatrix}$, $D=\begin{pmatrix}1\\-2\\7\end{pmatrix}$. Számítsuk ki: $A+B$, $3A-2B$, $A^T+B^T$, $AD$, $CB$, $CD$, $D^TD$, $AC+BC$, $(A+B)C$.

- [ ] **8.2** $v=\begin{pmatrix}4\\-3\end{pmatrix}$, $w=\begin{pmatrix}0\\-1\end{pmatrix}$, $A=\begin{pmatrix}-1&3\\2&-4\end{pmatrix}$, $B=\begin{pmatrix}4&2\\-2&-3\end{pmatrix}$, $C=\begin{pmatrix}2&-3\\-3&-5\end{pmatrix}$, $E=\begin{pmatrix}1&0\\0&1\end{pmatrix}$. Számítsuk ki: $v^Tw$, $vw^T$, $Av$, $Aw$, $A(u+v)$, $w^TA^T$, $A+B$, $(-2C)^T$, $2A-3B$, $(A+B)^T$, $AB$, $BA$, $B^TA^T$, $AE$, $EA$, $AC$, $A(B+C)$, $C(A-2B)$, $A^TA$, $AA^T$.

- [ ] **8.3** $v=\begin{pmatrix}-4\\-2\\1\end{pmatrix}$, $w=\begin{pmatrix}0\\1\\0\end{pmatrix}$, $A=\begin{pmatrix}-1&7&-3\\4&2&2\\-2&0&3\end{pmatrix}$, $B=\begin{pmatrix}1&5&-1\\2&0&-6\\-3&3&4\end{pmatrix}$, $C=\begin{pmatrix}-4&2&-3\\-3&1&-5\\0&-2&2\end{pmatrix}$, $E$ = egységmátrix. Ugyanazokat a kifejezéseket számítsuk, mint 8.2-ben (3×3-as esetre).

- [ ] **8.4** Müzliszelet cukor/fehérje táblázat alapján, $A=\begin{pmatrix}9&8\\7&10\end{pmatrix}$, $x=\begin{pmatrix}x_1\\x_2\end{pmatrix}$. Mit adnak meg az $y=Ax$ vektor koordinátái?

- [ ] **8.5** Populáció-dinamika: 1-2-3 éves egyedek, reprodukciós ráták (6, 8), túlélési arányok (50-50%). Adjuk meg az $A$ mátrixot úgy, hogy $y=Ax$ a következő évi egyedszámokat adja. Mit ad meg $A^2x$, illetve $A^5x$?

- [ ] **8.6** Ajándékcsomagok (3+3, illetve 2+4 gyümölcs/csokoládé), összesen 170 gyümölcs, 250 csokoládé. Írjuk fel az egyenletet mátrix-vektor alakban.

- [ ] **8.7** Számítsuk ki a determinánsokat Sarrus-szabállyal!
  - (a) $\begin{pmatrix}1&3\\1&-2\end{pmatrix}$ (b) $\begin{pmatrix}5&-2\\2&1\end{pmatrix}$ (c) $\begin{pmatrix}2&1\\4&2\end{pmatrix}$
  - (d) $\begin{pmatrix}1&1&1\\1&2&3\\1&4&9\end{pmatrix}$ (e) $\begin{pmatrix}2&0&0\\-5&1&2\\3&8&-7\end{pmatrix}$ (f) $\begin{pmatrix}\cos\alpha&\sin\alpha&0\\-\sin\alpha&\cos\alpha&0\\0&0&1\end{pmatrix}$

- [ ] **8.8** Határozzuk meg az inverzet, ha létezik!
  - (a) $\begin{pmatrix}4&3\\7&5\end{pmatrix}$ (b) $\begin{pmatrix}2&3\\2&2\end{pmatrix}$ (c) $\begin{pmatrix}3&1\\-1&2\end{pmatrix}$ (d) $\begin{pmatrix}6&-2\\-3&1\end{pmatrix}$ (e) $\begin{pmatrix}6&-3\\4&2\end{pmatrix}$ (f) $\begin{pmatrix}5&-2\\4&1\end{pmatrix}$

- [ ] **8.9** Számítsuk ki a determinánsokat kifejtési tétellel!
  - (a) $\begin{pmatrix}2&0&0\\-5&1&2\\3&8&-7\end{pmatrix}$
  - (b) $\begin{pmatrix}2&-1&1\\-5&1&2\\3&0&-5\end{pmatrix}$
  - (c) $\begin{pmatrix}-2&2&-1&2\\0&-3&0&5\\3&-2&4&1\\-4&3&0&1\end{pmatrix}$

---

## 9. Lineáris egyenletrendszerek

- [ ] **9.1** 960 Ft-unk van, 20 és 50 Ft-os érmékben, összesen 30 érme. Melyikből mennyi?

- [ ] **9.2** Két szendvicstípus (6 szalámi+3 sajt, ill. 3 szalámi+1 sajt), összesen 144 szalámi, 63 sajt. Melyikből mennyi készült?

- [ ] **9.3** Az $Ax=b$ egyenletrendszer megoldásszámának megállapítása (megoldás nélkül):
  - (a) $A=\begin{pmatrix}-2&1&3\\0&2&4\\0&-2&0\end{pmatrix}$, $b=\begin{pmatrix}-4\\5\\3\end{pmatrix}$
  - (b) $A=\begin{pmatrix}-1&2&4\\-1&2&4\\-1&2&4\end{pmatrix}$, $b=\begin{pmatrix}-4\\5\\3\end{pmatrix}$
  - (c) ugyanaz az $A$, $b=\begin{pmatrix}5\\5\\5\end{pmatrix}$
  - (d) $A=\begin{pmatrix}-1&2&1\\3&2&5\end{pmatrix}$, $b=\begin{pmatrix}5\\-3\end{pmatrix}$
  - (e) $A=\begin{pmatrix}-1&2\\3&2\end{pmatrix}$, $b=\begin{pmatrix}0\\0\end{pmatrix}$
  - (f) $A=\begin{pmatrix}-1&2\\2&-4\end{pmatrix}$, $b=\begin{pmatrix}0\\0\end{pmatrix}$

- [ ] **9.4** Gauss-eliminációval kapott lépcsős kibővített mátrix-alakok (a)–(j) elemzése: $A$ rangja, kibővített mátrix rangja, oszlopvektorok lineáris függetlensége, $b$ benne van-e az oszloptérben, megoldásszám. *(lásd az eredeti feladatlap mátrixvázlatait)*

- [ ] **9.5** Oldjuk meg az alábbi lineáris egyenletrendszereket!
  - (a) $x_1-x_2+x_3=6,\ 2x_1+x_2-x_3=5,\ -3x_1+2x_2+5x_3=3$
  - (b) $x_1+4x_2=5,\ 2x_1+8x_2=10$
  - (c) $-3x_1+x_2+2x_3=-2,\ 4x_1-6x_2-x_3=17,\ x_1-5x_2+x_3=10$
  - (d) $2x_1+x_2-x_3=5,\ x_1-x_2+x_3=6,\ x_1+2x_2-2x_3=-1$
  - (e) $2x_1-x_2+2x_3=0,\ -6x_1+x_2-7x_3=0,\ 4x_1-4x_2+6x_3=0$
  - (f) $x_1-x_2+3x_3+5x_4=2,\ 2x_1+2x_2+x_4=0,\ -x_1-3x_2+3x_3+4x_4=2$
  - (g) $x+y+z=3,\ x+2y+2z=5,\ 3x+5y+6z=14,\ 2x+4y+5z=11,\ x+2y+3z=6$
  - (h) $-2x_1-x_2+4x_3=3,\ 2x_1+3x_2-x_3=1,\ -4x_1-10x_2-5x_3=-12$
  - (i) $-2x_1+2x_2+x_3=0,\ 6x_1-3x_2-4x_3=-8,\ -4x_1+x_2+x_3=4$
  - (j) $2x_1-4x_2+2x_3=-2,\ -4x_1+6x_2-x_3=5,\ x_1-2x_3=0$
  - (k) $-4x_1-4x_2+2x_3=-2,\ -2x_1-7x_2+3x_3=6,\ 2x_1+12x_2-5x_3=-13$
  - (l) $3x_1+2x_2-4x_3-x_4=4,\ -x_1-2x_2+3x_3+5x_4=-5,\ 2x_1+4x_2-2x_3-5x_4=8$
  - (m) $-x_1+3x_2-2x_3=3,\ 2x_1-4x_2+9x_3=3,\ -x_1+7x_2+7x_3=20,\ x_1-5x_2-2x_3=-11$

---

## 10. Mátrix rangja, determinánsa, inverze

- [ ] **10.1** Határozzuk meg az alábbi mátrixok rangját!
  - $\begin{pmatrix}3&1&-1\\0&1&-4\end{pmatrix}$, $\begin{pmatrix}3&1&-1\\-6&-2&2\end{pmatrix}$, $\begin{pmatrix}1&2&0\\-2&2&-6\\0&4&-4\\3&-3&9\end{pmatrix}$, $\begin{pmatrix}-1&0&1\\0&2&3\\0&0&7\end{pmatrix}$, $\begin{pmatrix}3&2&1&-1\\-9&-8&-1&6\\-6&0&-8&-8\\9&8&7&5\end{pmatrix}$, $\begin{pmatrix}-2&4\\1&3\\0&2\\-5&2\end{pmatrix}$

- [ ] **10.2** Határozzuk meg a determinánsokat!
  - (a) $\begin{pmatrix}1&0&0\\0&0&1\\0&1&0\end{pmatrix}$ (b) $\begin{pmatrix}3&-1&2\\0&0&5\\0&1&-5\end{pmatrix}$ (c) $\begin{pmatrix}2&-1&1\\4&-2&5\\-2&1&-5\end{pmatrix}$
  - (d) $\begin{pmatrix}-2&2&0&1\\4&-2&-1&1\\-4&-2&4&-8\\8&-6&1&0\end{pmatrix}$ (e) $\begin{pmatrix}6&0&-2&-2\\-2&1&0&1\\-4&8&-5&6\\6&-6&0&-2\end{pmatrix}$

- [ ] **10.3** Határozzuk meg az inverzet!
  - (a) $\begin{pmatrix}-1&2&1\\2&-5&1\\-3&7&1\end{pmatrix}$ (b) $\begin{pmatrix}-3&-7&0\\-1&-2&1\\2&5&2\end{pmatrix}$

---

## 11. Lineáris transzformációk

- [ ] **11.1** Vizsgáljuk meg a mátrixokkal adott lineáris transzformációk hatását tetszőleges $x=\begin{pmatrix}x_1\\x_2\end{pmatrix}\in\mathbb{R}^2$-re: $A=\begin{pmatrix}1&0\\0&1\end{pmatrix}$, $B=\begin{pmatrix}1&0\\0&0\end{pmatrix}$, $C=\begin{pmatrix}0&0\\0&1\end{pmatrix}$, $D=\begin{pmatrix}1&0\\0&-1\end{pmatrix}$.

- [ ] **11.2** Ugyanez $\mathbb{R}^3$-ra: $A=I_3$, $B=\begin{pmatrix}1&0&0\\0&1&0\\0&0&0\end{pmatrix}$, $C=\begin{pmatrix}1&0&0\\0&0&0\\0&0&1\end{pmatrix}$, $D=\begin{pmatrix}0&0&0\\0&0&0\\0&0&1\end{pmatrix}$, $E=\begin{pmatrix}1&0&0\\0&1&0\\0&0&-1\end{pmatrix}$.

- [ ] **11.3** Írjuk fel annak a lineáris transzformációnak a mátrixát, ami $\begin{pmatrix}1\\0\end{pmatrix}\mapsto\begin{pmatrix}2\\5\end{pmatrix}$, $\begin{pmatrix}0\\1\end{pmatrix}\mapsto\begin{pmatrix}3\\1\end{pmatrix}$. Mi lesz $\begin{pmatrix}2\\2\end{pmatrix}$ képe?

- [ ] **11.4** Írjuk fel annak a lineáris transzformációnak a mátrixát, ami $\begin{pmatrix}2\\5\end{pmatrix}\mapsto\begin{pmatrix}1\\0\end{pmatrix}$, $\begin{pmatrix}3\\1\end{pmatrix}\mapsto\begin{pmatrix}0\\1\end{pmatrix}$.

- [ ] **11.5** Létezik-e olyan $\varphi:\mathbb{R}^3\to\mathbb{R}^3$ lineáris leképezés, melyre $\varphi(1,1,1)=(1,2,3)$, $\varphi(1,0,1)=(1,-1,1)$, $\varphi(0,1,0)=(0,2,1)$ teljesül?

- [ ] **11.6** Határozzuk meg a mátrixokkal adott lineáris transzformációk sajátértékeit, sajátvektorait!
  - $\begin{pmatrix}2&1\\1&2\end{pmatrix}$, $\begin{pmatrix}3&4\\5&2\end{pmatrix}$, $\begin{pmatrix}-2&3\\-4&5\end{pmatrix}$, $\begin{pmatrix}3&1\\-1&1\end{pmatrix}$, $\begin{pmatrix}2&0&0\\0&2&0\\0&0&4\end{pmatrix}$, $\begin{pmatrix}2&-1&-1\\0&1&-2\\0&0&-1\end{pmatrix}$, $\begin{pmatrix}-1&3&-1\\-3&5&-1\\-3&3&1\end{pmatrix}$, $\begin{pmatrix}1&2&2\\2&1&2\\2&2&1\end{pmatrix}$

---

## 12. Euklideszi terek

- [ ] **12.1** Határozzuk meg $a=(-1,2,1)^T$ és $b=(2,2,-3)^T$ belső szorzatát és a vektorok normáját.

- [ ] **12.2** Határozzuk meg a bezárt szöget!
  - (a) $a^T=(2,1)$, $b^T=(1,3)$
  - (b) $a^T=(3,\sqrt3)$, $b^T=(2,0)$
  - (c) $a^T=(1,3,1)$, $b^T=(-4,-2,0)$
  - (d) $a^T=(-2,3,4)$, $b^T=(-6,-4,2)$
  - (e) $a^T=(1,-2,3)$, $b^T=(2,4,2)$
  - (f) $a^T=(1,1,0)$, $b^T=(1,1,-\sqrt2)$
  - (g) $a^T=(0,-1,0)$, $b^T=(1,-\sqrt3,0)$

- [ ] **12.3** Adjuk meg $\lambda$-t úgy, hogy $a\perp b$!
  - (a) $a^T=(1,1)$, $b^T=(-2,\lambda)$
  - (b) $a^T=(4,2,1)$, $b^T=(-4,-2,\lambda)$
  - (c) $a^T=(1,2,1,1)$, $b^T=(-4,-2,2,\lambda)$
  - (d) $a^T=(1,\lambda,\lambda)$, $b^T=(-3,-2,\lambda)$

- [ ] **12.4** Írjuk fel azokat a vektorokat, amelyek $\mathbb{R}^2$-ben ortogonálisak $x=(-1,2)^T$-re!

- [ ] **12.5** Írjuk fel azokat, amelyek $\mathbb{R}^3$-ban ortogonálisak $x=(-1,2,4)^T$-re!

- [ ] **12.6** Írjuk fel azokat, amelyek $\mathbb{R}^3$-ban ortogonálisak $x=(-2,3,4)^T$-re **és** $y=(1,-2,1)^T$-re is!

- [ ] **12.7** Adjuk meg azt a vektort, melynek iránya megegyezik $x=(-1,2,1)^T$ irányával, de normája 1.
