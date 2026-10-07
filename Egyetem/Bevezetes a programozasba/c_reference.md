# C Nyelvi Referencia

Gyors áttekintő és referencia a C programozási nyelvhez.

## Tartalomjegyzék

1. [Alapok](#alapok)
2. [Adattípusok](#adattípusok)
3. [Operátorok](#operátorok)
4. [Vezérlési szerkezetek](#vezérlési-szerkezetek)
5. [Tömbök és Stringek](#tömbök-és-stringek)
6. [Pointerek](#pointerek)
7. [Függvények](#függvények)
8. [Struktúrák és Unionok](#struktúrák-és-unionok)
9. [Dinamikus memóriakezelés](#dinamikus-memóriakezelés)
10. [Fájlkezelés](#fájlkezelés)
11. [Preprocesszor](#preprocesszor)
12. [Gyakori hibák](#gyakori-hibák)

---

## Alapok

### Minimális program

```c
#include <stdio.h>

int main(void) {
    printf("Hello, World!\n");
    return 0;
}
```

### Fordítás (compile)

```bash
gcc -Wall -Wextra -o program program.c
./program
```

- `-Wall -Wextra`: kapcsolja be a legtöbb figyelmeztetést, mindig érdemes használni
- `-o program`: az output futtatható fájl neve

### Kommentek

```c
// Egysoros komment
/* Többsoros
   komment */
```

---

## Adattípusok

| Típus | Méret (tipikus) | Leírás |
|---|---|---|
| `char` | 1 byte | egyetlen karakter |
| `int` | 4 byte | egész szám |
| `short` | 2 byte | rövid egész |
| `long` | 4/8 byte | hosszú egész |
| `long long` | 8 byte | nagyon hosszú egész |
| `float` | 4 byte | lebegőpontos szám (single precision) |
| `double` | 8 byte | lebegőpontos szám (double precision) |
| `_Bool` / `bool`* | 1 byte | logikai érték (C99+, `<stdbool.h>`) |
| `void` | - | "nincs típus", pl. függvény visszatérési értéknél |

\* `bool`, `true`, `false` a `<stdbool.h>` include-olásával érhető el.

### Előjel nélküli típusok

```c
unsigned int count = 10;
unsigned char byte_value = 255;
```

### Módosított típusok mérete garantáltan (C99+)

```c
#include <stdint.h>

int8_t   a;   // pontosan 8 bit, előjeles
uint8_t  b;   // pontosan 8 bit, előjel nélküli
int32_t  c;   // pontosan 32 bit
uint64_t d;   // pontosan 64 bit
```

### `sizeof` operátor

```c
printf("%zu\n", sizeof(int));    // pl. 4
printf("%zu\n", sizeof(double)); // pl. 8
```

### Típuskonverzió (casting)

```c
int total = 7;
int count = 2;
double average = (double)total / count;  // explicit cast, 3.5
```

---

## Operátorok

### Aritmetikai

```c
int a = 10, b = 3;
a + b;   // 13
a - b;   // 7
a * b;   // 30
a / b;   // 3  (egész osztás!)
a % b;   // 1  (maradék, modulo)
```

### Összehasonlító és logikai

```c
a == b;  a != b;  a < b;  a > b;  a <= b;  a >= b;
a && b;  a || b;  !a;
```

### Bitszintű (bitwise) operátorok

```c
a & b;   // bitwise AND
a | b;   // bitwise OR
a ^ b;   // bitwise XOR
~a;      // bitwise NOT
a << 2;  // bal shift
a >> 2;  // jobb shift
```

### Increment / decrement

```c
int i = 0;
i++;   // post-increment
++i;   // pre-increment
i--;   ///--i;
```

### Értékadó operátorok

```c
a += 5;  a -= 5;  a *= 5;  a /= 5;  a %= 5;
a &= 5;  a |= 5;  a ^= 5;  a <<= 1;  a >>= 1;
```

---

## Vezérlési szerkezetek

### if / else

```c
if (score >= 90) {
    printf("A\n");
} else if (score >= 80) {
    printf("B\n");
} else {
    printf("C\n");
}
```

### switch

```c
switch (grade) {
    case 'A':
        printf("Excellent\n");
        break;
    case 'B':
        printf("Good\n");
        break;
    default:
        printf("Unknown\n");
}
```

### Ciklusok

```c
// for
for (int i = 0; i < 10; i++) {
    printf("%d\n", i);
}

// while
int i = 0;
while (i < 10) {
    printf("%d\n", i);
    i++;
}

// do-while (legalább egyszer lefut)
int i = 0;
do {
    printf("%d\n", i);
    i++;
} while (i < 10);
```

### break / continue

```c
for (int i = 0; i < 10; i++) {
    if (i == 5) break;      // kilép a ciklusból
    if (i % 2 == 0) continue; // ugrás a következő iterációra
    printf("%d\n", i);
}
```

---

## Tömbök és Stringek

### Tömbök

```c
int numbers[5] = {1, 2, 3, 4, 5};
int matrix[3][3] = {{1,2,3}, {4,5,6}, {7,8,9}};

numbers[0];       // 1
int length = sizeof(numbers) / sizeof(numbers[0]); // elemszám
```

Fontos: a C-ben nincs beépített határellenőrzés (bounds checking) – a tömbön túli indexelés undefined behavior.

### Stringek

A C-ben a string egyszerűen egy `char` tömb, amit egy `\0` (null terminátor) zár le.

```c
char name[20] = "Alice";      // automatikusan lezárja \0-val
char greeting[] = "Hello";    // méret automatikusan meghatározva

#include <string.h>

strlen(name);            // hossz (null terminátor nélkül)
strcpy(dest, src);        // másolás
strcat(dest, src);        // összefűzés
strcmp(str1, str2);       // összehasonlítás, 0 ha egyenlő
strncpy(dest, src, n);    // biztonságosabb másolás, max n karakter
```

⚠️ `strcpy` és `strcat` nem ellenőrzik a buffer méretét – gyakori buffer overflow forrás. Érdemes `strncpy`/`snprintf` használata.

### Formázott string összeállítás

```c
char buffer[50];
snprintf(buffer, sizeof(buffer), "Value: %d", 42);
```

---

## Pointerek

```c
int value = 42;
int *ptr = &value;   // ptr a value címét tárolja

printf("%d\n", *ptr);    // dereferencing -> 42
printf("%p\n", (void*)ptr); // a cím kiírása

*ptr = 100;    // value most 100 lesz
```

### Pointer aritmetika

```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;      // tömb neve -> pointerré alakul (decay)

p[0];    // 10
*(p+1);  // 20 - ugyanaz, mint p[1]
p++;     // a következő elemre lép (típusmérettel arányosan)
```

### Dupla pointer

```c
int value = 5;
int *ptr = &value;
int **ptr_to_ptr = &ptr;

**ptr_to_ptr;   // 5
```

### NULL pointer

```c
int *ptr = NULL;

if (ptr != NULL) {
    // biztonságos dereferencing
}
```

### Function pointer

```c
int add(int a, int b) { return a + b; }

int (*operation)(int, int) = add;
int result = operation(3, 4);   // 7
```

### const pointerek

```c
const int *ptr1;        // amire mutat, nem módosítható
int * const ptr2 = &x;  // maga a pointer nem módosítható (kell inicializálni)
const int * const ptr3 = &x; // egyik sem módosítható
```

---

## Függvények

```c
// deklaráció (prototípus)
int add(int a, int b);

// definíció
int add(int a, int b) {
    return a + b;
}

// void visszatérési érték
void print_message(void) {
    printf("Hello\n");
}
```

### Paraméterátadás

C-ben minden **pass by value** – ha módosítani akarunk egy eredeti változót, pointert kell átadni:

```c
void increment(int *value) {
    (*value)++;
}

int x = 5;
increment(&x);   // x most 6
```

### Tömb átadása függvénynek

```c
void print_array(int arr[], int length) {
    for (int i = 0; i < length; i++) {
        printf("%d ", arr[i]);
    }
}
// a tömb valójában pointerként érkezik meg -> a méretet külön kell átadni
```

### Rekurzió

```c
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

### Változó paraméterszám (variadic)

```c
#include <stdarg.h>

int sum(int count, ...) {
    va_list args;
    va_start(args, count);
    int total = 0;
    for (int i = 0; i < count; i++) {
        total += va_arg(args, int);
    }
    va_end(args);
    return total;
}
```

---

## Struktúrák és Unionok

### struct

```c
struct Point {
    int x;
    int y;
};

struct Point p1 = {10, 20};
p1.x;   // 10

// typedef-fel egyszerűbb elérés
typedef struct {
    int x;
    int y;
} Point;

Point p2 = {5, 5};
```

### Struktúra pointer

```c
Point p = {1, 2};
Point *ptr = &p;

ptr->x;       // nyíl operátor (arrow), ugyanaz mint (*ptr).x
(*ptr).y;     // explicit dereferencing
```

### Beágyazott struktúrák

```c
typedef struct {
    Point origin;
    int width;
    int height;
} Rectangle;

Rectangle r = {{0, 0}, 100, 50};
r.origin.x;   // 0
```

### union

```c
union Value {
    int int_value;
    float float_value;
    char char_value;
};
// egyszerre csak egy mező érvényes, megosztott memóriaterület
```

### enum

```c
enum Color { RED, GREEN, BLUE };  // RED=0, GREEN=1, BLUE=2

enum Color current_color = GREEN;
```

---

## Dinamikus memóriakezelés

```c
#include <stdlib.h>

// malloc: nem inicializált memória foglalása
int *arr = malloc(5 * sizeof(int));

// calloc: nullázott memória foglalása
int *arr2 = calloc(5, sizeof(int));

// realloc: méret módosítása
arr = realloc(arr, 10 * sizeof(int));

// felszabadítás - MINDIG kötelező, ha malloc/calloc-ot használtunk
free(arr);
arr = NULL;   // jó gyakorlat, elkerüli a dangling pointert
```

### Gyakori hibák dinamikus memóriánál

- **Memory leak**: elfelejtett `free()` hívás
- **Dangling pointer**: felszabadított memóriára mutató pointer használata
- **Double free**: ugyanazt a memóriaterületet kétszer szabadítjuk fel
- **Buffer overflow**: a foglalt méreten túli írás/olvasás

---

## Fájlkezelés

```c
#include <stdio.h>

FILE *file = fopen("data.txt", "r");   // "r", "w", "a", "r+", stb.
if (file == NULL) {
    perror("Error opening file");
    return 1;
}

char line[100];
while (fgets(line, sizeof(line), file) != NULL) {
    printf("%s", line);
}

fclose(file);
```

### Írás fájlba

```c
FILE *file = fopen("output.txt", "w");
fprintf(file, "Value: %d\n", 42);
fclose(file);
```

---

## Preprocesszor

```c
#include <stdio.h>       // fájl beillesztése
#define MAX_SIZE 100      // makró konstans
#define SQUARE(x) ((x) * (x))   // makró függvény

#ifdef DEBUG
    printf("Debug mode\n");
#endif

#ifndef HEADER_H
#define HEADER_H
// header guard - duplikált include elleni védelem
#endif
```

---

## Gyakori hibák

| Hiba | Probléma | Megoldás |
|---|---|---|
| `if (x = 5)` | értékadás összehasonlítás helyett | `if (x == 5)` |
| Tömb indexelés határon túl | undefined behavior | ellenőrizd az indexet |
| `free()` elmaradása | memory leak | mindig `free()` a `malloc()` után |
| Uninitialized pointer dereferencing | crash / UB | inicializálj `NULL`-ra, ellenőrizz |
| Integer division | `5 / 2 == 2`, nem 2.5 | castold `double`-re |
| Format string mismatch | UB `printf`-nél | típusnak megfelelő format specifier |

### Gyakori format specifier-ek (printf/scanf)

| Specifier | Típus |
|---|---|
| `%d` | int |
| `%u` | unsigned int |
| `%f` | float/double |
| `%c` | char |
| `%s` | string (char*) |
| `%p` | pointer |
| `%x` | hexadecimális |
| `%zu` | size_t |

---

*Ez a referencia a leggyakrabban használt C nyelvi elemeket fedi le, nem teljeskörű nyelvi specifikáció.*
