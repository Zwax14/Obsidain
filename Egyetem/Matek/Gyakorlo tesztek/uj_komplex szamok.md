---
title: Komplex számok – ZH gyakorlás (füzet szerint)
tags:
  - discrete-math
  - complex-numbers
  - exam-prep
type: study-note
status: to-review
---

# 🧮 Komplex számok – a füzet 1–8. feladata

> [!abstract] Felépítés
> A feladatok sorszáma megegyezik a füzet bal oldali számozásával. Mindegyiknél csak az van, **hogyan kell megcsinálni**.
> A végeredmény a lenyíló `Megoldás` dobozban van, előbb próbáld meg magad.

> [!info] Alapok
> $z=a+bi$, $a$ = valós rész, $b$ = képzetes rész, $i^2=-1$.
> Konjugált: $\overline{z}=a-bi$. Abszolút érték: $\lvert z\rvert=\sqrt{a^2+b^2}$.

---

# 1️⃣ $n$-edik gyökök (trigonometrikus alak)

> [!question] Feladat (füzet)
> Adja meg a $z=8\left(\cos\frac{2\pi}3+i\sin\frac{2\pi}3\right)$ szám 3. gyökeit ($n=3$).

**Képlet:** $z=r(\cos\varphi+i\sin\varphi)$ $n$-edik gyökei:
$$\sqrt[n]{r}\left(\cos\frac{\varphi+2k\pi}{n}+i\sin\frac{\varphi+2k\pi}{n}\right),\qquad k=0,1,\dots,n-1$$

**1. lépés – hossz** (mind a 3 gyöknél ugyanaz):
$$\sqrt[3]8=2$$

**2. lépés – szögek:** írd be $\varphi=\frac{2\pi}3$, $n=3$ és sorban $k=0,1,2$:

| $k$ | Szög | Egyszerűsítve |
| --- | --- | --- |
| $0$ | $\dfrac{\frac{2\pi}3+2\cdot0\cdot\pi}{3}$ | $\dfrac{2\pi}9$ |
| $1$ | $\dfrac{\frac{2\pi}3+2\cdot1\cdot\pi}{3}=\dfrac{\frac{8\pi}3}{3}$ | $\dfrac{8\pi}9$ |
| $2$ | $\dfrac{\frac{2\pi}3+2\cdot2\cdot\pi}{3}=\dfrac{\frac{14\pi}3}{3}$ | $\dfrac{14\pi}9$ |

(Az összetett törtet úgy egyszerűsítsd, hogy a számláló törtjét osztod 3-mal: $\frac{8\pi}{3}:3=\frac{8\pi}{9}$.)

**Ellenőrzés:** a szögek egymástól $\frac{2\pi}n=\frac{6\pi}9$ távolságra vannak ($2,8,14$), és a gyök $3$-szorosa visszaadja $\varphi$-t: $3\cdot\frac{2\pi}9=\frac{2\pi}3$ ✅

> [!success]- ✅ Megoldás
> $$2\left(\cos\tfrac{2\pi}9+i\sin\tfrac{2\pi}9\right),\quad2\left(\cos\tfrac{8\pi}9+i\sin\tfrac{8\pi}9\right),\quad2\left(\cos\tfrac{14\pi}9+i\sin\tfrac{14\pi}9\right)$$

> [!warning] Gyakori hibák
> - $k$ csak $0$-tól $n-1$-ig megy, $n$ db gyök van.
> - A $2k\pi$ **a számlálóban** van, az osztás ($n$-nel) az egészre vonatkozik.
> - A hossz $\sqrt[n]{r}$, **nem** $r$.

---

# 2️⃣ Szorzás algebrai alakban

> [!question] Feladat (füzet)
> Ha $z=-10+6i$ és $v=-10-i$, mennyi $z\cdot v$ valós és képzetes része?

**Képlet:** a füzetben megjelölt betűkkel $z=a+bi$ ($a=-10,\ b=6$), $v=c+di$ ($c=-10,\ d=-1$):
$$(a+bi)(c+di)=(ac-bd)+(ad+bc)i$$

**Valós rész:** $ac-bd=(-10)(-10)-6\cdot(-1)=100+6=106$

**Képzetes rész:** $ad+bc=(-10)(-1)+6\cdot(-10)=10-60=-50$

**Ellenőrzés mindent mindennel:**
$(-10+6i)(-10-i)=100+10i-60i-6i^2=100-50i+6=106-50i$ ✅

> [!success]- ✅ Megoldás
> $z\cdot v=106-50i$, valós rész: **106**, képzetes rész: **−50**

> [!warning] Gyakori hiba
> A képzetes **rész** a $-50$ (az $i$ együtthatója), nem a $-50i$. A $bd$ tag előjele: $-bd=-6\cdot(-1)=+6$.

---

# 3️⃣ Összeadás

> [!question] Feladat (füzet)
> Mivel egyenlő $z+v$, ha $z=10-8i$ és $v=7-5i$?

Valós rész a valóssal, képzetes a képzetessel:
$$(10+7)+(-8i-5i)=17-13i$$

> [!success]- ✅ Megoldás
> $z+v=\mathbf{17-13i}$

> [!warning] Gyakori hiba
> Kivonásnál ($z-v$) a mínusz **mindkét** tagra vonatkozik: $(10-7)+(-8+5)i=3-3i$.

---

# 4️⃣ Irányszög (argumentum)

> [!question] Feladat (füzet)
> Adja meg a $z=-1-\sqrt3\,i$ szám trigonometrikus alakját (irányszögét).

**1. lépés – hossz:**
$$\lvert z\rvert=\sqrt{(-1)^2+(-\sqrt3)^2}=\sqrt{1+3}=2$$

**2. lépés – segédszög** (a füzetben koszinusszal: $\cos\alpha=\frac{\lvert a\rvert}{\lvert z\rvert}$):
$$\cos\alpha=\frac{1}{2}\;\Rightarrow\;\alpha=60^\circ=\frac\pi3$$
(Tangenssel ugyanez: $\tan\alpha=\frac{\lvert b\rvert}{\lvert a\rvert}=\sqrt3\Rightarrow\alpha=\frac\pi3$.)

**3. lépés – melyik negyed?** A füzetben a síkra felírt előjelek: $(a,b)$

| Negyed | $a$ | $b$ | Irányszög |
| --- | --- | --- | --- |
| I. | $+$ | $+$ | $\alpha$ |
| II. | $-$ | $+$ | $\pi-\alpha$ |
| III. | $-$ | $-$ | $\pi+\alpha$ |
| IV. | $+$ | $-$ | $2\pi-\alpha$ |

Itt $a=-1<0$, $b=-\sqrt3<0$ → **III. negyed**:
$$\varphi=\pi+\frac\pi3=\frac{3\pi}3+\frac\pi3=\frac{4\pi}3$$

> [!success]- ✅ Megoldás
> $\varphi=\dfrac{4\pi}3$, vagyis $z=2\left(\cos\frac{4\pi}3+i\sin\frac{4\pi}3\right)$

> [!warning] Gyakori hiba
> Ne állj meg a segédszögnél ($\frac\pi3$)! **Mindig javítsd a negyed szerint.**

---

# 5️⃣ Abszolút érték

> [!question] Feladat (füzet)
> Adja meg a $z=2-2i$ abszolút értékét.

$$\lvert z\rvert=\sqrt{2^2+(-2)^2}=\sqrt{4+4}=\sqrt8=\sqrt{2^2\cdot2}=2\sqrt2$$

> [!success]- ✅ Megoldás
> $\lvert z\rvert=\mathbf{2\sqrt2}$

> [!warning] Gyakori hiba
> Négyzetösszeg, aztán gyök. A mínusz jel nem számít, mert négyzetre emelsz. Nem $2+2$ és nem $2-2$.

---

# 6️⃣ Konjugált és szorzás

> [!question] Feladat (füzet)
> Mivel egyenlő $\overline{z}\cdot v$, ha $z=-4-8i$ és $v=10+3i$?

**1. lépés – konjugált** (csak a képzetes rész előjele fordul meg):
$$\overline{z}=-4+8i$$

**2. lépés – szorzás:** $a=-4,\ b=8,\ c=10,\ d=3$
- valós: $ac-bd=-40-24=-64$
- képzetes: $ad+bc=-12+80=68$

**Ellenőrzés hosszakkal:** $\lvert\overline{z}\rvert^2\cdot\lvert v\rvert^2=80\cdot109=8720$ és $(-64)^2+68^2=4096+4624=8720$ ✅

> [!success]- ✅ Megoldás
> $\overline{z}\cdot v=\mathbf{-64+68i}$

> [!warning] Gyakori hiba
> A konjugált **$z$-é**, nem $v$-é. És $bd=8\cdot3=24$ **kivonódik** ($-40-24$).

---

# 7️⃣ Osztás

> [!question] Feladat (füzet)
> Mivel egyenlő $\dfrac zv$, ha $z=9+i$ és $v=4-6i$?

**Cél:** a nevezőből el kell tüntetni az $i$-t, ezért **bővítünk a nevező konjugáltjával** ($\overline{v}=4+6i$):
$$\frac{9+i}{4-6i}\cdot\frac{4+6i}{4+6i}$$

**1. lépés – számláló:**
$$(9+i)(4+6i)=36+54i+4i+6i^2=(36-6)+(54+4)i=30+58i$$

**2. lépés – nevező:** $(a+bi)(a-bi)=a^2+b^2$ (a füzetben $4^2-(6i)^2=16+36$):
$$4^2+6^2=16+36=52$$

**3. lépés – osztás és egyszerűsítés** (mindkét tagot $2$-vel):
$$\frac{30+58i}{52}=\frac{30}{52}+\frac{58}{52}i=\frac{15}{26}+\frac{29}{26}i$$

**Ellenőrzés visszaszorzással:** $\left(\frac{15}{26}+\frac{29}{26}i\right)(4-6i)=\frac{60+174}{26}+\frac{-90+116}{26}i=\frac{234}{26}+\frac{26}{26}i=9+i$ ✅

> [!success]- ✅ Megoldás
> $\dfrac zv=\dfrac{15}{26}+\dfrac{29}{26}i$

> [!warning] Gyakori hibák
> - A **nevező** konjugáltjával bővíts.
> - A nevező $a^2+b^2$ (itt $52$), nem $a^2-b^2$. Az $i^2=-1$ miatt lesz belőle $+$.
> - A végén egyszerűsíts: $\frac{30}{52}=\frac{15}{26}$.

---

# 8️⃣ Szorzás trigonometrikus alakban

> [!question] Feladat (füzet)
> Mivel egyenlő $z=z_1\cdot z_2$, ha
> $z_1=2\left(\cos\frac{7\pi}3+i\sin\frac{7\pi}3\right)$ és $z_2=3\left(\cos\frac\pi2+i\sin\frac\pi2\right)$?
>
> (A füzetben a szögek $\frac{7\pi}3$ és $\frac\pi2$ látszanak, a közös nevező a levezetésben $6$.)

**Szabály:** a **hosszok szorzódnak**, a **szögek összeadódnak**.

**1. lépés – hossz:** $2\cdot3=6$

**2. lépés – szög, közös nevezőre ($6$) hozva:**
$$\frac{7\pi}3+\frac\pi2=\frac{14\pi}6+\frac{3\pi}6=\frac{17\pi}6$$

**3. lépés – redukálás $0$ és $2\pi$ közé.** $2\pi=\frac{12\pi}6$, ezt vonjuk ki:
$$\frac{17\pi}6-\frac{12\pi}6=\frac{5\pi}6$$

(A füzetben fokban is ellenőrizted: $\frac{17\cdot180^\circ}{6}=510^\circ$, és $510^\circ-360^\circ=150^\circ=\frac{5\pi}6$ ✅)

> [!success]- ✅ Megoldás
> $z=\mathbf{6\left(\cos\dfrac{5\pi}6+i\sin\dfrac{5\pi}6\right)}$

> [!warning] Gyakori hiba
> A szögeket **összeadod**, nem szorzod. Ha $2\pi$-nél nagyobb, mindig vonj ki $2\pi=\frac{12\pi}6$-t (a közös nevezőtől függően $\frac{8\pi}4$, $\frac{6\pi}3$ stb.).

---

# 📌 Csalólap

| Téma | Amit tudni kell |
| --- | --- |
| $i^2$ | $-1$ |
| Konjugált | $\overline{a+bi}=a-bi$ |
| Abszolút érték | $\sqrt{a^2+b^2}$ |
| Összeadás | valós + valós, képzetes + képzetes |
| Szorzás | $(ac-bd)+(ad+bc)i$ |
| Osztás | bővíts $\overline{v}$-vel, a nevező $c^2+d^2$ |
| Irányszög | segédszög ($\cos\alpha=\frac{\lvert a\rvert}{\lvert z\rvert}$ vagy $\tan\alpha=\frac{\lvert b\rvert}{\lvert a\rvert}$), aztán negyed szerinti javítás |
| Trig. szorzás | hossz szorzás, szög összeadás ($>2\pi$ → vonj ki $2\pi$-t) |
| $n$-edik gyök | hossz: $\sqrt[n]r$; szög: $\frac{\varphi+2k\pi}n$, $k=0,\dots,n-1$ |
