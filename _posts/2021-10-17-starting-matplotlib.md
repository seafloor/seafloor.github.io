---
layout: single
title:  "Starting matplotlib"
date:   2021-10-17
category:
    visualisation
tags:
    matplotlib seaborn 
---

# What I do now
I waste a lot of time rewriting boilerplate in matplotlib. I don't think that's a particularly bad thing as it helps me remember all the basics pretty well. I'm also pretty fast now so, and there's basically no thinking involved.

Actually maybe it's not so true that it helps me - there a few things I always look up. First it's removing the legend. Sometimes I need to iterate through subplots and dropped all the legends but on. Sometimes I need to clear a subplots so as there's a spare square in a grid and I want to put the legend somewhere. And sometimes I want to get real fancy with colours. I think I always look these things up - though I can blunder through colours and I'm getting better at remembering removing the legend. Is it `ax.get_legend().remove()`? It is!!! It is!!! I looked it up and it is! 

# What I should do
Okay well done me, but my point is that there's still stuff I faff around over and so here I'm going to dump a decent matplotlib boilerplate that covers some of my most common things so I have a single place they all live. I don't really think it's worth make repo for as it's just not that much, but it would be nice to also have something to point to when people send me mpl questions too!

I should note finally that I'm pulling some bits of this out of quite old code, so while most of it's just me or from the docs, it's quite possible that at some point specific bits have come from other sites or stackoverflow and I've lost the reference over time.

I'm going to assume that some sort of catplot, so categorical on one axis and continuous on the other. I'm also going to stick with plotting something like AUC for different classifiers, because that's my most common use case.

First, the common imports I need to write:

```python
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
import matplotlib as mpl
import matplotlib.ticker as plticker
from matplotlib.patches import Rectangle
from matplotlib.collections import PatchCollection
import seaborn as sns
```


I know there's some redundancy there, but it's what I *do* use, not necessarily what I *should* use. Next up is the vanilla catplot:

```python
fig, ax = plt.subplots(figsize=(14, 7))
labs = data.groupby('Predictors').median()['AUC']
ylims = data.groupby('Predictors').max()['AUC']
    
pal = [sns.color_palette()[i] for i in (0, 4, 9)]  # choose specific colours from palette
sns.boxenplot(x='Predictors', y='AUC', showfliers=False, palette=pal, dodge=False,
              hue='Data Type', data=df)
    
# annotate mean scores 
_ = [ax.text(x=i, y=l+0.02, s='{:.2f}'.format(a), ha='center', fontsize=12) for i, (a, l) in enumerate(zip(labs, ylims))]
    
# stick the legend outside the plot
ax.legend(bbox_to_anchor=(1, 1), frameon=False, title='Data Type', fontsize=14, title_fontsize=16)
    
plt.xticks(rotation=45)
ax.set_xlabel('Predictors', fontsize=18)
ax.set_ylabel('AUC', fontsize=18)
    
# here because I ALWAYS forgot how to rotate tick labels without calling
# ax.set_yticklabels()
plt.xticks(fontsize=12, rotation=15) 
plt.yticks(fontsize=12, rotation=0)
ax.axhline(y=0.5, linestyle='--', color='grey', alpha=0.6)
sns.despine()
```


It doesn't get much more simple than that, but look at all that boilerplate just to get it to look reasonable! I love mpl's flexibility but it is a bit much. Lots would say swap to seaborn, and it helps, but I always need to drop down to mpl for a publication-quality plot. Always.

I'll write some more posts in the coming weeks with more code for common plots that's ready to go (for your benefit, but mainly for future me!)