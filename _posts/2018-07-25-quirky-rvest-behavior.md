---
layout: single
title: Quirky rvest Behavior
author_profile: true
tags: 
  - R
  - rvest
  - Web Scraping
---
I've recently started a new project which involves scraping lyrics from
the web. As before with the World Cup data, I turned to `rvest` to
obtain the data. However, this time I noticed some weird behavior with
the results.

Here is what happened:

    library(rvest)

    ## Loading required package: xml2

    ppm <- read_html("http://lyrics.wikia.com/wiki/The_Beatles:Please_Please_Me")

    ppm %>% html_node('.lyricbox') %>% html_text()

    ## [1] "Last night I said these words to my girlI know you never even try, girlCome on (come on), come on (come on)Come on (come on), come on (come on)Please please me, whoa, yeahLike I please youYou don't need me to show the way, loveWhy do I always have to say \"love\"?Come on (come on), come on (come on)Come on (come on), come on (come on)Please please me, whoa, yeahLike I please youI don't wanna sound complainingBut you know there's always rain in my heart (in my heart)I do all the pleasing with youIt's so hard to reason With you, whoa yeahWhy do you make me blue?Last night I said these words to my girlI know you never even try, girlCome on (come on), come on (come on)Come on (come on), come on (come on)Please please me, whoa, yeahLike I please you(Please) me, whoa, yeahLike I please you(Please) me, whoa, yeahLike I please you\n"

As you can see, some of the words are concatenated. This will cause
serious problems for a text analysis since "girlI" is not an actual
word. After going back to inspect the source, it seems like this occurs
whenever there is a new line.

I did some Googling to see if there was an easy way to handle this.
Apparently, a few other people have encountered this same issue and
posted about it on [Github](https://github.com/hadley/rvest/issues/175),
but there doesn't appear to be a fix posted as well.

It seems like this should be a simple matter to fix, but handling
strings is one of my weaker programming skills. Therefore, I turned to
the r/rstats subreddit for help, and fortunately
[u/Schrodingers-Human](https://www.reddit.com/r/rstats/comments/91ap8q/line_breaks_with_rvest/e2xjmfk)
posted a nice solution that seems to work!

Here is my slightly modified solution taken from Schrodingers-Human:

    library(rvest)
    library(stringr)

    ppm <- read_html("http://lyrics.wikia.com/wiki/The_Beatles:Please_Please_Me")

    ppm %>%
        html_node('.lyricbox') %>%
        as.character() %>%
        str_sub(start=23, end=-39) %>%
        str_replace_all("<br>", " ")

    ## [1] "Last night I said these words to my girl I know you never even try, girl Come on (come on), come on (come on) Come on (come on), come on (come on) Please please me, whoa, yeah Like I please you  You don't need me to show the way, love Why do I always have to say \"love\"? Come on (come on), come on (come on) Come on (come on), come on (come on) Please please me, whoa, yeah Like I please you  I don't wanna sound complaining But you know there's always rain in my heart (in my heart) I do all the pleasing with you It's so hard to reason With you, whoa yeah Why do you make me blue?  Last night I said these words to my girl I know you never even try, girl Come on (come on), come on (come on) Come on (come on), come on (come on) Please please me, whoa, yeah Like I please you (Please) me, whoa, yeah Like I please you (Please) me, whoa, yeah Like I please you"

Looks much better.
