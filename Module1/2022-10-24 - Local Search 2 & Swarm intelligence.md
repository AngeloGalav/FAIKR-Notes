### Some additional notes regarding Graph Search
As we saw [[2022-10-17 - INFORMED SEARCH STRATEGIES#Consistent Heuristics|in informed strategies using A*]], it's very important that the heuristic must be consistent (when using graph search).  

--------
# Meta-Heuristic
As we saw [[2022-10-19 - Local Search 2 & Swarm intelligence|in the last lesson]], while local search is fast, it is __not optimal__ and must be improved upon. We can try to visualize the local search as a search process over a graph: it ==starts from an _initial node_ and explores the graph moving from a node to one of its neighbors, until it reaches a termination condition==.  

In this case, we need to consider two graphs:
1. __Neighborhood graph__ to represent <u>search space topology</u>.
2. __Search graph__ to represent the <u>actual search space exploration</u>.

Here's an example:
![[meta_graphs_example.png]]
N.B.: in this example, we also see how a neighbor function can effectively be defined from an initial graph. 

## Simulated annealing
A popular algorithm in meta heuristics. 
It allows moves resulting in solutions of worse quality than the current solution, and the probability of doing such a move is decreased during the search.

It changes local search by doing ___worsening moves___, and each move has associated a probability to it. The probability of each move then decreases.
So that the probability of accepting a worsening move decreases when you proceed: at the beginning of the search, it is very probable that we would choose a worsening move, and the probability of choosing a worsening move decreases as we proceed in the search. 

The decrease of this probability is _exponential_.
The algorithm for this method is:
```js
s = GenerateInitialSolution()
T = T0 // temperature
while termination conditions not met do 
	sʹ = PickAtRandom(N (s)) 
	if f(sʹ) > f(s) then  // in this case, the solution is improved 
		s = sʹ    // sʹ replaces s
	else // if it doesn't improve, we might choose the other solution with a 
         // probability != 0
		Accept sʹ as new solution with probability p(T , sʹ , s) 
	end if 
Update(T) // update the temperature, which means that the probability w which 
		  // we will accept a worsening move will be lower
end while
```

The temperature can be computed in different ways:
- Logarithmic: $T_{k+1} = Γ / log(k + k_0)$
	- The algorithm is guaranteed to converge to the optimal solution with probability 1. Too slow for applications.
- Geometric: $T_{k+1} = αT_k$ , where $α ∈ \ ]0, 1[$
- Non-monotonic: the temperature is decreased (intensifications is favored), then increased again (<u>to increase diversification</u>). 

In the end of the execution, the probability a reaching a worsening move is almost 0. 

## TABU Search
Since local search is completely blind, __TABU Search__ tries instead to remember something about the past search, so that we have ==the possibility of not exploring the same states again and again without improvement==. 
It keeps tracks of moves and states, which are seen as "taboo" (to be avoided). This information are kept in a _taboo list_. This way, <u>we can escape from local minima and cycling</u>. 

In this way, cycles are also avoided. 
```js
s = GenerateInitialSolution()

TabuList = ∅ 

while termination conditions not met do
	s ← BestOf(N (s) \ TabuList ) // select a state from the Neighs and avoid the 
                                  // nodes in the TabuList 
	Update(TabuList ) 
end while
```

Small variation to the normal local search algorithm, but very effective. 

Since states are inefficient to memorize, usually *__moves__* are stored instead.
By saving moves instead of solutoins, though, we could avoid some solution that were not yet visited. Because of this, we use a <span style="color:limegreen"><b>ASPIRATION CRITERIA</b></span>, which essentially <u>goes through a forbidden move if the solution we obtain is better than the current one</u> (this can only be applied to the taboo list is made of moves and not states).

## ITERATED LOCAL SEARCH
This search acts on the neighbourhood. This local search has some perturbation steps in order to escape from the local optima. 
It has composed of two steps:
- Uses subsidiary local search steps (exploring the current solution), to find a local optima very quickly (__intensification__).
- Uses perturbation for effectively esaping local optima quickly (__diversification__).

```js
s0 ← GenerateInitialSolution() 
s∗ ← LocalSearch(s0) // finds local optima asap

while termination conditions not met do
	sʹ ← Perturbation(s∗, history) 
	s∗ʹ ← LocalSearch(sʹ) // re-run local search
	s∗ ← ApplyAcceptanceCriterion(s∗,s∗ʹ,history)
end while
```

\[by perturbing ==we mean jumping somewhere else== in the search space\]

This algorithm uses _high level search strategies_, which are _intensification_ and _diversification_,  which have to be applied to effectively explore the search space.

### Thoughts on local search 
- Modeling is an issue (You don't need to find a model of your problem, in general you're just exploring solutions). 
- High level search strategies have to be applied to effectively explore the search space (intensification/diversification) 
- Importance of search history 
- Parameter tuning often critical (not so much in simple search strategies). 


# Population based meta-heuristics

This methods uses a set of states, a population of states (instead of starting from a single state), and we apply some concepts found in nature (i.e. evolution, swarms etc).

The idea is this: if a solution with a component that is good and one that is not, I can merge this solutions in order to have the best of both worlds and have a good enough solution.

## Genetic algorithms
These algorithms are inspired by evolution. They have 3 key components:
- __Adaptation__: organisms are suited to their habitats.
- __Inheritance__: offspring resemble their parents.
- __Natural selection__: new, adapted types of organisms emerge and those that fail to change adequately are subject to extinction.

==The _fittest_ individuals have higher chance of the generating an offspring== (survival of the fittest).  The children are similar, but not equal, to their parents, and the traits characterizing the fittest individuals spread across population, generation by generation.

We GAs, __we are not simulating evolution__, but we are exploiting these key concepts for problem solving. 

#### Evolutionary cycle
- You generate the population
- You generate recombination, through parents. By recombination, we mean the genetic material of the parents. 
- _Mutation_: introduces variability in the genotypes.
- _Selection_: acts in the choice of the parents whose genetic material is then reproduced with variation. 
- _Replacement/insertion_: defines the new population from the new and the old one. 
![[evolutionary_cycle.png]]

#### Terminology
- A population is the set of individuals (solutions) 
- Individuals are also called genotypes 
- Chromosomes are made of units called genes (could be integers number) 
- The domain of values of a gene is composed of alleles (e.g., binary variable ↔ gene with two alleles)

#### GENETIC OPERATORS
- <span style="color:limegreen">Recombination or Crossover</span>: cross-combination of two chromosomes (loosely resembling biological crossover). Essentially, a part is taken from the first parent and another from the second parent. 
![[recombination_example.png]]
- <span style="color:limegreen">Mutation</span>: we modify a gene with a probability of _pM_, in order to introduce variability in the genotypes. 
![[mutation_example.png]]
- <span style="color:limegreen">Proportional selection</span>: the probability for an individual to be chosen is proportional to its fitness. Usually represented as a roulette wheel. 
![[gira_la_ruota_yea_yea.png]]
- <span style="color:limegreen">Generational replacement</span>: the new generation replaces entirely the old one. 
	- Advantage: very simple, computationally not expensive, easier theoretical analysis.
	- Disadvantage: <u>it might be that good solutions are not maintained in the new population</u>.

Alternatively, one can keep the best $n$ individuals from the old and the new population (or choose any variant of this scheme).

Here are some mathematical explanation of the operators:
![[genetic_operations_maths.png]]
Depending on the tuning of the parameters, the quality of the solution changes. 

## Genetic Algorithms
```
Initialize Population 
Evaluate Population 

while Termination conditions not met do 
	while New population not completed do 
		Select two parents for mating 
		Apply crossover 
		Apply mutation to each new individual 
	end while 
Population ← New population 
Evaluate Population 
end while
```

N.B: The algorithm is __NOT optimal nor complete__ . It is not guaranteed to find a solution. 

The termination conditions could be:
- Execution time limit reached. 
- Satisfactory solution(s) have been obtained. 
- __Stagnation__ (limit case: the population converged to the same individual)

### Thoughts on GAs
While crossover combines good parts from good solutions, <u>it might achieve the opposite effect</u>. 
Like we said before:
- Mutation introduces diversity. 
- Selection drives the population toward high fitness.

Here are some pros:
- Extremely simple. 
- General purpose. 
- Tractable theoretical models.
And some cons: 
- Coding is crucial -> it is important to fiddle with parameters values and tuning. 
- Too simple genetic operators.

### Memetic algorithms
They're a variant of genetic algorithms.  
You run a local search on each of the individuals before inserting them in the population, so that each solution becomes a local optima.

Essentially, you start with a population that is all made out of local optima. 

# When to choose either method?
Local search/metaheuristics approach preferable when: 
- neighborhood structures create a correlated search graph 
- computational cost of moves is low 
- ‘inventing’ moves is easy 

Population-based approach preferable when: 
- solutions can be encoded as composition of good building blocks 
- computational cost of moves in local search is high 
- it is difficult to design effective neighborhood structures 
- coarse grained exploration is preferable (e.g., huge search spaces)

----- 
# Swarm intelligence
Swarm intelligence (SI) is an artificial intelligence technique based around the study of collective behavior in decentralized, self-organized systems.

SI systems are typically made up of a population of simple agents interacting locally with one another and with their environment. Although there is normally no centralized control structure dictating how individual agents should behave, ==local interactions between such agents often lead to the emergence of global behavior==. Examples of systems like this can be found in nature, including ant colonies, bird flocking, animal herding, bacteria molding and fish schooling.

These agents are self organized, which means that a population of agents that are not planned in a central way. Each agents don't have a global view of the system. 

### Formal characteristics of a SI system 
- A set of simple individuals with limited capacities 
- Individuals are not aware of the system in its global view 
- Local communication patterns (direct or indirect/stigmergic) 
- Distributed computation: no centralized coordination of individual activities 
- Robustness & adaptivity

Other ingredients are also:
- Positive Feedback 
	- Reinforcement of common behaviors 
	- Amplification of random fluctuations and structure formation 
- Negative Feedback 
	- Saturation 
	- Competition 
	- Exhaust resources

Agents are also very simple and rule based. 

## Stimergy
Stigmergy is a form of indirect communication: an <u>agent modifies the environment and the other agents react to this chance</u>. 

For example, ants communicate through pheromone. Ants deposit pheromone trails while walking from the nest to the food and vice versa. 
Ants tend to choose (more likely) ==the paths marked with higher pheromone concentrations==. Cooperative interaction leads to the emergent behavior to find the shortest path.
![[stigmergy_example.png]]

The thing that makes this type of system work is the fact that ants chose the path using a probability and that they are __NON-DETERMINISTIC__. 

### ANT COLONY OPTIMIZATION
The ACO uses a __probabilistic parametrized model__ (the __pheromone model__) used to model pheromone trails. This is also our "engineering part", in which we have to tune the parameters. 

Ants build solution components in an incremental way (we're not using the tree representation since we're not doing an exploration).

When using ACO, we need to provide a representation of the search space as a graph $G=(C,L)$, where $C$ are the nodes (which represent the states) and $L$ are the edges.  

Not all paths can be feasible. 
_Constraints_ can be represented to define what is a consistent solution (unlike previous search strategies, in which they were handled not so well). 

### How do these ants proceed?
The connections, solution components or both have the following associated information:
- Pheromone $\tau$ - which is left by all the ants. 
- Heuristic value $\eta$

Both are taken into account when deciding which path to follow. 

We could interpret these values in this way:
- The __pheromone value__ abstracts natural pheromone trails and <span style="color:yellow"><i>codes the long term memory of the global search process.</i></span> 
- The __heuristic value__ represents the <span style="color:yellow"><i>prior background knowledge on the problem</i></span>.

Here's a basic schema of ACO:
![[ACO_schema.png]]

- The solution components in this case are represented by arcs. 
- The pheromone model is improved by other agents during the process (by leaving an intense pheromone trail).

### ACO Algorithm 
A simple first example of ACO can be summarized with this series of steps:
- Ants build a solution following a path on the construction graph. 
- A transition rule is followed to choose the next node to visit. 
- The heuristics and the pheromone are used. 
- Pheromone values are updated on the basis of the QUALITY OF THE SOLUTION FOUND by the ants.

Here's a very high-level algorithm template:
```
InitializePheromoneValues() 

while termination conditions not met do 
	for all ants a ∈ A do 
		s[a] ← ConstructSolution(τ ,η) 
	end for 
	
	ApplyOnlineDelayedPheromoneUpdate() 
end while
```


How do we chose our path in the ants system though?
Essentially, the past paths of each ant is memorized: starting from node $i$, we have to probabilistically chose the next [[2022-10-17 - INFORMED SEARCH STRATEGIES#Consistent Heuristics|consistent]] node to visit. To choose the next node, the probabilistic choice $p_{ij}$ depends on this formula:
$$
p_{ij} = 
    \begin{cases}
      \dfrac{[\tau_{ij}]^\alpha \ [\eta_{ij}]^\beta}{\sum_{k \ feasible}([\tau_{ij}]^\alpha \ [\eta_{ij}]^\beta)} & \text{if $j$ is consistent}\\ \\
     0 & \text{otherwise}
    \end{cases}\,
$$
Where:
- $\tau_{ij}$ is the pheromone trail.  
- The heuristics are valued as $\eta_{ij} = \dfrac{1}{d_{ij}}$, where $d_{ij}$ is the distance between $i$ and $j$. 
- $α$ and $β$ weight the importance of the pheromone and the heuristics.
- $j$ is the next possible node that we could visit. 
- On the denominator, we have the __sum__ of the probability of each possible next feasible node. 
- $\sum_{\text{k feasible}} p_{ik} = 1$

N.B.: a node $j$ without an edge from $i$ is 100% incosistent. 

### Updating the pheromone 
The pheromone is updated through the following rule:
 $$
 \tau_{ij} = (1 - \rho) \tau_{ij} + \sum^m_{k=1} \Delta \tau^{k}_{ij}
 $$

Where $\rho$ is the evaporation coefficient, and:
$$
\Delta \tau_{ij} = 
    \begin{cases}
      \dfrac{1}{L_k} & \text{if ant $k$ used arc $(i,j)$}\\ \\
     0 & \text{otherwise}
    \end{cases}\,
$$
And $L_k$ is the length of the path followed by ant $k$. 

### General algorithm ACO
```js
while termination conditions not met do 
	ScheduleActivities {
		AntBasedSolutionConstruction()
		PheromoneUpdate()
		DaemonActions() // optional
	} end ScheduleActivities 
end while
```

Where:
- `AntBasedSolutionConstruction()`: ants move by applying a stochastic local decision policy that uses values of pheromone and heuristic on graph components. While moving, ==ants take track of the partial solutions (paths) that have been built==. 
	- N.B: The pheromone is put on arcs!!.

- `PheromoneUpdate()`: you can update the pheromone in two ways: 
	- Ants update the pheromone <span style="color:yellow">during the solution construction</span> (online step-by-step pheromone update).
	- Ants can <span style="color:yellow">update backward the pheromone</span> on components used on the basis of the quality of the overall solution (online delayed pheromone update). 
	- _Evaporation_ is applied all time.

- `DemonActions()`: ==are “centralized” actions that cannot be executed by the single ants==. 
	- Local search procedure applied to each solution built.
	- Collection of global information to decide whether to leave additional pheromone to guide search from a global perspective.
	- Before updating the pheromone, you can run a local search (i.e. by exchanging the nodes) to try and improve the new solution found. 
	- The pheromone will be then updated according to the new solution found.  