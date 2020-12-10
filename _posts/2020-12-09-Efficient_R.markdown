---
layout: post
title:  Efficient R Programming
date:   2020-12-09 14:54
description: coding, R
---

Notes:
1. For this post, I intend to just note down whatever that comes to my mind without paying too much attention on the English
2. Somehow the KaTeX isn't working with my current set up. So I can't use math mode in this post. Will try to figure this out later. One thing at a time. And I really don't like this feeling that I need to work with things I don't understand
3. Not familiar enough with markdown and html, in the following sections, I'm deliberately not making use of markdown list since I want to have code chunks in each ordered list item. Haven't figured out how to make that work

I have taken computing classes in both R and Python. But don't know why I find R is the natural choice for me if I get to choose. I don't know whether it's partly because we took the R programming course before the python one and we needed to use R for all the assignments in the first semester. But I also had exposures in python before starting graduate school, yet I didn't particularly prefer it. Or it could be I was just too annoyed by jupyter notebook, and the weird shortcut keys of Spyder on macbook. Anyway, for now I always use R if I can. 

R is ... notorious for its flexibility. Having worked with python which shouts at me every other minute when I use some wrong syntax, or C++ which shouts every few seconds, R never complains much. You can use all sorts of syntax, it'll try its best to interpret and give you some results, which, of course could very well be quite different from what you intended if you aren't careful. And almost always, there are more than one ways to do things. 

While coding for HWs or projects, I always wonder what's the *best* practice. Best in terms of efficiency and also readability/ elegance in style. Sometimes you can't achieve both at the same time, then I'm curious what would be an optimal compromise. 

During the semester, most of the time I need to get something done before certain deadline -- accuracy is far more important than effiency. So I usually just focus on correctness and couldn't afford spending too much time figuring out the optimal syntax. But I've always hoped to be able to pick up good coding habits early, since the efforts saved will be compounding in time. 

Now it's winter break and while working on my research project, I happened to be going through another round of exercise to *microbenchmark* codes to cut runtime. I know that if I don't take notes, I'll soon forget these findings and later be wondering about the same things again. So here I want to summarize all the tips I've noticed so far in terms of efficient R programming. I hope I will come back and update the list every now and then and maintain it. But I can also well imagine that I'll give up on it or just forget about it and this post ends up being a one-time thing. Let's see how it goes.


### Resources

1. [Efficient R programming](https://csgillespie.github.io/efficientR/){:target="_blank"}.

  - I came across this book while looking for tips on effient R coding. If possible I'd want to read the whole book. But so far I've only carefully studied chapter 7. In the following section, I use ERG to refer to this book.


### What I've already tested

1.`ifelse`

I use this a lot because it looks very elegant. But as mentioned in ERG, it's not computationally efficient. For example, 

{% highlight r %}
ind <- ifelse(Y==1,1,-1)
{% endhighlight r %}

is not as efficient as this (assuming m and n are the dimension of Y)

{% highlight r %}
ind <- matrix(-1,m,n)
ind[Y==1] <- 1
{% endhighlight r %}
    
Or if the variavble Y is a vector, can make use of dplyr `if_else` function. This function doesn't work as well when Y is a matrix since the output results is always an 1-dim long vector. More detailed examples can be found in ERG.

2.log of normal density

The following are similar in terms of runtime.

{% highlight r %}
log(dnorm(a)), dnorm(a, log=T)
{% endhighlight r %}

3.`rmvnorm` function

To generate multivariate normal random numbers. I checked the performance of this function in three packages: `amen`, `mvtnorm` and `Rfast`. `mvtnorm` is way too slow. `amen` and `Rfast` are both much faster, but `amen` is definitely the fastest. 

("Can I add that I'll use `amen` even if it's not the fastest just for sheer affection?"<br>"No don't do that please..."<br>"Well, I've already done so anyway"<br>"What can I say ..." [eyes rolling]) 

4.`rowSums`

While `rowSums` is definitely much faster compared to using `apply` and its friends, it's not the most efficient. `rowSums2` in the `matrixStats` package is better.

5.Boolean operator across columns/ by row

For some reasons I've been using `rowSums` to check this, e.g., rowSum = number of col to check everything is TRUE across all columns in a row. This is clearly not efficient. 

One alternative is

{% highlight r %}
a[,1] & a[,2]
{% endhighlight r %}

But I assume this only works when you only have a few columns, you won't want to enumerate all the columns. In any case, the best I've found for now is again from `matrixStats` package, `rowAnys` and `rowAlls`. I really like this package :)

However, to calculate product across columns/ by row, so far I'm still sticking to `a[,1] * a[,2]`. The `matrixStats` version `rowProds` is quite slow compared to this. Maybe it's intended for some other more complicated scenarios.

6.Matrix initialization

Usually I need to initialize a matrix of the size that is same as one of the input variables in a function. A few things to note here:

  - if I really need to get the dimension from the input variable, `dim(M)[1]` is faster than `nrow(M)`. But if M is a vector, nothing beats `length(M)`
  - if I just want to initialize a matrix that's the same size in order to store results later (presumably I'd want to do this because later I need to do subsetting on this variable, otherwise I won't need to particularly initializ a variable), it's faster to use `res <- M * 0` rather than `res <- matrix(0, dim(M)[1], dim(M)[2])`
  
These things don't matter much if you only need to do it once, but they have noticeabl impact on runtime if you are calling a particular funtion a gazillion times in your run (I almost feel I'm oblidged to add a reference here on using the word *gazillion*).

7.Maximum across columns/ by row

I started by using `mapply(max,vec_a, vec_b)`. It's sooooo painfully slow. An alternative is the very basic `pmax` funtion from base R. Another alternative is `Pmax` function from the `Rfast` package. It seems that `Pmax` is marginally faster compared to `pmax`. But so far this is the only function I might want to use in `Rfast`, so I decided not to use it for now since once loaded, it masks a few other functions in other commonly used packages and this behaviour makes me very uncomfortable.

8.`&&` vs `&`

The former is the scalar version of the latter, just like `||` and `|`. Note from ERG that if use `a || b`, R doens't evaluate `b` if `a` is TRUE. while for `|`, the second argument will always be evaluated. So we should try to the non-vectorized version whenever possible.

9.`is.na()` vs `anyNAs`

Refer to ERG, `anyNAs` is more efficient.

### I'm still wondering about ... 

1. When I subset a matrix using indicators, sometimes if there's only one item left, the matrix will automatically collapse into a vector. In order to avoid the lost of dimension, I use `drop=F`. But I don't know whether this will lead to inefficiency in codes. 


