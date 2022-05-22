# Reinforcement Learning

## Chapter 1 Introduction
- we learn by interacting with our environment
- In this book we explore a ***computational*** approach to learning from interaction.
-  ***reinforcement learning*** is much more focused on **goal-directed learning from interaction** than are other approaches to machine learning.

### 1.1 Reinforcement Learning
- Reinforcement learning is learning what to do—how to map situations to actions—so as to maximize a numerical reward signal.
  - 2 characteristics
    - trial-and-error search
    - delayed reward
- We formalize the problem of reinforcement learning using ideas from dynamical systems theory, specifically, as the optimal control of incompletely-known Markov decision process (MDP)
- A learning agent must be able to 
  - **sense the state** of its environment to some extent
  - take actions that **affect the state**
- The agent also must **have a goal or goals** relating to the state of the environment
- Reinforcement learning is different
- One of the **challenges** that arise in RL, and not in other kinds of learning, is **the trade-off between exploration and exploitation**
  - The dilemma is that neither exploration nor exploitation can be pursued **exclusively without failing at the task**.
- Another key feature is that it explicitly considers the whole problem of **a goal-directed agent interacting with an uncertain environment**
  - starting with a complete, interactive, goal-seeking agent

### 1.2 Examples
- All involve ***interaction*** between an **active decision-making agent** and its **environment**, within which the agent seeks to achieve **a goal** despite **uncertainty** about its environment
  - At the same time, **the effects of actions** cannot be fully predicted;
  - thus the agent must **monitor** its environment frequently and react appropriately
- In all of these examples the agent can use its experience to **improve** its performance over time

### 1.3 Elements of Reinforcement Learning
- Beyond the ***agent*** and the ***environment***, one can identify four main **subelements** of a ***reinforcement learning system***: 
  - a ***policy***
    - defines the learning agent’s **way of behaving** at a **given time**
    - $\{states\} \rightarrow \{actions\}$
    - policies may be **stochastic**, specifying probabilities for each action.
  - a ***reward signal*** defines the goal of a reinforcement learning problem
    - **On each time step**, the environment sends to the reinforcement learning agent a single number called the reward
    - The agent’s sole objective is to **maximize the total reward** it receives over the long run.
  - a ***value function*** specifies what is good in the long run
    -   The value of a state is the total amount of reward an agent **can expect** to accumulate over the future, starting from that state
  -  optionally, a ***model of the environment***
     -  something that mimics the behavior of the environment
     -  model-based methods
     -  model-free methods

- **Rewards** are in a sense **primary**, whereas **values**, as predictions of rewards, are **secondary**
  - Nevertheless, it is values with which we are most concerned when making and evaluating decisions
    - Action choices are made based on value judgments
  - In fact, the **most important component** of almost all reinforcement learning algorithms we consider is a method for **efficiently estimating values**

### 1.4 Limitations and Scope
- Reinforcement learning relies heavily on the concept of **state**
  - **Informally**, we can think of the state as a signal conveying to the agent some sense of **“how the environment is”** at a particular time
  -  The formal definition of state as we use it here is given by the framework of **Markov decision processes**
     -  encourage the reader to follow the informal meaning
- it is **not strictly necessary** to estimate value functions to solve reinforcement learning problems
  - evolutionary methods
    - have advantages on problems in which the learning agent **cannot sense the complete state** of its environment.
- Our focus is on reinforcement learning methods that **learn while interacting** with the environment, which evolutionary methods do not do.

### 1.5 An Extended Example: Tic-Tac-Toe
-  the classical **“minimax”** solution from game theory is not correct here because **it assumes a particular way of playing by the opponent**
-  Let us assume that opponents' information is not available a priori for this problem, as it is not for the vast majority of problems of practical interest
-  How the tic-tac-toe problem would be approached with a method making use of a value function.
   1.  set up a **table** of numbers, one for each possible state of the game
       -  Each number will be the **latest estimate** of the **probability** of our winning from that state -- state's ***value***
       -  State A has higher value than state B, or is considered “better”than state B, if the current estimate of **the probability of our winning** from A is higher than it is from B
          -  three Xs in a row the probability of winning is 1
          -  three Os in a row the probability of winning is 0
          -  all the other states to 0.5
   2.  We then play many games against the opponent.
       -   Most of the time we move greedily
       -   Occasionally, however, we select randomly from among the other moves instead
   3.  While we are playing, we change the values of the states in which we find ourselves during the game
       -  attempt to make them **more accurate estimates** of the probabilities of winning 
       -  we **“back up”** the value of the state after each greedy move to the state before the move
       -  let $S_t$ denote the state **before** the greedy move
       -  $S_{t+1}$ denote the state **after** the move,
       -  the **update** to the estimated value of $S_t$, denoted $V(S_t)$,
       -  $$V(S_t)\leftarrow V(S_t) + \alpha[V(S_{t+1})-V(S_t)]$$
          -  $α$ is a small positive fraction called the step-size parameter
             -  learning rate
       - This update rule is an example of a ***temporal-difference learning method***
         - because its changes are based on a **difference**, $V(S_{t+1})-V(S_t)$
- the differences between **evolutionary methods** and **methods that learn value functions**.
  - evolutionary method holds the policy **fixed** and plays many games against the opponent, or **simulates** many games using a model of the opponent
    - each **policy change** is made only **after many games**, and only the **final outcome** of each game is used
      - what happens during the games is ***ignored***
  - Value function methods, in contrast, allow individual states to be evaluated
    - the emphasis on **learning while interacting** with an environment
    - there is a clear goal, and correct behavior requires **planning** or **foresight** that takes into account delayed effects of one’s choices
      - 照顾了长期收益 （比如设置陷阱）