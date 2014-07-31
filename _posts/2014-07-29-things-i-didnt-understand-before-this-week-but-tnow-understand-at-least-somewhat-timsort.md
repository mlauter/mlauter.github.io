---
layout: post
title: "Things I didn't understand before this week but now understand (at least somewhat) : Timsort"
description: My first venture into the python source code
modified: 2014-07-29 12:33:45 -0400
category: []
tags: [TIDUBTWBNU/AS, python, cpython, source code, sorting]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
{% include _image.html url="http://sortvis.org/images/dense-timsort.png" description= "timsort: from sortvis.org/algorithms/timsort.html" %}

This morning I was curious about how the built-in `sorted()` function works in python. So with the help of Tom, one of the facilitators here at Hacker School, I took my first dive into the python source code.

<br />
If you’ve heard anything about Timsort, what you probably know is that it takes advantage of partial ordering in the array you’re trying to sort in order to improve performance. Basically, instead of having a one-size-fits-all algorithm, Timsort chooses different approaches based on some stuff about the array it’s given.This is good because real world data is usually not totally random, that is, it usually does have some order or “sortedness” to it already. 

<br />
What I wanted to know, though, was how does this actually work?

<br />
I don’t know any C, but handily, within the CPython source code, there is a text file written by Tim Peters, explaining his eponymous function. Here is what I learned.

1. If the array that you are trying to sort has fewer than 64 elements, Timsort will simply do a **binary insertion sort**.

<br>
   In a normal insertion sort, you would start with the first element of the array and check it against the next item over. If the first item is greater than the second, you flip them, otherwise, you leave them where they are. Now you know your first two items are sorted, so you move on to the third item. You then figure out where the third item belongs by checking from right to left. If item 3 is larger than the preceeding item, you can leave it where it is. Otherwise, you need to check the first item. You continue on down the array until you've placed each item in its proper location.

 <iframe width="560" height="315" src="//www.youtube.com/embed/ROalU379l3U" frameborder="0" allowfullscreen> </iframe>

<br> 
   In the *binary* version, instead of searching through the elements one by one, you perform a binary search to find the appropriate location. This means, you go to the midway point of the array you're searching through, and decide whether your item is greater or smaller. Once you pick a half, then you choose the midpoint of that half and check again. Repeat until you are checking an array of length 2, and you find your spot. Though this method doesn't improve the running time O(n2) of insertion sort, it turns out that comparing two things is much more costly (at least in python?) than swapping, so it improves performance.



2. The notion of "**runs**"


The **run** is a very important concept for Timsort. If you have an array longer than 64 elements, the algorithm will take a first pass through the array checking for chunks that are stricty increasing or strictly decreasing (if the chunk is decreasing, it will be reversed). 

If these chunks are longer than a certain size, known as minrun, which is determined based on the size of the array,<sup id="fnref:1"><a href="#fn:1" class="footnote">1</a></sup> then this is a **natural run**--it occured naturally in your array. If the chunk is shorter than minrun, you grab `minrun - len(chunk)` items ahead of your chunk, and perform a **binary insertion sort** to create an artificial run. 

After this, what you have is an array of sorted chunks of varying lengths. If your data was totally random, then chunks will probably all be close to the minrun length. If not, you could have natural runs of wildly varying lengths:

![An array with consecutive, descending runs, a, b, and c](../images/timsort_list_image.jpg)


3. Merging


The next step is to merge sort your sorted chunks. We are only allowed to merge agacent chunks so that items do not get out of order with respect to the intervening chunks. One important property of Timsort is that it is *stable*, meaning that items of equal value remain sorted in order with respect to their original positions in the list. (I'll come back to this later).

As Timsort finds runs, it adds them to a stack (so an item seen first in the array goes on first and is at the bottom of the stack).

![the stack of runs](../images/timsort_stack_image.jpg)

Timsort tries to balance two competing needs when merging runs. On the one hand, we want to put off merging chunks in case it turns out merging our current run with the next run would be better than (more efficient than) merging with the previous run. On the other hand, we don't want to let the stack get too big, because then we'll have to reach really far down to get those earlier items, which would hinder performance. To enforce a compromise, Timsort keeps track of the three most recent items on the stack and creates two laws that must hold true of those items:

>1. a > b+c
2. b > c 

If either of these laws are broken when a new run is pushed to the stack, items are merged. If a is larger than c, a and b are merged, otherwise b and c are merged.

4. Timrun's merge sort

<div class="footnotes">
  <ol>
    <li id="fn:1">
      <p>Minrun is a value between or including 32 to 64 such that len(array) equals or is slightly less than a power of 2. We'll see why this is important later on, but the idea is when you do a merge sort, you want to have a power of two number of things to sort. 
      	</p>
    </li>
  </ol>
</div> 





