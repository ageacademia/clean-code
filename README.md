- [Chapter 1 - Clean Code](#chapter1)
- [Chapter 2 - Meaningful Names](#chapter2)
- [Chapter 3 - Functions](#chapter3)
- [Chapter 4 - Comments](#chapter4)
- [Chapter 5 - Formatting](#chapter5)
- [Chapter 6 - Objects and Data Structures](#chapter6)
- [Chapter 7 - Error Handling](#chapter7)
- [Chapter 8 - Boundaries](#chapter8)
- [Chapter 9 - Unit Tests](#chapter9)
- [Chapter 10 - Classes](#chapter10)
- [Chapter 11 - Systems](#chapter11)
- [Chapter 12 - Systems](#chapter12)
- [Chapter 13 - Concurrency](#chapter13)
- [Chapter 14 - Successive Refinement](#chapter14)
- [Chapter 15 - JUnit Internals](#chapter15)
- [Chapter 16 - Refactoring SerialDate](#chapter15)
- [Chapter 17 - Smells and Heuristics](#chapter17)

<a name="chapter1">
<h1>Chapter 1 -  Clean Code</h1>
</a>

Leblanc's law: _Later equals never_
> A new tiger team is selected. Everyone wants to be on this team because it’s a greenfield project. They get to start over and create something truly beautiful. But only the best
and brightest are chosen for the tiger team.

> Indeed, the ratio of time spent reading vs. writing is well over 10:1.
We are constantly reading old code as part of the effort to write new code

Boy Scout Rule: _Leave the campground cleaner than you found it._

<a name="chapter2">
<h1>Chapter 2 -  Meaningful Names</h1>
</a>

* Names should reveal intent, and be explicit. Favor _explicity_ over _simplicity_. What kinds of items are being manipulated? What is the significance of the subscript? What is the significance of the magic number? How would I use the return value?
* Avoid false clues. Don't re-use names of Unix variable names, don't use the word "List" in a name unless it actually is a `List`.
* Avoid number-series naming (`a1, a2... aN`) and noise words (`ProductInfo`, `ProductData`), because it's hard to differentiate the uses between each name.
* Use pronounceable names
* Don't encode type or scope into names. For strongly typed languages (like Java), the type can actually change, i.e. `PhoneNumber phoneString`.
* Avoid mental mapping. Use problem-domain terms or solution domain terms. 
  * Classes should have nouns, not verbs
  * Methods should have verbs. Accessors prefixed with `get`, mutators with `set`, and predicates with `is`.
  * Use static factory methods that describe the arguments. `Complex.FromRealNumber(23.0)`
* Avoid culture-dependent jokes, clever/punny names
* One word per concept, across the codebase. Use only one of `fetch`, `get`, `retrieve` to achieve the same purpose in different classes.
* If a variable name by itself is vague (`state`), add context to make it more meaningful `firstName`, `lastName`, put it in an `Address` class
* If a class name is redundant or too specific/rigit (`GasStationDeluxeAccountAddress`), shorten it or define a more appropriate name
