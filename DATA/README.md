
# Data Summary 
Our dataset is a consolidated CSV of English-language user reviews collected from The Movie Database (TMDB) for superhero films, currently including films from the Spider-Man, Avengers, Captain America, and Iron Man franchises. We have 485 observations or reviews. Reviews were collected through the TMDB API using a Python script, and each row contains the review text, movie title, review posting date, TMDB reviewer rating when available, and TMDB review ID. We will add each film’s release date and derive variables including time since release, review word count, positive and negative word counts, and a polarity score for the statistical analysis. The dataset and collection materials are stored in the group’s shared Google Drive folder and can be accessed through the link provided with the project materials.


# Provenance
All reviews originate from user-generated contributions on The Movie Database (TMDB) and were collected through the official TMDB API rather than by scraping the website. We created a Python script to retrieve reviews for selected superhero films and organize the API responses into a structured dataset containing the review text, movie title, review date, reviewer rating when available, and TMDB review ID. Review IDs are retained to support de-duplication and maintain a link between each observation and its source. Film release dates will also be obtained from TMDB and used with review posting dates to calculate the amount of time between a film’s release and each review.


# License
TMDB permits non-commercial use of its API with appropriate attribution [1], [2]. Our group also contacted TMDB and described our intended academic use of API-collected film reviews for sentiment analysis; TMDB responded that there was no issue with the proposed use. We will credit TMDB and include the required notice: “This product uses the TMDB API but is not endorsed or certified by TMDB.”


# Ethical Statements
The reviews we use are user-generated content and remain the property of their original authors. We will use the review text only for this coursework, keep the raw dataset out of any public or commercial release, and report our results in aggregate. 


# Data Dictionary 

## Data Dictionary

| Feature | Type | Description | Uncertainty / Notes |
|---|---|---|---|
| `movie_id` | integer | TMDB identifier for the film associated with the review | Unique to each film, but repeated across reviews of the same film |
| `movie` | string | Title of the film associated with the review | Film titles may not uniquely identify remakes without the accompanying year |
| `year` | integer | Release year of the film | Contains only the release year, not the full release date |
| `franchise` | string | Film the review concerns | 4 Marvel-related franchises |
| `author` | string | Display name of the review author returned by TMDB | User-generated identifier; not needed for the primary analysis |
| `review_id` | string | TMDB review identifier | Supports de-duplication and provenance |
| `review` | string | Full text of the TMDB user review | Length and formatting vary across reviews |
| `review_url` | string | URL linking to the original TMDB review | Retained for provenance and source verification |
| `created_at` | datetime | Original timestamp indicating when the review was created | Includes date, time, and UTC offset |
| `updated_at` | datetime | Timestamp indicating when the review was last updated | May differ from the original posting date |
| `author_username` | string | TMDB username associated with the review author | Helpful to see the same opinion through films but could cause incoordination |
| `author_rating` | numeric (0–10) | Numerical movie rating supplied by the review author | Frequently missing; 292 of 484 reviews have no rating |
| `review_date` | date (YYYY-MM-DD) | Date the review was originally posted | Derived directly from the TMDB timestamp; inherits uncertainty in creation |
| `review_month` | integer | Numeric month in which the review was posted | Derived from the review timestamp; errors could translate |
| `review_month_name` | string | Name of the month in which the review was posted | Derived directly from review_month so errors could translate |
| `review_year` | integer | Year in which the review was posted | Derived from the review timestamp; represents when the review was posted |
| `character_count` | integer | Number of characters in the review text | Calculated from character and word counts, so it inherits their limitations |
| `word_count` | integer | Number of words in the review text | Same as character_count |
| `sentence_count` | integer | Number of sentences identified in the review | Informal or incomplete writing can lead to miscalculation |
| `avg_word_length` | numeric | Average number of characters per word in the review | Same limitations as character_count |


# Exploratory Plots

<img src="https://github.com/user-attachments/assets/e2fa7145-2c96-4cb6-8e85-d2e0e1c086a3" width="500">

Figure 1 shows the number of times specific actors/ characters were mentioned. Since these movies include overlapping elements, we wonder to what extent these terms were referenced, considering the number of reviews each film had. 


<img src="https://github.com/user-attachments/assets/0bbe1865-fb68-44d5-b4d5-9d0ca6f26693" width="500">

Figure 2 shows when reviews were most written. Although each movie had different release dates, seeing the common dates of reviews can be useful for our project. 


<img src="https://github.com/user-attachments/assets/f399112b-31fa-412a-b2eb-56d6620e59f8" width="500">

In Figure 3, we can see the length of each review. This will help us think about assigning sentiment to specific reviews, considering that not all reviews are short and probably have multiple words depicting sentiment. 
