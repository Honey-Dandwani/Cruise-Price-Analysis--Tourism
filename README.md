# Pricing Analysis and Revenue Management Case Study

## Case Study Introduction

Copa Cruises is a 40-year old company that offers dining and sightseeing 
cruises. What started with only one ship at Maryland’s Eastern Shore, is now a 
big business operating at multiple locations with 40 vessels.

Copa operates scheduled tours at least twice daily during the peak 
travel/tourism seasons at each of its locations. The total number of scheduled 
tours varies based on the number of vessels available at each location. 

Copa also offers its ships exclusively for groups. In fact, a significant portion of 
Copa’s revenues come from group bookings. Typically, group customers book 
for corporate events, weddings, or private celebrations (e.g. family reunion). 
Copa customizes the table arrangement, the deck, and the menu to suit its 
customers’ needs for a group event. An event coordinator helps customers with 
the layout, food and dining options, and other special needs for their utmost 
satisfaction. The event coordinators at each of the locations oversee 
reservations and logistics of special events. 

Currently, event coordinators have full responsibility in pricing the cruises for 
groups. Once a customer makes an inquiry, an event coordinator first checks 
the availability by date. If the group can be accommodated given the capacity of 
a ship and there is availability for the desired date, further information about 
the event is gathered. A service contract that specifies the date, time, route 
and length of the cruise, the food and beverage selection, and additional service 
needs, is prepared. The event coordinator specifies a price per person to cover 
all the expenses of a tour in this contract. Information on each contract is 
recorded in Copa’s database.

Event coordinators charge different prices to different customers. However, 
price differentiation is done in an ad hoc manner. Each coordinator relies on 
his/her expertise and knowledge to quote a price to a customer. Many event 
coordinators think corporate groups are less sensitive to prices compared to 
private groups. Others think there are geographic differences: A private event is 
believed to be more likely to accept a contract with a higher price in New York 
City compared to other locations. In summary, there seems to be systematic 
biases at pricing group events. 

Copa is interested in providing guidance to its event coordinators to make 
better pricing decisions. Contract data that has been collected for a 12-month 
duration will be used for this purpose. Table-1 below shows a snapshot of data. 
Each transaction has a unique Customer-ID. The event takes place at a 
specific location, denoted by a number (e.g. location 1 is Boston). Information 
on how far in advance the booking was made, the type of event (wedding, 
private or business), the price (per person) quoted to the customer, and 
whether the customer booked the cruise at that price (recorded in the “Win” 
column; a 1 indicates customer booked the cruise, 0 indicates otherwise) are 
all recorded in the data file. 

| Customer | ID Location | Booking date (# of days before the event) | Type | Price quote($ per person) | Win |
| ------ | ------ | ------ | ------ | ------ | ------ |
|2007-1 | 8 | 47 | Wedding | $179.00 | 0 | 
|2007-2 | 10 | 195 | Private | $118.00 | 1 | 
|2007-3 | 6 | 84 | Private | $180.00 | 0 | 
|2007-5 | 1 | 177 | Wedding | $157.00 | 1 |

<p align="center">Table-1: Snapshot of collected data.</p>

The list of locations is also available in the data file. 
All the events recorded in the data set were for groups of size 50. The groups 
were homogeneous in their needs such as the length of the tour and the type of 
service provided on board. In other words, the cost of the cruise did not vary 
across customers included in this data set.


Dataset Link - https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Copa-Data-File-1.xls



## Problem Statement 

### Assume that Copa charges a single price per person for all events.
#### (a) What is the price (per person) that maximizes expected revenue per event for Copa? What is the expected revenue per transaction for that price?

#### Methodology

Assumption: Copa charges a single price per person for all events.

From the structure of the provided data, I observed that it had a similar format to that of a bid-response model, i.e., in addition to the information about the contracts, it contained a Quote column, and a 0-1 Win column.
To estimate an expected revenue maximizing price, I first calculated the Bid Response Function, or the Probability of booking the customer at quoted price p:

``` W(p) = 1 / [ 1 + EXP(a+bp)] ```

Since I am looking for a single price per person in this analysis, I focused only on the Quote and Win columns. I fit the logit model with the given data and calculated the Probability(Win) and Probability(Loss) to calculate the Maximum Likelihood estimation of winning. Then I assumed an arbitrary starting value of decision variables a and b, and created and solved the following Solver model to maximize ln(likelihood) with constraints: **a <= 0**; **b >= 0**.

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%201.png?raw=true)

I then used the obtained values of **a = -4.24**, **b = 0.033**, and **W(p)** to maximize our objective:

```Expected Revenue of a bid at Price p = (Contract Revenue at p conditional on winning) x W(p)```

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%202.png?raw=true)

I got the optimal single price **p* = $101.99** per person, and the **Exp. Revenue per transaction = $3589.12**.

#### (b) Assuming the annual number of inquiries remains the same, what would be the total expected revenue for the company if the price you determined in part (a) is used? How does the annual expected revenue compare to the total revenue earned by Copa in 2007?

#### Methodology

In the methodology explained in (a), I determined the optimal price per person. Then I expanded on the model:

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%203.png?raw=true)

The calculated total expected revenue was **$25,680,126.80** compared to revenue of **$16,927,650** in 2007.
This shows a difference of **$8,752,476.80** this year compared to the revenue earned by Copa in 2007.

### Analyze the data to determine whether there are multiple price segments..
#### (a) What is your segmentation criterion? Explain clearly.

#### Methodology

    1. Segmentation by Location - Every location in which Copa Cruises operates may have different dynamics and competitors, which may affect the customers’ willingness to pay, and influence optimal price.
    2. Segmentation by Event Type - Customers in different event type segments may have varying price sensitivity, expectations & needs.
    3. Segmentation by Location & Event Type - To analyze the synergy between customers in different geographic locations & their required event types.

#### (b) What is the revenue maximizing price for each segment? What is the expected revenue for each transaction in that segment? (You have to repeat the analysis in part (1) for each of the segments.)

#### Methodology

For each segmentation we calculated a and b by maximizing ln(likelihood). The solver models looked like:

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%204.png?raw=true)

Using the calculated value of W(p), we solved for price and expected revenue values for each of the segments.

Total maximum expected revenue for segmentation by location was **$25,676,045.85**

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%205.png?raw=true)

Total maximum expected revenue for segmentation by event type was **$25,697,511.83**

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%206.png?raw=true)

Similarly for segmentation by event type at a specific location, we created 30 different products.

This segmentation gave the maximum expected revenue of **$25,700,333.82**.

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%207.png?raw=true)

To make prices consumer-friendly, we rounded the price to the nearest integers. This gave us an expected revenue of **$25,700,005.69**

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%208.png?raw=true)

#### (c) What is the annual expected revenue for Copa for each segment? (Considering the transactions corresponding to each of your segments in the data file, you can determine how much revenue Copa would earn in each segment.) How does the expected annual revenue of Copa compare to total revenues in 2007 if optimal price for each segment was used.

#### Methodology

Since we had maximum revenue from segmenting by location & event type, we proceeded with that.

Total Annual Expected Revenue = **$25,700,005**

The Annual Expected revenue for Copa for each segment is given below:

![Summary_Page](https://github.com/nikunjachoure/Price-Optimization-Revenue-Management-Case-Study/blob/main/Image%209.png?raw=true)

Copa makes additional revenue of **$8,772,355.69** if the optimal prices (rounded) for each segment were used,
compared to the total revenue in 2007, and **$19,878.89** more than the increase in revenue if a single ppp is used.

## Next Steps

More exploration can be done to enhance the accuracy of the optimal prices, segmentation and revenue management. Some ways in which the problem statement can be modified are


    1. The contract in the data set does not have the same cost. The difference in cost may or may not affect your segmentation criteria
    2. If each group was not of the same size. 

