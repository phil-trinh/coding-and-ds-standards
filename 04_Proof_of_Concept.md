# 4 - Prototyping
## Expected Outputs from this Stage
After capturing all the requirements and business logic of the changes we would like to implement, it would be
 beneficial to have a quick proof of concept to confirm the validity of said requirements. This is to ensure that before
 putting major investment into development, we have an idea of the actual results to verify with in order to reduce risk
 of having to go back and rewrite the requirements or logic. Proof of concept products include, but are not limited to:
- [ ] resulting datasets,
- [ ] low-resolution visualizations that are close to the desired outcomes, and/or
- [ ] column statistics to spot check expected outputs


## Helpful Links
- [Adobe Prototyping 101](https://blog.adobe.com/en/publish/2017/11/29/prototyping-difference-low-fidelity-high-fidelity-prototypes-use)
- [Webflow Low vs. High Fidelity Prototypes](https://webflow.com/blog/low-vs-high-fidelity)


## Rationale for Prototyping
This concept of prototyping is borrowed from the UX/UI (User Experience/Interface) design world where building up a
 digital product involves having iterations of something **tangible** in order to "feel" and validate the user
 experience as the product develops. If a product were to be built all the way through without knowing how the end
 result will ultimately look, function, and be accepted by users, more development time has to be invested in order to
 pivot or sometimes even pivot away from the original design.
 
To mitigate this risk to development teams, prototypes are built throughout the development process to solicit feedback
 from stakeholders that drive adjustments to requirements, if needed. This ongoing, iterative process increases the
 likelihood of a successful outcome (i.e. users accepting the final product).
 
In practice for data science development, we can adopt this approach by taking our requirements, data inputs, and
 business logic to construct a low fidelity prototype. With low vs. high fidelity prototypes, there can be use cases
 to do one or the other:

### Low Fidelity
- Faster time to prototype
- Lower barrier to create
- Best for early stages of development
- Quick test of business logic
- Suitable for smaller teams

### High Fidelity
- Closer representation of final product
- Best for later stages of development
- Can test user acceptance of the final product
- Larger teams that have more bandwidth


## Approach to Prototyping for Data Science
For Data Science tasks, **low fidelity prototyping** would be more preferable typically because time-to-insight is
 usually prioritized to find value in the data being utilized. Part of the data science life cycle is to do some
 Exploratory Data Analysis (EDA) and some low fidelity prototyping of:
- resulting datasets from translating business logic to code can provide that exploration at the same time.  
![Dataset example](../images/lofi_dataset_example.png "Lofi Dataset Example")

- Building some core statistical charts, data quality checks, and summary metrics is helpful for checking that the right
  data is used and the business logic has the expected outcome.
    - This could be done in Palantir Foundry's Contour tool or a general business intelligence tool, such as Qlik,
      Tableau, or PowerBI. These tools all catalyze chart development through easy-to-use, drag-and-drop interfaces..
    - If none of these are available, quick, rudimentary charts created in Python/R would be sufficient too. Remember that
      these charts are supposed to be fast for quality checking, not necessarily for functionality.
![Contour summary charts example](../images/lofi_summary_example.png "Lofi Summary Example")

- Constructing barebones visualization(s) that would roughly represent a final dashboard, if that is what is needed.
    - This could be quickly created through Palantir Foundry's Contour Dashboard or Reports tool, Tableau, or any powerful
      business intelligence tool.
    - Dashboards should convey general layout and content but functionality and interactivity shouldn't be considered at
      this stage for the sake of expediency.
![Reports Dashboard Example](../images/lofi_dashboard_example.png "Lofi Dashboard Example")
