
# Basic Tutorial

### compile

compile= 프로그래밍 언어로 작성된 코드를 컴퓨터가 이해하고 실행할 수 있는 기계어로 변환하는 과정
그래서 `javac Main.java`를 하면 `Main.Class`라는 새로운 파일이 나온다.

### class

Every line of code that runs in Java must be inside a `class`.
The name of the java file **must match** the class name.
The `main()` method is required and you will see it in every Java program

### How to print

- `println()` method is similar to `Console.WriteLine()` in `C#`
- `print()` method is similar to `Console.Write()`in `C#`.
- When you are working with text, it must be wrapped inside double quotations marks `""`.
- When you are working with `char`, it must ne wrapped inside single quotation marks `''`.

- Multi-line comments start with `/*` and ends with `*/`.
- Any text between `/*` and `*/` will be ignored by Java.

### How to end a code statement

each code statement must end with a semicolon (`;`)


### How to run a file

Whenever you modify the file, you should command `javac Main.java` to run the updated version.

- When you are working with text, it must be wrapped inside double quotations marks `""`.

### Identifiers

- All Java **variables** must be **identified** with **unique names**.
- These unique names are called **identifiers**.
- Identifiers can be short names (like x and y) or more descriptive names (age, sum, totalVolume).
- **Note:** It is recommended to use descriptive names in order to create understandable and maintainable code

## Data types of Java


### Primitive variables vs non-primitive variables


The main difference between **primitive** and **non-primitive** data types are:

- Primitive types are **predefined** (already defined) in Java. Non-primitive types are created by the programmer and is not defined by Java (except for `String`).
- **Non-primitive types can be used to call methods to perform certain operations**, while primitive types cannot.
- A primitive type has always a value, while non-primitive types can be `null`.
- A** primitive type starts with a lowercase letter**, while non-primitive types starts with an uppercase letter.



### Primitive variables

#### Numeric variables

`byte`: -128~127 
`short`:  -32768 to 32767
`int`: -2147483648 to 2147483647
`long`: -9223372036854775808 to 9223372036854775807 
	long을 쓸때는 숫자뒤에 L을 붙임

작을수록 메모리를 적게 쓴다는 장점이 있음.

**float vs double**

double: 더 정확함(15자리)
float: 덜 정확함(8자리)

### Non-primitive variables



#### String

- `char` - stores **single characters**, such as 'a' or 'B'. **Char values are surrounded by single quotes**
- `String` - stores text, such as "Hello". String values are **surrounded by double quotes**

Methods of String: https://www.w3schools.com/java/java_ref_string.asp



## Loops

### if 

Java에서는 elif 대신 else if 라고 쓴다


`Java Switch`

- The `switch` expression is evaluated once.
- The value of the expression is compared with the values of each `case`.
- If there is a match, the associated block of code is executed.
- The `break` and `default` keywords are optional



When Java reaches a `break` keyword, it breaks out of the switch block.

This will stop the execution of more code and case testing inside the block.

When a match is found, and the job is done, it's time for a break. There is no need for more testing.


```java
int day = 4;
switch (day) {
  case 1:
    System.out.println("Monday");
    break;
  case 2:
    System.out.println("Tuesday");
    break;
  case 3:
    System.out.println("Wednesday");
    break;
  case 4:
    System.out.println("Thursday");
    break;
  case 5:
    System.out.println("Friday");
    break;
  case 6:
    System.out.println("Saturday");
    break;
  case 7:
    System.out.println("Sunday");
    break;
}
// Outputs "Thursday" (day 4)
```


### While

The `do/while` loop is a variant of the `while` loop.

The example below uses a `do/while` loop. **The loop will always be executed at least once, even if the condition is false**, because the code block is executed before the condition is tested


### For

#### for-each loop (exclusively for arrays)


```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
for (String i : cars) {
  System.out.println(i);
}
```


cf) **Multidimentional Arrays**

`int[][] = {{1,2,3},{4,5,6}}

