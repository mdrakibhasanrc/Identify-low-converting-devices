### Problem Statment

Different devices (mobile, desktop, tablet) have varying levels of engagement and conversions. If certain device categories underperform, businesses may lose revenue due to usability issues, slow speed, or poor experience.

### Short Description

This SQL query analyzes user behavior and conversion performance for mobile, desktop, and tablet users. It calculates key metrics such as total users, purchases, item quantities, purchase revenue, and conversion rates.

### Why It Matters

Identifies which devices drive the most conversions and revenue.

Helps businesses prioritize improvements for underperforming device categories.

Supports data-driven decision-making for UX and marketing strategies.

#### SQL Code:

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

### Explanation of Metrics 

Here’s a simple breakdown of each metric:

      Total Users – The number of unique visitors from each device.
      
      More users mean higher potential for sales, but not all visitors convert.
      
      Total Purchases – The number of orders placed.
      
      A higher number indicates that users are successfully completing purchases.
      
      Total Items Sold – The total number of products bought.
      
      Useful to track whether users buy multiple items per order or just one.
      
      Total Revenue ($) – The total earnings from purchases.
      
      This shows which device category is generating the most income.
      
      Conversion Rate (%) – The percentage of visitors who make a purchase.
      
      Formula: (Total Purchases / Total Users) × 100
      
      A higher conversion rate means users are finding the store easy to use.

### Summary & Insights

    Mobile Users (Highest Conversion Rate - 2.16%)
    
    Mobile shoppers are converting well, generating $146,768 in revenue.
    
    Action: Ensure a fast-loading, mobile-friendly site to maximize conversions.
    
    Desktop Users (Highest Revenue - $208,815)
    
    Desktop users bring in the most revenue, but their conversion rate (2.03%) is slightly lower than mobile.
    
    Action: Improve engagement with personalized offers & a smooth checkout.
    
    Tablet Users (Lowest Conversion Rate - 1.78%)
    
    Tablet users contribute the least revenue ($6,582) and have the lowest conversion rate.
    
    Action: Ensure tablet-friendly design with better touch navigation & layouts.

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

      Adjust Date Range: Modify the start_date and end_date values to analyze a different period.
      
      Use Your Own GA4 Dataset: Replace the sample dataset with your actual Google Analytics BigQuery export.
      
      Customize Metrics:
      
      Want to track average order value (AOV)? Add AVG(ecommerce.purchase_revenue).
      
      Need to measure session duration? Use AVG(event_bundle_sequence_id).
