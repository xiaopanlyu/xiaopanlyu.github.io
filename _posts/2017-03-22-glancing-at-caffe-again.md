---
layout: post
title: "Glancing at Caffe again"
date: 2017-03-22 20:35
description: I redefine the Caffe in my mind.
tags:
 - Caffe
 - DeepLearning
---

## Glancing at Caffe again
I have been learning Caffe for several months. At the beginning, I just think that is a project to implement a specific machine learning progress. But now, I have change my mind. I redefine the Caffe in my mind. Actually, Caffe is a integrated deep learning framework. A clear hierarchy is established in Caffe framework. Users  can build a deep learning model very convenience because of the existing of diverse basic functions and layers.  For deep learning model researchers, it's easy to focus on the process of design. I think the clear hierarchy design for Caffe helps me understand the principle of deep learning. More specifically, the hierarchy of deep learning main includes four modules, ie. blob, layer, net, and solver. Learning this deep learning framework from the hierarchy side, as following.

## Blob
Blob [(Binary Large OBject)](https://en.wikipedia.org/wiki/Binary_large_object)  is a collection of binary data stored as a single entity in a database management system. Blobs are typically images, audio or other multimedia objects. Caffe uses blob as the basic data structure. The blob is the standard array and unified memory interface for the framework.

## Layer
Layer module is composed of many layers, such as basic data layer,  function layer, convolution layer, pooling layer, etc. There is a father class layer. All the son class layer are inherited it. 

## Net
This module main to solve how to connect layers to a network.

## Solver
A network need a solver. Solving is configured separately to decouple modeling and optimization. There are six solvers included in Caffe so far.

The [Caffe solvers](http://caffe.berkeleyvision.org/tutorial/solver.html) are:

- Stochastic Gradient Descent (type: "SGD"),
- AdaDelta (type: "AdaDelta"),
- Adaptive Gradient (type: "AdaGrad"),
- Adam (type: "Adam"),
- Nesterov’s Accelerated Gradient (type: "Nesterov") and
- RMSprop (type: "RMSProp")


## Reference
- [https://en.wikipedia.org/wiki/Binary_large_object](https://en.wikipedia.org/wiki/Binary_large_object "https://en.wikipedia.org/wiki/Binary_large_object")
- [http://caffe.berkeleyvision.org/tutorial/solver.html](http://caffe.berkeleyvision.org/tutorial/solver.html "http://caffe.berkeleyvision.org/tutorial/solver.html")
