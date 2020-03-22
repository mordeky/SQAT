---
chapter-number: 7
title: Model-Based Testing
layout: chapter
toc: true
author: Arie van Deursen
---

In model based testing, we use models of the system to derive tests.
In this chapter we briefly show what a model is (or can be), and go over some of the models used in software testing.
The two models covered in this chapter are decision tables and state machines.

## Models

In software testing, a model is a simpler way of describing the program under test.
A model holds some of the attributes of the program that the model was made of.
Given that the model preserves some of the original attributes of the system under test, it can be used to analyse and test the system.

Why should we use models at all? A model gives us a structured way to understand
how the program operates (or should operate).

<iframe width="560" height="315" src="https://www.youtube.com/embed/5yuFf4-4JnE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Decision Tables

Decision tables are used to model how a combination of conditions should lead to a certain action.
These tables are easy to understand and can be validated by the client that the software is created for.
Developers can use the decision tables to derive tests that verify the correct implementation of the requirements with respect to the conditions.

### Creating decision tables

A decision table is a table containing the conditions and the actions performed by the system based on these conditions. In this section, we'll discuss how to build them in first place.

For now the table contains all the combinations of conditions explicitly.
Later we will look at ways to bring the amount of combinations present in the table down a bit.

In general a decision table looks like the following:
<table>
  <tr><th></th><th></th><th colspan="4">Variants</th></tr>
  <tr><td rowspan="2"><br><i>Conditions</i></td>
      <td>&lt;Condition1&gt;</td><td>T</td><td>T</td><td>F<br></td><td>F</td></tr>
  <tr><td>&lt;Condition2&gt;</td><td>T<br></td><td>F</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>&lt;Action&gt;</td><td>value1<br></td><td>value2</td><td>value3</td><td>value4</td></tr>
</table>

The chosen conditions should always be independent from one another.
In this type of decision tables, the order of the conditions also do not matter, 
e.g., making `<Condition2>` true and `<Condition1>` false or making `<Condition1>` false and after that `<Condition2>` true, should result on the same outcome.
(If the order does matter in some way, a state machine might be a better model.
We cover state machines later in this chapter.)

{% include example-begin.html %}
When choosing a phone subscription, there are a couple of options you could choose.
Depending on these options a price per month is given.
We consider the two options:

- International services
- Auto-renewal

In the decision table these will be turned into conditions.
True then corresponds to a chosen option and false corresponds to a option that is not chosen.
Taking international should increase the price per month.
Auto-renewal decreases the price per month.

The decision tables is the following:

<table>
  <tr><th></th><th></th><th colspan="4">Variants</th></tr>
  <tr><td rowspan="2"><br><i>Conditions</i></td>
      <td>International</td><td>F</td><td>F</td><td>T<br></td><td>T</td></tr>
  <tr><td>Auto-renewal</td><td>T<br></td><td>F</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>price/month</td><td>10<br></td><td>15</td><td>30</td><td>32</td></tr>
</table>

You can see the different prices for the combinations of international and auto-renewal.
{% include example-end.html %}

*Don't Care values:* In some cases the value of a condition might not influence the action.
This is represented as a don't care value, often abbreviated to "dc".

Essentially, "dc" is an combination of two columns.
These two columns have the same values for the other conditions and the same result.
Only the condition that had the dc value has different values in the expanded form.

<table>
  <tr><th></th><th></th><th colspan="3">Variants</th></tr>
  <tr><td rowspan="2"><br><i>Conditions</i></td>
      <td>&lt;Condition1&gt;</td><td>T</td><td>dc</td><td>F<br></td></tr>
  <tr><td>&lt;Condition2&gt;</td><td>dc</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>&lt;Action&gt;</td><td>value1<br></td><td>value1</td><td>value2</td></tr>
</table>
can be expanded to:
<table>
  <tr><th></th><th></th><th colspan="5">Variants</th></tr>
  <tr><td rowspan="2"><br><i>Conditions</i></td>
      <td>&lt;Condition1&gt;</td><td>T</td><td>T</td><td>T<br></td><td>F</td><td>F</td></tr>
  <tr><td>&lt;Condition2&gt;</td><td>T<br></td><td>F</td><td>T</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>&lt;Action&gt;</td><td>value1<br></td><td>value1</td><td>value1</td><td>value1</td><td>value2</td></tr>
</table>
After expanding we can remove the duplicate columns.
We end up with the decision table below:
<table>
  <tr><th></th><th></th><th colspan="4">Variants</th></tr>
  <tr><td rowspan="2"><br><i>Conditions</i></td>
      <td>&lt;Condition1&gt;</td><td>T</td><td>T</td><td>F<br></td><td>F</td></tr>
  <tr><td>&lt;Condition2&gt;</td><td>T<br></td><td>F</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>&lt;Action&gt;</td><td>value1<br></td><td>value1</td><td>value1</td><td>value2</td></tr>
</table>

{% include example-begin.html %}
We add another condition to the example above.
A loyal costumer receives the same discount as a costumer who chooses the auto-renewal option. 
However, a costumer only gets the discount from one of the two.
The new decision table is below:
<table>
  <tr><th></th><th></th><th colspan="6">Variants</th></tr>
  <tr><td rowspan="3"><br><i>Conditions</i></td>
      <td>International</td><td>F</td><td>F</td><td>F</td><td>T</td><td>T</td><td>T</td></tr>
  <tr><td>Auto-renewal</td><td>T</td><td>dc</td><td>F</td><td>T</td><td>dc</td><td>F</td></tr>
  <tr><td>Loyal</td><td>dc</td><td>T</td><td>F</td><td>dc</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>price/month</td><td>10</td><td>10</td><td>15</td><td>30</td><td>30</td><td>32</td></tr>
</table>
Note that when auto-renewal is true, the loyal condition does not change the outcome anymore and vice versa.
{% include example-end.html %}

*Default behavior*: Usually, $$N$$ conditions lead to $$2^N$$ combinations or columns.
Often, however, the number of columns that are specified in the decision table can be smaller.
Even if we expand all the dc values.

This is done by using a default action.
A default action means that if a combination of condition outcomes is not present in the decision table, the default action should be the result.

{% include example-begin.html %}
If we set the default charge rate to 10 per month the new decision table can be a bit smaller:
<table>
  <tr><th></th><th></th><th colspan="4">Variants</th></tr>
  <tr><td rowspan="3"><br><i>Conditions</i></td>
      <td>International</td><td>F</td><td>T</td><td>T</td><td>T</td></tr>
  <tr><td>Auto-renewal</td><td>F</td><td>T</td><td>dc</td><td>F</td></tr>
  <tr><td>Loyal</td><td>F</td><td>dc</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>price/month</td><td>15</td><td>30</td><td>30</td><td>32</td></tr>
</table>
{% include example-end.html %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/1u1qfJ2IrpU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Testing decision tables

Using the decision tables we can derive tests, such that we test whether the expected logic is implemented correctly.
There are multiple ways to derive tests for a decision table:

- **All explicit variants:** Derive one test case for each column. The amount of tests is the amount of columns in the decision table.
- **All possible variants:** Derive a test case for each possible combination of condition values. For $$N$$ conditions this leads to $$2^N$$ test cases. Often, this approach is unrealistic because of the exponential relation between the number of conditions and the number of test cases.
- **Every unique outcome / All decisions:** One test case for each unique outcome or action. The amount of tests depends on the actions in the decision table.
- **Each condition T/F:** Make sure that each conditions is true and false at least once in the test suite. This often results in two tests: All conditions true and all conditions false.

#### MC/DC

One more way to derive test cases from a decision table is by using Modified Condition / Decision Coverage (MC/DC). 
This is a combination of the last two ways of deriving tests shown above.

We have already discussed MC/DC in the Structural-Based Testing chapter.
MC/DC has the two characteristics of All devisions and Each condition T/F with an additional characteristic that makes MC/DC special:

1. Each condition is at least once true and once false in the test suite
2. Each unique action should be tested at least once
3. Each condition should individually determine the action or outcome

The third point is realized by making two test cases for each condition.
In these two test cases, the condition under test should have a different value, the outcome should be different, and the other conditions should have the same value in both test cases.
This way the condition that is under test individually influences the outcome, as the other conditions stay the same and therefore do not influence the outcome.

By choosing the test cases efficiently MC/DC needs less tests than all variants, while still exercising the important parts of the system.
Less tests of course means less time taken to write the tests and a faster execution of the test suite.

{% include example-begin.html %}
To derive the tests we expand and rearrange the decision table of the previous example:
<table>
  <tr><th></th><th></th><th>v1</th><th>v2</th><th>v3</th><th>v4</th><th>v5</th><th>v6</th><th>v7</th><th>v8</th></tr>
  <tr><td rowspan="3"><br><i>Conditions</i></td>
      <td>International</td><td>T</td><td>T</td><td>T</td><td>T</td><td>F</td><td>F</td><td>F</td><td>F</td></tr>
  <tr><td>Auto-renewal</td><td>T</td><td>T</td><td>F</td><td>F</td><td>T</td><td>T</td><td>F</td><td>F</td></tr>
  <tr><td>Loyal</td><td>T</td><td>F</td><td>T</td><td>F</td><td>T</td><td>F</td><td>T</td><td>F</td></tr>
  <tr><td><i>Action</i></td><td>price/month</td><td>30</td><td>30</td><td>30</td><td>32</td><td>10</td><td>10</td><td>10</td><td>15</td></tr>
</table>
First, we look at the first condition and we try to find pairs of combinations that would cover this condition according to MC/DC.
We look for combinations where only International and the price/month changes.

The possible pairs are: {v1, v5}, {v2, v6}, {v3, v7} and {v4, v8}.
Testing both combinations of any of these pairs would give MC/DC for the first condition.

Moving to Auto-renewal we find the pairs: {v2, v4}, {v6, v8}.
For this condition {v1, v3} and {v5, v7} are not viable pairs, because the action is the same among the two combinations.

The last condition, Loyal, gives the following pairs: {v3, v4}, {v7, v8}.

By choosing the test cases efficiently we should be able to achieve full MC/DC by choosing four of the combinations.
We want to cover all the actions in the test suite.
Therefore we need at least v4 and v8.
With these decisions we have covered the International condition as well.
Now we need one of v1, v2, v3 and one of v5, v6, v7.
To cover Loyal we add v7 and to cover Auto-renewal we add v2.
Now we also cover all the possible actions.

Now, for full MC/DC, we test the decisions: v2, v4, v7, v8.
{% include example-end.html %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/TxAFPJx6yKI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Implementing automated test cases for decision tables

Now that we know how to derive the test cases from the decision tables, it is time to implement them as automated test cases.

The most obvious way to test the combinations is to create a single test for each of the conditions.

{% include example-begin.html %}
We continue with the example we created the decision table for earlier.
To start we write the tests for combinations v2 and v3.
Assuming that we can use some implemented methods in a PhonePlan class the tests look like this:

```java
@Test
public void internationalAutoRenewalTest() {
  PhonePlan plan = new PhonePlan();

  plan.setInternational(true);
  plan.setAutoRenewal(true);
  plan.setLoyal(false);

  assertEquals(30, plan.pricePerMonth());
}

@Test
public void internationalLoyalTest() {
  PhonePlan plan = new PhonePlan();

  plan.setInternational(true);
  plan.setAutoRenewal(false);
  plan.setLoyal(true);

  assertEquals(30, plan.pricePerMonth());
}
```

{% include example-end.html %}
As you can see in the example above, the different tests for the different combinations are very similar.
The tests do the exact same thing, but just with different values.
To avoid the code duplication that comes with this approach to implementing decision table tests, we can use parameterized tests.

#### Parameterized Testing

Instead of implementing a single test for each of the combinations, we want to implement a single test for all the combinations.
Then for each combination we execute this test with the correct values.
This is done with a parameterized test.

In JUnit 5 this is done using the `ParameterizedTest` annotation.
Additionally, a `Source` is needed, which is given by another annotation.
In general this will look like the following:

```java
@ParameterizedTest
@CsvSource({
  "true, 5, 8.0, ...",  // test 1
  "false, 2, 0.6, ...", // test 2
  "true, 1, 5.3, ...",  // test 3
  "true, 3, 4.7, ..."   // test 4
})
public void someTest(boolean param1, int param2, double param3, ...) {
  // Arrange
  SomeObject some = new SomeObject();

  // Act
  double result = some.method(param1, param2);

  // Assert
  assertEquals(param3, result);
}
```

For testing the combinations out of decision tables, usually the `CsvSource` is most convienient. Other sources can be found in the [JUnit 5 docs](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests-sources). Each string in the `CsvSource` gives one test. Such a string consists of the arguments for the test function separated by commas. The first value is the first argument, the second value the second argument etc.

{% include example-begin.html %}
With the parameterized test we can easily create all tests we need for MC/DC of the decision table in the previous examples.

```java
@ParameterizedTest
@CsvSource({
  "true, true, false, 30",   // v2
  "true, false, true, 30",   // v3
  "true, false, false, 32",  // v4
  "false, false, false, 15"  // v8
})
public void pricePerMonthTest(boolean international, boolean autoRenewal,
    boolean loyal, int price) {
  
  PhonePlan plan = new PhonePlan();

  plan.setInternational(international);
  plan.setAutoRenewal(autoRenewal);
  plan.setLoyal(loyal);

  assertEquals(price, plan.pricePerMonth());
}
```

You can see that the test is very similar as the tests in the previous example.
Instead of directly using the values for one combination we use the parameters with the `CsvSource` to execute multiple tests.
{% include example-end.html %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/tzcjDhdQfvM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Non-binary choices and final guidelines

{% assign todo = "Write the video's accompanying text" %}
{% include todo.html %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/RHB_HaGfNjM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## State Machines

The state machine is a model that describes the software system by describing its states.
A system often has multiple states and various transitions between these states.
The state machine model uses these states and transitions to illustrate the system's behavior.

The main focus of a state machine is, as the name suggests, the states of a system.
So it is useful to think about what a state actually is.
The states in a state machine model describe where a program is in its execution.
If we need X to happen before we can do Y, we can use a state.
X would then cause the transition to this state.
From the state we can do Y, as we know that in this state X has already happened.
We can use as many states as we need to describe the system's behavior well.

Besides states and transitions, a state machine has an initial state and events.
The initial state is the state that the system starts in.
From that state the system can transition to other states.
Each transition is paired with an event.
This event is usually one or two words that describe what has to happen to make the transition.

Of course there are some agreements on how to make the models.
The notation we use is the Unified Modeling Language, UML.
For the state diagrams that means we use the following symbols:

- State: ![](/assets/img/model-based-testing/uml/state_symbol.svg)
- Transition: ![](/assets/img/model-based-testing/uml/transition_symbol.svg)
- Event: ![](/assets/img/model-based-testing/uml/event_symbol.svg)
- Initial state: ![](/assets/img/model-based-testing/uml/initial_state_symbol.svg)

{% include example-begin.html %}
For the coming examples we model a (part of a) phone.
We start very simple with a state machines that models the phone's ability to be locked or unlocked.

A phone that can be either locked or unlocked has two states: locked and unlocked.
Before all the face recognition and fingerprint sensors, you had to enter a password to unlock the phone.
A correct password unlocks the phone and if an incorrect password is given the phone stays locked.
Finally, an unlocked phone can be locked again by pushing the lock button.
We can use these events in the state machine.

![](/assets/img/model-based-testing/examples/locked_unlocked_machine.svg)

In the diagram the initial state is `LOCKED`.
Usually when someone starts using their phone, it is locked.
Therefore the initial state of the state machine should also be `LOCKED`.
{% include example-end.html %}

Sometimes an event can lead to multiple states, depending on a certain condition.
To model this in the state machines, we use conditional transitions.
These transitions are only performed if the event happens and if the condition is true.
The conditions often depend on a certain value used in the state machine.
To modify these values when a transition is taken in the state machine we use actions.
Actions are associated with a transition and are performed when the system uses that transition to go into another state.
The notation for conditions and actions is as follows:

- Conditional transition: ![](/assets/img/model-based-testing/uml/conditional_symbol.svg)
- Action: ![](/assets/img/model-based-testing/uml/action_symbol.svg)

{% include example-begin.html %}
When a user types the wrong password for four times in a row, the phone gets blocked.
We use `n` in the model to represent the amount of failed attempts.
Let's look at the conditional transitions that we need to model this behavior first.

![](/assets/img/model-based-testing/examples/blocked_condition_machine.svg)

When `n` (the number of failed unlock attempts) is smaller than 3, the phone stays in `LOCKED` state.
However, when `n` is equal to 3, the phone goes to `BLOCKED`.
Here we have an event, wrong password, than can lead to different states based on the condition.

In the previous state machine, `n` never changes.
This means that the phone will never go in its `BLOCKED` state, as that requires `n` to be equal to 3.
We can add actions to the state machine to make `n` change correctly.

![](/assets/img/model-based-testing/examples/blocked_complete_machine.svg)

The added actions are setting `n` to `n+1` when an incorrect password is given and to 0 when a correct password is given.
This way the state machine will be in the `BLOCKED` state when a wrong password is given for four times in a row.
{% include example-end.html %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/h4u9k-P3W0U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/O1_oC-7I5E4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Testing state-machines

Like with the decision tables, we want to use the state machine model to derive tests for our software system.
First, we will have a look at what might be implemented incorrectly.

An obvious error that can be made is a transition going to the wrong state.
This will cause the system to act incorrectly so we want the tests to catch such errors.
Additionally, the conditions in conditional transitions and the actions in transition can be wrong.
Finally, the behavior of a state should stay the same at all times.
This means that moving from and to a state should not change the behavior of that state.

For state machines we have a couple of test coverages.
In this chapter we go over three mainly used ways of defining test coverage:

- **State coverage:** each state has to be reached at least once
- **Transition coverage:** each transition has to be exercised at least once
- **Paths:** not exactly a way of describing test coverage, but we use paths to derive test cases

To achieve the state coverage we generally bring the system in a state through transitions and then assert that the system is in that state.
To test a single transition (for transition coverage) a bit more steps are needed:

1. Bring system in state that the transition goes out of
2. Assert that the system is indeed in that state
3. Trigger the transition's event.
4. If there is an action: check if this action has happened
5. Assert that the system now is in the new state that the transition points to

{% include example-begin.html %}
To achieve full state coverage we need to arrive in each state once.
For the phone example we have three states so we can make three tests.

- Check that the system is `LOCKED` when it is started
- Give the correct password and check that the system is `UNLOCKED`
- Give an incorrect password four times and check that the system is `BLOCKED`

With these three tests we achieve full state coverage, as the system is in each state at some point.

With the tests above, we have covered most of the transitions as well.
The only untested transition is the `lock button` from `UNLOCKED` to `LOCKED`.
To test this transition, we bring the system in `UNLOCKED` by giving the correct password.
Then we trigger the `lock button` and assert that the system is in `LOCKED`.
{% include example-end.html %}


#### Paths and Transition trees

Besides the individual transitions, we can also test the combinations of transitions.
These combinations of transitions are called paths.

A logical thought might be: Let's test all the paths in the state machine!
While this looks like a good objective, the number of paths will most likely be too high.
Take a state machine that has a loop, i.e., a transition from state X to Y and a transition from state Y to X.
When creating paths we can keep going back and forth these two states.
This leads to an infinite amount of paths.
Obviously, we cannot test all the paths. We will need to take a different approach.

The idea is that when using paths to derive test cases, we want each loop to be executed once.
This way we have a finite amount of paths to create test cases for.
We derive these tests by using a transition tree, which spans the graph of the state machine.
Such a transition tree is created as follows:

1. The root node is named as the initial state of the state machine
2. For each of the nodes at the lowest level of the transition tree:
  - If the state that the node corresponds to has not been covered before: \\
    For each of the outgoing transitions of this node's state: \\
      Add a child node that has the name of the state the transition points to. If this state is already in the tree, add or increment a number after the state's name to keep the node unique
  - If any nodes were added: \\
     Repeat from step 2.

This is also demonstrated in the example below.

{% include example-begin.html %}
To make the transition table a bit more interesting we modify the phone's state machine to have an `OFF` state instead of a `BLOCKED` state.
See the state machine below:

![Phone state machine with off](/assets/img/model-based-testing/examples/phone_off_machine.svg)

The root node of the transition tree is the initial state of the state machine.
We append a number to make it easier to distinguish this node from other nodes of the same state.

![Root node transition tree](/assets/img/model-based-testing/examples/transition_tree/transition_tree_0.svg)

Now we for each outgoing transition from the `OFF` state we add a child node to `OFF_0`.

![Two level transition tree](/assets/img/model-based-testing/examples/transition_tree/transition_tree_1.svg)

One node was added, so we continue by adding children to that node.
![Three level transition tree](/assets/img/model-based-testing/examples/transition_tree/transition_tree_2.svg)

Now, the only state we have not seen yet is `UNLOCKED` in the `UNLOCKED_0` node.
Therefore this is the only node we should add children to.
![Final phone transition tree](/assets/img/model-based-testing/examples/transition_tree/transition_tree_3.svg)

Now all the states of the nodes in the lowest layer have been visited before so the transition tree is done.
{% include example-end.html %}

From a transition tree, we can derive tests.
Each leaf node in the transition tree represents one path to test.
This path is given by going from the root node to this leaf node.
In the tests, we typically assert that we start in the correct state.
Then, we trigger the next event that is needed for the given path and assert that we are in the next correct state.
These events that we need to trigger can be found in the state machine.
Then this is done until the whole path is followed.

{% include example-begin.html %}
In the transition tree of the previous example there are four leaf nodes: `OFF_1`, `OFF_2`, `LOCKED_1`, `LOCKED_2`.
We want a test for each of these leaf nodes, that follows the path leading to that node.
For `OFF_1` the test should 'move' the system from `OFF` to `LOCKED` and again to `OFF`.
Looking at the state machine this gives the events `home`, `long lock button`.
In the test we would assert that the system is in `OFF`, then trigger `home`, assert that the system is in `LOCKED`, trigger `long lock button` and finally assert that the system is in `OFF`.

The tests for the other three paths can be derived in similar fashion.
{% include example-end.html %}

Using the transition tree, each loop that is in the state machine is executed once.
Now the amount of tests are manageable, while testing most of the important paths in the state machine.

<iframe width="560" height="315" src="https://www.youtube.com/embed/pvFPzvp5Dk0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Sneak paths and Transition tables

In the previous section, we discussed transition trees and how to use them to derive tests.
These tests check if the system behaves correctly when following different paths in the state machine.
With this way of testing, we check if the existing transitions in a state machine behave correctly.
We do not check if there exists any more transitions, transitions that should not be there.
We call these paths, "sneak paths".

A **sneak path** is a path in the state machine that should not exist.
So, for example, we have state X and Y and the system should not be able to transition directly from X to Y.
If the system can in some way transition directly from X to Y, we have a sneak path.
Of course, we want to test if such sneak paths exist in the system.
To this end we make use of transition tables.

A transition table is a table containing each transition that is in the state machine.
The transition is given by the state it's going out of, the event that triggers the transition, and the state the transition goes to.
A transition table typically looks somewhat like the following:
<table>
  <tr>
    <td>STATES</td>
    <td colspan="3">Events</td>
  </tr>
  <tr>
    <td></td>
    <td>event1</td>
    <td>event2</td>
    <td>event3</td>
  </tr>
  <tr>
    <td>STATE1</td>
    <td>STATE1</td>
    <td></td>
    <td>STATE2</td>
  </tr>
  <tr>
    <td>STATE2</td>
    <td></td>
    <td>STATE1</td>
    <td></td>
  </tr>
</table>

We construct a transition table as follows:

- List all the state machine's states along the rows
- List all events along the columns
- For each transition in the state machine note its destination state in the correct cell of the transition table.

{% include example-begin.html %}
We take a look at the same state machine we created a transition table for:
![](/assets/img/model-based-testing/examples/phone_off_machine.svg)

To make the transition table we list all the states and events in the table:

<table>
  <tr>
    <td>STATES</td>
    <td colspan="5">Events</td>
  </tr>
  <tr>
    <td></td>
    <td>home</td>
    <td>wrong password</td>
    <td>correct password</td>
    <td>lock button</td>
    <td>long lock button</td>
  </tr>
  <tr>
    <td>OFF</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>LOCKED</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>UNLOCKED</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
Then we fill the table with the states that the transitions in the state machine point to:
<table>
  <tr>
    <td>STATES</td>
    <td colspan="5">Events</td>
  </tr>
  <tr>
    <td></td>
    <td>home</td>
    <td>wrong password</td>
    <td>correct password</td>
    <td>lock button</td>
    <td>long lock button</td>
  </tr>
  <tr>
    <td>OFF</td>
    <td>LOCKED</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>LOCKED</td>
    <td></td>
    <td>LOCKED</td>
    <td>UNLOCKED</td>
    <td></td>
    <td>OFF</td>
  </tr>
  <tr>
    <td>UNLOCKED</td>
    <td></td>
    <td></td>
    <td></td>
    <td>LOCKED</td>
    <td>OFF</td>
  </tr>
</table>

We can see that there is, for example, a transition from `UNLOCKED` to `LOCKED` when the event `lock button` is triggered.
{% include example-end.html %}

Now that we have the transition table, we have to decide the intended behavior for the cells that are empty.
The default is to just ignore the event and stay in the same state.
In some cases one might want the system to throw an exception.
These decisions depend on the project and the customer's needs.

As discussed earlier, we can use the transition table to derive tests for sneak paths.
Usually, we want the system to remain in its current state when we trigger an event that has an empty cell in the transition table.
To test for all possible sneak paths, we create a test case for each empty cell in the transition table.
This test will first bring the system to the state corresponding to the empty cell's row (you can use the transition table to find a suitable path), then triggers the event that corresponds to the empty cell's column, and finally the test asserts that the system is in the same state as before triggering the event.
The amount of 'sneak path tests' is the amount of empty cells in the transition table.

With these tests we can verify both existing and non-existing paths.
These techniques combined give a good testing suite from a state machine.
So far, we looked at rather simple and small state machines.

<iframe width="560" height="315" src="https://www.youtube.com/embed/EMZB2IZT8WA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Super states and regions

When the modeled system becomes large and complex, typically so does the state machine.
At some point the state machine will consist of a lot of states and transitions, which makes it unclear and impractical to work with.
To resolve this issue and make a state machine more scalable we can use super states and regions.

#### Super states

A super state is a state that consists of a state machine.
Basically, we wrap a state machine in a super-state which we can then use as a state in another state machine.

The notation of the super-state is as follows: 

![Super state notation](/assets/img/model-based-testing/uml/super_state_symbol.svg)

Because the super state is, in essence, a state machine that can be used as a state, we know what should be inside of a super state.
The super state generally consists of multiple states and transitions, and it always has to have an initial state.
Any transition going into the super state essentially goes to the initial state of the super state.
A transition going out of the super state means that if the event on this transition is triggered in any of the super state's states, the system transitions into the state this transition points to.

With the super state we can choose to show it fully or we can collapse it.
A collapsed super state is just a normal state in the state machine.
This state has the super state's name and the same incoming and outgoing transitions as the super state.

With the super states and the collapsing of super states we can modularize and combine state machines.
This allows us to shift the state machine's focus to different parts of the system's behavior.

{% include example-begin.html %}
We can use a super state even in the small example of a phone's state machine.
The two states `LOCKED` and `UNLOCKED` both represent the system in some sort of `ON` state.
We can use this to create a super state called `ON`.

![Super state example](/assets/img/model-based-testing/examples/phone_super_state.svg)

Now we can also simplify the state machine by collapsing the super state:

![Collapsed super state example](/assets/img/model-based-testing/examples/phone_collapsed.svg)
{% include example-end.html %}

#### Regions

So far we have had super states that contain one state machine.
Here, the system is in only one state of the super state at once.
In some cases it may be useful to allow the system to be in multiple states at once.
This is done with regions.

A super state can be split up into multiple regions.
These are orthogonal regions, meaning that the state machines in the regions are independent from each other; they do not influence the state machines in other regions.
Each region contains one state machine.
When the systems enters the super state, it enters all the initial states of the regions.
This means that the system is in multiple states at once.

The notation of regions is: ![Region notation](/assets/img/model-based-testing/uml/region_symbol.svg)

Expanding regions is possible, but highly impractical and usually not wanted, because expanding the region requires creating a state for each combination of states in the different regions.
This causes the number of states and transitions to quickly explode.
We will not cover how to expand the regions because of this reason.

In general it is best to use small state machine and link these together using super states and regions.

{% include example-begin.html %}
So far when the phone was `ON` we modeled the `LOCKED` and `UNLOCKED` state.
When the phone is on, it drains the battery.
The system keeps track of the level of the battery.
Let's assume that our phone has two battery levels: low battery and normal battery.
The draining of the battery and the transitions between the states of this battery runs in parallel to the phone being locked or unlocked.
With parallel behavior like this, we can use the regions in our state machine model.
the state machine looks like the following, with the new battery states and the regions:

![Region state machine example](/assets/img/model-based-testing/examples/phone_region.svg)

You can see that we assumed the battery to start in the normal level state.
Therefore, when the system transitions to the `ON` state it will be in both `LOCKED` and `NORMAL BATTERY` states at once.
{% include example-end.html %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/D0IQxdjI0M0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Implementing state-based testing in practice

You know a lot about state machines as a model by now.
One thing we have not looked at yet is how these state machines are represented in the actual code.

States are very common in programming.
Most classes in Object-Oriented-Programming each correspond to their own small state machine.

In these classes, we distinguish two types of methods: **inspection** and **trigger** methods.
An **inspection** method only provides information about an object's state.
This information consists of the values or fields of an object.
The inspection methods only provide information. They do not change the state (or values) of an object.
**Trigger** methods bring the class into a new state.
This can be done by changing some of the class' values.
These trigger methods correspond to the events on the transitions in the state machine.

In a test, we want to bring the class to different states and assert that after each transition, the class is in the expected state.
In other words, a **test scenario** is basically a series of calls on the class's trigger methods.
Between these calls, we can call the inspection methods to check the state.

#### Abstraction layer

When a state machine corresponds to a single class, we can easily use the methods described above to test the state machine.
However, sometimes the state machine spans over multiple classes.
In that case, you might not be able to easily identify the inspection and trigger methods.
In this scenario, the state machines can even correspond to *end-to-end testing*.
Here, the flow of the entire system is under test, from input to output.

The system under test does not always provide a nice programing interface (API) to inspect the state or trigger the event for the transitions.
A common example of such a system is a web application.
In the end, the web application works through a browser.
To access the state or to trigger the transitions you would then need to use a dedicated tool, like [webdriver](https://webdriver.io/).
Using such a dedicated tool directly is not ideal, as you would have to specify each individual click on the web page to trigger events.
What we actually need is an abstraction layer on top of the system under test.
This abstraction can just be a Java class, with the methods that we need to be able to test the system as its state machine.
Hence, the abstraction layer will contain the inspection method to check the state and the trigger methods to perform the transitions.

With this small abstraction layer, we can formulate the tests very clearly.
Triggering a transition is just one method call and checking the state also requires only one method call.

#### Page Objects

Creating these abstraction layers is very common when testing web applications.
In this context, the abstractions are called **Page Objects**.

{% include example-begin.html %}
An example of a page object is shown in the diagram, made by Martin Fowler, below:

![Page Objects diagram by Martin Fowler](/assets/img/model-based-testing/page_objects.png)

At the bottom, you can see a certain web page that we want to test.
The tool for communicating through the browser (webdriver for example), gives an API to access the HTML elements.
Additionally, the tool supports clicking on elements. For example, on a certain button.

If we use this API directly in the tests, the tests become unreadable very quickly.
So, we create a page object with just the methods that we need in the tests.
These methods correspond to the application, rather than the HTML elements.
The page objects implement these methods by using the API provided by the tool.

Then, the tests use these methods instead of the ones about the HTML elements.
Because we are using methods that correspond to the application itself, they will be more readable than tests without the page objects.
{% include example-end.html %}

#### State Objects

Page objects give us an abstraction for single pages or even fragments of pages.
This is already better than using the API for the HTML elements in the test, but we can take it a bit further.
We can make the page objects correspond to the states in the navigational state machine.
A navigational state machine is a state machine that describes the flow through a web application.
Each page will be a represented as a state.
The events of the transitions between these states show how the user can go from one to another page.

With this approach, the page objects each correspond to one of the states of the state machine.
Now we do not call them page objects anymore, but **state objects**.
In these state objects we have the inspection and trigger methods.
Additionally, we have methods that can help with state **self-checking**.
These methods verify whether the state itself is working correctly, for example by checking if certain buttons can be clicked on the web page.
Now the tests can be expressed in the application's context, using these three types of methods.

#### Behavior-Driven Design

The state objects are mostly used for end-to-end testing in web development.
Another technique useful in end-to-end testing is behavior driven design.

In **behavior driven design** the system is designed with scenario's in mind.
These scenario's are written in natural language and describe the system's behavior in a certain situation.

For these scenarios to be used by tools, an example of a tool for scenario's is [cucumber](https://cucumber.io), we need to follow a certain format.
This is a standard format for scenario's, as it provides a very clear structure.
A scenario consists of the following:

- Title of the scenario
- Given ...: Certain conditions that need to hold at the start of the scenario.
- When ...: The action taken.
- Then ...: The result at the end of the scenario.

{% include example-begin.html %}
Let's look at a scenario for an ATM.
If we have a balance of $100, a valid card and enough money in the machine, we can give a certain amount of money requested by the user.
Together with the money, the card should be given back, and the balance of the account should be decreased.
This can be turned into the scenario 1 below:

```text
Story: Account Holder withdraws cash

As an Account Holder
I want to withdraw cash from an ATM
So that I can get money when the bank is closed

Scenario 1: Account has sufficient funds
Given the account balance is $100
 And the card is valid
 And the machine contains enough money
When the Account Holder requests $20
Then the ATM should dispense $20
 And the account balance should be $80
 And the card should be returned
```

The small introduction above the scenario itself is part of the user story.
A user story usually consists of multiple scenario's with respect to the user introduced.
This user is the account holder in this example.

{% include example-end.html %}

With the general `Given`, `When`, `Then` structure we can describe a state transition as a scenario.
In general the scenario for a state transition looks like this:

```text
Given I have arrived in some state
When  I trigger a particular event
Then  the application conducts an action
 And  the application moves to some other state.
```

Each scenario will be able to cover only one transition.
To get an overview of the system as a whole we will still have to draw the entire state machine.

<iframe width="560" height="315" src="https://www.youtube.com/embed/NMGX7TEMXdE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/gijO3mlcMCg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Other examples of real-world models

Slack shared their internal flow chart that decides whether
to send a notification of a message. Impressive, isn't it?

![How Slack decides to send notifications to users](/assets/img/model-based-testing/examples/slack.jpg)


## References

* Chapter 4 of the Foundations of software testing: ISTQB certification. Graham, Dorothy, Erik Van Veenendaal, and Isabel Evans, Cengage Learning EMEA, 2008.

* van Deursen, A. (2015). Beyond Page Objects: Testing Web Applications with State Objects. ACM Queue, 13(6), 20.

## Exercises

Here you find some exercises to practise the material of this chapter with.
For each of the exercises the answers are provided directly beneath the question.

{% include exercise-begin.html %}
The *ColdHot* air conditioning system has the following requirements:

- When the user turns it on, the machine is in an *idle* state.
- If it's *too hot*, then, the *cooling* process starts. It goes back to *idle* when the defined *temperature is reached*.
- If it's *too cold*, then, the *heating* process starts. It goes back to *idle* when the defined *temperature is reached*.
- If the user *turns it off*, the machine is *off*. If the user *turns it on* again, the machine is back to *idle*.

Draw a minimal state machine to represent these requirements.
{% include answer-begin.html %}

![](/assets/img/model-based-testing/exercises/coldhot_state_machine.svg)

You should not need more than 4 states.
{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
Derive the transition tree from the state machine of the assignment above.
{% include answer-begin.html %}

![](/assets/img/model-based-testing/exercises/coldhot_transition_tree.svg)

{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
Now derive the transition table of the *ColdHot* state machine.

How many sneaky paths can we test based on the transition table?
{% include answer-begin.html %}
<table>
  <tr>
    <td></td>
    <td>temperature reached</td>
    <td>too hot</td>
    <td>too cold</td>
    <td>turn on</td>
    <td>turn off</td>
  </tr>
  <tr>
    <td>Idle</td>
    <td></td>
    <td>Cooling</td>
    <td>Heating</td>
    <td></td>
    <td>Off</td>
  </tr>
  <tr>
    <td>Cooling</td>
    <td>Idle</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Heating</td>
    <td>Idle</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Off</td>
    <td></td>
    <td></td>
    <td></td>
    <td>Idle</td>
    <td></td>
  </tr>
</table>

There are 14 empty cells in the table, so there are 14 sneaky paths that we can test.
{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
Draw the transition tree of the following state machine:

![](/assets/img/model-based-testing/exercises/order_state_machine.svg)

Use sensible naming for the states in your transition tree.
{% include answer-begin.html %}
![](/assets/img/model-based-testing/exercises/order_transition_tree.svg)
{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
With the transition tree you devised in the previous exercise and the state machine in that exercise.
What is the transition coverage of a test that the following events: [order placed, order received, order fullfiled, order delivered]?
{% include answer-begin.html %}
We have a total of 6 transitions.
Of these transitions the four given in the test are covered and order cancelled and order resumed are not.
This coves a transition coverage of $$\frac{4}{6} \cdot 100\% = 66.7\%$$
{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
Devise the decision table of the state machine that was given in the exercise above.
Ignore the initial transition `Order placed`.
{% include answer-begin.html %}
<table>
  <tr>
    <td>STATES</td>
    <td colspan="5">Events</td>
  </tr>
  <tr>
    <td></td>
    <td>Order received</td>
    <td>Order cancelled</td>
    <td>Order resumed</td>
    <td>Order fulfilled</td>
    <td>Order delivered</td>
  </tr>
  <tr>
    <td>Submitted</td>
    <td>Processing</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Processing</td>
    <td></td>
    <td>Cancelled</td>
    <td></td>
    <td>Shipped</td>
    <td></td>
  </tr>
  <tr>
    <td>Cancelled</td>
    <td></td>
    <td></td>
    <td>Processing</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Shipped</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>Completed</td>
  </tr>
  <tr>
    <td>Completed</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
How many sneak paths are there in the state machine we used in the previous exercises?
Again ignoring the initial `Order placed` transition.
{% include answer-begin.html %}
20.

There are 20 empty cells in the decision table.

Also we have 5 states.
This means $$5 \cdot 5 = 25$$ possible transitions.
The state machine gives 5 explicit transitions so we have $$25 - 5 = 20$$ sneak paths.
{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
Consider the following decision table:
<table>
  <tr><th>Criteria</th><th colspan="6">Options</th></tr>
  <tr><td>C1: Employed for 1 year</td><td>T</td><td>F</td><td>F</td><td>T</td><td>T</td><td>T</td></tr>
  <tr><td>C2: Achieved last year's goal</td><td>T</td><td>dc</td><td>dc</td><td>F</td><td>T</td><td>F</td></tr>
  <tr><td>C3: Positive evaluation from peers</td><td>T</td><td>F</td><td>T</td><td>F</td><td>F</td><td>T</td></tr>
  <tr><td></td><td>10%</td><td>0%</td><td>5%</td><td>2%</td><td>6%</td><td>3%</td></tr>
</table>

Which decision do we have to test for full MC/DC?

Use as few decisions as possible.
{% include answer-begin.html %}

First we find pairs of decisions that are suitable for MC/DC: (We indicate a decision as a sequence of T and F. TTT would mean all conditions true and TFF means C1 true and C2, C3 false)

- C1: {TTT, FTT}, {FTF, TTF}, {FFF, TFF}, {FFT, TFT}
- C2: {TTT, TFT}, {TFF, TTF}
- C3: {TTT, TTF}, {FFF, FFT}, {FTF, FTT}, {TFF, TFT},

All condition can use the TTT decision, so we will use that.
Then we can add FTT, TFT and TTF.
Now we test each condition individually with it changing the outcome.

It might look line we are done, but MC/DC requires each action to be covered at least once.
To achieve this we add the FFF and TFF decision as test cases.

In this case we need to test each explicit decision in the decision table.

{% include exercise-answer-end.html %}

{% include exercise-begin.html %}
See the following generic state machine.

![](/assets/img/model-based-testing/exercises/generic_state_machine.svg)

Draw the transition tree of this state machine.
{% include answer-begin.html %}

![](/assets/img/model-based-testing/exercises/generic_transition_tree.svg)

{% include exercise-answer-end.html %}







{% include exercise-begin.html %}



The advertisement (ad) feature is an important source of income for the company. Because of that, the life cycle of an ad needs to be better modelled. 
Our product team defined the following rules:

* The life cycle of an ad starts with an 'empty' ad being created.
* The company provides information about the ad. More specifically, the company defines an image, a description, and how many times it should appear. When all these information is set, the ad then needs to wait for approval.
* An administrator checks the content of the ad. If it follows all the rules, the ad then waits for payment. If the ad contains anything illegal, it then goes back to the very beginning.
* As soon as the company makes the payment, the ad becomes available to users.
* When the number of visualizations is reached, the ad is then considered done. At this moment, the company might consider running the campaign again, which moves the ad to wait for payment again. The company might also decide to simply end the campaign at that moment, which puts the ad in a finalized state.  
* While appearing for the users, if more than 10\% of the users complain about the ad, the ad is then marked as blocked. Cute Babies then gets in contact with the company. After understanding the case, the ad either starts to appear again for the users, or gets marked as innapropriate. An innapropriate ad will never be shown again to the users.

Devise a state diagram that describes the life cycle of an ad.


{% include answer-begin.html %}

![](/assets/img/model-based-testing/exercises/ads.png)

{% include exercise-answer-end.html %}












{% include exercise-begin.html %}

A microwave oven has the following requirements:

* Its initial state is `OFF`.
* When the user `turns it on`, the machine goes to an `ON` state.
* If the user selects `warms meal`, then, the `WARMING` process starts. It goes back to `ON` when the defined `time is reached`. A user may `cancel` it at any time, taking the microwave back to the `ON` state.
* If the user selects `defrost meal`, then, the `DEFROSTING` process starts. It goes back to `ON` when the defined `time is reached`. A user may `cancel` it at any time, taking the microwave back to the `ON` state.
* The user can `turn off` the microwave (after which it is `OFF`), but only if the microwave is not warming up or defrosting food.

Draw a minimal state machine to represent the requirements. For this question do not make use of super (OR) states.
Also, remember that, if a transition is not specified in the requirements, it simply does not exist, and thus, should not be represented in the state machine.


{% include answer-begin.html %}

![](/assets/img/model-based-testing/exercises/solution-microwave-statemachine.png)

{% include exercise-answer-end.html %}




{% include exercise-begin.html %}

Devise a state transition tree for the microwave state machine.

{% include answer-begin.html %}


![](/assets/img/model-based-testing/exercises/solution-microwave-transitiontree.png)

{% include exercise-answer-end.html %}

{% include exercise-begin.html %}

Again consider the state machine requirements for the microwave.
There appears to be some redundancy in the defrosting and warming up functionality, which potentially can be described using super states (also called OR-states).
Which effect does this have on the total number of states and transitions for the resulting diagram with a super state?

1. There will be one extra state, and two less transitions.
2. There will be one state less, and the same number of transitions.
3. The total number of states will remain the same, and there will be two less transitions.
4. This has no effect on the total number of states and transitions.


{% include answer-begin.html %}

There will be one extra super state (ACTIVE), which will be a superstate of the existing WARMING and DEFROSTING states. The edges from ON to WARMING and DEFROSTING will remain.
The two (cancel and time out) outgoing edges from WARMING and DEFROSTING (four edges in total) will be replaced by two edges going out of the super ACTIVE state.
So there will be two fewer transitions.

{% include exercise-answer-end.html %}








{% include exercise-begin.html %}

See the requirement below:

```
Stefan works for Foodgram, a piece of software that enables users to send pictures of the dishes they prepare themselves. Foodgram's upload system has specific rules:

* The software should only accept images in JPG format.
* The software should not accept images that are bigger than 20MB.
* The software accepts images in both high and low resolution.

As soon as a user uploads a photo, the aforementioned rules are applied. 
The software then either says *"Congratulations! Your picture was uploaded successfully"*, or *"Ooops, something went wrong!"*" (without any specific details about why it happened).
```

Create a decision table that takes the three conditions and their respective outcomes into account. 

*Note: conditions should be modeled as boolean decisions.*


{% include answer-begin.html %}


||C1|C2|C3|C4|C5|C6|C7|C8|
|Valid format?|T|T|T|T|F|F|F|F|
|Valid size?|T|T|F|F|T|T|F|F|
|High resolution?|T|F|T|F|T|F|T|F|
|-|-|-|-|-|-|-|-|-|
|Outcome|success|success|fail|fail|fail|fail|fail|fail|



{% include exercise-answer-end.html %}







{% include exercise-begin.html %}


Twitter is a software system that enables users to share short messages within their friends. 
Twitter's revenue model is ultimately based on advertisements ("ads").
Twitter's system needs to decide when to serve ads to its users, and which ones. For a given user a given ad can be *highly-relevant*, and the system seeks to serve the most relevant ads as often as possible without scaring users away.

To that end, assume that the system employs the following rules to decide whether a user *U* gets served an ad *A* at the moment user *U* opens their Twitter app:

* If the user *U* has not been active during the past two weeks, she will not get to see add *A*;
* If the user *U* has already been served an ad during her last hour of activity, she will not get to see ad *A*;
* Furthermore, if the user *U* has over 1000 followers (an influencer), she will only get to see ad *A* if *A* is labeled as *highly-relevant* for *U*. Otherwise, user *U* will see *A* even if it is not *highly-relevant*.

We can model this procedure in a decision table, in various ways.
The complete table would have four conditions and 16 variants. We will try to create a more compact decision table. Note: We will use Boolean conditions only.

One way is to focus on the positive cases only, i.e., specify only the variants in which ad *A* is being served to user *U*. If you don't use 'DC' (don't care) values, how will the decision table look like?

{% include answer-begin.html %}

| | T1 | T2 | T3 |
|User active in past two weeks      | T | T | T| 
|User has seen ad in last two hours | F | F | F| 
|User has over 1000 followers       | T | F | F| 
|Ad is highly relevant to user      | T | T | F| 
|-|-|-|-|
|Serve ad?                          | T | T | T| 

{% include exercise-answer-end.html %}

