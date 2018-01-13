## [Select with Groups of 3 or 4 Takes Linear Time](https://arxiv.org/abs/1409.3600)
### Authors: Ke Chen, Adrian Dumitrescu
### Venue: WADS 2015.

### Problem:

Paper deals with the selection problem (of which finding the _median_ is the
most popular). In literature, it is now folklore to consider the _median of
medians_ algorithm, where a pivot is chosen (in two steps) by considering 
the medians of _groups of 5_. 

This paper asks (and answers) the question: why not _groups of 3_?

- they point out that an exercise in clrs is not entirely valid as to 
that groups of 3 will not work.
- thus, whether groups of 3 will or will not work is still open. 
- instead, they use groups of 3 as a starting point to devise an alternate
linear time median finding algorithm.
- (even groups of 4 can be made to work, with appropriate changes for 4
being even)

### Solution:
- the whole message of the median-of-medians algorithm is to find a 
suitable pivot. this involves two main considerations:
    - "suitable" means when using this pivot, we are able to throw away 
    at least a constant fraction of the input array
    - finding this pivot does not take too much time, or we will get killed
    in the recursion.
- for groups of 5, the procedure and the recurrence is as follows:
    - let T(n) be the time taken for input size n
    - medians of the groups of 5. takes O(n) time. 
    - medians of the medians - this is the pivot - takes T(n/5) time.
    - based on this pivot, how much do we discard? either the "top part"
    or the "bottom part". Roughly the "top" parts in n/10 groups of 5. So, 3 x n/10 = 3n/10. 
    - so, recurrence looks like: T(n) = T(n/5) + T(7n/10) + O(n)
- for groups of 3, this would look like T(n) = T(n/3) + T(2n/3) + O(n)
    - takes too much time in finding the pivot, T(n/3)
    - find _an approximate_ pivot, in faster time.
- _the repeated step algorithm_
    - group by 3 and find medians _twice_ in a row.
    - take n elements, group by 3, find medians. 
    - take n/3 medians, group by 3, find the n/9 medians.
    - the median of the n/9 medians is the *pivot*.
    - how much can we discard based on the new pivot?
        - given new pivot, at least how many items are bigger.
        - median of n/9 medians, in groups of 3
        - so n/18 x 2 (out of each group) = n/9
        - and these are medians in the earlier batch
        - so n/9 x 2 = 2n/9 
    - so the recurrence reads: T(n) = T(n/9) + T(7n/9) + O(n) and this
    gives T(n) = O(n)
    
#### Odd vs. Even
- group sizes of 4.
- with an even group size, there's the lower median, or upper median 
or the actual median (average of two)
- paper shows that if we _always_ choose the lower median (or always 
choose the upper median), we do not get the right recurrence. 
- answer: choose the median in every iteration according to the "data", depending on the 
current i (the parameter for selection) being < or > n/2, where n is the 
input size for that iteration.
- tl;dr: this works, but with some computations for the recurrence.