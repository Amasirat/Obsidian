
One of the newest fields of engineering and science. The term was first coined in 1956.

**An agent** is anything that acts inside a system. It's basically a generic term for an AI entity whether it be a software or hardware+software one.

There have been discussion as to whether its better to model **humans** for developing AIs or the process of being **rational**.
## Being Human

The turing test was designed to test how human an agent **seems**. The test includes a judge that would ask questions of an unknown entity that it does not know. If it can correctly guess if this entity is an AI 30% of the time, then the AI has failed the test.

This test can not really help us in actually creating intelligent agents, because mimicking a human speech and fooling another human is not a correct objective of intelligence since masterful imitation could get us reasonably there. However it is important because despite all of that, passing this test will require solving quite a lot of complex problems.

* Natural Language Processing: Ability to parse and create correct sentences.
* Storing what it knows or hears
* Machine Learning
* automated reasoning, to reason about and answer questions given what it knows and draw new conclusions

In general, trying to emulate a human will not give us the best results, since humans are **flawed**.
That is why another suggested method of modeling AI is being developed, and that is what we study here.

A summary:

* **Human Thinking**: We don't know how our thoughts work, so the task is too complex
* **Human Behavior**: The task will not give us the best result since human behavior is **flawed**
* **Rational Thinking**: The laws of Boolean algebra first codified by Aristotle, however computations can be incredibly intensive, and not all informal information can be used in the same way.
* **Rational Behavior**: It includes rational thinking and is concerned with doing the **right** thing in every given situation **given the limitations of the scene**. It is a lot more general than thinking rationally.
**Rational Behavior** is what we'll be striving for in this study.
# Chapter 2: Intelligent Agents

Agents are beings that **percieve** and **Take Action** inside their environment. An agent can be software or a combination of software and hardware.

For example, take a vacuum cleaning agent:

* There are two or more rooms, each one can either be clean or dirty.
* The agent has four choices: **Go Left**, **Go Right**, **Suck**, **Do Nothing**
* Its perceptions are limited to the room it is currently in.
* It can not affect any location that it is not in.

But is this agent **rational**? How do we measure such a thing?

There are four factors that determine an agent's rationality:

 * Its **Performance Measure**
 * Its prior knowledge of the **Environment**
 * Its **Sensors** and its perception history
 * The **Actions** it takes given its perception history.

These factors are shortened to **PEAS**:
* **Performance**
* **Environment**
* **Actuators**
* **Sensors**
Given this, our definition of a **rational agent** will be:

***An agent that given its perception history, the set of actions it can take, and also its environment, performs a set of actions that maximize its performance measure***

An agent that is **autonomous**, tries to **Gather Information** on its own through its series of perceptions and takes actions rather than the programmer feeding it with information. **That's what seperates AI from other types of computer programs**

It is not required for an agent to be **All-knowing** to be rational, it must simply choose a **correct** choice given the information it currently has.
## Performance Measure

Designers of an agent, have to define a correct performance measure or an objective that an agent has to reach. **If a performance measure is not chosen correctly, the agent can be incentivized to perform actions that are not desirable.**

For example, if our performance measure of the vacuum cleaner agent were the amount of dirt it picked up, it would incentivize the agent to clean a floor, then push back all of that dirt and clean it again so that this performance measure is maximized. However if we choose the performance measure based on the cleanliness of the environment in the least time steps, then the agent will perform as expected.

*Generally, performance measure should usually involve the change we want in the environment as opposed to the agent itself.*
## Environment

Different types of environments:
* **Fully Observable** vs **Partially Observable**: If an agent has access to all the information it requires from its environment as opposed to **Partially Observable** where some information is hidden from it.

* **Single-Agent** vs **Multi-Agent**: An environemnt where an agent is alone as opposed to a **Multi-Agent** environment where an agent is either in **competition** or **cooperation** with another agent.

* **Deterministic** vs **Stochastic**: In an environment where each action has a determined consequence that can be predicted, as opposed to a **Stochastic** environment where a decision might have a **probability** of several consequences.

* **Episodic** vs **Sequential**: Decisions of the agent can be seperated into atomized units without the affect of its prior decisions as opposed to **sequential** where each decision leads to the next.

* **Static** vs **Dynamic**: While the agent deliberates (Thinks and tries to decide on an action), the environment does not change as opposed to **Dynamic** Environments. If however the change is something related to the agent itself, for instance a timer, that environment would be **Semi-Dynamic**.

* **Known** vs **Unknown**: An environment where the agent knows the rules and knows what would happen by its actions as opposed to **Unknown**. It is different from **deterministic** or **Stochastic** environments in that, even in stochastic environments, each action has a **probability** of a consequence but in **Unknown** environments, the action is *trully* unknown.

# Structure of Agents

There are four types of agent programs:

* **Simple-Reflex Agents**: Simplest kind of agent, it takes action based on its **Current perception** of its environment using a series of condition rules.
	* Simple but not intelligent
	* Can only do well in **Fully Observable Environments**
	* If its goal changes, it'll be useless (because its conditions and actions are hardwired and it can not learn new rules and environments)
* **Model-Based Reflex Agents**: Its a reflex agent with the difference that it **saves a summary of its experience** I.E **State**.
	* Can be used in **Partially-Observable** Environments
	* Records the state that it is in
* **Goal-Based Agents**: They are model-based agents that do actions to service a particular goal
* **Utitlity-Based Agents**: It focuses on making decisions that maximize a **Utility Function** that is derived from the **Performance Measure**
		For example, an agent that has the goal of reaching a destination, could have a utility function that makes it want to maximize its speed so that it can get there faster. If this **utility function** is simillar to the **Expected Performance Measure** then the agent is being **rational** and is working towards its own **happiness**.(Utility)

There's also a new type that has been worked on recently which is called **Learning Agents**.

A **Learning Agent** Contains:
* **Critic**: gives feedback to the agent
* **Performance Element**: The part of the agent that acts and percepts
* **Learning Element**: Takes in feedback from the critic and knowledge from Perception Element and results in it changing the performance element as well.
* **Problem Generator**: Encourages the agent to **explore** and gives it new problems to solve (Basically brings the agent out of its comfort zone)

![[2024-10-26_18-32.png]]

# Chapter 3: Searching Algorithms

A form of a goal-based agent, is a **problem-solving agent**.

In order for a problem-solving agent to solve a problem. Our problem needs to be well-defined (**Problem Formulation**) and its goals need to be clear for there to be no possible ambiguity (**Goal Formulation**).

**Problem Formulation**: Process of deciding what actions and states to consider.
**Graph Formulation**: The process of deciding what the next goal should be

**Search** is a process of finding a series of actions that satisfy the current goal. One that maximizes the performance measure will be an **Optimal Solution**.

Every problem requires 5 factors that need to be fleshed out:
* **Initial State**: What state the agent will start in
* **Possible Actions**: Given each state, what actions can the agent take
* **Transition Model**: What will each action do. Basically what new state will be the **Result** of an action
* **Goal Test**: Test if the current state is what the agent desires
* **Path Cost**: A penalty for its actions reflected by the performance measure.

A **solution** to a problem is a set of actions that takes the agent from the **initial state** to a **goal state**

There are however some problem types:
* **Fully-Observable**: The set of all states is given to the agent (**Single-State Problem**)
* **Not Observable**: (**Non-deterministic**)
* **Partially-Observable**: (**Contingency Plan**)
* **Unknown State Problem**

For a fully-observable problem, the solution will be merely a sequence of actions.
in partially-observable or non-deterministic problems, the solution will depend on new perceptions that the agent aquires.

For simplicity, let's only consider **Single-State Problems** for now.
## State Space Graphs

A mathematical representation of a search problem in the form of a **graph**.

Each state will be its **nodes** and Each action will be the **links**, with the resulting node being the **transition** or **Result**.

**We can not have duplicate states (nodes)!!!**

By **searching** through this graph, we can start building a **search tree**
## General Tree Search

```Psuedo-Code
function Tree-Search (Problem, Strategy) returns solution or failure:
	initialize search tree with initial state and update frontier with it
	loop do:
		if frontier is empty---->return failure
		choose a node and remove it from frontier
		check if the chosen node is the goal--->if true return solution
		Expand the chosen node and add its leaf nodes to the frontier
	endloop
```

Once searching has ended on somewhere it can't continue any longer, we need another node to start searching in. Therefore, we can store each node inside a data-structure called the **fringe** or **frontier**, and use it to get new nodes to explore. A fringe can be a FIFO data structure like queues or LIFO like Stacks or another type of data structure.
## Uninformed Search Algorithms

The agent has no additional information other than the information that is already given in each state.
### Breadth-First Search

A tree search method that visits all of the nodes in the same depth as each other in the tree. This is achieved by using a FIFO data structure for Frontier. 

d: depth of the shallowest goal state
m: depth of the tree
b: branching factor (*example: b = 2 for a binary tree*)

```Psuedo-Code
node = problem.initial_node
if Goal_test(node) == true return success
frontier = FIFO queue with node as first element
explored = ()
loop:
	if frontier is empty return failure
	node = pop(frontier)
	explored += node
	for each action in problem.Actions(node.State)
		child = action
		if child is not in explored or frontier:
			if goal_test(child) == true return success
			frontier.insert(child)
```

**Analysis of algorithm:**
* Complete: **Yes**, for **finite** number of branching factors
* Time Complexity: **O(b<sup>d</sup>)** because each node expands to branching factor number of nodes until it reaches shallowest goal state
* Space Complexity: We keep every node in memory so O(b<sup>d</sup>)
* Optimal: Only optimal when path cost is a **non-decreasing** function of depth.

***The memory is a bigger problem than its execution time***
### Uniform-Cost Search

Similar to BFS however it chooses the node with the **lowest path cost g(n)** instead of the shallowest node. 

Another difference is that goal test is applied when it is **selected for expansion**

a test is added at the end, in case a found node exists in frontier with lower path cost

```Pseudo-Code
node = initial state
frontier = priority queue by Path Cost with node
explored = empty set()
loop:
	if frontier empty return failure
	node = pop(frontier) // chooses lowest cost node 
	if goal_test(node) return solution
	explored += node
	foreach action in problem.actions(node):
		child = child_node(node)
		if child not in explored or frontier:
			frontier.insert(child)
		else if child is in frontier with higher path cost:
		replace node in frontier with child
```

**Analysis of algorithm:**
* Complete: **Yes**, for **finite** number of b
* Time Complexity: **O(b<sup>1+ C*/minimum pathcost of all actions</sup>)** 
* Space Complexity:  **O(b<sup>1+ C*/minimum pathcost of all actions</sup>)** 
* Optimal: **Yes**
### Depth-First Search

Always expands the deepest node in current frontier.

It uses a LIFO frontier data structure (I.E the Stack)

```psudocode
frontier += initial_state
loop:
	node = frontier.pop()
	foreach action in node:
		frontier.add(action)
	if goal_test(node) return success
	
	
```

* Optimal: No, it may not find closest goal state.
* Complete: If it's inifinite then hell no, if it's finite probably.
* Time Complexity: O(b<sup>m</sup>)
* Space Complexity: O(bm)

Its space complexity is the one that makes this algorithm popular.
#### Depth-Limited Search

We take a depth limit where we can't go past that limit.

It is complete if l >= d
#### Iterative deepening Depth-First Search

We increase the depth limit each iteration until we reach the goal state.
## Bidirectional Search

The idea is to run two simultaneous searches, one forward one backward. That way its time complexity will be b<sup>d/2</sup> + b<sup>d/2</sup> instead of b<sup>d</sup>.

The Bidirectional Search is difficult to use if the goal state is abstract.

## Comparisons

![[2025-01-16_12-22.png]]
## Informed Search Algorithms

Algortihms where some knowledge of where the goal is, is given to the agent. This knowledge is called the heuristic function, a function that estimates how close a state is to a goal.

**Heuristic functions are not problem-agnostic and have to be made specifically for an exact problem**

**Best First Search** is a general search tree method where we use the evaluation function (f(n)) to dictate which node to go to.

### Greedy Best First Search

f(n) = h(n) in this search algorithm, meaning the node that is closest to the goal is prioritized. Although this algorithm is neither complete or optimal even in finite state.

* Complete: No
* Time Complexity: O(b^m)
* Space Complexity: O(b^m)
* Optimality: Not really
## A\* Search

By far the best and most optimal search algorithm. the evaluation function is a combination of g(n) and h(n): f(n) = h(n) + g(n). It goes to the node that is closest to the goal AND the one that is currently the least costly as well.

**Uniform Cost Search is a special case of A\* search where h(n) = 0**

This algorithm will be optimal on the condition that h(n) satisfies a few condtions:

* **Admissability**: h(n) has to **not overestimate**.
* **Consistency**: h(n) has to be smaller than the path cost to n' and h(n'): $$h(n) <= C(n, a, n') + h(n')$$
If a heuristic function is consistent then it is also admissable. if consistency is broken, then of course h(n) overestimated the path. By the way, this is actually the **triangle equality**.
### Proof for A\*'s Optimality

We have to prove *if h(n) is consistent, then f(n) for any path from n to Goal node is non-decreasing*

Suppose that an arbitrary node n' is a successor of n, then g(n') = g(n) + C(n, a , n')
its g(n') is g(n) + the cost to reach n' from n.

	h(n) <= h(n') + C(n,a,n') ==+g(n)===== 
	=> g(n) + h(n) <= h(n') + g(n) + (g(n') - g(n)) ===
	===> g(n) + h(n) <= h(n') + g(n') ==> f(n) <= f(n')
	
	g(n') = g(n) +  C(n,a,n') => C(n,a,n') = g(n') - g(n)

Therefore, if a goal state is chosen, we can be assured that it is optimal since the farthest f(n) is the cheapest cost and no other f of any node can get any larger than this.

* Complete: Yes
* Time Complexity: b^(h\*-h)
* Space Complexity: O(b^m)
* Optimal: Yes
### Heuristic Function

In order to generate heuristic functions, we can **relax the problem**.

**Relaxing a problem** is the process of ignoring some limitations to a problem.

For the 8-puzzle: we can have a few different relaxations:

* Can move to B from A if A is adjacent (Ignore blank space requirement) = h1
* Can move to B from A if B is blank (Ignore adjacency requirement) = h2
* (Ignore both blank space and adjacency requirement) = h3

This way we can ensure the function is admissable.

* For h1(n) we can designate number of misplaced tiles => h(start) = 8
* For h2(n) we can designate manhattan distance from start to where a tile should be. 
=> h(start)=18

h2 is always greater than h1 therefore h2 **dominates** h1.
# Local Search

In classical search we were looking for a **sequence of actions** as an answer however in local search methods, we look for a **state** as an answer. We don't care how we reach there.

The process is **find initial state, do modifications and repeat until goal state reached**.

Its advantages include:

* Use less memory
* find resonable solution in infinite (very large) state spaces. : **Best Local Optimum**

Examples of problems where state itself is the goal:

* N-Queens
* Traveling Sales Person
* VLSI Layout
* Job-Shop Scheduling
* Automatic program generation

## Hill-Climbing Search

An algorithm that chooses the optimal successor each time. It's like climbing a hill, we choose the highest states to reach the top.

This algorithm is classified as greedy for obvious reasons. (Steepest Ascent)

```Code
hill-climbing(state) {
	node = state.initial;
	loop:
		neighbor = highest_successor(node);
		if(neighbor.value <= node.value) return node;
		node = neighbor;
}
```

* ***The problem is we can get stuck on a local optimum**.
* **If an initial state's immediate successors are not higher than our state, we can get stuck there.**

We can solve the infinite loop when confronted with flat local maxima like this:

* Sideways Moves: Keep going sideways if neighbors have the same value
* Random Restart: Restart state randomly
* Stochastic hill-climbing: Move to a successor with a particular probability. The **First Choice Algorithm** is one that implements this system. Generates successors randomly until a better successor is found. It's a good strategy when successors are huge for each state.

## Simulated Annealing

This method was used to solve VLSI Layout problems extensively.

The idea is to simulate the process of annealing in metalurgy in code. With high temperatures, allow a lot of bad moves but with an exponential probability, make the bad moves less frequent once temperature has been cooled enough.

A schedule array stores the temperatures that we designate for the problem to be solved.

```Code
Simulated_annealing(Problem, schedule)
{
	current = problem.init;
	from t = 0 until infinity do:
		T = schedule[t];
		if T = 0 return current;
		next = randomly selected successor of current;
		deltaE = next.value - current.value;
		if deltaE > 0 then current = next;
		else current = next only with probability e^(deltaE / T);
}
```

P(S, S', t) = a \* :
* if E(S') > E(S) ====> 1
* otherwise ======> e^(E(S') - E(S) / T(t))

**The probability of a bad move decreases the smaller deltaE = E(S') - E(S) becomes.**

If T decreases slowly enough, with a probability of one is guranteed to reach a global optimum.
## Local Beam Search

Instead of just one state, have k states and do a hill-climbing search for the solution simultaneously for all k states. (Multi-threading)

**The problem with this method is that it eventually is concentrated in a small region after iterations**

Solution is **Stochastic Beam Search**, choose the successors with a probability.
## Genetic Algorithms

Another form of stochastic search algorithm inspired by ideas from biology and natural selection.

Successors can be generated by combining two parent states instead of modifying a single state.

In this method a state is represented as a string (Similar to a chromosome) and needs to be formulated based on the nature of the problem at hand.

* Start with k randomly generated states (Population)
* Evaluation Function that evaluates states (Fitness Function)
* Combine two parent states and get offsprings (Cross-over)
* Change one string from a string with a particular probability (Mutation)
* Next generation of states are chosen (Selection)

All of these steps can be constructed and is unique to a single problem.
![[genetic.png]]

Example for 8 queens:

Each number inside this chromosome is the position of a piece on a column.

state 1: {2, 4, 7, 4, 8, 5, 5, 2}
state 2: {7, 5, 2, 5, 1, 4, 4, 7}

The Fitness Function, evaluates which states should basically have sex! The fitness function here is the number of non-conflicting queens.

$$reproductionRate = fitness(i) / \sum_{}fitness$$

Some random chromosomes from the population are then picked and the ones with the highest fitness function are the winners and are chosen.

We choose a cut off point and merge the numbers before the cut off point from the first state to the numbers after the cut off point in the second state.

{2 ,4, 7 ||| 4, 8, 5, 5, 2} + {7, 5, 2 ||| 5, 1, 4, 4, 7} = {2, 4, 7, 5, 1, 4, 4, 7}

After this, if selection probability function succeeds, one number is changed.

{2, 4, 7, 5, 1, 4->3, 4, 7}: Changed 4 to 3.
## Local Search vs Systematic Search

![[2025-01-11_12-32.png]]
# Constraint Satisfaction Problems

It's a way of formulating problems, in a way that can limit the search space for state that do not fit the constraint defined. The search algorithms for these problems can use **general purpose** heuristics rather than problem-specific ones we tackled last time.

A CSP has three components:

* X: a set of variables
* D: a set of domains
* C: a set of constraints (allowable combination values)

Example: N-Queens

We can formulate this problem in two ways:

### Formulation 1:

* Variables: Xij ====> Places on the chess board
* Domain: {0,1} ====> Whether a piece is there or not
* Constraints:
	* Implicit: $$\sum_{i,j} Xij = N$$
	* Explicit: 
		* (Xij, Xik) = {(0,0), (0,1), (1,0)}}
		* ...

This formulation is problematic because the chosen variable makes it so that the variables are huge, therefore Formulation 2 is an improvement in a way.

### Formulation 2: 

* Variables: {Q1, Q2, ....., Qn} ==> The queen pieces
* Domains: {1, 2, ..., N}: The number of places on chess board
* Constraints:
	* Implicit: j != i the pieces should be non-threatening.
	* Explicit: (Qi, Qj) = {(1,3), (1,4), ...}

CSPs are used fundamentally because once a partial solution to the problem is discovered, beause of the defined constraints, a lot of states are eliminated. For example if the first queen is placed on i = 1, then we are certain j = 1 is not an answer and it will be completely out of the consideration.

We can represent CSP problems as graphs.

Some Notes:
* **Binary CSP**: Each constraint relates two varaibles
* **Inside Binary Constraint Graph** nodes are variables and arcs show constraints.
* General purpose algorithms use a graph structure to speed up searches.

By enforcing local consistency, it can cause inconsistent values to be eliminated.

* Consistent: An assignment that does not violate any constraint
* Complete: All varaibles are assigned.

## Types of Constraints

There are many types of constraints:
* Unary Constraint: Where it restricts one single variable from one domain. e.g SA != green
* Binary Constraint: relates two variables. e.g SA != NSW
* Trenary Constraint: relates three variables together.
* Global Constraint: involving an arbitrary number of variables. A common global constraint is *Alldiff* which means all variables involved must have different values.
* Preferences (Soft-Constraints): In real world examples, some constraints may not be deal breakers but some are more prefered than others, for example for a class scheduling problem, a professor may prefer to teach in the mornings however afternoon is still a correct solution.
These problems with preferences are **Constrained Optimization Problems**

In reqular state-space search, an algorithm can only search but in CSPs, an algorithm can either search or do a type of inference called **Constraint Propagation**

It's basically the act of reducing the amount of possible values using the constraints provided, which is called **local consistency**.

Different types of local consistency:
* Node Consistency: When all values in the domain satisfy a node's **unary constraints**. We say a network is consistent if this fact is true for it.
* Arc Consistency: When all values in the domain of two variables is consistent. for example for y = x^2, if Dx = {0, 1, 2, 3} then Dy has to be {0, 1, 4, 9}
* Path Consistency: When two variables are consistent with each other and in addition to a third variable in between them.
* K-consistency: Stronger form of propagation. for any set of k - 1 variables, and for any consistent assignment to those variables, a consistent value can always be assigned to the kth variable. 2-consistency is the same as Arc consistency.

```AC-3
function AC-3(csp) returns false if inconsistency is found
{
	queue = Get_Arcs(csp)
	while queue is not empty:
		(Xi, Xj) = Remove-First(queue)
		if Revise(csp, Xi, Xj) then:
			if size of Di = 0 return false
			for each Xk in Xi.Neighbors - Xj do:
				add(Xk, Xi) to queue
	return true
}

function Revise(csp, Xi, Xj) returns false if it did not revise the Domains of any varaible
{
	revised = false;
	for each x in Di do:
		if no value y in Dj allows (x,y) to satisfy (Xi,Xj)'s constraint:
			delete x from Di
			revised = true

	return revised
}
```

Some limitations of Arc Consistency include:
* Can have one solution left
* Can have multiple solutions left
* Can have no solutions left
## Solving CSPs

### Backtracking Search

The most basic uninformed algorithm for solving CSPs.

***It is basically a DFS algorithm with two improvements**

* **Check one variable at a time**: Since assignments are commutative, only consider assignments to one variable at a time.

* **Check Constraints as you go**: Only take into account variables that are allowed by the constraints

```PsuedoCode
function Backtrack(assignment, csp)
{
	if assignment complete then return assignment
	var = Select_Unassigned_Variable(csp)
	for each value in Order_Domain_Values(var, assignment, csp) do:
		add { var = value } to assignment
		inferences = Inference(csp, var, value)
		if inferences != failure then
			add inferences to assignment
			result = Backtrack(assignment, csp)
			if result != failure then
				return result
		remove { var = value } and inferences from assignment
	return failure
}
```

The function ***Inferences*** can optionally be used to enforce arc, path or k-consistency. If a value choice leads to failure, then value assignments are removed from assignment.

The straightforward way of implementing `Select_Unassigned_Variable()` and `Order_Domain_Values()` is to just select the next unassigned variable and Domain Value in order. However that is not efficient most of the time. 

There are two general methods of ordering, meaning in which order to select variables:
* **Minimum Remaining Value(MRV)**: Choose the variable that is the most constrained from its surroundings and therefore the least amount of options. For example if inside a map, we have painted a region's neighbors with red and blue, the region doesn't have any other choice but being green, therefore it is prefered to assign that straightaway so that we don't run into failure in a later step and have to backtrack. (Choosing Variable)
* **Least Constraining Value(LCV):** Once a variable has been selected, we have to decide which domain value to select, so we can select the one that has the least amount of constrains, the one that rules out the fewest options basically. (Choosing Domain Values) 
## Forward checking

It's the process of filtering the possible domain values after an assignment. A simple method is to remove a domain value from a variable's neighbors (given by the constraint) that it was assigned itself. 

The limitation of this is the fact that you can not know more than the step you already. If there is for example one domain value left in two neighbors where if one is chosen the other will become inconsistent, this can not be detected until the variable is already assigned. That's why Arc consistency works better in that regard although it is more expensive to compute.