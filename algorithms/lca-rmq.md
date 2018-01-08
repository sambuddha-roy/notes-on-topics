## [The LCA Problem Revisited](www.ics.uci.edu/~eppstein/261/BenFar-LCA-00.pdf)
### Authors: Michael Bender, Martin Farach-Colton

#### What is the problem about

The [basic problem](https://www.geeksforgeeks.org/lowest-common-ancestor-binary-tree-set-1/) is: given a tree T, and vertices
u and v in the tree, find the lca(u, v): the _least 
common ancestor_ of u and v in the tree. For this, 
we may:
- find the path from root to u 
- find the path from root to v
- find the last common vertex between these paths.

Here, we consider the situation where we might have
_many_ queries (u, v) for which we need to answer
lca(u, v) queries. Thus, we will allow some 
- preprocessing time to build suitable data structures
- and use query time to query the data structures.

**Main Result:**
For the (preprocessing, query) version, the 
paper gets (O(n), O(1)) time: i.e. it is possible
to preprocess the input tree in O(n) time so that 
any lca(u, v) query may be answered in O(1) time!

The paper resolves this by a bunch of cute and simple 
reductions!
#### Bunch of reductions

*Companion Problem:* 

Consider also the _RMQ_ problem, where given an 
array A, and indices i, j, RMQ(A, i, j) returns 
argmin {A[x]: i <= x <= j}. 

*Piece 1: reduction 1*
- LCA <= RMQ
    - reformulates lca(u, v) as finding shallowest vertex in dfs 
    trail between u and v.
    - considers euler walk over tree according to the dfs; this
    is the array on which RMQ is run. **this is the main
     observation.**
    - preprocessing time incurs extra O(n)
    - query time incurs extra O(1)
- extra mileage. 
    - in this reduction the output array consisting of
    the "depths" as according to euler walk
    has any two _consecutive_ entries of the array being
    _consecutive_ numbers.
    - these arrays are called arrays with +/-1 restriction.

*Piece 2:*
- RMQ in (O(n^2), O(1)) time. 
    - easy to do by dynamic programming.
    - update step is easy
- RMQ in (O(n log n), O(1)) time. 
    - _in computer science, think powers of 2_
    - only fill up RMQ(A, i, j) for abs(i -j) = power of 2. 
    - O(n log n) such entries. why?
    - can be filled in O(n log n) time. why?
    - gives RMQ(A, i, j) for any i, j in O(1) query time. how?
    ask for RMQ(A, i, i + 2^k) and RMQ(A, j - 2^k, j) for a 
    suitable k.

*Piece 3:*
- RMQ for +/-1 arrays in (O(n), O(1)) time.
    - these arrays are special: they can be described as 
    start (offset) + a vector of +1/-1's. 
    - _observation_: offset does not matter for finding minimum in an array.
        - eg. min index in [1, 3, 2] is _same_ as min index in 
        [4, 6, 5]
    - if such arrays are small in size, we can in fact enumerate
    all of these possibilities, and keep a lookup table. if the 
    array is of size k, then 2^k different possibilities for the
    array.
    - we will break up original array into _blocks_ of size k.
        - total of n/k blocks. 
        - use O(n) time to find for each block, the minimum, and
        store pointer. this gives us an array of n/k elements.
        - use the (O(n log n), O(1)) solution for this modified array, 
        with n/k being the new n.
        - to have n/k log n/k = O(n), we will keep k = O(log n).
    - if k is log n, then 2^k is n. 
        - we can keep a lookup table of 
        size 2^k x k_choose_2 = O(n log^2 n). not good.
    - if k is log n/2, then 2^k is sqrt(n)
        - now it works out. 
        - lookup table of size O(sqrt(n) log^2 n) = o(n), all good.
    
*Overall:*
- LCA <= RMQ +/-1
- RMQ +/-1 <= RMQ (O(n log n), O(1)) + +/-1 simplification 

#### Teaser

The authors also prove one more reduction - from RMQ to LCA!
Thus, general RMQ is no harder than RMQ +/-1. 