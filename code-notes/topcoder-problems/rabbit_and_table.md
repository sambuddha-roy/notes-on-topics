## [Problem RabbitAndTable](https://community.topcoder.com/stat?c=problem_statement&pm=14649)
#### Problem Translation:
You are given a matrix X of n integers. The problem is break up 
the indices into groups (picturesquely called _tables_) 
such that the index i is in a group of no more than X[i] others. 

Count the number of ways one can do this. Two ways are considered 
distinct if there are two indices who are partitioned differently
in these two ways.

#### Comments
- this is reminiscent of the [chinese restaurant process](https://en.wikipedia.org/wiki/Chinese_restaurant_process) (crp), 
where a new element attaches to one of the existing tables.
- this is like counting all configurations - the denominator in 
a bayesian calculation. 
- one more reason why one should know dp's well.
- we will anthropomorphize the rabbits and call them guys.

#### Thoughts/notes:
- let us first see how the problem breaks up. 
    - Suppose the array 
    X = [3, 3, 2], then for the first two guys, the total number of
    possbilities is (1, 1) (2 separate tables) or (2) (1 table with 
    both of them). 
    - Now think of adding the element 2 to this existing configuration. 
    - for the separate tables, (a) add the 1 to a new table or (b) to one
    of the existing ones. 
    - for the (2) configuration, we cannot add the new index/guy to 
    the existing table (constraint would get violated), so add a new
    table. 
- so we have to order the indices. maybe descending order, or ascending
order of their X-values; at this point this does not seem overly
important, apart from that there is an order in processing them.
- how do we get an efficient dp from this?
- **State**: let us play with the example above to see what can be a suitable
**state** for this problem.
    - in the state, we will keep i, the index into X, with the 
    understanding that we have processed all the indices upto i.
    - at this point, we would seem to need the _counts_ at the tables 
    created so far. if we have k tables so far, then we have to 
    keep these k numbers as state/memory.
    - hmm, keeping all these k numbers might turn out to be 
    costly.
    - in fact not all of these k numbers, we need to store a
    suitable _sketch_
        - how many of these k are _full_ (i.e. makes one of the
        *existing* constraints tight)
        - out of the non-full tables, how many are _full_ 
        corresponding to the _next_ X-value. Even these cannot be
        utilized. while placing a new guy wont violate an
        _existing_ constraint for the guys we have placed so far,
        it will violate the new guy's constraint.
    - given the above discussion, it seems like we should indeed
    process the indices in a specific order; at this point looks like
    a _descending_ order might be suitable.