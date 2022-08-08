---
title: "Jump around!"
date: 2022-08-06
slug: "leetcode-55"
description: "LeetCode problem 55 - Jump Game"
keywords: ["guide", "website", "leetcode", "medium", "problem", "warwick", "university", "student", "55", "jump"]
draft: false
tags: ["leetcode-medium"]
math: true
toc: true
---

## Investigating the problem

[Problem 55 - Jump Game](https://leetcode.com/problems/jump-game/)

You are given an integer array `nums`. You are **initially positioned at the array's first index**, and each element in the array represents your **maximum** jump length at that position.

Return `True` if you can reach the **last index**, or `False` otherwise.

## Initial thoughts and prerequisites

The first thing that I did was review the first example provided where `nums = [2,3,1,1,4]`. I noticed that there were multiple ways to get to the last index here. For example, you could go from index 0 to 2 then 2 to 3 and finally 3 to 4. Alternatively, you could go from index 0 to 1, then 1 to 4. Regardless, the output of the program is `True` as the problem only wants to know if it is possible to traverse the list or not, rather than finding the *minimum* number of moves requires. That means I could focus my efforts on understanding the conditions required to reach the last index.

Considering the given second example which included a 0 as well as the constraint `0 <= nums[i] <= 105`, I realised the only number that could hinder the ability to traverse the list would be a 0. Therefore, the problem is now identifying whether you can 'jump' past a 0 or not. If you land on a 0, then you cannot progress any further, but if you can land on any index past the 0, then you might be able to make it provided there are no more 0s that you cannot traverse. This means the program will need to search for 0s, and then see if there is a number before that 0 that allows for the jump to take the index to at least 1 past the index of the 0.

*How can you tell if you can traverse past the 0?* Looking at the second example where `nums = [3,2,1,0,4]` I imagined searching through the list for a 0. This would result in finding a 0 at index 3. I then thought that you could search backwards, seeeing if the previous numbers were larger than the gap needed to get to the index past 0. For example, looking at index 2 there is the number 1. This jump would get you to the 0, but not past it. That means we need to continue looking back to the next index which has value 2. This results in the same issue of only making it to the 0. Finally this is also the case for index 0. Once we have exhausted our backwards search that means I can return `False` as I know it is impossible to get past the 0.

## Developing the solution

I also wanted to consider the extreme case of an empty list to begin with. This should return true as the list has technically been fully traversed. The same can be said if there is only a singular number since it is both the starting and ending index. This allowed me to implement a first check.

```python
if (len(nums) == 0 or len(nums) == 1):
    return True
```

The next part to implement was the searching for the 0, and then seeing if that 0 could be traversed past. This section starts with a for loop to iterate through every index of the given list, as well as a simple if check to look for the number 0.

```python
for i in range(len(nums) - 1):
    if (nums[i] == 0):
```

Then I need to start searching backwards from the 0 to (and including) index 0. If the number before the 0 is greater than the gap between that index and the index of the 0, then the traversal can continue forwards. If the number cannot allow for the traversal over the 0, then we need to continue searching backwards until we find a number that can. If we make it to index 0 that means we have exhausted the options and can return false.

```python
for j in range(i, -1, -1):      
    if (nums[j] > i - j):
        # Can make it past 0
        break
    else:
        # Can't make it past 0
        if (j == 0):
            return False
```

The final step is to include the `True` condition. This is achieved when we fully iterate through the first for loop, as that means that the program will have reached the last index. This means I can place `return True` at the bottom of the program outside the for loop to achieve this.

## Complete solution

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        # Would already be at the last index
        if (len(nums) == 0 or len(nums) == 1):
            return True
    
        for i in range(len(nums) - 1):
            if (nums[i] == 0):
                for j in range(i, -1, -1):      
                    if (nums[j] > i - j):
                        # Can make it past 0
                        break
                    else:
                        # Can't make it past 0
                        if (j == 0):
                            return False
        
        return True
```

## Reflection

This took me quite a few tries to implement however the problem solving aspect of it came very quickly. The reason it took so many times for me to implement it was largely due to a silly mistake where I was not looping through `j` properly, causing me to not allow `j` to equal 0.

The time complexity of this solution is $O(n^{2})$ which can be derived from the use of the nested for loop. Looking at other solutions it appears that there are solutions in $O(n)$ time, so it is clear that my solution is not the most time efficient. To improve my solution I believe I will actually need to change the fundamental idea behind it as it is not necessary to focus on the elements with a value of 0.

Instead, I can just look at the maximum reachable index at each index, and if this is greater than or equal to the last index I can return `True`.

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:        
        maxIndex = 0
        
        for i in range(len(nums)):
            # Keep track of the maximum, reachable index
            if (nums[i] + i > maxIndex):
                maxIndex = nums[i] + i
            
            # Don't traverse past where it can't reach
            if (i == maxIndex):
                break
        
        # If at/past last index return True
        return maxIndex >= len(nums) - 1    
```

By doing this, I make the program about 200ms faster!