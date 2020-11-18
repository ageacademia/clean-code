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


<a name="chapter4">
<h1>Chapter 4 -  Comments</h1>
</a>

## Good comments:
* Legal comments
* Information on string formatting, regex
* Explain intent 
* Clarification of an obscure argument
* Warn about consequences (this takes a long time to run!)
* TODOs (should be done, but can't be done at the moment).
* Amplify something as important
* Public APIs should have docs

## Bad comments:
* Redundant comments, comments that repeat what the code is saying
* Misleading: function returning _when_ vs _if_
* Don't have docs for every function or variable
* Don't have journal or log comments
* If a comment reads like it could be implemented, then refactor so comment can be removed
* HTML comments in source
* Don't give systemwide information in the context of a local comment
* Don't give too much information (technical details are likely too specific when describing a function)
* The connection between a comment and the code shouuld be obvious. If you're using something like a magic number, you should clearly reference what it means in the comment.

<a name="chapter5">
<h1>Chapter 5 -  Formatting</h1>
</a>

## Vertical Formatting
* Files are no more than 500 lines
* Files should be like reading a newspaper: Top: name, high-level concepts and algorithms, detail downward

### Vertical distance
Concepts that are closely related should be verticaly close to each other! Avoid having to hop around source to follow a _logical_ process

#### Dependent functions
The caller should be above the callee, so the program has a natural flow.

## Horizontal Formatting
* Lines should be short (Hollerith limit: 80)

### Horizontal density 
* Assignment statements have whitespace around them `a = 1`
* Precedence of operators (`b*b - 4*a*c`)

### Horizontal alignment
* It's not useful. 

<a name="chapter6">
<h1>Chapter 6 -  Objects and Data Structures</h1>
</a>

* Try to not expose the details of your data. Abstractions should hide information away from the user.
* Objects abstract away detail and expose functions to manipulate data. Data structures expose data but don't expose any meaningful functions.

## Law of Demeter
A module should not know about the innards of the objects it manipulates. 
For a function `f` of a class `C`:, only call methods of
* `C`
* Objects created by `f`
* Objects passed to `f`
* Objects in an instance variable of `C`
* Invoke only friends, not strangers

### Train wreck
`outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath()` looks like many coupled train cars. Instead look to split them up into separate lines.
If `ctxt` and co are data structures without behavior, then they naturally expose their structure, so there's no Demeter violation.

### Hybrids
Avoid hybrids (half object and data structure)

### Hiding Structure
What if `ctxt` and co are actual objects? We shouldn't try to encapsulate the exact behavior we want into one method, because that leads to combinatorial explosion of methods.
If we look at its implementation, we should instead move the logic to the `ctxt` object instead.

<a name="chapter7">
<h1>Chapter 7-  Error Handling</h1>
</a>

* Write your Try-Catch statement first. This is so that the scope is defined first (and you think in terms of a transaction). Have your tests force exceptions, then change the behavior of the handler to pass the tests.
* Exceptions should have enough context to determine the source of an error (?)
* Create a class that handles a special case for you. This way, the client code doesn't have to deal with the exception, and you don't resort to pushing error handling to the edges of your code.
* Don't return `null` when handling errors.
* Don't pass `null` when handling errors (can you forbid passing null to an argument? maybe a default value?)

<a name="chapter8">
<h1>Chapter 8-  Boundaries</h1>
</a>

## Third party code
* If you're using `java.util.Map` for example, we might pass the object around, but we might not need all the functionality that `Map` provides.
* If you need to retrieve objects from the map, you'll either need to require generics `Map<Sensor> sensors`, or casting `(Sensor)sensors.get...`

A cleaner way is to make a boundary on `Map` so only the necessary methods are exposed:
```java 
public class Sensors {
 private Map sensors = new HashMap();
 
 public Sensor getById(String id) {
  return (Sensor) sensors.get(id);
 }
}
```

## Write tests that check understanding of API.

## Using Code that doesn't exist 
* If you're working with another team, you can define your own interface (that you _wish_ they could provide), and then build out whatever you need under those assumptions.
* Interesting things happen at boundaries. Good software design should be able to accomodate change wihtou significant rework.

<a name="chapter9">
<h1>Chapter 9 - Unit Tests</h1>
</a>

Three Laws of Test Driven Development (TDD)
* Avoid writing production code until a unit test has been written.
* Only write enough of a unit test that is sufficient to fail. 
* Only write enough production code to pass a test.

Build-Operate-Check pattern: build the test data, then operate (mutate) on it, and then check the changes. You can refactor the test logic so that the three-part logic is evident from reading. 

Minimize number of asserts per concept, and one concept per test.
Test should be 
* Fast
* Independent of one another
* Repeatable in different environments
* Clearly state if they pass or fail
* Written timely 

<a name="chapter10">
<h1>Chapter 10 - Classes</h1>
</a>

* Public static constants 
* Private variables
* Private instance variables 
* Public functions
* Private functions (stepdown rule)

Don't make classes too big. For example, class names including weasel words like Processor or Manager or Super often hint at unfortunate aggregation of responsibilities. We should also be able to write a brief description of the class in about 25 words, without using the words “if,” “and,” “or,” or “but.”

## Organizing for Change
Knowing when a class violates SRP (Single Responsibility Principle):
* Private method behavior that applies only to a subset of the class

DIP (Dependency Inversion Principle):
* Classes depend on abstraction instead of detail (think interfaces, base classes, instead of derived/children classes)

<a name="chapter11">
<h1>Chapter 11 - Systems</h1>
</a>

## Separation of concerns

### Abstract factories 
(you can use them to abstract away the construction of objects away from the main application). This allows dependencies to point _away_ from main, but main still retains control over when and how objects are built. 
```
 main   -- > application
 |            |
 |            |
 v            v
builder -- > object
```

### Dependency Injection
An object receives other objects it depends on, as opposed to creating them itself

### Cross-cutting concerns
Aspect-oriented programming (AOP). You declare which in-memory objects (entity beans) should be persisted, and then delegate the act of persistence to a persistence framework.
* Big Design Up Front (BDUF) is harmful because it inhibits adaptability
* Postpone decisions until the last possible moment (so you have more information)
