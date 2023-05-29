# **Calculated Field in Tableau for Exploratory Data Visualization**

## **a. Average Number of Customers per City**
COUNTD([Customer Id])/COUNTD([City])

## **b. Average Order Value (AOV)**
SUM([total_payment_value])/COUNTD([Order Id])

## **c. Level of Detail (LOD) of Unique Item**
{FIXED ([Customer Id]): COUNTD([Product Id])/COUNTD([Customer Id])}

## **d. GMV**
[Price]*[Qty Item]

## **e. RFM Analysis**
1. Calculate R, F, M measure <br>
RFM-R <br>
DATEDIFF('day',{ FIXED [Customer ID]:MAX([Invoice Date])},{MAX([Invoice Date])}) <br>
RFM-F <br>
({ FIXED [Customer ID]:COUNTD([Invoice ID])}) <br>
RFM-M <br>
{FIXED [Customer ID]: SUM([Sales Amount])} <br>

2. Calculate RFM score <br>
- R-Score <br>
IF [RFM-R] <= {FIXED:PERCENTILE([RFM-R],0.25)} <br>
THEN 1 <br>
ELSEIF [RFM-R] > {FIXED:PERCENTILE([RFM-R],0.25)} AND [RFM-R] <= {FIXED:PERCENTILE([RFM-R],0.5)} <br>
THEN 2 <br>
ELSEIF [RFM-R] > {FIXED:PERCENTILE([RFM-R],0.5)} AND [RFM-R] <= {FIXED:PERCENTILE([RFM-R],0.75)} <br>
THEN 3 <br>
ELSE 4 <br>
END <br>

- F-Score <br>
IF [RFM-F] <= {FIXED:PERCENTILE([RFM-F],0.25)} <br>
THEN 4 <br>
ELSEIF [RFM-F] > {FIXED:PERCENTILE([RFM-F],0.25)} AND [RFM-F] <= {FIXED:PERCENTILE([RFM-F],0.5)} <br>
THEN 3 <br>
ELSEIF [RFM-F] > {FIXED:PERCENTILE([RFM-F],0.5)} AND [RFM-F] <= {FIXED:PERCENTILE([RFM-F],0.75)} <br>
THEN 2 <br>
ELSE 1 <br>
END <br>

- M-Score <br>
IF [RFM-M] <= {FIXED:PERCENTILE([RFM-M],0.25)} <br>
THEN 4 <br>
ELSEIF [RFM-M] > {FIXED:PERCENTILE([RFM-M],0.25)} AND [RFM-M] <= {FIXED:PERCENTILE([RFM-M],0.5)} <br>
THEN 3 <br>
ELSEIF [RFM-M] > {FIXED:PERCENTILE([RFM-M],0.5)} AND [RFM-M] <= {FIXED:PERCENTILE([RFM-M],0.75)} <br>
THEN 2 <br>
ELSE 1 <br>
END <br>

- RFM-Score <br>
INT(STR([R-Score])+STR([F-Score])+STR([M-Score])) <br>

3. RFM Segment <br>
IF [RFM-Score] == 111 <br>
THEN 'Best Customers' <br>
ELSEIF [R-Score] == 1 AND [F-Score] <= 2 AND [M-Score] <=2 <br>
THEN 'Potential To Become Best Customer' <br>
ELSEIF [F-Score] == 1 <br>
THEN 'Loyal Customer' <br>
ELSEIF [M-Score] == 1 <br
THEN 'Big Spenders' <br>
ELSEIF [RFM-Score] == 311 <br>
THEN 'Almost Lost' <br>
ELSEIF [RFM-Score] == 411 <br>
THEN 'Lost Customers' <br>
ELSEIF [RFM-Score] == 444 <br>
THEN 'Lost Cheap Customers' <br>
ELSEIF [R-Score] = 2 <br>
THEN 'Look Out Buyers' <br>
ELSEIF [R-Score] >= 2 AND [F-Score] >= 2 <br>
THEN 'Occasional Buyers' <br>
ELSEIF [R-Score] == 1 AND [F-Score] >= 2 AND [M-Score] >=2 <br>
THEN 'New Customers Potential to Lost' <br>
END

## **f. Cohort Analysis**
1. Calculated field of first order date <br>
{FIXED([Customer Id]):MIN([Order Purchase Date])} <br>

2. Calculated field of difference month after first purchase time <br>
DATEDIFF('month',[first_order_date],[Order Purchase Date]) <br>

3. Drag first order date to rows > change to month 

4. Drag count distinct customer_id to text and color

5. Click CNTD(customer_id) in marks text > quick table calculation > percent of total
