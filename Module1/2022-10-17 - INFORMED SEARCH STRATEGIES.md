(__Lost the first 20 mins of this lesson__)

An Euristic should ALWAYS be optimistic, and not pessimistic, when predicting the solution. 

## Path search
Path search is a cool problem. 

### Best-first search
It is based on BFS (__BEST___ first search), which is a greedy algorithm. 
This, in turn, is based on Breadth-First Seatch, which is __not__ optimized. It doesn't garantuee that you'll find the optimal solution (that is, the shortest path). 
This means that also the evaluation function is not optimized.

Best-first search considers all of neighbours of each considered node, and uses the one with the least distance. 

Best-first is not optimal and incomplete, and it may risk to go down in a infinite loop if two cities have always the smallest distance. 

It isn't much optimized in space. 

### A* algorithm
A* is one of the most used heuristic algorithm used in AI.

Instead of only considering the distance to the goal, it also considers the __cost__ in reached the node $n$ from the root at each move.

$f(n)$ is the evaluation function of hte node, which is the same as Best-First search. 
($f(n) = g(n) + h'(n)$, where $g(n)$ is the depth of the node).

We combine the benefits of *depth-first* search (**efficiency**) with the ones of *uniform cost* search (**optimality** and **completeness**). 

Algorithm:
```
1. Let L be a list initial nodes of the problem. 

2. If L is empty fail; otherwise et n be the node for which g (n) + h'(n) is minimal. 

3. If n is the goal return solution (the path to reach it) 

4. Otherwise remove n from L and add to L all the children of n labeled with g(n) + h’(n). Return to step 2
```

This algorithm will not always find the shortest path, but there's a case in which it does. That is when the ==_heuristic function is optimistic_==.  Which means that if h'(n) \le h(n) for each node, then the A* always finds the optimal path. 
 

So, to calculate optimally a path (or the best path) you can use the __straight-line__ distance, so that you can evaluate 

The perfect heuristic h'=h is alwats feasible. If h'=0 we always obtmaint a feasible heuristic function. 

>[!THEOREM]
>If $h'(n) \le h (n)$ for each node, then the A* algorithm always finds the optimal path to the goal

#### Proof of A* optimality

- $f(G_2) = g(G_2),$ as $h(G_2) = 0$
- $g(G_2) \gt g(G)$, as $G_2$ us sub-optimal
- f(G2) \gt F(G), from above
- h'(n) \le h(n) as h' is feasible permissible
- g(n) + h'(n)

If it was pessimistic, we would have $g (n) + h '(n) \ge g (n) + h (n) = f (G)$ and then we could select $G_2$.