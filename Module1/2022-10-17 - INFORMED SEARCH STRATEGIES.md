## Intro
In the previous lesson, we used __uninformed search strategies__, in which the agent which does the search does not know anything about the problem at hand. 

Uninformed search methods in a (tree) search space of depth $d$ and branching factor $b$ have space complexity proportional $b^d$ to find a goal in one of the leaves.
This is unacceptable for certain complex problems, so we can resort to expanding the nodes using __heuristic domain knowledge__ (evaluation functions) to decide which node to expand first.

### Evaluation functions as a heuristic 
__Evaluation functions__ give a ==computational estimate of the effort to reach the final state==. 
The time spent to evaluate a node by means of a heuristic function should correspond to a reduction in the size of the space explored.
In Informed Search Strategies, there's a _trade-off_ between:
- Time it takes to solve the problem (<span style="color:red">baseline</span>). 
- Time spent in reasoning on top of it (<span style="color:red">meta-level</span>). 

Uninformed search strategies have no meta-level activities. 

Some of the problem related to this approach: 
- How to find the correct evaluation functions, i.e. how to select the most "promising nodes"?
- It is difficult to numerically characterize the empirical knowledge about the problem (aka, translating empirical knowledge into values). 
- Not always the obvious choice is the best:
![[heuristic fail example.png]]

N.B.: An Heuristic should ALWAYS be optimistic, and not pessimistic, when predicting the solution. 

## Best-First search
It's based on Breadth-First Search.
Best first search uses _evaluation functions_ that compute a number that represents the __desirability__ of the node expansion. Essentially, in the nodes' fringe that we have at a certain instant, ==we select the __most promising node of our fringe__==. 

The desirability could be, for example, from the physical distance from a goal. 

The Best-first search is an example of a <span style="color:limegreen"><b>greedy</b></span> algorithm.

Algorithm structure:
1. Let $L$ be a list of the initial nodes of the problem, ranked according to their distance from the goal (ascending order i.e. `priorityQueue` ); 
2. If $L$ is empty fail; otherwise let $n$ be the first node of $L$.
3. If $n$ is the goal, then return solution (the path to reach the goal).
	Otherwise, remove $n$ from $L$ and add to $L$ all the children of $n$. Then order the entire list $L$ based on an estimate of the relative distance from the goal. Return to step 2.

We can implement this search using the general search algorithm and by modifying the queuing function. QueuingFn = Insert successors in descending order of desirability.

#### Properties of Best-First Search 
The best-first search is <span style="color:red">not optimal</span> in the sense that it is not guaranteed to find the best solution, or the best path toward a solution (just like in the case of [[heuristic fail example.png|the maze]]).
This is because the best-first technique tries to find as soon as possible a node with 0 distance from the goal and not the node with the lowest depth.
![[bfs_fail_example.png]]
In this picture, the agent selects a longer path just because its starting node is the shortest.
The number in between the parenthesis is the evaluation function result. We must keep in mind that distance. 
In the case of the picture in the example, the evaluation function is __pessimistic__ since we are overestimating the effort for reaching the goal (aka the distance between nodes).  

Best-first search is not optimal and _may be incomplete_ (the same pitfall as the depth-first search, that is, if we have a cycle/loop). 

In the worst case, if the depth is $d$ and the branching factor $b$ the maximum number of expanded nodes will be $b^d$ (Time complexity).

In addition, it keeps in memory all nodes, the spatial complexity coincides with the temporal one. With a good heuristic function, <u>the spatial and temporal complexity can be reduced substantially</u>.

## A* algorithm
A* is one of the most used heuristic algorithm used in AI.

Instead of only considering the distance to the goal, it also considers the __cost__ in reached the node $n$ from the root at each move.

$f(n)$ is the evaluation function of the node, which is the same as Best-First search. 
($f(n) = g(n) + h'(n)$, where $g(n)$ is the depth of the node, or the cost of the edge).

We combine the benefits of [[2022-10-12 - Introduction to Search Strategies & Non-Informed Search Strategies#Depth-first search|depth-first search]] (**efficiency**) with the ones of [[2022-10-12 - Introduction to Search Strategies & Non-Informed Search Strategies#Uniform-cost search strategy|uniform cost search]] (**optimality** and **completeness**). 

Algorithm:
1. Let $L$ be a list initial nodes of the problem. 
2. If $L$ is empty then fail; otherwise let $n$ be the node for which $g (n) + h'(n)$ is minimal. 
3. If $n$ is the goal then return solution (the path to reach it) 
4. Otherwise remove $n$ from $L$ and add to $L$ all the children of $n$ labeled with $g(n) + h’(n)$. Return to step 2

This algorithm will not always find the shortest path (it is not optimal normally): it is <span style="color:red">optimal only</span> when the ==_heuristic function is optimistic_==.  

>[!INFO]- Important Theorem (feasibility)
>If $h'(n) \le h (n)$ for each node, then the A* algorithm always finds the optimal path to the goal

Let $h(n)$ be the <u>true distance</u> between the current node and the goal. A heuristic function $h'(n)$ is optimistic when $h'(n) \le h(n)$ is always true for each $n$. This heuristic function is said to be <span style="color:limegreen"><b>feasible</b></span>.
The perfect heuristic $h'=h$ is always feasible. If $h'=0$ we always obtain a feasible heuristic function (in this case, the Breadth-First search).

#### Proof of A* optimality
Suppose you have created a sub-optimal goal $G_2$ and to have it in the queue. Let $n$ be an unexpended node in the queue such that $n$ is on the shortest path to the optimal goal $G$.
![[visualization_a_star_proof.png]]

We need to prove that if $h'(n) \le h (n)$, A* is optimal (it always finds the optimal path). 
Let $g(n)$ be the true distance function. 

- $f(G_2) = g(G_2),$ as $h(G_2) = 0$ (since $G_2$ is a goal)
- $g(G_2) \gt g(G)$, as $G_2$ is sub-optimal
- $f(G_2) \gt F(G)$, from above
- $h'(n) \le h(n)$ as $h'$ is feasible permissible
- $g (n) + h '(n) \le g (n) + h (n) = f (G)$ 
- $f(n) ≤ f (G)$ 
Then $f(G_2) > f(n)$, and A* will never select $G_2$ for expansion.

If it was pessimistic, we would have $g (n) + h '(n) \ge g (n) + h (n) = f (G)$ and then we could select $G_2$.

## Eligible function heuristics
Here are some definitions of heuristics. 

Consider the tile game of 8. 
![[game_of_8_example.png]]
In this case we could use a number of heuristics, for example:
- Number of tiles that are out of place (in the picture, $h_1= 7$)
- The sum of the distances from the initial and final position of each tile. The distance is a sum of the horizontal and vertical distances (__Manhattan distance__). In the picture, the tiles 1 to 8 in the initial state have a distance of $h_2=2+3+3+2+4+2+0+2=18$.

Both of these possibile heuristics are egible, but $h_2$ is better since $h_1 <= h_2$. It is better to use a heuristic function with higher values, provided it is optimistic, since it's __closer to the real distance__.

#### How can we invent heuristics?
Often ==the cost of an exact solution of a relaxed problem is a good heuristic for the original problem==. If the problem definition is described in a formal language you can build relaxed problems automatically. 

Sometimes you can use the maximum among different heuristics, so that $h '(n) = max (h_1 (n), h_2(n) ... h_m(n))$.

Here's an example for a relaxed alternative problem (in this case, it's the game of 8). 
Normally, in the original game a tile can move from square A to square B if A is adjacent to B and B is empty. 

In general, relaxed problems remove some conditions. In the case of the game of 8, some relaxed conditions could be: 
- A tile can move from square A to square B if A is adjacent to B. (Manhattan distance) 
- A tile can move from square A to square B if B is empty. 
- A tile can move from square A to square B in one hop (tiles out of place).

## From Trees to Graphs 
We have assumed so far that the search space is a tree and not a graph. It is therefore not possible <u>to achieve the same node from different paths</u>.

This assumption is of course simplistic: think of the game of 8, the missionaries and cannibals etc. How to extend the previous algorithms for dealing with graphs?

In case we are dealing with graphs, the original search algorithm can be extended like this:
```js
function TREE_SEARCH(problem, fringe) -> solution or failure 
	closed = empty_set	
	fringe <- Insert(MakeNode(InitialState[Problem], fringe))
	loop do
		if fringe is empty {return failure} 
		node <- RemoveFront(fringe) //selects a node from the fringe
		if GoalTest[problem](State[node]) {return MakeSolution(Node)}
		if State[node] is not in closed {
			closed.insert(State[node])	
			fringe <- InsertAll(Expand(node, problem), fringe)
		}
```
Essentially, we just need to mark the nodes that we have already visited.

## Consistent Heuristics 
When using A* on graphs, though, you have to consider another condition in order to gain optimality: <span style="color:limegreen"><b>consistency</b></span> (or _monotonicity_).

>[!INFO]- Definition of consistency
>A heuristic is __consistent__ if, for each node $n$, for any successor $n’$ of $n$ generated by each action $a$, we have that:
>- $h(n) = 0$ if the corresponding status is the goal
>-  $h(n) \le c(n,a,n') + h (n')$ otherwise 
>
>Where $c()$ is the cost of going from to $n$ to $n'$ through the action $a$.
>

With monotonicity, we are guaranteed to find the shortest path from the root to the goal. 
We could see the consistency requirement as an evolution of the feasibility requirement from earlier. 
![[graph_cost_a_star_vis.png]]
##### Small proof of optimality through consistency 
if $h$ is consistent we have that: 
 - $f (n') = G (n') + h (n') = G (n) + c (n, a, n') + h (n')$ 
 - $G (n) + c (n, a, n') + h (n') ≥ g (n) + h (n)$
 - $g (n) + h (n) = f(n)$
 - then $f(n') \ge f(n)$. 

In this way, $f(n)$ never decreases along the path. 