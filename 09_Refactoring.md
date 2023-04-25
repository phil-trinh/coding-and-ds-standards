# 9 - Refactoring
## Expected Outputs from this Stage
- [ ] Algorithm Optimization
- [ ] Whole Pipeline Analysis
- [ ] Spark Optimization
- [ ] An Evaluation of Refactoring Thresholds

At the end of this stage, your **already functioning code** will be efficient, employ reuse either locally or through imported
libraries, and follow best practices for data pipelining. 



## What is Code Refactoring?
Refactoring means organizing your code without modifying its original functionality. Refactoring is the process of 
making small changes or adjustments to your code without affecting or changing how the code functions while in use.

### Why Do You Need to Refactor Your Code?
The Pattern: As code implementation gets underway, we are developing using requirements defined and agreed to by 
our customers and other stakeholders. Of course, this is the ideal; sometimes, we begin coding with nothing more 
than a problem statement. No matter how robust (or not) initial requirements are, there will be adjustments along 
the way, as new information surfaces, policies change, stakeholders move on, third-party systems and datasets shift, 
and testing begins. To accommodate these changes, we begin to layer in new functionality and new logic. The final 
code base, while fully functional, may be suboptimal--from a performance or maintenance perspective.

#### We refactor our code for three reasons: 
1. **It gives us a better understanding of our code and broadens our knowledge.**
With the code fully functioning and all business requirements implemented, we can step back and appreciate 
our code at a more logical level. View the full pipeline and data transforms; look for coding constructs or 
data structures that better fit the requirement; implement concepts, packages and libraries that may have seemed 
too much to absorb in the rush to deliver. Refactoring is a method of active learning. Even if you didn't write the 
initial code, refactoring it gives you a better grasp of what it does.

2. **It improves our code.**
Whether optimizing for efficiency (very important in big data processing), readability (better variable and 
function names, comments), reuse, leveraging more robust libraries/packages, better error handling, or 
deleting outdated logic, all make the final product more professional, reliable and efficient.

3. **It helps our team and future users/maintainers of our code.**
Perhaps this is most important, refactoring our code for all the reasons above will leave it more easily 
understandable for future analysis, using as a template for similar efforts, and enhancements/upgrades. 

### Refactoring Best Practices
- Refactor only functioning code. By definition, refactoring occurs after requirements have been implemented.
Once refactoring is complete, you will be able to verify results using both the before and after code. 
- Minimize refactoring. The modification is riskier the more drastic it is. Divide it into several more compact portions. 
- Take little, gradual steps. Check to see if everything is working after making a small code change.
- Don't begin adding functionality while refactoring is still being done. Refactoring should have no impact on
the system's output. Before moving on to the next challenge, complete this one.



## Algorithm Optimization
As with software development, common best practices apply. The guidance here is summarized, in part, from
[Foundry Development Best Practices](https://vantage.army.mil/docs/foundry/building-pipelines/development-best-practices/).
- Reduce cognitive load: keep it simple, and if complex functions are required, add clear documentation and links to 
resources for more information
- Refactor to eliminate duplicated code, which can require more maintenance and lead to errors. Use functions already
built in other libraries and packages (bonus, they might employ unit testing, for example, the python-utils library in Vantage)
- Remove technical debt: use the refactoring phase to eliminate code previously labeled "debt" or "TBD"
- Stick to the conventions established in your coding environment: for example, snake-case function names, variable naming conventions, 
docString structure
- Less is more: Systems with many smaller units are easier to understand and maintain than systems with a few large units;
bias towards:
  - Smaller transforms chained together
  - Shorter functions in transforms with helper functions in utility files
  - Simple **and linear** pipelines are simpler to maintain
- Remove old code that has been commented out. This archived code creates clutter and reduces legibility; old code is
easy to find in previous commits.
- Over-verbose commenting: Comments should explain rationale behind decisions, rather than document the logic itself. Strive
to write "self-documenting" code; if a set of statements is difficult to understand, that is a sign to refactor
and simplify.
- See **Spark Optimization** section for algorithm best practices related to efficient processing in Spark.


## Data Pipeline Analysis and Refactoring
- Consider whether each transform is located in the correct Project. Questions to consider: What is the scope of the
pipeline? Where does it start, what additional datasets does it take in? What use cases (other than the current one) will
benefit from the outputs?
- Consider the refresh rate on the pipeline. Revisit schedules ([see this link in Army Vantage for scheduling best 
practices](https://vantage.army.mil/docs/foundry/building-pipelines/scheduling-best-practices/#scheduling-best-practices)). 
Have schedules been implemented, are they efficient (not running too often), will they keep data up-to-date
(running often enough)?
- Consider whether there are critical columns or key validations that must be true for the pipeline to succeed. If yes, 
have controls been put in place to prevent data quality issues? Consider implementing Data Health Checks or Data Expectations,
or add logic to abort builds under set conditions (can be implemented with Incremental builds in Foundry).


## Spark Optimization
The following is excerpted from [this page in Army Vantage](https://vantage.army.mil/workspace/notepad/view/ri.notepad.main.notepad.185d3510-baad-4f05-b492-445a9aa8d48e); 
 resources are cited there.

**Data Cleaning**
- Drop the data we don't need at the beginning of a transform, using select statements (for columns) and filters 
(for rows).
- Choose efficient data types 
  - Data Types - [Spark 3.3.0 Documentation](https://spark.apache.org/docs/latest/sql-ref-datatypes.html)
  - Decimal points, how much precision do we need?
    - Float (if we're okay with small rounding errors)
      - This is probably the most common use case if you don't need high precision out to 5+ decimal points
    - Double
      - Use this data type if you need higher precision (5 - 10 decimals) and/or the dataset is backing an ontology
      object type and needs to be editable. Double is the only compatible data type for ontology edits.
    - Decimal (usually for financial precision)
  - Real numbers, are the current and future possible values within a certain range?
    - Byte (range: -128 to 127)
    - Short (approx. range: -33K to 33K)
    - Integer (approx. range: -2 billion to 2 billion)
      - This would be the default use case for a majority of datasets since it encompasses a wide range of numbers.
      Use the other data types if you're certain that current and future possible values would be within range without
      losing data.
    - Long (approx. range: -9 quintrillion to 9 quintrillion)

**Minimize for-loops**
- For row-wise operations, utilize column operations to replace values in current column or derive row 
values in new columns.
- For column-wise operations, use of .withColumn() in a for loop can lead to poor query planning performance. 
Avoid using .withColumn() inside for loops and instead use direct select() statements.
- Use of joins, Windows, groupby, distinct, or dropduplicates inside a for loop should .cache() the outer dataframes.

**Avoid collecting to drivers using Pandas. Instead, use:**
- Built-in PySpark API functions (most performant)
- Window functions
- User defined functions (UDFs) under certain conditions
  - It is highly recommended that you avoid UDFs wherever possible, because they are significantly less performant than
  an available PySpark equivalent. You'll find that in most situations, logic that seems to require a UDF can be
  refactored to use only native PySpark functions.
- Pandas UDFs
  - A pandas user-defined function is a user-defined function that uses Apache Arrow to transfer data and pandas to 
  work with the data. Pandas UDFs allow vectorized operations that can increase performance up to 100x compared to 
  row-at-a-time Python UDFs
  - Pandas UDFs can be accessed through the import line: from pyspark.sql.functions import pandas_udf

**Once code is optimized as suggested above, optimize further (if needed) with Spark environment settings**
- Change partition size
  - The size of partitions (outputs from PySpark written to dataset files) can be adjusted by changing the number of partitions.
  - To optimize, the size of each partition should be approximately 128MB each. Accordingly, the number of partitions
  should be the dataset size (all partitions added together) divided by 128MB.
  - Partitioning options:
    - coalesce (faster for **write** operations)
      - "coalesce combines existing partitions to avoid a full shuffle." The coalesce function can only decrease the number of partitions.
      - Use coalesce at the end of a transform before returning/writing out datasets.
    - repartition (slower for **write** operations)
      - "The repartition algorithm does a full shuffle of the data and creates equal sized partitions of data." The repartition 
      function can increase or decrease the number of partitions.
      - While coalesce is generally preferred because full shuffles are expensive for large datasets, repartitioning
      may occasionally make more sense if downstream read operations are a high priority and the dataset is not large.
- Reorganize the data
  - "The best way to do this is through using the command order by. While this will improve the speed of downstream filtering, 
  this command requires shuffling the data and is thus expensive, extending the length of this dataset's computation.
  To optimize the computation, choose two or three columns that users will filter on most frequently. The first column
  listed after the order by will be the fastest to filter on."
- Reduce the number of transforms
  - If an intermediate transform is not needed for another data pipeline, then collapse transforms into one to reduce
  overhead on reading/writing data.
- Apply Executor memory and quantity profiles as a last resort
  - Typically, we don't need to increase driver profiles as we shouldn't be collecting. However, if it is required for a
  particular calculation, then this is the very last profile to change.



## An Evaluation of Refactoring Thresholds
**How do I know when something should be refactored?**
There is an art and an intuition to refactoring, often learned from the challenge of maintaining code
developed by someone no longer available to the project. The following list is meant to generate ideas:
- Are there a large number of arguments required as input to a function? Indicates function may be too complex.
- Likewise, are there a large number of inputs/outputs to a transform?
- Is there similar logic in multiple transforms, in multiple helper functions, or across repositories? 
Refactor to reuse functions.
- Is there similar logic in different layers of a pipeline (e.g., both the /clean and /api layer format dates)? Refactor
to put similar cleaning, formatting, enriching, or calculating in the same layers.
- Are comments long yet still not helpful to understanding the code? Are variable and function names meaningful?
- Do you find circular logic in the pipeline, where outputs in a downstream transform are being used as inputs 
to datasets that come upstream?
- Use of for loops, collect statements and similar constructs (see Spark Optimization section above)
