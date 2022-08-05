---
title: "Creating an ADVANCED meal planner"
date: "2022-08-03"
slug: "mealplanner"
description: "A project post about a meal planner with extra features"
keywords: ["university", "blog", "new", "student", "meal", "planner", "food"]
draft: false
tags: ["project"]
math: false
toc: true
---

## Motivation

During my time as a first year I would often walk into to the kitchen at dinner time with no real idea of what I was about to eat. This worked out fine, but I wanted to be more organised for next year. I tried creating a meal planner using Excel during the final term of first year, which worked by randomly selecting a meal from 3 potential choices for each day. This was okay but it did not take into account if a meal would provide leftovers, and generally it was not good enough to use. Let's change that by making a more dynamic and intelligent meal planner!

## Prerequisites 

The program is quite simple and only involves basic programming structures such as for loops and functions. So for anyone with some general knowledge about programming this should be very accessible.

## Development

Originally I only intended on recreating the meal planner I had in excel using Python. This worked by randomly allocating a meal to each day of the week and that was all. I realised this had a few issues though, which ultimately led to me making the meal planner smarter. The main issue was that it did not account for leftovers that meals could provide. As a student I do not really have time to be spending an hour or two cooking every day and so it is really helpful if I can rely on eating leftovers for one to two days after cooking a meal. Another issue was that the meal planner was limited to one week and it always started on Monday, so, to improve this I wanted to allow the user to select the starting day and length of their plan.

By implementing these two new features it should hopefully make a meal planner that I will be able to configure to make it more useful to me and hopefully I will actually use it.

The first thing I had to do was store the meals as well as the number of days of leftovers they provided. This made me think of a dictionary as I would be able to store the meal name as the key, and then the number of days of leftovers as the value. I was able to access the meal and leftovers easily, and adding a new meal was easy so this seemed like the right option.

I also made sure to comment the code to practice that skill and ensure that I continued to understand what was happening in my program.

```python
# Dictionary of meals and the number of days they provide leftovers for
dinner_list = {
    "Chicken noodles": 1,
    "Chicken wrap": 2,
    "Tuna pasta mayo": 1,
    "Cottage pie": 2,
}
```

The next thing I wanted to implement was the user input element of the program. This includes allowing the user to choose the starting day as well as how long they want the plan. The only real problem with this section was deciding how to allow the user to choose the day of the week to start on. I did not want the user to have to type in the full day as that would involve lots of input validation to handle all the variations of the words, so I decided to use a numbering system instead. I then realised it was far too difficult to correspond a number to the day of the week so I decided to print out the days to allow the user to easily correspond the number to the day they want.

```python
print("Please enter the starting day: \n")
print("1. Monday \t 2. Tuesday\n")
print("3. Wednesday \t 4. Thursday\n")
print("5. Friday \t 6. Saturday\n")
print("\t 7. Sunday\n")

starting_day = int(input("Enter the starting day: "))
planner_length = int(input("Enter how many days you want to plan for: "))
```

With all the inputs handled I now needed to figure out how to display this at the end of the program. To make the output more readable I wanted the output as follows `[DAY]: [MEAL]`. This meant I needed to allocate a set amount of memory for the days, and then display the correct days. I implemented this using a couple of lists. The first list was a constant which simply stored the names of the days of the week to reference later for the output. The other list was an empty list to store the correct amount of days in it. In Python, to create an empty list of a set size you have to fill the list with `None` and multiply this by how many 'slots' in the list you want.

```python
DAYS_OF_THE_WEEK = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
relevant_days = [None] * planner_length
```

With the two new lists this allowed me to use all the data collected from the user to generate all the relevant days. For example, if the user wanted a plan starting on Thursday for 3 days, the `relevant_days` list would store `['Thursday', 'Friday', 'Saturday']` which I could then use to make the output all nice and correct.

```python
# Fill meal_days with the days of the week starting from the starting day
for i in range(planner_length):
    relevant_days[i] = DAYS_OF_THE_WEEK[(starting_day - 1 + i) % 7]
```

Now that I had my list of relevant days for the plan, I needed to link each day with a meal. As the schedule will likely include multiple meals, it means I needed a function to add a meal such that it could easily be repeated. I created an empty list called `meal_plan` which stored the meals selected. To add a meal I simply had to randomly choose a key from the `dinner_list` dictionary, and then append this to the meal plan list. 

Due to my later decision to remove the meal from the dictionary when handling the leftovers, I created a `try: {} except: {}` block to make my program more robust. However at this stage in development I did not necessarily need this feature, it was simply an improvement.

```python
# Function to choose a random meal from the dinner_list and add it to the meal_plan
def add_meal():
    try:
        meal_choice = random.choice(list(dinner_list.keys()))
        meal_plan.append(meal_choice)
    except:
        meal_plan.append("No meal")
        return
```

With the ability to add meals to the list the program was now capable of generating a meal plan. However, it could still not make use of the leftovers which was one of the main features I wanted to implement in this version. Therefore, the next stage was to create a function to handle the leftovers of the most recently added meal. Again, I used a `try: {} except: {}` to make the program more robust but it is not necessary for the main functionality of the program.

To add the leftovers into the meal plan, I created a for loop which would continue to append the same meal to the `meal_plan` list equal to the number of leftovers the meal provides (as specified in the values of the dictionary). Another feature I implemented later on was to delete the meal from the dictionary such that it could not be randomly selected again.

```python
# Function that handles leftover meals
def handle_leftovers():
    try:
        leftover_meals = dinner_list[meal_plan[-1]] 
    except:
        return
    
    for i in range(leftover_meals):
        meal_plan.append(meal_plan[-1])

    # Remove the meal from the dinner_list
    del dinner_list[meal_plan[-1]]
```

At this stage I could now generate a meal and the subsequent days of the meal plan would be filled with the same meal as long as there were leftovers. But there was still more that needed to be implemented. I needed to fill the meal plan so I needed the previous two functions to be called multiple times. *But how many times should they be called?* This was a fairly simple question to answer as all I needed to do was fill the meal plan list. So as long as the meal plan was not full I would need to continue adding meals.

To implement this I first needed a function to determine whether the list was full or not. As there did not seem to be some in-built functionality to determine if a set-sized list was full, I had to create it myself. I made use of the `planner_length` variable created earlier which stores how many days the user wanted to plan their meals for. If the length of the meal plan list is equal to this that would mean that the meal plan list would be full. Using this logic I could then create the function.

```python
# Function that determines if the meal plan is full
def is_full():
    if len(meal_plan) == planner_length:
        return True
    else:
        return False
```

With all the functionality in place, I just needed to call the functions while the meal plan was not full.

```python
# Driver function
def generate_meal_plan():
    while (not is_full()):
        try:
            add_meal()
            handle_leftovers()
        except:
            return
```

The final stage of this project was outputting the generated meal plan. As stated before I wanted to display the schedule like this `[DAY]: [MEAL]`. This could be achieved with a for loop iterating up to the `planner_length` and printing the day from the `relevant_days` list and the meal from the `meal_plan` list. An improvement I made on this was to specify if the meal was in the form of leftovers by checking if the chosen meal already appeared in the list. As the dictionary entries get delted after being used, this means that if it appears again in the meal plan list then that means it is getting repeated by the `handle_leftovers` function. Then I can simply append a string to specify that the user would be eating leftovers on a specific day.

```python
# Function that prints the days and meals in the meal plan
def print_meal_plan():
    for i in range(planner_length):
        # If the meal has appeared before, append leftover to print string
        if meal_plan[i] in meal_plan[:i]:
            print(relevant_days[i] + ": " + meal_plan[i] + " (leftover)")
        else:
            print(relevant_days[i] + ": " + meal_plan[i])
```

To run the program I then called the couple of relevant functions.

```python
# --- Run the program ---
generate_meal_plan()
print_meal_plan()
```

## The final result

The GitHub repository can be found [here](https://github.com/ryanbradshawcodes/Meal-planner).

## Reflection

This is a program that I will actually use (probably). This was another one of those projects that I actually felt motivated to complete because I was able to engage with it as I thought it would be useful. It was a fairly easy project overall, but it did take me a couple of iterations to get it working how I wanted. On top of that, I had to do a fair amount of planning (more that I would usually do anyway) to reach the solution I liked, so that helped me build those skills.

There are some additional features I would like to implement (commented at the top of the Python file) so I will hopefully be able to expand upon this project too. Stay tuned!