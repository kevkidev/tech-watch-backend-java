# java.lang.Record (Java SE 16+)

https://docs.oracle.com/en/java/javase/16/language/records.html

## What? Why ?

- Permet de créer un objet imuttable en une ligne de code.
- C’est comme un class normal mais sans setters
- Les getter ne sont pas precedé de ‘get’ => même nom que field
- is implicitly final => cannot explicitly extend a record
- Ex de Header:

  `record Rectangle(double length, double width) { }`

## Auto generation en arriere plan

- Private felds
- Public accessor
- Consctructeur surchargé (mm signature que header)
- Implemtation toString, hashcode, equals

## How to instanciate a record ?

`Rectangle r = new Rectangle(4,5);
`

## custom constructor

```java
record Rectangle(double length, double width) {
    public Rectangle(double length, double width) {
        if (length <= 0 || width <= 0) {
            throw new java.lang.IllegalArgumentException(
                String.format("Invalid dimensions: %f, %f", length, width));
        }
        this.length = length;
        this.width = width;
    }
}
```

equivalent: signature is implicit :smiley: (private fields are implicitly assigned)

```java
record Rectangle(double length, double width) {
    public Rectangle {
        if (length <= 0 || width <= 0) {
            throw new java.lang.IllegalArgumentException(
                String.format("Invalid dimensions: %f, %f", length, width));
        }
    }
}
```

You can manually declare an accessor to custom implementation or to change his modifier to `public`

- ensure that they have the same characteristics and behavior as those in the java.lang.Record class

## You can declare static/instance members, nested classes/interfaces (including nested record)

static

```java
record Rectangle(double length, double width) {

    // Static field
    static double goldenRatio;

    // Static initializer
    static {
        goldenRatio = (1 + Math.sqrt(5)) / 2;
    }

    // Static method
    public static Rectangle createGoldenRectangle(double width) {
        return new Rectangle(width, width * goldenRatio);
    }
}
```

instance

```java
record Rectangle(double length, double width) {

    // Field declarations must be static:
    BiFunction<Double, Double, Double> diagonal;

    // Instance initializers are not allowed in records:
    {
        diagonal = (x, y) -> Math.sqrt(x*x + y*y);
    }
}
```

**!! You cannot declare native methods in a record class.**

## Features

- generic :white_check_mark:
- implements interfaces :white_check_mark:
- Annotations :white_check_mark:
- work with sealed classes and interfaces :white_check_mark:
- You can serialize and deserialize instances of record classes (but: https://docs.oracle.com/en/java/javase/16/serializable-records/index.html)

## Use cases

- local record class:
  - improves the readability of the stream operations
  - are implicitly static
- In Java SE 16 and later, an inner class may declare members that are either explicitly or implicitly static, which includes record class members.

  - ```
      public class ContactList {

        record Contact(String name, String number) { }

        public static void main(String[] args) {

            class Task implements Runnable {

                // Record class member, implicitly static,
                // declared in an inner class
                Contact c;

                public Task(Contact contact) {
                    c = contact;
                }
                public void run() {
                    System.out.println(c.name + ", " + c.number);
                }
            }

            List<Contact> contacts = List.of(
                new Contact("Sneha", "555-1234"),
                new Contact("Raj", "555-2345"));
            contacts.stream()
                    .forEach(cont -> new Thread(new Task(cont)).start());
        }
    }
    ```

## Prevent errors : `error: reference to Record is ambiguous`

use fully qualified name for imports ex:
`import com.myapp.Record;`

## methods from `java.lang.Class`

- `RecordComponent[] getRecordComponents()`: Returns an array of java.lang.reflect.RecordComponent objects, which correspond to the record class's components.
- `boolean isRecord()`: Similar to isEnum() except that it returns true if the class was declared as a record class.
