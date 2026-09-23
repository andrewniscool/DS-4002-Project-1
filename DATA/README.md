
# Data Summary 
Our dataset is a consolidated CSV of English-language user reviews collected from The Movie Database (TMDB) for superhero films, currently including films from the Spider-Man, Avengers, Captain America, and Iron Man franchises. We have 485 observations or reviews. Reviews were collected through the TMDB API using a Python script, and each row contains the review text, movie title, review posting date, TMDB reviewer rating when available, and TMDB review ID. We will add each film’s release date and derive variables including time since release, review word count, positive and negative word counts, and a polarity score for the statistical analysis. The dataset and collection materials are stored in the group’s shared Google Drive folder and can be accessed through the link provided with the project materials.


# Provenance
All reviews originate from user-generated contributions on The Movie Database (TMDB) and were collected through the official TMDB API rather than by scraping the website. We created a Python script to retrieve reviews for selected superhero films and organize the API responses into a structured dataset containing the review text, movie title, review date, reviewer rating when available, and TMDB review ID. Review IDs are retained to support de-duplication and maintain a link between each observation and its source. Film release dates will also be obtained from TMDB and used with review posting dates to calculate the amount of time between a film’s release and each review.


# License
TMDB permits non-commercial use of its API with appropriate attribution [1], [2]. Our group also contacted TMDB and described our intended academic use of API-collected film reviews for sentiment analysis; TMDB responded that there was no issue with the proposed use. We will credit TMDB and include the required notice: “This product uses the TMDB API but is not endorsed or certified by TMDB.”


# Ethical Statements
The reviews we use are user-generated content and remain the property of their original authors. We will use the review text only for this coursework, keep the raw dataset out of any public or commercial release, and report our results in aggregate. 


# Data Dictionary 



# Exploratory Plots

<img width="594" height="333" alt="Screenshot 2026-09-23 at 2 38 03 PM" src="https://github.com/user-attachments/assets/e2fa7145-2c96-4cb6-8e85-d2e0e1c086a3" />


