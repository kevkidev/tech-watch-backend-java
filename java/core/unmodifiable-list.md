# Creating Unmodifiable Lists, Sets, and Maps

## source
- https://docs.oracle.com/en/java/javase/19/core/creating-immutable-lists-sets-and-maps.html#GUID-DD066F67-9C9B-444E-A3CB-820503735951

## what / why ?
-  elements cannot be added, removed, or replaced
- holds the same data as long as a reference to it exists
- generally consume much less memory than modifiable collection

## when ?
- depends on the data in the collection
- si valeurs constantes et connues
- si valeurs calculées et ne change pas après initialization
- si valeurs lues via un fichier de config

## List Static Factory Methods
- `List.of`: creer une liste non modifiable
-  Null values are NOT allowed
- duplicate elements are allowed

```java
List.of()
List.of(e1)
List.of(e1, e2)         // fixed-argument form overloads up to 10 elements
List.of(elements...)   // varargs form supports an arbitrary number of elements or an array
```

## Set Static Factory Methods
- `Set.of`: creer un set
- si doublons => IllegalArgumentException
```java
Set.of()
Set.of(e1)
Set.of(e1, e2)         // fixed-argument form overloads up to 10 elements
Set.of(elements...)   // varargs form supports an arbitrary number of elements or an array
```

## Map Static Factory Methods
- `Map.of`: creer une map
- A Map cannot contain duplicate keys => IllegalArgumentException
- Null cannot be used for either Map keys or values
```java
Map<String, Integer> stringMap = Map.of("a", 1, "b", 2, "c", 3);
// key: a, val: 1
// key: b, val: 2
// key: c, val: 3
```
Si plus de 10 pairs key-val 
- il faut utiliser Map.ofEntries() + `Map.entry`
- utiliser import static plus simple et lisible

```java
import static java.util.Map.entry;

Map <Integer, String> friendMap = Map.ofEntries(
   entry(1, "Tom"),
   entry(2, "Dick"),
   entry(3, "Harry"),
   ...
   entry(99, "Mathilde"));
```

## Copies of Collections
`List.copyOf`, `Set.copyOf`, `Map.copyOf`: 
- isolate the returned collection from changes to the original one.
- Creer une copie non modifiable d'une collection modifiable
- la copie est detachée de l'original c'est un clone pas une ref
- MAIS si original est deja non modifiable alors la copie sera une ref vu quelle ne pourra pas changer dans tous les cas ;)
- **ATTENTION !!! si les elements de la collection sont mutables alors ils seront modifiés dans les 2 listes.**
```java
List<Item> snapshot = List.copyOf(list);
```
- les 3 methodes prener un Collection en param => possible Map, List ou Set => conversion automatique
`Set.copyOf(list)`: si list est de type List avec des doublons 
- pas d'exeption levé
- il choisi un element parmis les doublons

## Stream: create unmodifiable with Collectors
```java
    Collectors.toUnmodifiableList()
    Collectors.toUnmodifiableSet()
    Collectors.toUnmodifiableMap(keyMapper, valueMapper)     
    Collectors.toUnmodifiableMap(keyMapper, valueMapper, mergeFunction)
```
-  the `toUnmodifiableSet` collector chooses an arbitrary one of the duplicates to include in the resulting Set
```java
    Collectors.toUnmodifiableMap(keyMapper, valueMapper)     
```
- if the keyMapper function produces duplicate keys, an `IllegalStateException` is thrown
-  If duplicate keys are a possibility, use the following collector instead :
```java  
    Collectors.toUnmodifiableMap(keyMapper, valueMapper, mergeFunction)
```
- it merge the values of each duplicate key into a single value.