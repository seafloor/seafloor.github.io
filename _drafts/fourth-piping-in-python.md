# Pandorable code
One of my favourite series of posts are [those by Tom Ausberger on modern Pandas](https://tomaugspurger.github.io/modern-1-intro.html). As for many other Python  enthusiasts, it completely transformed how I think about writing Pandas code. Now I mostly try to write everything as Pandorable. If you're not familiar, go check out the posts now, but pandorable code is that which looks a little like code from using the pipe function, `%>%`, in the tidyverse. You put your dataset name first and chain a series of methods together using dot notation. In R we would use the pipe and have a nice indented code block that makes it clear what has been applied to what data. In Python we can't just casually indent because *indentation matters in Python*, so we can use brackets to chain methods over new lines.

In practice, this means code that looks like this:
§
    df = df.melt()
    g = df.groupby('Classifiers')
    g = g.agg('mean')

becomes this:

    g = (df.melt()
           .groupby('Classifiers')
           .agg('mean'))

Now the dataset that's the focus of this chunk of code comes first, and each line is the next method we're going to apply. We also line each of the methods so it's nice and readable.

# The methods you're missing
Qeury annd assign.

# What about plotting?
Talk about passing straight to Pandas plotting function

# The pipe
Talk about piping into anything, but especially seaborn, and maybe even altair. Cover the special cases tom doesn't mention?

# Should all code be pandorable?
When I teach the tidyverse, I teach the pipe function from maggritr because I think it's excellent. But I also always ask students when they should avoid it.

The answer I give is that there are a few cases where you might want to avoid the pipe in R or pandorable code in Python.


There's also other niche cases where you might avoid it, for example if the code is for someone that has no clue what method chaining is then it might be a bit confusing, etc. 