> [!INFO]- Resources of this lesson
> Slides [here](https://virtuale.unibo.it/pluginfile.php/1352444/mod_resource/content/1/4-StriRicNonInf.it.en.pdf).

Many AI problems can be solved by exploring the so-called __solution space__. It contains all possible sequences of actions that might be applied by an agent. Some of these sequences lead to a solution.

The agent examines an alternative sequence of actions that lead to known states and chooses, then, the best one. ==The process of trying this sequence is called SEARCH==. 
It is useful to think about the search process like building a search tree whose nodes are states and whose branches are operators/actions.

### What is a search algorithm?
A search algorithm takes as input a problem and returns a solution in the form of a __sequence of actions__. 
Once the solution is found, the suggested actions can be performed (EXECUTION).

### <span style="color:limegreen">Notions and terms</span> in search strategies
A search algorithm generates a sequence of actions. It does so through these steps:
- ___Expansion___: one starts from a state, and applying the operators (or successor function) will generate new states. 
- ___Search strategy___: at each step, choose which state to expand. 
- ___Search tree___: It represents the expansion of all states starting from the initial state (the root of the tree).

The leaves of the tree represent either:
- states to expand
- solutions 
- dead-ends.

### Generating SEARCH TREES
A tree generation is an offline operation (meaning that we take a initial snapshot of the situation and find a solution based on that). Exploration obtained by expanding states that have been already explored.

Here's the general algorithm for generating a search-tree:
```bash
function TREE-SEARCH (problem, strategy) -> returns solution or failure
	initilize the search tree using the initial state of problem 
	while do 
		- if there are no candidates for expansion then 
			return failure 
		- choose a leaf node for expansion according to strategy 
		- if node contains a goal state then 
			return the corresponding solution 
		- else expand the node and add the resulting nodes to the search tree
```
 An example for a problem that can be formulated and the solved using a tree search:
 ![[problem_tree_search_example.png]]

### Problem formulation
How can we formulate the problem such that it can be solved by using search trees?

We have to keep in mind that a problem is defined by _four points_:
1. <span style="color:red">Initial state</span> (in the previous example, "at Arad")
2. <span style="color:red">Successor functions </span>$S(x)$, that is a ==set of action-state pairs==. 
	- e.g., S (Arad) = {\<Arad -> Zerind, Zerind\>, ... }
3. A <span style="color:red"><u>test goal</u></span>, could be:
	- explicit, E.g., `x = "At Bucharest"`
	- implicit, E.g., `checkmap(x)`
4. <span style="color:red">Cost of the path</span> e.g., distance travelled, the number of actions (hops) performed etc.
	- For example: $c (x, a, y) ≥ 0$ 

A solution is a sequence of actions that lead from the initial state to the goal.

### What is a Node in a search tree?
A node in the search tree corresponds to a data structure, which contains:
- The <u>state</u>.
- The <u>parent</u> node.
- The <u>operator</u> which has been applied to obtain the node. 
- The <u>depth</u> of the node.
- The <u>(total) cost of the path</u> from the initial state to the node.

### Implementation of the TREE-SEARCH 
This algorithm represents the general train of thought process of a typical search algorithm. 
```js
function TREE_SEARCH(problem, fringe) -> solution or failure 
	fringe <- Insert(MakeNode(InitialState[Problem], fringe))
	loop do
		if fringe is empty {return failure} 
		node <- RemoveFront(fringe) //selects a node from the fringe
		if GoalTest[problem](State[node]) {return MakeSolution(Node)}
		fringe <- InsertAll(Expand(node, problem), fringe)
```

A function that expands the nodes into the fringe (to __create a search tree__) could look something like this:
```js
function Expend(node, problem) -> returns a set of nodes 
	successor = [] 
	for each action,result in Successor_Fn[problem](State[node]) do 
		s = new Node()
		//initiating node fields
		ParentNode[s] = node  
		Action[s] = action 
		State[s] = result  
		PathCost[s] = PathCost[node] + Step_Cost(node, action, s)
		Depth[s] = Depth[node] + 1
		//add new node to its successors
		successors.insert(s)
	
	return successors 
		
		
``` 

Essentially, we are creating a tree while we are searching the solution

#### How can we evaluate the <span style="color:red">quality of a search strategy</span>?
[[2022-10-12 - Introduction to Search Strategies & Non-Informed Search Strategies#<span style="color:yellow;background-color:blue">Search strategies</span>|A more detailed explanation here]]
When we consider a search strategy, we have to ask a set of questions:
- Can we find a solution?
- Is it a good solution?
	- on-line cost: minimum path solution (does it give us the minimum path?)
	- solution cost
- What is the cost of the search process?
	- off-line cost: time to find a solution
	- computational cost

We could say that the total cost of the search strategy can we computed as:
$$
total \ cost = obtained\_path\_cost + search\_cost
$$

##### Game of 8 example 
An example regarding the process of the computation when finding a strategy can be found in the slides @ 13-14 of this lesson. 

## <span style="color:yellow;background-color:blue">Search strategies</span> 
The choice on <span style="color:limegreen">how to expand a search tree</span> is called <span style="color:cyan"><b>strategy</b></span>. At each node more actions may be performed (action: operators of applications). 


We have two possible types of strategies:
- __Non-informed__: Do not use any domain knowledge: apply rules arbitrarily and do an <span style="color:yellow"><i>exhaustive</i></span> search strategy.
	- These are _impractical_ for some complex problems.
- __Informed__: Use domain knowledge: apply rules following a heuristic. 
	- If we had a perfect heuristic, you would not need to do a search.

The Strategies are __evaluated__ according to four criteria: 
- <span style="color:red">Completeness</span>: does the strategy guarantee to find a solution if one exists? 
- <span style="color:red">Time complexity</span>: how long does it take to find a solution?
- <span style="color:red">Space complexity</span>: how much memory is needed to carry out the search? 
- <span style="color:red">Optimality</span>: does the strategy find the best solution when there are more solutions?

# Non-informed search strategies 
Some non-informed search strategies algorithm: 
- breadth-first (at a uniform cost)
- depth-first 
	- depth-first limited depth-first 
- iterative deepening.

## Breadth-first search 
Breadth-first search is a search algorithm in which we choose to expand the __LESS DEEP__ (not less costly) tree nodes.

> [!INFO]- Depth definition
>The depth of the root node is equal to 0; >
> the depth of any other node is the depth of its parent plus 1

So we expand the nodes at the lowest depth first. So, the _fringe_ is essentially a __FIFO queue__.

### Properties of the BFS algorithm
In the worst case, if depth is $d$ and the branching factor is $b$ then the maximum number of nodes expanded in the worst case will be $b^d$. So, time and space complexity is always $b^d$ in the worst case. 

This strategy ensures <span style="color:red">completeness</span>, but we don’t have EFFICIENT IMPLEMENTATIONS on single-processor systems (multiprocessor architectures).
The most serious problem, though, is the <span style="background-color:red;color:black">excessive memory footprint</span>. 

==Breadth-first search always finds the min-cost path if the cost equals the depth== (so it's __OPTIMAL__ when cost=1 for each step). Otherwise, we should use another strategy that always expands the least cost node -> [[#Uniform-cost search strategy|uniform cost strategy]] (Bellman-Ford algorithm). 

When encountering this algorithm, usually the notations symbols stand for:
- $b$ - maximum of the search tree branching 
- $d$ - depth of the least-cost solution
- $m$ - maximum depth of the search (can be infinite)

In the BFS, the fringe function (or queuing function) consists on adding the nodes at the end of a queue. 

## Uniform-cost search strategy
In the uniform-cost search strategy, each edge of the tree (or node) is labelled with a cost $g(n)$.  

The uniform-cost strategy is comprehensive and, unlike the breadth-first search, ideal when operators have not uniform costs. (Same temporal and spatial complexity as breadth-first search).

In this algorithm, the queuing function consists on ==entering the successors in order of __increasing__ path cost==.

## Depth-first search
Depth-first expands <span style="color:red"><b>DEEPEST</b></span> nodes first. Nodes at equal depth are arbitrarily selected (leftmost). In this sense, the _fringe_ function can be seen as a LIFO stack.  

#### Properties of DFS
Contrary to BFS, DFS has a <span style="color:red">modest memory occupation</span>. For a state space with branching factor $b$ and maximum search depth $d$ we have to store $b \cdot d$ nodes (linear).
The temporal complexity $b^m$, where $m$ is the maximum depth of the tree.

Depth-first search is efficient from an implementation (memory) point of view, since one path at a time is stored (a single stack). 
Nevertheless, ==depth-first search can be NON-COMPLETE== with possible loops in the presence of infinite branches (OR LOOPS).

## Limited depth search 
The limited depth search, It is a depth-first variant, with <u>the inclusion of a MAXIMUM DEPTH parameter</u>. When you reach the maximum depth or a failure, it explores alternative paths (if they exist), then <u>alternative paths at less than one unit of depth</u>, and so forth (<span style="color:red">backtracking</span>). 

You may establish a maximum limit of depth (it does not necessarily solve the problem of completeness). Avoids infinite branches. 

## Iterative deepening 
Iterative deepening search avoids the problem of choosing the maximum depth limit by trying all possible depth limits. 
- Start with 0, then 1, then 2 etc ...

It combines the advantages of depth and breadth-first strategies. It is <span style="color:red">complete</span> and explores a __single branch__ at a time. 
Many states are expanded multiple times, ==but this does not worsen considerably the execution time==. 
In general it is the ==favorite search strategy when the search space is very large==.

```js
function iterative_deepening_search(problem) -> solution, fail 
	for depth = 0 to infinity do
		result = limited_depth_search(problem, depth)
		if result != cutoff {return result} 
```

#### Properties of Iterative deepening 
- it is __complete__.
- Time $O(b^d)$, Space $O(bd)$
- It is optimal if step cost is 1. 

It can emulate the breadth-first search through repeated applications of the depth first search with an increasing depth limit.

### Comparisons of non-informed search strategies

![[non-informed_comparison.png]]
Where:
- $b$ = branching factor 
- $d$ = depth of the solution 
- $m$ = maximum depth of the search tree
- $l$ = depth limit

