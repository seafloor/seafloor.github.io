---
layout: single
title:  "I made a mistake"
date:   2025-02-09
category:
    coding
tags:
  - self-improvement
  - testing
---

I think we've all been there. There's a project to do. It's urgent. There's pressure on your time. Pressure to get results. And there's not just this project - you have a handful of others on the go too. You need to write some code for an analysis, so you do something you wouldn't normally do, something you'd never do without the time pressure, and something you actively advocate others don't do. You skip the unit tests.

I was writing code in R, on a remote workstation, to analyse some data. I suppose it's a slightly unfamiliar environment - I'm using a remote desktop, I'm forced to use a Windows OS, the screen is annoyingly much more blurry than a Mac. None of those are good reasons, of course, but they probably contributed a little to me not using standard procedures. So too did the fact that I thought it was a one-off analysis that I wouldn't return to (I did, causing the issue when new data didn't conform to the old data's standards). Really I made two mistakes. The first was the unit tests - they should have been included from the start, even if I thought I wouldn't re-use the code. The second was harder to catch initially - not handling errors properly (I failed to error when a Null value was returned instead of a vector) - but would have been caught with good enough unit tests. 

It does point to a wider issue where I could spend more time handling edge cases and validating inputs, which likely stems from the fact I'm a researcher, effectively in applied data science, and so I have to balance the time I spend coding with the time I spend reading the literature, writing and presenting results, or analysing and interpreting the data. The coding and initial processing is so foundational, but often valued so little by higher-ups until there's an issue. And I don't think this is fully their issue, but also a failure of me (and other researchers) to communicate what's important and advocate for more time planning and writing initial code for a project.

So yes, I know, I tell Master's students (almost word for word) that the only person you punish is yourself when you skip unit tests. I know that more expansive error handling, *always* doing unit tests, even if it's a quick analysis, and advocating for time spent up-front on code are all things I need to work on more.

If you're wondering about the impact of the error, it's minimal. The association I thought was there is in fact not there, so the main result has gone, but this is pre-publication work where we have other results. The main impact is my frustration at myself for breaking my own rules and my now-renewed commitment to testing!