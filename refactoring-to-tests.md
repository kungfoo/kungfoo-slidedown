# Refactoring to test
## Or: How can tests provide design feedback?
## Or: What tests do we want to write?

***

## 💊 Why do we test?

- We want our code to work _now_ and _continue to work in the future_.
- QA should find _nothing_.
- End users should find _even less_.
- Tests are our safety net for fearless change and refactoring.

***

## 🤞 On test code

Good code with good design (on the API/component/class level) should be _easy_ and _fast_ to test.

Test code should be treated with the _same care_ as all other code is; after all tests are your safety net when changing things.

The simplicity of testing something (or lack thereof) can be seen as direct systems design feedback and as such provides additional value over the mere checking of things working.

***

## 🤖 On test automation

Why do we automate?

![build pipeline](images/refactoring-to-tests/build-pipeline.png)

***

## 🐪 The testing pyramid

![testing pyramid](images/refactoring-to-tests/testing-pyramid.png)


[Martin Fowler, Practical Testing Pyramid][1]

***

## 🐪 The testing pyramid

While a great metaphor, it falls short in some areas:

- naming is quite vague
- a bit simplistic
- just call your tests 'service tests' and you're okay.

Other people tried different naming and other improvements, but the idea remains the same.

***

## 👍 What we want

- good coverage
- blazing fast to run
- no false positives/negatives and flakes
- simple to maintain
- simple to add new test cases

***

## 👍 What we want

Ground rules:

1. Write tests with different granularity.
2. The more high-level you get, the fewer tests you should have.

*** 

## 👍 What we want

In different words:

> Stick to the pyramid shape to come up with a healthy, fast and maintainable test suite: Write _lots_ of small and fast _unit tests_. Write _some_ more _coarse-grained tests_ and _very few_ high-level tests that test your application from _end to end_. 
&mdash; <cite>[Martin Fowler][1]</cite>

***

## 👎 What we don't want

![ice cream cone of tests](images/refactoring-to-tests/testing-icecream-cone.jpg)

***

## 📋 Typical test structure

1. Set up test data/objects.
2. Call method/system under test.
2. Assert that the expected results are returned.

Also known as: Arrange, Act, Assert

Or: Given, When, Then


***

## ✅ On the subject of unit tests 

What is a unit anyway?

There really is no singular definition and there need not be.

In a functional language it might be one single function, in an object-oriented language it is probably more like a single class instance. For some systems it even may be some classes collaborating.

***

## ❗ What is definitely not a unit test?

- Executing HTTP Requests
- Hitting a real DB instance over the network
- Setting up the entire system and clicking around on the UI

However, UI tests can still be considered unit tests in modern frameworks, if we use the proper techniques to mock all collaborators. More on that later.

***

## 👷 👨‍🏭 Solitary and sociable unit tests

A unit test typically replaces external collaborators with test doubles (come back to that later).

![unit test](images/refactoring-to-tests/unit-test.png)

***

## 👷 👨‍🏭 Solitary and sociable unit tests

![sociable and solitary testing](images/refactoring-to-tests/solitary-sociable.png)

***

## 👷 👨‍🏭 Solitary and sociable unit tests

Some testers prefer and systems can be more favorable towards: 

- sociable unit tests
- solitary unit tests

It is probably beneficial to use both to get your test suite to be amazing.

_You should discuss this when pairing and when reviewing code._

***

## 🙉 When to definitely opt for solitude

- Remove non-determinism when talking to remote services
- Remove speed blocks (slow collaborators)
- Most times: when talking to a database, filesystem, API


***

## ✅ Making code testable

From [SOLID principles][4]

- *S*ingle responsibility principle
- *D*ependency inversion principle

***

## Single responsibility principle

A class (can also be a function) should have a single responsibility. In [Uncle Bob][3]'s terms, it should only have _one and only one reason to change_.

***

## 👩‍⚕️ Employee

```java
public class Employee {
  public Money calculatePay() {}
  public String reportHours() {}
  public void save() {}
}
```

Reasons to change:

- Salary spec changes
- Reporting format changes
- Database schema or how to talk to it changes
- ...

***

## 👩‍⚕️ Employee (much improved)

```java
public class Employee {
  public Money calculatePay() {}
}

public class EmployeeReporter {
  public String reportHours(Employee e) {}
}

public class EmployeeRepository {
  public void save(Employee e) {}
}
```

These are much better separated into single responsibilities. You can see that the conflicting reasons to change have been separated into separate classes.

***


## Dependency Inversion Principle

> Depend upon Abstractions. Do not depend upon concretions.
&mdash; [Design Principles and Design Patterns][4]

 Every dependency in the design should target an interface, or an abstract class. No dependency should target a concrete class.

***

## 💉 Dependency Injection

One of the most common places where you depend on concrete implementations is _instantiation_.

Dependency injection solves this by allowing the concrete instances to be _injected_ into the actual code via a mechanism.

There are frameworks to help you assemble the runtime object graph, but usually we can do it by hand and wire things up that way.

***

## 💉 Dependency Injection

```java
public class PaymentProcessor {
  private final CreditCardService paymentService =
    new CreditCardService(...);

  public Result process(Payment payment) {
    var result = new Result();
    result.with(paymentService.validate(payment));
    if(result.isValid()) {
      return paymentService.process(payment);
    }
    return result;
  }
}
```

- Impossible to replace credit card payments with something else.
- Hard to test without hitting the actual credit card service interface.

***

## 💉 Dependency Injection

```java
public interface PaymentService {
  Result validate(Payment payment);
  Result process(Payment payment);
}

public class PaymentProcessor {
  public final PaymentService paymentService;

  public PaymentProcess(PaymentService paymentService) {
    this.paymentService = paymentService;
  }

  public Result process() { /* same as before */ }
}
```

***

## 💉 Dependency Injection

Beware of `static` utility helper things: They are next to impossible to inject/substitute and should in general be steered away from for anything you may want to substitute.

Of course, don't follow this dogmatically but apply some solid (no pun intended) reasoning.

***

## 🕵 Mocks, Stubs, Spies, Fakes

Once we have dependencies injectable we start talking about providing _test doubles_ for them. These need not be _mocks_!

- Fakes: Working implementations, but take shortcuts (return an empty list)
- Stubs: Provide canned answers to calls made during tests.
- Spies: Stubs that also record some information on how they were called.
- Mocks: pre-programmed with canned answers and expectations which form a specification of the calls they are expected to receive.


***


# ❓ Questions

***

# 👟 Exercise time 

***

## Setup

1. Form groups of 2-3 people, that will pair-/mob-program.
2. Grab _one_ computer and _one_ large screen.
3. Clone Exercise repo: `git clone git@github.com:beekpr/refactoring-to-test-workshop.git`

***

## 👟 Exercise 1

1. Find out what tests already exist.
2. Discuss what type of tests they are and what they are actually testing.
3. Share back.

***

## 🏃‍♂️ Exercise 2

1. Can you restructure the code so it can be tested more easily?
2. Give it a try and refactor the tests along with the changes.

***

## 🧪 Proposed analysis

***

## ✅ Tests

- There is one _integration test_ actually calling the REST API.
- It is testing a fair amount of things, but has to fiddle with HTTP things and details quite a bit.
- It is still quite fast to run on a decent machine.
- Fairly low level in terms of abstraction and types being used, things are basically `Maps` and `String`.

***

## 💻 System under test

It appears the endpoint for `POST /message` is doing a multitude of things:

- Deal with HTTP layer
- Validate phone numbers
- Validate text messages to send
- Send the actual messages
- Deal with exceptions that can happen while sending
- ...

***

## 💻 System under test

There is however no other way to test this code, short of invoking the methods directly and mocking http things, which seems messy.

Testing the handling of upstream failures such as network errors is _really hard_.


***

## 🧪 Proposed solution

- `TextMessage`, as a value type for text messages
- `TextMessageValidator` to encode the invariants of text messages
  - Must be of adequate length
  - Must use a valid phone number, country code etc.
  - May not contain placeholder text anymore
- `TextMessageValidationResult` as a type to collect multiple errors
  - So we can be nice to the user
  - So we can be nice to people testing things.

***

## 🧪 Proposed solution

All of these can easily be unit tested in isolation of any of the other details.

We can ensure all the invariants are met, that we have nice error messages and so on...

***

## 🧪 Proposed solution

```java
@Test
public void invalidCountryShouldReturnANiceError() {
  given("+999123456789", "This is a test message.")
    .isInvalid("Please use a valid country code");
}
```
Use a builder for test cases, for fluent, prose-like building of your tests.

Or use _spock_ for data driven tests (will be discussed in a future session).

***

## 🧪 Proposed solution

Introduce a service interface

```java
public interface TextMessageService {
  SentTextMessage send(TextMessage message)
    throws TextMessageServiceException;

  Optional<SentTextMessage> fetch(String messageId)
    throws TextMessageServiceException;
}
```

***

## 🧪 Proposed solution

Enable dependency injection on the REST API resource:

```java
public class MessageResource {
  public MessageResource(
    TextMessageService messageService,
    TextMessageValidator messageValidator
  ) { /* ... */ }
}
```

***

## 🧪 Proposed solution

Now you can add test cases for when the collaborator service throws due to network blips, solar winds and other oddities easily.

***

## 🧪 Proposed solution

```java
@Test
public void itShouldHandleUpstreamErrorsAs500s() {
  when(messageService.send(any(TextMessage.class)))
    .thenThrow(
      new TextMessageServiceException("500 Internal Service Error")
    );

  Response response = messageResource
    .post("+1230000", "This is a text message");

  assertThat(response.getStatus(), is(500));
}
```

***

# ⛳ The End

***

# Sources

A lot of stuff is from Martin Fowler's and Uncle Bob's blogs:

- [Practical Test Pyramid][1]
- [Test Double][2]

***

# Excursion 1

***

## 🦆 Liskov substitution principle

> Let `ϕ(x)` be a property provable about objects `x` of type `T`. Then `ϕ(y)` should be true for objects `y` of type `S` where `S` is a subtype of `T`.


***

## 🦆 Liskov substitution principle

![quacks like a duck](images/refactoring-to-tests/liskov.png)

***

## 🦆 Liskov substitution principle

> Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program. 

See [Uncle Bob, Design Principles and Design Patterns][4]


[1]:https://martinfowler.com/articles/practical-test-pyramid.html
[2]:https://martinfowler.com/bliki/TestDouble.html
[3]:https://github.com/97-things/97-things-every-programmer-should-know/tree/master/en/thing_76
[4]:https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf