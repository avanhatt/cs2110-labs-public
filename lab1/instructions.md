# Lab 1: Java Applications, Testing, and Strings

Welcome to your first CS 2110 lab assignment! As described in the [collaboration policy][ai], labs can be completed collaboratively, so feel free to work with the students nearby you. You can turn in the same submission as *one* other student, but may work with more than one other students. If you do not have a laptop today, pair with another student who does and turn in the same submission. 

In addition, we have lots of TAs here to support you in labs, so raise your hand as soon as you have a question!

[ai]: https://canvas.cornell.edu/courses/40456/assignments/syllabus

In this lab, we have the following learning objectives:
- Building a Java application in Eclipse.
- Running code and testing code in Eclipse.
- Using Java `String`s and `char`s (or characters) to manipulate text.

The lab has four parts:
1. [Part 1: Getting started: the Eclipse IDE](#part-1-getting-starting-the-eclipse-ide) _(this one is mostly logistical, but we promise, it'll get more exciting!)_
2. [Part 2: Java Applications and Packages](#part-2-java-applications-and-packages)
3. [Part 3: Testing and JUnit](#part-3-testing-and-junit)
4. [Part 4: String and Character](#part-4-string-and-character)

Throughout the lab, you will see `Checkpoint`s where we encourage you to especially raise your hand if you are stuck, and `TODO`s where you will write code that you'll need to turn in later.

## Part 1: Getting starting: the Eclipse IDE

There are many different ways to write Java code, but for larger projects, it's often best to use an Integrated Development Environment (IDE). An IDE is a software application with helpful features for writing and debugging code. You can think of an IDE as analogous to writing an English paper in an app that has spell check, grammar warnings, and word count—IDEs help find errors in your code and help you be more productive. IDEs also invoke the language's compiler and runtime (if applicable) so you can test and run your code.

In CS 2110, we will use the Eclipse IDE (other popular IDEs that support Java include IntelliJ and NetBeans). We will also show some code examples in a simpler app, jshell, that is designed for interacting with small bits of Java code in a Read-Evaluate-Print loop  (REPL).

### Eclipse Installation Instructions
Below are instructions for installing Eclipse and Java and modifying preferences to fit the needs of CS 2110.

The newest version of Eclipse comes with Java already pre-installed. However, if you want to use jshell on your own computer (more on jshell later) or run Java without using Eclipse, you will have to install a standalone version of Java. We outline that process at the end of this page.

If you run into any issues following these instructions, please check the Ed Q&A platform and post a new question if you do not find a solution.


> **Note**
> if you are using a lab computer where you are unable to install Eclipse, you can use an online IDE instead for this lab (such as https://www.jdoodle.com/online-java-compiler-ide/). However, the JUnit testing cannot be used in an online IDE that can't install external packages. For those parts, we'll try and pair you with a student who does have a working Eclipse installation.
> We will work with IT to try and get Eclipse installed on the Upson lab computers.

#### Additional terminology
When installing Java, you might come across these additional terms:

- _JVM: The "Java Virtual Machine"_. Java code is ultimately run on a computer after it has been translated to an intermediate format called bytecode that is run on the JVM (the term 'virtual' is used to imply a level above real machine code or assembly language).   The Java compiler translates Java source programs into this virtual machine language, and the JVM runs the resulting bytecode program on the actual machine hardware.
- _JRE: Java Runtime Environment_. This includes everything that is needed to run a Java program that has been translated into the JVM bytecode.
- _JDK: Java Development Kit_. This includes a compiler and other tools that are needed to transform Java source programs (programs written in Java) into the Java Virtual Machine bytecode.
- _Workspace_. A directory (folder) where all your Java projects will be stored. Whenever you start a new assignment, do it in a new project.

#### 1. Installing Eclipse and Java together
We show you how to install a version of Eclipse that contains everything you need to develop, debug, and run Java programs.

1. Visit http://www.eclipse.org/downloads/ This is the site to download the most recent version of Eclipse, ECLIPSE IDE 2022‑03.
2. Click the orange button titled "Download x86_64". That will open another page.
3. Click the orange button titled "Download". If you are on a Mac, this will download a file named "eclipse-inst-jre-mac64.dmg". The name will be different for Windows, Linux, or other machines.
4. Double-click the downloaded installation file. This will give you a window with the file "Eclipse Installer.app". Double-click it to start the installation. Follow the directions. You may be shown a window with a choice of IDEs to install. Install "Eclipse IDE for Java Developers". After choosing that, click the orange INSTALL button.
5. Click the green button title "Launch". This will start the Eclipse IDE.
You will be asked to "Select a directory as workspace". This is where all your Java projects will be stored.
6. If you have never installed Eclipse before, you can use the suggested directory. You can click the box that says to use it as the default.
    1. If you have installed Eclipse before, you can browse to your current Eclipse Workspace and use that workspace. Your projects in that workspace may be modified (automatically) to work with this new Eclipse IDE.
    2. Troubleshooting note: If you have a new Apple MacBook with the M1 chip, Eclipse may crash when you open it. Please post a question on Ed and we will help you resolve this.

#### 2. Configuring Eclipse
We have some specific instructions for configuring Eclipse that will make it easier to succeed in CS 2110.

1. Change the compiler compliance level (versioning)
CS 2110 focuses on Java version 11, but the newest version of Eclipse will install Java version 16 by default. This is okay (newer versions are compatible with older ones), but you should tell Eclipse to limit you to the features present in Java 11. When you have the Eclipse IDE running, use menu items:
    (Mac OS) ` Eclipse > Preferences > Java > Compiler`
    (Windows) `Preferences > Java > Compiler`
Near the top of the pane that opens, change the Compiler compliance level to 11.

2. Install formatting preferences
We have developed a file of formatting preferences for Java programs to make it easy for you to keep Java programs in a readable, understandable form. You must install these preferences. Please visit this [page](http://www.cs.cornell.edu/courses/JavaAndDS/eclipse/Ecl01autoformat.html) from our JavaHyperText online textbook and follow the instructions to download and import the preferences.

3. Installing standalone Java
We suggest installing a standalone version f Java if you want to use jshell or if you want to execute Java applications that are stored in `.jar` files. You need a JRE and a JDK. Here are basic instructions for installing both the JDK and the JRE.

4. Change the default encoding (Windows only) 
<details>
  <summary>Click to see Windows-only instructions if needed</summary>
To write code containing characters outside the Latin alphabet, Eclipse's default character encoding must be set to "UTF-8". This is not the default on Windows, so open Eclipse and go to Window>Preferences, then General>Workspace and look under "Text file encoding". If the default says Cp1252 (or anything other than UTF-8), then select Other: UTF-8 and apply the change.
</details>

#### 3. Checking your JRE and JDK
If you have Java already on your computer, here's how to find out which JRE you have on your computer. Open a command window (in Windows, `Start > Command Prompt`; in Mac OS X, `Applications > Utilities > Terminal`) and type `java -version` at the command prompt. It should look something like this:

```bash
$ java -version
openjdk version "11.0.11" 2021-04-20
OpenJDK Runtime Environment AdoptOpenJDK-11.0.11+9 (build 11.0.11+9)
...
```

This says you have version 11 installed. The output above is for the version installed on 29 June 2021. If you have an earlier one, we suggest you install the new one. It should be OK to have a later version of Java installed, but you should change the compiler compliance level to 11 (as discussed above in point 1).

If Java 11 is not installed on your computer, you need to install it.

Which JDK do you have? If you are on a PC running Windows and have never installed a version of the Java Development Kit (JDK) on your machine, you probably don't have it. If you are on a Mac or run Linux, you may have it by default. To find out, open a command window and type `javac -version`:

```bash
$ javac -version
javac 11.0.11
```
If you get an error message or the version is earlier than 11 you MUST (re)install the JDK. If you have 11.0.3 or later, that's fine. If you download the new one now, it will be version 11.0.11.

#### 4. Installing the JRE and JDK (OPTIONAL)
If you do not already have Java 11, you may want to install the JDK and JRE together, following these instructions.

<details>
    <summary> Click for optional instructions </summary>
Visit this website:  https://adoptopenjdk.net.
Choose OpenJDK 11 (LTS) and OpenJ9
Select an Operating System and an Architecture
The window will change to make appropriate Install buttons visible. If you have a choice between Normal and something else, choose Normal.
Download and install the JDK. Follow directions.
This should provide both a JRE and a JDK. Double-check their versions using the procedures above (you may need to log out and log back in first).
Note: There are reports that downloading using windows 10 with Google Chrome or Microsoft Edge may hang forever. The suggestion is to use Firefox in this case.

Note: If you are running Linux, your distribution may provide a JDK package that is more convenient than the AdoptOpenJDK downloads (e.g.  `openjdk-11-jdk` on Ubuntu).
</details>

#### 3. Compiling and running from the command line (OPTIONAL)
Note: this is entirely optional and can be skipped.
We don't use this feature in CS 2110, but sometimes it is useful to run a Java program without launching it from Eclipse or jshell. 

<details>
  <summary>Click to see optional instructions</summary>

1. Compiling
Say your method main is in a class `MyProgram` and it is contained in a source file `MyProgram.java`. If it is not in a package, navigate to the folder containing `MyProgram.java` and type `javac MyProgram.java`. This invokes the Java compiler (javac) directly.

If it is in a package (say, `myPackage`), the source should be in a folder called `myPackage`. Navigate to the folder containing myPackage and type `javac myPackage/MyProgram.java`.

2. Running
From the same folder you compiled from, type `java MyProgram <program arguments>`. If it is not in a package, and `java myPackage.MyProgram <program arguments>` if it is.

3. Specifying a Classpath
Sometimes you may need to inform Java where to find auxiliary classes. You can do this with the -cp option to the java command. Supply a sequence of folders telling Java where to look for classes, separated by `:` (Mac & Linux) or `;` (Windows).
</details>

> **Note**
> Checkpoint: if you are unable to open Eclipse now, raise your hand to ask a TA for help.

## Part 2: Java Applications and Packages

A standalone software program is called an application. You can this of this as the same core idea as applications or apps on your phone or computer: an application is a bundle of software with a shared purpose. Applications sometimes have Graphical User Interfaces, or GUIs, to interact with users (this is true of most phone apps). We'll cover these later in the course. Other applications simple are run within your IDE or via the command line, by taking user input, reading from files, or running test cases. Applications help us organize our code and make it easier for the user to interact in a structured way.

In Java, a program that has a class with a public static `main` method is an application (we'll cover the `public static` parts later in the course, but in short, they indicate that this method is accessible outside of the class without explicitly creating an instance):

```java
public static void main(String[] args) {
    // ...
}
```

The program, i.e. the application, is run by calling method main.
Eclipse has an easy way to do this, as we'll see in the next section.

### A first Java application in Eclipse

1. Open Eclipse and navigate to `File -> New -> Java Project`. Give your project the name `Lab1`. Check that your execution environment is Java 11. Select `No` for `Create module-info.java`. Click `Finish.`
2. Highlight the directory `src` in the file tree on the left. Do `File -> New -> Class`. Make `Package` field empty. Give it the name `Lab1`. Check the box for `public static void main(…)`. Click `Finish`.

That's it, we have a Java application! It doesn't do anything interesting yet. Let's start by just making it print to the command line. In Java, we print by calling a library function `println` and passing in a string. 

> **Note** 
> TODO: We will build on this class and eventually this is what you will turn in for Lab 1, so make sure to keep it open. Anything marked with this Note tag is either a Checkpoint (where you might want to call over a TA) or a TODO (which is something you'll need to turn in)!
 
3. Add the following line to the body of your main method:

```java
System.out.println("Hello World"); 
```

4. Let's run our application! Hit the green `play` button in the top menu or navigate to `Run -> Run`. You should see your message printed out.

> **Note**
> Checkpoint: if you are unable to select run or see the message, raise your hand to ask a TA for help.

### Java Packages

Packages in Java are reusable collections of classes (in some cases, you might also call these libraries). Packages are not necessarily applications: they main not have any executable `main` method. In CS 2110, we will use many useful packages.

We'll consider four main kinds of packages:
1. The default package: in project `directory/src`.
2. Local packages, or Java classes that are contained in a specific directory on your hard drive (possibly containing sub-packages).
3. Default packages that are included with your Java installation: for example, packages `java.lang` (for language features) and `java.io` (for dealing with Input/Output, or 'io'). You might also hear there referred to as API (for Application Programming Interface), a term used across many different languages. You can read more about these default packages in the [Java 11 documentation][java11]. In part 4, we'll walk through an example of using Java packages.
4. External packages, which are collections of software by other developers or companies that are shared with the Java community. In some languages, these might also be called libraries or frameworks. We'll cover one of these in the next section.

In Java, you use `import` statements at the top of classes to be able to use or call code from other packages (though you do not need to import code from Java's default package). We'll see an example of this in the next section.

[java11]: https://docs.oracle.com/en/java/javase/11/

## Part 3: Testing and JUnit

Now, we'll cover one especially useful external package that solves an essential problem: how can we test that our code is correct? Sure, we have seen that we can print code from Java and we could use that to always test our outputs, but this has many long-term shortcomings. We might not notice if small parts of the output change, or we might write too much code to want to keep track of all that output!

### Unit tests
Instead, the immensely popular best practice in software engineering is to write _unit tests_ that programmatically check small pieces (or units) of code. In CS 2110, this will most often look like writing tests that call a single method (which might then call other methods). You can then run all of your tests with a single click or command, where each result is compared to some expected result. Tests either succeed (get expected results) or fail (which indicates that either the code or the test is wrong). 

<details>
    <summary>As an aside, once we've written tests, can we be sure our code is correct?  Click for optional aside.</summary>
The answer, sadly, is usually not. Why? Test cases check specific inputs and see that the code returns the expected answer, so we know the code is correct <i>for those inputs</i>. But for many useful pieces of code, there are extremely large or even infinite numbers of potential inputs!  If a computer scientist <b>really</b> needs to know that our code is correct, they can use additional tools. One option is to randomly generate a lot of test input (this is known as <i>fuzzing</i>). Another option, known as <i>formal verification</i> (covered in CS 4160: Formal Verification), is to try and mathematically prove that their code is right. This is not always possible (for reasons that are covered in CS 4810: Introduction to the Theory of Computing), but can be a useful tool for ensuring software is bug-free. Some of Professor VanHattum's research is in the field of formally verifying systems (or low-level) code. 
</details>

Testing can also be a great way to know, _as you are writing your code_, whether you are implementing what you mean to implement. Code should implement a *specification*, or *spec*, that describes precisely what the code should accomplish. Often in CS 2110, these specifications are written out in English and given in a lab or programming assignment handout. In a software engineering or research job, the specifications may initially be an informal conversation. It's a good idea to write down a more precise spec _before_ you start coding. 

### JUnit testing package
You can think of [JUnit][] as an external package that provides unit testing support for Java. You write tests in your source code that are marked as JUnit tests, then you can run the tests and JUnit will alert you to any failed tests.

[junit]: https://junit.org/junit5/

> **Note**
> TODO: Let's add our first JUnit test.

1. In `Package Explorer (PE)` pane, select directory `src`.
2. Go to menu item `File -> New -> JUnit Test Case`.  Note that it put in name `Lab1Test` for the class to be created. If the Name is not there, put in a Name.
3. Press `Finish`. If it asks to put JUnit 5 on the build path, say yes.

You see class `Lab1Test`, with one method `test`. 

> **Note**
> TODO: Let's plan out a simple method that calculates the total price of an item with sales tax. Add the following method to your `Lab1.java` file.
```java
/**
Calculate the price with transaction fee when 'includeFee' is true. The price is the 
original dollar amount (i.e., 10) and the tax is an additional dollar amount (i.e., 2).
*/
public static int priceWithFee(boolean includeFee, int price, int fee) {
    return 0; // TODO
}
```
> _Before_ we write any implementation code, let's make our `test` method useful. Rename it to `testPriceWithFee` and add in this first line to check our example. We use an `assertEquals` statement to check whether the result is what we expect. Note: the `static` in the method indicates that this method is on the _class_ rather than an instance, so we use the class name and a `.` to refer to the method, rather than a variable name and a `.` To use `assertEquals`, you may need to import it. Eclipse should provide this option if you mouse over the error. 
```java
@Test
void testPriceWithFee() {
    assertEquals(12, Lab1.priceWithFee(true, 10, 2));
    // TODO: fill in 3 or more additional assertEquals
}
```
> TODO: write 3 additional `assertEquals` statements for this test. 
> TODO: Now, go back and implement the body of `priceWithFee`. 

To run the test we just wrote, use menu item `Run->Run` for class `Lab1Test.java`.
> **Note**
> Checkpoint: if you can't get this test to run, raise your hand for a TA.

Note that only methods with `@Test` before them in a JUnit test class are called. This annotation specifies to JUnit that this is a unit test and should be run.

## Part 4: String and Character

Java uses the String and Character classes to model strings of text (for example, "hello world", or the entire body of _A Midsummer Night's Dream_). Strings are essential to many useful computing applications (think: much of machine learning, compilers that translate your code to machine instructions, even self-driving cars need to model street sign text). 

Note that this information will be essential for Assignment 1, due Friday, as well.

In Java (and many other languages), the basic unit to model text is a `Character` or `char`. Characters model a single letter or symbol of text. [Unicode][] is a representation shared across many programming languages that represents _most_ common characters with a 2-byte code (see Ed post #2 for a discussion of less common Unicode).

### The `char` primitive type
In Java, `char` is a primitive type. We can create a `char` with _single_ quotes:
```java
char justA= 'a';
char justB= 'b';
System.out.println(justA);
```

There are some special `char`s used to model parts of text you might not think about: whitespace like spaces and tabs, or special characters to change how text is formatted.
A backslash `\` is often used to "escape" a character from being interpreted as part of the language source code (for example, if you need a single quote within a string, you want to make sure Java doesn't think that is the end of your string). 

- `' '`  - space
- `'\t'` - tab character
- `'\n'` - newline character
- `'\''` - single quote character
- `'\"'` - double quote character
- `'\\'` - backslash character
- `'\b'` - backspace character - avoid this
- `'\f'` - formfeed character  - avoid this
- `'\r'` - carriage return     - rarely use this

[unicode]: https://unicode-table.com/en/

### Casting `char` values

Because `char`s are just represented as 2 bytes, we can also cast them as integers to perform operations on them (and visa versa).

```java
(int)a     // gives 97
(char)97   // gives 'a'
(char)2384 // gives 'ॐ', om or aum, the sound of the universe in Hinduism
```  

Java implicitly casts `char`s to `int`s for performing arithmetic operations (`<`, `>`, `==`, `!=`, etc). For example:

```java
'a' < 'b' // same as 97 < 98, evaluates to true
'a' + 1   // same as 98 
```

> **Note**
> TODO: We want to write a method as follows that returns true if `a`, `b`, and `c` are in order (that is, `a` is strictly less than `b` is strictly less than `c`). Again, add this to `Lab1.java`
```java
public static boolean charsAreInOrder(char a, char b, char c) {
    return false; // TODO
}
```
> *Before* you write any code, let's check that you understand the problem. Add another test in your Lab1Test class. You can either use the menu buttons again, or copy-and-paste your first test and change the name. Call this new test `testCharsAreInOrder`. Add this to its body (note, since we are checking a boolean, can use `assertTrue`, `assertFalse,` or `assertEquals(true, ...)`:

```java
assertTrue(Lab1.charsAreInOrder('x', 'y', 'z'));
```
> Next, add three more lines of the same form that test cases where 1 new case should return `true` and 2 new cases should return `false`. Note, we use `!` to negative the value to check for the `false` case. Discuss with other students whether your cases are correct. If you are not sure, raise your hand and call over a TA! 
```java
assertTrue(Lab1.charsAreInOrder(...));
assertFalse(Lab1.charsAreInOrder(...));
assertFalse(Lab1.charsAreInOrder(...));
```
> Now, go back and implement `charsAreInOrder`. Click `Run` again to make sure all of your tests succeed.

### Checking properties of `char` values

The Java default packages provide useful utilities for working with `char` values in a class called `Character`. Here are some examples with evocative names:

```java
// These return boolean values
Character.isAlphabetic(c)
Character.isDigit(c)
Character.isLetter(c)
Character.isLowerCase(c)
Character.isUpperCase(c)
Character.isWhitespace(c)

// These return another `char` value
Character.toLowerCase(c)
Character.toUpperCase(c)
```

> **Note**
> TODO: Write a method as follows that checks if a character is alphabetic, and converts it to upper case if so. If the letter is not alphabetic, return the space character (`' '`).
```java
public static char toUpperCaseIfAlphabetic(char c) {
    // TODO ...
}
```
> To check that your code gets than answer you expect, add another test `testToUpperCaseIfAlphabetic`and fill in (replace the `...`) the following lines.
```java
assertEquals('A', Lab1.toUpperCaseIfAlphabetic('a')); 
assertEquals(..., Lab1.toUpperCaseIfAlphabetic(...)); // should check a ' ' return
assertEquals(..., Lab1.toUpperCaseIfAlphabetic(...)); // should check an already-upper-case letter
```
> Note that when solving these sorts of programming problems (say in a job interview or when you might hold TA hours in the future), it's always good to think through test cases like this. In particular, checking for _edge cases_ like the method returning `' '`, can help spot issues in your implementation. In computer science, the details matter and can cause bugs, so we want to test even unusual inputs!

### The class `String` 

While `char` is a primitive value that represents single characters of text, Java uses a _class_ to represent longer strings of text. The class `String` has a special treatment in Java: when you write a literal string in Java (with _double_ quotes), Java automatically creates an _instance_ of the class `String`. 

```java
"this is a string" // this constructs an instance of type String
```

Strings in Java are _immutable_, that is, once created you cannot change their value. Instead, to operate on strings, you call methods or functions that create new `String` instances.

### Operations on `String`s

The operator `+` in Java is _overloaded_: it is defined for many different primitive types and some classes. 

For `String`s, `+` catenates (or combines) multiple strings, for example:

```java
"abc" + "12" // gives "abc12"
```

`+` can also combine multiple different types through implicit casts. However, be careful, since the order of implicit casts matters! For example:
```java
1 + 2 + "abc" // gives "3abc"
"abc" + 1 + 2 // gives "abc12"
```

`+` in combination with `println` can be a great debugging tool. For example, if you want to check the value of a variables `c` and `d` in the middle of a method, you can write the following. 

```java
System.out.println("c is: " + c + ", d is: " + d);
// gives, i.e., "c is: 32, d is: -3"
```

Later in the course we'll learn to use more advanced debugging features in Eclipse that are often superior to using these types of print lines, but for now they are a good strategy. 

> **Note**
> TODO: Write a method that returns a _specific_ happy birthday `String` if today (June 22) is their birthday (`"Happy birthday, you are 21 years old!"`, with the correct number of years). If today is not their birthday, return `"Today is not your birthday."` You can assume the day, month, and year are valid (this is considered a _precondition_) of the method.
```java
// Precondition: day, month, and year are valid
public static String specificHappyBirthday(int day, int month, int year) {
    // TODO ...
}
```
> Again, preferably _before_ you write any code, create and fill in a new test.
```java
assertEquals(...);  // should check a birthday message included how old
assertEquals(...); // should check a non-birthday message
```

> Remember that spaces are useful in English (so something like `"are5years"` is avoidable).

### Picking out pieces of a `String`

As we alluded to earlier, `char`s make up the building blocks for longer `String`s. 

`String` has methods defined on it to get information about the composite characters. 

For example, to get the number of characters, you can use `.length()`. 

```java
"abc".length()   // gives 3
"a b c".length() // gives 5
```

We can also pull out the `char` at a specific position or index, starting with the index 0 (that is, the first character is at `0`, the last character is at `n - 1` for a `String` with length `n`).

```java
"abc".charAt(1)   // gives 'b'
"a b c".charAt(1) // gives ' '
```

We can also pull out smaller `String` instances, or substrings, based on indices.

```java
// s.substring(i, j) produces a new String containing chars at positions i..(j-1)   
"abcde".substring(2,4) // gives a new String "cd"
// note: the character at index 4, 'e', is excluded!
```

### More useful operations on `String`

Java provides many more useful operations. We sample some here, but you can look up more in the Java 11 documentation!

- `s.trim()`          – `s` but with leading/trailing whitespace removed
- `s.indexOf(s1)`     – position of first occurrence of `s1` in `s` (`-1` if none)
- `s.lastIndexOf(s1)` – similar to `s.indexOf(s1)`, but gives the last occurrence rather than the first
- `s.contains(s1)`    – true iff `String` `s1` is contained in `s2`
- `s.startsWith(s1)`  – true iff `s` starts with `String` `s1`
- `s.endsWith(s1)`    – true iff `s` ends with `String` `s1`
- `s.compareTo(s1)`   – 0 if `s` and `s1` contain the same string, `< 0` if `s` is less (we'll cover the order later), `> 0` if `s` is greater 

> **Note**
> TODO: Now, let's write two methods that should look familiar from the syllabus (under detailed preconditions). The syllabus asks you to try implementing these in any language; now, we have all the pieces to implement them in Java! If you need a refresher on for-loops (which have a harder-to-remember syntax in Java than some other languages), refer to the JavaHyperText entry for [for-loops][].
> First, we'll write a method that checks whether a string is a palindrome (the same backwards and forwards, like `"abccba"`). This first method should be pedantic and check for _exactly_ the same string: `"abA"`should return false, as should `"Madam, I'm Adam."` (because of capitalization, spaces, and punctuation).
> TODO: add (at least) these three examples in a new test case. 

> 
```java
public static boolean isPalindrome(String s) {
    // TODO ...
}
```

> TODO: Next, we'll write a second method (it can call your first method) that is more permissive about capitalization, spaces, and punctuation. This method should only look at alphabetic and numeric characters. Add a final test case and check that both `"abA"` and `"Madam, I'm Adam."` return `true`, then fill in the implementation. For this method, you may want to consider a Java-provided class that makes it easier to build and edit string---`StringBuilder`. You can read about `StringBuilder` in JavaHyperText [here][sb] or in the Java documentation.

[sb]: https://www.cs.cornell.edu/courses/JavaAndDS/files/stringBuilder.pdf

```java
public static boolean isPermissivePalindrome(String s) {
    // TODO ...
}
```

[for-loops]: https://www.cs.cornell.edu/courses/JavaAndDS/files/for-loop.pdf

## Handing in this submission

That's it! Congrats on completing your first CS 2110 lab.

Your submission should include the following methods _and_ their corresponding test cases:
- `priceWithFee`
- `charsAreInOrder`
- `toUpperCaseIfAlphabetic`
- `specificHappyBirthday`
- `isPalindrome`
- `isPermissivePalindrome`

This lab is due tonight at 6pm, with a grace period until midnight (that is, no late penalty until then). You can submit this lab on Canvas by uploading your two files, `Lab1.java` and `Lab1Test.java`. If you worked with another student, Canvas will let you join a group and enter a single submission. Please post publicly on Ed if you have any issues with this.