---
layout: post
title:      "CLI Data Gem Portfolio Project"
date:       2020-10-29 16:16:23 +0000
permalink:  cli_data_gem_portfolio_project
---


I was incredibly nervous starting my first portfolio project, as I'm sure many were. I have the most basic knowledge of programming, having taught myself a few things here and there during my six month furlough from my job, but Ruby was completely new to me.

The imposter syndrome was strong as I opened the project details, but I took a deep breath and started at the very basics - finding an API I enjoyed. I noticed on the list of APIs that we could use there was a Studio Ghibli API, and as a major Ghibli fan I was excited to jump into this one, but I took an oppertunity to look through a few others. I felt the Pokemon API was too difficult for me just then, having many subsections, I couldn't figure out how I would create a project with that information. Structurally, it didn't work out for me. 

So, I settled on the Studio Ghibli API and using the JSON and net/http gems, I was able to parse through the API and create a hash of all the films and their properties. At first, I wanted to have my app present a list of directors, and have the user choose the movie they wanted to explore based on the director, but after several failed attempts, and some discussion in office hours, I decided the best course of action would be to create a Movie class, listing the titles first, before going into the details of each film. I created a Movie class, to store each Movie instance. I created an API class where I pulled all the information from the Studio Ghibli API and created the new instances of the Movie class. I then made an CLI class where the core of my program would be written.

My user is able to access a numbered list of all the movies in the API and then choose the movie they would like to explore more by typing in the number corrleated with that movie. It will then list out all the details; the director, the year it was released, a brief summary, and the Rotten Tomatoes score. I wanted to take this program a step further, so I then allow the user to decide if they would like the movies organized by director, and then the movie titles organized alphabetically.

All in all, I had a positive experience with this project and I am very excited to keep learning.


