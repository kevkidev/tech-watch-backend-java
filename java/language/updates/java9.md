# Java 9 example

```java
public class PizzaV9 {
	private static class Pizzeria implements AutoCloseable {
		@Override
		public void close() throws Exception {
			System.out.println("Pizzeria closed.");
		}
		@SafeVarargs // before v9 : only to methods that cannot be overridden and
		private void build(String... pizzaNames) {// now it's possible on private methods
			for (String name : pizzaNames) {
				System.out.println("Building " + name);
			}
		}
	}
	private interface IPizzeria<T> {
		public void open(T value);
		/*
		 * Private interface methods are supported. This support allows nonabstract
		 * methods of an interface to share code between them.
		 */
		private void commonCode() {
		}
		default void method() {
			commonCode();
		}
	}
	public static void main(String[] args) throws Exception {
		System.out.println("Starting...");
		// final pizzeria
		Pizzeria pizzeria = new Pizzeria();
		// effectivily final pizzeria
		final Pizzeria pizzeria2 = new Pizzeria();
		// java 8
		try (Pizzeria p1 = pizzeria; Pizzeria p2 = pizzeria2;) {
			System.out.println("with v8");
		}
		// java 9
		try (pizzeria; pizzeria2) {
			System.out.println("with v9+");
			pizzeria.build("Rene", "Royale", "Orientale");
		}
		// You can use diamond syntax in conjunction with anonymous inner classes.
		IPizzeria<String> oo = new IPizzeria<>() { // <> not possible before v9
			@Override
			public void open(String value) {
				// TODO Auto-generated method stub
			}
		};
		// The underscore character is not a legal name.
		int _ = 0; // it doesn't compile
	}
}
```
