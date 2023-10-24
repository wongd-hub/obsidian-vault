#course_google-deepmind-reinforcement-learning #reinforcement-learning 

[Slide link](https://www.davidsilver.uk/wp-content/uploads/2020/03/MC-TD.pdf)

- *Model-free prediction*: Take an environment that can be represented as a MDP, but no one gives us the information on this (unlike with dynamic programming).
    - We'll understand how we figure out the optimal way to behave when no one tells us the environment.

- Two classes of methods here
    - *Monte-Carlo Learning*: Methods which go all the way to the end of a trajectory and estimate value by looking at sample returns.
    - *Temporal-Difference Learning*: Can be more efficient. Look one/n steps ahead and estimate the return after that step.
    - We can unify these methods and there's a whole spectrum of methods in between (TD($\lambda$))
## Introduction

- Last lecture: how do we solve a MDP (find the optimal behaviour that maximises reward) where we already know the dynamics and rewards. 
    - Use DP to evaluate a policy, then use that as an inner loop to find the optimal policy.
- This lecture: model-free prediction, go directly from the experience the agent has to a value function/policy with no prior knowledge of the MDP.
    - Will break this down into policy evaluation, then use our methods for policy evaluation to help us do control.
    - This lecture will focus on the policy evaluation/prediction; what is the value of a given policy.
- Next lecture: model-free control, find the optimal value function in the MDP

## Monte-Carlo reinforcement learning

Monte-Carlo learning describes a class of methods that have the agent fully explore a trajectory then estimate the value of each state/action by looking at sample returns.

- Learn directly from episodes of experience, so we don't need a model prior to learning.
- Learns from *complete* episodes (i.e. play the full scenario and propagate rewards backwards). In other words, we *do not bootstrap*.
    - Hence this only works for episodic MDPs - you need to terminate the episode for this to work.
- MC uses the simplest possible idea to estimate the value function. Take sample returns, and then estimate the value as the mean of observed returns.

> [!info] Monte-Carlo Policy Evaluation
> Recall that:
> - Return at time step $t$ is the total discounted reward from that point onwards $G_t = R_{t+1} + \gamma R_{t+2} + … + \gamma^{T-1} R_T$
> - The value function is the expected return $v_\pi(s) = \mathbb{E}_\pi [ G_t | S_t = s]$
> 
> *Goal*: learn $v_\pi$ (expected future return from any state) from observation of episodes of experience under policy $\pi$
> - Observation of the episodes gives us a stream of states, actions, and resulting rewards ($S_1, A_1, R_2, …, S_k \sim \pi$)
> - This gives us our total reward from each time step onwards (with which we calculate $G_t$)
> - Monte-Carlo learning then estimates $v_\pi(s)$ as the empirical mean return from each state (in place of the expectation above)
>   
> How do we calculate this for all states when we can't reset our state back to certain points (i.e. we need to run the full trajectory)? There are two methods…




