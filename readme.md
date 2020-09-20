---
title: "Modeling and prediction for movies"
output: 
  html_document: 
    fig_height: 4
    highlight: pygments
    theme: spacelab
---

## Setup

### Load packages

```{r load-packages, message = FALSE}
library(data.table)
library(purrr)
library(dplyr)
library(tidyr)
library(ggplot2)
```

### Load data

Make sure your data and R Markdown files are in the same directory. When loaded
your data file will be called `movies`. Delete this note when before you submit 
your work. 

```{r load-data}
load("movies.Rdata")

```



* * *

## Part 1: Data

```{r load-data}
movies1 <- data.table(movies)
```

* * *

## Part 2: Research question

can the genre or the runtime of a movie help us to predict its popularity?

firs some exploratory data for the runtime and the popularity with the IMDB raiting score 

```{r load-data}
ggplot(movies1, aes(imdb_rating)) + geom_density(kernel = "gaussian") + 
 theme_bw() + xlab("IMDB Rating")

 ggplot(movies1, aes(runtime)) + geom_density(kernel = "gaussian") + 
 theme_bw() + xlab("Runtime")

```
now we will see the average runtime and IMDB rating by genre

```{r load-data}
movies1[ , lapply(.SD, mean, na.rm = T), .SDcols = c("imdb_rating", "runtime"), by = "genre"]
```
We se some diference between the average  by genre

* * *

## Part 3: Exploratory data analysis


for EDA first i show you the distribution of the runtime and the IMDB rating by genre

```{r load-data}
ggplot(movies1, aes(imdb_rating, genre, col = genre)) + 
 geom_violin(scale = "area" ) +
  theme_minimal()

ggplot(movies1, aes(runtime, genre, col = genre)) + 
 geom_violin(scale = "area" ) +
  theme_minimal()
```
for the IMDB rating the genre with the highest average are the documentary and the comedy movies are the lowest IMDB rating on average,
the runtime of documentary movies its the most variation between the genres and the animation are the genre with less runtime on average

now see the interation between the factors

```{r load-data}
m3 <- with(movies1, lm(imdb_rating ~ runtime + genre))
ggplot(movies1, aes(runtime, imdb_rating, fill = genre, col = genre)) + 
 geom_point() +
  ylim(0, 11) +
  geom_smooth(model = m3) +
   facet_wrap(~ genre) +
    theme_bw()
```



* * *

## Part 4: Modeling


for the models we have two simple models of one variable,  one with  the genre and another model with the runtime
and finally compare the models with a full model with both factors by its r square

```{r load-data}
m1 <- lm(imdb_rating ~ genre, movies1)
m2 <- lm(imdb_rating ~ runtime, movies1)
m3 <- lm(imdb_rating ~ runtime + genre, movies1)
summary(m1)
summary(m2)
summary(m3)
```
the full model have the greatest adjusted r square 

* * *

## Part 5: Prediction


for the conclucion the popularity in in the IMDB raiting depends on the genre of the movie but documentaries genre in general 
have the highest reating on average, and the musicals with a long runtime will have less raiting

```{r load-data}
#load("movies.Rdata")
```
* * *

## Part 6: Conclusion

the genre and the runtime of a mive can help us to predict the IMDB raiting of a movie
