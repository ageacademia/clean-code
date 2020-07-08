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

<a name="chapter3">
<h1>Chapter 3 - Functions</h1>
</a>

* Lots of little functions
* For branching, like `if`, `else`, `while` - blocks inside should be 1 line long
* Functions should do one thing. That should be clear from the stated name of function - its contents are one level of abstraction below. If you can extract another function with a name that is not a restatement of its implementation, it's doing more than one thing.
* Function implementation should be on the same level of abstraction. `append` to a string is not the same level as `render(pagePath)`.
* [Open closed principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle) - program behavior can be extended _without_ access to source
* [Single responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle) - there should only be one reason for a class to change
* `switch` statements should appear once, are used to polymorphic objects, and hidden behind an inheritance relationship. 
```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
 switch (e.type) { 
  case COMMISSIONED:
   return calculateCommissionedPay(e); 
  case HOURLY:
   return calculateHourlyPay(e); 
  case SALARIED:
  return calculateSalariedPay(e); 
 default:
  throw new InvalidEmployeeType(e.type); 
 }
}
```
The above example violates many good principles (like the two above), but it's also very inflexible - if you have to implement other necessary functions like `isPayday(Employeee e, Date date)`, or `deliverPay(Employee e, Money pay)`, you would inevitably require the same `switch` statement structure. For this case, the solution is to use an abstract factory, so `switch` is guaranteed to be only used once, since the factory will polymorphically dispatch a `Employee` object, which implements the necessary logic.
* Long descriptive names are better than long descriptive comments. Try to let them tell a story (so that you would logically expect how the function to be implemented, and also helps you sanity check!)

### Function arguments
* Avoid three (triadic) arguments where possible.
* Try to reduce two (dyadic) arguments to one (monadic).

#### Monadic Forms
* Good: ask a question about an argument. Transform the argument, and return something new. Use the argument to _alter_ the system (be very explicit about this!)
* Bad: Using an output argument instead of a return value. If your function has to change the state of something, have it change the state of the owning object.

#### Flag arguments
* Try to avoid passing flags as arguments. Just split the function into two.

#### Argument Lists
* Functions that take variable arguments can be monads, dyads or triads, but it must be explicitly grouped. For example:
`public String format(String format, Object... args)` is clearly a dyad, but can take as many parameters as the `format` string requires.

#### Verbs and Keywords
* Function: verb. Argument: noun. e.g. `writeField(name)`
* `assertEquals` -> `assertExpectedEqualsActual(expected, actual)`.

### No Side Effects
* don't do _hidden_ things, which aren't explicitly returned or stated by the function name.

### Exceptions instead of Error Codes
* Exceptions prevent deeply nested structures, where you'd have to return error codes each time.
* Put try/catch blocks in their own functions, so there is nice separation
* Nothing after catch/finally block.

### How to write functions
Keep editing and revising it until it reads well and follows the rules
