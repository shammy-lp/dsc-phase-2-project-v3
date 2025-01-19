# Phase 2 Project : MOVIE DATA ANALYSIS


### Business Problem

- Your company now sees all the big companies creating original video content and they want to get in on the fun. They have decided to create a new movie studio, but they don’t know anything about creating movies. You are charged with exploring what types of films are currently doing the best at the box office. - You must then translate those findings into actionable insights that the head of your company's new movie studio can use to help decide what type of films to create.

### Objectives
1. To identify high-performing genres with the best ratings to optimize budgets for maximum ROI.
2. To align movie production with audience preferences by focusing on popular genres and attributes.
3. To plan releases during peak seasons for optimal audience reach and profitability.
4. To leverage movie length and quality metrics to determine ideal formats for various genres.

### The Data

In the folder `zippedData` are movie datasets from:

* [Box Office Mojo](https://www.boxofficemojo.com/)
* [IMDB](https://www.imdb.com/)
* [Rotten Tomatoes](https://www.rottentomatoes.com/)
* [TheMovieDB](https://www.themoviedb.org/)
* [The Numbers](https://www.the-numbers.com/)

### Interactive Analysis With Tableau

### Data Understanding
0.2. Import Data
1. Data Acquisition Procedure
- SQLite Database: Extracted data from the im.db SQLite database, specifically using the movie_basics and movie_ratings tables.
- CSV File: Used the tmdb.movies.csv dataset to acquire additional movie-related information.

2. SQL Query
- Performed an inner join between movie_basics and movie_ratings on the common column (id) to extract the following fields:
id, title, original_title, start_year, runtime_minutes, genres, averagerating, and numvotes.

3. Data Enrichment
- Conducted a left join with the tmdb.movies.csv dataset on the id column to incorporate the release_date field into the data.

4. Output Dataset:
- The final dataset included the following columns:
id, title, original_title, start_year, runtime_minutes, genres, averagerating, numvotes, and release_date.


0.4. Column descriptions
1. Id: - Unique identifier for each movie, used as a reference key in the database and for joining datasets.
2. title: Primary title of the movie in English or the localized language.
3. original_title: Original title of the movie in its original language (if different from the primary title).
4. start_year: The year the movie was released or first made publicly available.
5: runtime_minutes Total runtime of the movie in minutes. Represents the duration of the movie.
6.genres: List of genres associated with the movie, such as "Action," "Drama," or "Comedy." Multiple genres may be included.
7.averagerating: Average rating for the movie based on user reviews or critic ratings. Reflects the overall quality of the movie.
8. numvotes: Number of votes or reviews the movie has received, indicating its popularity or audience reach.
9. release_date: The specific release date of the movie (day, month, and year). This field is sourced from the tmdb.movies.csv.

### Data preparation
- Dropped null values and duplicates
- Checking for Outliers
![alt text](<Pictures/Screenshot 2025-01-18 232026.png>)

- Outliers can distort statistical analyses, skew model performance, and misrepresent data patterns. Removing them ensures more reliable insights and model accuracy. Using the IQR method, I identified and removed outliers lying beyond Q1-1.5*IQR and Q3-1.5*IQR, as this method robustly captures the central data while minimizing the impact of extreme values.

#### Feature Engineering
These were the engineered columns
1. movie_length
2. success_metric
3. title_similarity
4. month
5. sesason
6. movie_rating
7. genre_list



# Univariate Analysis
## 2.1. Runtime Analysis
![alt text](<Pictures/Screenshot 2025-01-19 024807.png>)
### 2.1.1. Observation
- Most common runtime: The most frequent movie runtimes are clustered around the 90-minute mark, and somewhat around 100 minutes.
- Typical Range: A majority of movies fall within the 80 to 110-minute range.
- Skew: The distribution is somewhat skewed to the right with a longer tail extending to longer runtimes.
- Longer Runtimes are less frequent: Movies with very long runtimes are less frequent

## Frequency Of Movie Genre
![alt text](<Pictures/Screenshot 2025-01-19 021315.png>)
### Observation
1. Of the tree charts looking at top 10 genres
- Drama is the most frequent genre, significantly higher than all others.
- Documentary and Comedy & Drama Follow: Documentary and Comedy & Drama are the next most common genres, though with fewer movies than Drama.
- Mid-Range Genres: Comedy, Horror, Crime & Drama, Action & Adventure, and Drama & Romance are in a similar middle range, with a similar number of movies each.
- Less Frequent Genres: Horror & Thriller, and Biography & Documentary are the least common of those shown.

# Bivariate Analysis
## Time Series
![alt text](<Pictures/Screenshot 2025-01-19 021602.png>)
### Observations
- October Peak: There's a clear peak in movie releases during October, which is the highest point on the graph.
- February Dip: February has the lowest number of movie releases, showing a noticeable dip.
- Fluctuating Trend: The number of movie releases fluctuates throughout the year, with a general upward trend from July to October, and then a sharp decline.
- Early Year Releases: January starts with a relatively high number of releases, but that decreases quickly.
- Mid-Year Dip: There's a lower release count in July, before an upward trend.
- Late-Year Drop: Following the peak in October, releases decline through November and December.

# Multivariate Analysis
## Genres Impact on Ratings and Votes
![alt text](<Pictures/Screenshot 2025-01-19 021839.png>)
### Observations
1. High Engagement, High Rating Genres
- Action, Adventure, and Crime genres occupy the top-right quadrant of the plot, indicating they not only receive high ratings (around 5-6) but also attract a large number of votes (4000-5000), suggesting these genres have both mass appeal and quality content.
2. Documentary Paradox:
- Documentaries show an interesting pattern - they receive among the highest average ratings (around 7) but relatively lower vote counts (around 1000), indicating high quality but potentially more niche audience appeal.
3. Drama's Dominant Position:
- Drama shows one of the largest bubble sizes and maintains both strong ratings (5-6) and substantial vote counts (around 3000), suggesting it's a consistently popular and well-received genre.
4. Adult Content Performance:
- Adult genre appears at the bottom-left with the lowest ratings (around 2) and minimal votes, indicating both low engagement and poor reception.
5. Quality vs. Popularity Split:
- There's a noticeable divide where genres either excel in ratings (like Documentary, History, Music) or in vote counts (like Horror, Sci-Fi), but few manage to excel in both - with Action, Adventure, and Drama being the notable exceptions.

## Pareto Analysis
![alt text](<Pictures/Screenshot 2025-01-19 022026.png>)
### Observations
1. Top 3 Dominance:
- Drama, Comedy, and Action are the clear leaders, accounting for a disproportionately large share of total votes (approximately 60-70% based on the cumulative votes line), suggesting these genres should be primary focus areas for studios.
2. 80-20 Rule Application:
- The first 7-8 genres (Drama through Documentary) account for roughly 80% of all votes, exemplifying the Pareto principle where a small number of genres drive the majority of audience engagement.
3. Sharp Initial Slope:
- There's a steep rise in the cumulative percentage line for the first few genres, followed by a long tail of genres with minimal individual contribution, highlighting the significant gap between leading and niche genres.
4. Rating vs. Votes Disparity:
- The cumulative rating percentage (green line) increases more gradually than the votes percentage (orange line), suggesting that higher vote counts don't necessarily correlate with proportionally higher ratings.
5. Low-Impact Genres:
- Genres like Sport, War, and Adult at the far right contribute minimally to both total votes and ratings, indicating they might be less commercially viable for studios unless there's a specific strategic reason to pursue them.

# Key Findings
1. High Performance Genres
- Drama, Comedy, and Action are the most engaging genres, contributing 60-70% of audience votes.
- Biography is the most successful genre with high ratings and significant engagement.
- Documentary achieves high ratings but appeals to a niche audience with low vote counts.
2. Success Metrics
- High-rated movies do not guarantee mass appeal (e.g., Documentaries with high ratings but low popularity).
- Most commercially successful films fall in the "Average" ratings category.
3. Seasonal Trends
- Fall dominates with the highest movie releases and success metrics, particularly in October.
- February and mid-year (July) show dips in releases and engagement.
4. Runtime Preference
- Most successful movies have runtimes between 80–110 minutes.
- Extended-length movies perform better in genres like Biography, Animation, and Action.
5. Engagement Analysis
- Genres like Action, Adventure, and Crime lead with high votes and solid ratings, showing broad appeal.
- Short films and niche genres like Horror and War consistently underperform.

# Recommendations
1. Focus on popular and successful genres
- Prioritize Drama, Comedy, and Action for mainstream appeal and financial returns.
- Allocate resources to Biography films to balance critical acclaim and audience satisfaction.
- Reduce investments in low-performing genres like Horror unless targeting niche markets.
2. Strategic Release Planning
- Release high-budget films in the Fall, especially in October, to leverage audience engagement peaks.
- Avoid releases during February and mid-year dips for blockbuster productions.
3. Target Optimal Runtime Formats
- Focus on movies with runtimes between 80-110 minutes for mainstream genres.
- Produce longer films for Animation, Biography, and Action, which benefit from extended runtimes.
4. Diversfy Formats For Niche Markets
- Consider documentaries in feature-length formats to maximize quality and niche engagement.
- Leverage high-rating genres like Documentary and History for targeted marketing.

# Conclusion
- Data-driven decision-making is essential for successful movie production.
- High-performing genres like Drama, Comedy, and Action should be prioritized for maximum audience engagement and ROI.
- Aligning movie runtimes with audience expectations enhances overall success.
- Strategic release timing during peak seasons, such as Fall, optimizes visibility and profitability.
- While high ratings are valuable, commercial success often depends on balancing quality with broad audience appeal. 
- This approach allows the studio to effectively target diverse audience segments and maintain a competitive edge in the market.

