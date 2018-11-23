---
layout: single
title: Fantasy Football Player Rankings
author_profile: true
tags: 
  - R
  - Fantasy Football
---

It's Thanksgiving, and for many, that means sitting around with the family watching football after eating lots of turkey.  It also means the fantasy football season is just a few weeks away from entering the playoffs.  In previous years, that wouldn't be something that I had much interest in because my fantasy teams have been terrible.  This year, however, I am doing...mediocre.  Despite the fact that I am currently poised to make the playoffs for once, I don't feel like my draft or management styles have changed any since finishing last and then second to last in the two prior seasons.  I also feel like I had drafted my teams the "right" way according to the experts, and that really made me wonder about the accuracy of fantasy football preseason player rankings.  

With that in mind, at the start of this fantasy football season I decided to take a look at how good of a job the professional prognosticators do a predicting fantasy football performance.  I found ESPN's 2017 preseason fantasy player rankings which included opinions from five of their experts.  I elected to use the PPR rankings since our league started to use that scoring system this year.  For results and the end of the season, I took data from FantasyPros, instead of ESPN, because it was easier to access their data and the points and rankings seemed to match on both sites.

I had to perform a little data cleaning which mostly centered around how the sites handled apostrophes in names differently.  I also decided to remove defneses and players who did not play in five or more games becuase I do not think that anyone's rankings should be punished by injuries.  The same should be said for all the poor people who drafted David Johnson with the #1 overall pick last year (like me).

Once that was done, I was ready to proceed with the analysis.  I mainly wanted to examine the correlation between preseason and end-of-season player rankings.  In order to do that, I calculated Spearman's correlation and also created a scatterplot to visualize any relationship.  The correlation between the two ranks was about 0.38, and you can see what the scatterplot looked like below.  Note that I switched up both axes so higher ranked and better performing players should be in the top right.

![Imgur](https://i.imgur.com/HIVgi3T.png)

The correlation is not very strong, and the scatterplot seems to back that up.  There are a number of players who were significantly under-ranked as well as several over-ranked (some due to injury and some not).  I think the main takeaway is that projecting individual performance in a sport like football is hard, and as an extremely unsuccessful fantasy owner, that makes me feel a little better.

As usual, the code and data can be found on [my Github](https://github.com/tylerlewiscook/fantasy-football).
