# Debug using the Scientific Method (see Code Complete 2nd Edition)

1. Stabalize the error
1. Locate the source of the error
    - Gather the data that produces the defect
    - Analyze the data and form a hypothesis.
    - Determine how to prove or disprove the hypothesis
    - Prove or disprove the hypothesis
1. Fix the defect
1. Look for similar errors

## Understand the error (problem)

- Speak to those with context
- Look at the change history
- Ensure you can describe the problem yourself
- Understand the expected results

## Stabalize the error

If a defect doesn't occur reliably, it's almost impossible to diagnose. Making an intermittent defect occur predictably is on of the most challenging tasks in debugging.

An error that doesn't occur predictably is usually a
- initialization error - variable involved in the calc isn't being initialized properly.
- timing issue
- dangling pointer problem

#### Building a test case to diagnose the problem:
- Find the simplest example that produces the error.
- Change the inputs and and watch the program's behavior under these controlled conditions.

#### Simplify the test case
Suppose you have 10 factors that, used in combination, produce the error.
- Form a hypothesis about which factors were irrelevant to producing the error.
    - Change the supposedly irrelevant factors, and rerun the test case. 
    - If you still get the error, you can **eliminate those factors** and you’ve simplified the test. 
- Try to simplify the test further. 
    - If you don’t get the error, you’ve disproved that specific hypothesis and you know more than you did before.
    - It might be that some subtly different change would still produce the error, but you know at least one specific change that does not.

## Locate the source of the error

Once the error is stabalized and the test case that produces it is refined, finding the source can either be trivial or challenging.

#### Brainstorm hypotheses

Rather than limiting yourself to the first hypothesis you think of, try to come up with several. Don’t analyze them at first—just come up with as many as you can in a few minutes. Then look at each hypothesis and think about test cases that would prove or disprove it. This mental exercise is helpful in breaking the debugging logjam that results from concentrating too hard on a single line of reasoning.

Keep a notepad of a list of things to try... if one approach isn't working, try another.

#### Use all available data
- Use **all the data available** to make your hypothesis. Are you only looking at the partial picture? If the data doesn't fit the hypothesis don't discard it. Ask **why it doesn't fit** and create a new hypothesis.
- Refining the test cases further (focus on inputs and parameters) could lead to a breakthrough.

#### Test code in isolation
- Exercise the code and data in your unit test suit or find some other way to **test the code in isolation**.
    - Pull dummy data from test case files (to overwrite parameter values) if the code is difficult to get to via a test suite.
    - Pull the code out into your own little test harness.
- Use the **correct available debugging tools**. Take some time to think about how to
    - add conditional breakpoints
    - isolate a specific thread
    - ...

#### Reproduce the error in several different ways

- **Reproduce the error in several different ways**. Sometimes trying cases that are similar to the error-producing case but not exactly the same is instructive. If you are not getting the expected results it means you do not understand the problem yet.
    - Consider what other condition would cause the same error and test this condition.
    - Consider what other similar condition would **not** produce the same error and test this condition.

> Errors often arise from combinations of factors, and trying to diagnose the problem
with only one test case often doesn’t diagnose the root problem.

- Use the results of negative tests - if a test case disproves your hypothesis you know something you didn't know before, namely, that the defect is not in the area you thought it was. That narrows your search field and the set of possible remaining hypotheses.

#### Narrow the suspicious region of the code

- Use print statements, logging, and tracing to identify the code producing the error.
- Systematically remove parts of the code and see where the error still occurs. If it does, its in the part you've kept.
    - Divide and conquer... remove half the code at a time. Which half contains the defect?
    - Comment out sections of code.
- Better yet, use the debugger to
    - set a breakpoint partway thorugh the program.
    - skip calls to routines, eliminate suspects by skipping execution

> Be suspicious of classes and routines that have had defects before.

- Check code that's changed recently.
- Run a diff tool between versions.
- Run an old version of the program and see if the error occurs
    - Scrutinize the differences.

> Consider the possibility that the defect is not in the section of code you're examining. Look away and divide and conquer.

#### Take a break from the problem
If you’re debugging and making no progress, once you’ve tried all the options, let it rest. Go for a walk. Work on something else. Go home for the day. Let your subconscious mind tease a solution out of the problem.

# CHECKLISTS: Debugging Reminders Techniques for Finding Defects

- ❑ Use all the data available to make your hypothesis.
- ❑ Refine the test cases that produce the error.
- ❑ Exercise the code in your unit test suite.
- ❑ Use available tools.
- ❑ Reproduce the error several different ways.
- ❑ Generate more data to generate more hypotheses.
- ❑ Use the results of negative tests.
- ❑ Brainstorm for possible hypotheses.
- ❑ Keep a notepad by your desk, and make a list of things to try.
- ❑ Narrow the suspicious region of the code.
- ❑ Be suspicious of classes and routines that have had defects before.
- ❑ Check code that’s changed recently.
- ❑ Expand the suspicious region of the code.
