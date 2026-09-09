# Core Java Reference Material

> # Operators and Expressions in Java 21

🏚️ [Home](index.md) 🔸 ⬅️ Previous: [Variables and Data Types](variables.md) 🔸 ➡️ Next: [Control Flow Statements](control-flow.md)

## Table of Contents

1. [What Is an Operator?](#1-what-is-an-operator)
2. [Operands, Expressions, and Statements](#2-operands-expressions-and-statements)
3. [Categories of Java Operators](#3-categories-of-java-operators)
4. [Operator Precedence, Associativity, and Evaluation Order](#4-operator-precedence-associativity-and-evaluation-order)
5. [Arithmetic Operators](#5-arithmetic-operators)
6. [Integer Division and Remainder](#6-integer-division-and-remainder)
7. [Floating-Point Arithmetic](#7-floating-point-arithmetic)
8. [Numeric Promotion](#8-numeric-promotion)
9. [Unary Operators](#9-unary-operators)
10. [Increment and Decrement Operators](#10-increment-and-decrement-operators)
11. [Assignment Operators](#11-assignment-operators)
12. [Compound Assignment and Implicit Casting](#12-compound-assignment-and-implicit-casting)
13. [Relational Operators](#13-relational-operators)
14. [Equality Operators](#14-equality-operators)
15. [Logical Operators](#15-logical-operators)
16. [Short-Circuit Evaluation](#16-short-circuit-evaluation)
17. [Bitwise Operators](#17-bitwise-operators)
18. [Shift Operators](#18-shift-operators)
19. [Conditional Ternary Operator](#19-conditional-ternary-operator)
20. [`instanceof` Operator and Pattern Matching](#20-instanceof-operator-and-pattern-matching)
21. [String Concatenation Operator](#21-string-concatenation-operator)
22. [Character Arithmetic](#22-character-arithmetic)
23. [Primitive and Reference Comparisons](#23-primitive-and-reference-comparisons)
24. [Operators with Wrapper Classes and Unboxing](#24-operators-with-wrapper-classes-and-unboxing)
25. [Overflow, Underflow, and Exact Arithmetic](#25-overflow-underflow-and-exact-arithmetic)
26. [Operator Examples in Conditions](#26-operator-examples-in-conditions)
27. [Practical Operator Programs](#27-practical-operator-programs)
28. [Operators That Java Does Not Support](#28-operators-that-java-does-not-support)
29. [Complete Java 21 Operator Token List](#29-complete-java-21-operator-token-list)
30. [Lambda Expressions and Method References](#30-lambda-expressions-and-method-references)
31. [Switch Expressions Through Java 21](#31-switch-expressions-through-java-21)
32. [Pattern Matching and Record Patterns in Java 21](#32-pattern-matching-and-record-patterns-in-java-21)
33. [Java 21 Preview Features Related to Expressions](#33-java-21-preview-features-related-to-expressions)
34. [Java Version Timeline for Operators and Expressions](#34-java-version-timeline-for-operators-and-expressions)
35. [Operators Best Practices](#35-operators-best-practices)
36. [Common Operator Errors](#36-common-operator-errors)
37. [Quick Revision Tables](#37-quick-revision-tables)
38. [Frequently Asked Interview Questions](#38-frequently-asked-interview-questions)
39. [Official Java 21 References](#39-official-java-21-references)

## 1. What Is an Operator?

An **operator** is a symbol that tells Java to perform an operation on one, two, or three values. The values on which an operator acts are called **operands**.

### Java version scope

This chapter targets **Java SE 21**. It covers:

- all operator tokens and operator expressions defined by Java SE 21;
- operator behavior for primitive values, references, strings, wrappers, and arrays;
- Java 8 lambda expressions and method references;
- Java 14 switch expressions;
- Java 16 pattern matching for `instanceof`;
- Java 17 always-strict floating-point evaluation;
- Java 21 record patterns and pattern matching for `switch`; and
- relevant Java 21 preview features in a separate, clearly marked section.

Most basic operators existed long before Java 21 and behave the same in older Java releases. The newer sections identify their required Java versions. Preview features are not part of the permanent Java SE 21 language unless preview mode is enabled.

This is an **operators and expressions** chapter. Unrelated Java 21 platform features—such as virtual threads, sequenced collections, and new APIs—belong in their own chapters.

```java
int total = price + tax;
```

In this expression:

- `+` is the operator;
- `price` and `tax` are operands; and
- `price + tax` is an expression that produces a value.

Operators are used to:

- perform calculations;
- compare values;
- combine conditions;
- assign and update variables;
- manipulate individual bits;
- test an object's type; and
- choose between two results.

[↑ Go to Table of Contents](#table-of-contents)

## 2. Operands, Expressions, and Statements

These terms are related but have different meanings.

| Term | Meaning | Example |
| --- | --- | --- |
| Operand | A value on which an operator acts | `a` and `b` in `a + b` |
| Operator | A symbol that performs an operation | `+` |
| Expression | Code that evaluates to a value | `a + b` |
| Statement | A complete instruction | `int sum = a + b;` |

### Operators by number of operands

| Type | Operands | Example |
| --- | ---: | --- |
| Unary | One | `-number`, `!ready`, `count++` |
| Binary | Two | `a + b`, `x > y`, `p && q` |
| Ternary | Three | `condition ? first : second` |

The conditional operator `?:` is Java's only ternary operator.

```java
int larger = a > b ? a : b;
```

[↑ Go to Table of Contents](#table-of-contents)

## 3. Categories of Java Operators

```mermaid
flowchart TD
    O[Java Operators] --> V[Value operations]
    O --> C[Conditions]
    O --> B[Bit operations]
    O --> S[Selection and type tests]
    V --> A[Arithmetic and unary]
    V --> AS[Assignment]
    C --> R[Relational and equality]
    C --> L[Logical]
    B --> BW[Bitwise]
    B --> SH[Shifts]
    S --> T[Ternary]
    S --> IO[instanceof]
```

### Main categories

| Category | Operators |
| --- | --- |
| Arithmetic | `+`, `-`, `*`, `/`, `%` |
| Unary | `+`, `-`, `++`, `--`, `!`, `~` |
| Assignment | `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`, `>>>=` |
| Relational | `<`, `>`, `<=`, `>=` |
| Equality | `==`, `!=` |
| Logical | `&&`, `||`, `!` |
| Bitwise | `&`, `|`, `^`, `~` |
| Shift | `<<`, `>>`, `>>>` |
| Conditional | `?:` |
| Type comparison | `instanceof` |
| String concatenation | `+` |
| Lambda arrow | `->` |

The same symbol can have different meanings based on its operands. For example, `+` can add numbers, apply unary plus, or concatenate strings.

```java
int sum = 10 + 5;                    // numeric addition
int positive = +sum;                 // unary plus
String message = "Total: " + sum;    // string concatenation
```

Java SE 21 defines **38 lexical operator tokens**. The arrow token `->` is included because it is used in lambda expressions and switch rules. The keyword `instanceof` behaves as a relational operator even though it is lexically a keyword. The method-reference token `::` is formally a separator rather than an operator.

[↑ Go to Table of Contents](#table-of-contents)

## 4. Operator Precedence, Associativity, and Evaluation Order

**Precedence** determines which operator binds more tightly when parentheses are absent. **Associativity** determines how operators at the same precedence level are grouped.

### Precedence table

The following table is ordered from highest to lowest precedence.

| Level | Operators | Description | Associativity |
| ---: | --- | --- | --- |
| 1 | `expr++`, `expr--` | Postfix increment and decrement | Left to right |
| 2 | `++expr`, `--expr`, `+expr`, `-expr`, `!`, `~` | Unary operators | Right to left |
| 3 | `*`, `/`, `%` | Multiplication, division, remainder | Left to right |
| 4 | `+`, `-` | Addition, subtraction, concatenation | Left to right |
| 5 | `<<`, `>>`, `>>>` | Shift | Left to right |
| 6 | `<`, `<=`, `>`, `>=`, `instanceof` | Relational and type comparison | Left to right |
| 7 | `==`, `!=` | Equality | Left to right |
| 8 | `&` | Bitwise or non-short-circuit AND | Left to right |
| 9 | `^` | Bitwise or boolean XOR | Left to right |
| 10 | `|` | Bitwise or non-short-circuit OR | Left to right |
| 11 | `&&` | Short-circuit AND | Left to right |
| 12 | `||` | Short-circuit OR | Left to right |
| 13 | `?:` | Conditional operator | Right to left |
| 14 | `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `^=`, `|=`, `<<=`, `>>=`, `>>>=` | Assignment | Right to left |
| 15 | `->` | Lambda arrow | Right to left |

The `->` token also introduces a switch rule, but a switch-rule arrow is part of switch syntax rather than a binary operation on two operands. Method references such as `String::length` are primary expressions and bind tightly, even though `::` is not classified as an operator token.

### Precedence example

```java
int result = 10 + 2 * 3;
System.out.println(result); // 16
```

Multiplication has higher precedence, so Java groups the expression as:

```java
int result = 10 + (2 * 3);
```

Parentheses can make a different grouping explicit:

```java
int result = (10 + 2) * 3;
System.out.println(result); // 36
```

### Associativity example

Subtraction is left-associative:

```java
int value = 20 - 5 - 3;
// Grouped as: (20 - 5) - 3
System.out.println(value); // 12
```

Assignment is right-associative:

```java
int a;
int b;
a = b = 10;
// Grouped as: a = (b = 10)
```

### Evaluation order

Java evaluates operand expressions from left to right. Precedence controls grouping; it does not permit Java to ignore the language's evaluation order.

```java
static int show(String name, int value) {
    System.out.println(name);
    return value;
}

int answer = show("left", 2) + show("middle", 3) * show("right", 4);
```

The method calls print `left`, `middle`, and `right` in that order. The multiplication result is still used before the addition result is produced.

Use parentheses when an expression's meaning is not immediately obvious, even if precedence already gives the intended result.

Primary expressions—such as method calls, field access, array access, object creation, method references, and parenthesized expressions—bind more tightly than the operator levels in the table. Cast expressions group with unary expressions. The punctuation used by these forms, including `.`, `[]`, `()`, and `::`, is not part of Java SE 21's lexical operator-token list.

[↑ Go to Table of Contents](#table-of-contents)

## 5. Arithmetic Operators

Arithmetic operators perform mathematical operations.

| Operator | Operation | Example | Result |
| --- | --- | --- | ---: |
| `+` | Addition | `10 + 3` | `13` |
| `-` | Subtraction | `10 - 3` | `7` |
| `*` | Multiplication | `10 * 3` | `30` |
| `/` | Division | `10 / 3` | `3` |
| `%` | Remainder | `10 % 3` | `1` |

```java
int a = 17;
int b = 5;

System.out.println(a + b); // 22
System.out.println(a - b); // 12
System.out.println(a * b); // 85
System.out.println(a / b); // 3
System.out.println(a % b); // 2
```

Arithmetic operators can be used with numeric primitive types. Their exact result type depends on numeric promotion.

[↑ Go to Table of Contents](#table-of-contents)

## 6. Integer Division and Remainder

When both operands are integer types, `/` performs **integer division**. Any fractional part is discarded; the result is truncated toward zero.

```java
System.out.println(7 / 2);   // 3
System.out.println(-7 / 2);  // -3
System.out.println(7 / -2);  // -3
```

To retain a fractional result, make at least one operand floating-point:

```java
System.out.println(7.0 / 2);          // 3.5
System.out.println((double) 7 / 2);   // 3.5
```

### Remainder operator

The `%` operator returns the remainder after division. In Java, a nonzero remainder has the same sign as the dividend, which is the left operand.

```java
System.out.println(7 % 3);    // 1
System.out.println(-7 % 3);   // -1
System.out.println(7 % -3);   // 1
```

For a mathematical non-negative modulus, `Math.floorMod()` is often clearer:

```java
System.out.println(-7 % 3);             // -1
System.out.println(Math.floorMod(-7, 3)); // 2
```

### Division by zero

Integer division or remainder by zero throws `ArithmeticException` at runtime.

```java
int value = 10;
int divisor = 0;

// int result = value / divisor; // ArithmeticException: / by zero
```

A constant integer expression such as `10 / 0` is rejected at compile time.

[↑ Go to Table of Contents](#table-of-contents)

## 7. Floating-Point Arithmetic

Floating-point operations use `float` or `double` values.

```java
double price = 99.50;
double taxRate = 0.18;
double total = price + price * taxRate;

System.out.println(total); // 117.41 approximately
```

### Special floating-point values

Unlike integer division, floating-point division by zero does not throw `ArithmeticException`.

```java
System.out.println(10.0 / 0.0); // Infinity
System.out.println(-10.0 / 0.0); // -Infinity
System.out.println(0.0 / 0.0);  // NaN
```

`NaN` means **Not a Number**. It has unusual comparison behavior:

```java
double value = Double.NaN;

System.out.println(value == value);       // false
System.out.println(Double.isNaN(value));  // true
```

### Decimal precision

Binary floating-point cannot represent every decimal fraction exactly.

```java
System.out.println(0.1 + 0.2); // typically 0.30000000000000004
```

Use `BigDecimal` for calculations that require controlled decimal precision, such as many financial calculations.

```java
import java.math.BigDecimal;

BigDecimal first = new BigDecimal("0.1");
BigDecimal second = new BigDecimal("0.2");

System.out.println(first.add(second)); // 0.3
```

`BigDecimal` uses methods such as `add()`, `subtract()`, `multiply()`, and `divide()` rather than arithmetic operators.

### Always-strict floating-point evaluation in Java 17+

Since Java 17, every floating-point expression is evaluated strictly according to its declared `float` or `double` precision. Java 21 therefore does not use wider, platform-dependent intermediate values.

The `strictfp` modifier remains accepted for compatibility, but it no longer changes floating-point expression evaluation and is considered obsolete in Java 21.

[↑ Go to Table of Contents](#table-of-contents)

## 8. Numeric Promotion

Java promotes smaller numeric operands before many arithmetic, comparison, bitwise, and shift operations.

### Unary numeric promotion

Unary `+`, unary `-`, and `~` promote `byte`, `short`, and `char` to `int`.

```java
byte value = 10;

// byte negative = -value; // compile-time error: result is int
int negative = -value;
```

### Binary numeric promotion

For most binary numeric operators, Java generally promotes operands according to this order:

1. if either operand is `double`, both become `double`;
2. otherwise, if either is `float`, both become `float`;
3. otherwise, if either is `long`, both become `long`; and
4. otherwise, both become `int`.

```java
int count = 5;
double rate = 2.5;

double result = count * rate; // int is promoted to double
```

### The calculation occurs before assignment

The destination variable does not change the type used by the operation.

```java
int large = 1_000_000;
long wrong = large * large;        // multiplication occurs as int first
long correct = (long) large * large;
long alsoCorrect = 1_000_000L * 1_000_000;
```

`wrong` receives an already-overflowed `int` result. Convert at least one operand to `long` before the operation.

[↑ Go to Table of Contents](#table-of-contents)

## 9. Unary Operators

Unary operators act on one operand.

| Operator | Meaning | Example |
| --- | --- | --- |
| `+` | Unary plus | `+number` |
| `-` | Numeric negation | `-number` |
| `++` | Increment by one | `count++` |
| `--` | Decrement by one | `--count` |
| `!` | Boolean logical complement | `!available` |
| `~` | Bitwise complement | `~mask` |

```java
int number = 8;
boolean active = true;

System.out.println(+number);  // 8
System.out.println(-number);  // -8
System.out.println(!active);  // false
System.out.println(~number);  // -9
```

For an integer `x`, `~x` is equivalent to `-x - 1` under two's-complement representation.

[↑ Go to Table of Contents](#table-of-contents)

## 10. Increment and Decrement Operators

`++` adds one and `--` subtracts one. Each has prefix and postfix forms.

### Prefix form

The variable is updated first, and the updated value becomes the expression's value.

```java
int count = 5;
int result = ++count;

System.out.println(count);  // 6
System.out.println(result); // 6
```

### Postfix form

The original value becomes the expression's value, and then the variable is updated.

```java
int count = 5;
int result = count++;

System.out.println(count);  // 6
System.out.println(result); // 5
```

### Comparison

| Expression | Value used in expression | Variable afterward |
| --- | ---: | ---: |
| `++x` when `x` is `5` | `6` | `6` |
| `x++` when `x` is `5` | `5` | `6` |
| `--x` when `x` is `5` | `4` | `4` |
| `x--` when `x` is `5` | `5` | `4` |

Do not place several increments or decrements in one expression. Although Java defines the result, the code becomes difficult to understand.

```java
int x = 5;
int confusing = x++ + ++x;
```

Prefer separate statements:

```java
int x = 5;
int first = x;
x++;
x++;
int second = x;
int clear = first + second;
```

[↑ Go to Table of Contents](#table-of-contents)

## 11. Assignment Operators

The simple assignment operator `=` stores a value in a variable.

```java
int score = 90;
score = 95;
```

Compound assignment operators combine an operation with assignment.

| Operator | Similar expanded form |
| --- | --- |
| `x += y` | `x = x + y` |
| `x -= y` | `x = x - y` |
| `x *= y` | `x = x * y` |
| `x /= y` | `x = x / y` |
| `x %= y` | `x = x % y` |
| `x &= y` | `x = x & y` |
| `x |= y` | `x = x | y` |
| `x ^= y` | `x = x ^ y` |
| `x <<= y` | `x = x << y` |
| `x >>= y` | `x = x >> y` |
| `x >>>= y` | `x = x >>> y` |

```java
int balance = 100;

balance += 50; // 150
balance -= 20; // 130
balance *= 2;  // 260
balance /= 4;  // 65
balance %= 10; // 5
```

An assignment is itself an expression and produces the assigned value. Chained assignment is therefore legal:

```java
int a;
int b;
int c;

a = b = c = 25;
```

Use chained assignment only when it remains clear.

[↑ Go to Table of Contents](#table-of-contents)

## 12. Compound Assignment and Implicit Casting

A compound assignment is not always identical to its expanded form. It includes an implicit conversion back to the left operand's type.

```java
byte value = 10;
value += 5; // compiles; result is converted back to byte
```

The expanded version requires an explicit cast:

```java
byte value = 10;

// value = value + 5;       // compile-time error: value + 5 is int
value = (byte) (value + 5); // compiles
```

Conceptually:

```java
E1 op= E2
```

behaves like:

```java
E1 = (typeOfE1) (E1 op E2)
```

However, the left-hand expression is evaluated only once in the compound form.

### Possible narrowing data loss

The implicit conversion can discard information.

```java
byte value = 120;
value += 10;

System.out.println(value); // -126 because the byte range was exceeded
```

### String compound assignment

`+=` also performs string concatenation when the left side is a `String`.

```java
String message = "Java";
message += " Operators";

System.out.println(message); // Java Operators
```

[↑ Go to Table of Contents](#table-of-contents)

## 13. Relational Operators

Relational operators compare numeric values and produce a `boolean` result.

| Operator | Meaning |
| --- | --- |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal to |
| `>=` | Greater than or equal to |

```java
int age = 20;

System.out.println(age < 18);  // false
System.out.println(age > 18);  // true
System.out.println(age <= 20); // true
System.out.println(age >= 21); // false
```

Java does not support chained mathematical comparisons.

```java
int age = 20;

// boolean valid = 18 <= age <= 60; // compile-time error
boolean valid = age >= 18 && age <= 60;
```

Relational operators do not compare `boolean` values or arbitrary objects.

[↑ Go to Table of Contents](#table-of-contents)

## 14. Equality Operators

`==` tests equality and `!=` tests inequality.

### Primitive equality

For primitive numeric values, Java applies numeric promotion and compares the values.

```java
int whole = 10;
double decimal = 10.0;

System.out.println(whole == decimal); // true
System.out.println(whole != decimal); // false
```

Boolean values can be compared only with boolean values:

```java
boolean ready = true;
System.out.println(ready == true); // true, but simply using ready is clearer
```

### Reference equality

For references, `==` checks whether both operands refer to the same object, not whether separate objects contain equal data.

```java
String first = new String("Java");
String second = new String("Java");

System.out.println(first == second);      // false
System.out.println(first.equals(second)); // true
```

### Comparing with `null`

Use `==` or `!=` to compare a reference with `null`.

```java
if (name != null) {
    System.out.println(name.length());
}
```

Do not call `equals()` on a reference that may be `null`.

### Floating-point equality

Floating-point calculations may contain rounding error, so exact `==` is often unsuitable for calculated values.

```java
double actual = 0.1 + 0.2;
double expected = 0.3;
double tolerance = 1e-9;

boolean closeEnough = Math.abs(actual - expected) < tolerance;
```

Choose a tolerance appropriate for the application's scale and requirements.

`NaN` is unequal to every numeric value, including itself. Positive and negative floating-point zero compare equal with `==`, even though some later operations distinguish their signs.

```java
System.out.println(Double.NaN == Double.NaN); // false
System.out.println(+0.0 == -0.0);             // true
System.out.println(1.0 / +0.0);               // Infinity
System.out.println(1.0 / -0.0);               // -Infinity
```

Wrapper equality is deliberately different in two cases: `Double.valueOf(Double.NaN).equals(Double.NaN)` is `true`, while `Double.valueOf(+0.0).equals(-0.0)` is `false`. Choose primitive or wrapper comparison intentionally.

[↑ Go to Table of Contents](#table-of-contents)

## 15. Logical Operators

Logical operators combine or reverse boolean values.

| Operator | Name | Result |
| --- | --- | --- |
| `&&` | Conditional AND | `true` only when both operands are `true` |
| `||` | Conditional OR | `true` when at least one operand is `true` |
| `!` | Logical NOT | Reverses a boolean value |
| `&` | Boolean logical AND | Evaluates both operands |
| `|` | Boolean logical OR | Evaluates both operands |
| `^` | Boolean XOR | `true` when operands differ |

```java
boolean hasTicket = true;
boolean hasId = true;

boolean canEnter = hasTicket && hasId;
boolean needsHelp = !hasTicket || !hasId;

System.out.println(canEnter);  // true
System.out.println(needsHelp); // false
```

### Boolean truth table

| `a` | `b` | `a && b` | `a || b` | `a ^ b` |
| --- | --- | --- | --- | --- |
| `false` | `false` | `false` | `false` | `false` |
| `false` | `true` | `false` | `true` | `true` |
| `true` | `false` | `false` | `true` | `true` |
| `true` | `true` | `true` | `true` | `false` |

[↑ Go to Table of Contents](#table-of-contents)

## 16. Short-Circuit Evaluation

`&&` and `||` use **short-circuit evaluation**.

- With `a && b`, Java skips `b` when `a` is `false`.
- With `a || b`, Java skips `b` when `a` is `true`.

### Safe null check

```java
String name = null;

if (name != null && !name.isEmpty()) {
    System.out.println(name);
}
```

When `name` is `null`, the second operand is skipped, preventing `NullPointerException`.

Reversing the operands is unsafe:

```java
// Unsafe: isEmpty() runs before the null check
// if (!name.isEmpty() && name != null) { }
```

### Preventing invalid arithmetic

```java
int divisor = 0;
int value = 20;

if (divisor != 0 && value / divisor > 2) {
    System.out.println("Condition met");
}
```

### `&&` vs `&` and `||` vs `|`

```java
static boolean check() {
    System.out.println("check called");
    return true;
}

boolean first = false && check(); // check() is not called
boolean second = false & check(); // check() is called
```

Use `&&` and `||` for normal conditions. Use boolean `&`, `|`, or `^` only when evaluating both sides is intentionally required.

[↑ Go to Table of Contents](#table-of-contents)

## 17. Bitwise Operators

Bitwise operators work on the individual bits of integral values: `byte`, `short`, `char`, `int`, and `long`. Smaller operands are promoted to `int`.

| Operator | Name | Bit rule |
| --- | --- | --- |
| `&` | Bitwise AND | `1` only when both bits are `1` |
| `|` | Bitwise OR | `1` when either bit is `1` |
| `^` | Bitwise XOR | `1` when the bits differ |
| `~` | Bitwise complement | Changes every `0` to `1` and every `1` to `0` |

```java
int a = 12; // binary: 1100
int b = 10; // binary: 1010

System.out.println(a & b); // 8  -> 1000
System.out.println(a | b); // 14 -> 1110
System.out.println(a ^ b); // 6  -> 0110
```

### Bit masks

Bit masks are commonly used for compact flags.

```java
int READ = 1;    // 0001
int WRITE = 2;   // 0010
int EXECUTE = 4; // 0100

int permissions = READ | WRITE;

boolean canRead = (permissions & READ) != 0;
boolean canExecute = (permissions & EXECUTE) != 0;

permissions |= EXECUTE; // add permission
permissions &= ~WRITE;  // remove permission
permissions ^= READ;    // toggle permission
```

Use named constants or an `enum` rather than unexplained numeric masks.

[↑ Go to Table of Contents](#table-of-contents)

## 18. Shift Operators

Shift operators move the bit pattern of an integral value.

| Operator | Name | Behavior |
| --- | --- | --- |
| `<<` | Left shift | Shifts left and fills low-order positions with zeros |
| `>>` | Signed right shift | Shifts right and copies the sign bit |
| `>>>` | Unsigned right shift | Shifts right and fills high-order positions with zeros |

```java
int value = 8; // 0000...1000

System.out.println(value << 1); // 16
System.out.println(value >> 1); // 4
```

For non-negative values that do not overflow:

- shifting left by `n` positions often corresponds to multiplying by `2^n`; and
- shifting right by `n` positions often corresponds to dividing by `2^n`.

These are not universal replacements for arithmetic because overflow, negative values, and rounding can change the result.

### Signed vs unsigned right shift

```java
int negative = -8;

System.out.println(negative >> 1);  // -4
System.out.println(negative >>> 1); // 2147483644
```

`>>` preserves the negative sign. `>>>` inserts zeros and therefore produces a large positive `int` in this example.

### Shift-distance masking

For an `int`, Java uses only the lowest five bits of the shift distance. For a `long`, it uses only the lowest six bits.

```java
System.out.println(1 << 32);  // 1, equivalent to 1 << 0
System.out.println(1L << 64); // 1, equivalent to 1L << 0
```

### Smaller integral types

Shift operations promote `byte`, `short`, and `char` to `int`.

```java
byte value = 4;
int shifted = value << 1;
```

### Unsigned operations on signed storage

Java's integer primitive types other than `char` are signed, but Java 8 and later provide utilities that interpret `int` or `long` bits as unsigned values.

```java
int bits = -1; // all 32 bits are 1

System.out.println(Integer.toUnsignedString(bits)); // 4294967295
System.out.println(Integer.compareUnsigned(bits, 1) > 0); // true
System.out.println(Integer.divideUnsigned(bits, 2));      // 2147483647
System.out.println(Integer.remainderUnsigned(bits, 2));   // 1
```

These methods do not create a new unsigned primitive type. They reinterpret the existing bit pattern for a particular operation.

[↑ Go to Table of Contents](#table-of-contents)

## 19. Conditional Ternary Operator

The conditional operator selects one of two expressions.

```java
condition ? valueWhenTrue : valueWhenFalse
```

Example:

```java
int age = 20;
String category = age >= 18 ? "Adult" : "Minor";
```

Only the selected branch is evaluated.

```java
String text = null;
int length = text != null ? text.length() : 0;
```

### Nested conditional expressions

The operator is right-associative:

```java
int score = 82;
String grade = score >= 90 ? "A"
             : score >= 75 ? "B"
             : score >= 60 ? "C"
             : "D";
```

Short nested expressions can be readable when formatted carefully. Use `if-else` when branches contain actions or the nesting becomes difficult to scan.

### Result type

The types of the second and third operands influence the type of the complete expression.

```java
boolean condition = true;
double value = condition ? 10 : 2.5;

System.out.println(value); // 10.0
```

Here the integer branch is promoted so the conditional expression can produce a `double`.

[↑ Go to Table of Contents](#table-of-contents)

## 20. `instanceof` Operator and Pattern Matching

`instanceof` checks whether an object is compatible with a specified reference type.

```java
Object value = "Java";

System.out.println(value instanceof String); // true
System.out.println(value instanceof Integer); // false
```

If the left operand is `null`, the result is always `false`.

```java
Object value = null;
System.out.println(value instanceof String); // false
```

### Traditional type check and cast

```java
if (value instanceof String) {
    String text = (String) value;
    System.out.println(text.length());
}
```

### Pattern matching for `instanceof` — standard since Java 16

Java 16 made type patterns for `instanceof` a permanent feature. The test and cast can be combined:

```java
if (value instanceof String text) {
    System.out.println(text.length());
}
```

The pattern variable is available only where the match is known to have succeeded.

```java
if (value instanceof String text && !text.isBlank()) {
    System.out.println(text.toUpperCase());
}
```

The short-circuit `&&` makes `text` safely available in the right operand.

Java uses **flow scoping**: the pattern variable is in scope only where the compiler can prove the pattern matched.

```java
if (!(value instanceof String text)) {
    return;
}

// The method continued only when the pattern matched.
System.out.println(text.length());
```

This does not work with `||` in the same way because the right operand may execute when the pattern did not match:

```java
// Compile-time error: text is not definitely matched on the right side
// if (value instanceof String text || text.isBlank()) { }
```

### Generics restriction

Because of type erasure, a parameterized type such as `List<String>` generally cannot be the target of an `instanceof` test. Use a reifiable type such as `List<?>`.

```java
if (value instanceof java.util.List<?>) {
    System.out.println("The value is a list");
}
```

Java 21 also permits a **record pattern** as the right operand of `instanceof`, allowing a record to be tested and deconstructed in one expression. See [Section 32](#32-pattern-matching-and-record-patterns-in-java-21).

[↑ Go to Table of Contents](#table-of-contents)

## 21. String Concatenation Operator

When either operand of binary `+` is a `String`, Java performs string concatenation.

```java
String language = "Java";
int version = 21;

String message = language + " " + version;
System.out.println(message); // Java 21
```

Text blocks, permanent since Java 15, are still `String` values and work with the same `+` operator.

```java
String query = """
        SELECT id, name
        FROM customer
        """;

String labelledQuery = "Customer query:\n" + query;
```

### Evaluation is left to right

```java
System.out.println("Result: " + 2 + 3);   // Result: 23
System.out.println("Result: " + (2 + 3)); // Result: 5
System.out.println(2 + 3 + " is the result"); // 5 is the result
```

Once a string participates, later `+` operations concatenate unless parentheses cause a numeric expression to be evaluated first.

### Concatenating `null`

```java
String name = null;
System.out.println("Name: " + name); // Name: null
```

### Repeated concatenation

Simple concatenation is readable and appropriate for ordinary expressions. For repeated updates in a loop, use `StringBuilder`.

```java
StringBuilder builder = new StringBuilder();

for (int i = 1; i <= 3; i++) {
    builder.append(i).append(' ');
}

String result = builder.toString();
```

Java does not allow user-defined operator overloading. String concatenation with `+` is built into the language.

[↑ Go to Table of Contents](#table-of-contents)

## 22. Character Arithmetic

A `char` stores an unsigned UTF-16 code unit. Arithmetic normally promotes it to `int`.

```java
char letter = 'A';
int code = letter;
int nextCode = letter + 1;

System.out.println(code);     // 65
System.out.println(nextCode); // 66
```

An explicit cast is required to convert the arithmetic result back to `char`.

```java
char nextLetter = (char) (letter + 1);
System.out.println(nextLetter); // B
```

Constant expressions that fit the target type can sometimes be assigned without a cast:

```java
char letter = 'A' + 1;
System.out.println(letter); // B
```

This works because `'A' + 1` is a compile-time constant whose value fits in `char`. The same assignment with variables would require a cast.

[↑ Go to Table of Contents](#table-of-contents)

## 23. Primitive and Reference Comparisons

Choosing the correct comparison depends on the operand type.

| Values being compared | Normal approach |
| --- | --- |
| Numeric primitives | `==`, `!=`, `<`, `>`, `<=`, `>=` as appropriate |
| `boolean` primitives | Direct condition, `==`, or `!=` |
| Object identity | `==` or `!=` |
| Object content | `equals()` or a type-specific comparison method |
| Possibly null objects | `java.util.Objects.equals(a, b)` |
| Arrays by content | `Arrays.equals()` or `Arrays.deepEquals()` |

### Null-safe object equality

```java
import java.util.Objects;

String first = null;
String second = null;

System.out.println(Objects.equals(first, second)); // true
```

### Array comparison

```java
import java.util.Arrays;

int[] first = {1, 2, 3};
int[] second = {1, 2, 3};

System.out.println(first == second);            // false
System.out.println(Arrays.equals(first, second)); // true
```

### String pool trap

```java
String first = "Java";
String second = "Java";
String third = new String("Java");

System.out.println(first == second); // often true because literals are interned
System.out.println(first == third);  // false
```

This does not make `==` a valid content comparison. Always use `equals()` when string contents matter.

[↑ Go to Table of Contents](#table-of-contents)

## 24. Operators with Wrapper Classes and Unboxing

Wrapper objects such as `Integer`, `Long`, and `Boolean` can be automatically unboxed when an operator requires a primitive.

```java
Integer first = 10;
Integer second = 20;

int sum = first + second; // both Integer objects are unboxed
```

### Null-unboxing danger

Unboxing a `null` wrapper throws `NullPointerException`.

```java
Integer count = null;

// int next = count + 1; // NullPointerException
```

The same risk exists in conditions:

```java
Boolean enabled = null;

// if (enabled) { } // NullPointerException during unboxing
if (Boolean.TRUE.equals(enabled)) {
    System.out.println("Enabled");
}
```

### Wrapper identity trap

Do not use `==` to compare wrapper values. It compares references unless unboxing is forced, and caching can make results appear inconsistent.

```java
Integer a = 100;
Integer b = 100;
Integer c = 1000;
Integer d = 1000;

System.out.println(a == b); // true for these constant values because of required caching
System.out.println(c == d); // identity is not a value-comparison guarantee
System.out.println(c.equals(d)); // true
```

Use `equals()` for wrapper value equality or explicitly compare primitive values when `null` has been handled.

[↑ Go to Table of Contents](#table-of-contents)

## 25. Overflow, Underflow, and Exact Arithmetic

Integer arithmetic does not automatically report overflow. Results wrap around within the fixed-width two's-complement range.

```java
int maximum = Integer.MAX_VALUE;
int wrapped = maximum + 1;

System.out.println(wrapped); // -2147483648
```

### Exact arithmetic methods

Use `Math` methods when overflow must be detected.

```java
try {
    int result = Math.addExact(Integer.MAX_VALUE, 1);
} catch (ArithmeticException exception) {
    System.out.println("Overflow detected");
}
```

Useful methods include:

| Method | Purpose |
| --- | --- |
| `Math.addExact(a, b)` | Addition with overflow checking |
| `Math.subtractExact(a, b)` | Subtraction with overflow checking |
| `Math.multiplyExact(a, b)` | Multiplication with overflow checking |
| `Math.incrementExact(a)` | Increment with overflow checking |
| `Math.decrementExact(a)` | Decrement with overflow checking |
| `Math.negateExact(a)` | Negation with overflow checking |
| `Math.toIntExact(value)` | Converts `long` to `int` with range checking |

### Floating-point overflow and underflow

Floating-point overflow can produce infinity. Underflow can produce a subnormal value or signed zero.

```java
double overflow = Double.MAX_VALUE * 2;
double underflow = Double.MIN_VALUE / 2;

System.out.println(overflow);  // Infinity
System.out.println(underflow); // 0.0
```

[↑ Go to Table of Contents](#table-of-contents)

## 26. Operator Examples in Conditions

Operators are frequently combined in selection and loop conditions.

### Range validation

```java
int mark = 78;

if (mark >= 0 && mark <= 100) {
    System.out.println("Valid mark");
}
```

### Leap-year check

```java
int year = 2028;

boolean leapYear = year % 400 == 0
        || (year % 4 == 0 && year % 100 != 0);
```

### Even or odd

```java
int number = 27;
String kind = number % 2 == 0 ? "Even" : "Odd";
```

### Access rule

```java
boolean isAdmin = false;
boolean ownsRecord = true;
boolean isLocked = false;

boolean canEdit = (isAdmin || ownsRecord) && !isLocked;
```

Parentheses make the access rule easier to verify, even though precedence would evaluate `&&` before `||`.

[↑ Go to Table of Contents](#table-of-contents)

## 27. Practical Operator Programs

### Program 1: Arithmetic calculator

```java
public class ArithmeticDemo {
    public static void main(String[] args) {
        int first = 20;
        int second = 6;

        System.out.println("Addition: " + (first + second));
        System.out.println("Subtraction: " + (first - second));
        System.out.println("Multiplication: " + (first * second));
        System.out.println("Division: " + (first / second));
        System.out.println("Remainder: " + (first % second));
    }
}
```

Output:

```text
Addition: 26
Subtraction: 14
Multiplication: 120
Division: 3
Remainder: 2
```

### Program 2: Largest of three numbers

```java
public class LargestNumber {
    public static void main(String[] args) {
        int a = 14;
        int b = 25;
        int c = 19;

        int largest = a >= b
                ? (a >= c ? a : c)
                : (b >= c ? b : c);

        System.out.println("Largest: " + largest);
    }
}
```

Output:

```text
Largest: 25
```

### Program 3: Eligibility check

```java
public class EligibilityCheck {
    public static void main(String[] args) {
        int age = 22;
        boolean hasId = true;
        boolean suspended = false;

        boolean eligible = age >= 18 && hasId && !suspended;

        System.out.println("Eligible: " + eligible);
    }
}
```

Output:

```text
Eligible: true
```

### Program 4: Permission flags

```java
public class PermissionFlags {
    private static final int READ = 1;
    private static final int WRITE = 1 << 1;
    private static final int DELETE = 1 << 2;

    public static void main(String[] args) {
        int permissions = READ | WRITE;

        System.out.println("Can read: " + hasPermission(permissions, READ));
        System.out.println("Can delete: " + hasPermission(permissions, DELETE));

        permissions |= DELETE;
        System.out.println("Can delete now: " + hasPermission(permissions, DELETE));
    }

    private static boolean hasPermission(int permissions, int required) {
        return (permissions & required) == required;
    }
}
```

Output:

```text
Can read: true
Can delete: false
Can delete now: true
```

[↑ Go to Table of Contents](#table-of-contents)

## 28. Operators That Java Does Not Support

Java deliberately omits some operators found in other languages.

| Feature | Java status | Alternative |
| --- | --- | --- |
| Exponentiation operator such as `**` | Not supported | `Math.pow(base, exponent)` |
| Unsigned left-shift operator `<<<` | Not supported | `<<` already fills with zeros on the right |
| General user-defined operator overloading | Not supported | Use clearly named methods |
| Comma operator | Not supported as a general expression operator | Use separate statements |
| Pointer dereference operators | Not exposed | Java uses managed references |

The caret `^` means XOR, not exponentiation.

```java
System.out.println(2 ^ 3);           // 1, bitwise XOR
System.out.println(Math.pow(2, 3));  // 8.0
```

The lambda arrow `->` is an operator token in Java 21. The method-reference separator `::`, the member-access separator `.`, and brackets or parentheses are related syntax, but they are not lexical operator tokens.

[↑ Go to Table of Contents](#table-of-contents)

## 29. Complete Java 21 Operator Token List

The Java SE 21 lexical grammar defines exactly **38 operator tokens**:

```text
=   >   <   !   ~   ?   :   ->
==  >=  <=  !=  &&  ||  ++  --
+   -   *   /   &   |   ^   %   <<   >>   >>>
+=  -=  *=  /=  &=  |=  ^=  %=  <<=  >>=  >>>=
```

### Token classification

| Tokens | Use |
| --- | --- |
| `=` | Simple assignment |
| `>`, `<`, `>=`, `<=` | Numeric comparison |
| `!` | Boolean complement |
| `~` | Integral bitwise complement |
| `?`, `:` | Together form the conditional operator `?:` |
| `->` | Lambda body or switch rule |
| `==`, `!=` | Primitive value equality or reference identity |
| `&&`, `||` | Short-circuit boolean operations |
| `++`, `--` | Prefix or postfix increment and decrement |
| `+`, `-`, `*`, `/`, `%` | Arithmetic; `+` also concatenates strings |
| `&`, `|`, `^` | Integral bitwise or non-short-circuit boolean operations |
| `<<`, `>>`, `>>>` | Integral shifts |
| `+=`, `-=`, `*=`, `/=`, `%=` | Arithmetic or concatenation compound assignment |
| `&=`, `|=`, `^=` | Bitwise or boolean compound assignment |
| `<<=`, `>>=`, `>>>=` | Shift compound assignment |

### Important lexical distinctions

- `instanceof` is a keyword that acts as a relational operator.
- `::` is a separator used in method-reference expressions.
- `.`, `(`, `)`, `[`, and `]` are separators used in tightly binding primary expressions.
- A cast such as `(long) value` is a cast expression, not one of the 38 operator tokens.
- `new` is a keyword used by class and array creation expressions, not an operator token.
- `&` is also reused in intersection types, and `|` is reused in multi-catch types; those appearances are type syntax rather than evaluated bitwise operations.
- In a nested generic type such as `List<List<String>>`, Java treats adjacent `>` characters as separate closing tokens in the type context rather than as a shift operator.

[↑ Go to Table of Contents](#table-of-contents)

## 30. Lambda Expressions and Method References

Lambda expressions and method references became permanent in Java 8. They are closely related to operators because the Java 21 lexical grammar classifies `->` as an operator token and because both forms participate in expression typing.

### Lambda arrow `->`

A lambda expression supplies an implementation of a functional interface.

```java
import java.util.function.IntBinaryOperator;
import java.util.function.Predicate;

IntBinaryOperator add = (left, right) -> left + right;
Predicate<String> nonBlank = text -> text != null && !text.isBlank();

System.out.println(add.applyAsInt(4, 6)); // 10
System.out.println(nonBlank.test("Java")); // true
```

The lambda body can be one expression or a block.

```java
IntBinaryOperator maximum = (left, right) -> {
    int result = left >= right ? left : right;
    return result;
};
```

The lambda arrow has the lowest expression precedence. Operators inside an expression body therefore belong to that body.

```java
java.util.function.Function<Integer, String> sign =
        number -> number >= 0 ? "non-negative" : "negative";
```

### Target typing and poly expressions

A lambda has no standalone object type. Its target must be a compatible functional interface supplied by assignment, invocation, or casting context.

```java
java.util.function.IntUnaryOperator square = value -> value * value;

// var square = value -> value * value; // error: no target type
```

### Captured local variables

A lambda may read a local variable only when that variable is `final` or effectively final.

```java
int factor = 3;
java.util.function.IntUnaryOperator scale = value -> value * factor;

// factor++; // would make factor non-effectively-final
```

### `var` in lambda parameters — Java 11+

Java 11 permits `var` in an implicitly typed lambda parameter list. If one parameter uses `var`, they all must use it.

```java
java.util.function.BinaryOperator<Integer> larger =
        (var left, var right) -> left >= right ? left : right;
```

This syntax is especially useful when parameter annotations are required.

### Method-reference separator `::`

A method reference identifies behavior without invoking it immediately. It receives its meaning from a target functional interface.

| Form | Example | Meaning |
| --- | --- | --- |
| Static method | `Integer::parseInt` | Call a static method |
| Bound instance method | `prefix::concat` | Call a method on one existing object |
| Unbound instance method | `String::length` | Call a method on the supplied receiver |
| Constructor | `ArrayList::new` | Create an object |
| Array constructor | `String[]::new` | Create an array |

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Function;
import java.util.function.IntFunction;
import java.util.function.Supplier;

Function<String, Integer> length = String::length;
Supplier<List<String>> listFactory = ArrayList::new;
IntFunction<String[]> arrayFactory = String[]::new;
```

`::` is formally a separator, not one of Java 21's 38 operator tokens. Method references are always poly expressions, so overload resolution and the target functional interface determine the referenced method.

[↑ Go to Table of Contents](#table-of-contents)

## 31. Switch Expressions Through Java 21

Java 14 made switch expressions permanent. A `switch` expression selects and produces a value, so it can appear inside assignments, method arguments, returns, conditional expressions, and other larger expressions.

### Arrow rules

```java
int day = 6;

String kind = switch (day) {
    case 1, 2, 3, 4, 5 -> "Weekday";
    case 6, 7 -> "Weekend";
    default -> throw new IllegalArgumentException("Invalid day: " + day);
};
```

An arrow rule does not fall through to the next rule. Multiple constants can share one rule by using commas.

### Block rule and `yield`

Use a block when a rule requires multiple statements. `yield` supplies that block's result to the enclosing switch expression.

```java
int score = 82;

String grade = switch (score / 10) {
    case 10, 9 -> "A";
    case 8 -> {
        System.out.println("Good work");
        yield "B";
    }
    case 7 -> "C";
    case 6 -> "D";
    default -> "F";
};
```

`yield` is not an operator. It is a restricted identifier used by a statement within a switch-expression block.

### Exhaustiveness

Every switch expression must be exhaustive: all possible selector values must have a result or throw an exception. A `default` rule is often necessary. An enum listing all constants and an eligible sealed hierarchy can be exhaustive without an explicit `default`.

### Target typing

A switch expression can be a poly expression. Its surrounding target type can influence the types expected from its result expressions.

```java
boolean wholeNumber = false;
Number value = switch (wholeNumber ? 1 : 2) {
    case 1 -> 10;
    default -> 2.5;
};
```

### Selector types in Java 21

Classic switch supports `char`, `byte`, `short`, and `int`, their wrapper types, `String`, and enum types. Java 21 pattern matching expands switch to selector expressions of any reference type. Java 21 does not support `boolean`, `long`, `float`, or `double` as switch selector types.

Pattern labels, `case null`, guarded cases, and record patterns are covered in the next section.

[↑ Go to Table of Contents](#table-of-contents)

## 32. Pattern Matching and Record Patterns in Java 21

Java 21 permanently added both **pattern matching for `switch`** and **record patterns**. These are standard Java 21 features and do not require `--enable-preview`.

### Type patterns in switch

```java
static String describe(Object value) {
    return switch (value) {
        case null -> "null";
        case Integer number -> "integer " + number;
        case String text -> "text of length " + text.length();
        default -> "other type";
    };
}
```

A pattern both tests the value and introduces a variable whose type is narrowed safely.

### Guarded patterns with `when`

A `when` guard adds a boolean condition after a pattern matches.

```java
static String classify(Object value) {
    return switch (value) {
        case String text when text.isBlank() -> "blank text";
        case String text when text.length() > 20 -> "long text";
        case String text -> "other text";
        case null -> "null";
        default -> "not text";
    };
}
```

Specific or guarded cases must appear before broader unguarded cases. Otherwise, the later case is dominated and the compiler rejects it.

### Explicit null handling

Type and record patterns do not match `null`. Java 21 permits `case null` when null is an expected selector value.

```java
String result = switch (value) {
    case null -> "missing";
    case String text -> text;
    default -> value.toString();
};
```

Without an applicable `case null`, switching on a null selector normally throws `NullPointerException`; a plain `default` does not make type patterns match null.

### Record patterns

A record pattern tests the record type and extracts its components.

```java
record Point(int x, int y) {}

static String location(Object value) {
    if (value instanceof Point(int x, int y)) {
        return "(" + x + ", " + y + ")";
    }
    return "not a point";
}
```

The record component patterns may use `var` when the component types should be inferred.

```java
if (value instanceof Point(var x, var y)) {
    System.out.println(x + y);
}
```

### Nested record patterns

Record patterns compose recursively.

```java
record Line(Point start, Point end) {}

static int horizontalLength(Object value) {
    return switch (value) {
        case Line(Point(var x1, var y1), Point(var x2, var y2))
                when y1 == y2 -> Math.abs(x2 - x1);
        case Line ignored -> 0;
        default -> -1;
    };
}
```

### Exhaustive switch over a sealed hierarchy

```java
sealed interface Shape permits Circle, Rectangle {}
record Circle(double radius) implements Shape {}
record Rectangle(double width, double height) implements Shape {}

static double area(Shape shape) {
    return switch (shape) {
        case Circle(double radius) -> Math.PI * radius * radius;
        case Rectangle(double width, double height) -> width * height;
    };
}
```

Because every permitted implementation of `Shape` is covered, the switch expression is exhaustive without a source-level `default`. Passing `null` still throws `NullPointerException` unless a `case null` rule is included.

### Generic record patterns

Java 21 can infer generic record-pattern type arguments when the selector's static type provides enough information.

```java
record Box<T>(T value) {}

static void printBox(Box<String> box) {
    if (box instanceof Box(var text)) {
        System.out.println(text.toUpperCase());
    }
}
```

Record patterns do not bypass type erasure. The required cast to a parameterized record type must still be reifiable or otherwise safely checkable.

[↑ Go to Table of Contents](#table-of-contents)

## 33. Java 21 Preview Features Related to Expressions

Java 21 also included preview language features. Preview features are intentionally separate from permanent Java SE 21 features: they require special compiler and runtime flags and may change or disappear in later releases.

### Enabling Java 21 preview features

```bash
javac --enable-preview --release 21 Example.java
java --enable-preview Example
```

The compiler and runtime must both be JDK 21 when running class files that use Java 21 preview features.

### String templates — Java 21 preview

String templates combined literal text, embedded expressions, and a template processor. The Java 21 preview syntax used `\{...}` for an embedded expression.

```java
int quantity = 3;
double price = 25.0;

String message = STR."Total: \{quantity * price}";
```

This is **not standard Java 21 syntax** without preview mode. Use ordinary concatenation, `String.format()`, or another formatting API when preview features are not enabled.

### Unnamed patterns and variables — Java 21 preview

The underscore `_` could mark a pattern component or variable whose value was intentionally unused.

```java
record Point(int x, int y) {}

if (value instanceof Point(int x, _)) {
    System.out.println("x = " + x);
}
```

This syntax also requires Java 21 preview mode. In permanent Java 21 code, give the component a legal name even when it is unused.

### Unnamed classes and instance main methods — Java 21 preview

Java 21 previewed simplified source files for small programs. This feature does not add an operator, but it can make beginner operator examples shorter. It still requires preview mode.

```java
void main() {
    int result = 10 + 20;
    System.out.println(result);
}
```

For ordinary Java 21 source, continue to use a named class and `public static void main(String[] args)`.

[↑ Go to Table of Contents](#table-of-contents)

## 34. Java Version Timeline for Operators and Expressions

The core arithmetic, comparison, logical, bitwise, shift, conditional, assignment, and `instanceof` operators predate modern Java releases. Later releases mainly added new expression forms, pattern capabilities, and related syntax.

| Java release | Operator or expression-related change relevant to this chapter |
| ---: | --- |
| Java 5 | Autoboxing and unboxing allowed wrapper values to participate more naturally in operator expressions |
| Java 7 | Binary integer literals and numeric-literal underscores improved bit-oriented code; multi-catch reused `|` as type syntax |
| Java 8 | Lambda expressions added `->`; method references added `::`; unsigned integer utility methods were added |
| Java 10 | `var` added local-variable type inference for initialized local declarations |
| Java 11 | `var` became legal in implicitly typed lambda parameter lists |
| Java 14 | Switch expressions and arrow switch rules became permanent; `yield` supplies a value from a block rule |
| Java 15 | Text blocks became permanent `String` literals and work with ordinary string concatenation |
| Java 16 | Pattern matching for `instanceof` became permanent |
| Java 17 | Floating-point evaluation became always strict; sealed classes became permanent and support exhaustive pattern reasoning |
| Java 21 | Record patterns and pattern matching for switch became permanent |
| Java 21 preview | String templates, unnamed patterns and variables, and unnamed classes with instance main methods were preview features |

### Java 21 baseline summary

- Every ordinary operator example in this chapter is valid for Java 21.
- Sections using switch expressions require Java 14 or later.
- Type patterns for `instanceof` require Java 16 or later.
- Record patterns, `when` guards, and final pattern-switch syntax require Java 21.
- Examples in Section 33 compile only with Java 21 preview mode.
- Java 21 introduced no new primitive arithmetic, equality, bitwise, or shift operator symbol.

[↑ Go to Table of Contents](#table-of-contents)

## 35. Operators Best Practices

- Use parentheses to communicate intent in mixed expressions.
- Use `&&` and `||` for normal boolean conditions so unnecessary work is skipped.
- Put null or validity checks before dependent operations in a short-circuit expression.
- Use `equals()` or `Objects.equals()` for object content comparison.
- Compare strings by content rather than by reference identity.
- Convert an operand before arithmetic when a wider result type is required.
- Use `BigDecimal` when controlled decimal precision is required.
- Use `Math.addExact()`, `Math.multiplyExact()`, and related methods when overflow must be detected.
- Keep increment, decrement, and assignment side effects out of complicated expressions.
- Use the ternary operator for short value selection, not for multi-step logic.
- Use named constants for bit masks.
- Add parentheses around bit-mask tests, such as `(flags & READ) != 0`.
- Remember that compound assignment can perform an implicit narrowing conversion.
- Do not replace clear multiplication or division blindly with shifts.
- Use arrow switch rules to avoid accidental fall-through when a switch returns a value.
- Keep switch expressions exhaustive, and handle `null` explicitly when it is a valid input.
- Place guarded or specific pattern cases before broad, unguarded cases.
- Use record patterns when direct deconstruction improves clarity; keep named accessors when the pattern becomes overly nested.
- Give every lambda an obvious functional-interface target type.
- Treat Java 21 preview examples as experimental and compile them only with matching preview flags.
- Prefer readable expressions over clever expressions.

[↑ Go to Table of Contents](#table-of-contents)

## 36. Common Operator Errors

### Error 1: Expecting decimal integer division

```java
double average = 5 / 2; // 2.0, not 2.5
```

Fix:

```java
double average = 5.0 / 2;
```

### Error 2: Comparing strings with `==`

```java
String first = new String("Java");
String second = new String("Java");

if (first == second) { // compares identity
    System.out.println("Equal");
}
```

Fix:

```java
if (first.equals(second)) {
    System.out.println("Equal");
}
```

### Error 3: Checking for null too late

```java
// if (name.length() > 0 && name != null) { }
```

Fix:

```java
if (name != null && !name.isEmpty()) {
    System.out.println(name);
}
```

### Error 4: Overflow before assigning to `long`

```java
int value = 1_000_000;
long result = value * value; // int multiplication overflows
```

Fix:

```java
long result = (long) value * value;
```

### Error 5: Forgetting binary numeric promotion

```java
byte a = 10;
byte b = 20;

// byte sum = a + b; // result is int
int sum = a + b;
```

### Error 6: Assuming `+=` is exactly textual shorthand

```java
short value = 10;
value += 5; // implicit conversion to short

// value = value + 5; // compile-time error without a cast
```

### Error 7: Confusing `&` with `&&`

```java
if (object != null & object.isReady()) {
    // The right operand still runs when object is null.
}
```

Fix:

```java
if (object != null && object.isReady()) {
    // The right operand runs only when object is non-null.
}
```

### Error 8: Treating `%` as always non-negative

```java
System.out.println(-5 % 3); // -2
```

Use `Math.floorMod(-5, 3)` when the required result is within `0` to `divisor - 1` for a positive divisor.

### Error 9: Using `^` for powers

```java
int wrong = 2 ^ 3; // XOR, result is 1
double correct = Math.pow(2, 3);
```

### Error 10: Unboxing `null`

```java
Integer value = null;
// int result = value + 1; // NullPointerException
```

Validate the wrapper or supply an intentional default before arithmetic.

### Error 11: Relying on precedence in a complex rule

```java
boolean permitted = admin || owner && active;
```

This is grouped as `admin || (owner && active)`. Add parentheses that match the business rule:

```java
boolean permitted = admin || (owner && active);
```

or:

```java
boolean permitted = (admin || owner) && active;
```

### Error 12: Assuming a shift distance is unrestricted

```java
int result = 1 << 32; // result is 1, not 0
```

For `int`, only the lowest five bits of the distance are used.

### Error 13: Using a complicated increment expression

```java
int i = 3;
int result = i++ + ++i;
```

Java defines the evaluation, but a reader must mentally track several state changes. Split the work into named steps.

### Error 14: Using a pattern variable outside its flow scope

```java
if (value instanceof String text) {
    System.out.println(text.length());
}

// System.out.println(text); // compile-time error: text is out of scope
```

Keep the use inside a region where the compiler knows the match succeeded, or restructure with an early return.

### Error 15: Placing a broad switch pattern first

```java
// Compile-time error: String case is dominated by Object case
String result = switch (value) {
    case Object object -> "object";
    case String text -> "text";
};
```

Fix:

```java
String result = switch (value) {
    case String text -> "text";
    case Object object -> "object";
};
```

### Error 16: Assuming `default` always handles null in a pattern switch

```java
String result = switch (value) {
    case String text -> text;
    default -> "other";
};
```

If `value` is `null`, this normally throws `NullPointerException`. Add `case null` when null is part of the input domain.

### Error 17: Forgetting that a switch expression must be exhaustive

```java
// Compile-time error when not every possible int value is covered
// String label = switch (number) {
//     case 1 -> "one";
// };
```

Add a suitable `default`, cover every enum constant, or cover every permitted type of an eligible sealed hierarchy.

### Error 18: Using `break` instead of `yield` for a switch-expression value

```java
String result = switch (number) {
    case 1 -> {
        System.out.println("one");
        yield "ONE";
    }
    default -> "OTHER";
};
```

An arrow expression returns its expression directly. A multi-statement block uses `yield`, not `break value`.

### Error 19: Giving a lambda no target type

```java
// var square = value -> value * value; // compile-time error

java.util.function.IntUnaryOperator square = value -> value * value;
```

Lambdas and method references need a compatible functional-interface target.

### Error 20: Mixing `var` and ordinary lambda parameter syntax

```java
// Invalid: all parameters must use the same declaration style
// java.util.function.BinaryOperator<Integer> add =
//         (var left, right) -> left + right;
```

Use `(var left, var right)`, `(left, right)`, or explicit types for both parameters.

### Error 21: Compiling Java 21 preview syntax without preview mode

```java
// Java 21 preview syntax; ordinary javac --release 21 is insufficient
// String message = STR."Total: \{total}";
```

Compile with `javac --enable-preview --release 21` and run with `java --enable-preview`, or replace the preview syntax with permanent Java 21 syntax.

[↑ Go to Table of Contents](#table-of-contents)

## 37. Quick Revision Tables

### Arithmetic and assignment

| Purpose | Operators |
| --- | --- |
| Basic arithmetic | `+`, `-`, `*`, `/`, `%` |
| Change by one | `++`, `--` |
| Assign | `=` |
| Calculate and assign | `+=`, `-=`, `*=`, `/=`, `%=` |

### Comparison and logic

| Purpose | Operators |
| --- | --- |
| Numeric ordering | `<`, `>`, `<=`, `>=` |
| Equality or inequality | `==`, `!=` |
| Short-circuit logic | `&&`, `||` |
| Boolean negation | `!` |
| Boolean non-short-circuit logic | `&`, `|`, `^` |

### Bits, selection, and types

| Purpose | Operators |
| --- | --- |
| Bitwise operations | `&`, `|`, `^`, `~` |
| Bit shifts | `<<`, `>>`, `>>>` |
| Select one of two values | `?:` |
| Test reference-type compatibility | `instanceof` |
| Join strings | `+` |

### Key rules to remember

| Situation | Rule |
| --- | --- |
| `int / int` | Produces an `int`; fractional part is discarded |
| `byte + byte` | Produces an `int` |
| `int * int` assigned to `long` | Multiplication still occurs as `int` |
| `object1 == object2` | Compares reference identity |
| `object1.equals(object2)` | Normally compares logical content when implemented |
| `false && expression` | Skips `expression` |
| `true || expression` | Skips `expression` |
| `x += y` | Includes conversion back to the type of `x` |
| `null instanceof Type` | Always `false` |
| Integer overflow | Wraps unless an exact method is used |

### Version-specific expression features available in Java 21

| Feature | Permanent since | Key syntax |
| --- | ---: | --- |
| Lambda expression | Java 8 | `(x, y) -> x + y` |
| Method reference | Java 8 | `String::length` |
| `var` lambda parameters | Java 11 | `(var x, var y) -> x + y` |
| Switch expression | Java 14 | `switch (x) { case 1 -> "one"; default -> "other"; }` |
| Pattern `instanceof` | Java 16 | `value instanceof String text` |
| Record pattern | Java 21 | `value instanceof Point(int x, int y)` |
| Pattern switch | Java 21 | `case String text when !text.isBlank() -> ...` |
| Java 21 preview syntax | Preview only | Requires `--enable-preview` |

[↑ Go to Table of Contents](#table-of-contents)

## 38. Frequently Asked Interview Questions

> ### Fundamentals

### 1. What is an operator in Java?

An operator is a symbol that performs an operation on one, two, or three operands and produces a value or side effect.

### 2. What is an operand?

An operand is a value or expression on which an operator acts. In `a + b`, `a` and `b` are operands.

### 3. What is the difference between an expression and a statement?

An expression evaluates to a value. A statement is a complete instruction, such as a declaration, assignment, method call, or control-flow statement.

### 4. What are unary, binary, and ternary operators?

A unary operator takes one operand, a binary operator takes two, and a ternary operator takes three. Java's only ternary operator is `?:`.

### 5. Can the same operator have multiple meanings?

Yes. For example, `+` performs numeric addition, unary plus, or string concatenation depending on its position and operands.

### 6. What is operator precedence?

Precedence determines how operators of different levels are grouped when parentheses are absent. For example, multiplication binds more tightly than addition.

### 7. What is associativity?

Associativity determines grouping when adjacent operators have the same precedence. Most binary arithmetic operators associate left to right, while assignment and the conditional operator associate right to left.

### 8. Does Java specify expression evaluation order?

Yes. Java evaluates operand expressions from left to right, including method arguments. Precedence controls grouping, not an arbitrary reordering of operand evaluation.

> ### Arithmetic and Promotion

### 9. What does `10 / 3` produce?

It produces the integer value `3` because both operands are integers.

### 10. How can integer operands produce a decimal division result?

Convert at least one operand to `float` or `double`, such as `(double) total / count`.

### 11. What is the difference between remainder and mathematical modulus?

Java's `%` follows truncated division, so a nonzero result has the dividend's sign. `Math.floorMod()` is useful when a non-negative modulus is required with a positive divisor.

### 12. What happens during integer division by zero?

It throws `ArithmeticException` at runtime. A constant expression that divides an integer by zero is a compile-time error.

### 13. What happens during floating-point division by zero?

It produces infinity, negative infinity, or `NaN`, depending on the operands; it does not throw `ArithmeticException`.

### 14. Why does `byte + byte` produce `int`?

Binary numeric promotion converts `byte`, `short`, and `char` operands to `int` for arithmetic.

### 15. Why can `long result = intValue * intValue` overflow?

Both multiplication operands are `int`, so multiplication occurs as `int`. Assignment to `long` happens only afterward. Cast an operand to `long` first.

### 16. Does Java detect integer overflow automatically?

Ordinary integer operators do not. The result wraps within the type's range. Use methods such as `Math.addExact()` when overflow must be detected.

### 17. Why is `0.1 + 0.2` not exactly `0.3` as a `double`?

Most decimal fractions cannot be represented exactly in binary floating-point, so calculations may include a small rounding error.

### 18. When should `BigDecimal` be used?

Use it when controlled decimal precision and rounding are required, particularly for many monetary calculations.

> ### Increment and Assignment

### 19. What is the difference between `++x` and `x++`?

Both increment `x`. `++x` produces the updated value, while `x++` produces the old value before updating the variable.

### 20. Is `x += y` always exactly the same as `x = x + y`?

No. Compound assignment evaluates its left side once and implicitly converts the result back to the left side's type.

### 21. Why does `byteValue += 1` compile but `byteValue = byteValue + 1` fail?

The compound form includes an implicit conversion back to `byte`. The expanded arithmetic expression produces `int` and therefore needs an explicit cast.

### 22. Can an assignment be used as an expression?

Yes. Assignment produces the assigned value, which makes chained assignments possible. Embedding assignments in conditions is usually best avoided because it is easy to misread.

### 23. In which direction are chained assignments grouped?

They group from right to left. `a = b = 5` is grouped as `a = (b = 5)`.

> ### Comparison and Logic

### 24. What is the difference between `==` and `equals()` for objects?

`==` tests reference identity. `equals()` normally tests logical equality according to the class's implementation.

### 25. How should strings be compared?

Use `equals()` for case-sensitive content comparison or `equalsIgnoreCase()` when that behavior is specifically required. Use `Objects.equals()` for null-safe equality.

### 26. How should a reference be compared with `null`?

Use `== null` or `!= null`.

### 27. What is short-circuit evaluation?

It means `&&` skips its right operand when the left operand is false, while `||` skips its right operand when the left operand is true.

### 28. What is the difference between `&&` and `&` with booleans?

Both perform AND, but `&&` short-circuits. Boolean `&` always evaluates both operands.

### 29. What is the difference between `||` and `|` with booleans?

Both perform OR, but `||` short-circuits. Boolean `|` always evaluates both operands.

### 30. What does boolean XOR do?

`a ^ b` is true when exactly one operand is true and false when the operands are equal.

### 31. Why is `name != null && !name.isEmpty()` safe?

If `name` is null, `&&` skips the second operand, so `isEmpty()` is not invoked.

### 32. Can Java use `18 <= age <= 60`?

No. Write `age >= 18 && age <= 60`.

> ### Bits, Shifts, and Selection

### 33. What is the difference between bitwise and logical operators?

Bitwise operators manipulate bits of integral values. Several of the same symbols—`&`, `|`, and `^`—can also combine booleans without short-circuiting.

### 34. What does the `~` operator do?

It flips every bit of an integral value. After numeric promotion, `~x` is equivalent to `-x - 1` in two's-complement arithmetic.

### 35. What is the difference between `>>` and `>>>`?

`>>` copies the sign bit into vacated positions. `>>>` fills vacated high-order positions with zeros.

### 36. Is there a `<<<` operator in Java?

No. Left shift `<<` always fills vacated low-order positions with zeros.

### 37. What happens when an `int` is shifted by 32 positions?

Only the lowest five bits of the distance are used, so shifting an `int` by 32 is equivalent to shifting it by zero.

### 38. What is the conditional operator?

`condition ? first : second` evaluates the condition and then evaluates only one of the two result expressions.

### 39. When is the ternary operator preferable to `if-else`?

It is useful for a short expression that selects one of two values. `if-else` is clearer for multi-step behavior or complex branches.

### 40. Is `^` an exponentiation operator?

No. It is XOR. Use `Math.pow()` for exponentiation.

> ### Types, Strings, and Common Traps

### 41. What does `instanceof` do?

It tests whether a non-null object is compatible with a specified reference type.

### 42. What is the result of `null instanceof String`?

It is `false`.

### 43. What advantage does pattern matching for `instanceof` provide?

It combines the type test with a safely scoped variable, avoiding a separate explicit cast.

### 44. What does `"Result: " + 2 + 3` produce?

It produces `"Result: 23"` because operations proceed left to right and string concatenation has already begun.

### 45. How can the previous expression produce `"Result: 5"`?

Use parentheses around the numeric addition: `"Result: " + (2 + 3)`.

### 46. Does Java support user-defined operator overloading?

No. Classes define methods instead. String concatenation with `+` is language-defined behavior, not user-defined overloading.

### 47. Why can arithmetic with a wrapper object throw `NullPointerException`?

The operator requires Java to unbox the wrapper. Unboxing a `null` reference throws `NullPointerException`.

### 48. Why should `Integer` values not be compared with `==`?

`==` can compare wrapper references, and caching may make some values appear equal by identity. Use `equals()` for value equality after handling `null`.

### 49. How should floating-point calculated values be compared?

Use a tolerance appropriate to the domain, or use a decimal or exact representation when exact equality is a requirement.

### 50. What operator rules should a fresher remember?

- Integer division discards the fractional part.
- Smaller integral values are commonly promoted to `int`.
- Arithmetic happens before conversion to the assignment target.
- `==` compares object identity; `equals()` compares content when defined that way.
- `&&` and `||` short-circuit.
- Compound assignment includes an implicit conversion.
- `%` can produce a negative result.
- `^` means XOR, not power.
- Integer overflow wraps unless checked explicitly.
- Parentheses make mixed expressions easier to verify.

[↑ Go to Table of Contents](#table-of-contents)

> ### Java 21 Coverage Questions

### 51. How many lexical operator tokens does Java 21 define?

Java SE 21 defines 38 operator tokens. The list includes the lambda and switch arrow `->`.

### 52. Is `instanceof` one of the 38 operator tokens?

No. `instanceof` is lexically a keyword, but it behaves as a relational operator in expressions.

### 53. Is `::` an operator in Java 21?

Formally, no. The lexical grammar classifies `::` as a separator used by method-reference expressions.

### 54. What is Java's lowest-precedence expression operator?

The arrow of a lambda expression, `->`, has lower precedence than assignment operators.

### 55. When did switch expressions become permanent?

They became a permanent Java language feature in Java 14.

### 56. What is the difference between `->` and `yield` in a switch expression?

An arrow introduces a switch rule. A rule containing one expression produces that value directly; a multi-statement block uses `yield` to provide its value.

### 57. Do arrow switch rules fall through?

No. An arrow rule selects only its own expression, block, or `throw` statement.

### 58. Why must a switch expression be exhaustive?

It must produce a value or complete abruptly for every possible selector value. Exhaustiveness prevents a path with no result.

### 59. Which types cannot be switch selectors in Java 21?

`boolean`, `long`, `float`, and `double` cannot be switch selector types. Pattern matching otherwise permits reference-type selectors, while classic switch supports its traditional integral, wrapper, string, and enum types.

### 60. When did pattern matching for `instanceof` become permanent?

It became permanent in Java 16.

### 61. What is flow scoping for a pattern variable?

The variable is in scope only where the compiler can prove the pattern matched. This allows use after a successful `&&` condition or after an unmatched path returns.

### 62. Are record patterns permanent in Java 21?

Yes. Record patterns became a permanent feature in Java 21 and do not require preview flags.

### 63. Is pattern matching for switch permanent in Java 21?

Yes. Type and record patterns in switch expressions and statements are permanent in Java 21.

### 64. What does a `when` guard do?

It applies an additional boolean test after a case pattern matches, such as `case String text when text.isBlank() -> ...`.

### 65. What is pattern dominance?

A case is dominated when an earlier case already matches every value it could match. Java rejects dominated switch labels at compile time.

### 66. How does a Java 21 pattern switch handle `null`?

Use `case null` to handle it explicitly. Type and record patterns do not normally match null, and switching on null without an applicable null label throws `NullPointerException`.

### 67. How can a sealed hierarchy make a switch exhaustive?

If the switch covers every permitted direct subtype relevant to the selector type, the compiler can accept it without a source-level `default`.

### 68. What changed for floating-point operators in Java 17?

Floating-point evaluation became always strict. In Java 21, `strictfp` no longer changes evaluation and is obsolete.

### 69. Which Java 21 expression-related features were preview features?

String templates and unnamed patterns or variables were relevant preview features. Unnamed classes and instance main methods were also previewed for simpler source files.

### 70. Are Java 21 preview features enabled by default?

No. Compile with `javac --enable-preview --release 21` and run with `java --enable-preview` on JDK 21.

### 71. Are Java 21 string templates ordinary permanent Java 21 syntax?

No. They were preview syntax in Java 21. Permanent Java 21 code should use concatenation or formatting APIs unless preview mode is intentionally enabled.

### 72. Is `_` an ordinary variable name in Java 21?

No. A single underscore has been reserved since Java 9. Its use for unnamed patterns and variables was preview-only in Java 21.

### 73. Did Java 21 add a new arithmetic, equality, bitwise, or shift symbol?

No. Java 21's permanent expression changes focused on record patterns and pattern matching for switch, not new primitive arithmetic operator symbols.

### 74. Which examples in this chapter require Java 21 specifically?

Record patterns, final pattern-switch syntax, `when` guards, and the Java 21 preview examples require JDK 21. Earlier core-operator examples work on older releases as indicated in the version timeline.

[↑ Go to Table of Contents](#table-of-contents)

## 39. Official Java 21 References

- [Java Language Specification, Java SE 21 — Chapter 3: Lexical Structure](https://docs.oracle.com/javase/specs/jls/se21/html/jls-3.html)
- [Java Language Specification, Java SE 21 — Chapter 14: Blocks, Statements, and Patterns](https://docs.oracle.com/javase/specs/jls/se21/html/jls-14.html)
- [Java Language Specification, Java SE 21 — Chapter 15: Expressions](https://docs.oracle.com/javase/specs/jls/se21/html/jls-15.html)
- [Oracle Java SE 21 Language Changes](https://docs.oracle.com/en/java/javase/21/language/java-language-changes-release.html)
- [Oracle Java 21 Record Patterns Guide](https://docs.oracle.com/en/java/javase/21/language/record-patterns.html)
- [Oracle Java 21 Pattern Matching for switch Guide](https://docs.oracle.com/en/java/javase/21/language/pattern-matching-switch.html)
- [JEP 361: Switch Expressions](https://openjdk.org/jeps/361)
- [JEP 394: Pattern Matching for `instanceof`](https://openjdk.org/jeps/394)
- [JEP 440: Record Patterns](https://openjdk.org/jeps/440)
- [JEP 441: Pattern Matching for `switch`](https://openjdk.org/jeps/441)
- [Java 21 String Templates Preview Specification](https://docs.oracle.com/javase/specs/jls/se21/preview/specs/string-templates-jls.html)
- [Java 21 Unnamed Patterns and Variables Preview Specification](https://docs.oracle.com/javase/specs/jls/se21/preview/specs/unnamed-jls.html)

[↑ Go to Table of Contents](#table-of-contents)

---

🏚️ [Home](index.md) 🔸 ⬅️ Previous: [Variables and Data Types](variables.md) 🔸 ➡️ Next: [Control Flow Statements](control-flow.md)

<!-- Mermaid rendering support for GitHub Pages/Jekyll. -->
<script type="module">
  import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";

  document.querySelectorAll("pre > code.language-mermaid").forEach((code) => {
    const diagram = document.createElement("pre");
    diagram.className = "mermaid";
    diagram.textContent = code.textContent;
    code.parentElement.replaceWith(diagram);
  });

  mermaid.initialize({
    startOnLoad: false,
    securityLevel: "strict"
  });

  await mermaid.run({ querySelector: ".mermaid" });
</script>
