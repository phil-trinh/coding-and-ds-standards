# 10 - Hot Fixes
## Expected Outputs from this Stage
Items to think about:
- [ ] Prior to Fixing a Bug
- [ ] Remember Development Steps to Implement the Hot Fix (don't cut corners)
- [ ] Post Hot Fix Implementation
- [ ] How to prevent more bugs in the future


A hot fix is generally defined as a patch to a live system due to a bug or vulnerability. Often times the coding process
 has already been completed and running smoothly for a period of time, before a bug surfaces that suddenly breaks the
 whole pipeline. Hot fixes should be targeted and effectively return the code to a fully operational status.

# Prior to Fixing a Bug
1. Before trying to start fixing the bug, ensure all stakeholders have a clear understanding of the issue. Create a 
   clear plan of attack.
2. Strictly define the scope of the fix. Who will work on it and how will it be conducted?
3. Collect all possible information from the incident. Who reported, who is affected, priority of the issue, was the
   issue reported manually or automatically?
4. Identify the software version and source code repository/branch the pipeline was developed on. List other
   projects/repositories that may be associated with and affected by this bug as well.
5. Document location of log files and associated error messages. These can often be used to track down the source of the
   issue.
6. Reproduce and isolate the issue - identify the file and lines of code that might be creating this issue.
7. Check backing datasets and automated connections.
8. Create a hypothesis for what/where the issue is from.
9. Verify the hypothesis.
10. Set deadlines for code fix, testing and deployment. The timeline may change depending on the severity of the issue.
    a) Determine who is responsible for documenting and entering issues into bug tracker.
    b) Convey the issue and proposed plan of attack to the client if needed.

# Implement Hot Fix
All the usual best practices for code implementation should be followed:
- Create a development branch for fix.
- Use docstrings and inline code comments to document new code.
- Update any existing docstrings and inline code comments if code logic has changed due to the fix.
- Test code and implement unit tests as well, if applicable.
- Ensure code can still build and final outputs correct the error that the fix is addressing.
- Create a Pull Request (PR) for peers to review and be aware of.

# Post Hot Fix Implementation
If something is wrong with the requirements, target structure or business logic, the corrections should be updated to
 the original documentation. Continue to monitor the issue to ensure the fix was properly implemented in production.    

# Prevention
Before looking into hot fixes, let's look into preventing issues in the first place.
1. Ensure having reviewer(s) on all code changes before merging into master. Multiple reviewers is even better.
2. Follow the best practices for requirements gathering, planning target structure, and capturing business logic to
   have steps documented. 
3. Ensure code standards and that code includes comments throughout. Make it as easy as possible for the next person to
   understand why things are happening and what the code is accomplishing. 
4. Meet and discuss code/methods before approving reviews. Make sure everyone involved in the process is ready for
   completion & on the same page.
5. Meet with stakeholders to ensure your pipeline will deliver the required product. Make sure the end result is what
   was expected. 
6. Create clear and concise documentation on pipeline. Documentation should include schema, metadata and other
   documentation such as business logic to explain the process. This should be captured in the previous best practice
   steps.
7. Manage data scheduling and health checks. Make sure appropriate health checks are applied to the end dataset. Make
   sure the health checks are passing before going live.
8. If manual uploaded data is part of the pipeline, ensure uploaders keep using the same data schema as previously used.
