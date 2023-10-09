#course_google-deepmind-reinforcement-learning #reinforcement-learning 

[Slide link](https://www.davidsilver.uk/wp-content/uploads/2020/03/DP.pdf)
## Introduction

- Dynamic programming is one of the ways in in which we solve Markov Decision Processes. It is one of the fundamental building blocks behind RL.
- We'll walk through three basic paradigms in which dynamic programming can be applied:
    - **Policy evaluation**: if someone gives you a policy, how can you evaluate how good that policy is?
    - **Policy iteration**: This is the first method with which we can start to solve the MDP (i.e. find the optimal policy). This essentially takes policy evaluation and turns it into a loop which runs until we find our optimal policy.
    - **Value iteration**: A related approach, which works directly on value functions and not the policy space. This uses the Bellman equation iteratively until we arrive at the optimal solution.
- We'll also go through some extensions to dynamic programming. In subsequent lectures, we'll pull out the key ideas of the three methods above and turn them into more general RL methods.

### What is dynamic programming?

> [!info] Dynamic programming
> '*Dynamic*' implies that we're trying to solve a problem that has a sequential or temporal aspect
> 
> '*Programming*' refers to mathematical programming (like linear programming), i.e. optimising some program (in our case, the policy - a mapping between states and actions)
> 
> > Dynamic programming lets us solve complex problems by breaking them down into subproblems and putting them back together to get the overall solution.
> 
> As well as in RL, the method of dynamic programming is applied in many fields:
> - Scheduling algorithms
> - Graph algorithms (e.g. shortest path)
> - Graphical models (e.g.  Viterbi algorithm)
> - Bioinformatics (e.g. lattice models)

This is a general solution method for problems that have two properties:

- **Optimal substructure** - i.e. the principle of optimality applies - the optimal solution can be decomposed into subproblems ^3a68c9
    - This means that you can solve some overall problem by breaking it down into pieces, and the optimal solution to those pieces tells you how to get the optimal solution to your overall problem.
    - If this didn't apply, it would be fruitless to solve your subproblems since they won't take you any closer to solving the overall problem.
    - An example is the shortest path problem. The shortest path between you and some point X can be broken down into the shortest path between you and some closer point Y, then the shortest path between Y and X.

- **Overlapping subproblems** - i.e. recurring subproblems, which means that solving one subproblem helps you solve others
    - This means that we can cache our solutions to these subproblems and reuse them.

- **Markov Decision Processes satisfy both properties**.
    - The Bellman equation provides a recursive decomposition and tells us how to break down the optimal value function into the optimal behaviour for one step and the optimal behaviour after that step. This means MDPs satisfy the principle of optimality.
    - The value function allows us to cache and reuse information we've figured out about the MDP. From any state, the value function tells you the best way to behave from that state onwards. Once you have this, you can work your way backwards without having to re-compute the rest of the states.

### Planning by dynamic programming

The focus of this lecture will be *planning* via dynamic programming.

- Recall that planning is not the full RL problem, but is where someone tells you the structure of an MDP, and given the knowledge of how the environment works we try to solve for the perfect behaviour.

- We can solve two cases of planning in the MDP
    - We can plan to solve the *prediction* problem where the input is the MDP ($\langle \mathscr{S}, \mathscr{A}, \mathscr{P}, \mathscr{R}, \gamma \rangle$) or Markov Reward Process and a policy $\pi$. The output of the planning is the value function $v_\pi$, specifying how much you're going to get from each state. This is policy evaluation. 
        - Recall that an MDP can be specified by its state and action spaces, the reward function, and the discounting factor
    - We can plan to solve the *control* problem where we're trying to figure out the best thing to do in the MDP (i.e. full optimisation). Again, someone tells you the full MDP but no policy, as the output of planning here will be the optimal value function, $v_*$. Knowing the optimal value function implies that we'll also know the optimal policy $\pi_*$, which is what we ultimately care about.

## Policy evaluation
### Iterative policy evaluation

Someone tells you the policy, and you need to evaluate it.

- **Problem**: evaluate a given policy, $\pi$
- **Solution**: iterative application of Bellman expectation backup
    - Note that we use the Bellman expectation equation to do policy evaluation, but later we'll use the Bellman optimality equation to do control.

- We will apply the Bellman expectation equation iteratively to do this. 
    - We'll start with some arbitrary initial value (for example 0) for all states in the MDP (call this a vector, $v_1$).
    - We'll then apply our Bellman expectation equation in a one-step lookahead from all states, and using this we'll update the value of each state to a vector, $v_2$.
    - Iterate this process many times and we eventually converge on the true value function, $v_\pi$ (this will be proven at the end of the lecture).

> [!info] Synchronous backups
> We use *synchronous* backups, which means that we consider all states at every iteration. In other words:
> 
> > At each iteration ($k + 1$), update $v_{k+1}(s)$ from $v_k(s^\prime)$ $\forall \space s \in \mathscr{S}$, where $s^\prime$ is a successor state to $s$.
>   
> We will discuss *asynchronous* backups later.

How are we actually performing this update?
 ![[Pasted image 20231004215136.png]]

- Consider again this one-step lookahead tree. Recall that to find the value of some state $s$ (the root of this tree), we need to:
    1. Look ahead across all actions we could take from this state
    2. See what state $s^\prime$ the environment might place us in once we take each of those actions, in our case these are represented by the leaves
    3. Then back up the value functions of each successor state, the expected value of $v_{k+1}(s)$ is then the sum of these value functions weighted by the probability that you'll end up in them

- We can see this in equation form (normal and matrix form) below. Note how we are summing - for all possible actions - the probability of taking that action from the current state, multiplied by the immediate return, and the discounted and probability-weighted value functions for all possible successor states.
    - ==In other words, we define the value at the next iteration by plugging in the previous iteration's value functions at the leaves and backing those up to the root.== 
    - We do this for every single state in the MDP at every iteration.
    - Note also that since this is the *expectation* equation, we are using a sum, not a max.
$$v_{k+1}(s) = \sum_{a \in \mathscr{A}} \pi(a|s) \left( \mathscr{R}^a_s + \gamma \sum_{s' \in \mathscr{S}} \mathscr{P}_{ss'}^a v_k(s') \right)$$
$$\mathbf{v}^{k+1} = \mathscr{R}^\pi + \gamma \mathscr{P}^\pi \mathbf{v}^k$$

> [!tip] Small gridworld example - evaluating a random policy
> ![[Pasted image 20231004221751.png]]
> 
> Consider a small gridworld with a 4 x 4 grid.
> - The two shaded states in the edges are terminal states where you no longer get any reward, since rewards are negative, the goal is to get into one of these terminal states.
> - The reward for any change in state is -1 ($r = -1$), and a each state you can choose from 4 actions, move north, east, south, or west. 
>     - But if you choose an action that would take you off the grid, you remain in your current state and still incur a -1 reward.
> 
> Based on this, you can think of the optimal policy as tell you how long it'll take you to get to one of the terminal states.
> - We will consider a random policy (i.e. $\pi(n|\cdot) = \pi(e|\cdot) = \pi(s|\cdot) = \pi(w|\cdot) = 0.25$). How many steps will it take from each state to get to the closest terminal state?
>   
> In the following images, focus only on the left column for now. 
> - In the first iteration ($k = 0$), we plug in an arbitrary value for each state, 0.
> - In the next iteration, we apply the Bellman expectation equation to all states to calculate a new value function for each state.
>     - From a given state, we look at what actions we can take and generally see that each action will take you into another state in which the immediate reward is -1 and the value function of the successor state is 0 (from the first iteration). Therefore the updated value for all states apart from the terminal states is -1. ==Why are the values for the states adjacent to the terminal states not (-3 + 0) / 4 = -0.75? Does moving to a terminal state still incur an immediate reward of -1, then 0 onwards? -- **No**, this is because the reward is attached to the action that changes the state, not the state itself.==
>     - From the terminal states, you can't exit, you stay in the same state and keep getting 0 reward.
> - We apply the same process again on the third iteration ($k = 2$), again we consider the immediate reward of each possible action *plus* the value of where you end up.
>     - To get the -1.7 (the one in the top row), we can go north, east, or south; each incurs an immediate reward of -1, plus the value of where you end up is -1 (from $k = 1$). If you go west, you incur -1 but the value of where you end up is 0. Hence (-2 $\times$ 3 + -1) / 4 = -1.7(5).
> - At $k = \infty$, we converge on the true value function.
> 
> ![[Pasted image 20231004222017.png]]  
> ![[Pasted image 20231004223318.png]]
> 
> Discussing the right hand column now, this tells us that even though we're evaluating a random policy, we can use this information to compute a better policy by performing greedily with respect to the current value function. 
> - As we evaluate values at each state, we can see which state provides the highest value, and therefore which states are best to move to (i.e. the best policy occurs when we start to act greedily from the state we're in).
> - At $k = 0$, we can see that all states have all 4 arrows as 'best' options. At this point, all states have 0 value, so all states are equivalently valued/tied.
> - After one iteration, we're already doing something non-random. The states next to the terminal states point to the terminal states since they have a higher value, so if we behave greedily with respect to that value function, we'd want to move into the terminal states.
> - After just 3 iterations, we already have the optimal policy. You don't need to iterate this to convergence to get to the optimal policy.

## Policy iteration

Before we were just trying to evaluate a fixed policy. Now we want to find the best policy. We'll do this by iteratively applying policy evaluation and continually acting greedily with respect to each iteration's policy.

Given a policy $\pi$, how do we give back a policy we can say for certain is better than or equal to the policy before? We'll break this down into two steps

- Evaluate the policy $\pi$, figure out its value function: $v_\pi(s) = \mathbb{E} \left[ R_{t+1} + \gamma R_{t+2} + \dots \mid S_t = s \right]$
- Improve the policy by acting greedily with respect to $v_\pi$: $\pi^\prime = greedy(v_\pi)$
    - i.e. Look ahead in each direction and pick the action that results in the highest value

> [!info]
> After your initial policy, every step you take will be deterministic. Recall also that for any MDP there is always at least one [[2 RL Markov Decision Processes#Optimal policy|deterministic optimal policy]].

- In the Small Gridworld example above, we only needed to do this once (i.e. we fully evaluated the random policy once, and found a better policy by acting greedily) - $\pi^\prime = \pi^*$. But in general, we'll need to do more iterations of evaluation/improvement.
    - This process of policy iteration always converges to $\pi^*$.

> [!info]
> ![[Pasted image 20231007110637.png]]
> This diagram illustrates what we're doing here. We start with arbitrary inputs to our value function and a policy, then we:
> - Evaluate the value function for that policy (i.e. we calculate $v_\pi$, and go on the up arrow in the diagram)
> - Act greedily with respect to that value function to find a new policy (and go on a down arrow in the diagram)
> And as we continually do this, we converge on both the optimal value function and the optimal policy.
> 
> Note that no matter where you start, you always converge on the optimal policy. Why don't we converge to local maxima if we can start at any point? Because of the [[2 RL Markov Decision Processes#Optimal policy|partial ordering]] where a value function must have greater than or equal value *in all states* for it to be considered greater than or equal to another value function. Therefore each iteration is guaranteed to yield a value function that is the same or better than the last one. ==Probably need to flesh out this thinking a bit more, [more here](https://stats.stackexchange.com/a/297805)==.

> [!tip] Jack's car rental example
> A car rental company has two different locations you can rent from. There is a maximum of 20 cars per location.
> - There is a random rent and return process; people come in and randomly rent cars from these locations and will also randomly return cars. The rate of renting and returning differs between locations.
>     - The random process is Poisson-distributed, $n$ returns/requests with probability $\frac{\lambda^n}{n!} e^{-\lambda}$
>     - 1st location: av. requests = 3, av. returns = 3
>     - 2nd location: av. requests = 4, av. returns = 2
> - Each night you can transfer up to 5 cars between locations. What is the optimal policy for shifting cars around so that you minimise the amount of times a customer comes in and there are no cars to rent?
>     - Reward is $10 per car successfully rented.
>       
> Setting up the MDP:
> - States: two locations, max 20 cars in either
> - Actions: moving up to 5 cars between locations each night
> - Reward: $10 per car rented
> - Transitions: the Poisson-distributed return/request rates
>   
> Policy iteration looks like this. Axes are the number of cars in each location (i.e. they show the state). What is plotted is the number of cars you should transition between location 1 to 2.
> 
> ![[Pasted image 20231007112703.png]]
> 
> - We start off with our arbitrary policy where we do nothing.
> - We'll start our process by evaluating that policy which will give us some sort of value function (bottom right) upon which we can act greedily to get to our first policy iteration.
> - We continue until our policy converges to the optimal policy.

### Policy improvements

Doing this more formally:

- Consider a deterministic policy, $a = \pi(s)$

- We can improve this policy by acting greedily. We look at the value of being in a state and taking a particular action, then following your policy after that. This is the action value, so we want to pick the action that gives us the largest $q$ ($\arg\max$: the action that maximises $q$).
    - $\pi^\prime(s) = \arg\max_{a \in A} q_\pi(s, a)$

- We can show that acting greedily will at least improve our value over one step.
    - $q_\pi(s, \pi'(s)) = \max_{a \in \mathcal{A}} q_\pi(s, a) \geq q_\pi(s, \pi(s)) = v_\pi(s)$
        - If we follow our new greedy policy ($\pi^\prime$) for one step from state $s$ then the original $\pi$ onwards ($q_\pi(s, \pi'(s))$), we want to know if that gets more or less value than following $\pi$ all the way from $s$ ($q_\pi(s, a)$).
        - Plugging in the $\arg\max$ definition into the first action-value - since we're always picking the action that maximises $q_\pi$, our resulting action-value will be the maximum across all actions ($a \in \mathcal{A}$) from state $s$.
        - The maximum action-value from state $s$ must be at least as good as one particular action we can plug in ($\max_{a \in \mathcal{A}} q_\pi(s, a) \geq q_\pi(s, \pi(s))$).
        - Overall, we can see that the value function improves or stays the same over one step if we act greedily over that step.

- Iterating this and unpicking the action-value using the Bellman equation, we can see that the greedy policy over one step, then the original policy from there results in a value function that is $\geq$ the original policy over all steps.
$$
\begin{align*}
v_\pi(s) &\leq q_\pi(s, \pi'(s)) = \mathbb{E}_{\pi'} [R_{t+1} + \gamma v_\pi(S_{t+1}) | S_t = s] \\
&\leq \mathbb{E}_{\pi'} [R_{t+1} + \gamma q_\pi(S_{t+1}, \pi'(S_{t+1})) | S_t = s] \\
&\leq \mathbb{E}_{\pi'} [R_{t+1} + \gamma R_{t+2} + \gamma^2 q_\pi(S_{t+2}, \pi'(S_{t+2})) | S_t = s] \\
&\leq \mathbb{E}_{\pi'} [R_{t+1} + \gamma R_{t+2} + \dots | S_t = s] \\
&= v_{\pi'}(s)
\end{align*}
$$

- If improvements stop then we'll be at the optimal policy
    - $q_\pi(s, \pi'(s)) = \max_{a \in \mathcal{A}} q_\pi(s, a) = q_\pi(s, \pi(s)) = v_\pi(s)$
    - This satisfies the [[2 RL Markov Decision Processes#For v*|Bellman optimality equation]], i.e. $v_\pi(s) = \max_{a \in \mathcal{A}} q_\pi(s, a)$
    - $\therefore v_\pi(s) = v_*(s) \space \forall \space s \in \mathcal{S}$, and $\pi$ is the optimal policy - this is one way of solving MDPs

### Extensions to policy iteration
#### Modified policy iteration

- Does policy evaluation need to converge to $v_\pi$? In a previous example we saw that we landed on the optimal policy long before we calculated the exact value function.
- *Modified policy iteration* has us stop the process early to avoid any wasted work.
    - We could introduce a stopping condition. $\epsilon$-convergence of value function - if our value function changes by less than $\epsilon$ on any iteration, stop the process.
    - Or simply stop after $k$ iterations.
        - $k = 3$ was sufficient in the small gridworld example.
- We could also update our policy after every iteration. i.e. Stop evaluation after $k = 1$, act greedily with respect to that value function, then evaluate again.
    - This is equivalent to *value iteration*.

## Value iteration

- Recall that MDPs satisfy the requirements for dynamic programming including having [[3 RL Planning by Dynamic Programming#^3a68c9|optimal substructure]]. This means that any optimal policy can be subdivided into two components:
    - An optimal first action $A_*$
    - Followed by an optimal policy from the successor state $S^\prime$

> [!info] Theorem: Principle of Optimality
> A policy $\pi(a|s)$ achieves the optimal value from state $s$ ($v_\pi(s) = v_*(s)$) iff:
> 
> > For any state $s^\prime$ reachable from $s$, $\pi$ achieves the optimal value from state $s^\prime$, i.e. $v_\pi(s^\prime) = v_*(s^\prime)$.

- The intuition behind *value iteration* is that you start off at the end of your problem (i.e. someone tells you what your final reward is), then you work backwards from that figuring out the optimal path.
    - This still works with loopy and/or stochastic MDPs.
    - This also still works in MDPs with no 'final state'.

- Formally, assuming that we know the solution to $v_*(s^\prime)$, solution $v_*(s)$ can be found from a one-step lookahead 
    - $v_*(s) \leftarrow \max_{a \in \mathcal{A}} \mathscr{R}^a_s + \gamma \sum_{s' \in \mathcal{S}} \mathscr{P}^a_{ss'} v_*(s')$, recall that this is the [[2 RL Markov Decision Processes#For V*|Bellman optimality equation]].

> [!tip] Example: another small gridworld
> 
> ![[Pasted image 20231009162942.png]]
> 
> There is one goal state in this example, we want to find out how many steps it takes to reach the goal. 
> - Previously, we were looking at some random policy and trying to evaluate that. Here we're optimising for the shortest path.
> 
> Starting in the goal corner and working our way backwards, we see that the states adjacent to the goal have value of -1 since moving from them to the goal gives an immediate reward of -1. We naively apply the -1 to all other states in the MDP for now since we don't know their value yet.
> - On the second step, we see that the states further on from those are -2 since they are two steps away from the goal. In reality, we look across each state and assess possible successor states and associated rewards:
>     - The -1 states remain -1 since the best action you can take will take you to the goal; -1 reward + 0 value. The -2 states are -2 because a step in any direction will result in a -1 reward + -1 value. This is iterated until all states have had a one-step lookahead from their perspective.
>       
>  Eventually we end up with the optimal value function.
### Value iteration in MDPs

> Reiterating that we're *not* solving the entire reinforcement learning problem here, someone is telling us the dynamics of the system (the immediate rewards and the state transition matrix).

- **Problem**: find the optimal policy $\pi$
- **Solution**: iterative application of Bellman optimality backup (recall we used the Bellman expectation equation for policy iteration)
    - $v_1 \rightarrow v_2 \rightarrow \dots \rightarrow v^*$
    - We start off with our initial value (can be arbitrary values), then iterate over whole sweeps of our state space (i.e. *synchronous backups*), then update our value functions in each state using a one-step lookahead and the previous iteration to seed the current iteration's values.
    - The difference between this and policy iteration is that we're not building an explicit policy in every step, we're just building value functions.
        - This means that if we stop the algorithm at an intermediate step, we can end up with a value function that isn't achievable by *any* policy. However at the end of this process, we know that we have the value function of the optimal policy.
        - This is equivalent to modified policy iteration with $k = 1$. If we try to evaluate the policy for just one step, we're taking the greedy policy (an $\arg\max$) and evaluating it to get the max over one step - which is the same as using the Bellman optimality equation.

![[Pasted image 20231009171312.png]]

- The image above is the same slide we saw earlier for the Bellman expectation equation, but using the optimality equation. The same logic applies.
- We do synchronous sweeps - in every iteration, each state gets a turn at being the root of this diagram.
    1. We perform a one-step lookahead from each state. 
    2. We maximise over what we're able to do from that state, then we take an expectation over all the things the environment might do. 
    3. We use our previous iteration's value functions ($v_k(s^\prime)$) for the values at the leaves to calculate $v_{k+1}(s)$

i.e.
$$\begin{align}
v_{k+1}(s) &= \max_{a \in A} \left( \mathscr{R}^a_s + \gamma \sum_{s' \in S} \mathscr{P}^a_{ss'} v_k(s') \right) \\
\boldsymbol{v}_{k+1} &= \max_{a \in A} \left( \boldsymbol{\mathscr{R}}^a + \gamma \boldsymbol{\mathscr{P}}^a \boldsymbol{v}_k \right)
\end{align}
$$

Note that if both $v$s had no subscript, this would be exactly the Bellman optimality equation.

### Summary of DP algorithms

A summary of the contents of this lecture is in the image below.

![[Pasted image 20231009172400.png]]

- All of these are planning problems, we're given the dynamics of the MDP and we're trying to solve it.
    - In **Prediction**, we're trying to solve how much value we'll get from a particular policy. We use the Bellman expectation equation here. If we take that Bellman equation and turn it into an iterative update, we end up with *Iterative Policy Evaluation*.
    - In **Control**, we're figuring out how to maximise our total reward. We could use *Policy Iteration* (alternating between applying the Bellman expectation equation and policy improvement - acting greedily). Alternatively, we could use *Value Iteration* where we use the Bellman Optimality Equation (the one with the $\max$ in it) iteratively.
        - Between Policy Iteration and Value Iteration we also have Modified Policy Iteration, where Policy Iteration with $k = 1$ is equivalent to Value Iteration.

- All of these algorithms have been based on state-value functions $v_\pi(s)$ or $v_*(s)$. If we have *m* actions and *n* states then:
    - We're considering $n$ states, and for each state we consider $n$ possible successor states. With $m$ actions, our complexity is $O(mn^2)$ for synchronous backup.
- Using the action-value function $q_\pi(s,a)$ or $q_*(s,a)$ has higher complexity, but there are still legitimate reasons (like model-free control, this lets us get away from doing a one-step lookahead and allows us to not know the dynamics of the problem) for using it which we'll see in future lectures:
    - There are Bellman equations for $q$ and we can turn those into iterative updates, but their complexity is $O(m^2n^2)$ since we have to consider each state-action, and each successive state-action from there.

## Extensions to dynamic programming

- DP methods described so far use *synchronous* backups. However, there are *asynchronous backups* where we pick any state we want to be the root of the backup, perform the backup, then re-run this immediately without updating every single state.
    - This significantly reduces computation, and as long as you select every state at least once (regardless of order) then your algorithm will still converge to the optimal value function.

- Three asynchronous methods we'll focus on here. The point of difference is that each of these methods is a way to pick which states we update in each sweep.
    - *In-place* dynamic programming
    - *Prioritised sweeping*
    - *Real-time* dynamic programming

### In-place dynamic programming

- In synchronous value iteration, we store two copies of the value function $\forall \space s \in \mathcal{S}$.
    - You have to plug the old value into the leaves, and you also need to store the new value function which is what you're computing at the root.
    - Then you'll switch your new values into your old values and repeat.
$$
\begin{align} 
\color{red}{\mathscr{v}_{\text{new}}(s)} &\leftarrow \max_{a \in A} \left( \mathscr{R}^a_s + \gamma \sum_{s' \in S} 
\mathscr{P}^a_{ss'} 
\color{red}{\mathscr{v}_{\text{old}}(s')} \right) \\
\color{red}{\mathscr{v}_{\text{old}}} &\color{red}{\leftarrow \mathscr{v}_{\text{new}}} \end{align}
$$
- With in-place value iteration, we forget about the differentiation between old and new. When we sweep over states, we use the latest value instead of the old value.
    - Since this uses more new information it tends to be more efficient, and the question starts to become how can you order things to be most efficient - this motivates *prioritised sweeping*.
    - With some problems you can be much more efficient.
$$
\begin{align}
\color{red}{v(s)} &\leftarrow \max_{a \in A} \left( \mathscr{R}^a_s + \gamma \sum_{s' \in S} \mathscr{P}^a_{ss'} \color{red}v(s') \right)
\end{align}
$$
### Prioritised sweeping

- The idea here is to come up with some measure of how important it is to visit any state within the MDP, and prioritise visiting states based on a ranking of this measure.
- Use the magnitude of Bellman error to guide state selection.
    - The intuition is the states that are changing the most after your update are the ones that are most important to update.
$$\left| \max_{a \in A} \left( \mathscr{R}^a_s + \gamma \sum_{s' \in S} \mathscr{P}^a_{ss'} v(s') \right) - v(s) \right|$$
- The algorithm is to:
    1. Backup the state with the largest remaining Bellman error
    2. Update the Bellman error of affected states after each backup
    3. Repeat
- Note this requires knowledge of predecessor states ==does this mean that we can only perform this after a few steps of Value/Policy Iteration?==
### Real-time dynamic programming

- Don't just sweep over everything naively, set an agent loose and record which states it visits. Then select those states for backup.
    - After each time step $S_t$, $A_t$, $R_{t+1}$, backup state $S_t$
$$v(S_t) \leftarrow \max_{a \in A} \left( R^a_{S_t} + \gamma \sum_{s' \in S} P^a_{S_ts'} v(s') \right)$$
### Full-width backups

- DP uses *full-width backups*; this means that when we build the lookahead diagrams, we're considering *all* actions and *all* successor states. This is computationally expensive.
    - In order to do this lookahead, we need to know the dynamics of the system since we're doing planning.
- This is still effective for medium-sized problems (millions of states), but for larger problems we suffer from Bellman's *curse of dimensionality*. The number of states $n = |\mathcal{S}|$ grows exponentially with number of state variables. Here, even one full backup is too expensive.
    - In future lectures, we'll look at how to solve this problem - we'll do this by sampling particular trajectories (sampling just one trajectory in some instances). This means that:
        - Model-free: we don't need to have advance knowledge of the MDP
        - Breaks the curse of dimensionality, in fact the cost of backup is now constant and independent of $n = |\mathcal{S}|$.

**Contraction Mapping** wasn't covered in this lecture due to lack of time.