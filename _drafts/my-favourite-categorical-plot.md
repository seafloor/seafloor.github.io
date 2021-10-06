A while ago I read a paper on effective visualisations that gave me the layout for a plot I use all the time now. I can't even remember the journal or the paper, but I'm pretty sure it was something in psychology or some sort of bioscience. The general ppaper was fine but there was one plot in particular that I just really enjoyed because it was so clean.

The plot itself is just a standard scatterplot, but the difference it that the median is annotated as a horizontal bar, so no additional quantiles and no overlayed scatterinng over boxplots. I think because I was so used to seeing quick and dirty versions of these, even in peer-reviewed papers, that I really wanted to implement the plot in Python (if I remember correctly, they might have given R code for the paper).

*insert plot*

The issue was that no seaborn plot or matplotlib markers covers usinng rectangles! You can use tonnes of other things, just not rectangles. Eventually though I found some code for creating custom markers in matplotlib and I used this as the basis for my plots. 

*add in code for  plot*