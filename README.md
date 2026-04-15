# Project Design: Test-Driven Development

## 1. Introduction

At this point in the semester, your team has defined your project, produced a simple RESTful design, created
low-fidelity diagrams, and each member has identified three core features with detailed user stories and BDD acceptance
tests. Some initial implementation should already exist in your repository.

This assignment asks each team member to implement two of their core features using a disciplined Test-Driven
Development (TDD) process anchored by Behavior-Driven Development (BDD) acceptance scenarios. The goal is not a fully
working application — it is evidence of a rigorous engineering process visible in your git history.

## 2. The TDD Workflow: Red-Green-Refactor

TDD is a discipline: tests are written before the code that makes them pass. You will follow the three-phase cycle
below for every increment of functionality.

### Phase Definitions

| Phase    | Phase Description                                                                                                                                                                                                                                                                                                             |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RED      | Write a failing test. Start with a failing Cucumber scenario for the feature. Then write failing RSpec unit tests for the individual components (model validations, controller actions, helper methods) needed to make that scenario pass. Run the tests and confirm they fail. _Commit with a message beginning with [RED]._ |
| GREEN    | Write the minimum code to make the test pass. Write only enough implementation code to make the failing test(s) pass. Do not add untested functionality. Run the tests and confirm they pass. _Commit with a message beginning with [GREEN]._                                                                                 |
| REFACTOR | Improve the code without changing its behavior. With tests green, clean up the implementation. Remove duplication, improve naming, extract methods, follow Rails conventions. Run all tests to confirm nothing broke. _Commit with a message beginning with [REFACTOR]._ The refactor phase is not optional.                  |

### BDD as the Outer Loop

Your acceptance tests (Cucumber scenarios) define the outermost loop. Before writing any RSpec unit tests for a
feature, write a Cucumber scenario that describes the feature from the user's perspective. This scenario will fail
first and pass last — only when all unit-level work is complete.

* Start: Write a failing Cucumber scenario for the feature.
* Middle: Use RSpec to TDD each model, controller, and view component needed.
* End: Confirm the Cucumber scenario now passes. This is the definition of done for a feature.

### Commit Message Convention

Your commit messages must signal which TDD phase the commit represents. Graders will use this to evaluate your
process. A commit without a phase prefix will not receive credit for demonstrating the R-G-R cycle.

| Phase    | Example Commit Message                              | What Is Committed                                                                                                         |
|----------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| RED      | [RED] User cannot sign up without email             | A new failing RSpec or Cucumber test. No implementation code. Test run output (or CI log) confirms it fails.              |
| GREEN    | [GREEN] Add email presence validation to User model | Minimal implementation code to make the failing test(s) pass. The simplest possible solution — no premature optimization. |
| REFACTOR | [REFACTOR] Extract email validation into concern    | Cleaned-up code with no new behavior. All tests continue to pass. Duplication removed, readability improved.              |
| RED      | [RED] Guest user cannot view dashboard              | Next failing test in the feature. Cycle repeats.                                                                          | 

## 3. Individual Requirements

### Features to Implement
Each team member is responsible for implementing 3 of their previously defined core features using TDD. These features 
were documented in your user stories and acceptance tests from the prior assignment.
* Do not choose features that have already been substantially implemented. The TDD process must be visible.
* If there is overlap between team members' features, coordinate early to avoid conflicts. Use branches to isolate work.
* Features must have a corresponding Cucumber scenario (BDD) and at least 3 RSpec tests each.

### Branch Requirements
All feature work must happen on a dedicated branch. Never commit feature work directly to `main`.
* Create a new branch for each feature (or for a logical sub-increment of a feature). A minimum of 3 branches per team member is required.
* Naming convention: `feature/<your-initials>/<short-feature-name>`  (e.g., `feature/jd/user-login`)
* Your branch history must show the Red-Green-Refactor commits within that branch before you open a pull request.

### Pull Request Requirements
Open a pull request (PR) to merge each feature branch into main. Each PR must:
* Have a descriptive title that names the feature and the TDD phase if relevant.
* Include a non-trivial description in the PR body: explain what the feature does, link to the relevant user story, describe your test approach, and note anything interesting or difficult about the implementation.
* Request a code review from all team members by assigning each of them as a Reviewer in GitHub AND mentioning their @username in the PR description or a comment.
* Not be merged until at least one teammate has formally approved the PR via GitHub's review system.
* Be merged by the PR author, not a teammate.

### Code Review Requirements
Each team member must review at least 4 pull requests opened by teammates. A code review must:
* Be submitted as a formal GitHub review (using the Review Changes interface — not just a comment).
* Include substantive, non-trivial comments. Examples of non-trivial comments:
  * Suggesting a more efficient Rails query or ActiveRecord scope.
  * Noting a missing edge case or sad-path test.
  * Recommending extraction of a method to reduce duplication.
  * Identifying a potential security or validation concern.
  * Providing positive feedback on a well-structured test or clever solution.
* Examples of trivial (unacceptable) comments: 'Looks good!', 'LGTM', 'OK'.

## 4. Team Requirements
### Git Repository Setup
* Protect the main branch: require at least 1 approving review before merging. Enable branch protection rules in your 
GitHub repository settings.
* Configure a Continuous Integration (CI) pipeline ([GitHub Actions](https://github.com/features/actions) recommended)
that runs both RSpec and Cucumber on every push to any branch and on every pull request.
* The main branch must be 100% green (all tests passing) at the time of final submission.

### Continuous Integration Configuration
Your GitHub Actions workflow file should run both test suites. A minimal example structure:

```yaml
# .github/workflows/ci.yml
on: [push, pull_request]
jobs:
test:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - uses: ruby/setup-ruby@v1
      with:
        bundler-cache: true
    - run: bundle exec rails db:test:prepare
    - run: bundle exec rspec
    - run: bundle exec cucumber
```

Consult the [GitHub Actions](https://docs.github.com/en/actions) documentation and the 
[Rails CI guides](https://docs.github.com/en/actions/tutorials/build-and-test-code/ruby) for building and testing your
Ruby code.

### Team Coordination
* Hold a brief team meeting (in person or virtual) early in the assignment period to divide features, identify 
dependencies, and set a PR review schedule.
* If two members need the same model or controller, communicate and coordinate. Use short-lived branches to reduce merge conflicts.
* Do not wait until the last day to open PRs or request reviews. Reviewers need time to provide substantive feedback.

## 5. Test Expectations

### Cucumber (BDD) Scenarios
At least one Cucumber feature file per implemented feature (3 per team member minimum).
Scenarios must cover the primary happy path and at least one sad path (e.g., invalid input, unauthorized access).
Step definitions must use Capybara for browser-level feature testing where a UI is involved.

### RSpec Unit & Integration Tests
* Model specs: test all validations (presence, uniqueness, format, etc.), associations, and custom methods.
* Controller/request specs: test authorized and unauthorized access, successful responses, and redirects on failure.
* At least one _sad-path_ test per feature (e.g., what happens when required data is missing or the user lacks permission).
* Tests must be self-explanatory: use descriptive describe, context, and it blocks.

[! NOTE]
We strongly advise the use of the [SimpleCov](https://github.com/simplecov-ruby/simplecov) gem. Although we don't 
expect 100% line coverage, having the gem added and being able to see its metric is a great opportunity for your
professional development.

### What Is Not Required
* 100% line coverage across the entire codebase. Coverage of the 3 features per person is what matters.
* Full end-to-end implementation of the entire application. Partial implementations that are well-tested are preferred 
over complete implementations that are not tested.
* JavaScript testing. Focus on server-side Rails behavior.

## 6. Submission

### What to Submit
Submit your team's GitHub repository via Gradescope by the deadline. Ensure the repository is accessible to the course staff.

## Next
* [Grading rubrics](grading_rubric.md)
* [Frequently Asked Questions (FAQ)](guidance_and_faq.md)
