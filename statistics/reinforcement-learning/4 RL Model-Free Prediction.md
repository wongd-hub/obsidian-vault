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

Similarly to MC learning, TD learning:
- Learns directly from episodes of experience
- Is model-free, no prior knowledge of MDP transitions/rewards

However, TD learning differs in that:
- We no longer need to see entire episodes, TD learns from incomplete episodes
- TD updates an initial guess of $v_\pi(s)$ towards another guess

Together, the behaviour in the last two points describes the process of *bootstrapping*.

### MC vs. TD

*Goal*: learn $v_\pi(s)$ on-line (i.e. mid-episode(?)) from experience under policy $\pi$

How do MC and TD differ?:

- *Incremental Every-Visit Monte-Carlo* updates a value $V(S_t)$ towards an *actual* return $G_t$ that is propagated backwards from the termination of the episode.
    - In other words, we update our estimate of $V(S_t)$ by a factor ($\alpha$) of the error between our next observed return and our current estimate of $V(S_t)$.
$$V(S_t) \leftarrow V(S_t) + \alpha \left( G_t - V(S_t) \right)$$

- *Simplest Temporal-Difference Learning algorithm, TD(0)* updates value $V(S_t)$ toward an *estimated* return composed of the immediate reward and the discounted value of the successor state, $R_{t+1} + \gamma V(S_{t+1})$
    - $R_{t+1} + \gamma V(S_{t+1})$ is referred to as the TD target (what we're updating towards)
    - $\delta_t=R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$ is referred to as the TD error
$$V(S_t) \leftarrow V(S_t) + \alpha \left( R_{t+1} + \gamma V(S_{t+1}) - V(S_t) \right)$$

> [!tip] How do these methods differ in practice?
> Suppose your agent is driving a car, it sees a car hurtling toward it and it thinks it'll crash. At the last second, it doesn't crash and you continue driving.
> 
> - In Monte-Carlo, since you didn't have a crash/termination, you wouldn't get a negative reward to disincentivise whatever behaviour led to this situation. 
> - In Temporal-Difference, you can immediately update the value you had before to say that this is a bad situation and that we want to avoid situations like these. You don't need to wait until you die to update your value function.

> [!tip] Example: driving home
> ![[Pasted image 20231026075046.png]]
> 
> Imagine you're driving on your way home from work. As you leave the office, you have an initial guess of how long it'll take to get home (first row, 30 minutes).
> - It's raining when you reach the car, you think it'll take you slightly longer. 5 minutes have already elapsed and you think it'll take you 35 minutes from here to get home.
> - …
> - When you get to your home's street, you think there'll be 3 minutes left to go
> - You get home
> 
> How would we update our value function based on our trajectory/experience? The following diagram shows the difference between MC and TD methods for this problem.
> - At each point in the MC method, you're updating towards the *final* outcome, you need to wait until you've gone all the way until you have any proper estimate.
> - With TD learning, at every step you have a guess at how long it will take which is being adjusted upwards or downwards based on your experience at each step. At all points in time, you have a working estimate of the value function.
> 
> ![[Pasted image 20231026075446.png]]

### Advantages and disadvantages of MC vs. TD

- As we saw in the previous examples, TD can learn *before* seeing the final outcome
    - TD can learn on-line after every step
    - MC must wait until the end of the episode before the return is known

- As a consequence, TD can learn *without* a final outcome
    - TD can learn from incomplete sequences, whereas MC cannot
    - TD works for continuing (non-terminating) environments where as MC only works in episodic (terminating) environments

### Bias-Variance trade-off

- The following measures are *unbiased*:
    - The true return $G_t = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{T-1} R_T$ is an unbiased estimate of $v_\pi(S_t)$
        - Hence, Monte-Carlo is unbiased
    - The true TD target (i.e. we are told the true value function) $R_{t+1} + \gamma v_{\pi}(S_{t+1})$ is an unbiased estimate of $v_\pi(S_t)$ (the Bellman equation)

- However, the actual TD target $R_{t+1} + \gamma v_{\pi}(S_{t+1})$  (using our best guesses) is a *biased* estimate of $v_\pi(S_t)$
    - But the TD target has much lower *variance* than the return $G_t$
    - This because there is noise inherent to each random action, transition, and reward that makes up $G_t$ (the entire trajectory), whereas the TD target depends on only one random action, transition, and reward.

> [!info]
> - MC has high variance, but zero bias
>     - Good convergence properties (just works out of the box, even with function approximation)
>     - Not very sensitive to the initial value since you're not bootstrapping off that initial value
>     - Very simple to understand and use
> 
> - TD has low variance but some bias
>     - Usually more efficient than MC since it's lower variance
>     - TD(0) converges to true value function $v_\pi(s)$ (but not always with function approximation)
>     - More sensitive to initial value due to bootstrapping from that value
> 
> NB: We'll touch on *function approximation* in later lectures, the general gist is that estimating the value function for each value separately is inefficient, so we use a function approximation to estimate the value function

> [!tip] Example: random walk
> ![[Pasted image 20231026082205.png]]
> - Actions: left and right
> - Policy: a uniform random policy that goes left/right 50%/50%.
> - States: left-most square is a terminal state, all actions have 0 rewards, apart from the state on the right which moving into has a return of 1
>   
> Solving this using TD, we can see that:
> - At the 0th iteration, we have all 0s, our initialising values
> - At the 1st iteration, we start to move towards the true value function (light grey)
> - At 100 iterations, we are close to the true value function
> 
> ![[Pasted image 20231026082438.png]]
> 
> Comparing against MC, we calculate the RMSE against the actual value of each state as we increase the number of iterations
> - We can see that TD much more efficiently arrives an optimal solution whereas MC's RMSE trends downwards much more slowly (but presumably will zero in on the true value function precisely)
> 
> ![[Pasted image 20231026082642.png]]

### Batch MC and TD

- We've seen that MC and TD converge $V(s) \rightarrow v_\pi(s)$ as episodes $\rightarrow \infty$
- What happens if we stop after a finite number of episodes, or show only a certain set of episodes to the agent and train over those repeatedly?

$$
\begin{align*}
\begin{aligned}[t]
& S^1_1, a^1_1, r^1_2, \dots, S^1_T \\
& \vdots \\
& S^K_1, a^K_1, r^K_2, \dots, S^K_T
\end{aligned}
\end{align*}
$$

e.g. Repeatedly sample episode $k \in [1, K]$ and apply MC or TD(0)

> [!tip] Example: AB
> Consider a simple MDP with two states, A and B, no discounting, and 8 episodes of experience
> 
> ```
> A, 0, B, 0
> B, 1
> B, 1
> B, 1
> B, 1
> B, 1
> B, 1
> B, 0
> ```
> What is $V(A)$ and $V(B)$? $V(B)$ is a little more intuitive since we've seen more of it, we are likely to estimate it as $\frac{6}{8}$. However, we've only seen A once.
> 
> The answer is that $V(A)$ is $0$ or $\frac{6}{8}$ depending on if you use MC or TD.
> - With MC, we've seen one episode of A, and the return was $0$ from that point until termination. $\therefore V(A) = 0$.
> - With TD, we build the likeliest MDP that explains what we've seen so far (see below, i.e. the maximum likelihood MDP). Our values will then be based off of this.
>     - When we are in A, we transition to B 100% of the time (based off the first observed episode)
>     - From B, we have a $\frac{6}{8}$ chance to get a reward of 1
>     - $\therefore V(A)=\frac{6}{8}$
>       
> ![[Pasted image 20231026084513.png]]

From this, we can intuitively see that:

- MC always converges to the solution with the minimum mean-squared error - i.e. the best fit to the observed returns
$$\sum_{k=1}^{K} \sum_{t=1}^{T_k} \left( g^k_t - V(s^k_t) \right)^2$$
- TD(0) converges to the solution of the maximum likelihood Markov model - i.e. solution to the MDP $\langle S, A, \hat{P}, \hat{R}, \gamma \rangle$ that best fits the data

    - First equation: Transition probability of moving from $s$ to $s^\prime$ by taking action $a$ 
        - Numerator: Number of times that taking action $a$ from $s$ leads to $s^\prime$
        - Denominator: Number of times taking action $a$ from state $s$

    - Second equation: Reward function for taking action $a$ from state $s$
        - Numerator: Sum of reward $r^k_t$ only if taking action $a$ from state $s$
        - Denominator: Number of times taking action $a$ from state $s$

$$
\hat{P}^a_{s,s'} = \frac{1}{N(s, a)} \sum_{k=1}^{K} \sum_{t=1}^{T_k} 1(s^k_t, a^k_t, s^k_{t+1} = s, a, s')
$$
$$
\hat{R}^a_s = \frac{1}{N(s, a)} \sum_{k=1}^{K} \sum_{t=1}^{T_k} 1(s^k_t, a^k_t = s, a) r^k_t
$$

- Because of this, TD exploits the Markov property and hence works better in Markov environments
- MC does not exploit the Markov property, usually more effective in non-Markov environments

### Unified view

Bringing this all together

*Monte-Carlo backup*: $V(S_t) \leftarrow V(S_t) + \alpha \left( G_t - V(S_t) \right)$

- MC we can think of like backups where we start at some state $S_t$ and implicitly there's a lookahead tree from that state. How do we make use of this lookahead tree to find the value function of $S_t$?
    - MC samples one complete trajectory ($S_t$, $A_t$, $R_t$, $S_{t+1}$, $A_{t+1}$, etc) and uses it to update the value function at the root state and every intermediate state

![[Pasted image 20231026085847.png]]

-----

*Temporal-Difference backup*: $V(S_t) \leftarrow V(S_t) + \alpha (R_{t+1} + \gamma V(S_{t+1}) - V(S_t))$

- In TD, the backup is just over one step. We sample our action, state and reward (a sample of one step ahead), then back that up to the root node.

![[Pasted image 20231026195505.png]]

----

*Dynamic Programming backup*: $V(S_t) \leftarrow \mathbb{E}_\pi \left[ R_{t+1} + \gamma V(S_{t+1}) \right]$

- In DP, we also did a one-step lookahead but we didn't sample. We had to know the dynamics (all transition probabilities and actions) and had to use those to compute a full expectation across all eventualities at the root node.
- You could also go all the way down the tree which would give you an *exhaustive tree search*.

![[Pasted image 20231026195719.png]]

#### Bootstrapping and sampling

- Lets tease these components into dimensions with which we can define our algorithm space

|    | **Bootstrapping**                  | **Sampling**                                                   |
| --- | :----------------------------------: | :--------------------------------------------------------------: |
|     | *using an estimate of the returns* | *update samples an expectation (instead of all eventualities)* |
| MC  |           ❌                         |        ✅                                                        |
| DP  |           ✅                         |           ❌                                                     |
| TD    |         ✅                           |     ✅                                                           |

#### Unified view of reinforcement learning

![[Pasted image 20231026200615.png]]

- This is a unified view of methods for policy evaluation (we also have one for control later).
    - On the y-axis we have *breadth/width* of the lookahead. We can either do full-width backups (across all actions/eventualities), or we can sample backups like we do in TD/MC.
    - On the x-axis we have the *depth* of the lookahead. We can either look only a few steps ahead (TD) (which necessarily means bootstrapping) or look all the way until the end of the episode and propagate the reward backwards (MC).

For the remainder of this lecture, we'll discuss the spectrum on the x-axis. There exists a generalised method that has pure MC and TD(0) on its extremes, $TD(\lambda)$.

### TD(lambda)
#### *n*-Step prediction and return

- With $TD(0)$, we take one step then estimate our reward. Why not take this to an arbitrary number of steps, $n$?

![[Pasted image 20231026231655.png]]

- Consider the following $n$-step returns for $n = 1, 2, \dots, \infty$:
$$
\begin{array}{rll}
n = 1 & (TD(0)) & G_t^{(1)} = R_{t+1} + \gamma V(S_{t+1}) \\
n = 2 & & G_t^{(2)} = R_{t+1} + \gamma R_{t+2} + \gamma^2 V(S_{t+2}) \\
\vdots & & \vdots \\
n = \infty & (MC) & G_t^{(\infty)} = R_{t+1} + \gamma R_{t+2} + \ldots + \gamma^{T-1} R_T \\
\end{array}
$$
- Hence, the generalised $n$-step return is:
$$G_t^{(n)} = R_{t+1} + \gamma R_{t+2} + \ldots + \gamma^{n-1} R_{t+n} + \gamma^n V(S_{t+n})$$
- Taking the $TD(0)$ update equation and replacing the return over one step with our $n$-step return, we are left with the following equation:
$$V(S_t) \leftarrow V(S_t) + \alpha (G_t^{(n)} - V(S_t))$$

This is valid for any $n$, so which $n$ is the best?

> [!tip] Example: random walk
> Same random walk example we looked at earlier in the lecture. This graph shows the RMSE against the true value function across different step sizes ($\alpha$) on the x-axis, each line represents a different $n$(-steps).
> 
> *NB*: On-line vs. off-line refers to whether you immediately update your value function after your step, or if you defer your updates until the end.
> 
> - As $n$ approaches $\infty$, we see what happens when we use Monte Carlo learning. You are left with a high minimum error in this case.
> - There is exists a global minimum with the right $\alpha$ and $n$
> 
> ![[Pasted image 20231026233227.png]]
> 
> But how do we know which $n$ is the correct $n$ if we don't know the true value function? In the remainder of this lecture we're going to arrive at an algorithm that gets the best of all $n$.
#### Averaging *n*-step returns
#### lambda Return

> [!info] Averaging $n$-step returns
> - We can average $n$-step returns over different $n$, e.g. we do a single backup using the average of the 2-step and 4-step TD reward estimates $\frac{1}{2} G^{(2)} + \frac{1}{2} G^{(4)}$
> - This combines information from multiple $n$, and there is an efficient way to average across all $n$ - the $\lambda$-return.

![[Pasted image 20231027084020.png]]

> [!tip]
> Note that we'll approach $TD(\lambda)$ from the forward view first to gain intuition on the theory, then backwards to see the real implementation.

- The $\lambda$-return $G^\lambda_t$ combines returns from all $n$ steps $G^{(n)}_t$ efficiently.

    - It is defined as the geometrically weighted average of returns from all $n$. Weights are calculated as $(1-\lambda)\lambda^{n-1}$
        - This uses a constant, $\lambda$ which tells us how much we decay the weight at each $n$ step increment.
        - All weights sum to 1, the last weight is the complement of all existing weights (hence the terminal weight may be larger than previous weights).
$$G_t^\lambda = (1 - \lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_t^{(n)}$$
- We can see from this that $\lambda = 1$ is equivalent to Monte-Carlo, whereas $\lambda=0$ is equivalent to $TD(0)$
    - Using an intermediate value of $\lambda$ allows us to tune our bias-variance tradeoff (i.e. the tradeoff between high variance but low bias deep backups and low variance but high bias bootstrapping)

![[Pasted image 20231027085124.png]]

> See how the weights look above. Why use a geometric weighting instead of arithmetic? Geometric weightings are memoryless(?) which allows us to compute this efficiently - we can do $TD(\lambda)$ for the same cost as $TD(0)$ this way.

- The forward view on $TD(\lambda)$ is then $V(S_t) \leftarrow V(S_t) + \alpha \left( G_t^\lambda - V(S_t) \right)$
#### Forward view TD(lambda)

![[Pasted image 20231027085318.png]]

- A bit like Monte-Carlo, if we want to implement the forward view version of this algorithm, we have to wait until the end of the episode to get our $n$-step returns.
- However there's an equivalent version that achieves the same result but without needing to wait until the end of the episode.

> [!tip] Example: random walk
> ![[Pasted image 20231027085543.png]]
> 
> This shows again our RMSE against the true value function, however each line now represents a different value of $\lambda$.  We can see that the best value of $\lambda$ does just as well as the value of the best $n$ in the previous example, and that the sweet spot is much more robust.
> 
> Further, the optimal value of $\lambda$ would be the same regardless of the size of the random walk==(?)==.
#### Backward view TD(lambda)

- Forward view provides the theory, backward view provides the mechanism
- Backward view allows us to update on-line, every step, from incomplete sequences

> [!tip] Eligibility traces
> ![[Pasted image 20231027090342.png]]

1:30:38

