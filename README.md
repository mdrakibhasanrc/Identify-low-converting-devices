### How This Query Help you

Different devices (mobile, desktop, tablet) have varying levels of engagement and conversions. If certain device categories underperform, businesses may lose revenue due to usability issues, slow speed, or poor experience. This SQL query analyzes user behavior and conversion performance for mobile, desktop, and tablet users. It calculates key metrics such as total users, purchases, item quantities, purchase revenue, and conversion rates.

### Why It Matters

i) Identifies which devices drive the most conversions and revenue.

ii) Helps businesses prioritize improvements for underperforming device categories.

iii) Supports data-driven decision-making for UX and marketing strategies.

#### Copy This SQL Code and Paste your Bigquery SQL Editor & Use Your Own GA4 Dataset Replace the sample dataset:

```
-- declare start date and end date for the date range filter
declare start_date date default '2020-11-01';  
declare end_date date default '2021-01-31';

-- select device category and aggregate data for user, purchase, quantity, and revenue
select  
     -- category of the device used by the users
     device.category as device_category,  
     
     -- count of distinct users in the specified date range
     count(distinct user_pseudo_id) as total_user,  
     
     -- count of purchases that occurred during the period
     countif(event_name='purchase') as total_purchase,  
     
     -- sum of the total number of items purchased in the transaction, only for 'purchase' events
     sum(if (event_name='purchase', ecommerce.total_item_quantity, null)) as total_item_quantity,  
     
     -- sum of revenue generated from all purchases, only for 'purchase' events
     sum(if (event_name='purchase', ecommerce.purchase_revenue, null)) as total_purchase_revenue,   
     
     -- calculate conversion rate by dividing purchases by unique users, and multiplying by 100 to get percentage
     round(safe_divide(countif(event_name='purchase'), count(distinct user_pseudo_id)) * 100, 2) as conversion_rate  
     
-- specify the dataset being queried from Google BigQuery sample eCommerce events
from `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`  

-- filter data between the start date and end date
where parse_date('%Y%m%d', event_date) between start_date and end_date  

-- group the results by the device category
group by device_category;

```
#### Query Result: 

![image](https://github.com/user-attachments/assets/9b65df47-a606-4e2b-a395-c8188ac0497a)

### Explanation of Metrics 

Here’s a simple breakdown of each metric:

i) Total Users – The number of unique visitors from each device.
      
 More users mean higher potential for sales, but not all visitors convert.
      
ii) Total Purchases – The number of orders placed.

A higher number indicates that users are successfully completing purchases.
      
iii) Total Items Sold – The total number of products bought.
      
Useful to track whether users buy multiple items per order or just one.
      
iv) Total Revenue ($) – The total earnings from purchases.
      
This shows which device category is generating the most income.
      
v) Conversion Rate (%) – The percentage of visitors who make a purchase.
      
Formula: (Total Purchases / Total Users) × 100
      
A higher conversion rate means users are finding the store easy to use.

### Summary & Insights

I) Mobile users have the highest add-to-cart (23,223) and begin-checkout (15,696) counts, but their cart abandonment rate (89.86%) and checkout abandonment rate (85.0%) are also high. Optimizing the mobile checkout process and offering incentives could help reduce abandonment and increase conversions.

II) Desktop users show strong engagement with 34,047 add-to-carts and 22,272 begins-checkout, but their cart abandonment rate (90.52%) and checkout abandonment rate (85.52%) remain concerning. Focusing on improving desktop checkout usability could help retain more users through the purchase stage.

III) Tablet users have lower engagement with 1,273 add-to-carts and 789 begins-checkout, but their abandonment rates are slightly higher than mobile (cart abandonment: 91.28%, checkout abandonment: 85.93%). Improving the tablet experience and addressing potential usability issues could enhance conversions.
 

### Recommendations for Improvement

✅ Optimize Mobile Experience:
    
Improve page speed, reduce checkout steps, and ensure a smooth browsing experience.
    
Use mobile-friendly payment options (Apple Pay, Google Pay) to reduce drop-offs.
    
✅ Enhance Desktop Personalization:
    
Show personalized product recommendations to encourage conversions.
    
Offer a guest checkout option to reduce friction.
    
✅ Improve Tablet Usability:
    
 Ensure buttons, text, and images adjust properly to tablet screens.
    
Run A/B tests to see what design changes improve engagement.

### How to Use This Query Template

i)  Adjust Date Range: Modify the start_date and end_date values to analyze a different period.
      
ii) Use Your Own GA4 Dataset: Replace the sample dataset with your actual Google Analytics BigQuery export.

iii)  Run the query to generate insights. 

iv)  Analyze the results and take data-driven actions.
      
### Customize Metrics:
      
i) Want to track average order value (AOV)? Add AVG(ecommerce.purchase_revenue).

ii) Need to measure session duration? Use AVG(event_bundle_sequence_id).
