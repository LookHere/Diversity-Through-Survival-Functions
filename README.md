# Measuring Diversity Analytics Through Survival Functions: 
An attempt to identify structural or unconscious bias using commonly collected people data

The objective of this project is to explore whether commonly collected people data and survival analysis can be used to indicate group differences that may help in identifying instances of structural or unconscious bias.  

Commonly used Diversity, Equity, Inclusion, and Belonging (DEIB) analytics often focus on survey results or on reported demographic headcounts.  Although those are useful, they may overlook some indicators of structural or unconscious bias.  Using the Cox Proportional Hazard model may help identify groups with these problems, though such analysis will not identify the specific bias each group faces.  Identifying these groups can be useful indicators to help HR teams focus their efforts both in identifying issues directly and/or in balancing against the bias.

## Cox Proportional Hazard 
The Cox proportional hazards model is a regression model that works both for quantitative and for categorical variables, which is useful with Human Resources datasets.  Unlike some other survival models, the Cox model assesses simultaneous effects from several risk factors, giving it greater flexibility for workers with intersectional identities, who may fall into multiple demographic categories that are discriminated against (Cox Proportional-Hazards Model, n.d.).

## Model Implementation
The Cox Proportional Hazard model can be implemented in multiple systems.  For simplicity we use the coding language R here since it is a free software platform developed for statistical methods.

After organizing the data and running the needed calculations, we can verify if there is a statistical significance to any of the demographic data on tenure.  One area to look at is concordance, which measures the goodness of fit.  Concordance is 0.5 when the model is as accurate as random chance, and 1.0 when the model is a perfect prediction.  A concordance of over 0.8 indicates a strong model, as long as its confidence interval does not extend below 0.5 in the range.  

A p-test can be used for the model and also for the factors in the model.  The smaller the p-value, the more confidence we can place in the model; generally, under 0.05 is a standard.  The Wald test is useful in identifying the p-value for the model

### Cumulative Event
This graph shows the percentage of terminations (events) expected over time.  The percentage of the original populations who have terminated (from 0% to 100%) increases until all employees have terminated at around 1,000 days.  This chart indicates that females are leaving quicker than other populations.
![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/CumulativeEventSmall.jpg)

### Cumulative Hazard
Instead of looking at the expected number of terminations for the entire population, this graph expresses the same information as the risk someone will have terminated at a certain time.  This may be more useful in explaining the situation to an employee since it is based on their estimated risk of turnover.
![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/CumulativeHazardSmall.jpg)

### Survival Probability % 

Another way to to look at the results is to consider the chance someone will survive over a period of time.  A confidence interval was also added to this view (and could be added to any of the graphs).  We can see a wider confidence interval for the non-binary population; since there were fewer instances of non-binary employees, the model has less confidence in its projection of that population.
![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/SurvivalProbabilitySmall.jpg)


### Validity

A main concern for this type of correlation analysis is false positives.  To test against that, I created the [Look Here Dummy Dataset] (https://github.com/LookHere/Look-Here-Dummy-Data) that procedurally generates employees based on categories (gender, race, division) and their ratios (based on national rates), in addition to hire and termination dates.  Since the dummy data is based on a randomized collection of these categories, there is no bias in it.  If the model finds no bias in the dummy data but finds bias in the live data, it will give us confidence in the vailidity of the model.

This test was run three times for populations of 20, 200, 2,000, and 20,000 headcounts.  

All headcount levels had some categories (ex. marketing, female, white) that passed their own p-test (lower than 0.05).  These (falsely) indicated biase for these categories.

![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/Ptest1.png)

Two of the organizations at the headcount level of 20 had a Wald p-test significantly under 0.05 indicating the model (falsely) could find bias.  All other tests were over 0.05, (rightfully) showing the models found no bias. 

![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/WaldTest1.png)

The three organizations at the headcount level of 20 employees had over a 0.8 concordance (over the dotted line), implying the model fits the data well and (wrongfully) implying they can be used to find bias.  All other categories were around 0.6 or below, (rightfully) implying they should not be used.
 
![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/Concordance1.png)

Considering all of these together, only the first and third organizations with 20 employees passed all tests, (falsely) indicating bias. All organizations from headcount levels 200 to 20,000 all failed the concordance test (rightfully) indicating that their data should not be used to find bias.  Since this fictional data is not influenced by structural or unconscious bias, finding indications at 20 employee headcount level implies that organizations at that size may be too small for this method of diversity analysis.  The next tests look to identify validity for organizations with headcounts between 20 and 200.

The second round of testing also (falsely) implied statistical significance of multiple categories for all headcount levels.

![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/Ptest2.png)

All headcount levels had one or two organizations that had under 0.05 for the Ward p-test, (falsely) implying that they are statistically significant.

![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/WaldTest2.png)

For the organizations with 20 employees, 7 of the 10 had over 0.8 concordance, (falsely) implying they should be used.  No other headcount level had any organization with concordance over 0.8 (correctly) implying they should not be used.   

![](https://raw.githubusercontent.com/LookHere/Diversity-Through-Survival-Functions/images/Concordance2.png)

For the second round of testing, 2 of 10 organizations with 20 employees passed all tests, (falsely) implying there is statistically significant bias.  No other levels had any organizations that identified bias where there was none.  Taking all of these results into consideration, it appears that the Cox Proportional Hazard model is not a good tool for diversity analysis for organizations under 50 employees.  
