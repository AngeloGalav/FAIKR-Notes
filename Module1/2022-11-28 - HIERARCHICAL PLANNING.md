## Sussman Anomaly in POP
To recap what was <span style="color:limegreen">Sussman Anomaly</span>: essentially, we have an objective with interacting goals in which we solve one of the goal, and in the process of solving the next one, the conditions for solving the previous goals are obliterated (we completely destroy what we have done).   

Normally, to solve the Sussman Anomaly using STRIPS, we have to "reload" the goal that is not true anymore, and solve the link again. In POP, that is not really required: we just have to add the causal links and solve the threats. 

Here's the final step:
![[excercise_sussman_conclusion.png]]

The new idea that lets us solve is the fact that __we are using a state that is already in the plan as the white knight__. The action in particular is _stack(b,c)_. 

# HIERARCHICAL PLANNING
Hierarchical planners are search algorithms that manage the creation of complex plans at __different levels of abstraction__, by ==considering the simplest details only after finding a solution for the most difficult ones==. 

In Hierarchical Planners, we need a language that enables operators at different levels of abstraction: 
- Values of "criticality" assigned to the preconditions 
- Atomic operators vs. Macro operators:
	- High level actions are made of smaller actions (the atomic actions) which are the one that we're actually executing.  

All operators are again defined by PRECONDITIONS and EFFECTS. 
Some popular hierarchical planning algorithms: 
- STRIPS-Like 
- Partial-Order
 
Given a goal, the hierarchical planner performs a ___meta-level search___ to generate a ___meta-level plan___ which leads from a state that is very close to the initial one to a state which is very close to the goal.

The plan is then completed with a lower level search, taking account of details omitted at the previous level. So a hierarchical algorithm must be able to: 
- organize at high level (meta-level) 
- expand abstract plans into concrete plans 
	- planning abstract parts in terms of more specific actions (planning basic level) 
	- expanding already prebuilt plans

## HIERARCHICAL PLANNING MACRO-OPERATORS
As we've just said, there are two types of operators: 
- <span style="color:limegreen">Atomic operators</span> represent elementary actions, typically defined as STRIPS rules. Atomic operators can ==be directly executed by an agent== 
- <span style="color:limegreen">Macro operators</span> in turn represent a ___set___ of elementary actions: they are decomposable into atomic operators. 
	- Macro operators before execution should be further decomposed. Their decomposition can be _precompiled_ or _from plan_.

We could have 2 type of decomposition of Macro operators:
- ___Precompiled decomposition___: the description of the macro operator also contains the DECOMPOSITION, the sequence of basic operators to be executed at run-time.
![[precompiled_decomposition_example.png]]
- ___Planned decomposition___: the planner must perform a low-level search for synthesising the atomic action plan that implements the macro action.

The planning algorithm can be either linear or non-linear.
==A hierarchical non-linear algorithm is similar to POP== where at each step one can choose between: 
- Reach an open goal with an operator (including macro operators) 
- expand a macro step of the plan (the decomposition method can be precompiled or schedule).

Essentially, this is the same as the POP algorithm, with the only difference that if an action is a macro-operator, we need to decide which decomposition to do. 

## DECOMPOSITION (POP-LIKE)
To ensure the decomposition is safe, some properties must be guaranteed:

__Constraints on the decomposition__: if the macro-action A has the effect X and is expanded with the plan P:
- X must be the effect of at least one of the actions in which A is decomposed and <u>it should be protected</u> until the end of the plan P 
- each precondition of the actions in P must be guaranteed by the previous actions in P or it must be a precondition of A 
- the P action must not threat any causal link when P is substituted for A in the plan

Only under these conditions you can replace the macro action A with the plan P. 
When we have to replace A with P, the following __orderings__ and __causal links__ should be added

Orderings
- for each B such that B < A then B < first(P) is imposed (first action of P)
- for each B such that A < B then last(P) < B is imposed (last action of P)

Causal links
- if <S, A, C> is a causal link in the initial plan, then it must be replaced by a set of causal links <S, Si, C> where Si are the actions of P that have C as a precondition and no other step of A before it has a C as a precondition.
- if <A, S, C> is a causal link in the initial plan, then it must be replaced by a set of causal links <Si, S, C> where Si are the actions that have C as an effect and no other step of P after it has the effect C. 

## ABSTRIPS
