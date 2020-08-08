---
layout: page
title: Auto-Encoding Variational Bayes
description:
img: /assets/img/proj_3.jpg
---

### **Overview**

This is the Final Project for STA 663 (2020 Spring). This is a group project where two of us implemented the basic Variational Auto-encoder algorithm described in [Kingma and Welling (2013)](https://arxiv.org/abs/1312.6114){:target="_blank"}. This implementation uses a Gaussian MLP encoder and a Bernoulli MLP decoder. The set up is suitable for dataset like [MNIST](http://yann.lecun.com/exdb/mnist/){:target="_blank"}. 

We also implemented a python package. It relies on `numpy`, `matplotlib` and `numba`. 

  - To install:

      `pip install -i https://test.pypi.org/simple/ VAE-christineymshen`

  - To use the modules:

      `from VAE import VAE`

Refer to [this repo](https://github.com/christineymshen/sta-663-FinalProj-VAE){:target="_blank"} for examples on how to use the package and the underlying codes. Alternatively can also refer to [my friend's repo](https://github.com/RV29/VAE){:target="_blank"} for a slightly different version of the package.

The picture used for this project in the [Project page]({{ '/projects/' | prepend: site.baseurl | prepend: site.url }}) was reconstructed by our model, based on random handwritten digit images after training with the MNIST dataset.

### **Thoughts**

The experience of this course has been ... interesting. 

For the final project, we needed to use what we've learned and implement an applied machine learning paper. A list of 12-13 papers were given to us for reference, together with sample reports from previous years. 

I scanned through the Abstract sections of all the papers and basically have no idea what they are talking about. Occasionally some terms catched my eyes and seem familiar, like *Hamilton Monte Carlo*, but still, I didn't understand what problems these papers are trying to solve, why these problems are interesting and worth looking at, and I had not the faintest idea what these algorithms are doing. So I had no preference at all on which paper we work on -- they all looked similarly mars to me anyway. 

I told my friend about my ignorance of machine learning and my indifference. Fortunately he did have a preference and relieved me from the burden of choice. And the one he had an eye on was Auto-Encoding Variational Bayes. In hindsight, if I knew how much additional work this one requires compared to some other papers, I might have said NO in the midst of that busy time. But it's always like this. If you believe this is something you should be able to accomplish, and just focus on getting it done rather than worrying about it and second-guessing your choice, it might turn out just fine. Well, as long as the task isn't too faraway from your reach, and in our case it's definitely true since this paper comes off the suggested list, isn't it. How hard can it be.

Now when I look back, I learned a lot while working on this project and I'm very happy with what we've got by the end of the project. But the whole experience has been as ~~frustrating~~ interesting just as the course, at least for me.

To start with, we decided to study the paper on our own and each try write our own piece of codes to implement the algorithm. While there are many VAE tutorials online, only few of them are helpful for us as most of them focus on implementation with existing packages like `tensorflow` and `Keras`. I made references to a few different tutorials, and it took me quite some time to put together pieces of information here and there only to understand the most basic concepts of the paper. Then in order to implement it, I had to use my shaky calculus, and the even shakier linear algebra to derive matrix form back-propagation formulas. 

While It was indeed very fulfilling to build everything from scratch myself, the learning curve was just too brain-frying-ly steep that it almost left me with a *trauma*. I wonder whether this is sort of the expectation for PhD students, that you should be able to work and study independently, with minimal guidance.

After we each have our own *proof-of-concept* implementation, we needed to further turn it into a package and optimize its performance so that it will not run forever for merely thousands of iterations. My friend had a much busier schedule at that time so I continued working on my version of codes. 

My first version was based on vanilla gradient descent, so the first thing I did was to change that into *ADAM* -- I still cannot say I understand what *ADAM* is really doing. Wel learned this algorithm during this course, without going into the underlying maths. I very much don't like this way of learning. Merely learning to implement some technique without a good understanding of the underlyings make me very comfortable. 

After that I vectorized my codes, then used `numba` jit decorator to speed up the codes. That was about all the time we've got to try different things. My friend created the package. We then concluded everything in the report and submitted at almost last minute.

Our final report was written and formatted in Jupyter notebook. We realized almost towards the last hour that neither of us can convert it into a correctly formated pdf file. Our final submission was based on a pdf file converted from print preview, and the formatting was... ~~unbearably ugly~~ interesting.

This made me realize how important it is to find a reliable TeX editor and start making an effort to learn about editing reports, which then led me to my explorations in using R markdown for LaTex in replace of Overleaf for another project report later.

One thing I've always wanted to try is to rewrite the codes in C++. From previous years' implementation reports and some other examples like [this](https://github.com/kelrenmor/sta663-FinalProject-SGHMC){:target="_blank"}, I feel like C++ should be able to further speed up the codes compared to `numba`. So after the semester ended, somehow, out of no reason, I started rewriting my codes in C++ using `eigen` and `pybind` (as suggested in our course material). It was again not a particularly enjoyable experience, but at least this time there wasn't a time limit. However, this version proved to be slower compared our submitted version -- it doubles the time. I'm still curious why C++ is not working well here. Whether it's because of my implementation, or it is inherent in this type of problem (e.g., VAE does not require matrix operations like inverse but mainly uses matrix multiplications). 

I also found two errors in the version we submitted. So after these revisions and while I had the free time, I created another python package myself, just for fun. I tidied up all the workings and rearranged everything in my repo. I feel like the whole world is back from chaos after the repo was updated and tidied up. And only then, this project was finally done for me.

One interesting sidenote is, I think after about a week that we decided to work on this paper, I learned that the research project I was likely going to work on is about Variational Bayes. And now I'm working on that project. Although so far I'm mostly playing with the Mean Field VB, it's interesting to see how things happen to connect together, even for just a tiny bit.

