---
layout: single
title: Sgt. Pepper's Word Cloud
author_profile: true
tags: 
  - R
  - Word cloud
  - Text mining
  - The Beatles
---

As my last post hinted at, I've been working on scraping Beatles lyrics from the web.  I was never really able to fully automate the process since the links didn't have an easily repeatable pattern - the lyrics for each song were on a page with that song in the address.  Nevertheless, I was able to complete the collection of lyrics from the "core" albums without too much hassle.  Everything can be found in my [beatles-lyrics](https://github.com/tylerlewiscook/beatles-lyrics) repo on Github.

I have several analyses planned using this data.  To get things started, I thought I'd create a simple word cloud for *Sgt. Pepper's Lonely Hearts Club Band*.  The code is really simple and I'm using the `tm` and `wordcloud` packages.  I have the songs for each album in their own folder, and all of the album folders are in a folder called "Beatles" that is in my usual R working directory.

First, load the packages and set the working directory:
```
library(tm)
library(wordcloud)

setwd("Beatles/")
```
Next, load the data and create a corpus from the *Sgt. Pepper's* lyrics before doing some basic processing:
```
sgtp <- VCorpus(DirSource("SgtPeppers/"))

sgtp <- tm_map(sgtp, removePunctuation)
sgtp <- tm_map(sgtp, content_transformer(tolower))
sgtp <- tm_map(sgtp, removeNumbers)
sgtp <- tm_map(sgtp, stripWhitespace)
sgtp <- tm_map(sgtp, removeWords, stopwords("english")) 
```
I did some transformations like: removing punctuation, removing any numbers, and converting to lower case.  Importantly, I also removed "stop words" (words like "the"), but I did not do any stemming.

From there, I created the document-term matix, and only a few more lines of code were needed to produce the word cloud.  Note that I am only including words with a minimum frequency of five.
```
dtm <- DocumentTermMatrix(sgtp)

freq <- colSums(as.matrix(dtm))

set.seed(1)
wordcloud(names(freq), freq, min.freq = 5, colors = brewer.pal(3, "Accent"))
```
The final result can be seen [here](https://i.imgur.com/oVXNNpZ.png):
![Imgur](https://i.imgur.com/oVXNNpZ.png)

There should be some more cool stuff coming from this. I know I want to do a sentiment analysis, and I also plan on trying out the `tidytext` package.  Also, I already have at least one Shiny app planned for this data.  Stay tuned!
