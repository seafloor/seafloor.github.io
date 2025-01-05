---
layout: single
title:  "Just stick it in a YAML"
date:   2025-01-04
category:
    HPC
---

## The problem with parameters

I remember the early years of my PhD when I was first running lots of comparisons of lots of models. I would have so many models, so many parameters. There wasn't just the tuning for models (mostly drawing from distributions for random search in cross-validation, but sometimes enumerated grids too), but also the huge range of simulations I ran. There are always other questions, another thing that can be changed, another question to answer. 

Does this effect I'm seeing level off when I saturate sample size? Is it driven by main effects? What if I introduce more realistic correlation between predictors? What if I degrade the signal to show poor genotyping quality (or poor predictor measurement in general)? I can see that there's an issue with *p >> n*, but is it really a fundamental issue with the model, or just a search problem in hyperparameter tuning? (*spoiler:* it's normally a search problem. There's a new shiny paper showing that they get 1% more AUC with their model? It's a minor change in optimisation. There's a big difference? They probably know how to tune their own model, or even developed it by testing on the same data, much better than the other one they didn't develop. Not that I'm pessimistic...). 

So I had a lot of parameters to change. I dealt with this a in bunch of ways over the years: pull parameters to the top of a script to change, pass them as command line arguments with argparse or bash args, wrap it in a script where all the parameters go, set them as defaults in a function, write an exceptionally long bash script that takes named args (--like=this) etc. etc. 

## The solution: stick it in a YAML

I was young and dumb. But at least I'm older now. And if I could say one thing to my younger self it would be to carry on doing sports because being a bioinformatician is terrible for your back and you're not getting any younger. Oh, and stick it in a YAML.

If you're up to speed with things you'll be familiar with YAML files, and I was too for a long time before using them for tracking all the different parameters that could change with different runs (what folk in ML and data science like to call "experiments", something that has never sat right with me as someone that used to work in a wet lab, but for the life of me I can't think why). But I wasn't using them myself for data science related projects, so I'm here to give you a push.

## But what *is* a YAML?

First, if you're not in-the-know, then just understand that it's a mark-up language (a recursive acronym for "YAML Ain’t Markup Language", but which originally appeared as "Yet Another Markup Language" before changing into the modern acronym) that's designed to be standardised and more readable for humans than something like XML or JSON by using identation for structure. Scroll down to the latest specs [here](https://yaml.org/). To show how useful it is, just take a look at the XML below I pulled from a random google page:

```
<note>
<to>Tove</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>
```

And here look at the equivalent YAML (converted [using this tool](https://www.site24x7.com/tools/xml-to-yaml.html)):

```
note:
  to: Tove
  from: Jani
  heading: Reminder
  body: Don't forget me this weekend!
```

Now that's elegant. Easily readable and changeable. Now imagine a run with many different settings, but you never have to change your code for a new run. You only ever update the YAML. And it looks something like:

```
run:
	seed: 123
	train_test_split: 0.75
	
simulations:
	n: 10000 # sample size
	p: 100 # predictors
	m: 10 # n causal predictors
	model: additive # relationship between predictors

tuning:
	gradient_boosted_trees:
		lr_min: 0.00001 # alpha drawn from log normal dist
		lr_max: 0.001
		n_trees: 1000
	random_forest:
		n_trees: 500
		min_samples_per_split: 5
```

Or something like that.

## Why use a YAML?

You can stick all your requirements for a model in one place and then you don't have to mess with the code. This also helps with:

- logging - a copy of the YAML can be saved with each run
- integration - workflow tools like snakemake often use yaml files
- sharing - others can read something like this much faster
- mistakes - easy to spot in a nice readable way
- a clean repo - you don't need to push every small change to github to avoid merge conflicts just because you ran a new "experiment"

and probably a lot more that I missed! 

## What about TOML?

That's my whole spiel. It's worth noting that it doesn't *have* to be a YAML. It could be a [TOML](https://toml.io/en/) (Tom's Obvious Minimal Language) file. With TOMLs, the YAML above would look like:

```
[run]
seed = 123
train_test_split = 0.75

[simulations]
n = 10000
p = 100
m = 10
model = "additive"

[[tuning]]
[tuning.gradient_boosted_trees]
lr_min = 0.00001
lr_max = 0.001
n_trees = 1000

[tuning.random_forest]
n_trees = 500
min_samples_per_split = 5
```

I love TOML the mostest, and sometimes I think it's even more readable than YAML. But aside from having used it for a few projects, it's not quite as widely supported by third party tools (though it's increasingly popular in python especially, e.g. with [pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/) for packaging) which often require a config to be given as a YAML, so I've fallen to using YAMLs by default too. 

## Conclusion

Nonetheless, the point stands. Whether you choose YAML or TOML, the principle is the same: if you have too many arguments or parameters to manage, put them in a structured configuration file. It’s cleaner, more maintainable, and easier to share. So, are you overwhelmed with too many parameters? Just stick it in a YAML. Or a TOML.