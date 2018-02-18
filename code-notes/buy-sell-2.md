## [Problem: Buy and sell stock - II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Note: 
This is a followup to [Buy and sell stock - I](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md)
and will refer to some of the notation/variants in that post.

### Variant 6
You are given the price of a stock for n days, i.e. an array 
of n (let's say) integers. 
You are allowed three operations on any given day: buy a stock, 
sell a stock or hold the stock. 
- initially, you start with no stock. 
- allowed k transactions
- at most one transaction (buy/sell) at a time
- transactions do not need to be _disjoint_, so at any
point, you may _hold_ two stocks.
- objective is to maximize the overall **profit**. 

#### Thoughts
- the trick applied for Variant 5 for 2 transactions will not 
scale because the number of **events** will blow up 
exponentially, giving us O(2^k n). 
- we need another trick.
- to be finished.
- the following does not work. yet.
- let's think in terms of the _state_ that we have to maintain.
- on any given day, we have to maintain
    - the index of the day, i.
    - number of transactions finished, call this r
    - number of transactions unfinished, i.e. the number
    of stocks we are _holding_, call this t. 
- and given this state, can we draw up a recurrence? again, 
we will consider the _participation_ on any given day, 
say day `i+1`:
    - hmm. how to proceed.

### Variant 6a. (general precedence orders on indices)
Variants 5 and 6 refer to specific precedence orders on the 
indices. What if we had a general DAG as a precedence order
on the indices?

### Variant 7 (unlimited transactions)
Here we are allowed as many transactions as possible, but 
with the constraint that at most one stock is held at any point
of time (so the transaction timelines are _disjoint_).

#### Thoughts
- this would following from the following Variant 8, with the transaction
cost t set to 0. 

### Variant 8 (unlimited transactions, transaction cost)
Same setup as Variant 7, but now we have transaction costs for 
each transaction. Each transaction cost is t.

#### Thoughts
- one first (greedy) thought can be: validate all transactions for which 
 a[i+1] > a[i] + t. 
- but this - considering only transactions involving _consecutive indices_ - is wrong. 
it might be that a[i+1] and a[i] are too
close, and this invalidates this transaction, and also 
the transaction a[i+1] -> a[i+2] is invalid for this reason,
but the transaction a[i] -> a[i+2] is valid and profitable.
- can we still be greedy? that would mean, that
    - start with a[i] and bypass elements till we get a[j] > a[i] + t
    - issue: what if multiple elements a[i] want to land/pick up the same
    a[j]?
    - say, t = 2, and the array is [2, 1, 5]. 
    - going greedily would end up with 2 getting paired with 5. optimal 
    is 1 getting paired with 5.
- ok we need to pair the _minimum_ so far with the new element.
- so, the algorithm should look like this:
    - sweep through the array
    - for each new array element, enter into the heap
    - compare top of the min heap with current element. if difference
    more than t, then this is a transaction, pop the top element
    from the heap.
    - continue.
- not liking the O(n log n) complexity here, will have to think more.

### Variant 9 (123-patterns)
Given an array A = [`a_1, a_2, ..., a_n`] find if there 
are three indices `i < j < k` such that `a_i < a_j < a_k`.

#### Thoughts:
- Like for the thought process going into [Variant 0](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#variant-0), 
we maintain two auxiliary arrays
    - `X = [x_1, x_2, ... , x_n]` where `x_i = 1` if there is 
    an index j such that j < i and `a_j < a_i` in the array A.
    - `Y = [y_1, y_2, ..., y_n]` where `y_i = 1` if there is 
    an index j such that `j> i` and `a_j < a_i` in array A.
- Note (like in Variant 0), if we had these two arrays with 
us, it is easy to detect a 123-pattern:
    - **stitching**: we need to find an index i such that both x_i and y_i
    are = 1. 
- So we may focus on constructing arrays X, Y from A in 
O(n) time. 
    - This is quite easy once we realize that the array X
    is encoding the _minimum_ of a prefix of A: x_i = 0 
    indicates that this index i is the _local minimum_ of 
    the prefix of array `A[:i]`
    - Thus given the minimum array (as in 
    [Variant 0](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#variant-0))
    we may construct the array X.
    - We can also construct array Y similarly. 
- thus, done with overall problem in O(n) time.

### Variant 9a (123-pattern, quantitative version)
Given an array A = [`a_1, a_2, ..., a_n`] find if there 
are three indices `i < j < k` such that `a_i < a_j < a_k`, 
_and maximize the difference (`a_k - a_j + a_j - a_i`) = (`a_k - a_j`)_
under these constraints. 

#### Thoughts:
- This is a slight extension to Variant 9, where when we _stitch_
arrays X, Y, we also maintain the corresponding minimum (in the
prefix of A) and the maximum (in the suffix of A). 
- Running through the stitched array gives us the answer.

### Variant 9b (1234-pattern)
Given an array A = [`a_1, a_2, ..., a_n`] find if there 
are three indices `i < j < k <l` such that `a_i < a_j < a_k < a_l`.

#### Thoughts:
- Like we proceeded from [Variant 0](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#variant-0)
to [Variant 2](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#variant-2-two-transactions)
we may proceed here too. 
- We decompose into components `i < j < k` and `k < l`.
- We ask: like we detected, in Variant 9, if an array A contains a 123-pattern, 
can we detect the presence of a 123-pattern, in O(n) time, _for every prefix of array A_? 
    - like in [Variant 2](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#variant-2-two-transactions),
    we can branch on _participation_. Given index s (and prefix array A[:s]), 
    either 
        - there is a 123-pattern in A[:s-1]
        - or A[s] _participates_ in a 123-pattern. For this, we need a
         12-pattern in A[:s-1] such that A[s] > the _2_ of the 12-pattern.
        - Note that the array X in 
        [Variant 9]() actually encodes the 12-patterns, 
        but, we really cannot afford to go through all the 12-patterns 
        in X[:s-1] and check if A[s] is greater than the corresponding _2_. 
        - that would end up costing us O(n^2) time overall.
    - so how? 
    - let's see what we really want from the array X. 
        - Recap: array X has a 
        1 in index i, i.e. x_i = 1 iff there is a 12-pattern ending in index i
        in the original array A. and, we can compute X from A in O(n) time.
        - what we really need from X is the following minimum: min{a_k: a_k is the 
        end of a 12-pattern}.
        - Thinking back on a similar situation in [Variant 2](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#variant-2-two-transactions),
        we see that there, we had to consider the _minimum_ of the prefix of A. Here,
        we have to consider/compute the minimum as indicated above. 
        - But this is one 1-time pass 
   

### Variant 10 (132-pattern)
Given an array A = [`a_1, a_2, ..., a_n`] find if there 
are three indices `i < j < k` such that `a_i < a_j` and 
`a_i < a_k < a_j`.

#### Thoughts
- 

