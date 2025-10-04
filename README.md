# Netflix Movies and TV Shows Data Analysis using SQL

![](https://images.ctfassets.net/y2ske730sjqp/1aONibCke6niZhgPxuiilC/2c401b05a07288746ddf3bd3943fbc76/BrandAssets_Logos_01-Wordmark.jpg?w=940)

## Overview
This project explores and analyzes a large Netflix dataset using SQL. The goal was to uncover insights about Netflix’s catalog — such as the balance between movies and TV shows, popular content ratings, and top-producing countries.

By formulating and solving a series of analytical questions, this project demonstrates how SQL can be used to organize, query, and draw insights from real-world data.

## Objectives

- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## Tools & Technologies

**PostgreSQL** — SQL database used for querying and analysis

**pgAdmin 4** — graphical interface to manage and visualize database operations

**CSV Dataset** — imported into PostgreSQL

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link:** [Movies Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)

## Schema

```sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix
(
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
```

## Business Problems and Solutions

### 1. Count the Number of Movies vs TV Shows

```sql
SELECT 
    type,
    COUNT(*)
FROM netflix
GROUP BY 1;
```

**Objective:** Determine the distribution of content types on Netflix.

### 2. Find the Most Common Rating for Movies and TV Shows

```sql
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
```

**Objective:** Identify the most frequently occurring rating for each type of content.

### 3. List All Movies Released in a Specific Year (e.g., 2020)

```sql
SELECT * 
FROM netflix
WHERE release_year = 2020;
```

**Objective:** Retrieve all movies released in a specific year.

### 4. Find the Top 5 Countries with the Most Content on Netflix

```sql
SELECT * 
FROM
(
    SELECT 
        UNNEST(STRING_TO_ARRAY(country, ',')) AS country,
        COUNT(*) AS total_content
    FROM netflix
    GROUP BY 1
) AS t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5;
```

**Objective:** Identify the top 5 countries with the highest number of content items.

### 5. List All Movies that are Documentaries

```sql
SELECT * 
FROM netflix
WHERE listed_in LIKE '%Documentaries';
```

**Objective:** Retrieve all movies classified as documentaries.

## Findings

-Movies dominate Netflix’s content library: the dataset shows a higher count of movies compared to TV shows.

-The most common rating overall was TV-MA, suggesting that mature content makes up a large share of Netflix’s offerings.

-The United States leads in total content production, followed by India, United Kingdom, Japan, and South Korea — showing Netflix’s heavy presence in both Western and Asian markets.

-2020 saw a steady release of movies, even during the global pandemic, reflecting Netflix’s continued production and acquisitions during that year.

-Documentaries form a distinct and diverse category, ranging from historical to social themes, highlighting Netflix’s investment in factual storytelling.

## Conclusion

This project shows how SQL can transform raw, unstructured datasets into meaningful insights. By asking targeted questions, we can quickly understand content distribution, audience focus, and market diversity in Netflix’s catalog. It demonstrates not only SQL proficiency but also the power of data analysis for understanding real-world trends.

## Author - Nischal Chhetri
This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch! 
- **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/nischal-chhetri145/)
