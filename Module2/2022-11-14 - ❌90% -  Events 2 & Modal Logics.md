-------
#### Table of contents


#### _Lesson resources_
Slides (AsTimeGoesBy) [here](https://virtuale.unibo.it/pluginfile.php/1461735/mod_resource/content/2/03-AsTimeGoesBy.pdf).
Slides (ModalLogics) [here](https://virtuale.unibo.it/pluginfile.php/1468211/mod_resource/content/1/04-ModalLogics.pdf).

------


[15 minutes]

# Events...
Event Calculus is based on the notion of events, or, better saying, on the happening of things. 
- Are these events instantaneous? 
- Or do they have a duration?

Before, how do we measure the passing of time?
- We need an origin 
	- E.g.: 1 January 1970 
- we can choose a measurement unit 
	- E.g.: seconds 
- points in time are measured w.r.t. (with respect to) distance from the origin

The question is not trivial: there are pro&cons for both the choices. However, there is a consensus that duration-based representation of events is richer. Notice: • a durative event can be represented in terms of start and end time points • an instantaneous event has duration zero

In 1984 James Allen proposed to reason about intervals, rather than point in times:
- an interval `i` begins at a certain time point: we introduce the function `Begin(i)` that return that timepoint 
- an interval `i` ends at a certain time point: `Ends(i)`.

Allen proposed 13 temporal operators, but some of them are simply the dual version of the others. 

###### Here are the temporal operators:
- Meet(i,j) ⇔ End(i) = Begin(j) 
- Before(i, j) ⇔ End(i) < Begin(j) 
- After(j, i) ⇔ Before(i, j) 
- During (i, j) ⇔ Begin(j) < Begin(i) < End(i) < End(j) 
- Overlap(i, j) ⇔Begin(i) < Begin(j) < End(i) < End(j) 
- Starts(i, j) ⇔ Begin(i) = Begin(j) 
- Finishes(i, j) ⇔ End(i) = End(j) 
- Equals(i, j) ⇔ Begin(i) = Begin(j) AND End(i) = End(j)

These are definition in logical representation.
![[Allen_graphical_representation.png]]
![[allen_graphical_time_2.png]]


--------
# Knowledge and Beliefs (Modal Logics)

#### Motivation
- Agents are in a world, and they might have incomplete knowledge about it. FOL can't represent  partial knowledge. 
- Each agent might have a different knowledge. 
- Up to know, no way for an agent to reason upon its own knowledge, or the knowledge of another agent. 
	- i.e., I can't  represent  the sentence "I know that I don't know" in FOL.

FOL is powerful for describing knowledge, however it is missing useful shortcuts for dealing with concepts such as knowledge, belief, etc. 

Moreover, agents act in a world that evolve along time. How to reason upon such evolution?

With FOL I also can't represent partial logics. 

#### Agent in our enviroment
So far, we discussed about representing knowledge. 

Implicitly, we adopted this point of view:
- our agent/program/decision system acts in a certain environment 
- knowledge about the environment is available 
- our agent take decisions/plan on the basis of the knowledge

It is a "godlike" pov.  Humans are not rational, and they do not attain to rules. 

The viewpoint of an agent with KB has some limits.:

In the environment there are other agents
- Success of any decision depends on the actions taken by other agents 
- These others agents might have their own knowledge… 
- … knowing something about the others might help to take our own decisions? 

Our agent has a KB, but it does not know it:
- no distinction between knowledge, belief, perception, … 
- __no possibility of any meta-reasoning on its own knowledge__. A professor knows that he knows its subject. We are missing the truth regarding our meta-reasoning. 

All of this is not possible in FOL. 

#### Propositional attitudes 
How to represent propositional attitudes? Using FOL predicates? 
- they refer to something that is not a term 
	- `Knows(Federico, location(pizza, fridge) )`
	- `location(pizza, fridge)` is usually a sentence, not a term 
- FOL predicates might support referential transparency 
	- `Knows(Lois, CanFly(Superman) )` 
	- `Superman=Clark` 
	- Does Lois knows that Clark can fly? [no, since he does not know that he is superman ]

Propositional attitudes behave differently from predicates 
- using FOL predicates is not the best choice ("does not work")

We need to introduce new operators.

## Modal operators 
The idea was to introduce new, special operators, named __modal operators__, that behaves differently from logical operators.

So, we introduce a new family of logics. __Modal logics__ is a family of logics. 
- same syntax as FOL 
- augmented with modal operators
- modal operators take as input sentences (rather than regular terms)
- each modal operator takes _two inputs_: 
	- the name of an agent, and the sentence it refers to
- Semantics is extended with the notion of possible worlds
- Possible worlds are related through an accessibility relation (that might depend on the operator)
- Different operators define different logics

Example: operator $\textbf{K}$ for indicating that agent $a$ knows $P$:
$$
\textbf{K}_aP
$$
- Federico knows there is a pizza in the fridge 
	- $\textbf{K}_{federico}location(pizza,fridge)$ 
- Federico knows that Elena knows that there is a pizza in the fridge 
	- $\textbf{K}_{federico}\textbf{K}_{elena}location(pizza,fridge)$


## Semantics of Modal Logics
[missed these 5 minutes]
The starting point: an agent in a given scenario is not omniscient regarding all the aspects of the current state of the world (the current world). Thus, an agent might think: • there is the current perception of the state of the world • the unknown is depicted as possible worlds, with an intuitive notion of possibility or accessibility from my current world We are not really interested on a specific notion of "accessibility": it might suffice that there is such a notion. The idea is something like: an agent knows p if in any accessible world accessible from the current, p is true.

##### Formally...
Given a set of primitive propositions $\phi$ , a Kripke structure $M=(S, \pi, K_1,\dots,K_n)$ is defined: 
- $S$ is the set of states of the world 
- $\pi: \phi → 2^S$ specifies for each primitive proposition the set of states at which the proposition hold 
- $K_1, \dots, K_n$ are binary relations over $S$, i.e. $K_i \subseteq S \times S$, with the meaning: $(s,t) \in K_i$ if agent i consider the world t possible from s. 

> [!warning ]
> The binary relations $K$ are DIFFERENT from the Modal operator $\textbf{K}$ , even though Chesani used a similar symbol for both of them. 
> 
> THEY ARE COMPLETELY DIFFERENT CONCEPTS. 

- Don't get confused by the accessibility/possibility relation: we are not really interested in it

[10 minutes lost]

### Semantics of Modal Logics – an example
[..]
M=(S, �, Ka, Kb): • Φ = ������, ℎ���,���� • Three stages, five worlds � = {�#, ℎ$,�$, ℎ%,�%} • � ������ = ℎ$,�$, ℎ%,�% • � ℎ��� = {ℎ$, ℎ%} • � ���� = {�$,�%} • �& = �, � � ∈ �} • �' = �, � � ∈ �} ∪ { ℎ$,�$ , �$, ℎ$ } Finally, we can establish the truth of sentences like: �, �# ⊨ �&¬������ ∧ �'�&�'(¬ℎ��� ∧ ¬����) �, �$ ⊨ ℎ��� ∨ ���� ∧ ¬�'ℎ��� ∧ ¬�'���� ∧ �' �&ℎ��� ∨ �&����
$

### Properties of the knowledge modal operator
Which are the axioms of our logic? It's up to us to add the ones we deem as useful / necessary / important / whatever.

###### A0 & MP
__Axiom A0__: All instances of propositional tautologies are valid
__MP Modus Ponens__ (Natural deduction): if $\varphi$ is valid and $\varphi$ => $\psi$ is valid, then $\psi$ is valid. 
- Natural Deduction  is not always true


###### A0 & MP
What about the knowledge operator K w.r.t. the modus ponens? (Distribution Axiom) A1: �"� ∧ �" � ⇒ � ⇒ �"� in other words, knowledge is closed under implication. But… A1 assumes quite powerful agents!!!


The definition so far of knowledge assumes quite powerful agents… 
Notice: 
- if $\varphi$ is true in all the worlds of a structure M (i.e., $\varphi$ is valid)… 
- … then $\varphi$ must be true, in particular, in all the worlds $(M, s)$ that agent i considers as possible. 
- If $\varphi$ is true in any possible world, then agent i knows $\varphi$ (by definition) in all the possible worlds.

(_Knowledge Generalization Rule_) __G__: for all structures M, if $M ⊨ \varphi$ , then $M ⊨ K_i\ \varphi$ .

In other words, our agent knows all the tautologies. This of course holds for tautologies only… i.e., $\varphi  ⇒ K_i \ \varphi$ is not valid (unless we axiomatize it).

Properties of the knowledge modal operator Which is the relation between knowing something and the truth value of that? (Knowledge Axiom) A2: If an agent knows � , then � is true �"� ⇒ � This is considered to be the central property of Modal logic for knowledge. (valid only if the possibility/accessibility relation is reflexive) What about belief? This axiom, in belief logic, does not hold. It is usually substituted by: A2': ¬�" �����


_What about the own knowledge of an agent?_

- (Positive Introspection Axiom) A3: $K_i \varphi ⇒ K_i K_i \varphi$ 
- (Negative Introspection Axiom) A4: $¬K_i \varphi ⇒ K_i ¬K_i \varphi$ 
 
The agent might have a partial knowledge of the world (hence the possible worlds), but ==with these axioms we get sure that at least it has perfect knowledge about its own knowledge==.

In short, we have all these axioms to our disposition:
- A0
- MP
- A1
- G
- A2
- A3
- A4

Shall we take all of them? Do we need all of them? If not, which one will we keep?