---
layout: post
title: "Python Sorting Algorithms"
date: 2012-04-22 13:41
comments: true
categories: [Python, Algorithms, Performance]
published: true
---

So how fast is sorting in Python? What algorithms are there out there and how do they perform over different array sizes and situations?
This blog post helps answer these questions and gives some insight into the powerful performance of the inbuilt python sort function. I'm sure many people have run 'sort(list)' and then wondered what algorithm has that just used. If my list get really big is that going to be dog slow or super fast?

Sorting is a key algorithms, keeping data structures sorted helps search, lookup and a multitude of other operations. Many algorithms include sorting as component of there execution. Getting the sorting right can offer lower the order of magnitude of processing and make that slow, poor performing process super fast!

So what sorting algorithms are there out there. 

Three often discussed algorithms are

   * Merge Sort
   * Quick Sort
   * Insertion Sort

We'll look at each of these and then compare there speed using a randomly generated list of ints. (Testing speed with alphanumeric and strings is also worthy of investigation but is beyond this post)

### Merge Sort

Merge sort is a comparative divide and conquer algorithm. The algorithm recursively divides up list until the length is one and then recombines using a merge function that maintains the sorted order as it recombines the lists. Merge sort is often described to be an O(nlogn) algorithm.

{% codeblock Merge Sort lang:python %}
#Merge
def merge(left, right):
    result, i, j = [], 0, 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result += left[i:]
    result += right[j:]
    return result

#Merge Sort
def merge_sort(l):
    length = len(l)
    cut=length/2
    if length<=1:
        return l
    #left = merge_sort(l[:cut])
    #right = merge_sort(l[cut:])
    return merge(merge_sort(l[:cut]),  merge_sort(l[cut:]))
{% endcodeblock %}


### Quick Sort

Quick sort like merge sort is another divide and conquer algorithm that is recursive in nature. A pivot value is chosen (below i use the half way point other approaches use the last or first element) and then the lists are broken into 3 parts. Lists which contain values larger and lower than the pivot and the pivot itself. These segmented lists are then themselves sorted. Once the list size is 1 these lists are then in the correct place to be recombined using list concatenation. This algorithm is also descriibed to be an O(nlogn) algorthim but not it's worst case operation can be O(n^2) although this worst case is rare.

{% codeblock Quick Sort lang:python %}
#Quick Sort
def quick_sort(l):
    length = len(l)
    if length <=1:
        return l
    else:
        pivot = l.pop(int(length/2))
        less, more = [], []
        for x in l:
            if x<=pivot:
                less.append(x)
            else:
                more.append(x)
        return quick_sort(less) + [pivot] + quick_sort(more)
{% endcodeblock %}

### Insertion Sort
Insertion sort is a very simple comparative sorting algorithm which is not recursive. In this algorithm the sorted array is built one entry at a time. The power of this algorithm is that only requires constant space (O(1) space) as it is an inplace algorithm using only a few variables and the list itself to move items around. The problem with insertion sort is that with large lists its terribly inefficient and is a O(n^2) algorithm 


{% codeblock Insertion Sort lang:python %}
#Insertion Sort
def insert_sort(l):
    for i in range(1, len(l)):
        save=l[i]
        j=i
        while j>0 and l[j-1] > save:
            l[j] = l[j-1]
            j-=1
        l[j] = save
    return l
{% endcodeblock %}


### Speed Test

So in order to compare these algorithms to each other and python inbuilt sort. I decided to code up a speed test function and some code to generate random unsorted integer lists. The code for this can be found in my 'hacks' project and under the 'algos' folder.

The first test was just to run these four algorithms. The three above and pythons inbuilt sort to compare how these far on small lists of ints. Straight away you can see that the insertion sort is O(n^2) and as n gets larger the performance drops of drastically.

{% img  images/algos/sort-compare-small.png %}

You can also see how well the inbuilt algorithm fairs as well as the merge and quick sort algorithms. I wanted to see how well insertion sort did on very small list sizes and the below shows the same tests on lists lower than 50 items.

{% img  images/algos/sort-compare-micro.png %}

As you can see altough insertion sort is terrible at large list sizes the fact that its inplace and not recurrsive means its actually quite good at small lists consitently beating both merge and quick sort for list under 16 elements.

####So why is the inbuilt algorithm so good?

The inbuilt algorithm uses an adaptive merge sort algorithm. I decided to create adaptive versions of the quick sort algorithms. These algorithms take a sort function and use that on small lists. The below code shows a default using insertion sort when the list is less then 7 elements in length.

{% codeblock Adaptive Quick Sort lang:python %}
#Adaptive Quick Sort
def adapt_quick_sort(l, sortf=insert_sort, size=7):
    length = len(l)
    if length <=size:
        return insert_sort(l)
    else:
        pivot = l.pop(int(length/2))
        less, more = [], []
        for x in l:
            if x<=pivot:
                less.append(x)
            else:
                more.append(x)
        return quick_sort(less) + [pivot] + quick_sort(more)
{% endcodeblock %}






