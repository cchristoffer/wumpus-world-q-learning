# wumpus-world-q-learning
Solving a simple wumpus game with Q-learning.

## The game
The wumpusMaze class will generate a 2D array with items 'A', 'W', 'P' and 'G', theese stand for Agent, Wumpus, Pit and gold. Surrounding the game are 'X's which are walls or unpassable terrain. After generation this class will with random walk check if the maze is solvable within 10.000 steps. If not, a new Wumpus World will be generated.

![Image of solvable world](https://i.imgur.com/j6purnm.png)
## The rules of the game
Pits and Wumpus will kill the Agent if it moves over one. Tiles surrounding Pits will set a state of Breeze while tiles surrounding Wumpus will have Stench. The Agent has a bow and arrow which can shoot in four directions and costs 10 score to shoot, if the agent shoots in the correct direction from an adjacent tile and hits the wumpus. It will emit a terrible scream and award the player 1000 points. Each agent move will also cost 1 point.
### End game!
The aim of the game is to find the gold without falling into a pit, getting eaten by wumpus in as few steps as possible. If the agent is able to find gold without dying, the world is solvable.

## The Agent
For this agent to learn using reinforcement learning we use Q-learning, Q meaning quality. Simply put the agent will reinforce actions that are considered "good" quality. By also normalizing probabilites of other actions will decrease.

## The policy
We used a 3-dimensional numpy array with width, height and actions (8) of ones the then normalize it so that each potential action for each tile will start at 0.125. This means that at starting position [1][1] there is a 12.5% for each action to occur (shoot and walk x4 directions).

## The doing
Based on the position of the Agent, we roll the dice! From start the agent will do random things and probably die.. Alot! 

## The learning
In this example we use 5000 epochs, meaning that we let the agent run amok until it dies or kills the wumpus and gets the gold for 5000 times. After each epoch add all it's actions to a record and check if the agent achieved it's goal by looking at the score, if it scored above 1000 it must have both killed wumpus with it's bow and found the gold. If it's traversal was a success, we loop trough the records and add the learningRate (0.1) to each of it's steps. By the normalizing the Q-table all steps not taken will automaticly decrease. If the agent fails, we decrease each taken action in the record by the learning rate as long as it doesn't not amount to less than 0.1. By normalizing after a failed traversal all actions not taken will also increase.

## Exploration
What if the agent finds a path to the gold early, but the road is long and inefficient? We solve this by adding an explorationRate! For each of the action there is a probability of the exploration rate that the Agent takes a completely random action. We want the agent to explore more in the beginning whist it has not found an optimal path, and less the more it learns. We solve this by  dividing our explorationRate with (epoch+1.0), meaning that the probabillity of explaration will decrease for each epoch of traversal.

## Testing our Agent!
![Image of Agent winning](https://i.imgur.com/M1hoUZT.png)

## To kill or not to kill?
Training the agent to both kill the wumpus and get the gold for 5000 epochs leave the agent with a 99.9999% chance of correct action for all the tile except one. Should the agent kill the wumpus  first or go for the gold? At the third move in our test run, both shooting down at the wumpus and moving up towards the gold are both correct. Beacuse in the end, both of theese actions must be taken in order to win. This leaves us with a 57% chance to both kill the wumpus and get the gold or a 43% chance to simply take the gold and leave wumpus in peace.

## Summary 
The Agent succeds in his goals, in the future i will however try a deep Q-learning algorithm to be dependant on the boolean states of the game rather than the tiles. Doing so would enable our Agent to solve any wumpus game we throw at it, not only the one specifically trained on.
