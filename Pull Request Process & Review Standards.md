# Objective
This document is to provide guidelines and steps to take when setting up and reviewing Pull Requests (abbreviated as "PRs") for the code base. These guidelines are meant to be concrete expectations to build into the muscle memory of any developer/engineer/data scientist as they're creating code in order to provide high quality code changes and reviews.

# Summary of Main Points
- Proposal of a team lead/code architect to be the main point of contact for a code base to be in the loop of functionality and organizational changes to the code.
- Set expectations for PR reviews in different phases of development
- Re-enforcing a standardized coding and organization style
- Provide general checklist to think about before setting up PRs for review to minimize churn/questions/bugs
- Filling out as much helpful information in the PR itself.


# Useful Links
- [Code Review Best Practices](https://blog.palantir.com/code-review-best-practices-19e02780015f)



# Submitting Pull Request Checklist
## Before Submitting
- Adhere to Team's Code Style Guide (to be linked) and Code Repository Architecture organization (to be linked)
- Double check desired inputs are correct
	+ Input dataset(s)
	+ Strings, flags, mappings, etc.
- Provide unit tests for desired outcomes (if applicable in the code base)
- Merge latest changes from Master branch (in case it has changed since you branched)
- Ensure code and/or output datasets can be built successfully with
	+ Correct schema
	+ Data Types
	+ Data Format
	
## When Creating the Pull Request
- Descriptive Title
	+ e.g. "Initial Implementation Phase of Project"
- Filled out description with the reason(s) why the changes are being made and a bulletized list of the main changes made (if there are several)
	+ "Clean datasets X, Y, Z"
	+ "Additional filters per guidance from domain owners"
	+ "Calculate percentage ratios of columns D and T"
- Include in desription the link to successful code/dataset builds (if applicable)
- Tag respective reviewers
	+ One reviewer should be a lead/code architect/owner on the task that knows the most about the code base
	+ Another reviewers hould be any teammate who has capacity to review and follow the ["Reviewers' Responsibilities"](Reviewers'-Responsibilities) below to the best of their ability
	+ More reviewers are welcome but can be optional for situational awareness
- Enable/Disable other merging options as desired


## Reviewers' Responsibilities
- Ensure developer has followed the above checklist items
	+ Code/Dataset Builds and checks have passed (if applicable)
	+ Affected datasets successfully built
	+ Descriptive PR title and description
	+ Code Style and Organization standards are followed
	+ Repository organization standards are followed
- Provide feedback on logic using comments within the PR (e.g. better flow, more efficient method, questions, typos)
- Double check impact of changes on affected datasets or outputs
- From the external link above, think through how the code changes address:
	+ Purpose
		* **Does this code accomplish the author's purpose?** Every change should have a specific reason (new feature, refactor, bugfix, etc). Does the submitted code actually accomplish this purpose?
		* **Don't be afraid to ask questions.** Functions and classes should exist for a reason. When the reason is not clear to the reviewer, this may be an indication that the code needs to be rewritten clearly or supported with comments or tests.
	+ Implementation
		* **Think about how you would have solved the problem.** If it's different, why is that? Does your code handle more (edge) cases? Is it shorter/easier/cleaner/faster/safer yet functionally equivalent? Is there some underlying pattern you spotted that isn't captured by the current code? Can it be refactored?
	+ Legibilitiy and Style
		* **Think about your reading experience.** Did you grasp the concepts in a reasonable amount of time? Was the flow sane and were vairable and method names easy to follow? Were you able to keep track through multiple files or functions? Were you put off by inconsistent naming?
		* **Does the code adhere to coding guidelines and styling?** Is the code consistent with the project in terms of style, conventions, etc.?
	+ Maintainability
		* **Is this sustainable long term?** Does the change need to be constantly be addressed in the future? (e.g. whenever new data comes in). Does the change create more technical debt?
		* More tests?** Is there a unit test that is missing or would be a good corner case to address? (If applicable)
- Reviewers only need to review. The should not need to merge changes after approval.

If it's easiest to clarify questions about the PR or walkthrough the changes in advance of the review, it is encouraged to schedule a brief (5-10 minute) meeting between requester and reviewer(s) to minimize back and forth comments.  

If components above are not addressed by the developer, the reviewer has the right to reject (and should reject) the Pull Request outright until they are resolved. However, Reviewer should communicate what is missing within the PR either in the comments or directly with the Requestor.

## After Pull Request is Approved and Merged to Master
- Developer requesting the PR review should be the one mergining the changes
- Wait until all CI/CD checks have successfully passed
- Build affected datasets/outputs, if desired to take effect right away
	+ Double check desired outcomes are correct for each affected dataset/output
- Setup/Adjust Build Schedules for datasets (if applicable)
- Setup/Adjust Health Checks for datasets (if applicable)


# Code Maturity Review Thoroughness
## Initial Code Building Phase

This is part of the Pseudocode & Code Wireframe Planning (to be linked). After a developer completes their pseudocode and code wireframing, a PR is made to review the initial code building phase.

- Code Repository is setup to Code Repository Architecture Standards (to be linked). Close attention to:
	+ folder organization
	+ presence of funcs/param/schema files
- Code Wireframe
	+ Docstrings
	+ Inputs/Outputs
	+ Concise comment blocks of operations
- CI/CD checks should successfully pass before PR is created
- Affected datasets/outputs should successfully build before PR is created
	+ Inputs are mapped
	+ Output datasets can be empty but with the desired schema definition

## Iterative Phase
- New changes still adhere to the Team's Code Style Guide
	+ Include docstrings for all user defined functions
	+ Type hints for input/output parameters of functions
	+ Updated comments if logic has changed
	+ code spacing and readability
- Folder and file organization is followed
- Impact analysis of changes to overall pipeline
	+ Build time (Memory usage may be considered here if build times are getting long)
	+ Complexity (too many transformations?, are all transformations needed?, can operations be done together?)
- Are there any common operations that can be utilized from a common utility library?


## Mature/Refactoring Phase
- New changes still adhere to the Team's Code Style Guide
- All affected datasets/outputs are built successfully
- Comments and docstrings are updated to reflect latest behavior
- Large string of or repetitive operations should be refactored into function files
- Constants should be refactored into param files
- Are there generalizable functions that can be brought into the common utility libray for others to use?


## Bug Fix Phase
- All items from [Mature/Refactoring Phase](#mature-refactoring-phase)
- How could this bug be mitigated in the future?
	+ Unit tests?
	+ More thorough documentation or planning?
	+ Data health checks?
	+ Development/Review gaps?