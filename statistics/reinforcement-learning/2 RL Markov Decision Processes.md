#course_google-deepmind-reinforcement-learning #reinforcement-learning 
## Markov processes

- *Markov decision processes* (MDPs) formally describe an environment for machine learning
    - We'll start with the nice case where the environment is *fully observable*. The agent gets all the information from the environment.
    - Almost all RL problems can be formalised as MDPs
        - Optimal control deals with continuous MDPs
        - Any partially observable problems can be converted into MDPs
        - Bandits (common formalism, you get a set of actions, you perform one action, get rewarded and the round ends) are MDPs with one state

As discussed in the [[1 RL Introduction to Reinforcement Learning#^c3f6cb|previous lecture]]:

> [!info] Definition - Recap: Markov State
> A state $S_t$ is *Markov* iff:
> $$\mathbb{P}[S_{t+1}|S_t]=\mathbb{P}[S_{t+1}|S_1, \dots, S_t]$$
> In other words, the probability of the next state given the state you're in is the same as if you showed all of the previous states to the system.
> 
> i.e. The current state is a sufficient statistic of the future
### State transition matrix

For a Markov state $s$ and a successor state $s'$, the *state transition probability* is defined by $\mathscr{P}_{ss'}=\mathbb{P}\left[S_{t+1}=s'|S_t=s\right]$.
- Recall that $S_t$ characterises everything about what happens next (Markov by definition), that should mean that there is some well-defined transition probability that the agent will transition to another state.

From this, we can produce a matrix which contains all transition probabilities from one state (rows) to another (columns). Note that each row of the matrix sums to 1.
$$
\mathscr{P} = \begin{bmatrix}
\mathscr{P}_{11} & \dots & \mathscr{P}_{1n} \\
\vdots & \ddots & \vdots \\
\mathscr{P}_{n1} & \dots & \mathscr{P}_{nn}
\end{bmatrix}
$$
We can now keep sampling from this matrix to move through states.
### Markov chains

- Formally, a Markov process is a memoryless random process. i.e. A sequence of random states $S_1, S_2, \dots$ with the Markov property
- All that's needed to define this is a state space $\mathscr{S}$ and the set of transition properties $\mathscr{P}$. This fully defines the dynamics of the whole system.

> [!info] Definition: Markov process
> A Markov process or a Markov chain is a tuple ($\mathscr{S}$, $\mathscr{P}$)
> - $\mathscr{S}$ is a finite set of states
> - $\mathscr{P}$ is a state transition probability matrix $\mathscr{P}_{ss'}=\mathbb{P}\left[S_{t+1}=s'|S_t=s\right]$

> [!tip] Example: Student behaviour
> ![[Pasted image 20230818154209.png]]
> We can take *samples* from this Markov chain which just means following an agent through a sequence of states until they hit a *terminal* state (one with no exit probabilities).
> 
> ![[Pasted image 20230818154408.png]]
> See above the state transition matrix for this particular Markov process.

## Markov reward processes

- A *Markov reward process* is a Markov process with value judgements, i.e. we add two things to our Markov process: a *reward function* and a *discount factor*.

> [!info] Definition: Markov reward process
> A Markov process or a Markov chain is a tuple ($\mathscr{S}$, $\mathscr{P}$, ==$\mathscr{R}$==, ==$\gamma$==)
> - $\mathscr{S}$ is a finite set of states
> - $\mathscr{P}$ is a state transition probability matrix $\mathscr{P}_{ss'}=\mathbb{P}\left[S_{t+1}=s'|S_t=s\right]$
> - ==$\mathscr{R}$ is a reward function $\mathscr{R}_s=\mathbb{E}[R_{t+1}|S_t=s]$==. i.e. If we're at time $t$ and state $s$, then our immediate reward (that we get at the next time step) is $\mathscr{R}_s$.
> - ==$\gamma$ is a discount factor, $\gamma \in [0, 1]$==

> [!tip] Example: Student MRP
> ![[Pasted image 20230818160241.png]]
> Say you get negative rewards from Facebook and going to class. What we really care about is the total reward we get across the whole sample.

### Return

- The *return* is what we want to maximise.
    - Most Markov reward and decision processes are discounted. 
        - Mathematically convenient to do so and avoids infinite returns in cyclic Markov processes
        - There is more uncertainty in the future due to the fact that we don't have a perfect model of the environment
        - Natural in financial settings since [money earned now will earn more interest than money earned later](https://www.investopedia.com/articles/03/082703.asp)
        - There is a natural preference for immediate rewards over future rewards in human/animal behaviour
        - It is possible to use un-discounted Markov processes (i.e. $\gamma=1$) if we know that all sequences end in a terminal node

> [!info] Definition: Return
> The *return* $G_t$ is the total *discounted* return from time-step $t$ 
> $$G_t=R_{t+1}+\gamma R_{t+2}+\dots=\sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$
> $\gamma \in [0, 1]$, our discount factor, tells us how much you care now about rewards you get in the future. In other words, the value of receiving $R$ in $k+1$ time-steps is $\gamma^kR$
> - $\gamma = 0$ means you care only about immediate returns
> - $\gamma = 1$ means you care about future returns equally as much as present returns

## Value function

- This is the central quantity we're interested in and is the total reward that you expect to get from a given state onwards.
    - In other words, $v(s)$ gives the long-term value of state $s$

> [!info] Definition: State value function
> The *state value function* $v(s)$ of an MRP is the expected return starting from state $s$
> $$v(s)=\mathbb{E}[G_t|S_t=s]$$
> This has to be an expectation because the environment we're in (a Markov process) is stochastic.

> [!tip] Example: Student Markov Chain returns
> ![[Pasted image 20230819230607.png]]
> This is the return starting at time step 1. How would we find the value (i.e. the expected return from time step 1 onwards)? We could take a bunch of samples starting in the first state and average out all possible returns.
> 
> Now consider the case where $\gamma=0$; i.e. we are maximally short-sighted and don't care about future returns. Here's what the state-value function might look like for our example. The value function at each state is equal to the reward at each state since we only care what we get in the immediate time step.
> 
> ![[Pasted image 20230819233220.png]]
> 
> The scenario where $\gamma=0.9$ leads to different values at each state since we're now accounting for returns at other states as well as their transition probabilities.
> 
> ![[Pasted image 20230819233526.png]]

## Bellman equation for MRPs

This is a fundamental relationship in RL

- The idea is that the value function obeys a recursive decomposition. You can take a sequence of rewards (the value function) and break it up into two parts:
    - The immediate reward ($R_{t+1}$)
    - Discounted value of the value function from the next state onwards ($\gamma v(S_{t+1})$)

$$
\begin{align*}
v(s) & = \mathbb{E} [G_t | St = s] \\
& = \mathbb{E} \left[ R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \ldots \mid S_t = s \right] \\
& = \mathbb{E} \left[ R_{t+1} + \gamma (R_{t+2} + \gamma R_{t+3} + \ldots) \mid S_t = s \right] \\
& = \mathbb{E} \left[ R_{t+1} + \gamma G_{t+1} \mid S_t = s \right] \\
& = \mathbb{E} \left[ R_{t+1} + \gamma v(S_{t+1}) \mid S_t = s \right]
\end{align*}
$$

> [!tip]
> The lecturer explains here that the use of the $t+1$ index to indicate immediate reward (i.e. $R_{t+1}$) comes from the idea that the agent's action must be seen by the environment before the environment updates its state and provides a reward to the agent. So you get the reward for your current action in the next time step. He also notes that this might be slightly inconsistent in the slides since there are different conventions surrounding this.

### Lookahead search formulation

- We can also think of this as a one-step lookahead search, where from our current state we look ahead to find the value function/s at the next possible state/s (weighted by the probability of transitioning to those states) and add the immediate reward we get along the way. 

![[Pasted image 20230821162857.png]]

- Using this concept, we can write the value function in the following format: 
    - The current return ($\mathscr{R}$) plus the weighted (using the state transition matrix $\mathscr{P}_{ss'}$) and discounted ($\gamma$) sum of the value functions at all possible next states ($v(s')$).

$$
\begin{align*}
v(s) & = \mathbb{E} [G_t | St = s] \\
& = \mathscr{R}_s + \gamma \sum_{s' \in S} \mathscr{P}_{ss'}v(s')
\end{align*}
$$

> [!tip] Example: Bellman Equation for Student MRP
> ![[Pasted image 20230821163831.png]]
> This example has a discount factor ($\gamma$) of 1. The value function in the red state is a sum of the current return ($-2$) and a weighted sum of the value functions at the two next possible states you can move to.

### Matrix formulation

- We can also express the Bellman equation concisely with matrices. See below, where $v$ is a column vector with one entry per state:
$$v = \mathscr{R}+\gamma \mathscr{P}v$$
$$
\begin{bmatrix}
v(1) \\
\vdots \\
v(n)
\end{bmatrix}
=
\begin{bmatrix}
\mathscr{R}_1 \\
\vdots \\
\mathscr{R}_n
\end{bmatrix}
+ \gamma
\begin{bmatrix}
\mathscr{P}_{11} & \dots & \mathscr{P}_{1n} \\
\vdots & \ddots & \vdots \\
\mathscr{P}_{n1} & \dots & \mathscr{P}_{nn}
\end{bmatrix}
\begin{bmatrix}
v(1) \\
\vdots \\
v(n)
\end{bmatrix}
$$
- Since this is a linear equation, it can be solved for smaller systems. However, the computational complexity of the solution (i.e. inverting $(I - \gamma \mathscr{P})$) is $O(n^3)$ for $n$ states, so for larger systems we'll talk about more efficient methods of solving the value function for each state:
    - [[3 RL Planning by Dynamic Programming|Dynamic programming]]
    - Monte-Carlo evaluation
    - Temporal-Difference learning

$$
\begin{align*}
v &= \mathscr{R} + \gamma \mathscr{P} v \\
(I - \gamma \mathscr{P}) v &= \mathscr{R} \\
v &= (I - \gamma \mathscr{P})^{-1} \mathscr{R}
\end{align*}
$$
