# 8 - Code Review & Merge
## Expected Outputs from this Stage
Points:
- [ ] Before Submitting a Pull Request
- [ ] While Creating a Pull Request
- [ ] When Reviewing a Pull Request
- [ ] When Merging a Pull Request after Approval


## Git 101 for DevOps Pipelines
Git code versioning lets developers make iterative changes to code that can be tracked, audited, and contributed to in
 parallel. Git projects consist of a default branch (typically referred to as "master" or "main") and development
 branches containing either new features or fixes/patches. Prior to merging a development branch (and its changes) into
 the default branch, a developer should submit a pull request (PR) so that teammates can peer review the code prior to
 merging it in.

Code reviews help to have multiple sets of eyes on code changes to mitigate bugs and human error. Teams should implement
 a process where the default branch is locked down (no direct changes), and proposed code changes from development
 branches are reviewed prior to being merged. This prevents inadvertent errors and encourages best practices, such as
 code styling and organization.


## Helpful Links
- [Git Branch Management](https://git-scm.com/book/en/v2/Git-Branching-Branch-Management)
- [Azure - Adobt a Git Branching Strategy](https://learn.microsoft.com/en-us/azure/devops/repos/git/git-branching-guidance?view=azure-devops)
- [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)


## Best Practices
Below are some best practices for creating/submitting PRs, reviewing PRs, and merging develop branches into the default
 branch. This is centralized around the Palantir Foundry platform but some of the specifics can be generalized to how
 code is implemented outside of this platform.

### Before Submitting a Pull Request
- Keep PRs as small as possible
    - Smaller PRs reduce the potential for conflicts with the default branch, and they make the reviewers' jobs much
      easier
    - Sometimes large PRs are unavoidable, so use your best judgment when determining the size and scope of code changes
      to include in a PR
- Test changes by building affected datasets on the branch
	- In some cases, you may want to build downstream datasets on the branches to ensure they aren't inadvertently
	  affected

### While Creating the Pull Request
- Descriptive Title
	- "Initial Implementation Phase of Project"
- Filled out description with a reason why the changes are being made and a bulletized list of the main changes made
	- "Clean datasets X, Y, Z"
	- "Left Join all together"
	- "Calculate percentage ratios of columns D and T"
- Include in description the link to the successful build of affected datasets
- Tag reviewers
	- Make sure the group of reviewers consists of the following:
		- One reviewer should be a lead person on the task, effort, or teammate that knows the most on what the goal of
		  the changes are and familiar with the code base. This person ultimately decides whether the PR is approved or
		  not.
	    - Another reviewer should be any teammate who has capacity to review and follow the "Reviewer's
	      Responsibilities" to the best of their ability
	    - More reviewers are welcome but can be optional for situational awareness
- If applicable: Enable Delete branch when merged and disable Merge when ready options
    
### When Reviewing the Pull Request
- Ensure developer has followed the above checklist items
	- Checks passed
	- Affected datasets successfully built
	- Descriptive PR Title and description
	- Code Style and Organization
	- Repo Organization
- Provide feedback on logic using the comments function of the code review, if applicable (e.g. better flow, more
  efficient method, or question for understanding)
- Double check impact of changes on affected datasets through the Impact Analysis tab
- The following come with experience but think through how the code changes address:
	- Purpose
		- Does this code accomplish the author’s purpose? Every change should have a specific reason (new feature,
		  refactor, bugfix, etc).
          Does the submitted code actually accomplish this purpose?
	- Implementation
		- Think about how you would have solved the problem. If it’s different, why is that? Does your code handle more
		  (edge) cases? Is it shorter/easier/cleaner/faster/safer yet functionally equivalent? Is there some underlying
		  pattern you spotted that isn’t captured by the current code? Can it be refactored?
	- Legibility and Style
		- Think about your reading experience. Did you grasp the concepts in a reasonable amount of time? Was the flow
		  sane and were variable and method names easy to follow? Were you able to keep track through multiple files or
		  functions? Were you put off by inconsistent naming? Does the code adhere to coding guidelines and code style?
		  Is the code consistent with the project in terms of style, conventions, etc.?
	- Maintainability
		- Is there a unit test that is missing or would be a good corner case to address?
          Is this sustainable long term? Does the change need to be constantly be addressed in the future
          (e.g. whenever new data comes in or when a file name is out of line)

### When Merging the Pull Request after Approval
Reviewers only need to review the PR - they should not be the ones to merge in the code. After receiving approval for
 the PR, the developer who submitted the PR should merge the code.
