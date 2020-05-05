<!--

author:   Sebastian Zug & André Dietrich
email:    Sebastian.Zug@informatik.tu-freiberg.de & andre.dietrich@informatik.tu-freiberg.de
version:  0.0.1
language: de
narrator: Deutsch Female

import: https://raw.githubusercontent.com/liaScript/rextester_template/master/README.md

-->

# Softwareentwicklung - 9 - OOP Konzepte

**TU Bergakademie Freiberg - Sommersemester 2020**

Link auf die aktuelle Vorlesung im Versionsmanagementsystem GitHub

[https://github.com/SebastianZug/CsharpCourse/blob/SoSe2020/07_CsharpElementeIII.md](https://github.com/SebastianZug/CsharpCourse/blob/SoSe2020/09_OOP_Konzepte.md)

Die interaktive Form ist unter diese Link zu finden ->
[LiaScript Vorlesung 09](https://liascript.github.io/course/?https://raw.githubusercontent.com/SebastianZug/CsharpCourse/SoSe2020/09_OOP_Konzepte.md#1)

---------------------------------------------------------------------

## 7 Fragen in 7 Minuten

**1.** Sie sind ein Entwickler in einem Videospielkonzern und sollen einen neuen NPC vom Type Goblin erstellen. Die Funktionen eines NPC's sind bereits vorhanden und müssen lediglich vererbt werden mit Ihren spezifischen Änderungen. Was für ein Konzept würden Sie verwenden?

[(X)] Class
[( )] Struct  

**2.** Eine Kindergarten möchte eine Lernapp entwickeln lassen, um den Kindern das Zuordnen von Tieren zu ihren charakteristischen Geräuchen zu erleichtern.  
Sie wurden Beauftragt diese App für den Kindergarten zu schreiben.  
Ein Tier hat folgende Eigenschaften:  
  1. Name  
  2. Geräusch  
Welches Konzept würde sich für das Umsetzen der App am Besten anbieten?

[( )] Class
[(X)] Sturct

**3.** Auf den Typ oder Member kann von jedem Code in der gleichen Assembly oder einer anderen Assembly, die darauf verweist, zugegriffen werden.

[( )] private 
[( )] internal 
[(X)] public  

**4.** Was wird für "b" auf die Konsole ausgegeben?

```csharp
struct Location { 
  public int x, y;
  public Location(int x, int y) 
    { 
      this.x = x; 
      this.y = y; 
    } 
  }
LocationA = new Location(20, 20); 
LocationB = LocationA;  
LocationA.x = 100;
System.Console.WriteLine(LocationB.x); 
```

[(X)] 20
[( )] 100

**5.** Wie viele Kekse sind in "KookieJar2" drin?

```csharp
class CookieJar {
  public int schoki, vanille;
  public CookieJar(int schokiInput, int vanilleInput)
    {
    this.schoki = schokiInput;
    this.vanille = vanilleInput;
    }
  }
CookieJar1 = new CookieJar(50, 74);
CookieJar2 = CookieJar1;
CookieJar1.vanille = 30;
System.Console.WriteLine(CookieJar2.vanille);
```

[( )] 74 Cookies
[(X)] 30 Cookies

**6. Jetzt sind Sie dran ...**

**7. Jetzt sind Sie dran ...**



## Structs

                                         {{0-1}}
*******************************************************************************

Ein struct-Typ ist ein ein Werttyp, der in der Regel verwendet wird, um eine
Gruppe verwandter Variablen zusammenzufassen. Beispiele dafür können sein:

* kartesische Koordinaten (x, y, z)
* Merkmale eines Produktes (Größe, Name, Preis)
* Charakteristik einer Datei (Speicherort, Name, Größe, Rechteinformationen)

Ausgangspunkt für die weiteren Überlegungen ist die Konfiguration von `structs`
in C.

```c         struct.c
#include <stdio.h>
#include <stdlib.h>

typedef struct RectangleStructure{
    double length;
    double width;
    double area;
} Rectangle;

int main(int argc, char const *argv[])
{
    Rectangle rect1, rect2, rect3;
    // Initialisierung:
    rect1.length = 2.5; rect1.width = 5;
    rect2.length = 4; rect2.width = 8;
    rect3.length = 1.5; rect3.width = 2.1;
    // Berechnung:
    rect1.area = rect1.length * rect1.width;
    rect2.area = rect2.length * rect2.width;
    rect3.area = rect3.length * rect3.width;
    // Ausgabe:
    printf("Rectangle 1 has an area of %5.1f\n",rect1.area);
    printf("Rectangle 2 has an area of %5.1f\n",rect2.area);
    printf("Rectangle 3 has an area of %5.1f\n",rect3.area);
    return 0;
}
```
@Rextester.C_clang

**Was fehlt uns?**

*******************************************************************************

                                       {{1-2}}
*******************************************************************************

Richtig, ein Set zugehöriger Funktionen!

```c    structAndFunctions.c
#include <stdio.h>
#include <stdlib.h>

typedef struct RectangleStructure{
    double length;
    double width;
    double area;
} Rectangle;

void initializeRectangle(Rectangle *actualRect, double length,
                         double width){
 actualRect->length = length;
 actualRect->width = width;
}

void computeArea(Rectangle *actualRect){
 actualRect->area = actualRect->length * actualRect->width;
}

void printRectangleArea(Rectangle *actualRect){
 printf("The Rectangle has an area of %f\n",actualRect->area);
}

int main(int argc, char const *argv[])
{
    Rectangle rect1;
    // Initialisierung:
    initializeRectangle(&rect1, 30, 40);
    computeArea(&rect1);
    printRectangleArea(&rect1);
    return 0;
}
```
@Rextester.C_clang

C sieht keine Möglichkeit vor Funktion und Daten "dichter" zusammenzubringen,
wenn man von Funktionspointern im `struct` absieht.

*******************************************************************************

                                 {{2-3}}
*******************************************************************************

Und in C#? C# erweitert das Konzept in Richtung der Konzepte der objektorientierten
Programmierung und integriert Operationen über den Daten in das `struct`-Konzept.
Dabei können sie folgende Elemente umfassen:

* Felder und Konstanten
* Methoden
* Konstruktoren und Destruktoren
* Eigenschaften(Properties)
* Indexer
* Events
* überladene Operatoren
* geschachtelte Typen

Konzentrieren wir uns zunächst auf die Felder und die Methoden. Wie sieht
eine entsprechende Definition des Bauplanes aller Instanzen von `Animal` aus?

```csharp        Struct.cs
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;               // Felder / Konstanten
    public string sound;              //

    public void MakeNoise() {         // Methode
    	Console.WriteLine($"{name} makes {sound}");
    }
  }
```

Sowohl die ganze Struktur als auch die einzelnen Felder sind mit entsprechenden Sichtbarkeitsattributen versehen. Hier wurde explizit `public` vorgesehen, damit ist Animal aber auch alle Elemente uneingeschränkt "sichbar". In der Regel ist das aber nicht gewünscht.

Weitere Modifikationen sind zum Beispiel über das Schlüsselwort `readonly` möglich, dass alle Felder als schreibgeschützt markiert.

Wie legen wir nun ein Objekt entsprechend der Spezifikation an? Wir haben mit dem `struct` einen neuen Typ definiert. Der Aufruf knnn (!) anhand des Formats `<datentyp> Variablennahme` erfolgen.

```csharp        FirstStructExample
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;               // Felder / Konstanten
    public string sound;              //

    public void MakeNoise() {         // Methode
    	Console.WriteLine($"{name} makes {sound}");
    }
  }

  public class Program
  {
    public static void Main(string[] args){
      Animal kitty;
      kitty.name = "Kitty";
      kitty.sound = "Miau";
      kitty.MakeNoise();
    }
  }
}
```
@Rextester.eval(@CSharp)

Warum nutzen einige Beispiele das Schlüsselwort `this`?  Es wird verwendet, um auf die aktuelle Instanz der Klasse zu verweisen. Obiges Beispiel kann also auch wie folgt geschrieben werden.

```csharp        FirstStructThisExample.cs
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;
    public string sound;

    public void MakeNoise() {
    	Console.WriteLine($"{this.name} makes {this.sound}");
    }

    public  void setName(string name) {
      this.name = name;
      this.MakeNoise();
    }
  }

  public class Program
  {
    public static void Main(string[] args){
      Animal kitty;
      kitty.name = "Kitty";
      kitty.sound = "Miau";
      kitty.MakeNoise();
      kitty.setName("Tom");
    }
  }
}
```
@Rextester.eval(@CSharp)


*******************************************************************************

### Konstruktoren

                                 {{0-1}}
*******************************************************************************

```csharp
Animal kitty;
kitty.name = ... ;
kitty.sound = ...;
...
kitty.state = State.Hungry;

Animal wally;
wally.name = ... ;
wally.sound = ...;
...
wally.state = State.Sleeps;
```

Haben wir auch wirklich alle initialen Variablen gesetzt? Das Vorgehen
scheint doch sehr unübersichtlich und fehleranfällig! Es wird etwas strukturierter, wenn wir die sogenannte Object Initialization Syntax aus C# 3.0 nutzen. Der Compiler generiert den zugehörigen Code für die spezifische Initialisierung.

```csharp                                      OIS.cs
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;
    public string sound;

    public void MakeNoise() {
    	Console.WriteLine("{0} makes {1}", name, sound);
    }
  }

  public class Program
  {
    public static void Main(string[] args){
      Animal cat = new Animal{
        name = "Kitty",
        sound = "Miau"
      };
      cat.MakeNoise();
    }
  }
}
```
@Rextester.eval(@CSharp)


*******************************************************************************

                                 {{1-2}}
*******************************************************************************

Konstruktoren sind spezielle Methoden für die Initialisierung eines Objektes. In
`structs` dürfen allerdings (im Unterschied zu Klassen) keine parameterlosen
Methoden sein. Der Compiler erzeugt diese automatisch, die Methode beschreibt
alle Felder mit den datentypspezifischen Nullwerten.

> *Anmerkung 1:* Konstruktoren sind Methoden und folglich steht das gesamte Spektrum
> der Variabilität bei deren Definition zur Verfügung (Überladen, vordefinierte
> Variablen, Parameterlisten, usw.)


> *Anmerkung 2:* Um einen Konstruktor für die Initialisierung zu nutzen
> braucht es einen erweiterten Aufruf.

Und wie erfolgt der Aufruf des Konstruktors, einer Funktion, die auf einer Datenstruktur wirkt, die es noch gar nicht gibt? Das Schlüsselwort `new` übernimmt diese Aufgabe für uns.

```csharp
<Type> <Variablenname> = new <Konstruktorsignatur>;
```

```csharp
public struct Animal
{
  public string name;
  public string sound;

  public void Animal(string name, string sound = "Miau") {
    Console.WriteLine($"{this.name} makes {this.sound}");
  }
}
```

| Konstruktor                                  | Aufruf                                      |
| -------------------------------------------- | ------------------------------------------- |
| Aufruf des (impliziten) Standardkonstruktors | `Animal kitty = new Animal()`               |
| `public Animal(name, sound)`                 | `Animal kitty = new Animal("kitty","Miau")` |
| `public Animal(name, sound = "Miau")`        | `Animal kitty = new Animal("kitty")`        |


<!-- --{{1}}-- Idee des Beispiels:
       + Deklaration eines parameterlosen Konstruktors
       + Deklaration eines Konstruktors mit einzelnem Parameter
       + Überladen des Konstruktors
-->
```csharp                                      Constructors
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;
    public string sound;

    public void MakeNoise() {
    	Console.WriteLine("{0} makes {1}", name, sound);
    }
  }

  public class Program
  {
    public static void Main(string[] args){
      Animal cat = new Animal();
      cat.name = "Kitty";
      cat.sound = "Miau";
      cat.MakeNoise();
      Animal cat = new Animal{name = "Wally"};
      cat.name = "Kitty";
      cat.sound = "Miau";
      cat.MakeNoise();
    }
  }
}
```
@Rextester.eval(@CSharp)

*******************************************************************************

### Initialisierung von Struct-Arrays

Häufig werden Instanzen gleicher Typen in Sammlungen erfasst. Dies kann zum Beispiel ein Array mit allen Tieren unseres virtuellen Bauernhofes sein. Wie werden diese dann initialisiert?

Das folgende Beispiel nutzt die Object Initialization Syntax für die individuellen Instanzen.

```csharp                                      Constructors
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;
    public string sound;

    public void MakeNoise() {
    	Console.WriteLine("{0} makes {1}", name, sound);
    }
  }

  public class Program
  {
    public static void Main(string[] args){
       Animal[] myAnimals = new Animal[]{
        new Animal{ name = "Kitty", sound = "Miau"},
        new Animal{ name = "Wally", sound = "Wuff"},
        new Animal{ name = "Berta", sound = "Muuuh"}
      };
      foreach(Animal animal in myAnimals){
        animal.MakeNoise();
      }
    }
  }
}
```
@Rextester.eval(@CSharp)

Versuchen Sie das Beispiel um einen Konstruktor und einen zugehörigen Aufruf für die Initalisierung zu ergänzen.

### Embedded Structs

Eine innerhalb einer Struktur (oder Klasse) definierte Struktur (oder Klasse)
wird als geschachtelter Typ bezeichnet.

Unabhängig davon, ob der äußere Typ eine Klasse oder eine Struktur ist, lautet
die Standardeinstellung von geschachtelten Typen private. Sie sind nur über
ihren enthaltenden Typ zugänglich. Es können aber auch weitere
Zugriffsmodifizierer angeben werden.

```csharp                   NestedStructs
public struct Container
{
    public struct Nested
    {
        private Container parent;

        public Nested()
        {
        }
        public Nested(Container parent)
        {
            this.parent = parent;
        }
    }
}

// Aufruf mit
Container.Nested nest = new Container.Nested();
```

zum Beispiel vgl. [C# Programmierhandbuch](https://docs.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/nested-types)

### Strukturtypvariablen als Verweis

ToDo

### Boxing and Unboxing

ToDo


### Sichtbarkeitsattribute

Strukturen und Klassen abstrahieren Daten und verbergen konkrete Realisierungen von
Programmteilen. Um zu steuern, welche Elemente eines Programms aus welchem
Kontext heraus sichtbar sind, werden diese mit Attributen versehen.

                                 {{0 - 1}}
*******************************************************************************


**Sichtbarkeit der Stuktur**

Strukturen, die innerhalb eines _Namespace_ (mit anderen Worten, die nicht in anderen Klassen oder Strukturen geschachtelt sind) direkt deklariert werden, können entweder `private, ``public` oder `internal` sein.

| Bezeichner | Konsequenz                                            |
| ---------- | ----------------------------------------------------- |
| `public`     | Keine Einschränkungen für den Zugriff                 |
| `internal`   | Der Zugriff ist auf die aktuelle _Assembly_ beschränkt. |
| `private`    | Der Zugriff kann nur aus dem Code der gleichen Struktur erfolgen.                                                      |

Wenn kein Modifizierer angegeben ist, wird standardmäßig `internal` verwendet.

<!--
style="width: 100%; max-width: 560px; display: block; margin-left: auto; margin-right: auto;"
-->
```ascii

            Programm.cs                       Farmland.cs
            +-----------------------+         +-------------------------+
            | class Program{        |    .->  | internal struct Animal{ |
            |   public void Main(){ |    |    |    ...                  |
            |      Animal Kitty;    | ---.    | }                       |
            |      ...              |         |                         |
            |      Farm Bullerbue;  | ----->  | public struct Farm{     |
            |   }                   |         |    ...                  |
            |}                      |         | }                       |
            +-----------------------+         +-------------------------+      .

Schritt 1                                     mcs -target:library Farmland.cs
Schritt 2   mcs -reference:Farmland.dll Programm.cs
```

Das struct "Animal" soll in einem anderen Assembly nicht aufrufbar sein. Wir
wollen die Implementierung kapseln und verbergen. Folglich generiert der entsprechende
Aufruf einen Compiler-Fehler. Zugehörige Dateien sind unter [GitHub](https://github.com/liaScript/CsharpCourse/tree/master/code/05_FunktionenStrukturen/DifferentAssemblies) zu finden.

Variante 1: Compilieren von Farmland und Programm in ein Assambly

```bash
mcs Programm.cs Farmland.cs
My name ist Kitty.
```


Variante 2: Compilieren von Farmland als externe Bibliothek

```bash
mcs -target:library Farmland.cs
mcs -reference:Farmland.dll Programm.cs
Programm.cs(8,13): error CS0122: `Farm.Animal' is inaccessible due to its protection level
Programm.cs(9,26): error CS0841: A local variable `cat' cannot be used before it is declared
```

*******************************************************************************


                                 {{1-2}}
*******************************************************************************

**Sichtbarkeit der Felder und Member einer Stuktur**

Für die Sichtbarkeit auf der Ebene der Felder und Member können für structs
drei Attribute verwendet werden `private`, `internal` und `public`. Standard
ist `private`.

```csharp                                      Attributes
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;
    internal string sound;
    private byte age;

    public Animal(string name, uint born, string sound = "Miau"){
      this.name = name;
      this.sound = sound;
      age = (byte) (2019 - born);
    }

    public void MakeNoise() {
    	Console.WriteLine("{0} ({1} years old) makes {2}", name, age, sound);
    }
  }

  public class Program
  {
    public static void Main(string[] args){
      Animal Wally = new Animal ("Wally", 2014, "Wau");
      Wally.MakeNoise();
      Wally.age = 5;
    }
  }
}
```
@Rextester.eval(@CSharp)


*******************************************************************************

### ... ja, aber ...

Welche Einschränkungen haben Structs gegenüber Klassen?

> `struct`s kennen das Konzept der Vererbung nicht! Eine Struktur kann nicht von einer
> anderen Struktur oder Klasse erben, und sie kann auch nicht die Basis einer Klasse
> sein.

https://docs.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/using-structs

Welche Konsequenzen hat das?

| Structs                                         | Klassen |
| ----------------------------------------------- | ------- |
| Werttyp (Variablen enthalten das Objektes)      | Referenzdatentyp         |
| Abgelegt auf dem Stack                          | Gespeichert auf dem Heap        |
| Unterstützen keine Vererbung                    |  Unterstützen Vererbung       |
| können Interfaces implementieren                | können Interfaces implementieren        |
| `internal` und `public` als Struct-Attribute | `private`, `internal` und `public` als Klassenattribute |
| `private`, `internal` und `public` als Feld und Member-Attribute| analog plus 3 weitere Attribute|
| keine parameterlosen Konstruktoren deklarierbar |         |

> **Merke:** Strukturen sind Wertdatentypen!

```csharp                                      Attributes
using System;

namespace Rextester
{
  public struct Animal
  {
    public string name;
    internal string sound;
    private byte age;

    public Animal(string name, uint born, string sound = "Miau"){
      this.name = name;
      this.sound = sound;
      age = (byte) (2019 - born);
    }

    public void MakeNoise() {
    	Console.WriteLine("{0} ({1} years old) makes {2}", name, age, sound);
    }
  }

  public class Program
  {
    public static void Main(string[] args){
      Animal Wally = new Animal ("Wally", 2014, "Wau");
      Wally.MakeNoise();
      Wally.age = 5;
    }
  }
}
```
@Rextester.eval(@CSharp)

## Beispiel der Woche ...

Im folgenden Beispiel werden Instanzen des Structs "Animal" von einem anderen
Struct "Farm" genutzt und dort in einer (generischen) Liste gespeichert.

```csharp                                      Usage
using System;
using System.Collections;
using System.Collections.Generic;

namespace Rextester
{
  public struct Animal
  {
    public string name;
    public string sound;

    public Animal(string name, string sound = "Miau"){
      this.name = name;
      this.sound = sound;
    }

    public void MakeNoise() {
    	Console.WriteLine("{0} makes {1}", name, sound);
    }
  }

  public struct Farm{
    public string adress;
    public List<Animal> animalList;

    public Farm(string adress) {
    	animalList = new List<Animal>();
    	this.adress = adress;
    }

    public void AddAnimal(Animal newanimal){
      animalList.Add(newanimal);
    }

    public void PrintAnimals(){
      foreach (Animal pet in animalList){
        pet.MakeNoise();
      }
    }
  }

  public class Program
  {
    public static void Main(string[] args){
      Animal Wally = new Animal ("Wally","Wau");
      Animal Kitty = new Animal ("Kitty","Miau");
      Farm myFarm = new Farm("Biobauernhof Freiberg");
      myFarm.AddAnimal(Wally);
      myFarm.AddAnimal(Kitty);
      myFarm.PrintAnimals();
    }
  }
}
```
@Rextester.eval(@CSharp)
