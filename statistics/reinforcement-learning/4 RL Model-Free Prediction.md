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
> How do we calculate this for all states when we can't reset our state back to certain points (i.e. we need to run the full trajectory)? There are two methods, both of which are effective…

### First-visit Monte-Carlo policy evaluation

In short, the first (and only the first) time we visit a given state $s$ each episode, we count it and track the returns from that point. Over all episodes, we estimate $v_\pi(s)$ as $\text{sum of returns over all first-time visits}/\text{number of episodes in which the state was visited}$.

In detail, to evaluate state $s$, the *first* time we visit it in episode $i$ we:
- Increment a counter $N(s) \leftarrow N(s) + 1$
- Increment total return $S(s) \leftarrow S(s) + G_t$
- Estimate value using mean return of first-time visits across all episodes $V(s) = S(s)/N(s)$
    - By the law of large numbers, as $N(s) \rightarrow \infty$, $V(s) \rightarrow v_\pi(s)$ 
### Every-visit Monte-Carlo policy evaluation

The same as [[#First-visit Monte-Carlo policy evaluation]], but instead of incrementing the counter and total return only the first time we visit a state, we do it *every time* we visit the state in each episode.

To evaluate state $s$, *every* time we visit it in episode $i$ we:
- Increment a counter $N(s) \leftarrow N(s) + 1$
- Increment total return $S(s) \leftarrow S(s) + G_t$
- Estimate value using mean return of visits across all episodes $V(s) = S(s)/N(s)$

> [!tip] Example: Blackjack
> 
> Sidenote: do something with [justBlackjack](https://wongd-hub.github.io/justBlackjack/)…
> 
> - *States*: 200, encompasses:
>     - Current sum of your hand (12-21), if sum $\le$ 11 then we automatically draw another card (as the max value of a card is 10)
>     - Dealer's showing card (ace-10)
>     - Do you have a 'useable' ace (yes-no)
> - *Actions*:
>     - Stand: Stop receiving cards and terminate episode
>     - Hit: Take another card
> - *Rewards*:
>     - Stand: 
>         - $+1$ if sum of cards $\gt$ sum of dealer's hand
>         - $0$ if sum of cards $=$ sum of dealer's hand
>         - $-1$ if sum of cards < sum of dealer's hand
>     - Hit: 
>         - $-1$ if sum of cards $\gt$ 21 (and terminate episode)
>         - $0$ otherwise
>  
>  For now, consider the simple policy of *stand* if sum of cards $\ge$ 20, else *hit*.
>  - The following graphs show the value function/expected return in each state using Monte-Carlo learning after 10k and 500k episodes.
>  - We can see that after 10k episodes, the case where we have a usable ace is still not stable - this is because this is a rare occurrence. On the other hand, not having a usable ace is already fairly stable at that point.
>  - Otherwise, you only really do well if you have 20/21, otherwise you do badly due to the simplistic policy.
>  
>  ![[Pasted image 20231024225149.png]]

### Incremental Monte-Carlo learning

> [!info] Incremental mean calculation
> A short proof to show that it is possible to calculate a mean *incrementally* as we continue to run through episodes.
> 
> The mean $\mu_1, \mu_2, \dots$ of a sequence $x_1, x_2, \dots$ can be computed incrementally. This can be done by taking the previous mean $\mu_{k-1}$ and adjusting it towards the newly observed value by some step size $\frac{1}{k}$ and error from the previous value $x_k - \mu_{k-1}$.
> 
> $$
> \begin{align*}
> \mu_k &= \frac{1}{k} \sum_{j=1}^{k} x_j \\
> &= \frac{1}{k} \left( x_k + \sum_{j=1}^{k-1} x_j \right) \\
> &= \frac{1}{k} \left( x_k + (k - 1)\mu_{k-1} \right) \\
> &= \mu_{k-1} + \frac{1}{k} (x_k - \mu_{k-1})
> \end{align*}
> $$

We use the same idea to perform incremental Monte-Carlo updates over each episode. This way we don't need to keep track of the sum ($S(s)$) and only the counter and working mean.
- Update $V(s)$ incrementally after episode $S_1, A_1, R_2, \dots, S_T$
- For each state $S_t$ with return $G_t$:
    - $N(S_t) \leftarrow N(S_t) + 1$
    - $V(S_t) \leftarrow V(S_t) + \frac{1}{N(S_t)}\left(G_t-V(S_t)\right)$

In non-stationary problems, it can be useful to track a running mean (i.e. forget old episodes).
- This is often the case, as things change in the real world so remembering all old returns would lead to bias.
- This also means that we also don't need to track the counter $N(s)$
$$V(S_t) \leftarrow V(S_t) + \alpha\left(G_t-V(S_t)\right)$$

## Temporal-Difference learning

34:00