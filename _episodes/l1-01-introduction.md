---
title: "Introduction to Software Buildsystems"
teaching: TBC
exercises: TBC
questions:
- "Why use a buildsystem?"
- "How can I build software with buildsystems?"
- "What makes a good buildsystem?"
objectives:
- "Appreciate the benefits of using a buildsystem"
- "Understand what buildsystems can and can't achieve"
- "Describe various buildsystems, and their relative strengths"
keypoints:
- "Buildsystems like CMake are the gold standard for compiling software projects"
- "Large and complex codebases require a long and repetitive series of commands to build"
- "Manual building quickly becomes unfeasible as the codebase grows"
- "Buildsystems ensure that our software is built correctly and reliably every time"
- "Allows us to consistently compile programs accross platforms and compilation toolsets"
- "Allows builds to be easily optimised for a given architecture using compiler flags"
- "Buildsystems may be extended with testing and deployment functionality"
---

## Why Use Buildsystems?




Compiling and distributing software is





SMALL PROGRAMS -- COMPILE BY HAND BY WRITING COMMANDS

MEDIUM TO LARGE PROGRAMS -- COMPLEXITY OF COMPILATION AND LINKING COMMANDS BECOMES PROHIBITIVE






There are a number of compelling reasons to properly test research code:

- Show that physical laws or mathematical relationships are correctly encoded
- Check that code works when running on a new system
- Make sure new code changes do not break existing functionality
- Ensure code correctly handles edge or corner cases
- Persuade others your code is reliable
- Check that code works with new or updated dependencies

Whilst testing might seem like an intimidating topic the chances are you're
already doing testing in some form. No matter the level of experience, no
programmer ever just sits down and writes some code, is perfectly confident that
it works and proceeds to use it straight away in research. Instead development
is in practice more piecemeal - you generally think about a simple input and the
expected output then write some simple code that works. Then, iteratively, you
think about more complicated example inputs and outputs and flesh out the code
until those work as well.

When developers talk about testing all this means is formalising the above process
 and making it automatically repeatable on demand.

This has numerous advantages over a more ad hoc approach:

- Provides a record of the tests that have been carried out
- Faster development times - get feedback on changes quickly
- Encourages writing more modular code
- Ensures breakages are caught early in development
- Prevent manual testing mistakes

As you're performing checks on your code anyway it's worth putting in the time
to formalise your tests and take advantage of the above.

> ## A Hypothetical Scenario
>
> Your supervisor has tasked you with implementing an obscure statistical method
> to use for some data analysis. Wanting to avoid unnecessary work you check
> online to see if an implementation exists. Success! Another researcher has
> already implemented and published the code.
>
> You move to hit the download button, but a worrying thought occurs. How do you
> know this code is right? You don't know the author or their level of
> programming skill. Why should you trust the code?
>
> Now turn this question on its head. Why should your colleagues or supervisor
> trust any implementation of the method that you write? Why should you trust
> work you did a year ago? What about a reviewer for a paper?
>
> This scenario illustrates the sociological value of automated testing. If
> published code has tests then you have instant assurance that its authors have
> invested time in the checking the correctness of their code. You can even see
> exactly the tests they've tried and add your own if you're not
> satisfied. Conversely, any code that lacks tests should be viewed with
> suspicion as you have no record of what quality assurance steps have been
> taken.
{: .callout}

## Types of Testing

Once testing is formalised, the inevitable consequence is that there would be
different types of testing for different purposes. In this course, we will
put the focus on unit tests and integration tests, but you might hear about:

- [Unit testing](https://softwaretestingfundamentals.com/unit-testing/)
- [Integration testing](https://softwaretestingfundamentals.com/integration-testing/)
- [System testing](https://softwaretestingfundamentals.com/system-testing/)
- [Acceptance testing](https://softwaretestingfundamentals.com/acceptance-testing/)
- [Functional testing](https://softwaretestingfundamentals.com/functional-testing/)
- [Compliance testing](https://softwaretestingfundamentals.com/compliance-testing/)
- [Performance testing](https://softwaretestingfundamentals.com/performance-testing/)
- [Regression testing](https://softwaretestingfundamentals.com/regression-testing/)
- [Usability testing](https://softwaretestingfundamentals.com/usability-testing/)
- [Penetration or security testing](https://softwaretestingfundamentals.com/security-testing/)
- (and others)

The jargon here can be intimidating, but you do not need to to worry about most
of these for this course.

You can get more information about most of these types of testing as well as
in general about several techniques used when writing tests in
[Software Testing Fundamentals](http://softwaretestingfundamentals.com).

### Unit Testing

This is the main type of testing we will be dealing with in this course. Unit
testing refers to **taking a component of a program and testing it in
isolation**. Generally this means testing an individual class or function.

For this kind of testing to make sense, or even just to work, your code needs to
be modular, written in small, independent components that can be easily unit tested.
Therefore, a side effect of writing unit tests is that it forces you, in a sense,
to improve the quality and sustainability of your code because, otherwise, it will
not be testable!

### Integration Testing

Integration goes a step forward and tests if multiple components working as a group
work as expected. They are typically designed to expose faults in the
interaction of the different units, e.g. inconsistent number or type of inputs/outputs,
wrong structure of these, inconsistent physical units, etc.

### Testing Done Right

It's important to be clear about what software tests can provide and
what they can't. Unfortunately it isn't possible to write tests that completely
guarantee that your code is bug free or provides a one hundred percent faithful
implementation of a particular model. In fact it's perfectly possible to write
an impressive looking collection of tests that have very little value at all.
What should be the aim therefore when developing software tests?

In practice this is difficult to define universally but one useful mantra is
that good tests ***thoroughly exercise critical code***. One way to achieve this
is to design test examples of increasing complexity that cover the most general
case the unit should encounter. Also try to consider examples of special or edge
cases that your function needs to handle especially.

A useful quantitative metric to consider is **test coverage**. Using additional
tools it is possible to determine, on a line-by-line basis, the proportion of a
codebase that is being exercised by its tests. This can be useful to ensure, for
instance, that all logical branching points within the code are being used by
the test inputs.

> ## Testing and Coverage
>
> Consider the following C++ function:
>
> ```cpp
> /*Returns the n'th term of the Fibonacci sequence.*/
> int recursive_fibonacci(int n)
> {
>     if (n <= 1) {
>         return n;
>     } else {
>         return recursive_fibonacci(n - 1) + recursive_fibonacci(n - 2);
>     }
> }
> ```
>
> Try to think up some test cases of increasing complexity, there are four
> distinct cases worth considering. What input value would you use for each case
> and what output value would you expect? Which lines of code will be exercised
> by each test case? How many cases would be required to reach 100% coverage?
>
> For convenience, some initial terms from the Fibonacci sequence are given
> below:
> 0, 1, 1, 2, 3, 5, 8, 13, 21
>
> > ## Solution
> >
> > ### Case 1 - Use either 0 or 1 as input
> >
> > **Correct output:*- Same as input
> > **Coverage:*- First section of if-block
> > **Reason:*- This represents the simplest possible test for the function. The
> > value of this test is that it exercises only the special case tested for by
> > the if-block.
> >
> > ### Case 2 - Use a value > 1 as input
> >
> > **Correct output:*- Appropriate value from the Fibonacci sequence
> > **Coverage:*- All of the code
> > **Reason:*- This is a more fully fledged case that is representative of the
> > majority of the possible range of input values for the function. It covers
> > not only the special case represented by the first if-block but the general
> > case where recursion is invoked.
> >
> > ### Case 3 - Use a negative value as input
> >
> > **Correct output:*- Depends...
> > **Coverage:*- First section of if-block
> > **Reason:*- This represents the case of a possible input to the function
> > that is outside of its intended usage. At the moment the function will just
> > return the input value, but whether this is the correct behaviour depends on
> > the wider context in which it will be used. It might be better for this type
> > of input value to throw an exception, however. The value of this
> > test case is that it encourages you to think about this scenario and what
> > the behaviour should be. It also demonstrates to others that you've
> > considered this scenario and the function behaviour is as intended.
> >
> > ### Case 4 - Use a non-integer input e.g. 3.5
> >
> >Luckily, this is not possible in C++, being a strongly typed language, so one
> > less thing to test. However, it can be an issue in other programming languages
> > like Python, in which case some sort of [defensive programming](https://en.m.wikipedia.org/wiki/Defensive_programming)
> > should be put in place to handle these cases.
> {: .solution}
{: .challenge}

### Summary

The importance of automated testing for software development is difficult to
overstate. As testing on some level is always carried out there is relatively
low cost in formalising the process and much to be gained. The rest of this
course will focus on how to carry out unit testing.

{% include links.md %}
