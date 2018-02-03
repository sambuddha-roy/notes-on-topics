## [Problem ManageSubsequences](https://community.topcoder.com/stat?c=problem_statement&pm=14794)
#### Problem:
You are given three Strings S, A and B, each consisting of uppercase English letters. Limak wants to add characters anywhere into S, including its beginning and end, so that the new string has the subsequence A but doesn't have the subsequence B. 
Find and return the minimum possible number of added characters. 
If Limak can't obtain a string with the desired properties, 
return -1 instead.

#### Comments:
- this is the general form of the [Santa problem](https://community.topcoder.com/stat?c=problem_statement&pm=14776)
(change/add letters
so that a string has santa but not satan)

#### Thoughts:
- **Simplification 1**:
find a suitable simplification. In this case the simplification
is to consider the problem of just adding letters to S so that 
it has the substring A (i.e. leaving out the part about _not_
having the substring B)
    - this simplification seems manageable
    - think atomically. we will stream through S and consider 
    what happens at every character.
    - the right matrix entry should be M[i, j] - where i runs 
    through the indices of S, and j runs through the indices of
    A.
    - what should the entry _mean_? 
    - M[i, j] = minimum number of additions required for the 
    string S[i:] to have the substring A[j:] 
    - so M[0, 0] should have the solution.
    - setting up the recurrence is easy:
        - if S[i] = A[j], then M[i, j] = M[i+1, j+1]
        - else we have two choices, either add on A[j] or don't.
        - Here, M[i, j] = min{1 + M[i+1, j+1], M[i+1, j]}
    - and that's it for this simplification
- and now, **the original problem**: 
    - Here, we will have to add on 
one more dimension corresponding to an indexing through the 
string B. 
    - So, M[i, j, k]: i for indexing S, j for indexing A, k for indexing
B. 
    - what should M[i, j, k] mean? We should work like M[i, j] - i.e. 
find the minimum number of added characters, but we should also keep
a check on the _maximum substring_ - actually the maximum 
 _prefix_ of B that we have seen so far. 
This is the point of the third index k. 


