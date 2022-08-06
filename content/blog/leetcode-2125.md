---
title: "Counting the number of laser beams in a bank"
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

You are given a 0-indexed binary string array bank representing the floor plan of the bank, which is an m x n 2D matrix. bank[i] represents the ith row, consisting of '0's and '1's. '0' means the cell is empty, while '1' means the cell has a security device.

There is one laser beam between any two security devices if both conditions are met:

* The two devices are located on two different rows: r1 and r2, where r1 < r2.
* For each row i where r1 < i < r2, there are no security devices in the ith row.

Laser beams are independent, i.e., one beam does not interfere nor join with another.

Return the total number of laser beams in the bank.

## Initial thoughts and prerequisites

For my solution you will need to know about:

* Conditionals
* For loops (indexing strings)

My original thought process was that I could search through the matrix until I found the first security device (indicated by a '1'). Then I would skip a row (because of condition 1) and begin searching through the entire row, counting any device I came across. Once that row was exhausted that would conclude the counting of beams for the first device I came across (because of condition 2 - the devices I just counted would be blocking any other devices on subsequent rows). Then I would continue the search of the whole matrix, until another device was found, and then repeat the process. I think this has the capacity to work, but I might have been making some mistakes and eventually I attempted a different approach. This also would have been a slower approach I imagine.

My next approach was more mathematically inclined and it worked by counting the number of devices on the first row, then multiplying that with the number of devices on the next row (provided it contained a device). The result of this multiplication would be added to a running total and other multiplications would occur for every pair of rows and get added to the running total. This would work because you are finding all the ways to connect some number of devices to some other number of devices - which is calculated through multiplication. For example, if you had one device on one row and another on the next row, there would be one beam connecting them (which is 1x1). Again, imagine two devices on one row and two devices on the next. For each device it will connect to two others which can be expressed as 2x2.

## Developing the solution

As the second approach was more successful I will only develop that one. The first stage is to count the number of devices on the first row. That's simple enough as you can loop through the entire first row and whenever you come across a '1' you can increment the count. A quick search showed that I could do this in a singular line using Python.

`row_ones_curr = sum(1 for i in bank[j] if i == "1")`

By containing this line of code in a for loop that traverses each string (row) in the matrix `for j in range(len(bank))` this allowed me to count the number of devices on each row. However, for this solution it was important to count the devices on a specific row and then perform some calculations, rather than collecting all the counts first.

As stated earlier, only rows containing devices needed to be considered. This is because if a row contained no devices, it would not change the number of beams in any way since it is essentially just empty space. Therefore a simple if statement allowed the program to ignore these empty rows. If the row contained at least one device then the program performed the multiplication mentioned above and the result would be added to the running total.

```python
if (row_ones_curr > 0):
    totalBeams += row_ones_curr * row_ones_prev
```

Note here that the program multiplied by a variable called `row_ones_prev`. I defined this as 0 at the start of the program, so the `totalBeams` variable will get incremented by zero. This is because we are looking for pairs of rows of devices, and the program has yet to discover a pair. The pairing functionality was added by setting the `row_ones_prev` to the `row_ones_curr` variable. This meant that for the next row containing devices, the devices on the pair of rows would get multipled and added. This process would then continue until all rows have been traversed.

Then all that was left was to return the final answer!

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

This was possibly the first ever medium question I completed. So I was very pleased about that, although unfortunately I was not able to complete it without having a quick look at some other example submissions to see the general idea of how to solve it. I think this problem has helped me work on my ability to abstract the important information out of a problem description. Although the diagram they provided was helpful for my initial understanding, I think it made me overcomplicate the problem slightly which led to me failing it a few times.

Looking at other submissions it seems like I can definitely optimise the code, but I seem to have used the optimal approach to solving the problem. The running time of my solution would be O(n) where 'n' is the number of rows, and this is derived from the fact I am using a for loop that iterates from 0 to n. All other operations are completed in O(1) time hence the for loop is the only significant element of the program. Therefore any optimisations would not make a massive difference in the running time, but of course it is always best to strive for time and space efficient code.

In hindsight the code for this problem was very easy, but it was the understanding of the problem itself which made it more complicated in my opinion. Nevertheless I feel more confident to approach more medium-rated questions now.