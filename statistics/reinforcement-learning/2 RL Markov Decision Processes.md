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
- Recall that $S_t$ characterises everything about what happens next (Markov by definition), that should mean that there is some well-defined transition probability that i will transition to another state.

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

> [!info] Definition: Return
> The *return* $G_t$ is the total *discounted* return from time-step $t$ 
> $$G_t=R_{t+1}+\gamma R_{t+2}+\dots=\sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$
> $\gamma \in [0, 1]$, our discount factor, tells us how much you care now about rewards you get in the future. In other words, the value of receiving $R$ in $k+1$ time-steps is $\gamma^kR$
> - $\gamma = 0$ means you care only about immediate returns
> - $\gamma = 1$ means you care about future returns equally as much as present returns

