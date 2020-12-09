---
layout: post
title:  Efficient R Programming
date:   2020-12-09 14:54
description: coding, R
---

(For this post, I intend to just note down whatever that comes to my mind without paying too much attention on the English).

I have taken computing classes in both R and Python. But don't know why I find R is the natural choice for me if I get to choose. I don't know whether it's partly because we took the R programming course before the python one and we needed to use R for all the assignments in the first semester. But I also had exposures in python before starting graduate school, yet I didn't particularly prefer it. Or it could be I was just too annoyed by jupyter notebook, and the weird shortcut keys of Spyder on macbook. Anyway, for now I always use R if I can. 

R is ... notorious for its flexibility. Having worked with python which shouts at me every other minute when I use some wrong syntax, or C++ which shouts every few seconds, R never complains much. You can use all sorts of syntax, it'll try its best to interpret and give you some results, which, of course could very well be quite different from what you intended if you aren't careful. And almost always, there are more than one ways to do things. 

While coding for HWs or projects, I always wonder what's the \emph{best} practice. Best in terms of efficiency and also readability/ elegance in style. Sometimes you can't achieve both at the same time, then I'm curious what would be an optimal compromise. 

During the semester, most of the time I need to get something done before certain deadline -- accuracy is far more important than effiency. So I usually just focus on correctness and couldn't afford spending too much time figuring out the optimal syntax. But I've always hoped to be able to pick up good coding habits early, since the efforts saved will be compounding in time. 

Now it's winter break and while working on my research project, I happened to be going through another round of exercise to \emph{microbenchmark} codes to cut runtime. I know that if I don't take notes, I'll soon remember these findings and later be wondering about the same things again. So here I want to summarize all the tips I've noticed so far in terms of efficient R programming. I hope I will come back and update the list every now and then and maintain it. But I can also well imagine that I'll give up on it or just forget about it and this post ends up being a one-time thing. Let's see how it goes.


### Resources

1. [Efficient R programming](https://csgillespie.github.io/efficientR/){:target="_blank"}.

  - I came across this book while looking for tips on effient R coding. If possible I'd want to read the whole book. But so far I've only carefully studied chapter 7. In the following section, I use ERG to refer to this book.


### Notes

1. `ifelse`

I use this a lot because it looks very elegant. But as mentioned in ERG, it's not computationally efficient. For example, 

{% highlight R %}

ind <- ifelse(Y==1,1,-1)

{% endhighlight R %}

is not as efficient as this (assuming $m$ and $$ n $$ are the dimension of $$Y$$)

{% highlight R %}

ind <- matrix(-1,m,n)
ind[Y==1] <- 1

{% endhighlight R %}

Or if the variavble $$Y$$ is a vector, can make use of dplyr `if_else` function. This function doesn't work as well when $$Y$$ is a matrix since the output results is always a one-dim long vector.
