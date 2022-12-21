
Many AI problems can be seen as constraint satisfaction problems, in which the objective is to find a state of the problem that meets a given set of constraints. 

One such example is the N queens problem, which we've already seen [[2022-10-19 - LOCAL SEARCH#N queens problem|here]].

## Modeling the N queens problem. 

In order to solve the problem, we need to model with variables, domains and constraints first. After that, we can solve it using __our search strategies__.

##### First attempt
The positions of the board are $N \times N$ represented by variables $8 \times 8$ $x_{ij}$ .
The instantiation of a variable $x_{ij}$ to the value 1 indicates that this position is assigned to a queen, and if the value is 0 the position is free. Domain of possible values: \[0,1\]. 

The constraints are that there cannot be two 1 simultaneously on the same row, column or diagonal.

##### Second attempt 
- The eight queens are represented by the variables $X_1, X_2, ..., X_8$.
- The subscript (index?) refers to the column occupied by the corresponding queen.
- The instantiation of a variable $X_i$ to the value $k$ belonging to the set \[1…8\] indicates that the corresponding queen is placed on the $k$-th row of the $i$-th column.

## Constraints 
There are two types of constraints: 
- those which bind the variables of the domain values. 
- those who impose __relationships__ between the values assumed by the variables.

Let's consider this set of constraints for the N queens problem:
![[constraint_n_queens.png]]
- The first constraint requires that the values of the variables of the problem are between the integers 1 and 8: <span style="color:red">unary constraints</span>.
- The next three constraints define relationships between the variables and, in particular, between two variables at a time: <span style="color:yellow">binary constraints</span>.
	- The second requires that no two queens will be in the same row at the same time.
	- The 3rd and 4th constraints are related to diagonal position. 

## Constraint Satisfaction Problem (CSP)
A CSP (Constraints Satisfaction Problem) is defined on a finite set of variables: 
- $(X_1, X_2, ..., X_n)$ decisions that we have to take 
- $(D_1, D_2, ..., D_n$) domains of possible values 
- a set of constraints.

A constraint $c(X_{i1}, X_{i2}, ... X_{ik})$ between k variables is a subset of the Cartesian product $D_{i1} \times D_{i2} \times ... \times D_{ik}$ that specifies which values of the variables are compatible with each other. 
This subset must not be explicitly defined but is represented in terms of relationships.
A solution to a CSP provides an assignment of all the variables that satisfies all the constraints.

CSPs can be solved through search:
- Initial state: empty assignment {} 
- Successor Function: assigns a value to a variable not yet assigned -> fail if there is none 
- Goal: The assignment is complete (all variables are assigned to a feasible value).

1. Same scheme for all CSPs 
2. Limited depth $n$ if $n$ is number of variables -> uses ___depth-first search___ 
3. The path is irrelevant. 
4. Problem with $d^n$ leaves (if $d$ is the cardinality of domains and $n$ is the number of variables). 

A possible search tree for a CSP is obtained after establishing an order for variables: ==each level of the tree corresponds to a variable and each node corresponds to a possible value assignment==. 
Each leaf of the tree would then represent an assignment of values to all the variables. If this assignment satisfies all constraints, then the corresponding leaf represents a solution to the problem, otherwise it is a failure. 
The search for a solution is equivalent to the exploration of the tree to find a leaf-solution.
In an n-variable problem in which all the domains have the same cardinality d, the number of leaves of the search tree is equal to $d^n$.

The tree generated with these methods are very big though, so we need to find a smart tree exploration strategy. 

## PROPAGATION ALGORITHMS
The propagation algorithms are smart search methods that ==exploit constraints to prevent failures== rather than recover from failures have already occurred. This eliminates subtrees that lead to a failure thus limiting unnecessary backtracking. 
This is achieved with:
- A priori pruning of the search tree: use of constraints between the variables of the problem to reduce the search space before reaching the failure.


Given a CSP there are two possible approaches to its solution:
- <span style="color:limegreen"><b>Propagation Algorithms</b></span>: based on the propagation of constraints to ==eliminate a priori==, while ==searching==, portions of the search tree which would lead to a failure. 
- <span style="color:limegreen"><b>Consistency Techniques</b></span>: based on the propagation of constraints in order to ==derive a simpler problem than the original==.

## A posteriori and PROPAGATION 
The two techniques that use __a posteriori constraints__ are: 
- The Generate and Test (GT). 
- The Standard Backtracking (SB).
These are also called __no propagation algorithms__.
![[generate_and_test.png]]

The <span style="color:limegreen"><b>propagation algorithms</b></span> are based on the inverse concept.
Techniques such ___Forward Checking___ (FC), and ___Looking Ahead___ (LA). 
- A module propagates the constraints as much as possible (constrain); at the end of propagation either we have reached solution, or a failure or new information about the free variables is required (generated).
![[propagation_example.png]]

### GENERATE AND TEST
The language interpreter develops and visits a decision tree covering it in depth by assigning values to variables without bothering to verify the consistency with any constraint.
Essentially, we try all permutations of colors and then check if the constraints are satisfied. 

Of course, it is very inefficient. 

### STANDARD BACKTRACKING
It is better than GT, but ==it still uses constraints in an a-posteriori fashion==.  
At each instantiation of a variable $x_i$, constraints involving $x_i$ and previously instantiated variables are checked. Therefore, the use of constraints is more effective than for Generate and Test as ==it does not continue searching in the branches that present contradictions==.

The constraints are used backwards (backward) and lead to an effective reduction of the search space w.r.t. the one produced by generate and test.
However this reduction is done backward after the assignment.

# PROPAGATION ALGORITHMS 
Exploiting constraints involving free variables (still to be assigned variables) would detect this situation in advance thus avoiding expensive backtracking. 
==The idea behind a priori propagation algorithms is an active use of the constraints during search==. They prune a priori the search tree. Each variable is associated with the the set of values that are still consistent after each assignment.

## FORWARD CHECKING
After each assignment of variable $x_i$, the forward checking propagates all constraints involving $x_i$, and all variables that are not yet instantiated.
It is based on the observation that the assignment of a value to a variable has impact on all the available values for the free variables. In this way, the constraints act forward and reduce the space of the solutions before exploring it.

Consider variables $X_1, X_2, X_3, X_4$, with $X_1$ and $X_2$ already assigned. 
The Forward Checking checks c(X2, X3) for each value in D3 and c (X2, X4) for each value in D4. ==The values of D3 and D4 that are incompatible with the current assignment of X2 are deleted==. 

If the domain of a free variable becomes empty we have a ___failure___.
Some examples are in slides 37 onwards of this [pack](https://virtuale.unibo.it/pluginfile.php/1352465/mod_resource/content/2/14-Constraints.pdf).

### LOOK AHEAD
Beside checking the constraints with the current instantiated variable, ==look ahead also checks __THE DOMAIN__ the non-assigned variables==. 
Look Ahead checks the existence, in the domains associated with the non-assigned variables, of ==values that are compatible with the constraints containing only non-instantiated variables==. It is verified the possibility of a future consistent assignment between (pairs of) free variables.

We can divide the look-ahead approaches into ___Partial Look Ahead (PLA) and Full Look Ahead (FLA)___. 

- __PLA__: constraint propagation containing the not yet assigned variable $X_h$ with not yet instantiated "future" variables, i.e. variables $X_{h + 1}, ..., X_n$ 
	- For each value in the domain of $X_h$ it checks if in the domain of not yet assigned variable $X_{h + 1}, ..., X_n$ there is a value compatible with it. 
- __FLA__: constraint propagation containing the not yet assigned variable $X_h$ with not yet instantiated variables $X_{k + 1}, ..., X_{h-1}, X_{h + 1}.., X_n$. 
	- For each value in the domain of $X_h$ it checks if in the domain of not yet assigned variable $X_{k + 1}, ..., X_{h-1}, X_{h + 1}.., X_n$ there is a value compatible with it.

Here is a good example of PLA vs FLA:
![[pla_vs_fla.png]]

<span style="color:red"><b>PLA</b>, verification</span>: 
- for each value in D1 checks if there exists at least one value in D2 and at least one value in D3 compatible (==in case these values do not exist we delete the value from the domain D1==)
- for each value in D2 checks if there exists at least one value in D3 compatible (in case this value does not exist we delete the value from the domain D2) 

<span style="color:red"><b>FLA</b>, beside the checks performed by PLA checks also</span>: 
- for each value in D2 checks if there is at least a value compatible D1 
- for each value in D2 checks if there exists at least one value in D3 compatible (in case this value does not exist we delete the value from the domain D2) 
- for each value in D3 checks if there exists at least one value in D2 and at least one value in D1 compatible (in case these values do not exist, we delete the value from the domain of D3)

Some good examples are in slides 51 of [this pack](https://virtuale.unibo.it/pluginfile.php/1352465/mod_resource/content/2/14-Constraints.pdf). 

Using FLA, the computational load due to additional verification of the consistency of constraints needs to be balanced by a reduced search time. Sometimes, in the first levels of the tree FC is more effective.

## SEARCH HEURISTICS
Another way in which we could have a bit more control during the search is through the use of HEURISTICS. Some of them can be applied into two aspects of the tree search:
- The choice in the ordering of variables; 
- The choice in the ordering of values to be assigned to the current variable;

The heuristics can then act on these two degrees of freedom to try to ensure the achievement of a good solution in a reasonable time for even the most complex problems.

We have 2 types of heuristics:
- ___VARIABLE SELECTION HEURISTICS___: ==determine what should be the next variable to instantiate==. The two most commonly used heuristics are:
	- the <span style="color:limegreen">first-fail</span> (a.k.a MRV: Minimum Remaining Values) that chooses the variable with ==the variable with the smallest domain==
	- <span style="color:limegreen">most constrained principle</span>, which chooses the ==variable appearing in the largest number of constraints==. Rationale: most difficult variables are assigned first. 

- ___VALUE SELECTION HEURISTICS___: determine what value to assign to the selected variable. There are no general rules here, but the rationale is to try first ==those values that are most likely to succeed== (<span style="color:limegreen">least constraining principle</span>)

A further classification of heuristics is as follows: 
- ___Static heuristic___: determine variable and value order before starting the search; This order remains unchanged throughout the research. 
- ___Dynamic heuristics___: select the next variable or value <u>each time a new selection is done</u> (so at every step of labeling). 

Dynamic heuristics are potentially better (less backtracking). The computation of the perfect heuristic (which requires no backtracking) is a problem that has, in general, the same complexity of the original problem. We must therefore find a tradeoff.

## CONSISTENCY TECHNIQUES
In contrast to propagation algorithms that propagate the constraints as a result of instantiations of variables involved in the problem, ==consistency techniques reduce the original problem by eliminating domain values that cannot appear in a final solution==. 

They can be applied statically or at every step of assignment (labeling) as powerful propagation techniques for the not yet instantiated variables. 
==All consistency techniques are based on a representation of the problem as a network (___graph___) of constraints==. 
The arcs can be oriented or non-oriented: for example, the constraint > is represented by a directed arc, while the constraint != from a simple arc (undirected or doubly oriented).

For each CSP exists a graph (constraint graph) in which the nodes represent variables and the arcs are the constraints. 
- The binary constraints (R) connecting two nodes $X_i$ and $X_j$
- The unary constraints are represented by ==arcs that begin and end on the same node== $X_i$.

For the map coloring problem, the graph would something like this:
![[map_coloring_graph.png]]

## NODE CONSISTENCY 
> [!NODE-CONSISTENCY: consistency level 1]
A node of a graph of constraints is ___consistent___ if, for each $X_{the} \in D_{the}$, the unary constraint on $X_{the}$ is satisfied.

![[node_consistency_example.png]]
In the above example, the node is not consistent because the value $g \in D_i$ violates the unary constraint on $X_i$. To make the node consistent, ==it is necessary to eliminate from the domain of $X_i$ the value g==. 
A graph is <span style="color:limegreen">node consistent</span> if all its nodes are consistent.

## ARC CONSISTENCY 
The consistency of level 2 <u>is obtained from a node-consistent graph</u> and applies to arcs.
>[!ARC CONSISTENCY - consistency level 2]
> An arc $a(i, j)$ is consistent if for each value $x \in D_i$ if there exists at least one value $y \in D_j$ such that the constraint between $i$ and $j$ is satisfied. $y$ is called __support__ for $i$.

![[arc_consistency_example.png]]
The arc in the example above is not consistent because, considering the value $r \in D_i$, there is no value belonging to $D_j$ which satisfies the constraint between them. 
T==o make the arc between and $X_j$ consistent it is necessary to delete the value of $r$ from the domain of $X_i$==.

## ITERATIVE PROCEDURE
Arc consistency is an iterative process that converges to a stable and arc-consistent network. When the network reaches a stable configuration, it is called QUIESCENT.

The arc consistency can be applied 
- _before the search_, as preprocessing to produce a simplified problem with the same solutions of the original one or 
- __as propagation step__ (as done for Full look ahead) after each variable assignment it is often called Maintaining Arc Consistency (MAC)

## PATH-CONSISTENCY
The consistency of level 3 is obtained starting from an arc-consistent graph.

>[!PATH CONSISTENCY - consistency level 3] 
>A path between the nodes $(i, j, k)$ is path consistent if, for every value $x \in D_i$, and $y \in D_j$ (that are node and arc-consistent), there is a value $z \in D_k$ that satisfies the constraints $P (i, k)$ and $P (k, j)$.

![[path_consistency_example.png]]
Let's consider the above constraint graph related to the map color problem. 
In this case, the graph is arc-consistent, but it is clear that it has no feasible solution.

To solve the path consistency  problem, additional data structures needed.
For each pair of variables (for each arc) we need to ==store pairs of compatible values== for which exist a support in the domain of any third variable (example: for the arc $X_i$ to $X_j$, none of the pairs $<r, b>$ and $<b, r>$ has a support value in the domain $D_k$). 
The network in the example, though, cannot be made path consistent thus leading to a failure.

By adding a color to the available ones (color $g$), the network becomes path consistent. 
Check the consistency of level $n = 3$ for this network that has exactly $n= 3$ variables is equivalent to verify that there is a solution AND all values that are left in the domain are part of a consistent solution.
This happens because ==the level of consistency is equal to the number of variables==.

## K-CONSISTENCY
In principle, for a CSP of $k$ variables, if we want to apply consistency techniques and obtain only those values that are part of a feasible solution in the variable domains, we need to apply $k$-consistency.
For each ($k-1$)-tuple of values consistent with the constraints imposed by the problem, there is a value for each $k$-th variable that satisfies the constraints among all $k$ variables.

In general, if a graph containing $n$ variables is $k$-consistent with $k <n$, then search in the remaining space is needed.
If a CSP containing n variables is n-consistent, then you can find a solution without search.
However, making n-consistent a network of n variables has an complexity ___exponential___ in n (the cost is the same as solving the original problem).