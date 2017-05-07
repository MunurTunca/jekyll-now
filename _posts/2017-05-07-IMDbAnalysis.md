---
layout: post
title: Factors that affect movie ratings - An exploratory analysis
---

### Intro

Entertainment has been an integral part of society for as far as history shows. From plays of old to movies and TV shows of now, humans have always been interested in these types of art forms. However, what makes any of these a success? Is there a combination of factors that influence how we rate entertainment forms,such as movies? Lets find out!

In this post we will be exploring factors that affect a movies rating on [IMDb](http://www.imdb.com/). As you may know, IMDb (Internet Movie Database), holds information on millions of movies with all the details from duration to director to aspect ratio.



### Data Collection

The first thing I did was to take a look at a movies page to see exactly what type of information was provided. I started with one of my favorite movies; My Neighbor Totoro. After looking through the page (most of which was spent getting distracted the beautiful world created by Hayao Miyazaki), I selected twenty one points of information to extract.

__Information Selected__:

1. Movie ID (to get to a movies page)
2. Title
3. Release Year
4. Category/Genres
5. Rating
6. Awards
7. Nominations
8. Length
9. Director
10. Writers
11. Studio/Distributor/Publisher
12. Cast
13. MPAA Rating
14. Language
15. Country
16. Summary
17. Storyline
18. Number of Votes
19. Number of Critic reviews
20. Number of user reviews
21. Number of news articles



Although there are numerous ways in collecting/getting this data from IMDb, I opted for web scraping to have more flexibility in the data I was using and to make sure that the data was accurate. To do this I used two python packages;  [Requests](http://docs.python-requests.org/en/master/) to make calls and get the HTML code and [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/) to parse through the data.

Now that the data collection method and points of information was decided on, it was time to get started on the fun part! Extracting the relevant information.

Stating from the title, I extracted all of the selected information and thought it would be general enough to be applicable to all pages. Boy was I wrong! IMDb is very dynamic in the way they display information. For example: if a movie has a single writer, it gets displayed as "Writer:", but if it has multiple writers, it's displayed as "Writer__<u>s</u>__:". Another one would be the banner displaying the number of awards and nominations a movie has received. Not each movie has!

With this in mind it was time for some error control using `try:` and `except:` statements in python. Using a combination of this and `if:` and `else:` statements, I was also able to get exactly twenty one columns of information. Consistency is very important when collecting data for analysis.

As the last step in web scraping, I wrote a function to gather IMDb movie IDs. With this information I had ~13k movies to extract information of. I randomly selected a subset of two thousand and ran my code.



### Findings

As mentioned in the intro, the point of this analysis was to find which factors contribute to a movies rating. To do this I visualized some parts of my data using [Plotly](https://plot.ly/) and [Seaborn](https://seaborn.pydata.org/).

Before I dived into anything, I wanted to take a look at the distribution of ratings.In a normally distributed rating data on a scale of 1-10, we would expect the average (mean) to be five stars. However, in this dataset the mean rating was  6.4. This told me that the data was skewed left which a simple distribution plot showed. 

<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~munur_tunca/24.embed"></iframe>



I was also curious about the trend of ratings by year. To visualize this, I created a scatter plot. Though no obvious pattern was observed, the scatterplot demonstrated that majority of the movies in the dataset were from 1980 onwards. It also showed that there was high variation in ratings for each year.

<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~munur_tunca/27.embed"></iframe>



Another interesting find was the rating distribution by movie genre. The mean rating for horror movies was the lowest out of all the categories and had large variance. Whereas, documentaries had the highest mean rating and a small variance.

<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~munur_tunca/50.embed"></iframe>



The last thing I visualized was a heat map of how each of the selected features correlated with the ratings. It didn't come as a surprise that the number of awards won and the number of award nominations was highly correlated with a movies rating. However there were some interesting insights in this heat map. 

__Insights__:

- Jim Belushi and Bruce Willis are associated with lower ranking movies. They either end up with bas scripts or may not be as talented as we think.
- Though Walt Disney Pictures and Animation are associated with higher ranked movies, Family movies appear in the bottom five with a negative correlation to rank



![imdb_feature_corr](C:\Users\Munur\Desktop\munurtunca.github.io\munurtunca.github.io\images\imdb_feature_corr.png)

### Modeling

The final step was to see if we could predict the rating based solely on the features selected. Using [Pandas](http://pandas.pydata.org/) data frames and ensemble methods from [Scikit-Learn](http://scikit-learn.org/stable/). I treated the rating as continuous rather than binary, or multiclass.

I began with a random forest model. `RandomForestRegressor(n_estimators=500, max_depth=20)` The best results I had was using 500 decision trees with a max depth of 20 branches. Though this performed well with the training data, it was nor so good with the test data.

```python
Train R^2 score: 0.921419560754
Test R^2 score: 0.491484943162

Train explained variance: 0.921462216521
Test explained variance: 0.492540897939

Train mse: 0.00154872395479
Test mse: 0.0108463953815
```

When looking at the top 10 important features selected by the random forest, I noticed that it was very similar to the correlation heat map displayed earlier.

| Feature        | Importance |
| -------------- | ---------- |
| Nominations    | 0.197310   |
| Rating Votes   | 0.110589   |
| Awards         | 0.087419   |
| User Reviews   | 0.079645   |
| Documentary    | 0.076293   |
| Release Year   | 0.076131   |
| Duration       | 0.056658   |
| News Articles  | 0.045210   |
| Critic Reviews | 0.041434   |
| Drama          | 0.035421   |



I moved on to a Bagging Regressor next. `BaggingRegressor(n_estimators=300, n_jobs=-1)` Same as the random forest, this model also performed a lot better on the training than the test data. Unfortunately, We cannot view the weight of features within the bagging model.

```python
Train R^2 score: 0.924736046894
Test R^2 score: 0.491801767177

Train explained variance: 0.924775864784
Test explained variance: 0.492814836654

Train mse: 0.00148336008587
Test mse: 0.010839637669
```

The third and last ensemble model I tried was the Extreme Random Forrest Regressor. `ExtraTreesRegressor(n_estimators=500, max_depth=20)` This model actually performed worse in the test dataset than both the bagging and random forest models. 

```python
Train R^2 score: 0.987421136866
Test R^2 score: 0.468104130682

Train explained variance: 0.987421136866
Test explained variance: 0.471355493243

Train mse: 0.000247913944572
Test mse: 0.0113450975007
```



Interestingly enough the only difference in feature importance was the substitution of critic reviews to USA.

| Feature       | Importance |
| ------------- | ---------- |
| Rating Votes  | 0.124119   |
| Nominations   | 0.088673   |
| Awards        | 0.082097   |
| Documentary   | 0.070765   |
| Drama         | 0.069845   |
| User Reviews  | 0.059009   |
| Release Year  | 0.049465   |
| Country USA   | 0.035205   |
| Duration      | 0.032704   |
| News Articles | 0.030400   |



### Next Steps

Though this analysis was not very fruitful, there are many more data points to explore. A big one is a movies summary and storyline. Using natural language processing (NLP) techniques we can evaluate these descriptions to compare movies with similar premises to see how it correlates with a movies rating.

We can also run linear regression, and classifier models such as KNN or other ensemble methods to see if we are better able to predict a movies rating.



### Final Thoughts

It may be cliché but movies are a form of visual art with many human experiences and emotions contributing to its rating. Although we can gather data on movies, we cannot evaluate this critical part of connectedness to a movie. This makes it difficult to evaluate it's ranking based solely on its attributes. 

As computers get smarter with more powerful processors and algorithms, one day they may be able create movies from scratch as they create art now without any input. When this happens then we may be able to understand contributions to a movies success better than before.