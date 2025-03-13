# 📊 Netflix Data Analysis Using PostgreSQL 🍿

![image](https://github.com/mih-shanto/Netflix-Data-Analysis-Using-SQL-Exploring-User-Behavior-and-Content-Trends/blob/main/Project%20Details/logo.png)

## 🚀Project Overview
In this project, I analyzed Netflix's vast content library using PostgreSQL, uncovering key patterns and trends in movies and TV shows. By writing complex SQL queries, I extracted valuable insights about content distribution, ratings, and more.

     1. Count the number of Movies vs TV Shows
    
    SELECT 
    	type,
    	COUNT(*)
    FROM netflix
    GROUP BY 1

    2. Find the most common rating for movies and TV shows
    
    WITH RatingCounts AS (
        SELECT 
            type,
            rating,
            COUNT(*) AS rating_count
        FROM netflix
        GROUP BY type, rating
	),
	RankedRatings AS (
	    SELECT 
	        type,
	        rating,
	        rating_count,
	        RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
	    FROM RatingCounts
	)
	SELECT 
	    type,
	    rating AS most_frequent_rating
	FROM RankedRatings
	WHERE rank = 1;


	3. List all movies released in a specific year (e.g., 2020)
	
	SELECT * 
	FROM netflix
	WHERE release_year = 2020
	
	
	4. Find the top 5 countries with the most content on Netflix
	
	SELECT * 
	FROM
	(
		SELECT 
			-- country,
			UNNEST(STRING_TO_ARRAY(country, ',')) as country,
			COUNT(*) as total_content
		FROM netflix
		GROUP BY 1
	)as t1
	WHERE country IS NOT NULL
	ORDER BY total_content DESC
	LIMIT 5
	
	
	5. Identify the longest movie
	
	SELECT 
		*
	FROM netflix
	WHERE type = 'Movie'
	ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC
	
	
	6. Find content added in the last 5 years
	SELECT
	*
	FROM netflix
	WHERE TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years'


	7. Find all the movies/TV shows by director 'Rajiv Chilaka'!
	
	SELECT *
	FROM
	(
	
	SELECT 
		*,
		UNNEST(STRING_TO_ARRAY(director, ',')) as director_name
	FROM 
	netflix
	)
	WHERE 
		director_name = 'Rajiv Chilaka'

	
	8. List all TV shows with more than 5 seasons
	
	SELECT *
	FROM netflix
	WHERE 
		TYPE = 'TV Show'
		AND
		SPLIT_PART(duration, ' ', 1)::INT > 5


	9. Count the number of content items in each genre
	
	SELECT 
		UNNEST(STRING_TO_ARRAY(listed_in, ',')) as genre,
		COUNT(*) as total_content
	FROM netflix
	GROUP BY 1



	-- 10. List all movies that are documentaries
	SELECT * FROM netflix
	WHERE listed_in LIKE '%Documentaries'



	-- 11. Find all content without a director
	SELECT * FROM netflix
	WHERE director IS NULL
## 📊 Key Findings & Queries:
- ✅ **Movies vs. TV Shows** – Analyzed the distribution to see which dominates Netflix.  
- ✅ **Most common ratings** – Found the most frequent content ratings on the platform.  
- ✅ **Yearly releases** – Listed all movies released in a specific year, such as 2020.  
- ✅ **Top 5 content-producing countries** – Identified which countries contribute the most to Netflix. 🌍  
- ✅ **Longest movie** – Found the movie with the highest runtime on the platform.  
- ✅ **Recent content additions** – Checked how much content was added in the last 5 years.  
- ✅ **Director spotlight** – Found all movies & TV shows by ‘Rajiv Chilaka.’ 🎬  
- ✅ **TV shows with more than 5 seasons** – Listed series with long-running success.  
- ✅ **Genre distribution** – Counted content items in each genre to understand popularity.  
- ✅ **Documentary movies** – Filtered out all movies that belong to the documentary category.  
- ✅ **Missing director info** – Identified content without a credited director.  

## 🔍 Skills Applied:
- ✔ Writing complex SQL queries in PostgreSQL  
- ✔ Data cleaning & filtering  
- ✔ Aggregation, sorting, and grouping  
- ✔ Data exploration & pattern recognition
  
⭐ If you find this project interesting, don't forget to **star** the repo!  
