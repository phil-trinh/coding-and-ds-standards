# 2 - Target Structure
## Expected Outputs from this Stage
As Part of the Anatomy of the Whole Data Pipeline:
- [ ] Appropriate Project Type
- [ ] Correct Directory Structure
- [ ] Project Documentation


## Anatomy of a Pipeline
Our goal is to create a trusted set of data pipelines that efficiently, accurately, and consistently move data from raw
 tables or source systems into a format that is clean and well organized for analytical consumption. A good set of data
 pipelines allows future analysts to avoid many low-level questions. I should not need to know which source system my
 data is coming from, it should already be connected to other relevant data in tidy tables. I should not have to
 repeat data cleaning for each analysis, I should be able to build future analysis on top of the efforts of previous
 analysts. Finally, I should be able to make changes upstream without needing to update all the analysis built upon
 it.

We can translate this description of our goal into a model for organizing the flow of data within the platform; one that
 optimizes for building a shared data landscape while allowing for separation of concerns, ease of maintenance and
 monitoring, and simplified sharing of data across analysts and teams.

In this context, pipeline is a term widely used in the most general sense to simply refer to the flow of data from a
 source system through to derived datasets via a series of intermediate datasets. In between each “step” of the pipeline
 is a “transformation” that defines how data from the input dataset(s) is manipulated to create the output dataset.

During this discussion, we're going to offer a suggested model for breaking long pipelines into manageable chunks, and
 then we're going to exhibit some management patterns and best practices we have seen succeed across domains.

These stages define the logical separation of Projects that compose a well-ordered pipeline. We'll go into each in more
 detail below.
1.	Data Connections land raw data from source systems into a Datasource Project.
2.	Datasource Project: One Datasource Project per logical datasource defines the basic cleanup steps for each raw
    dataset and applies a consistent schema.
3.	Transform Project: Datasets are imported from one or more Datasource Projects and transformed to produce canonical,
    re-usable datasets.
4.	Business Object Project: Datasets are imported from one or more Transform Projects and transformed to produce the
    canonical tables representing discrete business objects. A single Business Object Project often groups related sets
    of objects for a given business domain for ease of management.
5.	Workflow Project: Workflow projects import data from Business Object projects to pursue a specific outcome.
    Frequently this is an operation workflow, data science investigation, business intelligence analysis and report, or
    application development project.

Based on [Recommended Project Structure](https://vantage.army.mil/workspace/documentation/product/transforms/pipeline-anatomy) by Palantir.

Each pipeline stage is a discrete unit, and the outputs are the datasets made available for downstream projects to
 import and use for other use cases, pipeline development, analysis, and so on. Within each project, the responsible
 team should, in addition to the implementation of the transformation steps, also manage the stability and integrity of
 their outputs. This involves managing the build schedules, configuring and monitoring data health checks, and, where
 relevant writing unit tests or additional data integrity tests.

The sections below will walk through each downstream project type in more detail to follow the flow of data through the
 pipeline. In the process of designing a pipeline, instead work backwards from the Ontology layer to determine the
 necessary source systems to connect and pipeline transformations to implement.


## Projects
Projects provide a way of organizing data and code into manageable chunks in different directories. Although you could
 have all your code in a single directory, breaking it down into multiple modules makes it easier to isolate changes and
 focus on specific parts of the data pipeline. Depending on the size of your team, engagement, and pipeline you may have
 all of these steps in a single repository or divided across multiple repos.

When thinking about what makes a good project, consider as an analogy what makes a good microservice: well-defined
 purpose, clear output datasets/API used by downstream dependencies, ownership by a set of people small enough that they
 can effectively coordinate with each other and set their own standards for project management.

### 1. Data Connection
The Data Connection is the external link from your upstream data provider to your team's environment. Depending on how
 your team collects data this may be a connection to a source system, a call to an API, or it may not exist if data is
 dropped into your environment by an external service.

### 2. Datasource Project
Each source system should land data into a matching Project inside the platform. The normal pattern involves landing
 data from each sync in as 'raw' a format as possible. The transformation step to a “clean” dataset is defined in this
 step of the pipeline.

Establishing this project-per-datasource model has a number of convenient benefits:
-	Encourages exploration of new data sources in a consistent location
-	Provides a single location to specify Access Controls to a data source
-	Minimizes the chance of duplicated effort ingesting the same data by different teams
-	Code Repositories are small & purpose-built to handle the type and format of source system data
-	Allows anonymization, clean up, and standardization of data before it is accessed by the pipeline layer

#### Datasource Project Goals
Inside each data source project, the goal is to prepare the ingested data for consumption by a wide variety of users,
 use-cases, and pipelines across the organization.

The output datasets, and their schema, should be thought of as an API; that is, the goal will be for them to be
 logically organized and stable over time, as the output datasets from a data source project are the backbone of all
 downstream work. The outputs should map 1:1 with the source tables or files; each row in the dataset should have a
 direct matching row in the source system. Consider implementing a unique "source id" for each row in the dataset to 
 more easily trace raw/clean data to processed data in the source system.

To this end, some typical objectives of the raw → clean transformation include:
-	Imposing a consistent column naming scheme
-	Ensuring appropriate types are applied to columns
-	Handling missing, malformed, or mis-typed values
-	Establishing primary Data Health checks
-	Removing PII or other sensitive data unsuitable for general consumption

Even where the source system provide column data type information, it is sometimes necessary to bring in values as a
 STRING type. Pay special attention to DATE, DATETIME, and TIMESTAMP types, which are often represented in source
 systems in non-standard formats and numeric types, which are occasionally troublesome. If these types do prove to be
 unreliable in format from the source system, importing as a STRING type provides the option to implement more robust
 parsing and define logic for handling malformed values or dropping unusable or duplicated rows.

In addition to these programmatic steps, clean datasets should be rigorously documented. A qualitative description of
 the dataset, the source system provenance, the appropriate contacts and management protocol and, where relevant,
 per-column metadata describing features of the data will all ensure future developers use the data appropriately.

While every datasource will be unique, the steps of cleaning and preparing a source system often have shared steps, such
 as parsing raw string values to a given type and handling errors. When data sources have a number of similar cleanup
 transforms, it's best practice to define a library in Python to provide a set of consistent tooling and reduce
 duplicated code.

Recommended Folder Structure
- /data
  - /processed - (optional) file-level transformations to create tabular data from non-tabular files
  - /raw - datasets from a Data Connection sync should land here
  -	/clean - datasets here are 1:1 with datasets in /raw with cleaning steps applied
- /analysis - resources created to test or document the cleanup transforms and show the shape of the data
- /scratchpad - any temporary resources created in the process of building or testing the cleanup transforms
- /documentation - the Data Lineage graphs that show the pipeline steps for cleanup as well as additional
  documentation (beyond the top-level Project README).

### 3. Transform Project
The goal of Transform Projects is to encapsulate a shared, semantically meaningful grouping of data and produce
 canonical views to feed into the Business Objects layer.

These projects import the cleaned datasets from one or more Datasource Projects, join them with lookup datasets to
 expand values, normalize or de-normalize relationships to create object-centric or time-centric datasets, or aggregate
 data to create standard, shared metrics.

Recommended Folder Structure
- /data
  - /transformed - (optional) these datasets are output from intermediate steps in the transform project
  - /output - these datasets are the "output" of the transform project and are the only datasets that should be
	relied on in downstream Ontology projects
- /analysis - resources created to test or document the cleanup transforms and show the shape of the data
- /scratchpad - any temporary resources created in the process of building or testing the cleanup transforms
- /documentation - the Data Lineage graphs that show the pipeline steps for cleanup as well as additional
  documentation (beyond the top-level Project README).

### 4. Business Objects Project
The Business Object enforces a shared communication layer between raw data coming into your pipeline and the variety of
 analytical tasks for which the data is consumed. The curation of clean, organized, and stable Business Objects is the
 highest order objective to ensure a wide-range of valuable projects can move forward simultaneously while enriching the
 common operating picture.

A Business Objects project is the center-point of any pipeline and represents the final transformation necessary to
 produce datasets that conform to the definition of a single or related-group of objects.

Since the business objects datasets represent the canonical truth about the business object they represent, they form
 the starting point for all “consuming” projects. While the provenance and transformation logic of upstream cleaning and
 pipeline steps are visible, conceptually these steps and intermediate datasets could be a black box for the project
 developers, analysts, data scientists, and operational users who consume data only from the Ontology projects. In this
 sense, the Business Objects layer serves as an API for business objects.

Similar to Datasource projects, essential to maintaining a Business Objects project are:
- Robust Documentation
- Meaningful Health Checks
- Regular Schedules
- Curation in a Data Catalog with appropriate Tags.

In addition, as your Business Objects grow more robust and more teams contribute to the Business Objects layer,
 maintaining the integrity of the dataset "APIs" becomes more critical. To this end, consider implementing additional
 checks to ensure that proposed changes preserve the integrity of the output dataset schema.

Recommended Folder Structure
- /data
  - /transformed - (optional) these datasets are output from intermediate steps in the Business Objects project
  - /business_objects - these datasets are the "output" of the Business Objects project and are the only datasets
    that should be relied on in downstream use-case or workflow projects
- /analysis - resources created to test or document the cleanup transforms and show the shape of the data
- /scratchpad - any temporary resources created in the process of building or testing the cleanup transforms
- /documentation - the Monocle graphs that show the pipeline steps for cleanup as well as additional documentation
  (beyond the top-level Project README).

### 5. Workflow Project
Workflow Projects, also known as Use Case Projects, should be flexibly designed for the context at hand, but usually are
 built around a single project or a team to an effective unit of collaboration and delineate the boundaries of
 responsibility and access.

In general, workflow projects should reference data from the Business Objects Projects, to ensure that operational
 workflows, business intelligence analysis, and application projects all share a common view of the world. If, in the
 course of developing a workflow project, the data sources available in the Business Objects layer aren't sufficient as
 sources, it's an indication that the Business Objects should be enriched and expanded. Avoid referencing data from
 earlier (or later) in the pipeline as this can fragment the source of truth for a particular type of data.

Recommended Folder Structure

The structure of Workflow projects will be more varied than other project types and should focus on making the primary
 resource(s) immediately accessible and well-documented.
- /data
  - /transformed - (optional) these datasets are output from intermediate steps in the workflow project
  - /analysis - (optional) these datasets are output from analysis workflows
  - /model_output - (optional) these datasets are outputs from model workflows and can then be analyzed to determine
	model fit
  - /user_data - (optional) if your workflow enables users to create their own "slice" of data, store them here in
    user-specific folders
- /analysis - resources and reports created to drive decisions and feedback loops
- /models - any models created should be stored here
  - /templates - any Code Workbooks templates shared for project-specific usage should be stored
- /applications - additional workflow applications or sub-applications are stored here. A primary application should be
  stored at the Project root for prominent access.
  - /develop - applications in development, new features, and templates are stored here
- /scratchpad - any temporary resources created in the process of building or testing the cleanup transforms
- /documentation - the data lineage graphs that show the pipeline steps for cleanup as well as additional documentation
  (beyond the top-level Project README).


## Project Documentation
Each project should be documented thoroughly throughout the development process. Here are a few common patterns and best
 practices:
- Save one or more data lineage graphs at the root of each project to capture the most important datasets and pipeline
  relationships. Different graphs can use different color schemes to highlight relevant features, such as the regularity
  of data refresh, the grouping of related datasets, or the dataset characteristics, such as size.
- Add a short description to the project to the readme.md. Focus on putting the project into context for users who may
  be unfamiliar with it.
- Place further global project documentation in Markdown (.md) files created locally and uploaded to a Documentation
  folder.
