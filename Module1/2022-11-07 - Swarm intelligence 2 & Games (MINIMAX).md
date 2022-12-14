# HoneyBees 
HoneyBee colony algorithm (also called ABC). 
There are bees that change their role during the execution of the algorithm. 
There are 3 types of bees:
- <span style="color:red"><b>employed bees</b></span> that are <u>associated with a specific nectar source</u>.
- <span style="color:red"><b>onlookers</b></span> that by <b><i>observing</i> the employed bees chose a nectar source</n>. 
- <span style="color:red"><b>scouts</b></span> that <u>discover</u> new food sources.

Initially, food sources are discovered by scout bees. Then food is consumed and the source exhausted. The employed bees in that source become scout.

Using this structure, the food position represents the solution (there are as many solutions as scout bees), while the food quantity represents the fitness. 

The algorithm can be summed up with a structure like this:
```
InitializationPhase() 
repeat 
	EmployedBeePhase() 
	OnlookerBeePhase() 
	ScoutBeePhase() 
	Sol=BestSolutionSoFar 
Until (Cycle=MaxCycleNum or MaxCPUtime)
```

Just like ACO, this algorithm is incomplete and non-optimal. 

#### Analising the algorithm 
`InitializationPhase()` is the phase in which a ==set of food source positions are randomly selected by the bees== and their nectar amounts are determined.
Each solution $X_m$ (where $m=1 \dots N_{scouts}$) is componsed of variables $X_{m_i}$ ($i=1\dots n$). Each variable is subject to a lower and upper bound, and is initialized to:
$$
X_{m_i} = lb_i + rand(0,1) (ub_i - lb_i)
$$

The `EmployedBeePhase()` is the phase in which, after sharing the information about the nectar amount, ==every employed bee goes to the food source visited by her at the previous cycle== (since that food source exists in her memory), and then chooses a new food source by means of visual information ___in the neighbourhood___ of the present one. 

The <span style="color:limegreen">fitness funciton</span> of $X_m$ is computed using the objective function (___funzione obiettivo___) of the problem:
$$
ftn(X_m) = 
    \begin{cases}
      \dfrac{1}{1+obj(X_m)} & \text{if $obj(X_m) \ge 0$} \\ \\
     1+|obj(X_m)| & \text{if $obj(X_m) \lt 0$}
    \end{cases}\,
$$
We would swap the functions if we are maximizing (in this example we are minimizing). 

`OnlookerBeePhase()` : An onlooker bee chooses a food source depending on the probability value with that food source. 
$$
p_m = \dfrac{ftn(X_m)}{\sum^{n_{pop}}_{i=1} ftn(X_i) }
$$
It uses positive feedback. 

`ScoutPhase()` : in this phase, the employed bees are intensifying around a given solution, and scout bees are helping diversification. ==Scouts chose nectar sources at random==.
The <u>employed bees that cannot improve the solution</u> after a given number of attempts ==become scout and abandon the food source==. Because of this, it uses negative feedback.


Swarm intelligence algorithms have machine learning applications. 

# PSO: Particle Swarm Optimization
The observation of rules that guide the bird flock moves show that each individual entity has three driving trends:
- Follow neighbours 
- Stay in the flock 
- Avoid collisions

With these rules it is possible to describe and model the collective move of a flock with NO COMMON OBJECTIVE. PSO adds this common objective: which is food search.

With a common objective, a single individuals that finds a food source has two alternatives: -
- Move away from the group to reach the food (<span style="color:red">individualistic choice</span>) 
- Stay in the group (<span style="color:red">social choice</span>)

If more than one agent finds a food source, <u>other flock members do the same</u>, and gradually the whole group will change direction towards promising areas. 
The information propagates to all members.  

With PSO we can solve optimization problems with the following analogy: 
- __Individuals__: <span style="color:yellow">tentative configurations</span> that move and sample the solution $N$-dimensional space. 
- __Social interaction__: each individual agent <span style="color:yellow">takes advantage from other searches moving toward promising regions</span> (best solution globally found).
 
Search strategy can be found as a balance between exploration and exploitation: 
 - __Exploration__: individual agents that search for a solution. 
 - __Exploitation__: <u>social behaviour which is the exploitation of other individual successful behaviour</u>. N.B.: exploitation in the sense of _sfruttamento_. 

A feature that appears to be important is the concept of <span style="color:red"><b>proximity</b></span> 
- ==individuals are affected by the actions of other individuals that are closer to them== (<i>sub-groups</i>) 
- ==individuals are part of more sub-groups==, and then the __spread__ of information is globally guaranteed.
__Sub-groups__ are not tied to the physical proximity of the configurations in the parameter space <span style="color:yellow">but are a priori defined</span> and may take into account also considerable shifts between individuals.

#### The algorithm
PSO optimizes a problem by setting a population (Swarm) of __candidate solutions__ (particles). 
PSO moves these particles in the search space through simple mathematical formulas. 

The ==movement== of particles is guided by the best position found so far in search space (from individual to population) and it ==is updated when better solutions are discovered==.

$f: R^n \longrightarrow R$ fitness or cost function to minimize: it takes a solution (a vector) and produces a fitness value. The gradient of $f$ is not known (too ez otherwise). 
Goal: ==find solution $a$ such that $f(a) \le f(b)$ for all $b$ in search space== (aka a __global minima__). 

#### Structure of the algorithm

This is the initialization phase:
![[PSO_alg.png]]
$U()$ is the function that returns a random value. 

The actual body of the algorithm:
![[PSO_algo_2.png]]

### Parameter tuning
Each algorithm of swarm intelligence needs an activity of paramenter tuning, which essentially consist on trial and error. 
Automatic parameter tuning exists though. 

--------
# Games
Multi-agent environment that has to account for the presence of an opponent. 
__Game Theory__ is a branch of the economy.

When we have games we consider the games with the following properties:
- __two-player games__ (we call them MIN and MAX) in which the moves are alternated and players have <span style="color:limegreen"><b>complementary objective</b> functions (win and lose)</span>; 
- __games with perfect knowledge__ in which players have the same information (not true in card games such as poker, bridge, etc).

The <span style="color:yellow">development of a match can be interpreted <b>as a tree</b></span> in which the root is the starting position and leaves the final positions. So the solution is a search in this tree. 
![[game_tree_example.png]]

### MIN-MAX algorithm
The __minmax__ algorithm is designed to <u>determine the optimal strategy for <b><i>MAX</i></b></u> and to suggest, therefore, the first best move to be performed; 

- MIN is assumed to play at his best.
- ==We are not interested in the path but only in the next move==.

Each node of the tree is labelled with +1 or -1, where +1 is the victory direction of MAX and -1 is the victory of MIN. 
So, MAX wants to go the +1 nodes, while MIN will go to the -1 nodes.

![[minmax_tree_example.png]]

It is important to note that ==MAX moves for square nodes while MIN for circle nodes==. 
Let's examine the situation from the previous picture:

- Consider the node labelled `o`, in which MAX should move. 
	- The game is over in one move. 
	- MAX can move to `r` or `p` and lose, or he can move to `q` and win. 
	- Therefore `o` is a winning position for MAX and is labelled with a $(+1)$ 
- Consider the node `k`, in which MIN should move.  
	- Whatever move MIN does, MIN loses. So the label is $(+1)$. 
	- Consider the node `i`. ==MIN has a winning option== and the node is labeled with a $(-1)$.

In short, <u>a node where MAX has to move has a label equal to the maximum of the children labels</u> (of the previous nodes). The opposite happens for MIN. 

##### The MIN-MAX algorithm
![[min_max_algo.png]]
Let's improve this explanation with an actual algorithm:
```js
function eval_min_max(Tree t, Node n){
	t = t.ExpandTreeFromNode(n) //this means that we "create" a tree for a move n
	t.EvaluateLeaves() // establish which is winning and which is not

	n1 = t.get_unlabelled_node()
	while (n1 != null) {
		if (n1.depth % 2 == 0) { // node of MIN
			n1.label = min(n1.children.labels)
		} else { // node of MAX
			n1.label = max(n1.children.labels)
		}
		n1 = t.get_unlabelled_node()
	}
	if n1 == null return (n, t)
	else return (n1, t) // return the node, and the newly expanded tree
}
```

A more detailed explanation of the evaluation of a node $n$ in a game tree:
1. Put $n$ in $L$, a list of open nodes (not yet expanded nodes) 
2. Let $x$ be the first node in $L$. If $x = n$ and there is a value assigned to it, then return this value. 
3. Otherwise, if $x$ has an assigned value $V_x$, then $p$ is the father of $x$ and $V_p$ is a provisional value assigned to it. 
	- If $p$ is a MIN node, $V_p= min (V_p, V_x)$, 
	- otherwise $Vp = max (V_p, V_x)$. 
	- Remove $x$ from $L$ and return to step 2. 
4. ==If $x$ is not assigned any value and is a leaf node, assign it either a 1, or a -1, or a 0==. Put $x$ in $L$ because we need to update its ancestors and return to step 2. 
5. If x is not assigned any value, and is not a terminal node, assign $V_x = - ∞$ if $x$ is a MAX and $V_x = + ∞$ if it is a $min$. Add the children of $x$ to $L$ and return to step 2. 

#### MIN-MAX Properties 
- It's <span style="color:yellow">complete and optimal</span>.
- $O(B^m)$ in time, and $O(B\cdot m)$ in space, where $B$ is the branching value and $m$ is the maximum depth of the tree. So, it has the same space complexity as depth-first. 

### MIN-MAX Problems
A major problem of this approach is the number of nodes. A representation of complex games using MIN-MAX generates a tree with a very large number of nodes, and this makes it impossible to expand the tree efficiently. 
So, we have to <span style="color:red">prune the tree</span>.

##### Evaluation functions for tree pruning
We can use heuristic to solve this issue, and for example we could use a weighted sum of values (linear) for the _evaluation function_. 

For example, in chess \[not important btw\]:
![[chess_example_not_important.png]]

Using this method, in step 4 of the algorithm, if $x$ is not assigned any value and is a leaf node or ==you decide not to expand the tree further, assign the value to that node using the evaluation function $e(x)$==.

Here's the algorithm for MIN-MAX in pseudocode:
```js
function MinMax_decision(state) returns an action: 
	v = Max_Value(state)
	return action in Successors(state) with value v

function Max_Value(state) returns a utility value:
	if CUTOFF_TEST(state, depth) then return Eval(state)
	v = -inf
	for a, s in Successors(state) :
		v = Max(v, Min_Value(s))
	return v

// same as Max value with inverted operators  
function Min_Value(state) returns a utility value:
	if CUTOFF_TEST(state, depth) then return Eval(state)
	v = +inf
	for a, s in Successors(state) :
		v = Min(v, Max_Value(s))
	return v


//since min calls max and max calls min, the turn exchange is expressed.
```

Here's a better explained algorithm, found on [GeeksForGeeks](https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-3-tic-tac-toe-ai-finding-optimal-move/?ref=rp).
```python
function minimax(board, depth, isMaximizingPlayer):
    if current board state is a terminal state :
        return value of the board
    
    if isMaximizingPlayer :
        bestVal = -INFINITY 
        for each move in board :
            value = minimax(board, depth+1, false)
            bestVal = max( bestVal, value) 
        return bestVal

    else :
        bestVal = +INFINITY 
        for each move in board :
            value = minimax(board, depth+1, true)
            bestVal = min( bestVal, value) 
        return bestVal 
```

The problem now lies in the fact that we <u>do not know when to expand a node or not</u>.
If $e(n)$ was perfect, I would not have this problem. I could stop after the first level and evaluate the children of the root node. 

A simple way to solve this problem would be to always expand the nodes, and to stop only if the tree reaches a certain depth $p$. 
This approach, though, ignores more tactically complicated moves (in which $e(x)$ is highly variable). In these cases, <u>the search should stop when reaching a <b>quiescence</b> state</u>, in which  $e(x)$ values <span style="color:yellow">change slower</span>.
Another problem is the __horizon effect__: with useless moves, you should ==extend depth to values larger than $p$==, so that the basic moves are not really taken into account. 

In other words, you should extend the search only in promising nodes. 

# Alpha-Beta pruning
From what we have seen so far computers simply play all possible matches up to a certain depth, evaluate leaves and propagate back the evaluation. So they also consider moves and nodes that will never occur. 

What you should try is to reduce the search space. The best-known technique is that of the <span style="color:limegreen"><b>alpha-beta cut</b></span>.

Consider a node $N$ in the tree. Will the player move to that node? __If the player had a better choice (we call it $M$) in the parent node level or at any point along the path, then $N$ will never be selected.__ If we reach this conclusion, we can eliminate $N$. 

We call ALFA the value of the best choice found on the path for MAX (the highest) and BETA the value of the best choice found on the path for MIN (the lowest).
The algorithm updates ALPHA and BETA and cuts branches when their choice the worst.

In other words:
- **Alpha** is the best value that the **maximizer** currently can guarantee at that level or above.
- **Beta** is the best value that the **minimizer** currently can guarantee at that level or below.

Here's the pseudocode of MiniMax with Alpha Beta pruning optimization:
```js
function minimax(node, depth, isMaximizingPlayer, alpha, beta):

    if node is a leaf node :
        return value of the node // e(x)
    
    if isMaximizingPlayer :
        bestVal = -INFINITY 
        for each child node :
            value = minimax(node, depth+1, false, alpha, beta)
            bestVal = max( bestVal, value) 
            alpha = max( alpha, bestVal)
            if beta <= alpha:
                break
        return bestVal

    else :
        bestVal = +INFINITY 
        for each child node :
            value = minimax(node, depth+1, true, alpha, beta)
            bestVal = min( bestVal, value) 
            beta = min( beta, bestVal)
            if beta <= alpha:
                break
        return bestVal

```

Here's a simple explanation for the game:
- The left-most branch must ALWAYS be explored. 
- The MIN is assigned the minimum of the values in the MAX nodes, and viceversa. 
- if beta <= alpha, STOP immediately at any time, and go to the next branch. 
- At each leaf exploration, you must do the (beta <= alpha) test. 

The value `(beta <= alpha)` indicates that the MIN has an advantage to MAX (since ALPHA is the best value for MAX at that point and BETA is the best value for MIN at that point). 

#### MINIMAX Tree generation + AlphaBeta cuts algorithm 

Initialization of the algorithm
1. Put the node $n$ in $L$, which is a list of open nodes (not yet expanded nodes) 

Recursive part
2. Let $x$ be the first node in $L$. If $x = n$ and there is a value assigned to it, return this value. 
3. Otherwise, if $x$ has a value assigned $V_x$, let $p$ be the father of $x$. If $x$ has not an assigned value, go to step 5. 
	- We need to determine if $p$ and its children can be removed from the tree. If $p$ is a min node, $alpha$ is the __maximum of all current values__ assigned to the <span style="color:limegreen">brothers</span> of $p$ and of nodes that are <span style="color:limegreen">ancestors</span> of $p$. 
	- If there are no such values $alpha = - \infty$. 
	- If $V_x \le alpha$, then remove $p$ and all his descendants from $L$ (dually if $p$ is a max node).
	
4. If $p$ cannot be eliminated, let $V_p$ be its current value. If $p$ is a min node, $V_p= min (V_p, V_x)$, otherwise $V_p = max (V_p, V_x)$. Remove $x$ from $L$ and return to step 2. 
5. If $x$ is not assigned to any value and is a terminal node, or we decide not to further expand the tree, give it a value using the evaluation function $e(x)$. Leave $x$ in $L$ because you have to update the ancestors and return to step 2. 
6. If $x$ is not assigned to any value, and is not a terminal node, assign $V_x = -\infty$ if $x$ is a max and $V_x = +\infty$ if it is a min. Add the children of $x$ to $L$ and return to step 2.

_\[Honestly, I think the algorithm shown earlier is way better at explaining the concept than whatever the fuck this is...\]_

### Properties of AlphaBeta cuts
The best case scenario is when the best nodes are evaluated first, and the other are always cut (of course, it is entirely theoretical scenario). 
In this case, we can reduce the number of nodes (which is $b^d$) to about $b^{(d / 2)}$. (In practice, it is reduced by the square root branching factor, or you can look forward twice more at the same time). 
In the average case with random distribution of the node values, ==the number of nodes becomes about $b^{(3d / 4)}$==. So it is important to <span style="color:limegreen">choose a good order of the children of a node</span>. 

Also note that all the results mentioned herein are for an "ideal" game tree with depth and branching fixed for all the branches and nodes.
In many other examples, we have Repeated States, the list of closed nodes (see graph search).

Here's a representation of the best case scenario of Min Max search, in which all the best nodes are evaluated first:
![[minimax_best_case_scenario.png]]