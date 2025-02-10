---
layout: single
title:  "SQL Teething Problems"
date:   2025-02-10
category:
    coding
tags:
  - sql
  - data-engineering
  - python
---

I love-hate SQL. Yes, it's one of those posts.

When I first discovered SQL and used it to extract data from a few million records and export for analysis in a matter of seconds, I thought it was incredible. Fast. Efficient. I loved the logical syntax (I'd been using pandas and dplyr for over a decade at this point, and teaching the dplyr verbs for 5 of those, so the jump in syntax was minimal). It was the honeymoon period and everything was good.

It even reached new heights with optimisation. Everyone loves optimising a data pipeline (right?!) and I was no different. I remember first learning CTEs (common table expressions), which are simple `WITH` blocks that logically separate out blocks of computation, and using them so much that everything became really slow because it was the first bit of actual logical separation in what is otherwise fairly ugly and flat code. Then realising that the reason memory was blowing up and plans were taking so long to execute was because the entire CTE had to be computed and held in-memory, and that separating it out like this, unlike for things like subqueries, meant it wasn't subject to the engine's (here db2) optimisations. 

```
# a CTE
WITH exposed AS (
   SELECT DISTINCT ID
   FROM medications_table
   WHERE drug_code LIKE 'b21%'
)
SELECT * 
FROM exposed t1
INNER JOIN outcome_table t2
  ON t1.ID = t2.ID;
  
# a more efficient subquery
SELECT * 
FROM exposed 
WHERE EXISTS (
   SELECT DISTINCT ID
   FROM medications_table
   WHERE drug_code LIKE 'b21%'
);

# note - I haven't tested this (all my fun SQL tables and code are locked away on private servers which hold medical data)
# but it fits the general pattern than subqueries are open to being optimised,
# and the whole subquery isn't forced to compute in one chunk beforehand
```

But issues began to pile-up. There are the small things like inconsistency across the different flavours of SQL. One version has ADD_YEARS, another doesn't. You find the perfect solution to your SQL problem and then discover it's not in that version of DB2, or things like storing functions have been disabled by the admins. Or maybe it's just plain unclear how missing values are handled and getting to the right docs is a pain.

Then you seem to hit some sort of ceiling on easily-accessible knowledge. It's not one that should exist this early on. I remember seeing an article for advanced SQL knowledge and being excited I was going to learn all these new things, only to find the most advanced material it briefly covered were CTEs and window functions. It's true for all tech topics that are in any way popular (like "advanced machine learning" courses) that they become increasingly shallow as the audience gets wider. Don't even get me started on deep dives. I've never seen a deep dive that went more than a few millimeters below the surface. And this isn't a humblebrag, *I'm-so-skilled-isn't-life-hard* moment, but a genuine complaint that I'm not that skilled at SQL and yet most learning materials that get banded about aren't that useful already.

Finally, the straw that broke the camel's back was iteration. Is it so hard to ask for easy iteration? I shudder to think about how many mistakes have been introduced somewhere because someone wrote in pure SQL, where iteration and modularisation just are't anywhere near as easy or intuitive as in other languages like Python.

So here I am, at the crossroads. For a while pure SQL pulled me on to new things, and we had a good ride, but now it feels like I'm dragging it along the road with me. I think it's time to hitch SQL to the back of Python and benefit from a few things. 

1. SQL isn't a general purpose programming language. And it isn't supposed to be. A lot of this could come across as me whining about features SQL isn't supposed to have anyway. What I'm really complaining at here is that I've reached the point where I need to expand to other tools.
2. Python libraries for calling SQL are mature and popular. Whether it's SQLAlchemy for handling a range of SQL flavours, PyMySQL or psycopg2 for MySQL and PostgreSQL respectively, or plain old pandas or plain new polars, Python provides powerful options for working with databases that I have yet to properly explore. There's also Jinja2 for created SQL queries dynamically (e.g. with variables) in Python, and probably a lot more I've missed!
3. Iteration. Tests. Debugging. Error handling. Dynamic queries. Everything worth having in programming that isn't in pure SQL (or is hand to find or use or not as well supported) and has had me banging my head against a wall.

My plan is to start with SQLAlchemy alongside pandas and polars, reaching for other tools as I need them. I miss easy testing and debugging so much when I'm stuck in SQL (using Oracle in this case) that I'm really just excited to use what I've come to expect as standard feaures of a language. I probably should have pushed myself to this point earlier, but really I wanted a nice solid foundation in SQL before I fell back to something like Python or R and focused more on them. Wish me luck!