
# Funnel Analysis Insights and Recommendations
#### Link to the project: [Funnels](https://docs.google.com/spreadsheets/d/1UABzkCq1RqcS25MjUwer2x4LL5YvUFyA_vO26Aui9z6KQ/edit?usp=sharing)

---

### Repository Description
This repository contains a comprehensive analysis of user interaction data, focusing on funnel behavior across events (`user_engagement`, `view_item`, `add_to_cart`, etc.), segmented by country and device type. Key insights and actionable recommendations are provided to address drop-offs at various stages of the funnel.

---
### Visualizations:
#### Users by Country by Event
<img src="https://github.com/user-attachments/assets/76407b6f-493a-4a20-8ad1-17b93e14dee6" width="800" /> 

This chart shows the distribution of users across different events, segmented by country. The funnel reveals key insights into user behavior.

#### Users by Event by Category
<img src="https://github.com/user-attachments/assets/225a2105-7e0f-45fc-aea1-06fadd46e34c" width="800" />

This chart illustrates how users engage with different events based on their categories, offering a detailed view of interaction patterns.



---

### Key Insights and Recommendations

#### Insight 1: Significant Drop-off After User Engagement
- **Observation**: Over 70% of users do not progress from `user_engagement` to `view_item`.
- **Device Observation**: Tablets experience slightly higher drop-off rates than desktops and mobile devices.

##### Recommendations:
1. **Improve Product Discovery**:
   - Enhance the product recommendation engine.
   - Refine search functionality to help users find items faster.
2. **Engaging Content**:
   - Provide personalized, engaging content on landing pages.
   - Optimize content for tablet users to address usability issues.
3. **Responsive Design Enhancements**:
   - Ensure seamless, responsive experiences across all devices.

---

#### Insight 2: Drop-off Between 'view_item' and 'add_to_cart'
- **Observation**: The second-largest drop-off occurs here, indicating issues with perceived item value or usability barriers during cart additions.
- **Device Observation**: Tablets show the highest drop-off in this stage.

##### Recommendations:
1. **Optimize Product Pages**:
   - Add high-quality images, detailed descriptions, and customer reviews.
   - Highlight product features to build trust and interest.
2. **Improve Add-to-Cart Functionality**:
   - Make the add-to-cart process seamless, especially on tablets.
   - Use prominent and persuasive "Add to Cart" buttons.
3. **Incentives for Cart Additions**:
   - Offer promotions or discounts to encourage cart additions.

---

#### Insight 3: Drop-off Between 'add_payment_info' and 'purchase'
- **Observation**: Although smaller, a significant drop-off exists here. Mobile users are most likely to complete purchases once payment info is added.

##### Recommendations:
1. **Trust and Clarity**:
   - Provide clear order summaries for desktop and tablet users.
   - Include assurances like security badges to build trust.
2. **Streamline Checkout**:
   - Minimize steps in the checkout process.
   - Optimize the flow for mobile-friendliness.
3. **Customer Assistance**:
   - Offer live chat or real-time assistance for users during the checkout stage.

---

### SQL Queries

#### Query 1: Generate Unique Events Table
```sql
WITH
ranked_events AS (
SELECT
*,
ROW_NUMBER() OVER (PARTITION BY user_pseudo_id, event_name ORDER BY event_timestamp DESC ) AS rn
FROM
`turing_data_analytics.raw_events` )
SELECT
event_name,
COUNT(*) AS event_count
FROM
ranked_events
WHERE
rn = 1
GROUP BY
event_name
ORDER BY
event_count DESC
```
#### Query 2: Funnel Aggregation By Country
```sql
Top 3 countries by events:	
	
WITH ranked_events AS (	
SELECT *,	
ROW_NUMBER() OVER (	
PARTITION BY user_pseudo_id, event_name	
ORDER BY event_timestamp DESC	
) AS rn	
FROM `turing_data_analytics.raw_events`	
)	
SELECT	
country,	
COUNT(*) AS event_count	
FROM ranked_events	
WHERE rn = 1	
GROUP BY country	
ORDER BY event_count DESC	
LIMIT 3	
```
	
#### Query 2: Events By Specific Category
```sql					
WITH							
ranked_events AS (							
SELECT							
*,							
ROW_NUMBER() OVER (PARTITION BY user_pseudo_id, event_name ORDER BY event_timestamp DESC ) AS rn							
FROM							
`turing_data_analytics.raw_events`							
)							
SELECT							
event_name,							
category,							
COUNT(*) AS event_count							
FROM							
ranked_events							
WHERE							
rn = 1							
AND category = 'desktop'							
AND event_name IN ('user_engagement', 'view_item', 'add_to_cart', 'begin_checkout', 'add_payment_info', 'purchase')							
GROUP BY							
event_name, category							
ORDER BY							
event_count DESC;
```					

