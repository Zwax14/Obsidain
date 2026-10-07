# Java Nyelvi Referencia

Gyors áttekintő és referencia a Java programozási nyelvhez.

## Tartalomjegyzék

1. [Alapok](#alapok)
2. [Adattípusok](#adattípusok)
3. [Operátorok](#operátorok)
4. [Vezérlési szerkezetek](#vezérlési-szerkezetek)
5. [Tömbök](#tömbök)
6. [OOP alapok: Osztályok és Objektumok](#oop-alapok-osztályok-és-objektumok)
7. [Öröklődés (Inheritance)](#öröklődés-inheritance)
8. [Polimorfizmus](#polimorfizmus)
9. [Interface-ek és Abstract osztályok](#interface-ek-és-abstract-osztályok)
10. [Kivételkezelés (Exception Handling)](#kivételkezelés-exception-handling)
11. [Collections](#collections)
12. [Generics](#generics)
13. [Stringek](#stringek)
14. [Gyakori hibák](#gyakori-hibák)

---

## Alapok

### Minimális program

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### Fordítás és futtatás

```bash
javac Main.java   # fordítás -> Main.class (bytecode)
java Main          # futtatás JVM-en
```

A fájl neve **kötelezően** meg kell egyezzen a public class nevével (`Main.java` -> `public class Main`).

### Kommentek

```java
// Egysoros komment
/* Többsoros komment */
/** JavaDoc komment - dokumentáció generáláshoz */
```

---

## Adattípusok

### Primitív típusok

| Típus | Méret | Leírás |
|---|---|---|
| `byte` | 1 byte | -128 és 127 között |
| `short` | 2 byte | rövid egész |
| `int` | 4 byte | egész szám |
| `long` | 8 byte | hosszú egész (`L` suffix, pl. `100L`) |
| `float` | 4 byte | lebegőpontos (`f` suffix, pl. `3.14f`) |
| `double` | 8 byte | lebegőpontos, dupla pontosság |
| `char` | 2 byte | egyetlen Unicode karakter |
| `boolean` | 1 bit | `true` / `false` |

### Referencia típusok (wrapper osztályok)

```java
Integer boxedInt = 42;      // autoboxing
int primitiveInt = boxedInt; // unboxing

// wrapper osztályok: Integer, Long, Double, Float, Character, Boolean, Byte, Short
```

### Típuskonverzió (casting)

```java
int total = 7;
int count = 2;
double average = (double) total / count;  // explicit widening/narrowing cast

double d = 3.99;
int truncated = (int) d;   // 3, elveszik a törtrész
```

### Konstansok

```java
final int MAX_SIZE = 100;   // nem módosítható értékadás után
```

---

## Operátorok

### Aritmetikai

```java
int a = 10, b = 3;
a + b;   // 13
a - b;   // 7
a * b;   // 30
a / b;   // 3 (egész osztás, ha mindkét operandus int)
a % b;   // 1
```

### Összehasonlító és logikai

```java
a == b;  a != b;  a < b;  a > b;  a <= b;  a >= b;
a && b;  a || b;  !a;
```

⚠️ Objektumoknál a `==` a referenciát hasonlítja össze, nem a tartalmat! Tartalom összehasonlításhoz `.equals()` kell.

```java
String s1 = new String("test");
String s2 = new String("test");
s1 == s2;        // false - különböző objektumok
s1.equals(s2);   // true - tartalom egyezik
```

### Ternáris operátor

```java
int max = (a > b) ? a : b;
```

### instanceof

```java
if (obj instanceof String) {
    String str = (String) obj;
}
```

---

## Vezérlési szerkezetek

### if / else

```java
if (score >= 90) {
    System.out.println("A");
} else if (score >= 80) {
    System.out.println("B");
} else {
    System.out.println("C");
}
```

### switch (hagyományos és modern)

```java
// hagyományos
switch (grade) {
    case 'A':
        System.out.println("Excellent");
        break;
    default:
        System.out.println("Unknown");
}

// modern switch expression (Java 14+)
String result = switch (grade) {
    case 'A' -> "Excellent";
    case 'B' -> "Good";
    default -> "Unknown";
};
```

### Ciklusok

```java
// for
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}

// enhanced for (for-each)
int[] numbers = {1, 2, 3};
for (int number : numbers) {
    System.out.println(number);
}

// while
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}

// do-while
int i = 0;
do {
    System.out.println(i);
    i++;
} while (i < 10);
```

---

## Tömbök

```java
int[] numbers = {1, 2, 3, 4, 5};
int[] numbers2 = new int[5];         // alapértelmezett értékekkel (0)
int[][] matrix = new int[3][3];       // kétdimenziós tömb

numbers.length;    // 5 - tömb hossza (nem metódus!)

import java.util.Arrays;
Arrays.sort(numbers);
Arrays.toString(numbers);   // "[1, 2, 3, 4, 5]"
```

Java-ban a tömbök indexelése határellenőrzött – túlindexelés `ArrayIndexOutOfBoundsException`-t dob.

---

## OOP alapok: Osztályok és Objektumok

```java
public class Dog {
    // mezők (fields)
    private String name;
    private int age;

    // konstruktor
    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // metódus
    public void bark() {
        System.out.println(name + " says woof!");
    }

    // getter/setter
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

// használat
Dog myDog = new Dog("Rex", 3);
myDog.bark();
```

### Hozzáférés módosítók (access modifiers)

| Módosító | Elérhetőség |
|---|---|
| `private` | csak az osztályon belül |
| (default, nincs kulcsszó) | csomagon (package) belül |
| `protected` | csomagon belül + alosztályokban |
| `public` | mindenhonnan |

### static tagok

```java
public class Counter {
    static int count = 0;   // osztályszintű, minden példány osztozik rajta

    public Counter() {
        count++;
    }
}
```

---

## Öröklődés (Inheritance)

```java
public class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public void makeSound() {
        System.out.println("Some generic sound");
    }
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name);   // szülő konstruktor hívása
    }

    @Override
    public void makeSound() {
        System.out.println(name + " says Woof!");
    }
}
```

### super kulcsszó

```java
@Override
public void makeSound() {
    super.makeSound();   // szülő metódus meghívása
    System.out.println("plus a bark");
}
```

Java-ban **nincs többszörös öröklődés** osztályok között (csak interface-eknél lehet több forrásból örökölni).

---

## Polimorfizmus

```java
Animal myAnimal = new Dog("Rex");  // szülő típusú referencia, gyerek objektum
myAnimal.makeSound();               // a Dog implementációja fut le (dynamic dispatch)

// Method overloading (statikus polimorfizmus)
public int add(int a, int b) { return a + b; }
public double add(double a, double b) { return a + b; }
public int add(int a, int b, int c) { return a + b + c; }
```

---

## Interface-ek és Abstract osztályok

### Interface

```java
public interface Movable {
    void move();               // implicit public abstract
    default void stop() {      // default metódus (Java 8+)
        System.out.println("Stopped");
    }
}

public class Car implements Movable {
    @Override
    public void move() {
        System.out.println("Car is moving");
    }
}
```

### Abstract osztály

```java
public abstract class Shape {
    abstract double area();     // nincs implementáció, kötelező override-olni

    void describe() {           // van implementáció, örökölhető
        System.out.println("This shape has area: " + area());
    }
}

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}
```

**Interface vs Abstract class:** egy osztály több interface-t is implementálhat, de csak egy osztályból örökölhet (akár abstract, akár nem).

---

## Kivételkezelés (Exception Handling)

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("This always runs");
}
```

### Checked vs Unchecked kivételek

- **Checked** (pl. `IOException`): fordítási időben kötelező kezelni (`try-catch` vagy `throws`)
- **Unchecked** (`RuntimeException` leszármazottak, pl. `NullPointerException`): nem kötelező kezelni

### Saját kivétel

```java
public class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public void setAge(int age) throws InvalidAgeException {
    if (age < 0) {
        throw new InvalidAgeException("Age cannot be negative");
    }
}
```

### try-with-resources

```java
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    String line = reader.readLine();
} catch (IOException e) {
    e.printStackTrace();
}
// a resource automatikusan lezáródik
```

---

## Collections

```java
import java.util.*;

// List - rendezett, duplikátumok engedélyezettek
List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.get(0);       // "Alice"
names.size();        // 2

// Set - egyediek, nincs sorrend garantálva (HashSet)
Set<String> uniqueNames = new HashSet<>();
uniqueNames.add("Alice");

// Map - kulcs-érték párok
Map<String, Integer> ages = new HashMap<>();
ages.put("Alice", 30);
ages.get("Alice");    // 30
ages.containsKey("Alice");  // true

// iterálás
for (String name : names) {
    System.out.println(name);
}

for (Map.Entry<String, Integer> entry : ages.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

### Gyakori Collection implementációk

| Interface | Implementáció | Jellemző |
|---|---|---|
| `List` | `ArrayList` | dinamikus tömb, gyors elérés |
| `List` | `LinkedList` | láncolt lista, gyors beszúrás/törlés |
| `Set` | `HashSet` | nincs sorrend, gyors keresés |
| `Set` | `TreeSet` | rendezett |
| `Map` | `HashMap` | nincs sorrend |
| `Map` | `TreeMap` | kulcs szerint rendezett |

---

## Generics

```java
public class Box<T> {
    private T content;

    public void set(T content) {
        this.content = content;
    }

    public T get() {
        return content;
    }
}

Box<String> stringBox = new Box<>();
stringBox.set("Hello");

// Generic metódus
public static <T> T getFirst(List<T> list) {
    return list.get(0);
}

// Bounded type parameter
public static <T extends Number> double sum(List<T> list) {
    double total = 0;
    for (T item : list) {
        total += item.doubleValue();
    }
    return total;
}
```

---

## Stringek

```java
String greeting = "Hello";
String name = "World";

greeting.length();               // 5
greeting.charAt(0);              // 'H'
greeting.toUpperCase();          // "HELLO"
greeting.toLowerCase();          // "hello"
greeting + " " + name;           // "Hello World"
greeting.equals("Hello");        // true (tartalom szerint)
greeting.substring(1, 3);        // "el"
greeting.contains("ell");        // true
greeting.replace("l", "L");      // "HeLLo"
greeting.split(",");             // tömb feldarabolva ,-nál
String.valueOf(42);              // "42"
```

### StringBuilder (hatékony összefűzéshez)

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" ");
sb.append("World");
String result = sb.toString();   // "Hello World"
```

A `String` immutable Java-ban – minden módosítás új objektumot hoz létre. Ciklusban történő sok összefűzésnél mindig `StringBuilder`-t használj.

---

## Gyakori hibák

| Hiba | Probléma | Megoldás |
|---|---|---|
| `==` objektumoknál | referencia összehasonlítás | `.equals()` használata |
| `NullPointerException` | null objektumon metódushívás | null ellenőrzés, `Optional` |
| `ArrayIndexOutOfBoundsException` | tömb határon túli indexelés | ellenőrizd `array.length`-et |
| `ClassCastException` | hibás típuskonverzió | `instanceof` ellenőrzés előtte |
| `ConcurrentModificationException` | collection módosítása iterálás közben | `Iterator.remove()` vagy `CopyOnWriteArrayList` |
| String összefűzés ciklusban | lassú, sok objektum jön létre | `StringBuilder` |

---

*Ez a referencia a leggyakrabban használt Java nyelvi elemeket fedi le, nem teljeskörű nyelvi specifikáció.*
