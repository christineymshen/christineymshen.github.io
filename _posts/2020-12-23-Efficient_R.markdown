---
layout: post
title:  Efficient R Programming
date:   2020-12-09 14:54
description: coding, R
---

Last updated: 13/01/2020.

Notes:
1. For this post, I intend to just note down whatever that comes to my mind without paying too much attention on the English
2. Somehow the KaTeX isn't working with my current set up. So I can't use math mode in this post. Will try to figure this out later. One thing at a time. And I really don't like this feeling that I need to work with things I don't understand
3. Not familiar enough with markdown and html, in the following sections, I'm deliberately not making use of markdown list since I want to have code chunks in each ordered list item. Haven't figured out how to make that work

I have taken computing classes in both R and Python. But don't know why I find R is the natural choice for me if I get to choose. I don't know whether it's partly because we took the R programming course before the python one and we needed to use R for all the assignments in the first semester. But I also had exposures in python before starting graduate school, yet I didn't particularly prefer it. Or it could be I was just too annoyed by jupyter notebook, and the weird shortcut keys of Spyder on macbook. Anyway, for now I always use R if I can. 

R is ... notorious for its flexibility. Having worked with python which shouts at me every other minute when I use some wrong syntax, or C++ which shouts every few seconds, R never complains much. You can use all sorts of syntax, it'll try its best to interpret and give you some results, which, of course could very well be quite different from what you intended if you aren't careful. And almost always, there are more than one ways to do things. 

While coding for HWs or projects, I always wonder what's the *best* practice. Best in terms of efficiency and also readability/ elegance in style. Sometimes you can't achieve both at the same time, then I'm curious what would be an optimal compromise. 

During the semester, most of the time I need to get something done before certain deadline -- accuracy is far more important than effiency. So I usually just focus on correctness and couldn't afford spending too much time figuring out the optimal syntax. But I've always hoped to be able to pick up good coding habits early, since the efforts saved will be compounding in time. 

Now it's winter break and while working on my research project, I happened to be going through another round of exercise to *microbenchmark* codes to cut runtime. I know that if I don't take notes, I'll soon forget these findings and later be wondering about the same things again. So here I want to summarize all the tips I've noticed so far in terms of efficient R programming. I hope I will come back and update the list every now and then and maintain it. But I can also well imagine that I'll give up on it or just forget about it and this post ends up being a one-time thing. Let's see how it goes.


## Resources

1. [Efficient R programming](https://csgillespie.github.io/efficientR/){:target="_blank"}

  - I came across this book while looking for tips on effient R coding. If possible I'd want to read the whole book. But so far I've only carefully studied chapter 7. In the following section, I use ERG to refer to this book.
  
2. [Advanced R](http://adv-r.had.co.nz/){:target="_blank"}

  - This book is occasionally referred to by ERG. So far I scanned through the sections on Environments and Closures. Quite helpful.
  
3. [R inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf){:target="_blank"}

  - This is another book referenced in ERG. I read a little bit to understand Closures. It's a fun read.
  
## ERG Reading Notes

### R Studio set up

1. Global option, "General" tab, `Restore .RData`. I followed the suggestion and turned this off. Occaisionally when I happened to be working with large data sets/ results, it could take quite a while when I start a R session. It's fine if I still need to work with these data, but sometimes it could be annoying if these come from the a project I've been working on and I don't really want to go back there. For some reasons whenever I open a new R session, it seems by default will go back to the last project... and hence will load these data I most likely won't need. 

2. I also changed my code editing option to get rid of autocompletion of braces. Sometimes this is actually helpful, but most of the time I find this feature very annoying ... 

3. I then updated font size from 10 to 12 ... getting older isn't it ... 

4. RStudio autocompletion. This is interesting. I like the autocompletion function in the editor, but I hate the function in RStudio Console. For some reason, auto-completion isn't working properly there. Most of the time while working in the Console, after I type of half of the function name or whatever, RStudio will give me a list of things it guesses I might want to type, which is helpful. But after I select one, it usually won't replace the part I've already typed and this is just horrible. It always leaves me with something like "rnornorm". I'd have to go back to remove the first part myself. Alternativelly I'll have to insist typing everythign myself while RStudio happily showing off its guess list, covering part of my screen and making it hard for me to type. I once asked a friend whether he knows how to get rid of this. He said he's aware of it and also had no idea how. 

    - type Tab to find out the list of suggestions from autocompletion, and then use Tab again to confirm your selection. 
    - noted that RStudio autocompletion isn't case sensitive which is the case for base R console
    - RStudio is also able to find hidden files in the subfolder
    
5. section 2.5 recommends making use of the `Project` feature. I've decided to give it a go. [20210113] So far I like the project set up better. It's easier whenever I need to resume from other work. I don't need to manually set the working directory. And it forces me to use a structure that is more suited for my project.

### Efficient programming

<ol>

<li> [20201229] In section 3.6.1, the book touches on function closures. And this happens to be something I've been looking for. The background is ... I have a function, say `f`, which essentially does linear interpolation based on pre-stored values in a huge look-up table. I need to load the look-up table object before calling this function. I've been loading it in the global environment so that whenever I call `f`, it'd be able to access the table. However, I'm also aware of how undesirable it is to directly write into the global environment. And after I came across the following in section 6 of the book R Inferno ... I decide to try to change, since I don't have a boss "turning red with anger over me" ... 

    So after researching for the whole evening. I finally start to understand closures and implemented it in my codes. I feel like I might need to think about these things more to get myself comfortable with the ideas of enclosing, binding, execution and calling environment etc. 

<blockquote>
If you think you need "<<-", think again. If on reflection you still think you need "<<-", think again. Only when your boss turns red with anger over you not doing anything should you temporarily give in to the temptation.
</blockquote>
</li>
<li> Section 3.7 of ERG talks about making use of compiler to speed up codes. Basically we can always use the byte compiler that comes with R. A bit like JIT when I was learning python. Obviously when I have a suite of almost 20 functions, I won't want to go through each one of them and `cmpfun()` them. A simple way to use byte compiler <a href="https://www.r-statistics.com/2012/04/speed-up-your-r-code-using-a-just-in-time-jit-compiler/" target="_blank">see here</a> is to simply add the following at the beggining of your codes. I tried with my function, not obvious speed up thus far. As most documention points out, this kind of compiler works best on those notorious "bad codes" which uses of loooooooops, which, nobody in real life does.

{% highlight r %}
library(compiler)
enableJIT(3)
{% endhighlight r %}
</li>
</ol>

### Efficient workflow

For most of the stuff in this chapter, I've learned them through the hard ways and am practicing already. But there's one point it reminds me of: the importance of regression tests. 

For example in my current project, I've been frequently adding in new algorithms/ functionalities to my code. And often times the newly added codes are inter-linked with existing ones since I usually want to make use of any existing codes that's still useful. Then how can I be sure that the old functions still work as I expected and that nothing breaks? I really need to think about how to build in regression tests in a project. And every time I make changes to a function, major or minor, I should be able to do a few quick routine tests to at least by 80% sure that whatever used to work is still working.

ERG mentions in this chapter that treating your project as a dummy package is one (time-consuming) way to pick up good habits which will benefit you in the long run. Um ... It's worth trying. Especially what the author mentioned as "dummy package". But somehow I've got a mental block on building package ever since I took the 1-credit course last sem. Let's see ... 

## General thoughts

1. if I only need to use 1-2 functions in a package, especially a package that I'm not very familiar with, or less commonly used, it seems better just to call the function using `namespace::function` rather than loading the package. Safer, less drama;

2. Use R script files for all my functions so that it's easier to debug. Use rmd mainly for write up and generating graphs;

3. It's nice to use defensive programming, i.e., make use of asser_that etc., but take note that this function is very slow, you don't want to put it in a function which you'll be calling for a million times. 

## What I've already tested

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
    
Or if the variavble Y is a vector, can make use of dplyr `if_else` function. This function doesn't work as well when Y is a matrix since the output results is always an 1-dim long vector. More detailed examples can be found in ERG. [20201229] Another undesirable thing about `if_else` is, it appears to always evaluate both TRUE and FALSE results irregardless of the conditional statements. This can be undesirable if for example, when the statement is TRUE, we might not even have evaluated some of the objects there. 

[20210104] This way is slightly faster:

{% highlight r %}
ind <- (-1)^Y
{% endhighlight r %}

2.log of normal density

The following are similar in terms of runtime.

{% highlight r %}
log(dnorm(a)), dnorm(a, log=T)
{% endhighlight r %}

But, definitely go with `dnorm(a, log=T)` or pnorm(a, log.p=T)` when dealing with tail distribution, or if you are working with the log space. Much better numerical stability. And ... could be actually more efficient given the form of the normal pdf.

3.`rmvnorm` function

To generate multivariate normal random numbers. I checked the performance of this function in three packages: `amen`, `mvtnorm` and `Rfast`. `mvtnorm` is way too slow. `amen` and `Rfast` are both much faster, but `amen` is definitely the fastest. 

Tested the `Rfast::dmvnorm` takes only about 1/4 the time of `mvtnorm::dmvnorm`. Really should try to avoid using `mvtnorm` whenver possible.

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

10.return list instead of vector
[20201225 updates]

In one of the functions that gets called very very often, I need to pass back 2 or 3 numbers. Potentially I can either pass back through a list, or a vector.

If I use list, I can easily retrieve the parameters through their names, it would be clear and less prone to mistakes. But I wonder whether the structure of list would slow down the process. 

If I use vector, I would need to retrieve the parameters by syntax such as `res[1]`. I guess it'd be faster compared to using list, or at least as fast, but then there's a risk that if the order of the parameters are accidentally switched, it might lead to mistakes hard to discover. Alternatively, I can also assign names to elements of vector and try to retrieve through names as `res["a"]`.

I wonder how much difference does returning list or vector make. If there are differences, whether it's worthwhile for losing codes readability. And if there's no difference at all, indeed I'll go with using list. 

I tested using three simple functions:

{% highlight r %}
f1 <- function(a,b,c){
  list(a=a,b=b,c=c)
}
f2 <- function(a,b,c){
  c(a=a,b=b,c=c)
}
f3 <- function(a,b,c){
  c(a,b,c)
}
{% endhighlight r %}

With 20000 passes, f1 is slightly slower compared to f3, median time 24 vs 21 milliseconds. f2 is definitely slowest, doubles the time. So all in all, I think it's not worthwhile to use vector just for saving such little time but risk running into error some later time.

(I'm listening to Anastasia while working now. I suspect I might have typed completely nonsensible/ grammarly incorrect sentences ...)

11.Order of numerical and logical comparisons

Let a, b, c all be vectors of length m. I want to have an (m x 1) boolean vector d = (a<c) or (b<c). I wonder which way is faster

{% highlight r %}
d <- (a<c) | (b<c)
{% endhighlight r %}

or
{% highlight r %}
d <- Rfast::pmin(a,b) < c
{% endhighlight r %}

Tested the first one takes 1/3 time.

12.initialize logical vector

`rep(TRUE,m)` is slightly faster compared to `!logical(m)` if I want to initialize a logical vector of TRUE of length m.

13.matrix inverse

If the matrix is symmetric, which is the case for me most of the time now anyway, then it's faster to use `chol2inv(chol(M))` compared to `solve(M)`. Microbenchmarking shows that it takes less than half the time.

## I'm still wondering about ... 

1. When I subset a matrix using indicators, sometimes if there's only one item left, the matrix will automatically collapse into a vector. In order to avoid the lost of dimension, I use `drop=F`. But I don't know whether this will lead to inefficiency in codes. 


