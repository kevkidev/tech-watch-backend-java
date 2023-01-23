# Sealed classes (Java SE 16+)
## sources 
- base: https://docs.oracle.com/en/java/javase/17/language/sealed-classes-and-interfaces.html
- plus: https://openjdk.org/jeps/409

## what / why ?
- Même chose que final mais possible de choisir quel classe peut heriter grace au mot clé `permits`.

```java
public sealed class Shape
    permits Circle, Square, Rectangle {
}
```

Si class fille declarer dans meme fichier => pas besoin de permits:
```java
package com.example.geometry;

public sealed class Figure
    // The permits clause has been omitted
    // as its permitted classes have been
    // defined in the same file.
{ }

final class Circle extends Figure {
    float radius;
}
non-sealed class Square extends Figure {
    float side;
}
sealed class Rectangle extends Figure {
    float length, width;
}
final class FilledRectangle extends Rectangle {
    int red, green, blue;
}
```

## Contraintes sur les classes filles :
- etre accessible par la classe mere à la compilation
- etendre la sealed directement
- il faut un des 3 modifiers : final, sealed, non-sealed
- meme module ou package que la sealed
- peut etre un record class

## non-sealed
non-sealed: Can be extended by unknown subclasses; a sealed class cannot prevent its permitted subclasses from doing this

En gros `class Truc` equivaut à `non-sealed class Truc` sauf que dans le cas 2 Truc est une classe permitted (qui herite d'une sealed) c'est tout. C'est juste qu'il faut specifier obligatoirement un modifier pour les permitted donc au lieu de rien on met `non-sealed`.

## Interface
Meme principe que pour la classe.
```java
sealed interface Expr
    permits ConstantExpr, PlusExpr, TimesExpr, NegExpr {
    public int eval();
}
```

## conversion
- on peut caster si heritage permit ou si relation possible. 
- Pas de cast possible si class en final
```java
public sealed interface Shape permits Polygon { }
public non-sealed interface Polygon extends Shape { }
public final class UtahTeapot { }
public class Ring { }

public void work(Shape s) {
    UtahTeapot u = (UtahTeapot) s;  // Error
    Ring r = (Ring) s;              // Permitted
}
```

## methods
- `java.lang.constant.ClassDesc[] permittedSubclasses()`: Returns an array containing java.lang.constant.ClassDesc objects representing all the permitted subclasses of the class if it is sealed; returns an empty array if the class is not sealed
- `boolean isSealed()`: Returns true if the given class or interface is sealed; returns false otherwise