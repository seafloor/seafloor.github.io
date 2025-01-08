---
layout: single
title:  "How to learn industry things as an academic"
date:   2025-01-05
category:
    R
---

This is the blog equivalent of a William present, so-named after [Just William](https://en.wikipedia.org/wiki/Just_William_(book_series)), wherein you give someone a gift so that you can play with it yourself. This is a classic thing kids do when very young: trying to give their parents a birthday present that's the new octonauts toy set they want to play with. So this is my William present - a little overview of how to learn industry things as an academic. And really, it's a guide to me, from me, on how I can do these things better, as I set myself up to leave bioninformatics.

The topics need clarifying first because I, like others in my position, am not worried about my knowledge or skills in doing the job itself, for the most part. My transition will hopefully be to data engineering, so there's some knowledge and skills to learn. But a lot of it is in how work is done in industry, which is very different to academia. So what we're really after are knowledge and skills on the work itself, and then knowledge and skills on how to work. The former is fairly easy to come by - a textbook like [Fundamentals of Data Engineering](https://www.oreilly.com/library/view/fundamentals-of-data/9781098108298/), an online course like the [AWS Data Engineering course on Udemy](https://www.udemy.com/course/aws-data-engineer), blog posts from [FAANG companies](https://en.wikipedia.org/wiki/Big_Tech#Acronyms) etc. There are really too many resources and options for filling this gap. Even soft skills like writing, presenting, talking about your work etc. are, at least in the biosciences, well-honed in academia.

It's the "how to work" part which is the issue. In academia we generally work alone, someone collaborate on code or repositories, which often amounts to shared poorly-managed github without any formal pipeline, code review, project management system etc. It's largely every researcher for themselves.

Assuming then that you, like me, think you can cover the skills parts yourself, and you're aiming for something like data engineering or data science, you're likely worried about one or more of the following

- Testing and automation (unit tests, integration tests). Unit testing is more common now in bioinformatics, but integration tests or test-driven development? Much less common
- CI/CD (or any automation pipeline - uncommon in academia)
- Agile methodology (agile, scrums, trello, jira etc., none which are even talked about in a normal bioinformatics lab)
- Collaboration in version control (Git? Routine. Public GitHub repos? Common. Commits to a shared repo? Sometimes. Git flow or trunk-based development with a formal code review system? Keeping dreaming)

In general, these summarise as project management, workflow and collaboration procedures that we just don't touch in academia. So how to learn them? Here are some (brief) options and my plan for implementating them.

1. **Push your lab:** If you work with other bioinformaticians, give them a small metaphorical kick. Make a lab github page for common routines. Add unit testing, and try to get others to commit to code reviews and working together on the repository. It will eventually be a massive boon to the group's productivity, a big advert for your skills, and help with the public face of your lab. Apart from the opportunity cost of spending time on a paper, there are no downsides (and even then I'd say it's easily offset). I'm doing this now [with our lab](https://github.com/seafloor/escott-price-lab-pipelines). Getting others to follow through with requests to add code is difficult (they all have their own research to do, so it's understandable), but it's been worth the effort to create.
2. **Push yourself:** The option you might not have wanted is that making your own projects is the way to go. I've had a few interviews for post-doc positions, and they never ask for proof of my skills in-person, or projects *per se* - they ask about the papers I've written. In industry it's the opposite (I think) - people care less about your publishing record and more about what you can *do*. So, as Austin Kleon puts it, *show your work*. Make a public project, use free trials of Agile tools to manage the project, follow ideas for TDD etc. For this I'm planning to do 3 great projects which I will make publicly on GitHub, 
3. **Join them:** You were never trying to beat them, but you can join them anyway. Find a local hackathon or group event where you can work as part of a team. You can check out [eventbrite](https://www.eventbrite.co.uk/d/united-kingdom--london/hackathon/) for options. This is one of the scariest options for me, but in the next 6 months I aim to join something and give it my best!
4. **Join them (virtually)**: The cop-out I wanted to in-person meet-ups, but which isn't really a cop-out at all. Contribute to open source. There are many tutorials and beginner friendly options in R and Python. This is a great way to get introduced to working as a group and the different CI/CD pipelines used. Look for the new contributer or beginner friendly issue tags on GitHub. This is one of my top priorities for the next few months. I'm planning to contribute to some of the SciPy ecosystem packages - maybe scitkit-learn or pandas!
5. **Read:** I've put this last because, as an academic, it's probably what you'll lean towards anyway, so it's good to focus on projects instead. There are a lot of resources, like posts on ways of working, [books on scrum](https://www.goodreads.com/book/show/19288230-scrum), or just sites to go through for their trials and tutorials from [scrum](https://www.scrum.org/), [trello](https://trello.com/) and [jira](https://www.atlassian.com/software/jira), for example. This is an easy one and the first step, but doing something with these tools is the most important thing!

What did I leave off? The big ones would be a part-time role or internships. These are intentional omissions because you are likely already over-worked and under-paid in academia, and you don't need more of it, and there's an element of "you need 3 years work experience to get work experience", which makes things seem impossible. 

So, use free tools, join with others, do it publicly and openly, and create something! 
