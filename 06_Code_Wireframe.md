# 6 - Code Wireframe
## Expected Outputs from this Stage
At the end of this stage you'll have the beginnings of your code structure set up with directories, files, and tests.
 In the next step you'll populate those with the specifics of your implementation.

The most common files you'll include are:
- [ ] Main Transform
- [ ] Funcs
- [ ] Params
- [ ] Schema
- [ ] Unit Tests

For an example of how to implement this in Palantir Foundry, see the `Example Code Wireframe for Palantir`
 directory in this repo.



## Main Transform
The transform is the main section of your code, this is what is actually executed. We should keep this file high level,
 focused mainly on where the data is coming from and where it is going. Major intermediate steps can go here if it makes
 the code easier to understand, and you don't want to write them to a file. Specific instructions like select, filters,
 or calculations should not go in this file unless the transform is very simple, most of that more detailed information
 should go in functions in the `funcs` file that accompanies the transform.

You should use data expectations to check the quality of the data coming in and out of your pipeline.

You should also write a descriptive docstring for this transform, according to the guidelines at the bottom of this
 section.

You should create an integration test that ensures this function is processing the data as you expect in the
 `tests` directory.



## Funcs
The bulk of your code will go in this file, including all the "helper functions" that you call in your main transform.
 Each function should perform a small number of tasks (usually only 1) and contain only a few parameters. All the
 processes being done in a function should occur at the same cognitive level. e.g. if a function is selecting and
 filtering data then any cleaning of specific fields should be split out into a different function that you call in your
 parent function. That way you only need to look at the cleaning code if you're interested in that specific detail.

Each function should start with a docstring that explains what the function does, what data it expects, and what it
 returns. Write each of those now as part of your wireframe so your team can discuss the inputs and outputs and confirm
 they are accurate before you start writing your code. Make sure they are simple and clear enough for a new user to
 understand the function out of context. This docstring will appear in most IDEs when a future developer hovers over
 your function. Ideally, I should be able to understand your function just from this text without needing to read any of
 the code.



## Params
The params file should contain the implementation details of how to connect to your datasets. This allows you to easily
 point your transform to a new location or data source if the objects you're using change. You can also use this file to
 store constants or other magic numbers used by your code in a single location. However, try to avoid storing lists of
 columns or other information that is only used in a single function, those should be included in the functions that use
 them.
 
 
 
## Schema
Schemas defined in advance of the creation of an output DataFrame can help ensure that a DataFrame meets expectations
 and contains the complete set of desired columns represented in a DataFrame. PySpark StructType and StructField types
 are used to define the schemas of output DataFrames. StuctFields define a column's name, data type, and whether nulls
 are allowed in the column. StructTypes are collections of StructFields and represent the complete schema of a
 DataFrame.
 
This is especially useful when applied to the final output DataFrame of a transform so that the expected
 columns of the schema are present, the data types for each column are applied, and the expected nullable behavior is
 adhered to before the data moves downstream.

### Defining a Schema
Below is an example of a schema definition using StructType and StructField. StructTypes contain a list of StructFields.
 Each StructField below has three arguments. Arguments are spaced out to improve readability, but this is not required.
- Name of column
- Data Type of column
- Whether or not the column can contain nulls, where True means that the column can contain nulls and False means that
  it cannot.

Please review this page for additional help:
 [sparkbyexamples](https://sparkbyexamples.com/pyspark/pyspark-structtype-and-structfield/)

### Example Schema Definition Syntax
```
from pyspark.sql import types as T

example_schema = T.StructType([
    T.StructField("date_updated",   T.DateType(),       True),
    T.StructField("lhc",            T.StringType(),     True),
    T.StructField("year",           T.StringType(),     True),
    T.StructField("categories",     T.StringType(),     True),
    T.StructField("values",         T.DoubleType(),     True),
    T.StructField("date_rank",      T.IntegerType(),    True),
    T.StructField("primary_key",    T.StringType(),     False),
])
```

### Applying a Schema and Verify Output with Empty Dataframe
Once a schema is defined, it must be applied to the output DataFrame. A nice verification of the placement of the output
 dataset is to apply the above schema with an empty dataframe. The following example will create an empty Spark RDD
 object and apply the 'example_schema,' defined above, to then create an empty dataframe from.

```
from pyspark.sql import SparkSession
 
# Create a new spark session in order to create the DataFrame and apply the schema
spark = SparkSession.builder.getOrCreate()
 
# Create empty RDD
empty_data = spark.sparkContext.emptyRDD()

# Then apply schema and create dataframe
output_dataframe = spark.createDataFrame(data=empty_data, schema=example_schema)
```



## Unit Tests
Unit tests help us make sure the code we wrote meets the needs of our functional partners. Tests give us a common
 language to talk to our customers about what the code should do in each situation. By talking through what it should do
 before we start working we can find potential issues before we start writing code.

Additionally, code that has unit tests is much harder to break. If you have written unit tests when you initially create
 a pipeline I can automatically test if my changes are breaking any functional business rules without needing to read
 and understand all the code. If the changes I make to the code don't cause any other tests to fail (and you wrote good
 tests when you first wrote the code) I can feel safer making my changes. If I do cause a test to fail I can see exactly
 what business rule I'm breaking and understand that detail. Code with good test coverage means I don't need to spend as
 much time reviewing pipelines before pushing my changes and I don't have to check with the clients or other teammates
 to get information they've already learned initially building the pipeline.
 
In code wireframing, the unit testing structure should be laid out in the project along with outlining the files with
 the known unit test cases already known. There's no need to implement the unit tests at this time. A good
 organizational rule of thumb is to create a set of folders inside your project directory that match your code structure
 to store your unit tests. This will make it easy to find all the tests associated with one of your modules. In pytest,
 a test script starts with the prefix "test_" and then should describe the function or set of functions it applies to.

For example, a good structure would look like:
- src
  - my_project
    - transform
      - example_transform.py
      - example_transform_funcs.py
  - tests
    - transform
      - test_brown_eyes.py



## Docstrings for All Functions
The purpose of a docstring is to provide a set of basic information to a future user about the functionality and
 application of your function/module/class.

Note: The python definition of a docstring is laid out in PEP 8 and further defined in
 [PEP 257](https://peps.python.org/pep-0257/). However, there are multiple common formats for writing docstrings that
 are used in different parts of the Python ecosystem. There is no single right way to write a docstring, the important
 part is that you are using docstrings and that you are consistent in which format you choose throughout your project.
 For the rest of this example we will use the reStructuredText (reST) format used by Sphinx to generate documentation
 because it is the official Python documentation standard and probably the most common.
 [Google](https://google.github.io/styleguide/pyguide.html) docstrings and
 [numpydoc](https://numpydoc.readthedocs.io/en/latest/format.html) are both well accepted alternatives. If you have no
 preference or are starting a new project this guide gently suggests reST as an initial standard.

Python makes use of the object paradigm to encode a \_\_doc__ method in each object. By creating a docstring following
 an object (function/module/class etc.) it is automatically assigned to the \_\_doc__ method and can be accessed using
 the help() function in the terminal. You can also access docstrings via the
 print(*package/module/function name*.\_\_doc__). Your IDE makes this docstring available via mouse hover-over when
 calling or importing said function/module/class.

### What Needs a Docstring:
- Functions
- Modules (think utils.py)
- Classes
- Methods
- Transforms

### Criteria For Excluding Docstrings:
In order for a docstring to not be necessary an object should meet the following criteria:
- Not Externally Visible
- Very Short
- Obvious to the client/future users

### Docstring Conventions:
The following are a list of conventions that one should adhere to when crafting docstrings:
- Docstrings should have enough information for someone to write a call without referencing the code.
- Docstrings should give detail on calling syntax and arguments but implementation details should be left inside
  comments within the function.
  - Ex: If a function changes one of the input parameters it should be recorded in the docstring.
- Docstrings are declarative not imperative
  - Ex: """Squares the input value""" vs """Square the input value""" (declarative/imperative)
  - Ex: """Fetches rows from table""" vs """Fetch rows from table""" (declarative/imperative)
- Docstrings can reference one another if you are writing a function that overrides another method somewhere. Your
  docstring can read """See base class""" or something similar.

### Formatting:
The specifics of your docstring implementation will depend on which docstring standard you are using (reST/Sphinx,
 Google, numpydoc) and any other team-specific guidelines. If you have no preference, or are starting a new project,
 this guide gently suggests reST as an initial standard.

An in-depth tutorial of Sphinx docstring format can be found
 [here](https://sphinx-rtd-tutorial.readthedocs.io/en/latest/docstrings.html). The example below is adapted from that
 tutorial.

In general, a typical Sphinx docstring has the following format:
```
"""[Summary]

[Longer Description]

:param [ParamName]: [ParamDescription], defaults to [DefaultParamVal]
:type [ParamName]: [ParamType](, optional)
:raises [ErrorType]: [ErrorDescription]
:return: [ReturnDescription]
:rtype: [ReturnType]
"""
```

The first line is a brief (one-line!) explanation of the function. A longer description should follow it, separated by a
 blank line. 

The options should then follow, again separated by a blank line. Each option should be a separate line, directly below
 each other (i.e., no blank line).
* A **pair** of `:param [ParamName]:` and `:type [ParamName]:` directive options must be used for each parameter. You
  should repeat this pair of options for every parameter in your function.
* The `:raises:` option is used to describe any errors that are raised by the code. You may repeat this option for all
  the errors raised in your function, or omit if not applicable.
* The `:return:` and `:rtype:` **pair** of options are used to describe any values returned.

For guidance on using Google docstrings in Palantir Foundry, see the appendix
 [here](best_practices_appendix.mdoogle-docstring-formatting-guidelines).

### Additional Resources:
- [PEP 257](https://peps.python.org/pep-0257/)
- [PEP 8](https://peps.python.org/pep-0008/)
- [Google Py-Guide: Comments & Docstrings](https://github.com/google/styleguide/blob/gh-pages/pyguide.md#38-comments-and-docstrings)
- [RealPython: Documenting Python Code](https://realpython.com/documenting-python-code/)
- [StackOverflow: What are the most common types of docstrings?](https://stackoverflow.com/questions/3898572/what-are-the-most-common-python-docstring-formats)
- [DataCamp: Docstrings in Python](https://www.datacamp.com/tutorial/docstrings-python)

We recommend using the [pytest](https://docs.pytest.org/) package to write and manage your unit tests.



## Documenting Column Descriptions
Column descriptions are intended to provide users of datasets with information about what each column represents and
 should include any helpful information and caveats about a column that would be important for users to know. Some
 important pieces of information to include might be a general description of the column, whether the column is the
 primary key of the dataset, whether a column is a foreign key for another dataset, and any additional information that
 may not be obvious about a column but is important to understand. Column descriptions can be stored directly in code,
 usable and viewable by developers, and can be stored in any external documentation for dataset end-users in a data
 dictionary.
 
Palantir's Foundry has the capability of programmatically applying column descriptions as part of executing transformations
 in a data pipeline. If this capability is available in other data processing platforms, it should certainly be used.
 
### Dictionaries
In Foundry, column descriptions are created and stored in a dictionary. This allows for quick reference by users of the dataset
 working in code. We recommend this column description dictionary be created in the schema.py file to coexist with the final dataset
 schema (see above section "Example Schema Definition Syntax").
```
dataset_col_descriptions = {
    "date_updated": "Calendar date when values were updated.",
    "reporter": "Reporting entity (unit, command, etc.).",
    "year": "Year when value was recorded as a four digit year.",
    "categories": "Categories of project status (Complete, Accepted, Assigned, Submitted, Reviewed, Revised).",
    "values": "Counted amount per category.",
    "date_rank": "Chronological order of year column.",
    "primary_key": "Unique row identifier (reporter + year + categories)."
}
```

### Tables
Data dictionaries can also be used to store and reference column descriptions by non-technical end-users. Below is an
 example data dictionary in a tabular format.

| Column name | Column description |
| ----------- | ----------- |
| date_updated | Calendar date when values were updated.|
| reporter   | Reporting entity (unit, command, etc.).  |
| year      | Year when value was recorded. |
| categories   | Categories of project status (Complete, Accepted, Assigned, Submitted, Reviewed, Revised). |
| values      | Counted amount per category.|
| date_rank   | Chronological order of year column.|
| primary_key      | Unique row identifier (reporter + year + categories). |

### Example Column Description Template
This template demonstrates how to propagate column descriptions through the data pipeline in Palantir Foundry. 
 It collects the column descriptions from upstream datasets in a single dictionary, updates
 the description of a column, and then saves the output dataset with the new column description metadata.

NOTE: This example template assumes that two upstream datasets already have column descriptions. See "Example Code
Wireframe" below for the initial setting of column descriptions.
```
from transforms.api import transform, Input, Output
@transform(
    my_output=Output("/Army_NIPR/CRRT_ONTOLOGY/code/transform/recently_deceased_soldiers/column_desc"),
    my_first_input=Input("/Army_NIPR/CRRT_SENSITIVE/data/DCIPS/casualty_snapshot"),
    my_second_input=Input("/Army_NIPR/CRRT_SENSITIVE/data/DAMIS/progress_release_snapshot"),
)
def compute(my_first_input, my_second_input, my_output):
    first_df = my_first_input.dataframe().limit(10)
    second_df = my_second_input.dataframe().limit(10)

    # Load column descriptions from the first input to a new dictionary
    new_descriptions = my_first_input.column_descriptions

    # Add the column descriptions from the second input to the dictionary
    new_descriptions.update(my_second_input.column_descriptions)

    # CODE HERE TO TRANSFORM DATASETS
    final_df = first_df.join(second_df, on="primary_key")

    # Update any descriptions that changed or create new entries in the dictionary for any new columns
    new_descriptions["current_uic"] = "This is the soldier's current UIC from DCIPS source."

    my_output.write_dataframe(
        final_df,
        column_descriptions=new_descriptions,
    )
```



## Example Code Wireframe
Once implemented, the code wireframe will build an empty dataset with the target schema and column descriptions as defined in schema.py.

### mishaps.py  # main transform
```
from pyspark.sql import functions as F, SparkSession
from transforms.api import transform, Input, Output

from .mishaps_params import mishaps_select, INPUT_DIR, OUTPUT_DIR
from .mishaps_schema import MISHAPS_SCHEMA, MISHAPS_COL_DESCRIPTIONS


@transform(
    out=Output(OUTPUT_DIR + "bhm_mishaps_output"),
    mishaps=Input(INPUT_DIR + "Mishaps_Sample_Data"),
)
def mishaps_transform(mishaps, out):
    """
    This function takes in mishap raw case data and maps categories from each column and downselects for Class A mishaps
    only. The resulting dataset contains case information per row that will be aggreageted in Workshop for the 
    visualizations.

    Args:
        mishaps: Raw mishaps data uploaded monthly by Army Analytics Group (AAG) from ASMIS2.0 data source

    Returns:
        out: Cleaned, aggregated and date ranked Mishap data
    """
    mishaps_df = mishaps.dataframe()

    # Select raw columns needed to pair down raw dataset, in params: mishaps_select

    # Filter the dataset for MishapClassificationCode == A

    # Rename USArmyMilitaryArmyCountableFatalCount "fatality_count"

    # Create new column "metric_category"
    #   If MishapCategoryLevel1 = "Afloat (Maritime)" return "Afloat"
    #   else if MishapCategoryLevel1 = "Aviation" return "Aviation"
    #   else if MishapCategoryLevel1 = "Ground" AND MishapCategoryLevel2 = "Other, Ground" or "Sports, Recreation and
    #           Physical Fitness" return "Ground"
    #   else if MishapCategoryLevel1 = "Ground" AND MishapCategoryLevel2 = "Weapons/Explosives" return "Weapons"
    #   else if MishapCategoryLevel1 = "Ground" AND MishapCategoryLevel2 = "Motor Vehicle" return "Motor Vehicle"
    #   else if MishapCategoyLevel1 = "Space" return "Space"

    # Create new column called "metric_secondary_category"
    #   Rename MishapCategoryLevel2 value "Aerostat" --> "Unmanned"
    #   if "metric_category" = "Aviation" return MishapCategoryLevel2 (should be Manned/Unmanned) 
    #   else if "metric_category" = "Motor Vehicle" return MishapCategoryLevel3
    #   else if "metric_secondary_category" = "Private Motor Vehicle" return MishapCategoryLevel4 (should return PMV
    #           levels)

    # Rename MishapDutyStatusDescription to "duty_type" and filter to just the ON/OFF values (drop "Not Applicable" and
    # "Reserve Component Non-Duty") 

    # Rename MishapFiscalYear to "fiscal_year" - cast as string

    # Create a new column called "fiscal_month" that is the concatenation of MishapFiscalMonthNbr (cast as string) and
    # MonthName, this will be used as the x-axis label in the charts (e.g., 2-November)

    # Rename column CaseNumber to "primary_key"

    # Coalesce for spark efficient processing using calculations in file settings
    # mishaps_df = mishaps_df.coalesce(1)
    
    # Create temporary empty dataset for Code Wireframe
    spark = SparkSession.builder.getOrCreate()
    emptyRDD = spark.sparkContext.emptyRDD()

    # Apply schema to final output dataset, emptyRDD will become name of final dataset once code implemented
    output_df = spark.createDataFrame(emptyRDD, FINAL_SCHEMA)
    
    # Write out final dataset, applying column descriptions from schema.py
    out.write_dataframe(ouput_df, column_descriptions=MISHAPS_COL_DESCRIPTIONS)

```


### mishaps_params.py
```
# Set up directory information
INPUT_DIR = "/Army_NIPR/Business Health Metrics/data/People/Safety & Security/Inputs/"
OUTPUT_DIR = "/Army_NIPR/Business Health Metrics/data/People/Safety & Security/Datasets/"
```


### mishaps_schema.py
```
import pyspark.sql.types as T

MISHAPS_SCHEMA = T.StructType([
    T.StructField("primary_key", T.StringType(), False),
    T.StructField("fatality_count", T.IntegerType(), False),
    T.StructField("metric_category", T.StringType(), True),
    T.StructField("metric_secondary_category", T.StringType(), True),
    T.StructField("duty_type", T.StringType(), True),
    T.StructField("compo", T.StringType(), False),
    T.StructField("fiscal_year", T.StringType(), False),
    T.StructField("fiscal_month", T.StringType(), False),
    T.StructField("case_month_end", T.DateType(), False),
    T.StructField("date_rank", T.IntegerType(), False),
])

MISHAPS_COL_DESCRIPTIONS = {
    "primary_key": "Unique identifier",
    "fatality_count": "Total number of fatalities associated with case",
    "metric_category": "Top-level classification of case (e.g., Ground, Aviation, Motor Vehicle)",
    "metric_secondary_category": "Secondary level of classification of case (e.g., type of vehicle, manned/unmanned).",
    "duty_type": "Whether case occurred while on or off duty. Will always be 'On Duty' for Aviation",
    "compo": "Active, Reserve or National Guard",
    "fiscal_year": "Four-digit string indicating fiscal year in which case occurred",
    "fiscal_month": "Two-digit fiscal month including month name in which case occurred",
    "case_month_end": "Calendar month ending date in which case occurred for use in displaying data in Workshop",
    "date_rank": "Sequential order of cases grouped by case_month_end, with 1 for the most recent month, counting up through all previous months in the dataset",
}
```
