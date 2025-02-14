---
layout: single
title:  "Immortal Time Bias"
date:   2025-02-13
category:
    statistics
tags:
  - survival-analysis
  - r
  - epidemiology
---

I recently had to overhaul an analysis after I realised a common issue had snuck in. As you can probably guess by the title, that issue was immortal time bias. 

Despite sounding like something out of an anime, the problem is a common issue in medical statistics when looking at the effect of a drug on an outcome. It's particularly prevalent if you have an exposure where you think it reduces risk of a disease. In my case, I was looking at medications which might reduce the later risk of dementia.

Immortal time bias is [outlined nicely](https://www.bmj.com/content/340/bmj.b5087) by Lévesque and Kezouh as a "period of follow-up during which, by design, death or the study outcome cannot occur". The example used by Lévesque and Kezouh is statins and diabetes, but it can occur in a range of situations. 

My example went like this. I had a drug and electronic health records. I was interested to know if exposure to said drug reduced the risk of later dementia. To figure this out, I did something I thought made sense, which was to take everyone who was dementia-free at a given point, and then "follow them up" (in quotation marks because I already had the data, so no waiting needed) to see who did and who didn't get dementia. I would then look at which of them were on the drug beforehand and run a survival model (as `coxph(Surv(dementia, age_exit) ~ drug)`). This all seemed sensible to me.

The issue I'd missed was that individuals in my study could have taken the drug of interest at any point before the starting point. This meant that, of everyone on the drug, I had inadvertently excluded everyone that died or got the outcome (dementia), and so everyone remaining could *only* end up in the study if they never died or got the outcome. They were effectively immortal up until baseline; it was impossible for them to have received the outcome or died, because I had made it so.

The consequence of this is that the drug will always seem more favourable, because there will be a proportion of the dataset who are on the drug and in the sample, must therefore never have received the outcome, making it seem like the drug prevents the outcome, when in reality, I had created that effect by defining the study criteria.

I first spotted this when I noticed the residual plots were skewed for the Cox model (and hence the proportional hazards assumption was violated) and so ran an extended Cox model which accounts for time-varying covariates. Practically, this is done in R with the `survival` package by including the age at which the drug was first taken in the setup for the analysis. This effectively removed the previous assumption that they had taken it sometime between study start and end, and removed the effect.

The ultimate solution wasn't a new model though. I went back and, instead of starting with everyone that was dementia free, I took everyone on the drug. Now instead of reducing the exposure to a binary variable (which had lots of issues, not least that it ignored all the information I had about how many times a person had taken the drug), I only had individuals exposed to the medication, so I calculated cumulative dose exposed and checked its association with dementia status (after a lot more quality control and sanity checks). After expanding this to account for demographic variables and cardiovascular risk factors through propensity scores, the result was that the medication was indeed associated with a decreased risk. 

Though this sounds like a victory (the hypothesised association was there!), I think the real victory was being willing to completely rethink an analysis after spotting a statistical issue, and having the patience to recalculate everything. And hopefully if you come across a similar situation you will now spot it a little earlier than I did.