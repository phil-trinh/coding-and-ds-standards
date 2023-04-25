# Best Practices For Data Engineering Code Development

## Procedural Stages
1. Requirements Gathering
2. Target Structure
3. Business Logic
4. Proof of Concept
5. Pseudocode
6. Code Wireframe
7. Code Implementation
8. Review and Merge
9. Refactoring
10. Hot Fixes
11. Why

## [Requirements Gathering](01_Requirements_Gathering.md)
The first stage in development. This initial stage is the planning that goes towards explicitly stating what needs to be
 done and producing a rough plan of how it will be done. Requirements Gathering is important because it lays a
 foundation to ensure that developers will make an end-product that addresses an end-user's initial problem. Come up
 with questions either for clients or your team to answer to help reach consensus towards an end-goal. When finished
 with Requirements Gathering, in most cases, you will have successfully crafted a problem statement, created
 client-signed wireframe(s), and have a data ingest plan.

## [Target Structure](02_Target_Structure.md)
As the next stage of the development process, we should know what the expected outputs are. Otherwise, how do we know what to build
 if we don't know what the end result is going to be? This step can be tricky if requirements aren't fully realized,
 meaning ourselves, or the customer, doesn't know what they want. It's important to gather as many requirements as
 possible to be able to define the target structure effectively.

## [Business Logic](03_Business_Logic.md)
Here is where you outline how to get from input(s), as identified in Requirements Gathering, to the target structure in the
 target folder. Before writing code, you must first plan, document, and check the logical steps
 you will need to implement to create your data pipeline. These are the "instructions" or "recipe" that you will follow.
 **Without creating a plan for the code implementation**, what do you need to do to logically transform the input(s) to
 the output(s)?

## [Proof of Concept](04_Proof_of_Concept.md)
After capturing all the requirements and business logic of the changes we would like to implement, it would be
 beneficial to hav a quick proof of concept to confirm the validity of said requirements. This is to ensure that before
 putting major investment into development, we have an idea of the actual results to verify with in order to reduce risk
 of having to go back and rewrite the requirements or logic.

## [Pseudocode](05_Pseudocode.md)
Creating the Pseudocode and Code Wireframes during the planning phases of code development is essential to gather
 requirements, layout the plan of attack, and reduce churn on code changes due to bugs or scope creep. Pseudocode makes
 sure we can describe the process in English and programming logic.

## [Code Wireframe](06_Code_Wireframe.md)
Once you know what the client is asking you for and have a plan to build it, it's time to start to plan out how the code
 will be placed in our repository and files. We want to follow a common directory structure and process across teams and
 projects so that it is easier and faster to start working on someone else's code when you join their project. Although
 there are different ways you could structure your code repository, this organization structure works very well for data
 pipelines. You can customize this structure as needed for your project, but this is a reasonable default for most
 projects.

## [Code Implementation](07_Code_Implementation.md)
After setting up the repository and file infrastructure in the code wireframe step, it's finally time to start
 implementing the actual code logic to fill in the details of the wireframe.

## [Review + Merge](08_Review_and_Merge.md)
This section will set expectations and points to consider before setting up, while creating, when merging, and
 post-merge of a Pull Request for code changes after code has been implemented.

## [Refactoring](09_Refactoring.md)
As a good practice, it's always good to go back through our code after implementation in production to refine, optimize,
 and/or reorganize code after we know how it's been working in the field. Refactoring can bring benefits of faster
 processing times, reduced costs in the long term for computation, easier navigation, and reiterate learnings along the
 way.

## [Hot Fixes](10_Hot_Fixes.md)
Often times the coding process has already been completed and running
 smoothly for a period of time, before a bug surfaces that suddenly breaks the whole pipeline. Hot fixes should be
 targeted and effectively return the code to a fully operational status.

## Why?
- Maintainability
- Teamwork
- Standards
- Scale
- Best Practices