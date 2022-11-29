------
##### Table of contents

##### _Lesson resources_
Slides (Modal Logics) [here](https://virtuale.unibo.it/pluginfile.php/1468211/mod_resource/content/1/04-ModalLogics.pdf)
Slides (Semantic Networks) [here](https://virtuale.unibo.it/pluginfile.php/1470117/mod_resource/content/1/05a-Semantic%20Networks.pdf) [wrong title, it shouldn't be Knowledge and Beliefs]
Slides (Intro to Description Logic) [here](https://virtuale.unibo.it/pluginfile.php/1470119/mod_resource/content/1/05b-IntroDL.pdf) 

-----

# Intro
Last time we observed that each subset of the axioms describes another logic. 
i.e.
- A0, A1, MP, and G: Logic K 
- A0–A4, MP, G: Logic S5 
- A0–A3, MP, and G: Logic S4

It up to us to decide which operators, which axioms and mix the ingredients to achieve a logic.

There are also other operators :
- B, for belief logics 
- E (everyone knows), for group knowledge 
- CK (common knowledge), for group knowledge 
- D (distribute knowledge), for group knowledge

# Linear-time Temporal Logic (LTL)

The world observed by an agent evolves through time… … the framework of modal logic has been exploited to support a qualitative representation and reasoning over the evolution of worlds along the temporal dimension
- the accessibility relation now is mapped into the temporal dimension 
- each world is labelled with an integer: time is then discrete (so the number of worlds is numerable)

Two possible ways for the world to evolve: 
- __Linear-time__: from each world, there in only one other accessible world 
- __Branching-time__: from each world, many worlds can be accessed (CTTL, computational tree temporal  logic)

### Temporal logics 
Which operators? Notice that we chose to organize time along a discrete evolution of worlds.
- Something is true at the next moment in time ◯$\varphi$

- There is a $\varphi$ that is always true in the future ☐$\varphi$ (box operators) 
	- i.e. if i cut a wire, it will remain cut for all the possible future. 

- There is a $\varphi$ that is true sometimes in the future ◇$\varphi$ 
	- i.e. i will have lunch sometime later (idk when, but i will)

- There exists a moment when $\psi$ holds and $\varphi$ will continuously hold from now until this moment $\varphi U \psi$ 
	- i.e. Chesani will continue the lesson until the lesson finishes.

- $\varphi$ will continuously hold from now on unless $\psi$ occurs, in which case $\varphi$ will cease $\varphi W \psi$  (_weak until_)

The execution of something is represented by the accessibility function. 

### Temporal Logics – semantics
The semantics is defined again starting from a Kripke structure M. 
However: 
- the accessibility relation is mapped on a linear structure, where ==each world/state is simply labelled with a natural number==… 
- … thus indicating the state or the number is the same
![[temporal_logic_semantics.png]]

An example:
![[Screenshot_20221115_093914.png]]

In this way we can represent symbolically a way of reasoning that is very similar to how humans reason irl. 

### Model Checking
LTL (but also CTL) lend themselves nicely to the modelling of system specifications that can be considered as _finite state machines_. In other words, this shit can actually be used irl. 

They proved quite effective also in __modelling distributed systems__, where different agents can exchange messages and information. 

A question arises: given a specification of a (distributed) system, is it possible to prove that some properties always/sometimes hold?
- Is it possible to prove that a certain state will never occur? (safety properties) 
- Is it possible to prove that a certain state will occur, sooner or later? 
- Is it possible to prove that my system will not end up in a loop? (livelock/deadlock)

Model Checking is a discipline that investigated the possibility of proving such formal properties. 

Many approaches and tools have been proposed, among them many tools adopted the LTL semantics. 

Many applications fields benefitted from these techniques: 
- hardware design 
- safety-critical systems 
- safety-critical processes and operations 
- security protocols

Each model __is not guaranteed to terminate since it is based on FOL__. What we can do is base ourself on __propositional logic__ instead, which is guaranteed  to be decidable. 


---------------------------------------------
# Semantic Networks

Categories are a human, powerful mechanism for addressing the knowledge representation. 

FOL is a powerful tool for describing knowledge, however sometimes it is not the easiest way for obtaining this goal. 

Historically, several different approaches have been proposed. Moreover, we have learned the possibility of creating new logics (e.g., modal logics).

__We want to represent and reason about categories and objects (and properties).__

We can do that by using Semantic Networks. 
![[semantic_network.png]]

Particular importance is assumed by the link labels, that characterize the _nature of the link_.

#### Characteristics of a semantic network
- Categories and Objects represented with the same symbol… 
- Four different types of links, with the same graphical representation 
	- Mary, John, and `SisterOf`: relation between objects 
	- Persons have 2 legs: is it referred to the category? 
	- Mammals, Persons, and `SubsetOf`: is-a like relation 

`hasMother` relation: which meaning?
- The category of Persons does not have mothers… indeed, it's a category!
- The intended meaning is more complex: each member of Persons has a mother who belongs to the category Female Persons.
- $\forall x,x \in Persons \implies \forall y hasMother(x,y) \implies y \in FemalePersons$

Summing up, there are: 
- properties about the categories as a whole 
- properties about the members of the categories

Reasoning about inheritance and properties is easy: 
- start from your object: does it exhibit the property? 
- if not, look at the category it belongs: does it exhibit the property? 
- if not, move up to the next category in the hierarchy, and check it again… 
- if needed, iterate previous step

### Semantic Networks – multiple inheritance and properties 
Things are nasty when allowing multiple inheritance… the "Nixon diamond" problem:
![[nixon_diamond.png]]
Is Nikon a pacifist? a non-pacifist? It can be both (incoherence)… It is not acceptable in logic.

OOP Languages suffer the problem, when dealing with overridden methods: 
- C++ allows for multiple inheritance, but you have to specify for an instance if you are viewing it from the perspective of one class, or another. 
- Java and C# do not allow multiple inheritance (but allow multiple interfaces).

### Semantic Networks – few limits

FOL is surely more powerful 
- negation? 
- universal and existential quantified properties?
- disjunction? 
- nested function symbols?

We could extend the graphical language of semantic networks… but at the cost of simplicity.

#### Semantic Networks – an advantage on default and overriding

Default properties can be assigned directly to the categories. 
It is immediate to specify exceptions: 
- per-categories i.e. legs(2) 
- per-instances i.e. legs(1)

# Frames
Intuitively: a frame is a piece of knowledge that describes an object in terms of its properties:
- it has a (unique) name 
- each property is represented through couples <slot – filler> (ovvero, campo + valore)
![[frame_example.png]]

In terminological approaches (DL) an object is an instance of a category on the basis of sufficient conditions (logical approach). 

In the Frames proposal, an object belongs to a category if it is similar enough to some typical members of the category, named prototypes. 
- Prototypes allow to reason about general properties of the (objects belonging to the) categories 
- Exceptions are allowed, for objects that exhibit different property values (Defeasible) 
- Default values are used when "real" values are missing 
- Links between frames are though property values

##### Frames and procedural information
Slots can contain additional information about the fillers, named facets: 
- default value 
- type 
- allowed range 
- …

More interestingly: 
- fillers can take the form of procedural attachments 
- … that can be activated by specific facets

Slots can contain additional information about the fillers: 
- fillers can take the form of procedural attachments 
- … that can be activated by specific facets 

- looking for the value of a slot: if-needed 
- adding values: if-added 
- deleting values: if-removed 

The Frames proposal can be viewed as an attempt to integrate declarative and procedural knowledge. 
