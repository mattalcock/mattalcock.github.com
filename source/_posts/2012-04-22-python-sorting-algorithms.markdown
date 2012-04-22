---
layout: post
title: "Python Sorting Algorithms"
date: 2012-04-22 13:41
comments: true
categories: [Python, Algorithms, Performance]
published: true
author: Matt Alcock 
---

So how fast is sorting in Python? What algorithms are there out there and how do they perform over different array sizes and situations?
This blog post helps answer these questions and gives some insight into the powerful performance of the inbuilt python sort function. I'm sure many people have run 'sort(list)' and then wondered what algorithm has that just used. If my list get really big is that going to be dog slow or super fast?

Sorting is a key algorithms, keeping data structures sorted helps search, lookup and a multitude of other operations. Many algorithms include sorting as component of there execution. Getting the sorting right can offer lower the order of magnitude of processing and make that slow, poor performing process super fast!

So what sorting algorithms are there out there. 

Three often discussed algorithms are

    * Quick Sort
    * Merge Sort
    * Insert Sort

We'll look at each of these and then compare there speed using a randomly generated list of ints. (Testing speed with alphanumeric and strings is also worthy of investigation but is beyond this post)
