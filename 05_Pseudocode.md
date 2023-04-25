# 5 - Pseudocode
## Expected Outputs from this Stage
- [ ] Pseudocode Implemented in Code Base
- [ ] Identify Common Functions for/from Common Library


## Objective
Creating the Pseudocode and Code Wireframes during the planning phases of code development is essential to gather
 requirements, layout the plan of attack, and reduce churn on code changes due to bugs or scope creep. Pseudocode makes
 sure we can describe the process in English and programming logic. The code wireframe ensures the in-line code
 documentation, expected inputs and outputs, and repository organization is setup properly in order to focus on the
 code implementation in later phases of development.

The purpose is to ensure that the documentation is set up and easy to copy/paste into the code to create a wire frame. 
 The goal is to write the pseudocode in such a way that each line becomes a comment for a small line or block of code.
 The pseudocode should serve as a way to help your teammates understand what you are trying to achieve with the overall
 transform, or a particular function in the transform. It can also serve as a way to lay out the logic and think through
 the steps required. The goal is to write the pseudocode in a clear, plain language (no technical terms, no code, no
 acronyms, no jargon) so that we assume as little as possible about the reader's background knowledge. This can then
 serve as a way to help describe to a customer the process we will use, but without being too technical. 


## Business Logic in Pseudocode
"In the initial state of solving a problem, it helps a lot if we could eliminate the hassle of having to be bound by the
 syntax rules of a specific programming language when we are designing or validating an algorithm. By doing this, we can
 focus our attention on the thought process behind the algorithm, how it will/ won’t work instead of paying much
 attention to how correct our syntax is.

Here is where pseudocode comes to the rescue. Pseudocode is a technique used to describe the distinct steps of an
 algorithm in a manner that is easy to understand for anyone with basic programming knowledge.

Although pseudocode is a syntax-free description of an algorithm, it must provide a full description of the algorithm’s
 logic so that moving from it to implementation should be merely a task of translating each line into code using the
 syntax of any programming language."
 (https://towardsdatascience.com/pseudocode-101-an-introduction-to-writing-good-pseudocode-1331cb855be7)

"It’s simply an implementation of an algorithm in the form of annotations and informative text written in plain English.
 It has no syntax like any of the programming language and thus can’t be compiled or interpreted by the computer."
 (https://www.geeksforgeeks.org/how-to-write-a-pseudo-code/)
1. Arrange the sequence of tasks and write the pseudocode accordingly.
2. Start with the statement of a pseudo code which establishes the main goal or the aim.
3. Use appropriate naming conventions. 
4. Elaborate everything which is going to happen in the actual code. Don’t make the pseudo code abstract.
5. Use standard programming structures such as IF-THEN, FOR, WHILE, the way we use it in programming. The logic that
   goes in this block should be indented for clarity.
6. Don’t write the pseudo code in a complete programmatic manner. It is necessary to be simple to understand even for a
   layman or client, hence don’t incorporate too many technical terms.

You should be able to answer the following questions by the end of the [pseudocode] document:
- Would this pseudocode be understood by someone who isn't familiar with the process?
- Is the pseudocode written in such a way that it will be easy to translate it into a computing language?
- Does the pseudocode describe the complete process without leaving anything out?
- Is every object name used in the pseudocode clearly understood by the target audience?
(https://www.wikihow.com/Write-Pseudocode)


## Guidelines
- List out input datasets needed
- In a table format, list out final data model

When possible, describe the step in plain English, without using code, syntax, or technical terms. Each line of
 pseudocode should translate roughly into one line of code - or one small section of code. These lines should translate
 into the comments for each small section of your utility function(s) that need to be written.

There is no universal syntax, but there are a few general principles:
- Start off with a description of the algorithm being written up.
- Use <-- to indicate assignment. This might be less confusing than the typical = operator to someone who does not read
  code.
  - x <-- 1 might be a bit clearer than x = 1
  - x = 1 can be read as "assign 1 to x", or as "stating x equals 1 always"
  - Possibly better still would be to write "ASSIGN 1 TO x"
- Use capitalized keywords like IF/ELSE/ELSE IF/FOR/WHILE with indented blocks. Use THEN even though there is no Python
  equivalent so that it reads a bit more like English. Stop with an END to guide the user in case the indentation is
  hard to pick up. Here, the bullets are helping to show indented blocks, there is no requirement to include bullets,
  but it may be an easy way to show indentation.
  - IF the value is greater than 0 THEN
    - Print "High"
  - ELSE IF the value is less than 0 THEN
    - Print "Low"
  - ELSE
    - Print "Zero"
  - END IF
  - FOR counter FROM 1 TO 100 STEP BY 1
    - ...explain the steps...
  - END FOR
  - Same for a WHILE/END WHILE


## Example
### Inputs
- Dataset A
- Dataset B

### Final Data Schema
| Column Name | Data Type | Nullable | Description           |
|-------------|-----------|----------|-----------------------|
| key         | string    | False    | Unique Identifier     |
| column_A    | int       | True     | ...represents this... |
| column_B    | date      | True     | ...represents that... |

### Final Dataset Example
| key | column_A | column_B   |
|-----|----------|------------|
| 1   | 500      | 2016-07-28 |
| 2   | 135684   | 2016-07-29 |

### Pseudocode
- Union Dataset A and B
- IF column_1 is null THEN
  - Return 0
- ELSE
  - Return column_1's value
- END IF
- Grouping by column_B, compute mean of column_1, alias as column_A
- Create primary_key column by concatenating column_1 and column_2
