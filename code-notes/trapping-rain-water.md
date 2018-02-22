### [Problem: Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description/)
Given n non-negative integers representing an elevation map where the width of each bar is 1, 
compute how much water it is able to trap after raining.

### Note: 
The accompanying code is [here]()

#### Thoughts:
- one can try out a few examples and see that if we think of a traditional left-to-right
sweep, we will have to maintain a lot of information such as the relative peaks etc. 
- interestingly, the main insight into this problem comes from the notion of the 
[Lovasz extension](http://www2.maths.lth.se/matematiklth/personal/fredrik/discreteoptimization/lecture-5_ulen.pdf)
	- (one version of) the Lovasz extension extends functions over {0, 1}^n to [0, 1]^n
	- uses precisely this water-filling procedure. 
- the core idea is to think of filling the water from the bottom, 
	- start at _sea level_ (i.e. level = 0)
	- look at a certain _level_ 
	and finding out how many slots in the array will get filled with water at that level. 
	- and keep incrementing the level.
	- of course when we say _increment_ the level, we do not need to increment by level += 1 but rather
	by the quantum that is indicated by the heights of the _peaks_ in the data.
