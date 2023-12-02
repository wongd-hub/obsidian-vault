#course_google-deepmind-reinforcement-learning #reinforcement-learning 

[Slide link](https://www.davidsilver.uk/wp-content/uploads/2020/03/MDP.pdf)
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

### Value function

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

### Bellman expectation equation for MRPs

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
- Since this is a linear equation, it can be solved for smaller systems. However, the solution is computationally complex (inverting $(I - \gamma \mathscr{P})$ is $O(n^3)$ for $n$ states), so for larger systems we'll talk about more efficient methods of solving the value function for each state:
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
- This is not true of Markov Decision Processes; so this is useful for when we want to evaluate rewards, but not for maximising those rewards.

## Markov decision processes

- Markov Decision Processes are just Markov Reward Processes with actions.
    - In the previous examples, there's no agency - you couldn't take actions or make decisions.
    - We add one more component to our Markov Reward Process, the set of actions $\mathscr{A}$.

> [!info] Definition: Markov decision process
> A *Markov Decision Process* is a tuple $\langle \mathscr{S}, \mathscr{A}, \mathscr{P}, \mathscr{R}, \gamma \rangle$, where:
> - $\mathscr{S}$ is a finite set of states
> - ==$\mathscr{A}$ is a finite set of actions==
> - $\mathscr{P}$ is a state transition probability matrix, $\mathscr{P}^a_{ss'}=\mathbb{P}\left[S_{t+1}=s'|S_t=s, A_t=a\right]$ where $a$ represents the action. Note that this matrix (i.e. where you end up) now depends on the action you take.
>     - Think of this as one separate set of transition probabilities for each action that you might take.
> - $\mathscr{R}$ is a reward function, $\mathscr{R}^a_s=\mathbb{E}\left[R_{t+1}|S_t=s, A_t=a\right]$
>     - This can also depend on the action
> - $\gamma$ is the discount factor $\gamma \in [0, 1]$

> [!tip] Example: Student MDP
> We've refactored the student example such that the focus is now on the action/decision you take. There are no longer probabilities attached to each action that moves you to each state since you are now choosing what action to take and hence which state you'll end up in.
> - Note if you choose to go to the pub, this is the only place where randomness is built in; once you're there there are probabilities as to whether you'll transition to a set of other states.
> 
> ![[Pasted image 20230901134420.png]]
> 
> The goal now is to find the best path through the decision making process that maximises the sum of rewards that you end up with.

### Policies

- This is how we formalise what it means to make decisions. A *policy* fully defines the behaviour of an agent.

> [!info] Definition: Policies
> A *policy* $\pi$ is a distribution over actions given states,
> $$\pi(a|s) = P [A_t = a | S_t = s]$$
> - This distribution gives you the mapping between the state $s$ and the probability of taking each available action.
> - This is something under the agent's control, and is useful to keep stochastic since that helps with things like exploration
> - Note there is no need to mention rewards here because in an MDP, a state $s$ fully characterises all future rewards.

- In an MDP, the policies depend just on the current state (the *Markov property*). As a result, we consider *stationary* policies here - i.e. the policy doesn't change depending on the time step, they only depend on the state that we're in.
    - i.e. $A_t \sim \pi(\cdot | S_t), \forall t > 0$ - your action at time $t$ is sampled from your distribution given your state at time $t$, where the distribution is the same given the state for all time > 0. This is as opposed to if the policy changed with time (not the case in what we're discussing here - $A_t \sim \pi_t(\cdot | S_t)$)

> [!tip] Sidebar on the relationship between MRPs and MDPs
> On the relationship between MRPs and MDPs - we can always recover a Markov Reward Process from a Markov Decision Process.
> - Given an MDP $\mathscr{M} = \langle \mathscr{S}, \mathscr{A}, \mathscr{P}, \mathscr{R}, \gamma \rangle$ and a fixed policy $\pi$
> - The state sequence $S_1, S_2, \dots$ is a Markov chain (recall this is characterised by a set of states $\mathscr{S}$ and a state transition matrix $\mathscr{P}$) $\langle \mathscr{S}, \mathscr{P}^\pi \rangle$. 
>      - i.e. If we have some policy that helps us pick actions, we're going to draw a sequence of states; that sequence of states is a Markov chain.
>  - The state and reward sequence $S_1, R_1, S_2, \dots$ is a Markov Reward Process $\langle \mathscr{S}, \mathscr{P}^\pi, \mathscr{R}^\pi, \gamma \rangle$
>  - $\mathscr{P}^\pi$ and $\mathscr{R}^\pi$ are the transition dynamics and the rewards averaged across the policy:
> $$\mathscr{P}^{\pi}_{s, s'} = \sum_{a \in \mathscr{A}} \pi(a|s) \mathscr{P}^{a}_{s s'}$$
> $$\mathscr{R}^{\pi}_{s} = \sum_{a \in \mathscr{A}} \pi(a|s) \mathscr{R}^{a}_{s}$$

### Value function

- We already had a value function for an MRP but there was no agency or decisions. Now we have a policy, there is some way we can choose to behave throughout the process.
    - This means there is not one expectation (i.e. value function) anymore, there are different expectations depending on how we behave.

> [!info] Definition: State-value function
> The *state-value function* $v_\pi(s)$ of an MDP is the expected return starting from state $s$, and then following policy $\pi$. i.e. How good is it to be in state $s$ if I'm following $\pi$ - how much reward can I get form this state onwards?
> $$v_\pi(s) = \mathbb{E}_\pi [G_t | S_t = s]$$
> - $\mathbb{E}_\pi$ means the expectation when we sample all actions according to this policy

> [!info] Definition: Action-value function
> The *action-value function* $q_\pi(s, a)$ is the expected return starting from state $s$, taking action $a$, and then following policy $\pi$. i.e. How good is it to take a particular action from a particular state? 
> - For a given policy, if I'm in state $s$ and I take action $a$, what is the expected return/total reward I'll get from this point onwards?
> - This is what we intuitively care about when we're trying to evaluate what the best decision for the next action is. This is the key quantity we'll be using to optimise our MDP.
>   
>   $$q_\pi(s, a) = \mathbb{E}_\pi [G_t | S_t = s, A_t = a]$$

- Overall, the state-value function tells us how good it is to be in a particular state, and the action-value function tells us how good taking certain actions from that given state will be.

> [!tip] Example: State-Value function for Student MDP
> ![[Pasted image 20230901143739.png]]
> We're fixing the policy so that whenever we have a choice, we'll choose with 50% probability between the choices. Here there is a low value for starting in the -1.3 state since there's a high probability you'll end up in the Facebook state.

### Bellman expectation equation for MDPs

- As with the MRP, the state-value and action-value functions can be decomposed into immediate reward plus discounted value of successor state:
    - State-value: $v_\pi(s) = \mathbb{E}_\pi [R_{t+1} + \gamma v_\pi(S_{t+1}) | S_t = s]$
    - Action-value: $q_\pi(s, a) = \mathbb{E}_\pi [R_{t+1} + \gamma q_\pi(S_{t+1}, A_{t+1}) | S_t = s, A_t = a]$

- Recall that the idea underlying these equations is that the value function at the current time step is equal to the immediate reward *plus* the value function of where you end up.
#### Bellman for state-value

- Looking at this from a look-ahead search perspective:
    - If you're in a state value function $v_\pi$ at the white dot; what it's saying is that it'll average over the two actions we might take (the black dots). The probability we'll go left/right is defined by our policy.
    - For each of the actions we might take there is an action-value function ($q_\pi$) telling us how good it is to take that action from the white dot state.
        - So we can calculate the value of being in our white dot state by looking ahead, getting the action-values from each of the actions we might take, then averaging them out weighted by the probability we'll go there based on our policy $\pi$.

![[Pasted image 20230901145529.png]]

i.e.
$$v_\pi(s) = \sum_{a \in \mathscr{A}} \pi(a|s) q_\pi(s, a)$$
#### Bellman for action-value

- Say we start off taking some action - the root of this tree is a state, and we're considering one specific action from that state.
    - Consider that after we take this action and move to the relevant state, we could go in any direction from there. The action-value function is the weighted average of the return of all possible actions after you take the action you're considering.

![[Pasted image 20230901154931.png]]

$$q_\pi(s, a) = R^a_s + \gamma \sum_{s' \in S} P^a_{ss'} v_\pi(s')$$
#### Bringing it together for state-value functions

- Stitching together the two figures from the last two slides gives us a way of understanding $v$ in terms of itself. This is how we end up solving Markov Decision Processes. 
    - How we understand how good it is to be in a state is to do a two-step lookahead - performing all actions that are possible and ending up in all possible successor states. Those successor states will have state-value functions as well.

- The Bellman equation averages across all possible actions from a given state:
    - The immediate return of the action given the current state; and, 
    - The discounted and weighted average of the value function for all possible successor states (weighted by the probability of ending up in those states using the state transition matrix)

![[Pasted image 20230908135607.png]]

$$
v_\pi(s) = \sum_{a \in A} \pi(a|s) \left[ \mathscr{R}^a_s + \gamma \sum_{s' \in S} \mathscr{P}^a_{ss'} v_\pi(s') \right]
$$
#### Bringing it together for action-value functions

- You can do the same thing for action values. Using the same idea, starting form a particular state and action, we can look ahead 2 steps to the state we end up in, then what action we might take next.

![[Pasted image 20230908140107.png]]

$$
q_\pi(s, a) = \mathscr{R}^a_s + \gamma \sum_{s' \in S} \mathscr{P}^a_{ss'} \sum_{a' \in A} \pi(a' | s') q_\pi(s', a')
$$

> [!tip] Example: Bellman Expectation Equation in Student MDP
> ![[Pasted image 20230908140325.png]]
> We'll consider just one state (the red 7.4 state) - recall we're evaluating a policy where we do everything 50/50). We can verify that the value of this state is 7.4 by doing look-aheads to each of the possible actions and states from there.
> - If we Study again, the immediate reward is 10, and the value function of the successor state is 0.
> - If we go to the Pub, our immediate reward is 1, but we'll also be kicked into any of 3 other states with probabilities 0.2, 0.4, 0.4.

#### Matrix Form

- The Bellman equation can also be expressed in matrix form. $\mathscr{R}^\pi$ is our immediate rewards averaged over our policy and $\mathscr{P}^\pi$ is our transition probabilities averaged over our policy.

$$v_\pi = \mathscr{R}^\pi + \gamma \mathscr{P}^\pi \mathbf{v}_\pi$$
$$ v_\pi = (I - \gamma \mathscr{P}^\pi)^{-1} \mathscr{R}^\pi $$

- There are more efficient ways to solve for the value function which we'll talk about later.

## Optimal value function

- Here we start to think about how we find the best behaviour in an MDP.
    - The *optimal value function* specifies the best possible performance in an MDP; i.e. we have 'solved' an MDP once we know this function.

> [!info] Definition: Optimal value function
> The *optimal state-value function* $v_*(s)$ is the maximum value function over all policies. i.e. the maximum amount of reward you can extract from the system by following the 'optimal' policy.
> $$v_*(s) = \max_\pi v_\pi(s)$$
> The *optimal action-value function* $q_*(s,a)$ is the maximum action-value function over all policies. i.e. Given you take a certain action, what is the maximum possible value you can get from that point onwards?
> $$q_*(s, a) = \max_\pi q_\pi(s, a)$$
> Once we know $q_*(s,a)$, then we're basically done and have solved the MDP.

> [!tip] Example: Student MDP
> ![[Pasted image 20230924130359.png]]
> Assuming a discount factor ($\gamma$) of 1 for simplicity, the diagram above shows the optimal value function for each state in the process. 
> - The last node (10) shows that the value of that node is 10 because you can get +10 for studying, but only +1 for going to the pub
> - The second last node (8) is 8 because sleeping only gives a reward of 0 here; so instead you can study (-2) and then sleep (+10) which gives a value of 8
> 
> This doesn't really tell us how to behave - only what the reward for optimal behaviour. To do this, we define our $q_*$s. So each of the actions is now labelled with their value below.
> ![[Pasted image 20230924130933.png]]
> Considering the two possible actions from the middle (8) node:
> - The sleep action locks in 0 immediate reward and ends in a terminal state with 0 value, so sleeping will give a total reward of 0
> - Studying locks in -2 immediate reward, then the value of the next state is 10, so the action-value function here is -2 + 10 = 8

## Optimal policy

- Define a partial ordering between policies: $\pi \geq \pi^{\prime} \text{ if } v_\pi(s) \geq v_{\pi^{\prime}}(s), \forall s$
    - That is, a policy is better than or equal to another if the value function for that policy is better than or equal to the other's.
    - This needs to be true in *all* states for us to say a policy is $\geq$ another

> [!info] Theorem
> For any Markov Decision Process:
> - There exists an *optimal policy* $\pi_*$ that is better than or equal to all other policies, $\pi_* \geq \pi, \forall \pi$
> - All *optimal policies* achieve the optimal value function, $v_{\pi_*}(s)= v_*(s)$
> - All *optimal policies* achieve the optimal action-value function, $q_{\pi_*}(s,a) = q_{*}(s,a)$
>   
> i.e. There is always *at least* one (there can be more than one) optimal policy that is better than or equal to all other policies. This means you won't need to 'mix and match' policies to get the optimal reward.

- Formally, an optimal policy can be found by maximising $q_{*}(s,a)$ at every state.
    - i.e. At every state, you pick the action that gives you the most possible reward with probability 1.
    - There is always a deterministic optimal policy for any MDP - no need to randomise actions, there is always a set of actions that is optimal
    - If we know $q_{*}(s,a)$, we know the optimal policy
$$
\pi_*(a|s) = 
\begin{cases} 
1 & \text{if } a = {\arg\max}_{a \in A} q_*(s, a) \\
0 & \text{otherwise}
\end{cases}
$$

> [!tip] Example
> ![[Pasted image 20230924133336.png]]
> The optimal actions are now highlighted in red - we can now see the optimal (reward maximising) path through the MDP.
> 
> Intuitively, we arrive at these numbers by working backwards, looking ahead from each state. This is the logic underlying the Bellman equation.

## Bellman optimality equation

i.e. the *real* Bellman equation.
### For v*

![[Pasted image 20230924133648.png]]

- To solve for the optimal value of being in some state, consider each of the actions you might take from there as well as their action-values. We then take the maximum of them (instead of the average of them like previously). i.e.:
$$v_*(s) = \max_{a} q_*(s, a)$$
### For q*

![[Pasted image 20230924134051.png]]

- The other half of this; how we can figure out the optimal action-value for each possible action.
- Given you take a certain action, it's now up to the environment to determine which state we end up in next - note we have no control over this. Hence, we'll need to average over all possible states we can end up in given the action we've taken.
    - Assuming we know the optimal value of each of those states, we average those values weighted by the probability that we'll end up in each of them. Recall that $\mathscr{P}^a_{ss^{\prime}}$ denotes the probability, given an action $a$ from state $s$, that you'll end up in state $s^{\prime}$.
$$q_*(s, a) = \mathscr{R}^a_s + \gamma \sum_{s' \in \mathscr{S}} \mathscr{P}^a_{ss'} v_*(s')$$
### For V*

![[Pasted image 20230924134858.png]]

- Stitching the $v_*$ and $q_*$ look-aheads together, to determine your optimal value function at a given state, you:
    - Look ahead over the actions you can take and finding the one with maximal value
    - Then looking over the states you can end up in given that maximal value action and averaging the optimal values of those
$$v_*(s) = \max_{a} \mathscr{R}^a_s + \gamma \sum_{s' \in \mathscr{S}} \mathscr{P}^a_{ss'} v_*(s')$$
### For Q*

![[Pasted image 20230924135247.png]]

- A re-ordering of the same idea, and starting from an action instead of a state.
- To determine the optimal action-value function for a given state and action, 
    - Look ahead across all possible states the environment will put you in given your starting state and action
    - From those states, you have agency again and can choose the next action with the maximal action-value
    - Average these across all possible states you might be blown into from the original state/action.
$$q_*(s, a) = \mathscr{R}^a_s + \gamma \sum_{s' \in \mathscr{S}} \mathscr{P}^a_{ss'} \max_{a'} q_*(s', a')$$

> [!tip] Example
> ![[Pasted image 20230924135713.png]]
> Focusing on the left-most node (the red 6); you can either:
> - Facebook (R = -1), then end up in a state with optimal value of 6 (overall value -1 + 6)
> - Study (R = -2), then end up in a state with optimal value of 8 (overall value -2 + 8)

### Solving the Bellman optimality equation

- Previously we saw linear equations that could be solved using matrix inversion, however the Bellman optimality equation is non-linear.
    - In general, there is no closed-form solution.
- There are iterative solution methods to solve this (dynamic programming), we'll discuss these in the next lecture
    - Value Iteration
    - Policy Iteration
    - Q-Learning
    - Sarsa

## Extensions to MDPs

- Skipping this for now; this covers:
    - Infinite and continuous MDPs 
    - Partially observable MDPs 
    - Undiscounted, average reward MDPs