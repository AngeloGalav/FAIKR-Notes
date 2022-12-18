# STRIPS
Strips stands for _Stanford Research Institute Problem Solver_.
It is a specific language for the actions, and has an easier syntax than the situation calculus (less expressive more efficient).

### Syntax of STRIPS  
- For state representation, we write the _fluents that are true in a given state_.
	- For example: $on(b,a), clear(b), clear(c), ontable(c)$. This are all possible fluents that could describe a state.
	- Goal representation: ==fluents that are true in the goal state==. We can have variables (i.e.: $on(X,a)$). 
- Each <span style="color:limegreen"><b>state</b></span> is represented by 3 lists:
	- __PRECONDITIONS__: fluents that should be true for applying the move 
	- __DELETE List__: fluents that become false after the move 
	- __ADD List__: fluents that become true after the move
	- For example, $Move(X, Y, Z)$ has 
		- Preconditions: $on(X,Y), clear(X), clear(Z)$ 
		- Delete List: $clear(Z), on(X,Y)$ 
		- Add list: $clear(Y), on(X,Z)$ 
	- Sometimes, ADD and DELETE list are glued into the EFFECT list:
		- Effect List: $¬clear(Z), ¬on(X,Y), clear(Y), on(X,Z)$.

Frame problem solved with the <span style="color:limegreen">Strips Assumption</span>: <span style="color:yellow">everything which is not in the ADD and DELETE list is unchanged</span>.

## The STRIPS Algorithm
Strips also makes use of an ==ad hoc algorithm for the plan construction==.
It is a linear planner based on __backward search__. 
The initial state fully known (Closed World Assumption), and we have two data structures:
- Goal stack 
- Description of the current state

In the STRIPS algorithm, there are interactive subgoals, which can be seen as substates of the whole plan. 

Here is the actual STRIPS algorithm:
- Initialize the stack with the goals to reach 
- while _stack not empty_ do:
	- if top(stack) = A and Aθ⊆CurrentState then: _(note that A can be a <span style="color:yellow">set of goals</span> or a single one)_
		- pop(A) and execute substitution θ on the stack 
	- else if top(stack) = a then 
		- Select a rule R with a ∈ Addlist(R)
		- pop(a), push(R), push(Precond(R)); 
	- else if top(stack) = a1 ∧ a2 ∧ … ∧ an then <span style="color:blue">[1]</span>
		- push(a1),…, push(an) 
	- else if top(stack) = R then pop(R) and apply R on S

N.B.: R are _actions/rules_, while A and a1...an are _fluents_.  
N.B.: Fluents separated by commands get translated into a conjuction (i.e. ontable(a,b), ontable(b,c) == ontable(a,b) $\land$ ontable(b,c).  

==The order in which we instert the goals in the goal stack at \[1\] is __a non-deterministic choice point__. The end of goals should be verified afterwards - interacting goals==. 
What this means is that we have to consider all the possible orderings to find all the goals. 

Remember: THIS IS A BACKWARDS SEARCH ALGORITHM (from Goal to IS). 
The algorithm is very confused tho: look at the following notes to understand better. 

VERY USEFUL EXAMPLES [AT SLIDES 39 TO 42 HERE](https://virtuale.unibo.it/pluginfile.php/1352459/mod_resource/content/1/11-Planning.pdf)
Here's a sneak peek of the example still:
![[strips_example_1.png]]
![[strips_example_2.png]]
You can clearly see the concept of interacting subgoals in these images. 
N.B.: in this example, the words in bold are _rules_, the rest are actions.  

#### Notes on the algorithm 
- The problem is divided into subgoals which might interact 
- Many possible goal orderings. 
- At each step we select one subgoal from the goal stack. 
- When we have a set of actions that reach a goal, we execute them on the state that proceeds forward. 
- The process goes on until the stack is empty 
- When at the top of the stack we find an and of goals, we need to check that this is still satisfied in the current state before removing it. If it is not, we have to reinsert the and change order.

### STRIPS PITFALLS 

1. __Very large search space__. In the example (the one in the slides) we have seen a single path, but there are many alternatives 
	- Non deterministic choice in the ordering 
	- More actions applicable to reduce a goal 
- Solution: __Heuristic strategies__ to select __the goal__ or __the action__ 
- MEANS-ENDS ANALYSIS:
	- Find the most significant difference between the state and the goals 
	- Reduce that differently in the previous nodes of the path and make another plan 

2. ___Interacting goals___. Consider the case in which we have interacting goals $G_1, G_2$. We could be planning actions for reaching G2, but ==then to solve G1 we destroy what we have done for G2==. At the end of the planning G2 is not true anymore.
	- Complete solution: Try all possible orderings of goals and subgoals. 
	- Practical solution (Strips): 
		- Solve them independently, and then verify afterwards.  
		- If the conjunction is not true, <span style="color:limegreen">change ordering</span>. 

##### SUSSMAN ANOMALY
Another possible pitfall. 
Essentially, it consists on the fact that we could be applying STRIPS and reach the goal inefficiently due to the presence of __interacting subgoals___: a subgoal can interfere with the realization of the "global" goal.  

Consider this example:
![[sussman_example.png]]
![[sussman_example_2.png]]
In this case, the conjunction on(a,b) ∧ on(b,c) is not valid.
So, to obtain the goal we <span style="color:red">reinsert</span> on(a,b) in the goal stack and obtain: 
9. pickup(a) 
10. stack(a,b)
In the end, we have obtained what we need, <u>but not that efficiently</u>.