---
layout: single
title: Scraping World Cup Goal Data
author_profile: true
excerpt_separator: "\n---\n"
categories: 
  - R
  - Web scraping
  - World Cup
---

Watching the World Cup over the last couple of weeks has got me wondering several things.  Is the stoppage time determination completely arbitrary?  What is with that magical medical spray? Will Neymar ever walk the same after that [vicious injury](https://www.reddit.com/r/soccer/comments/8vjhtz/neymar_rolling_around_in_pain_after_getting/)?

Several of these questions are difficult to answer, but one potential thing I could investigate is the distribution of when goals are scored during a game.  I'm interested in things like comparing the frequency of goals scored in each half and seeing if there is an increase in goals near the end of the game.  As far as the statistics go, my curiosity could be satisfied with some simple plots so finding the right data is probably the only challenging thing I will need to do.

A quick Google search did not yield the exact data that I needed so I would have to compile everything myself.  This wasn't a deterrence, however, because it presented an excellent opportunity to refresh my web scraping skills in R.  I decided to scrape the data from Wikipedia using the [rvest](https://github.com/hadley/rvest) package in coordination with [SelectorGadget](https://selectorgadget.com/).

Here is the bit of code that got me started:
```R
library(rvest)

years <- seq(1930, 2014, 4)
years <- years[-c(which(years == 1942), which(years == 1942)+1)]
times <- NULL
which.cup <- NULL

for(i in 1:length(years)){
	page <- paste("https://en.wikipedia.org/wiki/", years[i], "_FIFA_World_Cup")
	temp.page <- read_html(page)
	nodes <- html_nodes(temp.page, '.mw-parser-output div td small')
	output <- html_text(nodes)
	times <- c(times, output)
	which.cup <- c(which.cup, rep(years[i], length(output)))
}
```
At this point, things looked good.  I manually checked all the times for the 1930 World Cup and everything matched.  Next, I created a table displaying the total number of goals scored for each World Cup.  Curiously, my results indicated that there were only 17 total goals scored in 1982, and there was no way that was right.  For some reason, starting in the 1970s, the main Wikipedia entries for each World Cup stop displaying the exact times goals are scored during group play.  That means I needed to get additional data from each individual group page to combine with what I already had for the knockout portion of each tournament.  

That was frustrating, but it seemed like the solution would be fairly straightforward.  Unfortunately, here is where things really hit the fan.  So today, World Cup groups are identified by letters, but they were previously numbered.  The size of the tournament has changed over the years so the number of groups has changed as well.  Also, in 1982 there were two rounds of group play!  

This inconsistency in the tournament format meant I had to deal with several special cases.  I ended up using an inelegant "brute force" approach to writing the code for the remaining group stage data, and I am fairly confident that a more efficient solution is out there (possibly using a different data source).  Nevertheless, in the end I had the data I wanted.  The raw data from Wikipedia as well as the R script that I used to obtain the data can be found on Github [here](https://github.com/tylerlewiscook/world-cup).
