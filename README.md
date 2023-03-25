# Las Vegas Business Category Analysis(Yelp Dataset)

---
## Introduction
In this analysis, we analyze the Yelp dataset to gain insights into the businesses and their attributes. We will be using SQL to query the database and answer specific questions related to the data.

## Overview
The analysis is divided into three parts. In the first part, we group the businesses in a specific city and category by their overall star rating and compare the businesses with 2-3 stars to the businesses with 4-5 stars. In the second part, we group the businesses based on whether they are still open or closed and find differences between the two groups. In the third part, we conduct an analysis to determine which business category will have more prospects if opened in a particular city.

## Getting Started
To conduct this analysis, we will be using SQL to query the Yelp dataset. You will need to have access to the Yelp dataset and a software that can execute SQL commands. We will be using the following tables for our analysis: business, category, and hours.

## Data
The data used in this analysis is the Yelp dataset, which contains information on businesses, reviews, and user data. The specific tables used in this analysis are:

- business: contains information on businesses, such as business ID, name, address, city, state, postal code, latitude, longitude, star rating, and number of reviews.
- category: contains information on the categories associated with a business.
- hours: contains information on the opening and closing hours of a business.
## Inferences and Analysis
### Part 1 : Business Grouping
In this part, we group the businesses in a specific city and category by their overall star rating and compare the businesses with 2-3 stars to the businesses with 4-5 stars. We answer the following questions:

- Do the two groups you chose to analyze have a different distribution of hours?
- Do the two groups you chose to analyze have a different number of reviews?
- Are you able to infer anything from the location data provided between these two groups?

We used the following SQL code to conduct the analysis - 

```SQL
SELECT 
  CASE 
    WHEN b.stars BETWEEN 2 AND 3 THEN '2-3'
    WHEN b.stars BETWEEN 4 AND 5 THEN '4-5'
    ELSE 'other' 
  END AS range_stars,
  b.name,
  h.hours,
  b.postal_code,
  review_count   
FROM 
  business b 
  INNER JOIN hours h ON b.id = h.business_id 
  INNER JOIN category c ON b.id = c.business_id
WHERE 
  (c.category LIKE '%Shopping%' AND b.city = 'Las Vegas') 
  AND (b.stars BETWEEN 2 AND 3 OR b.stars BETWEEN 4 AND 5)
GROUP BY name;
```
#### Results
The query returned the name, hours of operation, postal code, and review count of businesses in Las Vegas that fall under the "Shopping" category and have an overall star rating of 2-3 or 4-5. The businesses were grouped by their star rating.

#### Inferences
The 4-5 star group appears to have shorter hours of operation than the 2-3 star group. However, this conclusion is based on a small sample size of only three businesses.

### Part 2 : Open vs. Closed Businesses
In this part, we group the businesses based on whether they are still open or closed and find differences between the two groups. We answer the following questions:

- What differences can you find between the ones that are still open and the ones that are closed?

We used the following SQL code to conduct the analysis - 

```SQL
SELECT 
	CASE 
		WHEN is_open = 1 THEN 'Still opened'
		ELSE 'Closed' 
	END AS condition, 
	ROUND(AVG(stars),2) avg_stars, 
	ROUND(AVG(review_count),2) avg_review_count, 
	count(DISTINCT id) AS num_of_shops 
FROM 
	business 
GROUP BY 
	is_open;
```
#### Results
The query returned the average star rating, review count, and number of shops for businesses that are still open and those that have closed down.

#### Inferences
On average, businesses that are still open tend to have more reviews and a higher star rating than those that have closed down.

### Part 3 : Prospect of Business
In the third part, we chose to conduct an analysis on which business category will have more prospects if opened in a particular city. For this analysis, we used a specific category's average review_count and average star rating. We included businesses that are still open.

- Which business category will have more prospects if opened in a particular city?

We used the following SQL code to conduct the analysis - 

```SQL
SELECT 
  c.category,
  ROUND(AVG(b.stars),3) AS avg_stars,
  ROUND(AVG(b.review_count),3) AS avg_review,
  COUNT(b.id) AS num_shops_exist
FROM
  business b 
  INNER JOIN category c ON b.id = c.business_id 
WHERE is_open = 1 AND b.City = 'Las Vegas'
GROUP BY
  c.category
ORDER BY 
  avg_review DESC,
  avg_stars DESC;
```
#### Results
The query returned the average star rating, average review count, and number of shops that exist for each category in Las Vegas. The dataset is sorted in descending order of average review count, followed by the average star rating.

#### Inferences
From the analysis, it can be concluded that the Asian Fusion, Chinese, Malaysian, Noodles, Soup, and Taiwanese categories have the highest average review count and star rating, suggesting that they may have more prospects if opened in Las Vegas. On the other hand, the Bars, Nightlife, and Restaurants categories have a relatively lower average review count and star rating, indicating that they may have less potential. However, it is important to note that this analysis is based on the data available and does not take into account other factors such as competition, location, and market trends.
## Conclusion
In conclusion, the Yelp dataset provided us with valuable insights into the businesses listed on Yelp. We were able to compare businesses based on their overall star rating, whether they are open or closed, and which business category will have more prospects if opened in a particular city. This analysis can help businesses make data-driven decisions on how to improve their performance and stand out from the competition.
