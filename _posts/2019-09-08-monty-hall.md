---
title: Solving the Monty Hall Problem
layout: post-page
---

Imagine you're on a game show. There are 3 doors, behind which are 2 goats and 1 car. To win, you must choose the door behind which the car can be found. 

You select a door at random and the gameshow host removes one door with a goat behind it. The host gives you the option to stick with the door you chose or switch to the other door. Do you switch? Does it make a difference to your chances of winning the car if you do?

## Surely it's just 50:50?

This seems like a reasonable answer, given that once the host has opened one of the doors with a goat behind, only one goat and one car are left to pick. But let's go through why this isn't quite right.

## Sticking with the door you chose

Let's say the car is behind door 1, with the 2 goats behind the other doors, and draw out all possibilities:

**Possibility 1**: Select door 1 (with the car behind). The host opens a door with one of the goats behind (either door 2 or door 3). You stick with door 1 (with the car behind). You win! 🏎️

**Possibility 2**: Select door 2 (with a goat behind). The host opens the other door with the goat behind (door 3). You stick with door 2 (with the goat behind). You lose! 🐐

**Possibility 3**: Select door 3 (with a goat behind). The host opens the other door with the goat behind (door 2). You stick with door 3 (with the goat behind). You lose! 🐐

We can see that if you stick with the door you chose, you lose in 2 of the scenarios out of a possible 3. This means your chances of winning are 1/3 (33%) and your chances of losing are 2/3 (66%). Try this when the car is behind door 2 or door 3 and you'll get the same results - you lose twice and win once when you stick! 

## Switching to the other door

Let's say the car is behind door 1 and draw out all possibilities:

**Possibility 1**: Select door 1 (with the car behind). The host opens a door with one of the goats behind (either door 2 or door 3). You switch from door 1 (with the car behind) to the other door (door 1 with a goat behind). You lose! 🐐

**Possibility 2**: Select door 2 (with a goat behind). The host opens the other door with the goat behind (door 3). You switch from door 2 to door 1 (with the car behind). You win! 🏎️

**Possibility 3**: Select door 3 (with a goat behind). The host opens the other door with the goat behind. You switch from door 3 to door 1 (with the car behind). You win! 🏎️

So if you select the door with the car behind and you switch to the other door, you lose one out of three times. But if you select a door with a goat behind and switch, you win two out of three times! 

If you select a door with a goat behind, the host has to remove the other goat (because the car can't be removed). So you switch from the goat to the car. As there are 2 doors with goats to select initially, this means selecting 1 of 2 doors with goats behind out of a total of 3 doors will lead to a winning position if you switch. Therefore if you switch, your chances of winning the car are 2/3 (66%) and 1/3 (33%) for selecting a goat. So switching increases your chances of selecting the car!

## Python Implementation
I'll be using Python to code my solution. To begin with, let's import our dependencies:

```python
import random
import matplotlib.pyplot as plt
import numpy as np
```

Here is the main function to run Monty Hall simulations:

```python
# Run a monty hall simulation 'games' times, with an option to switch doors
def monty_hall(games=100000, switching=True):
    wins = 0
    for i in range(games):
        doors = [0] * 3
        chosen_door_ind = random.randint(0,2)
        car_ind = random.randint(0,2)
        doors[car_ind] = 1
        
        if switching:
            del doors[chosen_door_ind]
            doors.remove(0)
            wins += doors[0] == 1
        else:
            wins += doors[chosen_door_ind] == 1
 
    perc_won = (wins/games) * 100
    perc_lost = ((games-wins)/games) * 100
    print("-"*75)
    print("Played {:,} games and switching = {}".format(games, switching))
    print("Won {:,} games ({}%), lost {:,} games ({}%)".format(
        wins, perc_won, games-wins, perc_lost))
    print("-"*75)
    return wins, games-wins
```

Let's break this down. This function will allow us to run a specified number of games, with a boolean parameter 'switching' which when set to True will result in switching to the other door in each game. This will allow us to perform what is also known as a Monte Carlo simulation.

On line 4 we begin our loop, iterating 'games' times. Line 5 initialises an empty list of 3 zeros, with each zero representing a door. On lines 6 and 7 we generate 2 random numbers, the first being the index of the door that is chosen by the player for this game, and the second being the index of the door with the car behind. Line 8 places a 1 in the doors list so we know where the car is. If the player is switching on each game, the player's chosen door is removed from the doors list (line 11) and the door with the goat behind (i.e. the other goat) is also removed (line 12). On line 13 we increase our win count by 1 if the remaining door has a value of 1 (i.e. a car is behind this door), otherwise, we add 0 indicating we lost this game.

Lines 17 and 18 calculate some statistics based on the number of games we won and lost respectively, and some output is presented on the screen. Finally, we return the total number of wins and losses calculated from the total number of games we initially specified.

We can now visualise these results as a bar chart with the following function:

```python
# Plot the results of 'games' number of monty hall simulations
def plot_results(wins, losses, games, switching):
    x = ['Wins', 'Losses']
    g = [wins, losses]
    x_pos = np.arange(len(x))
    plt.bar(x_pos, g, color='#4579cb',align='center')
    plt.ylabel('Number of Games')
    plt.ylim(0, games)
    plt.title(
        'Monty Hall ({:,} games, switching = {})'.format(games, switching))
    plt.xticks(x_pos, x)
    plt.show()
```

To run a specified number of Monty Hall simulations, we use the following code:

```python
games = 100_000
switching = True
wins, losses = monty_hall(games, switching)
plot_results(wins, losses, games, switching)
```

This generates the following plot:

<figure>
 <img src="/assets/images/posts/2019-09-08-monty-hall/switching.png" alt="Results after 100,000 simulations" />
 <figcaption>Results after 100,000 simulations</figcaption>
</figure>

This visualisation helps us confirm our hypothesis that with a large number of games, switching on each game on average results in a higher winning percentage than sticking with your original door!

Quite interesting don't you think? If you want to see more, check out Giles McMullen's YouTube video in the references below as it was the original inspiration for this post.

All code for this post is available on a GitHub repository [here](https://github.com/harryb0905/monty-hall-problem){:target="_blank"}.

## References

1. [Monty Hall Wikipedia page](https://en.wikipedia.org/wiki/Monty_Hall_problem){:target="_blank"}
2. [Monty Hall problem post](https://betterexplained.com/articles/understanding-the-monty-hall-problem/){:target="_blank"}
3. [Giles McMullen's YouTube video](https://www.youtube.com/watch?v=OKp3bYiKGrc&ab_channel=PythonProgrammer){:target="_blank"}