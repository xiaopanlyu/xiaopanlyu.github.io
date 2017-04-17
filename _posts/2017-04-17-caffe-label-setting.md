---
layout: post
title: "Caffe Label Setting"
date: 2017-04-17 
description: caffe labels.
tags:
 - Caffe
 - DeepLearning
---

# Caffe Labels Setting

## How to set up labels of dataset?

Recently, I have done a logistic regression in Caffe. But I can not get my training converged. I using the simplest one layer network which only include a "SoftmaxWithLoss" layer as following.
![Logistic Regressing Network]({{ site.baseurl | prepend:site.url}}/images/201704171055_LogReg.png){: .center-image }*Logistic Regressing Network*


I check my dataset, and find that my dataset has set the two class label as "1" and "2". I try to change the labels to "0" and "1", then my training has converged. But to be honest I don't know why. So I try to find the answer in the source code. Fortunately, I have got the truth in softmax_loss_layer.cpp at lines 133 and 139.

```c++
 if (propagate_down[0]) {
    Dtype* bottom_diff = bottom[0]->mutable_cpu_diff();
    const Dtype* prob_data = prob_.cpu_data();
    caffe_copy(prob_.count(), prob_data, bottom_diff);
    const Dtype* label = bottom[1]->cpu_data();
    int dim = prob_.count() / outer_num_;
    int count = 0;
    for (int i = 0; i < outer_num_; ++i) {
      for (int j = 0; j < inner_num_; ++j) {
        const int label_value = static_cast<int>(label[i * inner_num_ + j]);
        if (has_ignore_label_ && label_value == ignore_label_) {
          for (int c = 0; c < bottom[0]->shape(softmax_axis_); ++c) {
            bottom_diff[i * dim + c * inner_num_ + j] = 0;
          }
        } else {
          bottom_diff[i * dim + label_value * inner_num_ + j] -= 1;
          ++count;
        }
      }
    }

```

The softmax_loss_layer in Caffe using label value as the array index. And it must start with 0. 


