# airline
Customer satisfaction scores from 120,000+ airline passengers, including additional information about each passenger, 
their flight, and type of travel, as well as ther evaluation of different factors like cleanliness, comfort, service,
and overall experience.

## Table of Contents
* [Introduction](#Introduction)
* [Dashboard Requirements](#Dashboard-Requirements)
* [Recommended Analysis]
* [Installation / Usage](#Installation--Usage)
* [DAX Formulas Used in Measures](#DAX-Formulas-Used-in-Measures)


* [Introduction](#Introduction)
  As their data analyst, I have been tasked with these questions dashboard for the stakeholders and Executive Team.
This dashboard would give the airline 's Executives insights into the company's sales performance.

* [Recommended Analysis]
  - Which percentage of airline passengers are satisfied? Does it vary by customer type? What about type of travel?

  - What is the customer profile for a repeating airline passenger?

  - Does flight distance affect customer preferences or flight patterns?

  - Which factors contribute to customer satisfaction the most? What about dissatisfaction?
 
*  The dataset can be accessed from this link:https://www.mavenanalytics.io/data-playground?page=2

 ## Installation / Usage
* Install Power BI Desktop from Official [Power BI Download Site](https://powerbi.microsoft.com/en-us/downloads/)
* Download data files from link given in Introduction
* Clone/download this repository to your local machine
* Open Dashboard report file (analyize.pbix) in Power BI Desktop, to access the dashboard's interactivity 

  ## DAX Formulas Used in Measures
  ** AvgAge = AVERAGE(airline[Age])

    
  ** AvgSatisfaction = AVERAGE(airline[Int])

    
  ** Distance Group = 
    SWITCH(
     TRUE(),
   'airline'[Flight Distance] <= 500, "0-500",
   'airline'[Flight Distance] <= 1000, "501-1000",
   'airline'[Flight Distance] <= 1500, "1001-1500",
   'airline'[Flight Distance] <= 2000, "1501-2000",
   'airline'[Flight Distance] <= 2500, "2001-2500",
   'airline'[Flight Distance] <= 3000, "2501-3000",
   'airline'[Flight Distance] <= 3500, "3001-3500",
   'airline'[Flight Distance] > 3500, "3501+"
) .


  
** Food and Drink and Int correlation for Type of Travel = 
VAR __CORRELATION_TABLE = VALUES('airline'[Type of Travel])
VAR __COUNT =
	COUNTX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Food and Drink]) * SUM('airline'[Int]))
	)
VAR __SUM_X =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Food and Drink]))
	)
VAR __SUM_Y = SUMX(KEEPFILTERS(__CORRELATION_TABLE), CALCULATE(SUM('airline'[Int])))
VAR __SUM_XY =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Food and Drink]) * SUM('airline'[Int]) * 1.)
	)
VAR __SUM_X2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Food and Drink]) ^ 2)
	)
VAR __SUM_Y2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Int]) ^ 2)
	)
RETURN
	DIVIDE(
		__COUNT * __SUM_XY - __SUM_X * __SUM_Y * 1.,
		SQRT(
			(__COUNT * __SUM_X2 - __SUM_X ^ 2)
				* (__COUNT * __SUM_Y2 - __SUM_Y ^ 2)
		)
	).
 
  
  
  ** In-flight Service and Int correlation for Type of Travel = 
VAR __CORRELATION_TABLE = VALUES('airline'[Type of Travel])
VAR __COUNT =
	COUNTX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[In-flight Service]) * SUM('airline'[Int]))
	)
VAR __SUM_X =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[In-flight Service]))
	)
VAR __SUM_Y = SUMX(KEEPFILTERS(__CORRELATION_TABLE), CALCULATE(SUM('airline'[Int])))
VAR __SUM_XY =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[In-flight Service]) * SUM('airline'[Int]) * 1.)
	)
VAR __SUM_X2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[In-flight Service]) ^ 2)
	)
VAR __SUM_Y2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Int]) ^ 2)
	)
RETURN
	DIVIDE(
		__COUNT * __SUM_XY - __SUM_X * __SUM_Y * 1.,
		SQRT(
			(__COUNT * __SUM_X2 - __SUM_X ^ 2)
				* (__COUNT * __SUM_Y2 - __SUM_Y ^ 2)
		)




** In-flight Wifi Service and Int correlation for Type of Travel = 
VAR __CORRELATION_TABLE = VALUES('airline'[Type of Travel])
VAR __COUNT =
	COUNTX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[In-flight Wifi Service]) * SUM('airline'[Int]))
	)
VAR __SUM_X =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[In-flight Wifi Service]))
	)
VAR __SUM_Y = SUMX(KEEPFILTERS(__CORRELATION_TABLE), CALCULATE(SUM('airline'[Int])))
VAR __SUM_XY =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(
			SUM('airline'[In-flight Wifi Service])
				* SUM('airline'[Int]) * 1.
		)
	)
VAR __SUM_X2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[In-flight Wifi Service]) ^ 2)
	)
VAR __SUM_Y2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Int]) ^ 2)
	)
RETURN
	DIVIDE(
		__COUNT * __SUM_XY - __SUM_X * __SUM_Y * 1.,
		SQRT(
			(__COUNT * __SUM_X2 - __SUM_X ^ 2)
				* (__COUNT * __SUM_Y2 - __SUM_Y ^ 2)
		)
	).
 
  
  
  
  ** Int = SWITCH(airline[Satisfaction],"satisfied",5,
    "Netural or dissatisfied",1,
    0).

  
  
  
  ** SatisfactionPassanger = DIVIDE(CALCULATE(COUNTROWS(airline),airline[Satisfaction] ="satisfied"),COUNTROWS(airline)) * 100
 
  
  
  ** Seat Comfort and Int correlation for Type of Travel = 
VAR __CORRELATION_TABLE = VALUES('airline'[Type of Travel])
VAR __COUNT =
	COUNTX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Seat Comfort]) * SUM('airline'[Int]))
	)
VAR __SUM_X =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Seat Comfort]))
	)
VAR __SUM_Y = SUMX(KEEPFILTERS(__CORRELATION_TABLE), CALCULATE(SUM('airline'[Int])))
VAR __SUM_XY =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Seat Comfort]) * SUM('airline'[Int]) * 1.)
	)
VAR __SUM_X2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Seat Comfort]) ^ 2)
	)
VAR __SUM_Y2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('airline'[Int]) ^ 2)
	)
RETURN
	DIVIDE(
		__COUNT * __SUM_XY - __SUM_X * __SUM_Y * 1.,
		SQRT(
			(__COUNT * __SUM_X2 - __SUM_X ^ 2)
				* (__COUNT * __SUM_Y2 - __SUM_Y ^ 2)
   ).


** Segments Customers = SWITCH(TRUE(),
airline[Age]  in {1,2,3,4,5,6,7,8,9,10} , "Group kids",
airline[Age] in {11,12,13,14,15} , "Group TEENG",
airline[Age] IN {16,17,18,19,20,21,22,23,24,25,26,27,28,29} , "Group Adults",
airline[Age] in {30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50},"Group Middle-aged",
"old").


** TotalPassanger = COUNTROWS(airline)
