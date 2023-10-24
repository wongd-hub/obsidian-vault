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

## Monte-Carlo Reinforcement Learning

Monte-Carlo learning describes a class of methods that have the agent fully explore a trajectory then estimate the value of each state/action by looking at sample returns.

- Learn directly from episodes of experience, so we don't need a model prior to learning.
- Learns from *complete* episodes (i.e. play the full scenario and propagate rewards backwards). In other words, we *do not bootstrap*.
    - Hence this only works for episodic MDPs - you need to terminate the episode for this to work.
- MC uses the simplest possible idea to estimate the value function. Take sample returns, and then estimate the value as the mean of observed returns.