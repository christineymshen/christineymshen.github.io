---
layout: page
title: Auto-Encoding Variational Bayes
description:
img: /assets/img/proj_3.jpg
---

This is the Final Project for STA 663 (2020 Spring). This is a group project where two of us implemented the basic Variational Auto-encoder algorithm described in [Kingma and Welling (2013)](https://arxiv.org/abs/1312.6114){:target="_blank"}. This implementation uses a Gaussian MLP encoder and a Bernoulli MLP decoder. The set up is suitable for dataset like [MNIST](http://yann.lecun.com/exdb/mnist/){:target="_blank"}. 

We also implemented a python package. It relies on `numpy`, `matplotlib` and `numba`. 

  - To install:

      `pip install -i https://test.pypi.org/simple/ VAE-christineymshen`

  - To use the modules:

      `from VAE import VAE`

Refer to [this repo](https://github.com/christineymshen/sta-663-FinalProj-VAE){:target="_blank"} for examples on how to use the package and the underlying codes. Alternatively can also refer to [my friend's repo](https://github.com/RV29/VAE){:target="_blank"} for a slightly different version of the package.

The picture used for this project in the [Project page]({{ '/projects/' | prepend: site.baseurl | prepend: site.url }}) was reconstructed by our model, based on random handwritten digit images after training with the MNIST dataset.

