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
- not liking the O(n log n) complexity here, will have to think more 
to get O(n).

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
- Note (like in [Variant 0](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#variant-0)), if we had these two arrays with 
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
        - So, wait! what we essentially want is a min-prefix kind-of operation on the 
        array X! 
        - i.e. in _one_ 1-time pass on the array X we create a new array Z
        such that 
            - Z[i] = min{a_k: k <= i and k is the end of a 12-pattern in X[:i-1]}.
        - Given X and A, this array Z is easy to construct. Let's see a sample run:
            - for eg. let A = [2, 3, 4, 1]
            - so X = [0, 1, 1, 0]
            - Now, we have to consider only elements of A where x_i = 1, so that we 
            have X_A = [0, 3, 4, 0] and considering the min_prefix of this, we get the
            array Z = [0, 3, 3, 3] - here an entry being 0 means there is no 12-pattern
            ending at or before this index. 
            - For another example of 0's appearing in 
            the array Z, consider A = [4, 3, 2, 1] so that X = [0, 0, 0, 0].
            
- **clean up and finish**: 
    - **clean up**
        - continuing, either there is a 123-pattern in A[:s-1]
        - or A[s] participates in a 123-pattern. For this, we check if A[s] > Z[s-1]
        - so overall, we get detection of a 123-pattern in every prefix of array A in 
        O(n) time!
        - We encode this information in an array D (note how quickly we are running 
        out of good letter-names for our constructed arrays :-))
    - **finish**
        - We will use (i.e. _stitch_) the array D and the array Y in [Variant 9](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-2.md#variant-9-123-patterns) to decide if there is 
        a 1234-pattern in the original array A. 
        - total time taken = O(n)

### Variant 9c (12-pattern with max 1)
Here we are given the array A = [`a_1, a_2, ..., a_n`]  and we are to find two indices 
`i < j` such that `a_i < a_j` _and_ under this condition we are to find the largest `a_i`. 

#### Thoughts
- Given the array A, let us construct the (max-suffix) array B: B_i = max(A[i+1:])
- Now do one more sweep of the arrays A and B in tandem, outputting the max A_i such that 
A_i < B_i. 
- finito.

---

#### Interlude:
- Like we mentioned in the [Interlude](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-stock.md#interlude)
on the previous post, this is a scenario where we can compute a certain quantity 
not just for the full array but for every possible _prefix_ of the array, all 
in linear, i.e. O(n) time.
- The reader is invited to follow up on the two extra variants
    - the _quantitative_ variant of the 1234-pattern a la [Variant 9a](https://github.com/sambuddha-roy/notes-on-topics/blob/master/code-notes/buy-sell-2.md#variant-9a-123-pattern-quantitative-version).
    - the 123..k-pattern for an arbitrary k. we should be able to do this in 
    O(kn) time. 
        - But can we do better than this? For instance, if k = n, we are 
        essentially asking if the array is sorted, which is O(n) time rather 
        than O(kn) = O(n^2) time.

--- 

### Variant 10 (132-pattern)
Given an array A = [`a_1, a_2, ..., a_n`] find if there 
are three indices `i < j < k` such that 
`a_i < a_k < a_j`.

#### Thoughts
- we can observe the following structure. Given an array that indeed has such indices `i < j < k`
we can assume that `a_i` is the minimum of the array A[:j] (i.e. elements of A up to and not including 
j, though including j will not make a difference in this case).
- also, the elements `j < k` in the remaining array A[j:] constitute something like Variant 9c above, a
12-pattern with max 1. 
- so we would apply divide and conquer: break up the array at index j. in A[:j] (i.e. the prefix), find the
minimum element, while in A[j:], the suffix, find a 21-pattern with max 1 (note that Variant 9c talks about a 
12-pattern and here we need a 21-pattern). 
- question: while variant 9c involves finding such a _max 1_ for a single array, in order to effectively divide
and conquer, we would need these values for every suffix A[j:] of the array A. Can we do this?
	- yes: given array A, let us construct the max-suffix array B, where B[i] = max(A[i:]) (in O(n) time)
	- given array B, traverse from right to left (i.e. from end to beginning), and create array C, where 
	`C[i] = (True, B[i+1])` if `B[i] > B[i+1]` else `C[i] = (False, Empty)`.
	- what does array C encode? array C essentially maintains the 21-patterns with max 1 for every suffix of the array A
	as desired. 
- we also need to use D that is the min-prefix array for A i.e. `D[i] = min(A[:i])` (where A[:i] does not include the 
i_th element).
- given the arrays C and D, we can quickly compute the answer by traversing once more, and checking if `C[i][0] = True` and 
`D[i] < C[i][1]`. Done.


### Variant 11 (arbitrary permutation pattern)
Here, we are given an array A = [`a_1, a_2, ..., a_n`], and we want 
to check if there is a _permutation_ pi in it. 
Clearly Variants 9, 10 are special cases of this Variant, for specific
permutations.

#### Thoughts:
- we can extend Variant 10 to 1432-patterns, but not clear how much more mileage those stitching ideas have. 
- no clue in the general case. 
