---
layout: single
title:  "0 - A brief introduction to R"
date:   2023-10-16
toc: true
toc_label: "Contents"
toc_sticky: true
category:
    r
tags:
    r
---

Welcome to the first post in a series introducing R to new users and getting you up-to-speed with modern R, centred around use of the tidyverse. If you have used R before and are comfortable using R studio and doing basic mathematical operations then I suggest you skip ahead to the next post. We'll cover basics like assigning values to a variable here, so if that's old news then please move ahead.

I've also set this as "post 0" because it essentially ignores the tidy verse in favour or introducing R itself and how to run things. To some it may seems sacrilegious to not begin with the tidyverse, and if that's you then please also skip ahead to the next post.

In this first post, I'll cover:

- an introduction to R, including installing and using R studio, and how to get help
- how to load and use packages
- a brief overview of the main data types in R

# Background

## A history of S
I'm assuming that if you're here to learn R, you already know what it is. For completeness, and for those of you entirely new to data science, R is a statistical programming language that allows for a lot of general purpose programming. This differs from something like Python, which is a general purpose programming language that has a lot of statistical packages.

You may already know that R is based on the S language. S stood likely stood for 'statistics', as this was the common letter/word in all the acronyms on offer, when it was invented in the 1970s. This happened at the famed Bell labs, where creations like UNIX, the C language and the transistor were born. People usually refer principally to John Chambers for writing S as the main creator, but also had contributions from Rick Becker, Doug Dunn, Jean McRae, and Judy Schilling.

It's useful to know about S because it explains a lot about R's behaviour. Over the years, lots of changes were made to S, with more OOP support brought in during the 1990s. Its creation was hugely significant, and replaced earlier use of Fortran for such analyses (though I still know people that write some statistical analyses in Fortran), but ultimately its direct impact on the world was to be overshadowed. S was sold as a commercial language, but its successor, R was open source, meaning anyone could use it for free.

## S to R


# Setup

## Installation

Well, first to the obvious - we need to install R. R can be found [here](https://www.r-project.org/). The easiest thing to do is to head over to [Posit](https://posit.co/download/rstudio-desktop/) which has links for the latest R and RStudio. You should install them both.

## R for Data Science

We'll be referring to the latest version of the [R for Data Science](https://r4ds.hadley.nz) book. This is now in its second edition. If you've covered R previously and are here for a refresher, you might be familiar with the original book. The key updates to this are that modelling is now elsewhere (both it and ggplot2 are better-covered in separate books), some updates to function names, and the pushing of Quarto over Rmarkdown. Rmarkdown is not going anywhere and is still very much supported in RStudio, but if you want to use Quarto for presenting, it's certainly an option.

To note - the chapter to refer to for extra reading are chapters 1, 3, 5 and 9. These will go over the same basics we cover here, but from a different (and significantly more well-polished) perspective.

# The basics

## The console

If you're completely new to R then you can open wither the R console or Studio itself. It's good to know what the R console looks like, but practically all of our work will take place in RStudio. It's an integrated development environment that gives us a lot of extra features when working in R. Alternatively you could work in Jupyter - this allows you to use different kernels so that you can swap between R and Python, but I won't focus on it here. Instead I'll assume you're working in RStudio from now on.

# insert picture

Once you have RStudio loaded, you can see the console and the prompt at the bottom left. The prompt begins with a ">", after which you can type the code you want. If it hangs (doesn't return anything or execute anything) it may be waiting for more input, like closing a bracket you opened. You can cancel a command with the `esc` key or `ctrl+c`. In some cases it may hang for a while before cancelling a command. This is likely because it's running something in C under the hood and has to wait for the code to return in R to be cancelled.

## Maths

As you would expect from a statistical programming language, we can do basic mathematical operations easily in R, e.g. `2 + 2`. You can of course chain these together, e.g. 

```
# plain
2 + 2 * 4 / 16**3

# with parentheses for clarity
2 + (2 * 4) / *16**3)
```

Sometimes the output from this will be an extremely large or small number. In that case we can turn on/off scientific notation (1e-5 vs. 0.00001) in our outputs with:

```
options(scipen = 999) # turns off scientific notation
options(scipen = 0) # turns it back on
```

`options(scipen) is slightly odd in that you need to use a more positive number to drop scientific notation and a more negative number to increase it, rather than a simple on/off option. The main issue here though is that you are setting the output globally, which you usually don't want. To instead format a single value/output for be in scientific notation or not, use `format(x, scientific = T)`, where we can change the variable x by passing T or F to scientific.

Beyond the basics, we have more that can be done by using the *many* functions built in to base R, including (but not limited to):

- `sin()`, `cos()`, `tan()`
- `exp()`, `log()`, `log10()`
- `abs()`, `sqrt()`, `round()`

Note that the log is a natural log by default, and we can use other options like `log2()`. Also note that round will actually intuitively, but some people may expect or want .5 values to round up - R rounds them down by default. 

# Beyond the basics

This is a section I've ambitiously placed here but you'll probably circle back to as you need to look things up or saving code for later. There's always some uncertainty about where to put information like this. Some students like to know to lay of the land and all the general information before they're comfortable diving-in, while other want to get started with some analysis and nice graphics before filling in missing details as-needed. Luckily this isn't a live session so you can always circle back if that works best for you.

## Scripts

Now use the console to do basic mathematical operations, it's time to start also recording what we write. Using the top left panel in RStudio we can create scripts. In these we can write code to be save for later in a ".R" file. When writing these down it's good to also comment what we've done. The hash symbol (#) can be used to create comments - everything after this is ignored by R. Four hashtags in a row will allow you to fold over code blocks in R to make long documents more readable.

## Getting help

We can get help with the `?` or `??` notation. Type them into the console followed by a function name and press enter to search for help. A single question mark will bring up the vignette or docs for any matching function. The double question mark allows for searching and partial matching and will give you more options if nothing shows up with `?`. 

More likely though is that you will google for what you need. When doing so, good resources include R-bloggers, stackoverflow, crossvalidated, and textbooks like the R cookbook. Most R resources are available for free!

## Style

I won't go over style extensively here, but please follow [the tidyverse style guide](https://style.tidyverse.org/). Google have their own one too, but it's just a fork of the tidy verse one, with some minor changes thrown in.

## Installing packages
Packages can be installed simply by typing `install.packages('tidyverse')`, where the package name you want can be put in place of 'tidyverse'. You need the quotation marks here, and you can optionally select a mirror (Bristol/London for the UK) to download from.

Loading installed packages can be done graphically using the "packages" tab on the bottom right panel. Here you can see all your installed packages, and load them in the current R session by ticking them. Alternatively you can type `library(tidyverse)` into the console, this time without quotations marks.

## Your working directory
R needs to load/save data from/to a location, so it has one by default. You can check this with `getwd()` (for get working directory). Changing it is as easy as `setwd("/new/path/")`, where you will need to change the slashes to be OS appropriate for Mac/Windows/Linux and you can set a relative or full path. Note it's also easy to brows graphically again in RStudio using the "Files" tab on the bottom right panel.

# Data types

We can do operations and check for help, but so far it's just been interactive. Saving variables allows us to interact with them later and perform more complex operations. In R we use `<-` to assign a value to a variable, e.g.

```
x <- 3
y <- 4
z <- x * y
```

When we create these variables, how is it stored? R has a few basic types:

- Double (numeric)
- Integer
- Character
- Logical
- Complex

These are the potential classes of values we enter. For example, `"thing"` will be of type character. We can check this using `typeof(x)`.

# Data structures

A data structure is a specialised format or pattern that allows for efficient organising and retrieval of information. Using more specialised data structures usually means we put more work in now to create everything and get better organisation and retrieval later. Here we'll briefly cover vectors, lists, matrices and data frames.

## Vectors
 
These are arrays of values which are all the same time. We create arrays using the concatenate function (abbreviated to "c"), as so:

```
v <- c(1, 2, 3, 4, 5)
```

Here we created a vector of length 5. To get a specific element of the vector, we type `v[1]`, using the square bracket notation for indexing. R is 1-indexed, so `v[1]` really does return the first element (unlike in many other languages, like Python). We can make vectors of characters, or boolean arrays, as long as all the elements are the same type. 

### A note on numeric vectors

Operations on numeric vectors are done element-wise by default. So we can do the following:

```
x <- c(1, 2, 3, 4)
y <- c(4, 3, 2, 1)
x + y
# return a vector c(5, 5, 5, 5)
```

where the elements at the same index have been added separated and the results concatenated into a new vector.

Indexing the vector can go beyond just `v[2]` or the second element or `v[-1]` for the last. We can slice or subset as so:

```
v[1:3] # slices inclusively, i.e. elements 1, 2 and 3 are extracted
v[c(1, 4)] # only extracted elements 1 and 4 as a new vector
```

### A note on boolean (logical) vectors

We can use the `&` and `|` symbols to perform logical AND and OR operations. These are again done element-wise, so 

```
c(TRUE, TRUE) & c(TRUE, FALSE)
```

Will return `c(TRUE, FALSE)`, as the corresponding elements of the two vectors were compared, and the first in vector vector evaluated to TRUE, while at the second element in the second vector there was a FALSE, meaning it's not the case that TRUE and FALSE are both TRUE, so it evaluates to FALSE.

Logical reasoning takes a little getting used to, but playing with the console and different operations is really the easiest way to get a handle on things. 

There are also the operators `&&` and `||`. These almost seem there trip up learners as they're less frequently used but are easy to see/type. They are short-circuiting logical operators, meaning they only compare the first element of each vector, assume the same is true for the rest and return that first answer. Usually, you want a vanilla `&` or `|`.

## Lists
If you want different types of objects stored together in R, then lists are your guy. We can create them as below, and indexing them using a double square bracket notation.

```
l <- list(5, 'things', c(1, 2, 3), mean)

# indexing lists
l[[1]]
```

Lists aren't used as frequently as vectors, but they're still useful for putting different data types together in a single object. One example of this could be return multiple objects from a function. As R only returns a single object, we can make that object a list in order to return multiple outputs. This isn't necessarily a good thing, but it is something you'll see "in the wild".

## Matrices
Moving on and up, we have matrices. A matrix is what you would expect if you have done some linear algebra - a 2-d array of numbers. We can actually create matrices of other data types, such as characters. You can also add row and column names, but really the bulk of the use of matrices is for mathematical operations on 2-d arrays of numbers. Most of this probably happens under the hood in functions you already you. 

The key thing to know is that matrices need to be a single data type, and that they can be created using

```
m <- matrix(c(1, 2, 3, 4, 5, 6, 7, 8, 9), nrow=3, ncol=3, byrow=TRUE)
```

Note that here we've filled the matrix from a vector. The byrow argument is important - the default (FALSE) will instead fill in a column-wise manner, while here it's done row-wise.

## Data Frames

Finally we have data frames. This is most important data structure for data science. Like matrices they have a tabular 2-d structure, but unlike matrices they can have columns with different types. This means we can store different types of information in one table and use it plotting, analysis etc.

As with other objects, we have a specialised function for initialising data frames:

```
d <- data.frame("location" = c('roath', 'pontcanna', 'canton', 'splott', 'llandaf'), "price" = c(1400, 1500, 1300, 1100, 1600))

# access a single element
d[4, 1]

# accessing a single column
d$price

# subsetting a data frame
d[d$location %in% c('roath', 'canton', 'splott') & d$price < 1400, ]

```

Here we can see that the square bracket notation can be used to subset a data frame. When we do this to filter the rows, what we're actually doing is passing a logical vector of TRUE/FALSE, which come from the condition we use. This allows us to select rows that meet our criteria. And when we combine conditions using `&` or `|`, we use the same element-wise evaluation of logical operators that we discussed earlier.

We also want to take basic looks at our data, so we can use functions such as:

```
head(d)

tail(d)

summary(d)

str(d)
```

And much more if we move beyond base R!

We'll pick up from here by focussing more on the packages available in the tidyverse in the next lesson.
