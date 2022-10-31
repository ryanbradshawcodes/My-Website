---
title: "Creating a Sudoku solver in C [WIP]"
date: "2022-08-06"
slug: "sudoku"
description: "Talk through my project of creating a Sudoku solver in C"
keywords: ["guide", "project", "google", "domain", "warwick", "university", "student", "C", "sudoku"]
draft: false
tags: ["project"]
math: false
toc: true
---

## Motivation

I know that my second year as a Computer Systems Engineer will involve a lot of programming in C. To prepare for this I wanted to ensure that my fundamental knowledge was at a high standard. The best way I know of doing this is by completing a personal project using the language! I have opted to create a sudoku solver because I think it will be hard enough to give me a good challenge, but it is not so difficult that it would takes months to complete. I also quite like Sudoku, so that will hopefully keep me determined to finish this project.

I am also going to try and work on my programming skills, meaning that I will limit the tutorials I watch and try and figure out more things by myself, only using google to search up key questions that I have conjured up through planning. This will be reflected in the development portion of this blog as it will likely be a lot longer than my other projects as there is a lot that I need to consider. As such, I will split the development section up into a planning section (where I will not be coding anything) and then a coding section (which will involve...coding). 

The planning section will be more 'natural' in the sense that it will showcase my thinking before I have actually completed the project. The code development will likely be more refined and completed after I have completed the project such that it is not too long.

## Prerequisites

## Planning

### Abstraction

The first thing to do is to clearly outline the project goal. To do this I have to imagine what I want to be able to do with the final product, and then succintly describe that. What I am imaging is that the program is run and then a board is generated in the terminal. I could either have this board be randomly filled with numbers, or I could allow the user to enter their own Sudoku problem. Then once the problem is shown, I could just ask the user to press enter which will start the solution generation process. Once the Sudoku is solved it will be outputted to the terminal. I could also involve some file handling here with the output (and user input if I decided to choose that route). This could work by the output to the terminal also getting sent to a file so that the user can keep it. For the input, there could be a file with a Sudoku board that the user could fill out. This would likely be more intuitive that trying to fill out the board using the command line.

That is basically it for the program as a whole. I have some options for the input, then there is the process of solving the Sudoku, then I have an idea of how I want the output to be. Now I need to consider the process part of this project which is going to be the *hard* part. I think I will employ some abstraction here to try and make this easier to figure out. So generally a Sudoku board consists of 9 grids, each of which contain 9 cells. 

![9x9 Sudoku board](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Sudoku_Puzzle_by_L2G-20050714_standardized_layout.svg/1200px-Sudoku_Puzzle_by_L2G-20050714_standardized_layout.svg.png)

I am just going to consider 1 of these grids for now to see if I can figure out how I may go about creating this solver. This means I only have 9 cells in total to work with (rather than 81) and to complete the Sudoku I need to follow the [normal rules](https://www.sudokuonline.io/tips/sudoku-rules) but scaled down.

![3x3 Sudoku board](https://hackernoon.com/hn-images/1*dFNtAnfevAa9wP-VQ10oXQ.png)

So the first step is considering how this board will be stored as that will govern how it can be accessed. I believe the only sensible way of storing a grid like this would be through the use of a 2D array. So with the image above, it would be formatted as such in C `int grid[][] = {{2, NULL, 3}, {1, NULL, NULL}, {NULL, NULL, 1}}`. *Now how do I solve this* The first case I am noticing is when there is only 1 missing cell in a row (in this case the top row). The final number to input in the top row can be figured out very easily, all I have to do is look for which number is not already in the row. *Is 3 in the top row?* no *Is 2 in the top row?* no, that means it can only be a 1 in this position. I can use the same logic for diagonals, which allows me to fill in the middle cell with a 3. From there I now only have one missing cell in the middle column, so I can fill that with a 2 using the same logic. At this stage our grid is defined as such `{{2, 1, 3}, {1, 3, NULL}, {NULL, 2, 1}}`. As there is one NULL cell in each row/column, they can also be solved easily and we can complete this Sudoku. This was a very nice starting grid, but what if you are given fewer numbers? That also has made me consider how I would randomly generate a new Sudoku problem, what conditions need to be met to generate a new valid Sudoku board? *Can it even be invalid?* 

I will consider the extreme case where you are given no numbers to start with. You just have a blank 3x3 grid and the Sudoku rules in the back of your head. The first move cannot be invalid, since it cannot possible break any of the rules. The subsequent moves are then susceptible to being invalid. As you can do any move to start with, that means you can start at any cell with any number. So each cell has 3 possible numbers, and there are 9 cells to pick from. So there are 27 available starting moves and not one singular valid, completed board. So is the general consensus of having a Sudoku problem that there should only be 1 valid solution? If that is the case randomly generating a new Sudoku problem seems like quite a challenge in itself. Nevertheless, for the process of solving the Sudoku it would not matter if you were given an empty grid as you can still solve it (much more easily than if there were numbers on it). The challenge comes from if there are already some numbers on the board in that case. So I need to consider an example where it is not as easy to figure out as the previous 3x3 we solved.

In the example there were four numbers given. As I have just recognised if you were given no numbers then it would be easy as you can just fill out each row and alternate the order of the numbers for each row to abide by all the Sudoku rules. If you were given just one number it would essentially be the same story. I think the difficulty starts to increase when you have two numbers. I think this is the case because this is when you can start to have invalid moves that are not instantly apparent. I have an example of this in the video below. The first time I worked through the Sudoku I solved it, however I then demonstrated that you can encounter an invalid move.

* Note that there was not a single number to fill out on each row meaning the algorithm would need to look at both the row and column

{{< youtube 152n02A0aIs >}}

So, I can sometimes encounter a situation where there are multiple valid numbers that can be entered into a cell, though it may lead to an error later on. This means I am going to need a way to *backtrack*, esentially replicating the undo button when a Sudoku rule is broken. I think I can break the solving algorithm down into succint steps:

1. Search through the 2D array, starting from the top left cell
2. When the first empty cell is encountered, check the numbers in the same row and column
3. Generate a list of all available numbers that can be entered
4. Enter the first of the remaining numbers and then search for the next empty cell
5. Continue this process until entering all available numbers breaks a rule, then backtrack to the previous insertion where there was a choice of valid number
6. If you exhaust all those valid numbers, backtrack again
7. If you backtrack all the way to the first move then the Sudoku is impossible to solve

Point 5 and 6 are the only ones that I am not too sure on how to implement at this point. So I will primarily focus on breaking that down, though I will need to consider all steps thoroughly.

### Decomposition

To **search through a 2D array** I would simply need a nested for loop, one loop to go through the rows, and the other to go through the columns. I want to start from the top left and go row by row as that seems like the most natural way to search through it. 

I would then need to **find the first empty cell** which can be done with a simple if statement within the nested for loop. However, the check that I use will be depedent on how I set up the 2D array. For example I could initially set every cell to have the value -1 and then update a few values to generate the board. The if statement would then check if the cell has a -1 in it. If I did not intialise the array to an initial value anything could be stored in those memory addresses meaning I would only be able to identify an "empty" cell if it was not the numbers 1 through 9.

To **generate a list of available numbers** I need to search through the row and column the empty cell is positioned in and keep a track of all the numbers that have not appeared. I think do this space effectively I will allocate enough memory for the numbers 1 through 9. Then as I search the row and column I will remove the numbers that appear from the list. 

Then I can **enter the first of the remaining numbers** as the next move and then carry on the search for the next empty cell. I would then need to store that list somewhere where I can access it by that specific cell so that if I backtrack I can easily get the next available number for that cell. I think this will require some experimentation once I start coding to figure out how to implement. 

I would then **continue the process** by just allowing the for loops to continue iterating and then repeating the same procedures. 

To check whether **entering a number breaks a rule** I would probably need a separate function that reads through the entire array looking for if numbers are being repeated where they should not be. I would also need to check that each number is unique in a 3x3 grid, although that actually may be given provided the rows and columns are following the rules properly.

If a number does not break the rule then you just continue looping, otherwise I will need a way to **backtrack**. The only way I have implemented backtracking before was by using a stack, so that may be how I implement it here too. Maybe I could have a function that searches backwards from where you start backtracking. This would search for a cell that points to an array with more than 1 value (as that means there was a random choice made) rather than a cell with only 1 value stored there.

If the program keeps going back and forth to the same location and then the array of potential choices diminishes to 1, I would need to **backtrack again**. In the case where you backtrack again, you would then have to replenish all the available choices from the backtracking spot you just exhausted though as now one of those numbers may be valid. *How do I do that??* 

The final thing to check when backtracking is whether there are any more places to backtrack. If not they the Sudoku would be **impossible** as the program will have tried all combinations with no success.

### Preparing to code

At this stage I have some ideas of how to complete this. I know I will definitely be able to start writing some code although I imagine I will start running into problems very quickly. I think I will just try and create a solver for a 3x3 grid to start with since it should be easier to test when something goes wrong. Hopefully my solution will be scalable meaning I can then easily adjust it so that it works on a full 9x9 grid.

I will have around 4 to 6 days to complete this project before I go away for a couple of weeks. So my goal for now is to finish the project before I have to leave. I will try and set a goal for each day to help keep me on track.

#### Day 1

I think the first milestone I want to achieve is to be able to create a Sudoku board. I think it will be easier to allow the user to create their own Sudoku, as otherwise I would have to generate one randomly and ensure it followed all the rules. I think it will be easiest to input numbers using the command line. So, for example, the program would output 'Enter value for cell [0][0]: ' and then the user can put a value in. I think I can then improve that by using a premade file which has a board displayed with dashes and pipe characters, and allow the user to fill in the cells they want. I then want to output the created board to the user, where they can they say if they want to redo it or get it solved.

#### Day 2

On day 2 I want to get information stored in each cell. I need to store the current value of the cell as well as an array of other potential values the cell could take. Maybe I could just store an array at each cell, where the first index of the array is the cell's value. I do not know if you can store a variable and an array at an index, unless you used structures. I think just an array would probably be easier though. However, if I need to change the value of a cell that would mean I would need to rearrange the array which is a costly operation. If I was using structures I could just access the value variable and change that with less hassle *maybe*.

Regardless, by the end of day 2 I want the Sudoku board displayed with appropriate storage in place such that the rest of the algorithm is possible to implement.

#### Day 3

I will now hopefully be ready to start searching the 2D array and finding the first cell without a set value. Once I have that in place I then need to develop the function to find the available numbers for the given cell and then update the cell's data. Once the cell has all the available numbers it can use, I can then set the empty cell to the first available value. I will hopefully be able to do this multiple times to by utilising the for loops.

With this in place the program should actually be able to solve **some** Sudoku boards. For example, the first 3x3 grid I investigated there was no need to backtrack as the problem could always be solved first try.

#### Day 4

On potentially the final day I will need to start allowing the program to cope when entering a number breaks a rule. That means I first need a function that is able to identify if a rule is being broken. The only two rules are that every row and column should contain unique numbers, so I could add a number, look at every row and every column, if any number appears twice that means a rule is being broken. Then I can delete the value I just inserted, and try the next available number from the array assigned to the cell. If I exhaust this list that is when backtracking happens.

If I can simply print to the console that backtracking is required, I think I would be satisfied with that.

#### Day 5

If I have more time I then need to actually implement the backtracking feature, which needs to be scalable (as in multiple backtracks can happen). If I store a structure in each cell. That structure can contain the current value, and an array of available values. So the program gets to a cell with no set value, it then fills in the list of available values, since the original values will also be empty simply copy over the available array to the original array (this copying over would only happen the first time you visit a cell). Then input the first available value and carry on. At the next cell if you have to go through all available values (meaning the available array would be empty - there is no such thing as empty in C though so you would be continuously setting the values to -1 for example), you then backtrack to the last cell that still contains some available values. You would then have to set a new value and then continue the search again, but each subsequent empty cell would need to have its available array remade. I am not sure as to whether multiple backtracks can happen in a 3x3 grid, so that may not be an issue I need to deal with straight away. Although I will want to scale my program up to the 9x9 grid so I will have to consider this regardless.

## Code Development

### Preface

I completed the project in 4 days. In the first day I was able to complete what I estimated would take 3 to 4 days, so I was absolutely flying. I then realised I had forgotten a consideration to do with the validity of the board. After realising that would be a relatively easy fix, that led me onto my more drastic realisation. The realisation was that my method would not work with my current plan, which caused me to have to go back to planning. As I wanted to complete the project before Friday this caused me to rush the planning stage and ultimately led to me taking a peek at Google to gather some ideas faster. Ultimately, I was not able to completely develop the program myself in the given timeframe, however I have realised how powerful planning can be.

~~ Made lots of progress first day, got the board set up with correct data stored and it could solve basic ones
~~ Then I developed backtracking to fix a single issue which could arise in 3x3 board
~~ Now trying to get bigger board, completely forgot that then you have cells where all numbers need to be unique there
~~ Might have to redo anyway because the program isn't able to handle lots of backtracks
~~ So make version 2, want to improve imput, start at 4x4 grid, need to have better backtrack system --> recursion

VERSION 2

Wanted to get file input going first and nice printing
Then do storage for cells again
Make a board checker (checks for duplicates and uniques in sub-cells)
Start with 4x4 grid as that has sub cells and can require 2 backtracks --> get example of this and video. Seems like you can get two errors, but not consecutive errors. Might have to go up to 9x9

Need to make use of recursion. When solving the board I need to give the current state of the board back to the function and see if a move can fill out the grid.
Done


### Generating the board

### Storing data in the cells

### Interacting with the board

### Catching rule breakers

### Backtracking



