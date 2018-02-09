## [Problem: Buy and sell stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
#### Problem:
You are given the price of a stock for n days, i.e. an array 
of n (let's say) integers. 
You are allowed three operations on any given day: buy a stock, 
sell a stock or hold the stock. Initially, you start with no 
stocks. The objective is to buy a stock and then sell it 
(on a different day) so as to maximize the profit. 

#### Comments:
- We will actually go through a bunch of variants of this problem.
let's call the above version Variant 0.
- Some of the variants will be _natural_ and others will be _weird_
(kinda like generalization for generalization's sake :-)).
- we will indicate the time complexity. Also note that these are 
the thoughts for the pseudocode, and not the optimal (from pov 
of # of variables in the program to keep, etc.). 
- for instance in Variant 0, in an actual program, we will 
need to keep just two running quantities through the program 
(say, _minimum_ and _difference_ or so.)

#### Variant 0:
Given an array A = [a_1, a_2, ..., a_n] find indices i and j with 
i < j so that the amount a_j - a_i is _maximized_

#### Thoughts:
- given the optimal solution, a_i is surely the _minimum_ value 
among all the entries between [a_1, ..., a_j].
- similarly, a_j is the _maximum_ value between all the entries 
in [a_i, a_(i+1), ..., a_n]
- if not, we could have a better (i.e. higher) a_j - a_i value.
- this naturally leads to the following subproblem:
    - for each i, compute the _minimum_ of the _prefix array_
    [a_1, a_2, ..., a_i]. Store results in a new array P 
    (for prefix).
    - for each j, compute the _maximum_ of the _suffix array_
    [a_j, a_(j+1), ..., a_n]. Store results in a new array S 
    (for suffix).
- clearly if we have the two subproblems solved, then the original 
problem is easy:
    - for each index i, look at the entry P[i] and S[i+1] (while 
    observing obvious boundary conditions, etc.). The difference
    (between S[i+1] and P[i]) gives us a candidate.
    - max over all such candidates gives the answer. 
- good. can i solve the subproblem in O(n) time?
    - yes! For instance, for P: A sweep through the array while
    keeping on updating the minimum gives the answer. 
    - in formal terms, P[i] = min(P[i-1], a_i). either the minimum 
    corresponding to P[i] lies in the i_th term a_i or lies 
    before, i.e. is P[i-1]

#### Variant 1:
Given _two_ arrays A = [a_1, ..., a_n] and B = [b_1, ..., b_n]
find indices i and j (where i < j) such that the amount b_j - a_i is maximized.

#### Thoughts:
- the only observation is that we really do not need much of a change
from Variant 0. 
- here, P will depend on A and S will depend on B.
- rest stays the same. 
- hey, it is not even necessary that A and B are the same length.
    - but of course, in that case, the time complexity is 
    O(max(len(A), len(B))).
    
#### Variant 1a:
Given an array A = [a_1, ..., a_n], _functions_ f, g, 
find indices i and j (where i < j)
such that the amount f(a_j) - g(a_i) is maximized. 

#### Thoughts:
- easily maps to Variant 1. Given A, construct new arrays 
A_1 = [g(a_1), ... g(a_n)] and B = [f(a_1), ..., f(a_n)]
- in an actual program, of course we will not do this explicitly, 
and we will result in a O(n) algorithm.
- also, of course, nothing special about the _subtraction_ or 
_maximization_. we could as well be _minimizing_ (say)
f(a_j) + g(a_i)