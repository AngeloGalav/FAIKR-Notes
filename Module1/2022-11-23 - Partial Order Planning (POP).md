## Search in the space of Plans: non-linear planners
Linear planners are search algorithms that explore the state space. The plan is a linear sequence of actions to achieve the goals.

__Non-linear planners__ are search algorithms that generate a plan as a (local) search problem in the space of plans. In the search tree ==each node is a partial plan== and operators are plan <span style="color:yellow">refinement</span> operations.

A non-linear generative planner assumes that the initial state is a fully known __Closed World Assumption__: everything that is not explicitly stated in the initial state is considered as false.

<span style="color:limegreen"><b>Least Commitment planning</b></span>: ___never impose more restrictions than those that are strictly necessary___. Avoid making decisions when they are not required. This avoids many backtracking operations.

A non-linear plan is represented as:
- a set of <span style="color:yellow">actions</span> (instances of <span style="color:yellow">operators</span>) 
- a (not exhaustive) set of <span style="color:yellow">orderings</span> between actions (and possible effects)
- a set of <span style="color:yellow">causal links</span> \[described later\] 

The _initial plan_ is an empty plan with two fake actions: 
- <span style="color:red"><b>start</b></span>: No preconditions. Its effects match the initial state. 
- <span style="color:red"><b>stop</b></span>: No effects. Its pre-conditions match the goals 
- Ordering: start → stop (start \< stop)

At each step, either the set of operators or the set of orderings or the set of causal links ==is _increased_ until all goals are met==. Non required orderings are not inserted.
A solution is a set of partially specified and partially ordered operators. 

To obtain a real plan, the partial order should be linearized (**linearization** is finding the [linear approximation](https://en.wikipedia.org/wiki/Linear_approximation "Linear approximation") to a function at a given point) in one of the possible total orders (_linearization operation_).

What does this mean? Simply, let's consider the following. 
![[non_linear_plan_example.png]]
This formalization is way less cluttered, since ==it doesn't matter in which order each action is executed as long as the execution is compliant with the graph== (i.e. I can put the left sock first or the right: it doesn't matter as long as I put them before the shoe).   

Some other possible plan linearizations are: $putsock(left) \rightarrow putsock(right) \rightarrow wearshoe(right) \rightarrow wearshoe(left)$.  
$putsock(left) \rightarrow wearshoe(left) \rightarrow putsock(right) \rightarrow wearshoe(right)$.  
$putsock(right) \rightarrow wearshoe(right) \rightarrow putsock(left) \rightarrow wearshoe(left)$.  
etc...

# Partial Order Planning
The POP algorithms has this structure:
while (plan not complete) do:
- select an action $S_n$ that has a precondition not satisfied;
- select an action $S$ (new or already in the plan) that has $C$ among its effects; 
- add the __order constraint__ $S < S_N$ (specifies the direction of the edge);
- if $S$ is a new action, then add the constraint $Start < S <Stop$;
- add the causal link $<S, S_n, C>$;
- solve any threat on causal links end

In case of failure, if choice points exist, the algorithm backtracks and explores alternatives.

A <span style="color:limegreen">causal link</span> is a triple that consists of two operators $S_i$, $S_j$ and a subgoal $C$, which has to be __precondition__ of $S_j$ and effect of $S_i$.
A causal link stores the causal relations between actions: it traces why a given operator has been introduced in the plan. ==Causal links help to tackle the problem of interacting goals==.
$$
S_i \longrightarrow^{C} S_j 
$$
###### Causal links and threats
An action $S_3$ is a <span style="color:red"><b>threat</b> for a causal link</span>  $<S_1, S_2, C>$ if it has an effect that _negates_ $C$ and no ordering constraint exists that prevents $S_3$ to be performed between $S_1$ and $S_2$.
![[threat_example.png]]

To eliminate threats, we could adopt these two solutions:
- ___demotion___: the constraint $S_3 < S_1$ is imposed ($S_3$ can only be executed before $S_1$) 
- ___promotion___: the constraint $S_2 < S_3$ is imposed ($S_3$ can only be executed after $S_2$)

### POP Example
Consider the following configuration:
![[pop_plan_example_1.png]]
First, we need to select a precondition (in this case, we only have preconditions in the goal state) to be fulfilled. We choose _have(drill)_. 
We then select an action that has _have(drill)_ as an effect. This action is _buy(X,Y)_ \[we will later perform the substitution\]. 
We then link ==variable _Y_ with the term _drill_==, and impose the order constraint Start < _buy(drill)_< Stop.
Finally, we insert the causal link _<buy (X, drill), stop, have (drill)>_.
![[pop_example_2.png]]
We do the same thing for _have(milk)_ and _have(banana)_ and we obtain:
![[pop_example_3.png]]

We now need to select another unsatisfied precondition. We select _sells(X, drill)_, ==which is true in the initial state through substitution (X=_hws_)==. The same happens for sells(X,milk) and sells(X,banana) with _X=sm_.
![[pop_example_4.png]]
We now select the precondition _at(hws)_ of _buy(hws, drill)_, which of course is unsatisfied. 
The action _Go(X,Y)_ satisfies this precondition, since it has _at(Y)_ as one of its effects.  
![[pop_example_5.png]]
Select _at(X)_ as precondition of _go(X, hws)_, which is true in Start with _X=home_ and ==protect the causal link==. We do the same for _at(sm)_.
![[pop_example_6.png]]
<span style="color:red">Here comes the fun part.</span> If you noticed, we are now having 2 threats: 
- The causal link \<START, go(home, hws), at(hws)\> is threatened by the action _go(home, sm)_.
- The causal link \<START, go(sm, hws), at(sm)\> is threatened by the action _go(home, hws)_. 
It is almost as if there's a cross-threatenization. If we solve one, automatically we solve the other also. 
This means that if the agent performs _go(home, hws)_ it cannot be _at(home)_ to perform _go(home, sm)_ and viceversa.  

\[N.B.: in the exams, you'll ALWAYS have to specify all the threats.\]

So, let's try to solve the conflict between actions _go(home, hws)_ and _go(home, sm)_. 
In this case, ==imposing ordering constraints does not work==.
What we can do is <span style="color:limegreen">backtrack</span> to the precondition _at(x)_ of the action _go(X,sm)_. 
At this point, we can ==add an indirection step== by using _go(home, hws)_ from the state _Start_ with X = home to satisfy _at(X)_ with _X=hws_.  
We add a ___order constraint___ so that _buy(hws, drill) < go(hws, sm)_. In this way, we can't go to the supermarket until we've passed from the hardware store. _hws_ is now protected by the causal link between go(home, hws) and buy(hws, drill).
![[pop_example_7.png]]

Finally, we are almost over. We need to solve _at(home)_ precondition of the _Stop_ goal: this is easily solved by putting the action _go(home)_ before Stop.
![[pop_example_8.png]]

The final plan look something like this:
![[pop_example_final.png]]

# Modal Truth Criterion (MTC)
- Promotion and demotion alone are not enough to ensure the completeness of the planner.
- A planner is complete if it always finds a solution if a solution exists. 
- The __Modal Truth Criterion__ is a construction process that guarantees planner completeness. 
- A Partial Order Planning algorithm interleaves goal achievement steps with threat protection steps. The MTC provides __five plan refinement methods__ (one for the open goal achievement and 4 for threat protection) that ensure the completeness of the planner.

The 5 plan refinement methods are:
1. <span style="color:red"><b>Establishment</b></span>, open goal achievement by means of: 
	1. a new action to be inserted in the planning 
	2. an ordering constraint with Action already in the plan or simply 
	3. of a variable assignment. 
	Essentially, it consists on adding an action in order to achieve a goal. 
	I have an open condition (a fluent that is not yet achieved), I select it, and I select an action that achieves it (It can be and old one or a new one). 
2. <span style="color:red"><b>Promotion</b></span>, ordering constraint that imposes the threatening action before the first of the causal link; (we already seen this)
3. <span style="color:red"><b>Demotion</b></span>, ordering constraint that imposes the threatening action after the second of the causal link; (we already seen this)
4. <span style="color:red"><b>White knight</b></span>, insert a new operator or use one already in the plan between $S_1$ and $S_3$ such that it establishes the precondition of $S_3$ threatened by $S_1$. 
	Essentially, it inserts the threatening action __in between__ the two states. But obviously, the condition ¬C, that threatens the situation and deletes C, has no way of providing C (it is already providing ¬C). So, I add a so-called ___white knight___.
	![[wk_example.png]]
	White knight is used to solve cross-threatenization, and we used it in the second last step of the previous POP example. 
5. <span style="color:red"><b>Separation</b></span>, insert ___non codesignation constraints___ between the variables of the negative effect and the threatened precondition so to avoid unification. This is useful when variables have not yet been instantiated. i.e.:
![[Separation_example.png]]
\[N.B.: separation it is almost NEVER used in the exams or irl. \]
