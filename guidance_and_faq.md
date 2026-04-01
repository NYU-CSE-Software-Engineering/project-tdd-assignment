# Guidance & Frequently Asked Questions

## _"How small should each RED commit be?"_
As small as one test. Writing one failing test, committing it, then implementing the minimum code, committing that, 
is perfectly correct. You do not have to write all tests for a feature upfront. Iterate through small R-G-R cycles.

## _"What if I need to modify shared code (e.g., the User model)?"_
Coordinate with teammates. Decide who owns that change, make it on a branch, review it together, and merge it before 
others build on top of it. Use `git rebase` or `merge` to incorporate the change into active branches.

## _"Can my Cucumber steps re-use steps from a previous assignment?"_
Yes, but make sure they still accurately describe the feature's acceptance criteria. Steps should document the 
intended behavior, not just assert that existing code works.

## _"What counts as a non-trivial PR comment?"_
A comment is non-trivial if it would change or improve the code being reviewed, or if it explicitly acknowledges a 
thoughtful design decision. It must demonstrate that you read and understood the code, not just scanned it.

## _"The CI keeps failing on setup/database errors. What do I do?"_
Check that your `database.yml` test configuration uses environment variables compatible with the CI environment, that 
your schema is migrated (`db:test:prepare`), and that all required gems are in the `Gemfile`. Post a specific error 
message in the EDStem discussion board if you are stuck.  Also, consider visiting with one of our course assistants.
