### Some additional notes regarding Graph Search
It's very important that the heuristic must be consistent.  

# Meta-Heuristic
You have two graphs essentially:
1. __Neighborhood graph__ to represent search space topology
2. __Search graph__ to represent the actual search space exploration
![[meta_graphs_example.png]]

## Simulated annealing
A popular algorithm. 
It changes local search by doing _worsening moves_, and each move has associated a probability to it. The probability of each move then decreases.
So that the probability of accepting a worsening move decreases when you proceed. 

The decrease of this probabiloty is _exponential_.
The algorithm for this method is:
```
s = GenerateInitialSolution()
T = T0 
while termination conditions not met do 
	sʹ = PickAtRandom(N (s)) 
	if f(sʹ) > f(s) then 
		s = sʹ    // sʹ replaces s
	else 
		Accept sʹ as new solution with probability p(T , sʹ , s) 
	end if 
Update(T) 

end while
```
In the end of the execution, the probability a reaching a worsening move is almost 0. 

## TABU Search
Local search is completely blind. TABU Search tries instead to remember something about the past search, then we have the possibility of not exploring the same states again and again without improvement. 
It keeps tracks of moves and states, which are seen as "taboo" (to be avoided). These informations are kept in a _taboo list_.

In this way, cycles are also avoided. 
```
s ← GenerateInitialSolution()

TabuList ← ∅ 

while termination conditions not met do
	s ← BestOf(N (s) \ TabuList ) 
	Update(TabuList ) 
end while
```

Small variation to the normal local search algorithm, but very effective. 

Since states are inefficient to memorize, usually *__moves__* are stored instead.
Sometimes, we use a _ASPIRATION CRITERIA_, which essentially goes through a forbidden move if the solution we obtain is better than the current one (this can only be applied to the taboo list is made of moves and not states).

## ITERATED LOCAL SEARCH
This search acts on the neighbourhood. This local search has some perturbation steps to escape from the local optima. 

- Uses local search steps with __intensification__ (exploring all the current solution), to find a local optima very quickly. 
- Uses perturbation for effectively esaping local optima quickly (__diversification__).

```
s0 ← GenerateInitialSolution() 
s∗ ← LocalSearch(s0) 

while termination conditions not met do
	sʹ ← Perturbation(s∗, history) 
	s∗ʹ ← LocalSearch(sʹ) 
	s∗ ← ApplyAcceptanceCriterion(s∗,s∗ʹ,history)
end while
```

\[ by perturbing we mean jumping somewhere else \]

This algorithm uses _high level search strategies_, which are _intensification_ and _diversification_,  which have to be applied to effectively explore the search space.


# Population based metaheuristics

This methods uses a set of states, a population of states, and we apply some concepts found in nature (i.e. evolution, swarms etc).

The idea is this: if a solution with a component that is good and one that is not, I can merge this solutions in order to have the best of both worlds.

## Genetic algorithms
These algorithms are inspired by evolution. They have 3 key components:
- __Adaptation__: organisms are suited to their habitats.
- __Inheritance__: offspring resemble their parents.
- __Natural selection__: new, adapted types of organisms emerge and those that fail to change adequately are subject to extinction.

The _fittest_ individuals have higher chance of the generating an offspring (survival of the fittest).  The children are similar, but not equal, to their parents, and the traits characterizing the fittest individuals spread across population, generation by generation.

We GAs, __we are not simulating evolution__, but we are exploiting these key concepts for problem solving. 

#### Evolutionary cycle
- You generate the population
- You generate recombination, through parents. By recombination, we mean the genetic material of the parents. 
- Mutation: introduces variability in the genotypes.
- Selection: acts in the choice of the parents whose genetic mateiral is then reproduced with variation. 
- Replacement/insertion: defines the new population from the new and the old one. 
![[evolutionary_cycle.png]]
#### Terminology
- A population is the set of individuals (solutions) 
- Individuals are also called genotypes 
- Chromosomes are made of units called genes (could be integers number) 
- The domain of values of a gene is composed of alleles (e.g., binary variable ↔ gene with two alleles)

#### GENETIC OPERATORS
- Recombination or __Crossover__: cross-combination of two chromosomes (loosely resembling biological crossover). Essentially, a part is taken from the first parent and another from the second parent. 
- **Mutation**: we modify a gene with a probability of _pM_.
- __Proportional selection__: the probability for an individual to be chose is proportional to its fitness. Usually represented as a roulette wheel. 

Generational replacement: the new generation replaces entirely the old one. 
- Advantage: very simple, computationally not expensive, easier theoretical analysis.
- Disadvantage: it might be that good solutions are not maintained in the new population.

Alternatively, one can keep the best n individuals from the old and the new population (or choose any variant of this scheme)

Here are some mathematical explanation of the operators:
![[genetic_operations_maths.png]]
Depending on the tuning of the parameters, the quality of the solution changes. 

## Genetic Algotrithms
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

The termination conditions could be:
- Execution time limit reached. 
- Satisfactory solution(s) have been obtained. 
- Stagnation (limit case: the population converged to the same individual)

### Memetic algorithms
You run a local search on each of the individuals, so that each solution becomes a local optima, and And then you run a local search. 

## When to choose either method?
Local search/metaheuristics approach preferable when: 
- neighborhood structures create a correlated search graph 
- computational cost of moves is low 
- ‘inventing’ moves is easy 

Population-based approach preferable when: 
- solutions can be encoded as composition of good building blocks 
- computational cost of moves in local search is high 
- it is difficult to design effective neighborhood structures 
- coarse grained exploration is preferable (e.g., huge search spaces)


# Swarm intelligence
Swarm intelligence (SI) is an artificial intelligence technique based around the study of collective behavior in decentralized, self-organized systems.

These agents are self organized, which means that a population of agents that are not planned in a central way. Each agents doesnt have a global view of the system. 

[..]

### Stimergy
Stigmergy is a form of indirect communication: an agent modifies the environment and the other react to this chance. 
Example: ants communicate through pheromone

[..]

### ANT COLONY OPTIMIZATION
Probabilistic parametrized model, the __pheromone model__, used to model pheromon trails.

Ants build solution components in an incremental way (we're not using the tree representation since we're not doing an exploration).

Not all paths can be feasible. 

[..]

### ACO System
Ants build a solution following a path on the construction graph. 


```
InitializePheromoneValues() 

while termination conditions not met do 
	for all ants a ∈ A do 
		sa ← ConstructSolution(τ ,η) 
	end for 
	
	ApplyOnlineDelayedPheromoneUpdate() 
end while
```

### ANT-SYSTEM
[..]

Daemon actions: are centralized actions that cannot be executed by single ants. Essentially, they consist on local search procedures applied to each solution built. 
\[ Local search is alwalys a way to improve the solution. \]
