+++
Categories = ["Development","Code Review", "Go"]
Description = "ReReRe, not to be confused with Git’s ReReRe command, is an insightful code review process to help submitters and reviewers sharpen their skills."
date = "2017-07-17T23:11:24-04:00"
title = "Code reviews using ReReRe"
type ="posts"

+++

ReReRe, short for Review/Retest/Reflect, is my three phase process for reviewing code changes. It is one of the key processes that I’ve come to rely on for learning and growing my craft.

Side note, I refer to GitHub and its pull request system within the post; however, ReReRe works independently of GitHub and should work for any team with a formal code review process. In fact, I generally review code changes locally within my editor, as opposed to on GitHub, to allow for off-line reviews. Having a process for off-line reviews is great for building a review tool chain that works independently of any code review system, both on and off the Internet.

### Review

This is the first phase, where I start by checking out the code changes, via git, onto my local machine for review. I then step through the code changes within my editor for technical soundness. More specifically, I check the code for readability, correctness, and simplicity.

#### Readability:

- Does the code read well? Or do I find myself bouncing around trying to figure out what’s happening?
- Does the code clearly outline what it’s trying to achieve?
- Are there tests for the changes? Do the tests make sense?

#### Correctness:

- Does the code follow SOLID principles?
- Are there any glaring syntax issues? Styling issues?
- Does the code validate inputs? And if so how does the code fail for bad inputs?

#### Simplicity:

- Is the code doing more work than it needs to?
- Are there helpers or shared libraries being used?
- Is the code written in a way that would make it easy to extend in future growth?

As I work through my checks, I leave comments in the code base prefixed with the pull request number to reflect later on in my process.

{{< highlight ruby "linenos=inline,hl_lines=8 15-17,linenostart=13">}}
# PR-107 line 14: potential bug with how this method is defined
def parse(opts={}, feed)
end
{{< / highlight >}}

If I come across new concepts or questions I add a comment, with the same PR-No: prefix, containing my questions. These will either go back to the submitter or get answered in my testing phase.

### Retest

This phase is named retest, and not test, because the submitter should have run their initial tests before submitting the code for review. I leverage any tickets, unit tests, and/or comments the submitter may have sent along with the review to get an idea of what to test.

> Reproduction steps for bugs are not optional.

I usually start with a requirements test; Does the code meet the requirements defined in the ticket? Which is helpful in cases where I know there has been a requirements change.

I then move onto unit tests sarting with a passing build and all green tests check. If there were any changes to existing unit tests I check that they test the right conditions. I also check for any edge cases that may have gone untested.

Depending on the nature of the change (e.g user facing, back-end change) I load up the appropriate environments to test the changes as if they were in production. This may be testing within multiple browsers for user facing changes, or within local and sandbox environments for back-end changes.

If there were any questions or thoughts I had in the logical code review I update and test where necessary— recording any findings that may have not been part of the initial tests or requirements.

### Reflection

This is the fun part as there is lots of room for learning, teaching, and collaboration.

I start by running a diff on the source code to extract any in-line comments or questions from the review phase. I filter out anything that may no longer be relevant now that I’ve reviewed and tested the code changes.

For the remainder, I go back and start leaving them as comments on the PR. For items where I’m requesting changes I make the change request along with a little blurb on why. If the requested change is a learning opportunity for the submitter I provide reference material. Likewise, if I come across a learning opportunity for myself I ask the submitter to provide references for me to review later. If it’s something specific to our code base I ask if we can talk about the code changes later.

> Office hours or an informal white board session is a great complement to any review.

If there are changes in the code that I’m truly not familiar with, but someone else on our team might be, I leave a comment asking that an additional review be done by another engineer.

I finish by submitting; repeating the process where necessary.

---
The motivation for this post stems from a short discussion I recently had with my fellow developers about the question “How do you code review?”.
