---
layout: page
title: R-Shiny News Dashboard
description:
img: /assets/img/proj_1.jpg
---

### **Overview**

This project was created for STA 523 Exam 2 (2019 Fall).

I created a news dashboard using R Shiny. It sources US news from [News API](https://newsapi.org/){:target="_blank"} which allows free access to up to 500 news per day. Users can read recent news headlines, or use key words to search for historical articles.

Refer to [here](https://christineshen421.shinyapps.io/News/){:target="_blank"} for the shiny app, and [this repo](https://github.com/christineymshen/RShiny_News){:target="_blank"} for the codes.

### **Thoughts**

This was my first R shiny project, done under time pressure with lots of trial and errors, and debugging. 

I envisaged it as a commercial app and tried to think of how a usual news dashboard looks like. I made references to widely used websites in designing the interface. I got the idea of the layout for the *News* tab from [CNN](https://www.cnn.com/){:target="_blank"}. And for the *User Manual* tab, I borrowed the idea from the then [Spotify support page](https://support.spotify.com/us/using_spotify/getting_started/){:target="_blank"} (I believe the layout is different now). I had a bit of fun to write all those gibberish in the User Manual tab. For the *Sources* tab, I used `DataTable` because among all the options I was aware of, `DataTable` seems to be optimal in terms of time/ efforts vs results. 

While working with R Shiny, one thing I constantly find frustrating is my lack of knowledge in CSS, HTML and JavaScript. It seems to me that it's hard to efficiently structure a project, and eventually scale things up, without making use of JavaScript. For almost all the sophisticated R shiny projects I've scanned through, they use JS extensively. So without a good knowledge in JS, the examples/ projects you can make reference to become substantially limited.

And almost always, trying to improve the UI without using CSS will inevitably lead to a messy large chunk of HTML codes which I won't even want to look at after I've done it. So every time I work on one-off projects like this, I find I'm constantly being challenged for my tolerance level in "ugly" UI or messy codes. And in cases where it's to a point that I can accept neither the ugliness nor the messiness, I would have no choice but to challenge my ability to stay up late -- spend more time to study how to use CSS.

On reflection, I'm not interested in UI design at all and I don't intend to learn HTML, CSS or JS beyond what's minimally needed. My interest in coding is mostly on the algorithmic side. So I guess for this type of projects in the future, I will go with existing template solutions (well, unless I'm being tested again like for this project), spend minimal time on formatting and focus more on the contents. For example this is currently my *plan* for this personal website.

Maybe one day I will change my mind and learn HTML, CSS and JS, who knows. But I don't see how I can possibly maintain a good working knowledge without extensively using them in my day-to-day work. And at least for now I would want to make sure they don't crawl into my daily work.