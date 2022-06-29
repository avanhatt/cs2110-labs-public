# Lab 2: Polymorphism and Collections

In today's lab, we'll extend some of the examples we've seen on polymorphism and introduce some new machinery to work with data, in the form of the Collections framework.

## Setting format preferences in Eclipse

Moving forward in the course, we will have higher expectations around coding style. 

JavaHyperText has instructions on _both_ installing an Eclipse preference file, and setting up Eclipse to apply additional actions every time you save a file.

Go to the [Import Preferences][prefs] page and follow the instructions for adding the `.epf` file.

_Then_, make sure to complete step 7, which involves following a PDF guide, to set `18` additional style actions. You may be impressed by how much these actions clean up your code! For example, one action is to replace boolean comparisons with their value (for example, replacing `isValid == true` with just `isValid`), which is a style mistake we saw a lot in Lab 1 submissions.

[prefs]: https://www.cs.cornell.edu/courses/JavaAndDS/eclipse/Ecl01autoformat.html

## Creating your `Lab2` project in Eclipse

Follow similar steps to Lab 1 (go back and look at that handout if needed). 
1. In Eclipse, create a new Java Project named `Lab2`. Again, check that you are using Java 11 and select `No` for `Create module-info.java`. 
2. Go to the Canvas page for [Lecture 3: Inheritance][l3] and download the files for `Account` and `InterestAccount`. Add these files to your `src` folder.

[l3]: https://canvas.cornell.edu/courses/40456/pages/lecture-3-specification-and-inheritance

## Exploring inheritance and polymorphism

Let's add a new class, `Bank`, for managing our accounts.

Banks should have lists of accounts. Add a private variable to track an array of accounts, and a constructor for `Bank` that takes in an initial list of accounts (of either `Account` or `InterestAccount`). Banks should not be constructed with no accounts. The constructor should print a helpful message with the total number of accounts. 

Add Javadoc comments to your entire Bank class so far.

Add a `main` function to `Bank`, and create one account of each type. Use these accounts to create a new `Bank`.

> **Note**
> TODO: Run in Eclipse to check everything is working as expected. Compare your answer to another student's or check with a TA.

### Access modifiers

Back in the `InterestAccount` class, say we wanted to add a new benefit to users where if their account number starts with `1` (indicating an old, _valued_ account), we increase their interest rate by 0.01. Try to add a function that checks their number and increases their interest rate (name it `specialRateBonus`). Once you've tried that, click the `Details` expansion below.

<details>

Trying to use `this.number` or `number` is a compiler error! This is because the `private` modifier keep fields private to that class _and exactly that class, including private from subclasses_. So even though in our conceptual "folder" analogy, we know every `InterestAccount` has a `number` field, we cannot directly access it. 

This is an intentional design choice made by encapsulation! In the context of massive teams working on projects, different stakeholders may be responsible for decisions on how to implement a superclass and all its subclasses. In our graphical user interface example, one team of programmers might own the superclass of a GUI window, while other teams create individual specializations that extend the window. `private` is designed to keep functionality as internal as possible to one specific class implementation.

If we try to use `number` and hover over the error, we see that Eclipse suggests a few fixes:

1. Use `protected` as an access modifier. This is the 4th access modifier in Java: `protected` means that code is accessible within the package (like the implicit modifier) _and also to subclasses_. We'll use this later in the lab, but let's see another solution first.
2. Add "getter and setter" for `number`. As we saw in lecture, getters and setters are _not_ the preferred method in 2110. Let's skip this as well.
3. (and on) some suggestions about adding `number` as a local variable (etc). This is not what we want.
</details>


> **Note**
> TODO: Instead of changing the access right now, let's add a method to `Account` called `valuedAccount` that checks if the account is a _valued_ account (i.e., the account number starts with `1`). 

For testing our interest rate functionality, we are going to use an overloaded function for JUnit's `assertEquals`. In particular, double arithmetic in Java can be imprecise due to how computers represent decimal numbers. So instead of asserting exact equality after we've done math on double values, we'll use `assertEquals(double expected, double actual, double delta)` (where delta is some allowed tolerance), you can read more documentation on this [here][junitdouble].

[junitdouble]: https://junit.org/junit4/javadoc/latest/org/junit/Assert.html#assertEquals(double,%20double,%20double)

> **Note**
> TODO: Add a new JUnit test file for the entire lab called `Lab2Tests` (you can right click on `src` and select JUnit test case). Add a test for `specialRateBonus`, starting with this case and adding at least one more case:
```java
@Test
void testSpecialRateBonus() {
    InterestAccount ic1= new InterestAccount("1", 0.0);
    ic1.credit(1);
    ic1.specialRateBonus();
    ic1.accrueInterest();
    assertEquals(1.01, ic1.balance(), 0.001);
    // ... additional tests
}
```

### Dynamic querying vs. method overriding

Back in the `Bank` class, we're going to write two versions of a method: one using dynamic querying, and one with method overriding and dynamic dispatch.

Each method should solve the same problem. We want a method to apply bonuses to any applicable account. Right now, only `InterestAccount`s have applicable bonuses (for the special rate bonus). 

> **Note**
> TODO: Complete the following.
1. First, write a method `applyBonusesDynamicQuery` that applies our special rate bonus on the bank's interest accounts. You should not add any methods to `Account` or `InterestAccount`. Your solution should be as general as possible for future updates to the code. 
2. Now, use method overriding to accomplish the same task without a dynamic query in a second method, `applyBonuses`.
3. Add test cases for both to `Lab2Test`. For this task, you can just add one case per test, but you should test the bank having both types of account.

To understand why we tend to prefer option (2), consider that we might want to add another type of account, like a `SavingsAccount`. If this account has its own types of bonuses, then the overriding method makes it easier (and more local to just the new code, instead of requiring changes in multiple places) to add this functionality. 

### Limits of manipulating arrays

Now, let's say we wanted to add a method to bank called `addAccount`. With our current implementation, this is not so clean to do: arrays in Java are _fixed size_, so we would need to create a new array to store in our field every time we want to add an account. In the next part of the lab, we'll see some better options!

## Java Collections Framework

We're beginning to enter the part of 2110 where we shift our focus more to data structures. 

Here's a list of some of the most commonly used core data structures that we'll be working with (sometimes, as components of a more complex data structure):

- _Bag_: bunch of values, with duplicates allowed. E.g. a bag of coins
- _Set_: bag with no duplicates
- _List_: a bag in which the values are ordered
- _Map_: set of (`key`, `value`) pairs.  Also called an _associative list_ or a _dictionary_ (conceptually: a set of words with meanings).
- _Stack_ and _Queue_: lists with restricted ways of modifying the elements.

Java provides a Collections Framework that has classes that implement these structures, so you don't have to implement them yourselves. Java provides these in `java.util`. 

### Implementing a list

While a list is similar in functionality to the Java arrays we've seen previously, the core difference is that lists maintain both their values, and a _size_ that can be modified (in this sense, they are similar to Python lists).

Let's say we wanted to implement our own list in Java. We might allocate an underlying array to store the elements of the list, but we would also need an `int` to track the size. We'll sketch out some quick examples using local variables (to actually implement the list, we would want an entire new class):

```java
/** The elements of the list are in b[0..n-1]*/ 
int[] b= new int[100]; // Start with some guess as a top size, or capacity
int n= 0; // List initially empty; we write the list as ()
// Add 5, 8, and 2 to the list		
b[n]= 5; n= n+1;
b[n]= 8; n= n+1;
b[n]= 2; n= n+1;
// list is now (5, 8, 2)
```

If we wanted to remove an element of the list:
```java
// Remove the first element of the list.
// e.g. change (5, 8, 2) to (8, 2)
for (int k= 1; k < n; k= k+1){
    b[k-1]= b[k];
}
n= n-1;
```

Of course, there are some issues with this initial implementation sketch! See if you can identify them and discuss with someone nearby, then expand the `Detail` below.

<details>

Issues
1. The size of list is limited to 100 (or whatever size the backing array is).
2. We are missing a lot of methods to make this useful: removing values, adding values, searching for a value, etc.

</details>

Instead of implementing our own list today, we'll use the Java Collections Framework implementation: an `ArrayList`. But, it's good to keep in mind that behind the scenes, Java does have to implement some sort of backing data structure to do the same sort of book-keeping of size and capacity that we saw above.

### Java ArrayList

Java's `ArrayList` is a powerful class that we'll see often moving forward.

To see the full Java documentation, search "java 11 arraylist" in your favorite search engine.

Here's an example of using an ArrayList:
```java
// Create empty list of values named ob.
// Any object can be stored in list ob.
ArrayList ob=  new ArrayList();
// Store the integers 0..n-1 in ob
for (int k= 0; k < n; k= k+1) {
     ob.add(k);
}
```

`ArrayList` as written has an important caveat: it can only store elements that are subclasses of `Object`. So why does the previous example work? Expand the detail once you think you know the answer:

<details>

`k` is auto-boxed to the `Integer` wrapper class.

</details>

`ArrayList` can also insert an element at a specific position in the list (without overwriting any element, like a plain array). 

```java
// Put -5 in position 0 of list
ob.add(0, -5);
```

In this example, this changes the list from `(0, 1, …, n-1)`
to `(-5, 0, 1, … n-1)`. 

But, remember, this doesn't come for free! Because the the list is maintained in a backing array by Java's own implementation, this causes the `n` elements `0, 1, …, n-1` to be moved up to make room for `-5` at the beginning. Next week, we'll dig more into the efficiency, or _time complexity_,  of various Collections operations.

### Useful methods on `ArrayList`

Here are some of the most commonly used methods:

```java
al.size()       // size of list al (number of elements in it)
al.add(5)       // append 5 to list al
al.add(3, 6)    // insert 6 as element number 3 ---pushing others up
al.get(3)       // get element number 3 (the first one is number 0)
al.contains(6) 	// return true iff list al contains 6
al.remove(8)  	// remove element number 8 from the list
// ... many more ...
```

### More on the implementation of `ArrayList`

As we said earlier, `ArrayList` maintains a backing array, which contains the list of values.

The implementation has to track two different ideas of size:
`al.size()` return the number `n` of values currently in the list.

The _capacity_ of the list is the number of elements in _the backing array_. If the user of the `ArrayList` tries to add more elements than the capacity, what happens?

<details>

Java's implementations needs to increase the size of the backing array. It could do this by adding just 1 more to the size, but instead, the typical solution is increase the size of the array by doubling it (or more). Next week, we'll see why this can be more efficient in the long run in _algorithmic complexity_.

</details>

When we use Java's built-in `ArrayList`, we don't need to maintain the size or capacity directly (Java's implementation does that for us). 

### Generics and `ArrayList<...>`

We know Java is statically typed, so in our previous `ArrayList` example, what was the type of `ArrayList`'s contained values?

When we didn't provide an full type with generics for `ArrayList`(`<>`), Java assumes we want an `ArrayList` of the `Object` superclass. 

So:
```java
ArrayList  al=  new ArrayList();
```

Creates an `ArrayList` that stores `Objects`. We typically want a more specific array list (because of the compile-time reference rule), which we write with something like this:
```java
ArrayList<Integer> numbers= new ArrayList <>();
```

## Stacks and Queues

Two other list-like collections we'll use later in the class are stacks and queues. As we said earlier, these differ in how they modify the elements of the collection.

### Stacks

A stack is an ordered list of elements that supports a restricted set of operations:

1. Adding an element to the _top_ of the stack.
2. Removing an element from the _top_ of the stack.

We can think of this like a stack (in the English sense) of pancakes or textbooks: we can remove something from the top, but we can't remove an element from the bottom without disrupting the rest of the elements.

Some might also call this a LIFO list, for `L`ast `I`n, `F`irst `O`ut (meaning: the last element that was added is the first to be removed.)

Useful methods:
```java
push(e)		// put e on top of stack
peek()      // look at, but don't remove, top stack element
pop()		// remove and return top stack element
size()		// number of elements on the stack
```

The phrase `stack` has probably come up in your computer science work before, even if you didn't specifically study the data structure. The call stack we sometimes talk about when referring to execution traces is a stack, as is the function call stack when we think about how procedures are called. 

If we think about the execution of calls in a Java program, the call to `main` is the first call started but the last call to complete (for example, when our program returns a final value, or as the last chance to have a try/catch block when an exception is hit).

### Queues

A queue is _also_ an ordered list of elements that supports a restricted set of operations:

1. Adding an element to the _end_ of the stack.
2. Removing an element from the _beginning_ of the stack.

We can think of this like a queue (in the _British_ English sense) of people waiting to be let into a building: the first to arrive is the first to be handled.

We might also call this a FIFO list, for `F`irst `I`n, `F`irst `O`ut (meaning: the first element that was added is the first to be removed.)

Useful methods:
```java
add(e)		// put e to end of queue
peek()      // look at, but don't remove, top queue element
remove()	// remove and return first element
size()		// number of elements on the queue
```

We won't use stacks or queues directly today, but keep them in mind for later in the class.

## Back to our bank example

> **Note**
> TODO: Back in Eclipse, add a new class, `Bank2`, that uses an `ArrayList` rather than an `array` to track accounts. For the purpose of this lab, copying code is fine - we want to see both versions of your implementation!

Now that we have a more flexible data structure storing our accounts, complete the following tasks in just `Bank2`:

> **Note**
> TODO: Add one method to `Bank2` to add a single new `InterestAccount` (`addNewInterestAccount`). Add a public `numberOfAccounts` method.  Add a test called `addNewInterestAccount` (your test can just check the number of accounts). 

> **Note**
> TODO: Add one method to `Bank2` to add an entire `ArrayList` of accounts to the current list of accounts (`addNewAccounts`). Hint: look in the Java 11 documentation to find a method that makes this cleaner. Add `testAddNewAccounts` as well (again, just the number is fine for this task).

We now want our bank to add some special functionality. For this task, you can assume the preconditions on the method that:
1. All account numbers are numeric and positive integers (you can use `Integer.parseInt()`, looking up the documentation as needed). 
2. All interest rates are positive.
Use only `ArrayList` functionality for this task (not any other collection). 

This task is meant to be more challenging than the others in this lab: work with other students and ask the TAs for help!

> **Note**
> TODO: Add a method to `Bank2`, `bestRateForOldestCustomer` that:
> 1. Finds the customer with the smallest account number (when interpreted as an integer). Use the _first_ customer you encounter in the list with the smallest number if there are multiple with the same number.
> 2. Create a new `InterestAccount` for this customer with a rate that is `0.01` higher than any other existing account. Insert the new account you make directly after their previous account in the central list of accounts. For this task, you should change the modifiers on `interestRate` and `number` to be `protected` to be able to access them from `Bank2` and `Lab2Tests`. You should add to `Bank2` a public `getAccount` method to return the result at a given index.

You can use the following test case:
```java
@Test
void testBestRateForOldestCustomer() {

    // Create sample accounts
    InterestAccount ia1= new InterestAccount("4", 0.02);

    // This account has the oldest/lowest number first
    InterestAccount ia2= new InterestAccount("1", 0.02);
    InterestAccount ia3= new InterestAccount("1", 0.05);

    // Make sure we can handle non-interest accounts
    Account a= new Account("6");

    // Create our list
    ArrayList<Account> accts= new ArrayList<>();
    accts.add(ia1);
    accts.add(ia2);
    accts.add(ia3);
    accts.add(a);

    // We expect the new account to be added at index 2
    Bank2 b= new Bank2(accts);
    b.bestRateForOldestCustomer();

    // Check the other accounts are in the right places
    assertEquals(5, b.numberOfAccounts());
    assertEquals(ia1, b.getAccount(0));
    assertEquals(ia2, b.getAccount(1));
    assertEquals(ia3, b.getAccount(3));
    assertEquals(a, b.getAccount(4));

    // Check the class and rate on the new account
    Account newAccount= b.getAccount(2);
    assertEquals(newAccount.getClass(), InterestAccount.class);
    InterestAccount newIAcct= (InterestAccount) newAccount;
    assertEquals(newIAcct.interestRate, 0.06, 0.001);
}
```

## Handing in this submission

That's it! 

Your submission should include the following files/new methods in a single compressed zip (see below):
- `Account.java`
    - `valuedAccount`
- `Bank.java`
    - Constructor
    - `applyBonusesDynamicQuery`
    - `applyBonuses`
- `Bank2.java`
    - Constructor
    - `applyBonusesDynamicQuery`
    - `applyBonuses`
    - `addNewInterestAccount`
    - `addNewAccounts`
    - `bestRateForOldestCustomer`
    - `getAccount`
- `InterestAccount.java`
    - `specialRateBonus`
- `Lab2Tests.java`
    - `testSpecialRateBonus`
    - `testApplyBonusesDynamicQuery`
    - `testApplyBonuses`
    - `testAddNewInterestAccount`
    - `testAddNewAccounts`
    - `testBestRateForOldestCustomer`

For this lab, create a zip file of your entire `src` directory and upload the single file to Canvas. If you aren't sure where your Eclipse Workspace is, you can find the location of `src` by right clicking it, selecting `Show In`, and selecting `System Explorer`. This will open the file navigator for your operating system.

If you worked with another student, Canvas will let you join a group and enter a single submission. Please post publicly on Ed if you have any issues with this.

### OPTIONAL, UNGRADED: implementing List

If you finished the previous parts of this lab early and want to better understand lists now, add a new class `ArrList` that implements the basic functionality of an `ArrayList`, but with your own backing array. Your `List` should be constructed with a specific `int` capacity and should double its size whenever the capacity is reached. You should be able to add a new element at a given index of the list, which shifts all other items to the next index (and potentially doubles the array). Similarly, you should be able to remove an element at an arbitrary index (what should happen to the other elements?).