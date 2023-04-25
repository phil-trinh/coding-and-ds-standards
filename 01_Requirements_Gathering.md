# 1 - Requirements Gathering
## Expected Outputs from this Stage
- [ ] Problem Statement
- [ ] Wireframe
- [ ] Data Ingest Plan
- [ ] Contingencies when Data Changes


## Problem Statement
A problem statement should be a quick, short, and simple explanation of what the project hopes to accomplish and why it
 is important. A good problem statement would be written in a few sentences and vocalized in a minute. Functionally, it
 serves the purpose for easy context to others in informal spaces of information sharing, such as elevator pitches,
 emails, or readme files. Problem statements are one way for team members to keep aligned with each other and
 consistent towards the project.


## Wireframe
A wireframe is a set of diagrams that visualize the overall skeleton and shows core functionality of the tool or product
 that your team wants to make. Typically, wireframes are designed in Adobe XD, PowerPoint, or paper; the goal is that a
 wireframe would serve as a revisable visual for the client, for them to understand the product you want to make and
 critique it. As such, wireframes should be shown to a variety of clients, for whom the product is designed, so that
 common patterns of criticisms and comments may be gathered, for their signing and approval.


## Data Ingest Plan
It is important to plan out how you would like to go about obtaining, mapping, storing, refreshing, and replacing data.
 Included in this planning step is also consideration of the general data pipeline architecture for the project.
 The Data Ingest Plan should be written out into the project readme or a separate readme if the project is more
 complex.

### README
A readme is a Markdown document (.md) that records project context which is learned throughout the project development
 lifespan that code documentation otherwise would not. Examples of context include: steps that got you to where you are,
 failed ideas, problems encountered, as well as things learned. A readme typically has these sections included: problem
 statement, questions explored, pseudocode, business logic, next steps, and data dictionary. From the [LMI DAS Handbook,
 "Organizing a Project Readme"](https://code.lmi.org/confluence/display/DASHandbook/Organizing+a+Project+Readme)
	
### Pipeline Architecture
Put simply, a production pipeline is a level of abstraction for a development lifecycle to follow in order to maximize a
 team's efficiency, keep everyone engaged, make it easier to find problems and maintain the project.

When thinking about and planning an appropriate Production Pipeline Architecture, it is important to plan around three
 areas: scope, build schedule, and data periodicity.

Some examples of topics to tackle and discuss with your team include: where the pipeline starts and ends, how it
 interacts with other pipelines, and answering what is going to be your pipeline refresh rate. How often should data
 refresh, specific times, and including or excluding weekends? Also consider when data should be counted as out of
 date. Make sure that your expectations are compatible as well. For example, if you decide you want data to refresh
 hourly, but the data source only provides updates every half a day, it will not be possible to refresh data hourly.
 Be careful when planning refresh rates because an unclear refresh rate definition makes it hard for a pipeline
 maintainer to prioritize work or to fix problems down the line. 

In a "Pipeline Architecture" section of your `readme.md` you should answer the following questions:
- What exactly is the scope of the pipeline?
  - Where does it start?
  - Where does it end?
  - Where should it feed into other pipelines?
    - If parts of your pipeline overlap with another pipeline or use case, consider treating the overlapping section as
    its own pipeline.


- What is the requirement on pipeline refresh rate?
  - Is a specific time of day when data needs to be refreshed by?
  - Should the pipeline run over weekends?
  - When is data considered critically out of date?
  - What will the expectation in terms of refresh rate and support?
    - Be careful — while this sounds easy, this area is where pipeline maintenance teams often face the most difficulty.
    Without clear definitions here, it is difficult to prioritize work as a pipeline maintainer or make the right fix.


- What is the expectation for end-to-end propagation delay?
  - In other words, how long should it take for data to flow through the pipeline from the moment it lands in your data
  lake, to the point where the outputs of the pipeline are updated?


- Who’s your contact point on each and every external source that you pull from?
  - External sources can be data ingestion syncs or a separate upstream pipeline that feeds into your pipeline.


- What are the functional guarantees for correctness in your data?
  - Are there critical columns or key validations that must be true for your pipeline?
  - What should happen if a guarantee is broken?
    - Should a failing validation prevent data from reaching your end user or should it fire an alert without preventing
    your pipeline from updating?
      - You may want to do this to allow other up-to-date data to flow through your pipeline.
      - Documentation is a good place to start tracking guarantees early on.


- Are the expectations determined compatible?
  - As complex system grow, they become more prone to failures. You generally want to allow sufficient time after an
  alert is fired to address the underlying issue - whether that is an unexpected pipeline failure or missing data from
  an upstream source.
    - Example 1: It’s important to think about the propagation delay in relation to the expected pipeline refresh rate.
    If the expected refresh rate is every 2 hours, and the pipeline takes 1.5 hours to build, it may become difficult
    to adhere to SLAs.
    - Example 2: if a workflow requires an hourly refresh rate, but the upstream data source only provides data twice a
    day. This means that you will not be able to achieve hourly refreshes.


- What will happen to this pipeline after the end of this project? Who will take it over?

Based on the [Building a Production Pipeline](https://www.palantir.com/docs/foundry/building-pipelines/building-production-pipeline/)
 guide by Palantir.


## Contingencies when Data Changes
Try to preemptively prepare for unpredictable changes regarding the data you will be accessing. What happens if the
 source makes its data private or removes the data entirely? Is there another source from which to feed the pipeline?
 How will your project's pipeline hold up? Try to come up with a backup plan or a workaround, maybe by building your
 pipeline to be adaptable. By at least thinking about potential issues like this and bringing it up with your team
 early, it may cause a future surprise to be mitigated.
