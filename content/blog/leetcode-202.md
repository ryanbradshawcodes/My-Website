---
title: "Finding myself a Happy Number"
date: 2022-08-03
slug: "leetcode-202"
description: "LeetCode problem 202 - Happy Number"
keywords: ["guide", "website", "leetcode", "easy", "problem", "warwick", "university", "student", "13", "roman"]
draft: false
tags: ["leetcode-easy"]
math: false
toc: true
---

## Investigating the problem

[Problem 202 - Happy Number](https://leetcode.com/problems/happy-number/)

A happy number is a number defined by the following process:

* Starting with any positive integer, replace the number by the sum of the squares of its digits.
* Repeat the process until the number is 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
* Those numbers for which this process ends in 1 are happy.
  
Return true if n is a happy number, and false if not.

## Initial thoughts and prerequisites

For my solution you will need to know about:

* While loops
* For loops

The first thing I thought is that you would need to use recursion to continuously calculate the the sum of the square of the digits. To end the recursion the base case would be when the number inputted is equal to 1. But then I considered what would happen if you entered a number that was not happy. In that case the function would recursively call itself until the program breaks due to reaching its [maximum recursion depth](https://www.geeksforgeeks.org/python-handling-recursion-limit/). This meant I needed to either identify that a number was not a happy number, or introduce some sort of limit. I originally only came up with the idea of setting a limit so that is what I will continue with for now.

Regarding calculating the next number, this was not too complicated. In order to "replace the number by the sum of the squares of its digits" I would first need to separate the number into its digits. Then I could square each digit while also keeping a running sum of them. The final sum would then be the number that I would input into the function again if it was not equal to 1 (as if I got a 1 then that would mean it is a happy number and I can stop and return true).

## Developing the solution

I first looked up how to split a number into its digits in Python as it did not instantly occur to me how I could do it. I found out that I could do `int(digit) for digit in str(n)` which gave me an array of all the digits of the number. I stored this in a variable called `digit_array` and I also created a variable called `sum` to keep the running total of the calculations.

Next I needed to square each digit and add it all up, so I created a for loop to iterate through each index of the digit array. As the array was traversed each index would be set to its square, and this would get added to the `sum` variable. With my initial recursive solution I would then call the function again with this sum, I also made the base case for if the sum was equal to 1 at the top of the function. This solution worked for when you were given a happy number, but when you were not given one the program would reach the maximum recursion depth.

I then decided to use a while loop instead and keep count of how many times the while loop ran using a `count` variable. I then assumed that if the while loop ran 100 times that the input was not a happy number because it should have returned a 1 by this point. *This is not the most efficient way to do this* I will explore that in the reflection.

## Complete solution

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        count = 0
        
        # If n is 1 then it's happy, otherwise loop up to 100 times
        while (n != 1 and count < 100):
            # Split number into an array of its digits
            digit_array = [int(digit) for digit in str(n)]
            sum = 0

            for i in range(len(digit_array)):
                digit_array[i] = digit_array[i] * digit_array[i]
                sum += digit_array[i]
                
            count = count + 1
            n = sum
        
        # If it looped a 100 times then assume it's unhappy
        return True if count != 100 else False
```

## Reflection

I assumed 100 loops will be enough to determine whether the number is happy or not, which works for all 404 test cases given in the problem. Hence the next question I had for myself is what is the *minimum* number of loops required to allow the program to solve the problem as quickly as possible? I was told in the question that if you were given a number that was not happy the calculation process would result in an endless **cycle**. This is a large clue as a cycle indicates that the numbers will get repeated. Therefore, I could have created a [set](https://www.w3schools.com/python/python_sets.asp) and added the starting number as well as the subsequent calculated numbers. Then if there was a number that already existed in the set, I would know a cycle has occured and that the number is not happy.

This problem helped make me appreciate that I should try and optimise my solutions as well, rather than simply accepting a more naive solution.