#course_google-deepmind-reinforcement-learning #reinforcement-learning 
## Markov processes

- *Markov decision processes* (MDPs) formally describe an environment for machine learning
    - We'll start with the nice case where the environment is *fully observable*. The agent gets all the information from the environment.
    - Almost all RL problems can be formalised as MDPs
        - Optimal control deals with continuous MDPs
        - Any partially observable problems can be converted into MDPs
        - Bandits (common formalism, you get a set of actions, you perform one action and the round ends) are MDPs with one state

As discussed in the [[1 RL Introduction to Reinforcement Learning#^c3f6cb|previous lecture]]:

> [!info] Definition: Markov State
> A state $S_t$ is *Markov* iff:
> $$\mathbb{P}[S_{t+1}|S_t]=\mathbb{P}[S_{t+1}|S_1, \dots, S_t]$$
> In other words, the probability of the next state given the state you're in is the same as if you showed all of the previous states to the system.

UP TO 3:28