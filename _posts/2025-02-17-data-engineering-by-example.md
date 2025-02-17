---
layout: single
title:  "Data Engineering by Example"
date:   2025-02-17
category:
    data-engineering
tags:
  - data-engineering
  - python
  - learning
---

I'm creating [a GitHub repository](https://github.com/seafloor/data-engineering-by-example) with examples of common procedures in data engineering. This is because I recently decided to get more practical and hands-on with my data engineering path. I have a tendency to focus on the academic and read around the fundamental ideas too much before diving it. I also know that the tooling around production environments is something I can easily get bogged down in. Instead of either of these things, what I really want to do is focus on the data engineering itself, with little to no focus on anything else apart from testing, which I consider essential even if you're doing exploratory stuff.

I recently heard about [PEP 723](https://peps.python.org/pep-0723/) and how [it's supported in uv](https://micro.webology.dev/2024/08/21/uv-updates-and.html). I actually heard about it through an interview on the RealPython podcast with Charlie Marsh, who created Ruff and founded Astral to develop it and uv. In it he talked about getting support for that off the ground in uv and how it can abstract away all the tooling and requirements for getting something running, including the python version. All that's required is an installation of uv. 

This really inspired me as it meant I could get the core tenets of reproducible scripts but without all the extra steps. My aim really is to require only uv for the whole repository, have a single dev environment which is optional (probably just black, ruff, pre-commit, mypy, pytest) for testing or adding to the repository. Otherwise, every directory will be another example, like taking data from the spotify API and into SQLite, for example. The aim is to be broad and get me lots of experience in common data engineering tasks while potentially serving as helpful examples to others later. Everything will be driven by python (there'll still be SQL, but via SQLAlchemy etc.).

So there you have it. The repository is available [here](https://github.com/seafloor/data-engineering-by-example). I'll probably have blog posts for each example which give more details on any issues I encountered while working on the problem.

