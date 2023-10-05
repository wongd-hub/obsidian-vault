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

- **Optimal substructure** - i.e. the principle of optimality applies - the optimal solution can be decomposed into subproblems
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
> Discussing the right hand column now, this tells us that even though we're evaluating a random policy, we can use this information to compute a better policy/value function. 
> - As we evaluate values at each state, we can see which state provides the highest value, and therefore which states are best to move to (i.e. the best policy occurs when we start to act greedily from the state we're in).
> - At $k = 0$, we can see that all states have all 4 arrows as 'best' options. At this point, all states have 0 value, so all states are equivalently valued/tied.


 

