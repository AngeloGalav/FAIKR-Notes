# Linear Planning
As we've seen last time, with linear planning with formulate the planning problem as a search in the state space, using classiscal search strategies. 

We could do the search in two ways:
- Forward: the search starts from the initial state and proceeds when it finds a state that is the superset of the goal. 
- Backwards: the search starts from the goal and proceeds backwards until a state that is a subset of the initial state is found.  

Remeber: $do(A,S)$ is the ==function which returns a state that is given by the execution of the action A in the state S==. 

Here is an example of Forward search:
![[forawrd_search.png]]
As you can see, using this methods the search ___space becomes very big___. We need to find a way to prune the tree (or an heuristic to guide us through). 


### Backwards search 
When using backwards search, the __goal regression mechanism__ is a mechanism to reduce a goal into subgoals during search by applying rules (through the application of actions). 

When dealing with goal regression, for each rule we have 3 list that we have to consider:
- PRECOND: P-list (list of preconditions)
- DELETE: D-list (list of deleted preconditions)
- ADD: A-list (list of added post-conditions)

Regression of $G$ through $R =  Regr[G, R]$ is: 
- $Regr[G,R] = true$ if $G∈Alist$ 
- $Regr[G,R] = false$ if $G∈Dlist$
- $Regr[G,R] = G$ otherwise

![[backward_algorithm.png]]

## DEDUCTIVE PLANNING
__Deductive planning__ uses logics (FOL in particular) for representing states, goals and actions and <span style="color:limegreen">generates a plan as a theorem proof</span>. 

Some examples of deductive planning are the __Green and Kowalsky__ formulations.
\[N.B.: the following prepositions are in the __Green__ formulations.\]

#### Situation calculus
In situation calculus, we use:
- First order logic to describe states 
- clauses to describe actions.

We have:
- <span style="color:yellow"><b>Situation</b></span>: a world snapshot describing properties (_fluents_) that hold in a given state $s$. 
	For example, in the block world:
	```js
	on(b,a,s) // in state S, b is on a
	ontable(c,s) // in state S, c in on the table
	handempy 
	clear(c) // c hasn't got anything on top of it
	diff(a,b) // if true, a is a different entity from b
	```
- <span style="color:yellow"><b>Actions</b></span>: define which fluents (_properties_) are true as a consequence of an action.
	For example:
```
on(X, Y, S) and clear(X, S) → (ontable(X, do(putOnTable(X), S))) and 
	 (clear(Y, do(putOnTable(X), S)))
```

We can use situation calculus to build a planning based on logic resolution.
In this case, we have to find a proof of a formula containing a state variable. At the end of the proof, the state variable will be instantiated to the plan to reach the objective.

For example:
- To prove $\vdash \text{ontable(b, \ S)}$ means that we have to ==find a state S in which $\text{ontable(b,\ S)}$ exists==. 
- We know that it exists, and it is $S = putOnTable(b, \ S)$. 

Deductive planning has an <span style="color:limegreen">high expressivity, and we can use it to describe complex problems</span>. On the other hand, it has one big limitation: <span style="color:red">the frame problem</span>.

## Frame Problem 
The frame problem is a famous knowledge representation problem.
At each time, __we have to explicitly list all fluents that change and that DO NOT change after a state transition__. If we have a complex domain, <span style="color:red">the number of these axioms <b>grows enormously</b></span>.

For example, consider the following situation:
![[example_1_frameproblem.png]]

![[example_2_frameproblem.png]]
A.11 and A.12 are written so that the implication (A => B) is expressed as (¬A $\lor$ B).

In this case, we have to prove that the goal $on(a,b,S1)$ is true.
We need to prove that $clear(X,S) \land clear(Z,S) \land on(X,Y,S) \land diff(X,Z) \longrightarrow on(X,Z, do(move(X, Y, Z), S))$ .

Through substitution, we can satisfy the premise.  ![[substitution_to_proof.png]]


In short: ==to solve a problem we need a complete description of the state after each move==. 

To describe an action, beside effect axioms we have to specify all fluents that would not change by the execution of an action (___frame axioms___). In our example we would need:
![[frame_example 1.png]]

If we do not explicitly state that what does not change during the execution of an action, then it remains unchanged in the next state, and <u>we need to store all the possible states</u>.

If the problem is too complex, there will be too many axioms

## Kowalsky formulations 
When using this type of formulation, we use:
- predicate $holds(rel, s/a)$ to describe all the relations $rel$ that are true in a given state $s$ or made true by the execution of an action.
- predicate $poss(s)$ that indicates if a state $s$ is __possible__ (namely <span style="color:red">reachable</span>).
- predicate $pact(a,s)$ to indicate that it is possible to execute an action $a$ in a state $s$, namely ==the preconditions of $a$ are true in $s$==.

If a state $S$ is possible and the preconditions of action $a$ are satisfied, then the state produced by the execution of action $a$ in the state $S$ is possible. In symbols:
$$
poss(S) \land pact(A,S) \implies poss(do(A,S))
$$

The __Kowalsky formulation__ has an advantage in respect to the Green formulation: <span style="color:yellow">it needs only <b>one frame assertion per action</b></span> (and not all frame axioms, one for each unchanged entity). 

For example, using Kowalsky formulation, the previous example would have been formulated as:
$$
holds(V,S) ∧ diff(V,clear(Z)) ∧ diff(V,on(X,Y)) \implies  holds(V,do(move(X,Y,Z),S)))
$$
that states that every term different from $clear(Z)$ and $on(X,Y)$ (these 2 are in 
the __delete list__) are __true__ after the execution of the action move.

Here's an example of the __Kowalsky formulation__. This is how the initial state is described:
![[kowalsky_example_init.png]]
In the Kowalsky formulation, action $move(X,Y,Z)$ has the following effects (ADD):
- $holds(clear(Y),do(move(X,Y,Z),S))$
- $holds(on(X,Z),do(move(X,Y,Z),S))$

Here is a list of the preconditions of move (PRECOND):
$pact(move(X,Y,Z),S) \longleftarrow holds(clear(X),S), holds(clear(Z),S), holds(on(X,Y),S), diff(X,Z)$

Frame condtions (the list of _deleted preconditions_ essentially, DELETE):
$holds(V,do(move(X,Y,Z),S)) \longleftarrow holds(V,S), \ diff(V,clear(Z)), \ diff(V,on(X,Y))$

Our goal is:
$poss(S), \ holds(on(b,g),S), \ holds(on(a,b),S)$

This Goal is true for the plan:
$do(move(a, d, b), do(move(b, e, g), s0))$

Here's how "move" would be translated in Kowalsky notation:
![[move_in_kowalsky.png]]