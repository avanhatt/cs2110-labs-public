# Lab 5: Hashing, Map, and Resizing

In today's lab, you'll build a resizable hash map that achieves the lofty goal of amortized constant-time access for element access (`get`) and insertion (`put`). Like in previous labs, you can work with a partner.

## Creating your `Lab5` project in Eclipse

Follow similar steps to Lab 1 (go back and look at that handout if needed). 
1. In Eclipse, create a new Java Project named `Lab5`. Again, check that you are using Java 11 and select `No` for `Create module-info.java`. 
2. From the Canvas lab page, download the files `ResizableMap.java` and `ResizableMapTests.java`. To begin, add the two files to your `src` folder. Use the default package (do not name the package). 
3. Add JUnit 5 to your build path as we've done in previous projects.

> **Note**
> TODO: When you run `ResizableMapTests.java` in Eclipse initially, all tests will fail. This is the functionality you will implement in this lab!

## Implementing a resizable hash table with chaining

In this lab, your task will be to build a hash table with _chaining_ that supports the basic operations `get`, `put`, and `size`. Under the hood, your hash table should also support a resize operation that allows it to grow as it reaches some specified maximum _load value_.

Today's lab is meant to have a bit of a build-your-own adventure style: we provide a basic test suite that your data structure needs to pass, but we'll provide pointers along the way to how you could expand the functionality of your implementation. In addition, in several places we ask you to try to implement functionality from the high level idea, but have more detailed pseudocode in expandable `Details` tabs in case you get stuck. 

We will only test your code using the methods in the provided interface/class (though we will read your code). So, you are free to design and name additional fields and methods that you want within the class.

## `ResizableMap` and its inner classes

We have defined a class `ResizableMap` that specifically models a hash table of mapping `String` keys to `String` values. This is to simplify the implementation a bit: you are welcome to go back and modify your implementation to be generic over `<K, V>` key and value types, but we suggest you start here.

`ResizableMap` implements the interface we provide called `BasicMap`. This basic map interface defines support for the core operations of a hashmap, where each is part of a  subset of operations of [Java's `Map<K, V>` interface][javamap] (which is too long to implement all of today).

[javamap]: https://docs.oracle.com/javase/8/docs/api/java/util/Map.html 

We have stenciled in the method declarations to implement this interface so that the initial code compiles, for it to run successfully, you will need to implement `size`, `put`, and `get`. In addition, we provide two extra methods declarations you must implement so that we can test your resizing functionality: `backingArraySize` and `currentLoadFactor`. 

`ResizableMap`'s constructor takes in an initial size and a maximum load factor. The backing array should start at the initial size and grow when the maximum load factor is reached, but first, focus on implementing the basic `Map` functionality.

We also provide an `Entry` inner class that has fields for the key and value. 

`ResizableMap` itself currently has no fields: *part of your job will be to add fields as necessary along the way.*

### Restrictions

So that you can see how things are implemented under-the-hood more directly, you should not use Java's `HashTable` (or other existing map implementations) or Java's ArrayList (since that does its own resizing) in your implementation. We have provided a simple `Chain` class that implements as LinkedList to use, you may opt to replace this with Java's `LinkedList<E>` representation, but the generics are a little more difficult to work with here. We suggest first starting with `Chain`.

## Implementing `size`

Your `ResizableMap` should track its own current size. Since adding multiple values with the same key should replace the old value, the size should only be increased when a new key (and its corresponding value) is added.

> **Note**
> TODO: Add the initial code necessary to return the size, even though we have not gotten to code that changes the size. With this, your code should pass the test `testConstructor`.

## Getting a `hashCode`

To start with, just use the default `hashCode` that is already defined as a method on type `String`. You may want to define a helper method to get the _index_ a String should go in, using the object to hashcode to index strategy we discussed in class. 

## Implementing `put`

Your `ResizableMap` should use a backing array that is initially the `initialSize` passed into the constructor. Think about which of the inner classes we've provided you want to use in the backing array.

> **Note**
> TODO: Add the backing array with the specified initial size. 

If you get stuck, expand the `Details` below for a hint and/or ask a TA:

<details>

We provide a `Chain` inner class that should be used as elements of the array.

</details>

Now, think about the cases you need to implement `put`, including that:
1. `put` should _replace_ the value for a key if the key is already in the table. 
2. `put` should return a value that is replaced if the key already has a value and otherwise return `null` (see Javadoc comment).

For iterating over the entries in a chain, you'll want to use for-each syntax (i.e., `for (Entry entry : chain)...`).

> **Note**
> TODO: Implement `put`, initially ignoring the resizing part. You may want to open the lecture notes.

If you get stuck, expand the `Details` below for more directions and/or ask a TA:

<details>

Steps in your algorithm for `put`:
1. `put` should first get the index of the key, possibly by calling your helper function.
2. `put` should then check whether the table already has a `Chain` stored at that index. If it does not, you'll want to create a new `Chain`.
3. Now that you have a chain, look through the entries to see if the key already exists. If the key already exists, you'll need to make sure you are able to return the old value before you overwrite it with a new value.
4. If the key doesn't already exist, add it as a new entry. Be sure to update the size.

</details>

## Implementing `get`

Get should be a bit more simple to implement: your implementation should return the value if the key exists and otherwise return null. You'll want to make sure you're using the same transformation logic from key to index.

> **Note**
> TODO: Implement `get`, returning the value for a key if it exists and otherwise null.

If you get stuck, expand the `Details` below for more directions and/or ask a TA:

<details>

Steps in your algorithm for `get`:
1. `get` should first get the index of the key, possibly by calling your helper function.
2. `get` then needs to check whether the key exists within the bucket at that index.

</details>

Once you have the initial `get`, `put`, and `size` functionality, your code should pass the tests: `testConstructor`, `testSinglePutGet`, `testDoublePutGet`, and `testForceCollisions`.

## Implementing resizing

Now, your task is to add the fields and methods necessary to allow your hash table to grow as more elements are added. In particular, your backing array should _double in size_ when the current load factor reaches (is equal to) the maximum load factor passed to the constructor.

> **Note**
> TODO: First, implement the bodies of the methods for us to track resizing: `backingArraySize` and `currentLoadFactor`.

Now, think about how you actually want to resize the array. You'll likely want to add a new helper function that handles the resizing logic.

When should your new helper function be called?

Expand the `Details` to see our design:

<details>

Our solution calls a resize helper function at the end of `put` after a new key and value are added. 

</details>

> **Note**
> TODO: Update your implementation to handle double-resizing when the maximum load factor is reached. 

If you would like more directions, expand the `Details` below for more directions and/or ask a TA. This is the part we covered in less detail in class, so expand the detail sooner for more information.

<details>

Steps in your algorithm for resizing:
1. Check whether the the current load factor has reached the maximum load factor.
2. Calculate the new backing array size by doubling the current backing array size.
3. Create a local variable to store the old array.
4. Set the backing array to a new array with the new size.
5. Remember to update/reset any fields tracking the size, now that you have a new (empty) backing array.
6. Add the items from the old table to the new array. You can use your existing `put` method!

</details>

## The `Iterable` interface

You may have noticed that our `Chain` inner class implements `Iterable<Entry>`. Read/watch the [JavaHyperText page][iterable] for more information on how to work with this interface.

[iterable]: https://www.cs.cornell.edu/courses/JavaAndDS/iteratorIterable/iterator.html

## Handing in this submission

That's it!

Your submission should include the following files:
- `ResizableMap.java`
   - all methods required to make the tests compile/pass.

If you want, you can go back and try to improve your hash table implementation. Here are some ideas:
1. Implement more of the Java `Map` interface, starting with `remove`. You may need to update the definition of `Chain`. 
2. Use generic key/value types instead of `String`. 
3. Play around with different definitions of `hashCode()`. For example, one downside to Java's default implementation in `Object` is that because it is based on the memory address, it always starts from an integer that ends in 4 (due to how memory addresses are _aligned_). 
