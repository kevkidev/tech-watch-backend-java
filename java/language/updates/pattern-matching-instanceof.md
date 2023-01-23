# pattern matching for instanceof - (java SE v16+) - JEP 394
## sources 
- base: https://docs.oracle.com/en/java/javase/16/language/pattern-matching-instanceof-operator.html
- plus: https://openjdk.org/jeps/394
## what/ why ?
Just un upgrade de l'existant pour coller aux autre changements. 

Permet de tester le type de l'objet avant d'extraire les datas.
Test plus cast en une ligne :smiley:

## sans pattern : 
- redundant, code lourd
- convertion manuelle
```java
public static double getPerimeter(Shape shape) throws IllegalArgumentException {
        if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return 2 * r.length() + 2 * r.width();
        } else if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return 2 * c.radius() * Math.PI;
        } else {
            throw new IllegalArgumentException("Unrecognized shape");
        }
    }
```

## avec pattern : 
- plus facile et lisible.
- conversion à la volé, moins d'erreur possible =>x plus safe
- r et c sont créées seulement si condition vrai


```java
  public static double getPerimeter(Shape shape) throws IllegalArgumentException {
        if (shape instanceof Rectangle r) {
            return 2 * r.length() + 2 * r.width();
        } else if (shape instanceof Circle c) {
            return 2 * c.radius() * Math.PI;
        } else {
            throw new IllegalArgumentException("Unrecognized shape");
        }
    }
```
## scope
Variable utilisable seulement dans le scope ou la condition est vrai 

- ici la condition est vrai en dehors du if => r accessible
- donc ça marche pas avec un ||
```java
   public static boolean bigEnoughRect(Shape s) {
        if (!(s instanceof Rectangle r)) {
            // You cannot use the pattern variable r here because
            // the predicate s instanceof Rectangle is false.
            return false;
        }
        // You can use r here.
        return r.length() > 5; 
    }
```
- ici la 2nd expression est executer que si la premiere est vrai => r est accessible
```java
   if (shape instanceof Rectangle r && r.length() > 5) {
            // ...
        }
```