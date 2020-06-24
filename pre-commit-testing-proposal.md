# Mandatory pre-commit testing for LLVM

*   Proposal: [LP-0123](link to the file on github when it lands)
*   Author(s): Tom Stellard, , Author 2, ...
*   Review Managers: TBD1, TBD2[, TBD3, TBD4]
*   Status: [WIP, Active review (date...date), Accepted, Rejected, Returned for revision, etc.]

_During the review process, add the following fields as needed:_

*   Pitch Thread: [1](url for discussion) [2](url for discussion)
*   Decision Notes: Rationale, Additional Commentary
*   Previous Revision: [1](url for previous significant revision)
*   Previous Proposal: LP-0012


## Introduction

This is a proposal to create mandatory pre-commit tests for the LLVM project.  The goals
of this proposal are:

1. Define a list of criteria the mandatory pre-commit tests must meet to allow
   for new tests to be added in the future without requiring a formal proposal.

2. Define an initial set of specific tests to install as mandatory pre-commit
   tests for the the llvm-project repository.


## Motivation

The goal of this proposal is to use pre-commit tests to minimize the number of
build failures in the llvm-project repository.  Build failures in the repository
are problematic, because they disrupt developers, other CI systems, and can
interfere with `git bisect`.


## Proposed solution

1. Criteria for adding pre-commit tests:

* 20 minutes run-time or less
* At most 1 false positive (meaning the test fails when there is actually no
  problem) per week.
* Must provide additional value compared to the existing tests.

In order to be added as a mandatory pre-commit test, a test must be
used as an non-mandatory pre-commit test for at least 1 month. A
non-mandatory pre-commit test is a test that runs for every
phabricator review, but does block commits to the repository.

The goal of this 1 month period is to determine if the proposed test
meets the criteria.

2. Inital set of specific tests:

* ninja check-all of combined llvm,lld,clang builds on Windows, Mac, and Linux
  TODO: This should be specific versions of Linux.
* TODO: Other tests.


TODO: How to implement these tests and integrate them into phabricator/github.

## Impact On Other Projects

## Frequently Asked Questions

## Alternatives considered

