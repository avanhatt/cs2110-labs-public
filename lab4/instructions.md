# Lab 4: Trees and Anonymous Functions

In today's lab, we'll extend some of the examples we've seen on trees and show how we can use a new Java construct, anonymous functions, to act on trees.

## Creating your `Lab4` project in Eclipse

Follow similar steps to Lab 1 (go back and look at that handout if needed). 
1. In Eclipse, create a new Java Project named `Lab4`. Again, check that you are using Java 11 and select `No` for `Create module-info.java`. 
2. Go to the Canvas page for [Lecture 10: Trees][l10] and download `trees-demo.zip`. Unzip the file, and to begin, add the Java files `BinTree.java` and `BinTreeTest.java` to your `src` folder. Use the default package (do not name the package). 
3. Add JUnit 5 to your build path as we've done in previous projects.

> **Note**
> TODO: Run `BinTreeTest.java` in Eclipse to check everything is working as expected. You should see 3 tests successfully run. If you don't, check with a TA.

[l3]:https://canvas.cornell.edu/courses/40456/pages/lecture-10-trees?module_item_id=1549162

## Height as a recursive functions

First, read through file `BinTree.java` to make sure you understand it.

Now, let's add some additional (recursive!) functionality. 

First, let's add a function to calculate the _height_ of a given tree. As we saw in class, the height of a tree is the length of the longest path from the root to a leaf.

For example, the height of a tree with just a root node is 0 (there are no paths). 

What is the height of this tree?
```
  2
 / \
1   3
     \
      4 
```

Click `Details` below to check.
<details>

The height of the tree is 2, since the longest path from the root to a node is 2 (`1-3-4`).

</details>

> **Note**
> TODO: add a recursive function `height()` to `BinTree.java` that returns the height as an `int`. Be sure to add a Javadoc comment here and throughout this lab.

Expand `Details` below for a hint.
<details>

Hint: at a given node, we care which of the children could be part of a longer path to the root (that is, which has a greater height). How can you use this to build up the current result from the recursive calls?

</details>

Once you have an implementation, let's test it.

> **Note**
> TODO: Create a new test file to be handed in called `MoreBinTreeTests.java` (right-click `src` > `New` > `JUnit Test Case`). Add a new test for `height`: `testHeight`. Be sure to check the examples we gave, as well as at least one more example (tip: you can use a tree's subtrees as distinct tests).

As we noted in lecture, this implementation of height would be more simple to implement if we knew the tree was _perfect_ and we knew it's size (then we could just compute the height mathematically). If you have extra time at the end of the lab, add another recursive function to determine if a tree is _perfect_. 

## Another recursive function: `alwaysMoreLeft`

Let's consider a tree _"always more left"_ if _for every node with at least one child_, the left subtree has strictly more nodes than the right subtree.

For example:
- The following perfect tree is _not_ `alwaysMoreLeft`, because `2` has an equal number of nodes in the left and right subtrees:
```
  2
 / \
1   3
```

- Our previous example is also _not_ `alwaysMoreLeft`, because `2` and `3` have subtrees with more nodes on the right:
```
  2
 / \
1   3
     \
      4 
```
- The following trees _are_ `alwaysMoreLeft`:
```
  2
 / 
1  
```

```
    2
   / \
  1   3
 /
4
```


> **Note**
> TODO: Write a method `alwaysMoreLeft` in `BinTree.java` that returns a boolean indicating whether the tree is more left.

Expand the `Details` below for a hint:

<details>

Hint: you can use the `size()` method that is already defined in `BinTree.java`.

</details>

> **Note**
> Add a new test for `alwaysMoreLeft` (`testAlwaysMoreLeft`) in `MoreBinTreeTests.java`. Check at least two trees that are `alwaysMoreLeft` and two trees that are.

Optional, ungraded bonus: it is more efficient (practically, not asymptotically) to implement `alwaysMoreLeft` without calling `size` (that is, you can recur on each subtree only once). If you have time at the end of the lab, try this.

## Another take on traversing trees

Say we wanted to iterate over all the values of the nodes in a tree, computing some mathematical function in order to change each value. 

We could imagine doing this for a single mathematical function, like "adding 1". However, we might want to give user of our binary tree the ability to supply _any_ mathematical function that takes in a value of our generic type `T` and returns a value of our generic type `T` (like "subtracting 1" or "take the absolute value").

To abstract this "visit ever node and apply some function" functionality, we will use a Java construct called _anonymous functions_. An _anonymous function_ is an unnamed function that we can treat like a _value_ or _argument_. Let's first see some simpler examples of anonymous functions.

## Anonymous functions

Beyond being useful in some data structure traversals, anonymous functions (also known as "lambdas") are essential to many different programming languages and programming applications. Later this week, we will see how anonymous functions are used to program Graphical User Interfaces (GUIs). In general, many libraries and frameworks in all sorts of languages take parameters as anonymous functions. In Cornell's CS 3110, Data Structures and Functional Programming, lambdas are a core component in solving most of the programming use cases. Overall, anonymous functions are another useful way to abstract across use cases and simplify code.

Let's see how Java implements anonymous functions.

Consider this definition of `add1`:
```java
class C {
    public static int add1(int a) {
        return a + 1;
    }
}
```

If we wanted to rewrite this as an _anonymous_ function, we would remove it's name (`add1`) and use a new syntax, `->`:
```java
(a) -> a + 1       
```

Here, the `(a)` indicates that the anonymous function takes a single parameter named `a`, the `->` indicates the start of the body, and `a + 1` is the function body. You'll notice that we didn't need to `return`: anonymous function bodies are expressions that are returned by default.

If we instead wanted to add two variables, we would write:
```java
(c, d) -> {c + d};
```

Now, we said we could treat anonymous functions like values, and as such, we can assign them to objects:
```java
add= (c, d) -> {c + d};
addAgain= add;
```

Here are a few more examples of valid syntax:
```java
// with no parameter
() -> System.out.println(“Hello, world”)
// with explicit type information
(int id, String name) -> “id: ” + id + “, name” + name
// with a code block
(a, b) -> { if (a < b) return a; else return b;}
```

Let's see an example of how we can take an anonymous function as an argument. This is in fact one of the main purposes of an anonymous function: to provide an anonymous function as an argument of a method call, so that the method can call the function.

Saw we wanted to write a function to check if some predicate is true for every element in an array:
```java
/** Return true iff every element b[k]
  * satisfies p, i.e. p.test(b[k]) is true. */
static boolean check(int[] b, Pred p) {
    for (int k= 0; k < b.length; k= k + 1) {
        if (!p.test(b[k])) return false;
    }
    return true;
}
```

We describe Predicate using a special _FunctionalInterface_, like so:
```java
@FunctionalInterface   // An annotation, like @Override
/** An interface with one abstract method. */
interface Pred {
    boolean test(int k);
}
```
Conceptually, a functional interface is an interface with exactly one abstract method, which means a single anonymous function alone (without an explicit class implementation!) can _implement_ that interface. If you are curious, see [this JavaHyperText page][funcinterface] for more details. 

[funcinterface]: https://www.cs.cornell.edu/courses/JavaAndDS/files/anonInterface1.pdf

`check` can then call on the object `p` of type `Pred` with `p.test`. 

To actually use `check`, we pass in anonymous functions, for example:
```java
int[] c1= { 3, 5, 7, -9 };
int[] c2= { 3, 5, 7, 10 };

boolean all_odd= check(c1, v -> v % 2 == 1); // returns true
boolean all_positive= check(c1, v -> v > 0); // returns false
boolean all_positive= check(c2, v -> v > 0); // returns true
```

At compile-time, Java will fail if any of the anonymous functions passed in to `check` do not take in an `int` argument and return a `boolean`, like `Pred.test` requires.

## Anonymous functions with trees

Back in `BinTree.java`, let's use this anonymous function machinery to accomplish our goal: being able to change all of the values of a tree with some function that is passed in. 

Add this code to the bottom of `BinTree.java` to define the functional interface for our anonymous function argument:

```java
@FunctionalInterface   // An annotation, like @Override
/** An interface with one abstract method that produces and consumes a
 *  value of generic type T. */
interface UnaryFunc<T> {
    T func(T v);
}
```

We call this a _unary_ function because it takes one (_uni_) argument.

> **Note**
> TODO: Write a method in `BinTree.java` called `changeValues` that takes in an anonymous function that implements `UnaryFunc<T>`. `changeValues` itself should not return anything, but it should apply the provided anonymous function to every value in the current tree. 

Now, add tests for `changeValues` for two different concrete types as generic type `T`.

> **Note**
> TODO: Write a test in `MoreBinTreeTests.java`, `testChangeValuesInteger`, that applies 2 different anonymous functions to trees of integers and checks that the tree values have changed.

> **Note**
> TODO: Write a test in `MoreBinTreeTests.java`, `testChangeValuesString`, that applies 2 different anonymous functions to trees of integers and checks that the tree values have changed.

As a general programming language idiom, this idea of applying a function to change every value in a collection is called _map_ (this is a related but distinct use from "map" as a collection of key and value pairs). If you're curious, you can find a good summary of _map_ as a general concept [here][wikimap], including a comparison of what map looks like across different programming languages.

[wikimap]: https://en.wikipedia.org/wiki/Map_(higher-order_function)
## Handing in this submission

That's it!

Your submission should include the following files:
- `BinTree.java`
    - `height`
    - `alwaysMoreLeft`
    - `changeValues`
- `MoreBinTreeTests.java`
    - `testHeight`
    - `testAlwaysMoreLeft`
    - `testChangeValuesInteger`
    - `testChangeValuesString`

This was intentionally a shorter lab to give you more time to work on Assignment 4 later today. But, if you have extra time at the end and want to keep working with other students, you can work on some of the tree-based practice problems on Canvas under Test 4 study materials.
