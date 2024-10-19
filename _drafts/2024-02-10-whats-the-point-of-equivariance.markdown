---
layout: post
title: "What's the Point of Equivariance Anyway?"
date: "2024-02-10"
tags: ["equivariance", "machine learning"]
---

_Note: This post is mainly intended for those already familiar with the notion of equivariance and equivariant architectures. If you haven't encountered these before, I recommend this [excellent introduction](https://fabianfuchsml.github.io/equivariance1of2/) by Ed Wagstaff & Fabian Fuchs_

[Equivariant](http://proceedings.mlr.press/v139/satorras21a/satorras21a.pdf) [neural](http://proceedings.mlr.press/v119/bogatskiy20a/bogatskiy20a.pdf) [networks](https://arxiv.org/pdf/2110.13097) [are](https://pure.uva.nl/ws/files/60770359/Thesis.pdf) [all](https://pubs.acs.org/doi/full/10.1021/acs.chemrestox.3c00032) [the](https://proceedings.neurips.cc/paper_files/paper/2022/file/4a36c3c51af11ed9f34615b81edb5bbc-Paper-Conference.pdf) [rage](https://iopscience.iop.org/article/10.1088/1361-6420/ac104f/pdf). But are they really better than other approaches to incorporating symmetries in the data, such as data augmentation, symmetric loss functions, or even symmetry-free architectures?

The promise of equivariance is potentially two-fold:
1. A lower __sample complexity__, due to the model not having to 'learn' symmetries from the data, thus achieving a lower loss with less data.
2. Better __generalization__, since the model that we learn is guaranteed to respect the symmetries present in the problem, so we can ensure that it is the right 'neighborhood' of the function/hypothesis space.

In this post, I want to complicate this picture a bit and look at situations where equivariant architectures may in fact lead to __worse__ performance than non-equivariant ones. In particular, I'm going to focus on 3 different ways that equivariant architectures can fail to give good results, along with different papers that illustrate each of these scenarios. These are roughly ordered from 'most intuitive' to 'most theoretical', as follows:
1. Equivariant architectures may struggle with problems that are not __fully equivariant__.
2. Equivariant architectures can be __harder to optimize__.
3. Even when our problem is fully equivariant, equivariant architectures may fail to effectively approximate the solution!

# Equivariant Architectures Struggle with Partial Symmetry

Many real world datasets are not 'truly' equivariant, in the sense that their symmetries do not form a group. For example, consider the humble MNIST. If we rotate the image below by a few degrees in either direction, it should still be classified as a 6. This can be thought of as a continuous rotational symmetry. However, if we rotate by 180 degrees, suddenly the image looks like the number 9. Therefore an architecture which is fully invariant to rotations would be unable of learning the difference between the numbers 6 and 9.

<!-- Image of 6 and 9 from MNIST -->

This is a toy example, and few would suggest using a rotationally equivariant architecture for MNIST (especially when we can get 78% accuracy using only [gzip](https://jakobs.dev/solving-mnist-with-gzip/)). However, it illustrates the fact that many symmetries are only partial, and a fully equivariant network will be unable to generate this kind of function. One solution to this is to use data augmentation instead, since you can generate augmented data which respects your partial symmetries exaclty (such as only rotating MNIST images by angles between +-20 degrees. 

<!-- Maybe mention MCF paper as saying 'symmetries aren't always everything' -->

# Equivariant Architecures can be Harder to Optimize

# Equivariant Architectures can be Provably Worse!