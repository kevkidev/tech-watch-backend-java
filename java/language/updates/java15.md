# Java 15 example: Text Blocks

https://docs.oracle.com/en/java/javase/14/text-blocks/index.html

```java
public class PizzaV14 {

	public static void main(String[] args) throws Exception {
		var welcomeMessage = """
				Welcome to the pizzeria""";

		System.out.println("""
				Starting v14...""");

		System.out.println("""
				spicy""".toUpperCase());

		var name = "List of pizza\n" + "   - Pepperoni\n" + "   - Meat\n" + "   - Veggie\n" + "   - Margherita\n";
		System.out.println(name);

		// v15
		// no \n at the end
		name = """
				List of pizza
				  - Pepperoni
				  - Meat
				  - Veggie
				  - Margherita""";

		System.out.println(name);

		// one \n at the end
		name = """
				List of pizza
					- Pepperoni
					- Meat
					- Veggie
					- Margherita
					""";

		System.out.println(name);

		// escaping new lines
		name = """
				List of pizza
					- Pepperoni \
					- Meat \
					- Veggie \
					- Margherita \
					""";

		System.out.println(name);

		// escaping new lines after the \s => each line is exactly the same characters
		// long
		name = """
				List of pizza
					- Pepperoni \s
					- Meat      \s
					- Veggie    \s
					- Margherita\s
					""";
		System.out.println(name);

		String source = """
				String message = "Hello, World!";
				System.out.println(message);
				""";
		System.out.println(source);

		// When a text block contains sequences of three or more double quotes,
		// escape the first double quote of every run of three double quotes.
		String code = """
				String source = \"""
				    String message = "Hello, World!";
				    System.out.println(message);
				    \""";
				""";
	}
}
```
