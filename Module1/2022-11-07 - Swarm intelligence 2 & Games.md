# HoneyBees

HoneyBee colony algorithm (also called ABC). 
There are bees that change their role during the execution of the algorithm. 
There are 3 types of bees:
- __employed bees__ that are associated with a specific nectar source
- __onlookers__ that by observing the employed bees chose a nectar source 
- __scouts__ that discover new food sources

Initially, food sources are discovered by scout bees. Then food is consumed and the source exhausted. The employed bees in that source become scout.

Using this structure, the food position represents the solution, while the food quantity represents the fitness. 

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

#### Analising the algorithm 
`InitializationPhase()` is the phase in  which


The `EmployedBeePhase()` is the phase in which, after sharing the information about the nectar amount, goes to the food source area visited by herself at the previous cycle since that food source exists in her memory, and then chooses a new food source by means of visual information in the neighbourhood of the present one. 

The fitness funciton of $X_m$ is computed using the objective function of the problem:


`OnlookerBeePhase()` : An onlooker bee chooses a food source depending on the probability value with that food source. 
$$
p_m = \dfrac{}{\sum^{n_pop}_{i=1} ftn(X_i) }
$$
It uses positive feedback. 

`ScoutPhase()` : in this phase, the employed bees are intensifying around a given solution, and scout bees are helping diversification. 


Swarm intelligence algorithms have machine learning applications. 

## PSO: Particle Swarm Optimization
Agents starts moving around a food source, and the and the whole flock follows.
The interesting thing about this algorithm is that all particles make individualistic choices. 

Each agent avoids collisions, but stays in the flock and follows its neighbour. Normally, a particle system like this does not have a common objective. PSO adds this common objective: which is food search.

[..]

__Sub-groups__ are not tied to the physical proximity of the configurations in the parameter space but are __a priori defined__ and may take into account also considerable shifts between individuals.

#### The algorithm
PSO optimizes a problem by setting a population (Swarm) of candidate solutions (particles). 

PSO moves these particles in the search space through simple mathematical formulas. 

The movement of particles is guided by the best position found so far in search space (from individual to population) and it is updated when better solutions are discovered.

$f: R^n \longrightarrow R$ fitness or cost function to minimize: it takes a solution (a vector) and produces a fitness value. The gradient of $f$ is not known. 

Goal: find solution $a$ such that $f(a) \le f(b)$ for all $b$ in search space. 

#### Structure of the algorithm

This is the initialization phase:
![[PSO_alg.png]]

The actual body of the algorithm:
![[PSO_algo_2.png]]

### Parameter tuning
Each algorithm of swarm intelligence needs an activity of paramenter tuning, which essentially consist on trial and error. 

Automatic parameter tuning exists though. 


# Games
Multi-agent environment that has to account for the presence of an opponent. 
__Game Theory__ is a branch of the economy.

When we have games we consider the following subsets:
- __two-player games__ (we call them MIN and MAX) in which the moves are alternated and players have __complementary objective__ functions (win and lose); 
- __games with perfect knowledge__ in which players have the same information (not true in card games such as poker, bridge, etc).

The development of a match can be interpreted __as a tree__ in which the root is the starting position and leaves the final positions. So the solution is a search in this tree. 
![[game_tree_example.png]]

### MIN-MAX algorithm
The __minmax__ algorithm is designed to determine the optimal strategy for MAX and to suggest, therefore, the first best move to be performed; 

- MIN is assumed to play at his best.
- ==We are not interested in the path but only in the next move==.

Each node of the tree is labelled with +1 or -1, where +1 is the victory direction of MAX and -1 is the victory of MIN. 
So, MAX wants to go the +1 nodes, while MIN will go to the -1 nodes.

![[minmax_tree_example.png]]
##### The algorithm
![[min_max_algo.png]]

##### Properties
A major problem of this approach is the number of nodes.

- Complete? Yes (if the tree is finite) 
- Optimal? Yes (against an opponent who plays at his/her best) 
- Temporal complexity? O($b^m$) , where b is the branching factor and m is the number of nodes. 
- Space complexity? O(bm) (depth-first) 
- For chess, b ≈ 35, m ≈100 for "reasonable" games $\longrightarrow$ this solution is out of reach!!

To improve this algorithm, we could __prune the tree__.

We can use heuristic to solve this issue, and for example we could use a weighted sum of values (linear) for the _evaluation function_. 

##### Alpha beta cuts
From what we have seen so far computers simply play all possible matches up to a certain depth, evaluate leaves and propagate back the evaluation. So they also consider moves and nodes that will never occur. 

What you should try is to reduce the search space. The best-known technique is that of the __alpha-beta cut__.

Consider a node N in the tree. WIll the plauer move to that node? If the player had a better choice (we call it M) in the parent node level or at any point along the path, then N will never be selected. If we reach this conclusion we can eliminate N. 

We call ALFA the value of the best choice found on the path for MAX (the highest) and BETA the value of the best choice found on the path for MIN (the lowest).
The algorithm updates ALPHA and BETA and cuts branches when their choice the worst.

