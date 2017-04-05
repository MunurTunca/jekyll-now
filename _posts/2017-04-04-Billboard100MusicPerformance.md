---
layout: post
title: Billboard 100 Music Performance
---

There are many reasons why a song may be on the Billboard 100 from popularity of the artist to the sound of the music.


Today it is harder than ever to keep track of the reasons behind a song’s rise to the top 100. With streaming services like Spotify, Pandora, Google Music, etc. increasing in popularity, the process has gotten harder. 


What if there was a better way to predict a songs success based on its attributes? This is what we will be looking at using songs that entered Billboard 100 in the year 2000. You can download the data used by clicking [here](/files/billboard.csv).


As a standard for using secondary data (data that was not collected by me), I will need to make a set of assumptions which comes with the risk of inaccurate analysis.


I will be making the following assumptions about this data:
1. The data is accurate.
2. The weeks are counted beginning the first day that the song entered the Billboard 100
3. Lengths of the songs are displayed as minutes, seconds, milliseconds, -- 
   * __Example__: 3,38,00 AM would mean that the song is 3 minutes 38 seconds long
4. The rank of the song on any given week is correct.
5. An asterisk means that the song did not chart for that week


This is what the data looks like. There are columns for the year (2000), name of the artist (last name first), name of song, length of song, genre, date entered Billboard 100, date peaked (highest charting), and the weeks with song rank (lower is better).

year | artist.inverted | track | time | genre | date.entered | date.peaked | x1st.week | x2nd.week | x3rd.week | ... | x75th.week | x76th.week 
--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- 
2000 | Destiny's Child | Independent Women Part I | 3,38,00 AM | Rock | September 23, 2000 | November 18, 2000 | 78 | 63 | 49 | ... | * | * 
2000 | Santana | Maria, Maria | 4,18,00 AM | Rock | February 12, 2000 | April 8, 2000 15 | 8 | 6 | ... | * | * 
2000 | Savage Garden | I Knew I Loved You | 4,07,00 AM | Rock | October 23, 1999 | January 29, 2000 | 71 | 48 | 43 | ... | * | * 
2000 | Madonna | Music | 3,45,00 AM | Rock | August 12, 2000 | September 16, 2000 | 41 | 23 | 18 | ... | * | * 
2000 | Aguilera, Christina | Come On Over Baby (All I Want Is You) | 3,38,00 AM | Rock | August 5, 2000 | October 14, 2000 | 57 | 47 | 45 | ... | * | * 

The first thing that peaked my curiosity was the song length. Knowing that the songs that chart on the Billboard 100 are often played on the radio, I concluded that songs that are shorter in length would be more popular. With that in mind I decided to explore this part of the data.


My hypothesis was that if a song is shorter than 4 minutes, it will remain on the billboard 100 for a longer time than a song that is 4 minutes or longer.


This will make the following my null and alternate hypothesis.
H0 = There is no relationship between song length and number of weeks on Billboard
H1 = There is a negative relationship between song length and number of weeks on Billboard

## Visualizing the data

Looking at histograms of how many weeks songs stayed on the billboard, we can see that many of them only stayed for 20 weeks. Both for songs under and over 4 minutes in length.


Songs that are four minutes or less by week

![four_less](/images/song_four_min_less_by_week.png)


Songs that are four minutes or more by week

![four_more](/images/song_four_min_more_by_week.png)


The songs had a nice distribution, many being between the 3-4 minute mark.

![songs_week](/images/all_songs.png)


The following scatterplot (with a regression line) demonstrates the relationship between song length and weeks on Billboard. It is easy to see that there is no relationship between these two variables.

![songs_week](/images/song_len_weeks_scatter.png)


## Statistical Testing


One thing that could quickly confirm what our scatterplot showed was doing a t-test and looking at the p-value.


__T statistic__: 0.0489501719137

__P-value__: 0.461026146868


With a P-Value of __46.1%__ we can safely say that there is no relationship between the length of a song and its prolonged success. As a result, we have failed to reject our null hypothesis.


## One more look


Before giving up on song length, I wanted to look at another relationship. I chose to look at how successful a song performed, it’s best rank on the Billboard 100, within all the weeks it was on the chart.


I created another scatterplot and just like before, there doesn’t seem to be any relationship between dong length and its popularity.

![songs_week](/images/song_length_position.png)


## Wrap Up


Our visualizations and statistical tests show that there is no relationship between song length and number of weeks a song charts or how far it climbs on the Billboard 100. As a result, we failed to reject our null (H0) hypothesis.
There are many other angles we can look at this problem in the future. 


Some examples include;
1. relationship between genre and best position on the chart,
2. relationship between time of year and position on the chart,
3. relationship between genre and length of time on the Billboard 100


Please leave any comments or questions you may have below!
