---
title: "Roman to integer converter"
date: 2022-07-31
slug: "leetcode-13"
description: "LeetCode problem 13 - Roman to integer converter"
keywords: ["guide", "website", "leetcode", "easy", "problem", "warwick", "university", "student", "13", "roman"]
draft: false
tags: ["leetcode easy"]
math: false
toc: true
---

## Investigating the problem

[Problem 13 - Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

Roman numerals are represented by seven different symbols: `I, V, X, L, C, D and M`.
Roman numerals are usually written largest to smallest from left to right. 
However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. 
Because the one is before the five we subtract it making four. The other special cases are:

* `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
* `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
* `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

## Initial thoughts and prerequisites

For my solution you will need to know about:

* Dictionaries
* For loops (indexing strings)

The first thing I considered when reading this problem was that I would need to take the symbols from the given string and convert them into their integer values. The easiest way of doing this that I am aware of is through a dictionary. A dictionary allows you to define pairs of keys and values, and collect them together under a single variable. Please [click here](https://www.w3schools.com/python/python_dictionaries.asp) if you would like to learn more about them.

In the previous paragraph I mentioned how I would have to "take the symbols from the given string". So the next step is thinking about this problem programatically. As this is a process that I am going to have to repeat (as I need to take each symbol from the string of up to 15 characters), it makes sense to use a loop. As I already know how many times I want to loop (to get every symbol) it makes more sense to use a for loop. Please [click here](https://www.w3schools.com/python/python_for_loops.asp) if you would like to learn more about them.

I have been told that Roman numerals are written largest to smallest from left to right. However, when this rule is broken it means that we instead perform a subtraction. Therefore, this indicates that I am going to have to compare each symbol to see if this ordering is broken. When the ordering is broken, I will need to perform a subtraction rather than an addition. Programatically, there is a branch in operation here depending on a certain condition. This is called selection and it is made possible through if, elif (else if), and else statements. Please [click here](https://www.w3schools.com/python/gloss_python_if_statement.asp) if you would like to learn more about them.

## Developing the solution

I firstly created the dictionary to map Roman numerals to their respective values. I called it conv (short for conversion), as that is an informative name that makes sense!

```python
conv = {
    'I': 1,
    'V': 5,
    'X': 10,
    'L': 50,
    'C': 100,
    'D': 500,
    'M': 1000
}
```

I then created a `value` variable and set it equal to 0. This acted as my running total that I returned once all the operations of the program have been completed.

The next step was to create the for loop as such: `for c in range(len(s) - 1)`. The letter `c` is a variable and hence can be called anything. The variable `c` will iterate from 0 up to the length of the string minus 1. *Why minus 1?* This will make sense in the next paragraph.

I now needed to compare the symbol I was currently on in the for loop with the next symbol to determine whether I had a special case or not. For example, if the input string was `II` then I would compare the values of the two symbols together and see that they are the same. This would mean that I would simply add the values to the running total. However, if the input string was `IV` I would compare the values and see that the rule "Roman numerals are usually written largest to smallest from left to right" has been broken so I would need to perform a subtraction. As in each iteration of the for loop I need to make a comparison to the **next** iteration, that means that the for loop cannot run when `c` is the last symbol. This is because I would be trying to compare the last symbol to the next symbol (which does not exist!) and that would give me an error.

Using the variable `c` that iterates through the input string `s`, I indexed the string by doing `s[c]`. I then had to convert this symbol to its numberical value using the dictionary called `conv`. If we encountered a special case then I subtracted the value. For example with `IV` the if statement would be true so I would subtract 1 from the `value` variable (now -1). Then I would look at the next symbol and add its value so we get 4 as expected. When there is not a special case, just add the symbols value to the `value` variable.

```python
if (conv[s[c]] < conv[s[c + 1]]):
    value -= conv[s[c]]
else:
    value += conv[s[c]]
```

The final piece of this puzzle is to recognise that the program will not currently work. Try it yourself with the input string `LVII`. *Can you recognise what is wrong, why, and then how we could fix this?*

As the for loop does not iterate for the last symbol in the string (it is only accessed during the final comparison) it means that the last symbol's value does not get added to the running total. This can be fixed in various ways but I decided to make a simple addition of the last symbol's value to the final return statement. Note that the index of the last symbol is defined as `len(s) - 1` since in programming you start counting at 0. For example, the string `Hello` has length 5, but to access the letter `o` you need to index the 4th position of the variable storing the string.

```python
return value + conv[s[len(s) - 1]]
```

And there we have it, a working solution!

## Complete solution

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        # Dictionary to correlate symbol and value
        conv = {
            'I': 1,
            'V': 5,
            'X': 10,
            'L': 50,
            'C': 100,
            'D': 500,
            'M': 1000
        }
        
        value = 0;
        
        # Read string by character
        # Should be largest to smallest
        # If not (IV) then subtract
        
        for c in range(len(s) - 1):
            # Handle special cases (e.g, IV)
            if (conv[s[c]] < conv[s[c + 1]]):
                value -= conv[s[c]]
            else:
                value += conv[s[c]]
                
        # Add the final symbol
        return value + conv[s[len(s) - 1]]
```

## Reflection

Although this was an easy problem, it did require a fair amount of thought to determine how to effectively deal with the special cases. I also had not used dictionaries in a while so it was a nice refresher for that.