---
layout: single
title: Visualizing When World Cup Goals Are Scored
author_profile: true
tags: 
  - R
  - ggplot2
  - World Cup
---

A surprising amount of hard work went into obtaining the simple dataset containing the game times when World Cup goals are scored, and now it is time to see if that work will pay off.  Fortunately, the actual analysis I want to do is very simple and it shouldn't take much effort using `ggplot2`.

Before plotting, a little data cleaning/processing is needed.  The raw data has each time as a string, and stoppage time goals look something like: 45'+2'.  I saw a few optoins for handling the stoppage time goals.  First, I could treat the goal times more like categorical data and just have something like a "45+" group.  This, however, loses a little bit of information since one can't see exactly when the goals are scored.  My next thought was to just add the times together.  So 45'+2' would become 47'.  The problem with this is that a goal scored in the second minute of stoppage time during the first half and a goal scored 2 minutes into the second half would be treated the same.  I ended up going with the latter option and chose to create a new variable to indicate whether a goal was scored in the first half, second half, or extra time.  This would help fix the problem created by adding up the stoppage time goals.  I imagine there are several more ways to address this issue, and some might prove superior to what I chose to do, but this seemed like a good solution given I just wanted to get a quick sense of the what is going on with the data.

Below is the first plot showing a histogram of all of the times when goals are scored:
![Imgur](https://i.imgur.com/IEjNDG6.png)
The distribution does not quite seem to be uniform.  Interestingly, to me it looks like there is a little spike towards the end of the second half.  I sort of expected this as teams might be motivated to press harder to get a goal to win or tie the game.

Next, I created a plot looking at the goal times broken down by "half" (so first, second, or extra time).  Those histograms are below:
![Imgur](https://i.imgur.com/x3FcqjW.png)

Finally, I wanted to see if the distribution looked different for different World Cups.  I created separate histograms for each year in order to investigate that question.  Those series of plots are here:
![Imgur](https://i.imgur.com/tRO9GIR.png)
The distribution seems for uniform in some years, and some years have some unique characteristics.  For example, what happened to boost the scoring right around half time in 1998?

The cleaned data with corresponding code, as well as the code used to create all of the histograms (with a couple bonus plots), can be found on[my Github](https://github.com/tylerlewiscook/world-cup).

P.S. At some point during all of this I found an excellent World Cup dataset on Kaggle that includes some other useful information.  You can check that out [here](https://www.kaggle.com/abecklas/fifa-world-cup).
