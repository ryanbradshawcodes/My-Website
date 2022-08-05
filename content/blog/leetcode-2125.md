---
title: "Counting the number of laser beams in a bank [WIP]"
date: "2022-08-04"
slug: "leetcode-2125"
description: "LeetCode problem 2125 - Number of Laser Beams in a Bank"
keywords: ["guide", "website", "leetcode", "medium", "problem", "warwick", "university", "student", "13", "roman"]
draft: false
tags: ["leetcode-medium"]
math: false
toc: true
---

## Investigating the problem

[Problem 2125 - Number of Laser Beams in a Bank](https://leetcode.com/problems/number-of-laser-beams-in-a-bank/)

You are given a 0-indexed binary string array bank representing the floor plan of the bank, which is an m x n 2D matrix. bank[i] represents the ith row, consisting of '0's and '1's. '0' means the cell is empty, while'1' means the cell has a security device.

There is one laser beam between any two security devices if both conditions are met:

* The two devices are located on two different rows: r1 and r2, where r1 < r2.
* For each row i where r1 < i < r2, there are no security devices in the ith row.

Laser beams are independent, i.e., one beam does not interfere nor join with another.

Return the total number of laser beams in the bank.

## Initial thoughts and prerequisites

## Developing the solution

## Complete solution

```python
class Solution: 
    def numberOfBeams(self, bank: List[str]) -> int:
        totalBeams = 0
        row_ones_prev = 0
        
        # For each row of the matrix
        for j in range(len(bank)):
            # Count the number of 1s in a row
            row_ones_curr = sum(1 for i in bank[j] if i == "1")
            
            # Don't allow for multiplications with 0
            if (row_ones_curr > 0):
                totalBeams += row_ones_curr * row_ones_prev
                row_ones_prev = row_ones_curr
        
        return totalBeams
```

## Reflection