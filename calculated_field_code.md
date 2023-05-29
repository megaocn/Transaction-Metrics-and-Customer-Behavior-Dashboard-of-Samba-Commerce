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
1. Calculate R, F, M measure
RFM-R
DATEDIFF('day',{ FIXED [Customer ID]:MAX([Invoice Date])},{MAX([Invoice Date])})
RFM-F
({ FIXED [Customer ID]:COUNTD([Invoice ID])})
RFM-M
{FIXED [Customer ID]: SUM([Sales Amount])}

2. Calculate RFM score
- R-Score
IF [RFM-R] <= {FIXED:PERCENTILE([RFM-R],0.25)} 
THEN 1
ELSEIF [RFM-R] > {FIXED:PERCENTILE([RFM-R],0.25)} AND [RFM-R] <= {FIXED:PERCENTILE([RFM-R],0.5)}
THEN 2
ELSEIF [RFM-R] > {FIXED:PERCENTILE([RFM-R],0.5)} AND [RFM-R] <= {FIXED:PERCENTILE([RFM-R],0.75)}
THEN 3
ELSE 4
END

- F-Score
IF [RFM-F] <= {FIXED:PERCENTILE([RFM-F],0.25)} 
THEN 4
ELSEIF [RFM-F] > {FIXED:PERCENTILE([RFM-F],0.25)} AND [RFM-F] <= {FIXED:PERCENTILE([RFM-F],0.5)}
THEN 3
ELSEIF [RFM-F] > {FIXED:PERCENTILE([RFM-F],0.5)} AND [RFM-F] <= {FIXED:PERCENTILE([RFM-F],0.75)}
THEN 2
ELSE 1
END

- M-Score
IF [RFM-M] <= {FIXED:PERCENTILE([RFM-M],0.25)} 
THEN 4
ELSEIF [RFM-M] > {FIXED:PERCENTILE([RFM-M],0.25)} AND [RFM-M] <= {FIXED:PERCENTILE([RFM-M],0.5)}
THEN 3
ELSEIF [RFM-M] > {FIXED:PERCENTILE([RFM-M],0.5)} AND [RFM-M] <= {FIXED:PERCENTILE([RFM-M],0.75)}
THEN 2
ELSE 1
END

- RFM-Score
INT(STR([R-Score])+STR([F-Score])+STR([M-Score]))

3. RFM Segment
IF [RFM-Score] == 111
THEN 'Best Customers'

ELSEIF [R-Score] == 1 AND [F-Score] <= 2 AND [M-Score] <=2
THEN 'Potential To Become Best Customer'

ELSEIF [F-Score] == 1
THEN 'Loyal Customer'

ELSEIF [M-Score] == 1
THEN 'Big Spenders'

ELSEIF [RFM-Score] == 311
THEN 'Almost Lost'

ELSEIF [RFM-Score] == 411
THEN 'Lost Customers'

ELSEIF [RFM-Score] == 444
THEN 'Lost Cheap Customers'

ELSEIF [R-Score] = 2
THEN 'Look Out Buyers'

ELSEIF [R-Score] >= 2 AND [F-Score] >= 2
THEN 'Occasional Buyers'

ELSEIF [R-Score] == 1 AND [F-Score] >= 2 AND [M-Score] >=2
THEN 'New Customers Potential to Lost'

END


## **f. Cohort Analysis**
1. Calculated field of first order date
{FIXED([Customer Id]):MIN([Order Purchase Date])}

2. Calculated field of difference month after first purchase time
DATEDIFF('month',[first_order_date],[Order Purchase Date])

3. Drag first order date to rows > change to month

4. Drag count distinct customer_id to text and color

5. Click CNTD(customer_id) in marks text > quick table calculation > percent of total
