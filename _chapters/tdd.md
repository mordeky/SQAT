---
chapter-number: 11
title: Test-Driven Development
layout: chapter
toc: true
author: Maurício Aniche
---

## Introduction

We are used to write some production code and, once we are finished, we move to writing the tests.
One disadvantage of this approach is that we might create the tests only much later.
Test-Driven Development (TDD) suggests the opposite: why don't we start by writing the tests and only then production code?

The TDD cycle is illustrated in the diagram below.

![Test Driven Development Cycle](/assets/img/tdd/tdd_cycle.svg)

The first think of the test. 
Then we write the test.
This test will fail, because we have not written the code yet.
Now that we have a failing test, we write code that makes the test pass.

And we do all of this in the simplest way we can.
This simplicity means that we create the simplest implementation that solves the problem and we start with the simplest possible test case.
After we wrote the code that makes the test pass, it is time to refactor the code we just made.

{% assign todo = "Missing a video about how TDD works here" %}
{% include todo.html %}


TDD has some advantages:

* By creating the test first, we also look at the requirements first.
This makes us write the code for the specific problem that it is supposed to solve.
In its turn, this prevents us from writing useless, unnecessary code.

* We can control our pace of writing the production code.
Once we have a failing test, our goal is clear: To make the test pass.
With the test that we create, we can control the pace in which we write the production code.
If we are confident in how to solve the problem, we can make a big step by creating a complicated test.
However, if we are not sure how to tackle the problem, we can break it into small parts and start with the simpler pieces first by creating tests for those pieces.

* The tests are derived from the requirements.
Therefore, we know that, when the tests pass, the code does what it is supposed to do.
When writing tests from the source code instead, the tests might pass while the code is not doing the right thing.
The tests also show us how easy it is to use the class we just made.
We are using the class directly in the tests so we know immediately when we can improve something.

* Related to the previous point is the way we design the code when using TDD.
Creating the tests first makes us think about the way to test the classes before implementing them.
It changes the way we design the code.
This is why Test Driven Development is sometimes also called Test Driven Design.
Thinking about this design earlier is also better, because it is easier to change the design of a class early on.
You probably have to change less if you just start with a class than when the class is utilized by other parts of the system. We discuss more about Test-Driven Design later.

* Writing tests first gives faster feedback on the code that we are writing.
Instead of writing a lot of code and then a lot of tests, i.e. getting a lot of feedback at once after a long period of time, we create a test and then write a small piece of code for that test.
Now we get feedback after each test that we write, which is usually after a piece of code that is much smaller than the pieces of code we test at once in the traditional approach.
Whenever we have a problem it will be easier to identify it, as the added code is smaller.
Moreover if we do not like anything it is easier to improve the code.

* We already talked about the importance of controllability when creating automated tests.
By thinking about the tests before implementing the code we automatically make sure that the code is easy to control.
This improves the code quality as it will be easier to test the controllable code.

* Finally, the tests that we write can indicate some problems in the code.
Too many tests for just one class can indicate that the class has too many functionalities and that it should be split up into more classes.
If we need too many mocks inside of the tests the class might be too coupled, i.e. it needs too many other classes to function.
If it is very complex to set everything up for the test, we may have to think about the pre-conditions that the class uses.
Maybe there are too many or maybe some are not necessary.

Given all these advantages, should we use TDD 100% of time?
There is, of course, no universal answer. While some research shows the advantages of TDD, others are more in doubt about it.

{% assign todo = "Missing a video about the advantages of TDD here" %}
{% include todo.html %}

## Test-Driven Design

Will I create a better class design if I use Test-Driven Development? Well, yes and no; TDD doesn't do magic. In the following, we discuss the effects of the practice, and how the practice can help developers during class design.

Developers commonly argue that TDD helps software design, improving internal code quality. For example, Kent Beck [1], Robert Martin [2], Steve Freeman [3], and Dave Astels [4], state in their books (without scientific evidence) that the writing of unit tests in a TDD fashion promotes significant improvement in class design, helping developers to create simpler, more cohesive, and less coupled classes. They even consider TDD as a class design technique [5], [6]. Nevertheless, the way in which TDD actually guides developers during class design is not yet clear.

After interviewing some developers, we observed the following.

### TDD does not drive to a better design by itself

Participants affirmed that the practice of TDD did not change their class design during the experiment. The main justification was that their experience and previous knowledge regarding object-orientation and design principles guided them during class design. They also affirmed that a developer with no knowledge in object-oriented design would not create a good class design just by practicing TDD or writing unit tests.

The participants gave two good examples reinforcing the point. One of them said that he made use of a design pattern he learned a few days before. Another participant mentioned that his studies on SOLID principles helped him during the exercises.

The only participant who had never practiced TDD before stated that he did not feel any improvement in the class design when practicing the technique. Curiously, this participant said that he considered TDD a design technique. It somehow indicates that the popularity of the effects of TDD in class design is high. That opinion was slightly different from that of experienced participants, who affirmed that TDD was not only about design, but also about testing.

However, different from the idea that TDD and unit tests do not guide developers directly to a good class design, all participants said that TDD has positive effects on class design. Many of them mentioned the difficulty of trying to stop using TDD or thinking about tests, what can be one reason for not having significant difference in terms of design quality in the code produced with and without TDD.

"When you are about to implement something, you end up thinking about the tests that you’ll do. It is hard to think "write code without thinking about tests!". As soon as you get used to it, you just don’t know another way to write code..."

According to them, TDD can help during class design process, but in order to achieve that, the developer should have certain experience in software development. Most participants affirmed that their class designs were based on their experiences and past learning processes. In their opinion, the best option is to link the practice of TDD and experience.

### Baby steps and simplicity

TDD suggests developers to work in small (baby) steps; one should define the smallest possible functionality, write the simplest code that makes the test green, and do one refactoring at a time.

In the interviews, participants commented about this. One of them mentioned that, when not writing tests, a developer thinks about the class design at once, creating a more complex structure than needed.

One of the participants clearly stated how he makes use of baby steps, and how it helps him think better about his class design:

“Because we start to think of the small and not the whole. When I do TDD, I write a simple rule (...), and then I write the method. If the test passes, it passes! As you go step by step, the architecture gets nice. (...) I used to think about the whole (...). I think our brain works better when you think small. If you think big, it is clear, at least for me, that you will end up forgetting something. ”

### Refactoring confidence

Participants affirmed that, during the process of class design, changing minds is constant. After all, there is still a small knowledge about the problem, and about how the class should be built. This was the most mentioned point by the participants. According to them, an intrinsic advantage of TDD is the generated test suite. It allows developers to change their minds and refactor all the class design safely. Confidence, according to them, is an important factor when changing class design or even implementation.

"It gives me the opportunity to learn along the way and make things differently. (…). The test gives you confidence."

A participant even mentioned a real experience, in which TDD made the difference. According to him, he changed his mind about the class design many times and trusted the test suite to guarantee the expected behavior.

### A safe space to think

In an analogy done by one of the participants, tests are like draft paper, in which they can try different approaches and change their minds about it frequently. According to them, when starting by the test, developers are, for the first time, using their own class. It makes developers look for a better and clearer way to invoke the class’ behaviors, and facilitate its use:

"Tests help you on that. They are a draft for you to try to model it the best way you can. If you had to model the class only once, it is like if you have only one chance. Or if you make it wrong, fixing it would give you a lot of work. The idea of you having tests and thinking about them, it is like if you have an empty draft sheet, in which you can put and remove stuff because that stuff doesn’t even exist."

We asked their reasons for not thinking on the class design even when they were not practicing TDD or writing tests. According to them, when a developer does not practice TDD, they get too focused on the code they are writing, and thus, they end up not thinking about the class design they were creating. They believe tests make them think of how the class being created will interact with the other classes and of the easiness of using it.

One of the participants was even more precise in his statement. According to him, developers that do not practice TDD, as they do not think about the class design they are building, they end up not doing good use of OOP. TDD forces developers to speed down, allowing them to think better about what they are doing.

### Rapid feedback

Participants also commented that one difference they perceived when they practiced TDD was the constant feedback. In traditional testing, the time between the production code writing and test code writing is too long. When practicing TDD, developers are forced to write the test first, and thus receive the feedback a test can provide sooner.

One participant commented that, from the test, developers observe and criticize the code they are designing. As the tests are done constantly, developers end up continuously thinking about the code and its quality:

*"When you write the test, you soon perceive what you don’t like in it (...). You don’t perceive that until you start using tests."*

Reducing the time between the code writing and test writing also helps developers to create code that effectively solves the problem. According to the participants, in traditional testing, developers write too much code before actually knowing if it works.

### The search for testability

Maybe the main reason for the practice of TDD helping developers in their class design is the constant search for testability. It is possible to infer that, when starting to produce code by its test, the production code should be, necessarily, testable.

When it is not easy to test a specific piece of code, developers understand it as a class design smell. When this happens, developers usually try to refactor the code to make it easier to test. A participant also affirmed that he takes it as a rule; if it is hard to test, then the code may be improved.

Feathers [7] raised this point: the harder it is to write the test, the higher the chance of a class design problem. According to him, there is a strong synergy between a highly testable class and a good class design; if developers are looking for testability, they end up creating good class design; if they are looking for good class design, they end up writing testable code.

Participants went even further. During the interviews, many of them mentioned patterns that made (and make) them think about possible design problems in the class they build. As an example, they told us that when they feel the need to write many different unit tests to a single method, this may be a sign of a non-cohesive method. They also said that when a developer feels the need to write a big scenario for a single class or method, it is possible to infer that this need emerges in classes dealing with too many objects or containing too many responsibilities, and thus, it is not cohesive. They also mentioned how they detect coupling issues. According to them, the abusive use of mocking objects indicates that the class under testing has coupling issues.

### What did we learn from it?

The first interesting myth contested by the participants was the idea that the practice of TDD would drive developers towards a better design by itself. As they explained, the previous experience and knowledge in good design is what makes the difference; however, TDD helps developers by giving feedback by means of the unit tests that they are writing constantly. As they also mentioned, the search for testability also makes them rethink about the class design many times during the day — if a class is not easy to be tested, then they refactor it.

We agree with the rationale. In fact, when comparing to test-last approaches, developers do not have the constant feedback or the need to write testable code. They will have the same feedback only at the end, when all the production code is already written. That may be too late (or expensive) to make a big refactoring in the class design.

![TDD feedback](/assets/img/tdd/tdd.svg)

We also agree with the confidence when refactoring. As TDD forces developers to write unit tests frequently, those tests end up working as a safety net. Any broken change in the code is quickly identified. This safety net makes developers more confident to try and experiment new design changes — after all, if the changes break anything, tests will warn developers about it. That is why they also believe the tests are a safe space to think.

Therefore, we believe that is is not the practice by itself that helps developers to improve their class design; but it is the consequences of the constant act of writing a unit test, make that class testable, and refactor the code, that drives developers through a better design.

Conclusion: Developers believe that the practice of test-driven development helps them to improve their class design, as the constant need of writing a unit test for each piece of the software forces them to create testable classes. These small feedbacks — is your test easy to be tested or not? — makes them think and rethink about the class and improve it. Also, as the number of tests grow, they act as a safety net, allowing developers to refactor freely, and also try and experiment different approaches to that code. Based on that, we suggest developers to experiment the practice of test-driven development, as its effects look positive to software developers.

## References

* Beck, K. (2003). Test-driven development: by example. Addison-Wesley Professional.

* [1] Beck K (2002) Test-driven development by example. 1° edn. Addison-Wesley Professional, Boston, USA.

* [2] Martin R (2006) Agile principles, patterns, and practices in C#. 1st edition. Prentice Hall, Upper Saddle River.

* [3] Steve Freeman, Nat Pryce (2009) Growing object-oriented software, Guided by Tests. 1° edn. Addison-Wesley Professional, Boston, USA.

* [4] Astels D (2003) Test-driven development: a practical guide. 2nd edition. Prentice Hall.

* [5] Janzen D, Saiedian H (2005) Test-driven development concepts, taxonomy, and future direction. Computer 38(9): 43–50. doi:10.1109/MC.2005.314.

* [6] Beck K (2001) Aim, fire. IEEE Softw 18: 87–89. doi:10.1109/52.951502.

* [7] Feathers M (2007) The deep synergy between testability and good design. https://web.archive.org/web/20071012000838/http://michaelfeathers.typepad.com/michael_feathers_blog/2007/09/the-deep-synerg.html.

## Exercises


{% include exercise-begin.html %}
We have the following skeleton for a diagram illustrating the Test Driven Development cycle.
What words/sentences should be at the numbers?

![Test Driven Development exercise skeleton](/assets/img/tdd/exercises/tdd_skeleton.svg)

(Try to answer the question without scrolling up!)
{% include answer-begin.html %}

1. Write failing test
2. Failing test
3. Make test pass
4. Passing test
5. Refactor

From the explanation above:

![Test Driven Development Cycle](/assets/img/tdd/tdd_cycle.svg)

{% include exercise-answer-end.html %}




{% include exercise-begin.html %}
Remember the `RomanNumeral` problem?

```
**The Roman Numeral problem**

It is our goal to implement a program that receives a string as a parameter
containing a roman number and then converts it to an integer.

In roman numeral, letters represent values:

* I = 1
* V = 5
* X = 10
* L = 50
* C = 100
* D = 500
* M = 1000

We can combine these letters to form numbers.
The letters should be ordered from the highest to the lowest value.
For example `CCXVI` would be 216.

When we put a lower value in front of a higher one, we substract that value from the higher value.
For example we make 40 not by XXXX, but instead we use $$50 - 10 = 40$$ and have the roman number `XL`.
Combining both these principles we could give our method `MDCCCXLII` and it should return 1842.
```

Implement this program. Practice TDD!

{% include answer-begin.html %}

How did it feel?

{% include exercise-answer-end.html %}




{% include exercise-begin.html %}
Which of the following **is the least important** reason why one does Test-Driven Development?

1. As a consequence of the practice of TDD, software systems get completely tested.
2. TDD practitioners use the feedback from the test code as a design hint.
3. The practice of TDD enables developers to have steady, incremental progress throughout the development of a feature.
4. The use of mocks objects helps developers to understand the relationships between objects.
{% include answer-begin.html %}
Option 1 is the least important one.

Although a few studies show that the number of tests written by TDD practitioners are often higher than the number of tests written by developers not practicing TDD, this is definitely not the main reason why developers have been using TDD. In fact, among the alternatives, it's the least important one. All other alternatives are more important reasons, according to the TDD literature (e.g., Kent Beck's book, Freeman's and Pryce's book.

{% include exercise-answer-end.html %}





{% include exercise-begin.html %}
TDD has become a really popular practice among developers. According to them, TDD has several benefits. Which of the following statements 
**is not** considered a benefit from the practice of TDD?

*Note:* We are looking at the perspective of developers, which may not always match the results of empirical research.

1. Better team integration. Writing tests is a social activity and makes the team more aware of their code quality. 

2. Baby steps. Developers can take smaller steps whenever they feel they have to.

3. Refactoring. The cycle suggests developers to constantly improve their code.

4. Design for testability. Developers are ``forced'' to write testable code from the very beginning.

{% include answer-begin.html %}
Option 1 is not a benefit of TDD. The
TDD literature says nothing about team integration.

{% include exercise-answer-end.html %}
