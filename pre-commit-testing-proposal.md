# Mandatory pre-commit testing for LLVM

## Introduction

This is a proposal to define mandatory pre-commit tests for the LLVM project.  
Mandatory pre-commit tests are a set of builds and tests on specific platforms 
that need to pass before a user is allowed to merge a commit it master. 

The goals of this proposal are:

1. Define a list of criteria that mandatory pre-commit tests must meet to allow
   for new tests to be added in the future without requiring a formal proposal.

2. Define an initial set of specific tests to install as mandatory pre-commit
   tests for the the llvm-project repository.


## Motivation

The goal of this proposal is to use pre-commit tests to minimize the number of
trivial build and test failures in the llvm-project repository.  Build failures in the repository
are problematic, because they disrupt developers, other CI systems, and can
interfere with `git bisect`.


## Proposed solution

1. Criteria for adding pre-commit tests:

* Build and test every commit on master in 20 minutes run-time or less and 
  10 minutes or less queuing time.
* At most 1 false positive (meaning the test fails when there is actually no
  problem) per week.
* Must provide additional value compared to the existing pre-commit tests.

In order to be added as a mandatory pre-commit test, a test must be
used as an optional pre-commit test for at least 1 month. An
optional pre-commit test is a test that runs for every
phabricator review, but does block commits to the repository.

The goal of this 1 month period is to determine if the proposed test
meets the criteria.

2. Inital set of specific tests:

* ninja check-all of combined llvm,lld,clang builds on Windows, Mac, and Linux
  TODO: This should be specific versions of Linux.
* No additional clang-tidy or clang-format findings.
* TODO: Other tests.

3. Integration in the existing pre-merge testing infrastructure:

With the refactoring of the pre-merge testing infrastructure on Buildkite,
we can use this infrastructure to add additional checks and platforms.

TODO: How to implement these tests and integrate them into phabricator/github.
TODO: Which CMake flags do we choose?


## Impact On Other Projects

As fewer bugs slip into the master branch, it would be easier for other projects
to base their work on master.

## Frequently Asked Questions

### How is this enforced on a tool level?

Tool support is outside of the scope of this proposal. We do not have an 
infrastructure at the moment that can prevent merging of broken commits. 

### How does this relate to GitHub Pull Requests?

Tool support is outside of the scope of this proposal. However GitHub Pull 
Requests would be one option of enforcing this.

### How about creating a "stable" branch?

Tool support is outside the scope of this proposal. However we could create 
a CI system that automatically merges the `master` branch to a `stable` branch
whenever all pre-merge tests are passing at the moment.

### How to recover from a broken master?

As today, in case someone broke the build/tests, the commit can be reverted by
anyone. 

### Can we auto-revert broken commits?

Tool support is outside the scope of this proposal. However automatic reverts
would be an option to enforce this in a setup where we cannot prevent broken
commits to be pushed to master in the first place.

### But I *really* need to commit this *now*!

There should always be a solution to push a commit to the master branch for
special situations, e.g. to revert a broken build. However the usage of this
shall be monitored and reported (e.g. automatic email).

### What happens if one of the testing machines is down?

People can commit as if this machine never existed. Once the machines is 
recovered, the machine is moved to optional per-merge testing until builds are 
passing again before if is moved back to mandatory.

### What happes if a testing machine becomes to slow?

After discussion with the machine owner, it will be moved from mandatory to 
optional pre-merge testing.

### Who provides the testing machines?

It's BYOM (bring your own machine), as with buildbots.

## Alternatives considered

