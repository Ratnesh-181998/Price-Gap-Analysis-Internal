PGA – Internal 

Business Requirement: 

In the GeM portal, buyers can currently shortlist products based on specific parameters to discover the 
best price available. However, the portal does not provide a method for validating whether the price of 
the selected product aligns with similar products, potentially leading to decisions based on incomplete 
information. 
Currently, the GeM portal focuses on price discovery but does not provide a mechanism to ensure that 
buyers are paying a fair price in comparison to other similar products. This lack of visibility into price 
variations among similar items could result in buyers paying more than necessary for a product. 
To address this gap, GeM aims to develop a Price Gap Analysis module for internal price validation. This 
module will allow buyers to validate the price of a selected product against a list of similar products 
identified using a product similarity solution. Key features include: 
• Price Similarity Validation: Identifying similar products that have the same score as the selected 
product based on features and characteristics. Here product similarity solution’s outcome is 
utilized to perform price similarity validation on similar products. 
• Minimum and Maximum Price Thresholds: Using statistical techniques, the solution will provide 
price variation thresholds based on historical sold prices and offer prices for similar products in 
CMS.  
By implementing the Price Gap Analysis for internal validation, GeM seeks to provide buyers with the 
tools to make more informed decisions, ensuring that their selected product's price is within a fair and 
reasonable range, ultimately improving value for money. 

<img width="1978" height="1075" alt="image" src="https://github.com/user-attachments/assets/a7c0898d-6f76-4169-8bfd-d287e28f369c" />


Proposed Solution: 

The Price Gap Analysis – Internal/CMS module is an and statistical-powered system designed to validate 
the price of a selected product by comparing it with a list of similar products that have the same score 
based on specific features.  
The objective of the price gap analysis – internal is to provide buyers with a detailed understanding of 
price variations among similar products by employing minimum and maximum thresholds. This Price 
Gap module will transform the GeM portal from a platform focused solely on price discovery to one that 
also offers validation of pricing fairness.  
The solution will allow buyers to see price ranges based on historical sold prices and offer prices. This 
will empower them to make more informed decisions, ensuring they are paying a fair price within the 
context of the marketplace, and ultimately enhancing the overall purchasing experience on the GeM 
portal. 

Solution Consumption: 

This solution is deployed on GeM server.  And available through dashboards and CMS API (currently in 
testing phase). GeM validation team has the access to dashboard to monitor and take appropriate 
actions.

<img width="1213" height="666" alt="image" src="https://github.com/user-attachments/assets/6287fa5b-6c8e-4b19-afd4-49f2eee9ed00" />

Data: 

To build this solution, required data consist of sold price data and offer price data for all the category 
id’s. The required data can be fetched from below tables. 
• Table: inb_variants 
• Table: inb_stock 
• Table: inb_browse_nodes 
• Table: inb_sellers 
• Table: inb_catalog_attrs

Model Methodology:  

1. Following four ways the data finalized to define the thresholds: 
• For any selected product, the first step is to check its historical sold price transactions. A 
minimum of five transactions is required for that product to ensure sufficient data for analysis.  
• If this is not possible due to an insufficient number of transactions, the next step is to check the 
sold price transaction data for similar products within the same category or group. Again, a 
minimum of five transactions is necessary for this group of similar products to proceed with the 
analysis.

• If both of the above conditions cannot be met, the next option is to use the offer price data for 
the selected product. In this case, at least five different sellers must have quoted prices for that 
product on the marketplace to ensure a reasonable comparison. 
• If none of these scenarios provide the required amount of data, the system will indicate that it 
cannot perform a price sanity check due to insufficient information. 
2. Once the required data is available from one of these steps, the next task is to calculate the price 
thresholds. The minimum price and maximum price are identified from the available data.  
3. The minimum threshold is calculated as 50% of the minimum sold price (Min Threshold = 0.5 * min 
sold price), while the maximum threshold is set at 1.5 times the maximum sold price (Max Threshold 
= 1.5 * max sold price).  
4. These thresholds provide a range for assessing whether a price is fair, too low, or too high based on 
historical data or offer prices. 

Note: PGA abrupt data will be used as input for sold price analysis and  removed outliers before proceeding to 
marketplace analysis. 
Historical time period for sold data – 1 year 

Steps In Model:  

1. Extract the required data: 
• From inb_variants , we get the make, model, title etc. of a product. 
• For inb_sellers, we get seller id and we will be getting the catalog id from inb_catalog_attrs. 
2. Clean data (pre-processing): 
• Read the output of abrupt use case and remove the flagged products. 
3. Transaction-Based Price Analysis – Sold Price Analysis: 
• The code begins by calculating the transaction count for each product using historical sold price 
data. Products with fewer than five transactions are separated for further analysis, while 
products with five or more transactions proceed to calculate the minimum and maximum price 
ranges. For products with insufficient transactions, the code checks if similar products within the 
same group have enough sold price data (at least five transactions). If sufficient group data is 
found, the minimum and maximum price ranges are computed; otherwise, the product is 
flagged for further investigation. 
4. Offer Price-Based Price Analysis: 
• If a product does not have enough historical sold price data, the code then checks offer prices 
from the marketplace. It calculates the minimum and maximum offer price ranges for products 
that have quotes from at least five different sellers.  
• If the criteria are not met, the system flags the product as having insufficient information to 
determine price ranges. This ensures that the system can handle cases where transaction data is 
limited by falling back on offer price analysis.

5. Final Price Range Determination: 
• The final step involves determining the most appropriate price range for each product, using 
either sold price data or offer price data, depending on which is available. The system assigns 
the minimum and maximum price ranges and the corresponding status to each product, 
ensuring that valid data is used. If both sold price and offer price data are unavailable, the 
product is marked as “cannot perform sanity due to insufficient information.” 
6. Handling Special Cases (Price Below 5 Rupees): 
• The code handles special cases where the product's sold or offer price is less than 5 rupees. If 
the offer price for any product is below this threshold, the code recalculates the minimum price 
and updates the final price range accordingly. The final results are then saved to a CSV file, 
ensuring the correct data is captured and output for further analysis.


Business Value: 

1. Enhanced Price Validation and Purchase Decision-Making: 
By leveraging the minimum, maximum, and median prices of selected products and their similar 
counterparts, buyers within the GeM portal can make more informed and validated purchasing 
decisions. Understanding the price range for a product, along with comparable alternatives, 
empowers buyers to avoid overpaying or selecting products quoted at abnormally high prices. This 
not only optimizes the purchasing process but also ensures that buyers maximize value for their 
expenditure by benchmarking products against comparable offerings within GeM. 
2. Risk Mitigation and Efficient Procurement: 
With access to comprehensive price ranges, buyers can better identify pricing anomalies, whether 
excessively high or low, and take corrective actions during procurement. This aids in mitigating risks 
such as over-inflated prices and ensures transparency in vendor pricing. Additionally, this price 
validation process improves procurement efficiency, as buyers can avoid engaging with suppliers 
who may be quoting prices significantly outside the norm.


