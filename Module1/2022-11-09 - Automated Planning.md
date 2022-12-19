__Automated Planning__ is an important problem solving activity which consists in __synthesizing a sequence of actions performed by an agent that leads from an initial state of the world to a given target state__ (goal).

Given: 
- an initial state 
- a set of actions you can perform 
- a state to achieve (goal)
 
Find: 
- a plan: a partially or totally ordered <span style="color:limegreen">set of actions</span> needed to achieve the goal from the initial state 

Planning is: 
- one application per se 
- A common activity in many applications.

## Automated planner
An automated planner is an intelligent agent that operates in a certain domain described by: 
1. a representation of the initial state 
2. a representation of a goal 
3. a formal description of the executable actions (also called operators) 

An automated planner ==dynamically defines the plan of actions needed== to reach the goal from the initial state.

### Action representation 
As we just seen, a planner relies on a _formal description_ of the executable actions. It is called __Domain Theory__.

Each action is identified by a name and declaratively modeled (described) through preconditions is postconditions, which are essentially two lists. 
- <span style="color:red"><b>Preconditions</b></span> are the conditions which must hold to ensure that the action can be executed;
- <span style="color:red"><b>Postconditions</b></span> represent the effects of the action on the world.

Often the Domain Theory consists of operators containing variables that define <span style="color:limegreen">classes of actions</span>, by which ==a different instantiation of the variables corresponds to a different action==. 
![[acton_example.png]]![[action_example_2.png]] 

## Planner properties 
The planners which are seen in this course are called __offline__ planners, which means that are general purpose planners and unrelated to a specific domain. i.e., the planner doesn't not know what a block is. 

- __Offline__ means that when we start we take a snapshot of our enivroment, and plan actions without executing them until we have a complete plan -> we can __backtrack__.
- A planner is __reversible__ when the choices made during the plan generation are backtrackable. 
- A planner is __complete__ if it always finds a plan when it exists. Online planners are not complete, since not all actions are backtrackable and they execute at each choice.
- A planner is __correct__ when the solution found leads from the initial state to the goal.
- (non important for now) A planner is __non decomposable__ when there can be interaction between subgoals (if we want to achieve more than one goal, then these goal can possibly interact).

### Execution properties
The execution is the implementation of the plan, and has always the following properties: 
- __irreversible__: often the execution of an action is not backtrackable.
- __non deterministic__: ==the plan can have effects that are different from what we expect==. Working in the real world pertains uncertainty. In some cases the plan may fail. Then we can in principle find a recovery plan from scratch or partially.

## Generative planning
It is an off-line planning that produces the whole plan before execution. It works on a snapshot of the current state.

It is based on some (often unrealistic) assumptions: 
- __Atomic time__: actions are not interruptible (non-preemption)
- __Deterministic effects__. 
- The ==initial state is a priori fully known==.  I suppose that I know everything, and everything that is not known/is not there is false (<span style="color:limegreen"><b>closed world assumption</b></span>).
- ==The plan execution is the only cause of changing in the world==. It is opposed to reactive planning. 

## PLANNING AS SEARCH
Planning can be seen as a search activity. There are many different views of planning as search, since states and operators change depending on the formalization of the problem.

- __Deductive planning as theorem proving__: states are sets of propositions that are true in a given state of the world, operators are deductive rules.

- __Planning as search in the state space__ (__LINEAR PLANNING__): states are sets of propositions that are true in a given state of the world, operators are actions.

- __Planning as search in the plan space__ (Non-linear planning): states are partial plans and operators are plan refinement/completion moves (very similar to local search).


## LINEAR PLANNING
A linear planner formulates the planning problem as a ==search in the state space and uses classical search strategies==. 

The search algorithm could proceed:
- __Forward__: the search starts from the <span style="color:yellow">initial state</span> and proceeding when it finds a state that is a ___superset of the goal___. 
- __Backward__: the search starts from the <span style="color:yellow">goal</span> and proceeds backward until it finds a state that is a ___subset of the initial state___.