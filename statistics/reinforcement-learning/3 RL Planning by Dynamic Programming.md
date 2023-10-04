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

- 13:02
