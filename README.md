# Reinforcement-Learning-Taxi-V3

## Problem Description:
There are four designated locations in the grid world indicated by R(ed), B(lue), G(reen), and Y(ellow). When the episode starts, the taxi starts off at a random square and the passenger is at a random location. The taxi drive to the passenger's location, pick up the passenger, drive to the passenger's destination (another one of the four specified locations), and then drop off the passenger. Once the passenger is dropped off, the episode ends.

### States
There are 500 discrete states since there are 25 taxi positions. The state is described by the location on the grid (a row and a column number between 0 and 4), a location to drop-off the passenger from four choices, and the passenger which can be in one of the four locations or inside the taxi.

### Actions
The taxi can move to the four cardinal directions : South, North, East or West. There are also two other possible actions which are the pick-up or the drop-off the passenger. For the mentioned six actions we have:

* South = 0
* North = 1
* East = 2
* West = 3
* Pick up = 4
* Drop off = 5

Note here that you are not penalized more if you try to move to an unauthorized location. For example, in the below figure, you cannot move to the west, but you still get the -1 penalty, no more no less. The same goes when you try to move farther than the 5x5 grid. However, you are severely penalized if you try to pick up or drop off the passenger at the wrong location.

![image](https://user-images.githubusercontent.com/125180530/218301217-cfc4076b-685b-48f7-a6aa-e7ad80e70ac2.png)

### Rendering
The first step is to render a random environment.

```
env = gym.make("Taxi-v3").env
env.reset()
print(env.render(mode='ansi'))
```

![image](https://user-images.githubusercontent.com/125180530/218301338-2ffbdb62-9aad-436b-ad4f-a45fbbdddc16.png)

We first implement the problem without Q-learning and in a random process. Then, for improving the model we use Q-learning. 

### Q-learning Algorithm
The agent needs to explore its environment and decides which action to take to maximize its expected reward. So, there is a trade-off between exploring (the environment) and exploiting (what the agent learnt).

With the use of a great memory, a memory large enough to remember a big number of the last moves, the agent can better decide which action to take to reach its goal sooner and more efficiently. That's why we need to use a table to simulate the mentioned memory and use its values to train our model.

The Q-table is a table where you find a Q-value for each couple (state, action). For each state, we can have several possibleactions, so several Q-values. The goal is to learn to choose the higher Q-value for each state which means choose the action with the best chance to get the maximum reward.

![image](https://user-images.githubusercontent.com/125180530/218301851-f64e8282-44f1-48ef-ad9b-cbddd60f7a0e.png)

To start learning, the Q-table is generally randomly initialized (or initialized to 0). The agent then explores its environment by randomly choosing an action and get a reward for its action. The idea is to let the agent exploring the environment and getting some rewards. But after a while, we need to exploit the information gathered. From a given state, the agent must compute the expected reward for each action it can take and choose the one with the maximum expected reward:

![image](https://user-images.githubusercontent.com/125180530/218301884-4af94c8d-43c0-498c-9684-fe6c86ebce33.png)

The updating process of the Q-values in the Q-table is done by the Bellman equation:

![image](https://user-images.githubusercontent.com/125180530/218301965-eb6e06f3-bc62-4e82-95a6-10e6ef6ea256.png)

* α is the learning rate

* γ is a discount factor to give more or less importance to the next reward

