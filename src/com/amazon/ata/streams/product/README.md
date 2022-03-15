# Product Page

## Introduction

Let's play some Code Golf!

In regular golf, players try to get the ball in the cup in the fewest number of strokes.
In traditional Code Golf, we try to replace code with the fewest number of characters,
but that almost always leads to unreadable code.
We really like meaningful variable names and readable code, so we're going to count "strokes" instead.

We've pulled four methods from `code.amazon.com`
and converted them to the "imperative" style that you've already explored.
They all relate to information you find on a product page: buying options,
the main images, extra "look" images, and related products with price filter,
shipping filter, and sort method.
You can see them in the `streams/product/ProductPage.java` class.

The supporting classes are in `product/types`. You *will not* need to use any methods
from the supporting classes that aren't already in `ProductPage.java`. You won't need
to know more than a method's inputs and outputs to complete this activity.

The unit tests in the `ProductPageTest` class are currently passing.

You're going to rewrite four methods in `ProductPage.java` to the more readable "fluent" style,
which uses call chains. Builders use fluent style to make object constructors more readable.
Streams use fluent style to make code more readable.

Most Amazon teams prefer fluent code because it "flows" better, making it easier to read.

Following convention, any method in this activity that returns a `Collection` will never return `null`, but will instead
return an empty `Collection`.

### Counting Strokes

What counts as a "stroke"? No set of rules could truly describe what makes
code readable, so we tried to distinguish between code that tells us to do
something and code that forces us to remember or think about something.

Here's what we came up with:

#### 0 strokes (free!)

* Method declarations, no matter how many lines they take up.
* Comments
* `for` and other loop declarations
* `map` and `flatMap` (they're the equivalent of `for` in fluent style)
* `if` and `else` statements
* `filter` and `orElse` (the equivalents of `if` and `else`) in fluent style
* braces (`{`, `}`), parentheses (`(`, `)`), and square braces (`[`, `]`)

#### 1 stroke

* Constructors
* Comparisons (aka relational operators) (e.g. `>`, `<`, `==`, `!=`)
* Conditional Operators, including:
  * Boolean operators (e.g. `||`, `&&`)
  * [Ternary operators (e.g. `<condition> ? <value1> : <value2>`)](hints/hint-01.md)
* Method calls, *including* method references and calls in lambda expressions
* `return`

#### 2 strokes
* Assignment (`=`): it requires a variable, and we can read code more easily if we don't have to track many variables.

Here's an example of counting strokes.
In each case, `getManager()` returns the alias of the provided employee's manager,
and `getDirects()` returns a `List` of the given manager's direct reports.

An imperative example:
```java
    // Golf score: 9
    public List<String> getCoworkers(List<String> names) { // Declarations are free!
        List<String> result = new ArrayList<>();           // 3: one assignment and one constructor
        for (String name : names) {                        // 0: loop declaration, no method calls or assignments
            String manager = getManager(name);             // 3: one assignment, one method call
            result.addAll(getDirects(manager));            // 2: two method calls
        }                                                  // 0: just braces
        return result;                                     // 1: return
    }                                                      // 0: just braces

}
```

In this fluent example, we use `flatMap` to turn a `Stream<List<String>>` (representing direct reports) into a 
a flat `Stream<String>`:
```java
    // Golf score: 7
    public List<String> getCoworkers(List<String> names) { // Declarations are free!
        return names.stream()                              // 2: one return, one method call
            .map(this::getManager)                         // 1: map is free, one method reference
            .map(this::getDirects)                         // 1: map is free, one method reference
            .flatMap(List::stream)                         // 1: flatMap is free, one mehod reference
            .collect(Collectors.toList());                 // 2: 2 method calls
    }                                                      // 0: braces are free
}
```

## Phase 0: Tee Up
Run the `ProductPageTest` to verify everything builds and passes tests.

## Phase 1: The drive

Convert the `getFirstBuyingOption()` method in `ProductPage` to use streams.

After converting the method, run the `ProductPageTest` test class
to ensure that you haven't missed any functionality. 

## Phase 2: The approach

Convert the following methods in `ProductPage` to use streams:
* `extractMainImageUrl()`
* `extractLookImageUrl()`

After converting the method, run the `ProductPageTest` test class
to ensure that you haven't missed any functionality.

## Phase 3: The putt

Convert the `getSimilarProducts()` methods in `ProductPage` to use streams.

After converting the method, run the `ProductPageTest` test class
to ensure that you haven't missed any functionality.

## Phase 4: Tally it up

Each method in Phases 1-3 above includes its original golf score, along with its "par": the golf score of the solution
that ATA Instructors coded with streams.

Now that you have converted each method to use streams, count the number of strokes each method takes, and add your
score to the javadoc for the method. Next, sum up your scores across all methods. How did you do? Do you think you can
drive your score lower? Compare your score to others in your group and collectively find what patterns can help lower
your score.

## Phase 5: Sometimes you end up in the bunker

You may notice some... questionable code choices in this Java package.
We mentioned that we pulled this code from Code Browser,
which means it doesn't necessarily follow Amazon's *current* best practices.

In particular, you may find classes that can return both `null` and an empty collection.
You'll encounter this sort of thing on-the-job, too;
you should advocate for improving the code, while completing your sprint tasks.
For now, we ask that you work within the bounds of the existing method behavior.

Once you finish converting the methods, consider these questions:

* Did the developer intend for `null` to mean something different from an empty collection?
* Do you do anything different when you receive a `null` or an empty collection?
* How would you update these classes to improve readability?
* How would that affect the code's golf score?

## Extension: Break out the sand wedge

Implement the changes you say you would update when answering the questions from Phase 5,
focusing on where you prefer an empty collection or `Optional` to `null`. 

Did your changes improve the code's readability? Did they affect the code's golf score?
