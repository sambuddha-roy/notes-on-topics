## [Problem IncreasingSequences](https://community.topcoder.com/stat?c=problem_statement&pm=13278)
#### Problem:
You are given two int[]s L and R, each of length n.

Find the number of strictly increasing sequences of 
integers A[0] < A[1] < ... < A[n-1] such that 
L[i] <= A[i] <= R[i] for every i. Return this number modulo 
998244353.

#### Approach notes:
- the number 998244353 is just another prime like 10^9 + 7 or so.
- try simplifying the problem
- **simplication 1**:
    - Replace R[i] by R[i] - L[i] and L[i] by 0. 
    - The problem stays essentially the same, but don't have to 
    bother with two arrays R and L
- **simplification 2**:
    - Since the output sequences need to have A[0] < A[1] < .. < A[n-1],
    suppose we have L[0] >= L[1], then we really will never manage to 
    have A[0] = L[0] - for then there really is nothing to put for 
    A[1]. So we should have L[0] < L[1], as also L[1] < L[2] etc. 
    - example output L = [2, 4, 6, 7]
    - So, from the end of the L array, keep enforcing this. 
- **Last step**:
    - we are close to the finish now.
    - For some time it felt like we should be just able to produce
    a _formula_ to compute the total number of sequences. 
    - this would be a *rules*-based approach. 
    - instead, adopt the *data*-oriented approach - dynamic programming.
    - also recall for yourself the familiar dynamic programming styled
    program for computing something as formulaic as the Fibonacci
    numbers. 
    - So! with this hindsight, we know what to do. Keep a matrix 
    M[n, k] where n = # of entries in array L, and k = L[n-1], the 
    largest of the L-values. 
    - M[i, j] = the number of sequences _ending_ in number j among 
    the _first_ i entries of the matrix L. 
    - the recurrence should be trivial to frame.
- Done. 
       
#### Variants
- easy to consider the non-decreasing sequences version where we 
allow A[0] <= A[1] <=...<= A[n-1]
- more interesting is to consider weird sequences where the sequence
(A[0], A[1], ..., A[n-1]) consists of _two_ increasing _chains_ 
(instead of just _one_ as in the topcoder problem):
    - this is also easy to do
    - keep, in effect, another _pointer_ marking where this
    change happens as we walk through the DP matrix.
  