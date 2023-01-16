# Java 11 example

```java
public class PizzaV11 {
	@FunctionalInterface
	private static interface Make {
		void make(int value);
	}
	private static class Pizzeria implements AutoCloseable {
		@Override
		public void close() throws Exception {
			System.out.println("Closed");
		}
		void make(Make maker) {
			maker.make(5);
		}
	}
	private static class Var {
	}
	// private static interface var { // wrong
	// }
	public static void main(String[] args) throws Exception {
		System.out.println("Starting v10...");
		// A new identifier named var is now available for local variables with non-null
		// initializers. Using this identifier, the type of the variable is inferred
		// from the context. The code from the previous example can be rewritten as
		// shown in the following example:
		var pizzeria = new Pizzeria();
		// var is a reserved type name, not a keyword, which means that existing code
		// that uses var as a variable, method, or package name is not affected
		{
			var var = 0; // it's okay
		}
		{
			var var = new Var(); // it's okay
		}
		Var var = new Var(); // it's okay
		var pizzas = Arrays.asList("Rene", "Royale", "Orientale");
		// var with try
		try (var piz = new Pizzeria();) {
			System.out.println("with v10+");
		}
		var piz = new Pizzeria();
		Make callback = (var val) -> { // var possible from v11
			System.out.println(val);
		};
		piz.make(callback);
	}
}
```
