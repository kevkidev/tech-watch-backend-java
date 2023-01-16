# Java 14 example: Switch expressions

https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html

```java
public class PizzaV14 {
	private static enum Category {
		VEGAN, SPICY, CANIBAL, FISH
	}
	public static void main(String[] args) throws Exception {
		System.out.println("Starting v13...");
		var name = "";
		var category = Category.CANIBAL;
		// before v13
		switch (category) {
			case CANIBAL:
			case SPICY:
				name = "Mexican";
				break;
			case FISH:
			case VEGAN:
				name = "4 seasons";
				break;
			default:
				throw new IllegalStateException("Unknow category " + category);
		}
		System.out.println(name);
		// v14
		switch (category) {
			case CANIBAL, SPICY -> name = "Mexican";
			case FISH, VEGAN -> name = "4 seasons";
			default -> throw new IllegalStateException("Unknow category " + category);
		}
		System.out.println(name);
		// or
		name = switch (category) {
			case CANIBAL, SPICY -> "Mexican";
			case FISH, VEGAN -> "4 seasons";
			default -> throw new IllegalStateException("Unknow category " + category);
		};
		System.out.println(name);
		// or
		System.out.println(switch (category) {
			case CANIBAL, SPICY -> "Mexican";
			case FISH, VEGAN -> "4 seasons";
			default -> throw new IllegalStateException("Unknow category " + category);
		});
		// other syntax with the yield but it's recommended that you use "case L ->"
		// labels
		name = switch (category) {
			case CANIBAL:
			case SPICY:
				yield "Mexican";
			case FISH:
			case VEGAN:
				yield "4 seasons";
			default:
				throw new IllegalStateException("Unknow category " + category);
		};
		System.out.println(name);
		// switch labeled statement group
		// using block enclosing the yield is mandatory to return the value
		name = switch (category) {
			case CANIBAL, SPICY -> {
				System.out.println("Mexican");
				yield "Mexican";
			}
			case FISH, VEGAN -> {
				System.out.println("4 seasons");
				yield "4 seasons";
			}
			default -> throw new IllegalStateException("Unknow category " + category);
		};
		System.out.println(name);
	}
}
```
