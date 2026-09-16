This is an analysis of a telco customer churn dataset sourced from [kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn). A copy was also uploaded to [github](https://github.com/EdricUy/Customer-Churn-Analysis/blob/main/Telco-Customer-Churn%20Data.csv).

This project aims to analyze churn based on contract types and service usage. 
This analysis is especially useful to know what to prioritize and push for during customer acquisition for a lower likelihood of churns. Power BI was used for this analysis and the file can be found [here](https://github.com/EdricUy/Customer-Churn-Analysis/blob/main/Telco%20Churn.pbix).

There are 2 types of services offered based on the data set, phone service and internet service. Under the internet service, there are 6 things that can be availed: device protection, online backup, online security, streaming movies, streaming TV, and tech support. 

![Raw Data](https://github.com/EdricUy/Customer-Churn-Analysis/blob/main/Images/Raw%20Data.png)

## Data Preparation
- Null values in the TotalCharges column were changed to 0 if Tenure was 0.
- Values in the SeniorCitizen column were changed from 0 and 1 to no and yes respectively.
- MonthlyCharges and TotalCharges were changed to Peso with the assumption that the raw data's values are in USD, with a conversion rate of 1:60.
- Added Tenure groups by years tenured
- Added Monthly Charge groups. ₱0-₱2,400 for low, ₱2,401-₱4,800 for mid, and higher than ₱4,800 for high.

A new table 'services' was created in power query to unpivot the 6 internet services in order to put them in a single chart later on for an easy comparison.

## Dashboard
### Page 1 - Contract Analysis
Measures were made for the cards, namely:  
- Total Customers
- Churned Customers
- Churn Rate
- Average Monthly Charge
- Average Tenure (in years)  
```
Total Customers = DISTINCTCOUNT('Telco-Customer-Churn Data'[customerID])
```
```
Churned Customers = CALCULATE(
    [Total Customers],
    'Telco-Customer-Churn Data'[Churn] = "Yes"
)
```
```
Churn Rate = DIVIDE(
    CALCULATE(
        DISTINCTCOUNT('Services'[customerID]),
        'Services'[Churn] = "Yes"
    ),
    DISTINCTCOUNT('Services'[customerID])
)
```
```
Average Monthly Charge = AVERAGE('Telco-Customer-Churn Data'[MonthlyChargesPeso])
```
```
Average Tenure = AVERAGE('Telco-Customer-Churn Data'[tenure])/12
```
![Contract Analysis](https://github.com/EdricUy/Customer-Churn-Analysis/blob/main/Images/Contract%20Analysis.png)
#### Customer type
The overall average churn rate is 31.83% and the monthly charge is at ₱3.89K. Both males and females are at roughly the same churn rate and monthly charge. The presence of a partner decreases churn rate to 24.02% and increases the monthly charge slightly to ₱4.07K. Having a dependent decreases churn rate to 20.31% but also has a lower average monthly charge of ₱3.57K. Senior Citizens have a higher churn rate at 43.21% but also a much higher monthly charge at ₱4.79K.
#### Payment method and Billing
Electronic checks by far results in a higher churn rate at 45.29% while automatic payments such as bank transfers and credit cards are at 16.71% and 15.24% respectively. Having not availed paperless billing results in a much lower churn rate at 16.33%.

#### Contract length and Tenure Groups
Month-to-month payments result in a churn rate of 42.71% while 1-year and 2-year contracts are at 11.27% and 2.83% respectively. By far the most significant difference of all metrics so far.

As for the tenure groups, getting past the 1-year mark is significantly the hardest, with a churn rate of 48.28%. After the 1st year, it goes down to 29.51% and every year after steadily goes down until a rate of 6.68% for those at more than 5 years of tenure. Interestingly enough, there is also an upward trend in the average monthly charges. 


### Page 2 - Usage Analysis
![Usage Analysis](https://github.com/EdricUy/Customer-Churn-Analysis/blob/main/Images/Usage%20Analysis.png)

#### Internet and Phone Service
For those that availed the internet service, Fiber optic had a significantly higher churn rate than DSL at 41.89% and 18.96% respectively, but also a higher monthly charge at ₱5.49k compared to DSL at ₱3.49k
A similar churn rate can be seen from those that availed the phone service and those that didn't, but availing for the phone service has a higher monthly charge of ₱1.5k as you would expect.

Taking a deeper dive at the available optional internet services, device protection, online backup, online security, and tech support all results in a significantly lower churn rate when availed with a difference of about 20%, while streaming movies and TV with only a 3% difference in the same direction.

### Page 3 - Tooltip
![Tooltip](https://github.com/EdricUy/Customer-Churn-Analysis/blob/main/Images/Tooltip.png)

This tooltip was used in most of the charts in the first 2 pages in order for the user to easily dissect and understand each chart further.

## Conclusion
During the process of a new customer acquisition, it is in the best interest of the company to pursue the following:
- Payment method: bank transfer or credit card
- Billing: not paperless
- Contract length: 1 year or 2 years
- Services: device protection, online backup, online security, tech support
