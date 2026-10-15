## 3.3 - Regular Expressions

Information extracted from [this webpage](https://nachoiborraies.github.io/java/md/en/03d). It's later expandend adding a section about combining regular expressions and functional programming.

## Using Regular Expressions

A **regular expression** (or **regex**, in its abbreviated form) is a sequence of special characters that allows us to detect patterns in texts. It is not meant for humans but for computers, so even if they appear to be very strange, a computer is capable of detect any text pattern if the regex is well defined.

For example, an identification number formed by 8 digits and one capital letter, or an email that contains an `@`.

Using regular expressions, we can easily detect these patterns in a given text, or also **enforce a text to comply with a format** when entered by the user.

## 1\. Basic Regular Expression Syntax

To work with regular expressions in Java, we need to use the classes from the `java.util.regex` package. Specifically, we'll rely on:

  * **`Pattern`** → allows defining a regular expression pattern.
  * **`Matcher`** → allows checking if a text matches a given pattern.

> **TIP:** Actually, we can use regular expressions in more ways than these and the String class, for instance, has several methods that use regex. However, `Pattern` and `Matcher` work very well for the general use of regex.

### 1.1. `Pattern` Class

The `Pattern` class has several useful methods, including:

  * `compile`: creates a `Pattern` object from a regular expression.
  * `matcher`: returns a `Matcher` object that can use that pattern with a text.

### 1.2. `Matcher` Class

The `Matcher` class allows using the following methods:

  * `find`: checks if a pattern is found anywhere in the text.
  * `matches`: checks if **the entire text** matches the pattern (not just a part).
    The static method `Pattern.matches()` also exists and does the same thing.

### 1.3. Example

Let's check if a text contains at least one digit between 0 and 9.
The `\d` symbol represents a digit, so we'll use it within the pattern:

```java
String texto = "Hola, me llamo Nacho y tengo 44 años";
Pattern p = Pattern.compile("\\d");
Matcher m = p.matcher(texto);

if (m.find()) {
    System.out.println("The text contains digit(s)");
} else {
    System.out.println("The text does not contain any digits");
}
```

> Note: We must escape the `\` character when writing it inside a String, which is why we use `\\d`.

### 1.4. Basic Symbols

| Symbol | Meaning |
| :--- | :--- |
| x | The literal character 'x' |
| \\t | Tab |
| \\n | Newline |
| [abc] | Any of the characters 'a', 'b', or 'c' |
| [^abc] | Any character **except** 'a', 'b', or 'c' |
| [a-zA-Z] | Range from 'a' to 'z' or 'A' to 'Z' |
| . | Any character |
| ^ | Start of line |
| $ | End of line |
| \\d | Digit (0–9) |
| \\D | Not a digit |
| \\s | Whitespace, tab, or line break |
| \\S | Anything that is not whitespace |
| \\w | Alphanumeric character or underscore |
| \\W | Anything that is **not** alphanumeric |
| (one | two) | The text "one" or "two" |

**Examples:**

  * Text that ends with a period:

    ```java
    Pattern p = Pattern.compile("\\.$");
    ```

  * Text formed by 4 digits:

    ```java
    Pattern p = Pattern.compile("^\\d\\d\\d\\d$");
    ```

  * The seasons of the year:

    ```java
    Pattern p = Pattern.compile("(winter|spring|summer|autumn)");
    ```

> **ACTIVITY**
>
> Create a project called **CarIDCheck** that prompts the user for a license plate number and verifies if it is composed of **4 digits followed by 3 capital letters**.
>
> (We are not checking if the letters are vowels or not, only that they are capital letters).

-----

## 2\. More Complex Expressions

To create more complex expressions, we can use **cardinality symbols**, which indicate how many times an element can be repeated.

| Symbol | Meaning |
| :--- | :--- |
| x? | The symbol x appears 0 or 1 time |
| x+ | The symbol x appears 1 or more times |
| x\* | The symbol x appears 0 or more times |
| x{n} | The symbol x appears n times |
| x{n,} | Appears at least n times |
| x{n,m} | Appears between n and m times (both inclusive) |

**Examples:**

  * Text formed by 4 digits:

    ```java
    Pattern p = Pattern.compile("^\\d{4}$");
    ```

  * ID formed by 8 digits and one capital letter:

    ```java
    Pattern p = Pattern.compile("^\\d{8}[A-Z]$");
    ```

> **ACTIVITY**
>
> Repeat the previous exercise using **cardinality symbols**.

> **ACTIVITY**
> Create a program called **EmailChecker** that prompts the user for an email address and checks if it is valid.
>
> A valid email will have:
>
>   * one or more alphanumeric characters,
>   * followed by `@`,
>   * followed by one or more alphanumeric characters,
>   * followed by a period `.`,
>   * and one or more alphanumeric characters.
>
> Valid example: `myEmail@one.com`
> Invalid example: `myOtherMail@aaa`

-----

## 3\. Using Groups

**Groups** allow us to isolate parts of a text that match a pattern, so we can process them later in the code.
Each group is defined within parentheses `( )` and we can access them with the `group()` method of the `Matcher` class.

**Example: getting all sequences of 4 digits in a text:**

```java
String texto = "Einstein nació en 1879 y Edison en 1847";
Pattern p = Pattern.compile("(\\d{4})");
Matcher m = p.matcher(texto);

if (!m.find()) {
    System.out.println("The text has no sequences of 4 digits");
} else {
    do {
        String dato = m.group();
        System.out.println("Found: " + dato);
    } while (m.find());
}
```

Every time we call `group()`, the next match is obtained.

### 3.1. Multiple Groups

We can define **more than one group** within an expression.
In this case, `group(n)` returns the group number `n`, starting from 1.

Example: identifying first and last names in a text:

```java
String texto = "Albert Einstein nació en 1879 y Thomas Edison en 1847";
Pattern p = Pattern.compile("([A-Z][a-z]+) ([A-Z][a-z]+)");
Matcher m = p.matcher(texto);

if (!m.find()) {
    System.out.println("No names were found");
} else {
    do {
        String nombre = m.group(1);
        String apellido = m.group(2);
        System.out.println("Found: " + nombre + " " + apellido);
    } while (m.find());
}
```

**Output:**

```
Found: Albert Einstein  
Found: Thomas Edison
```

> **ACTIVITY**:
>
> Create a program called **HourIdentifier** that searches for times within a text.
> A time is formed by two digits, followed by `:`, and two other digits (for example, `08:45`).
> We don't need to check if the time is valid, just identify it.

-----

## 4\. Regular Expressions with `Stream().filter()`

We can combine **regular expressions** with **Streams** to easily filter a list of strings based on a given pattern.

For example, if we want to keep only the elements that **contain at least one number**, we can do it like this:

```java
import java.util.*;
import java.util.regex.*;
import java.util.stream.*;

public class RegexStreamExample {
    public static void main(String[] args) {
        List<String> texts = List.of("abc", "a1b", "hola", "java8", "stream");

        Pattern pattern = Pattern.compile(".*\\d.*"); // contains at least one digit

        List<String> withNumbers = texts.stream()
            .filter(t -> pattern.matcher(t).matches())
            .collect(Collectors.toList());

        System.out.println("Texts that contain numbers:");
        withNumbers.forEach(System.out::println);
    }
}
```

**Output:**

```
Texts that contain numbers:
a1b
java8
```

What happens is:

  * `Pattern.compile(".*\\d.*")` defines a regular expression that means “contains at least one digit”. 
    * `.*` means *any character may appear 0 or more times*, `\\d` (the escaped `\d`) means *one digit* and, at the end, it appears again the `.*`.
  * `stream().filter(...)` evaluates each string using `matcher().matches()`.
  * `collect(Collectors.toList())` collects the matches into a new mutable list.

> **ACTIVITY**:
> Create a program that, given a list of possible email addresses, **uses `stream().filter()` and a regular expression** to keep only the **valid** emails (those that have `@` and a correct domain).

> **ACTIVITY**:
> Improve the practice from the previous programming unit (My Favourite Composer) with regular expressions, specifically **searchComposerView** so that it allows searching with partial names using **regular expressions**. For example, if you include [*Franz Liszt*](https://es.wikipedia.org/wiki/Franz_Liszt) and [*Franz Peter Schubert*](https://es.wikipedia.org/wiki/Franz_Schubert) and search for ***Franz***, both should appear. If you type ***schu***, both *Franz Schubert* and [*Robert Schumann*](https://es.wikipedia.org/wiki/Robert_Schumann) should appear. 
>
> Make the searcher to be not case sensitive.

>**ACTIVITY**: Complete the following exercises with arrow functions (which have been generated using AI):

---

**Question #1:** Given a collection of integers, obtain a new collection containing only the even numbers.

**Question #2:** Given a collection of integers, obtain only those that are greater than zero.

**Question #3:** Given a collection of numbers, create another collection where each number is doubled.

**Question #4:** Given a collection of integers, create a new collection containing the square of each number.

**Question #5:** Given a collection of words, create a collection containing the length of each word.

**Question #6:** Given a collection of words, obtain only those that contain more than 5 characters.

**Question #7:** Given a collection of words, create another collection with all the words converted to uppercase.

**Question #8:** Given a collection of integers, filter the even numbers and calculate the square of each one.

**Question #9:** Given a collection of words, keep the words with more than 4 characters and convert them to uppercase.

**Question #10:** Given a collection of numbers, keep only the positive numbers and multiply each one by 10.

**Question #11:** Given a collection of names, obtain the names that start with the letter A and convert them to uppercase.

**Question #12:** Given a collection of prices, create a new collection applying a 20% discount only to prices greater than 100.

**Question #13:** Given a collection of numbers, calculate the sum of all its elements.

**Question #14:** Given a collection of numbers, calculate the product of all its elements.

**Question #15:** Given a collection of numbers, obtain the largest number.

**Question #16:** Given a collection of numbers, obtain the smallest number.

**Question #17:** Given a collection of numbers, filter the even numbers and calculate their sum.

**Question #18:** Given a collection of numbers, calculate their arithmetic mean using functional operations instead of an explicit loop.

**Question #19:** Given a collection of numbers, calculate the square of each number and sum all the results.

**Question #20:** Given a collection of numbers, keep the even numbers, square each one, and sum the results.

**Question #21:** Given a collection of numbers, determine whether there is at least one negative number.

**Question #22:** Given a collection of numbers, determine whether all its elements are greater than zero.

**Question #23:** Given a collection of numbers, determine whether none of them is equal to zero.

**Question #24:** Given a collection of numbers, find the first even number. If there is none, return a result representing the absence of a value.

**Question #25:** Given a collection of words and a specific word, determine whether that word is present in the collection.

**Question #26:** Given a collection of names, find the first name that contains more than 7 characters.

**Question #27:** Given a collection of products, where each product has a name, price, category, and stock quantity, obtain all products whose price is greater than 100.

> You can use a class for the product, but a MAP `<ATTRIBUTE : VALUE>` or even an Array (just know in which position is the value of each attribute) could work. You can use this workaround for customers, invoices, students or any complex data you may need.  

**Question #28:** Given a collection of products, obtain a collection containing only the names of all the products.

**Question #29:** Given a collection of products, obtain the products whose stock quantity is zero.

**Question #30:** Given a collection of products, obtain a collection containing the price of each product increased by 21%.

**Question #31:** Given a collection of products, obtain the names of the products whose price is less than 20.

**Question #32:** Given a collection of products, calculate the total inventory value. For each product, calculate price × stock quantity and sum all the results.

**Question #33:** Given a collection of products, determine whether there is at least one product whose stock quantity is zero.

**Question #34:** Given a collection of products, determine whether all products have at least one unit available.

**Question #35:** Given a collection of products, obtain the product with the highest price.

**Question #36:** Given a collection of products, obtain the product with the lowest price.

**Question #37:** Given a collection of numbers, obtain a new collection sorted from smallest to largest.

**Question #38:** Given a collection of numbers, sort it from largest to smallest.

**Question #39:** Given a collection of products, sort them by price from lowest to highest.

**Question #40:** Given a collection of products, sort them alphabetically by name.

**Question #41:** Given a collection of numbers that may contain duplicate values, create a collection containing each number only once.

**Question #42:** Given a collection of words, remove duplicate words while ignoring differences in uppercase and lowercase letters.

**Question #43:** Given a collection of products, group them by category.

**Question #44:** Given a collection of products, determine how many products there are in each category.

**Question #45:** Given a collection of products, calculate the average price of the products in each category.

**Question #46:** Given a collection of products, find the most expensive product in each category.

**Question #47:** Given a collection of collections of numbers, create a single collection containing all the numbers.

**Question #48:** Given a collection of sentences, obtain a single collection containing all the words from all the sentences.

**Question #49:** Given a collection of customers, where each customer has a collection of orders, obtain a single collection containing all the orders from all customers.

**Question #50:** Given a collection of orders, where each order contains a collection of products, obtain a single collection containing all the products included in all orders and remove duplicate products.

**Question #51:** Given a collection of products, remove products that are out of stock, sort them by price from highest to lowest, take only the first 5, and return their names.

**Question #52:** Given a collection of products and a specific category, filter the products belonging to that category, remove those that are out of stock, sort them by price, and obtain their names.

**Question #53:** Given a collection of numbers, calculate the number of elements, the minimum, the maximum, the sum, the average, the number of even numbers, and the number of odd numbers.

**Question #54:** Given a collection of users, where each user has a name, age, and active status, keep only active users who are adults and obtain their names sorted alphabetically.

**Question #55:** Given a collection of users, calculate the average age, find the oldest user, determine whether there is at least one minor, determine whether all users are active, and obtain the names of the active users.

**Question #56:** Given a collection of invoice lines, where each line has a product, quantity, and unit price, calculate the total invoice amount. For each line, calculate quantity × unit price and then sum all the amounts.

**Question #57:** Given a collection of invoice lines, calculate the amount for each line, apply a 10% discount to lines whose amount is greater than 500, and sum all the final amounts.

**Question #58:** Given a collection of students, where each student has a name and a collection of grades, calculate the average grade for each student, obtain the students whose average is greater than or equal to 5, obtain the names of the passing students, and calculate the average grade for the entire class.

**Question #59:** Given a collection of sentences, split the sentences into words, convert all words to lowercase, remove punctuation marks, count how many times each word appears, and obtain the words that appear more than 5 times.

**Question #60:** Given a collection of products, where each product has a name, price, category, and stock quantity, discard products that are out of stock, keep products whose price is between 20 and 200, apply a 15% discount, sort them by final price from highest to lowest, take the first 10, and obtain only their names.

----
