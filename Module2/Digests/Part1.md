# Ontologies
An ___ontology___ is a formal, explicit ===description of a domain of interest===.
Us humans use ontologies all the time. In general, we tend to organize our knowledge through 2 notions:
- Categories
- Objects 

## Upper ontologies 
An ___upper ontology___ is an ontology that focuses ==on the largest, more general, more high level, most general purpose domain==.

At least two characteristics of upper ontologies: 
- they ___should be applicable to (almost) any special domain___ 
- we should be able to combine _different general_ concepts ==without incurring in inconsistencies== (e.g., time and space concepts should work together…)

### How should we represent categories and objects?
- A category can be represented as a _predicate_ or as an _object_ itself.
- In categories, we can express these concepts:
	- Membership ($\in$)
	- subclass ($\subset$)
	- members of categories have some properties
	- members of categories have these properties to be recognized as such
- Other, _important_, relations:
	- __Disjointness__: $Disjoint(s) \iff \forall c_1, c_2. c_1 \in s \land c_2 \in S \land c_1 \ne c:2 \implies c_1 \cap c_2 = \emptyset$
	- __Exhaustive Decomposition_: $ExhaustiveDecomposition(s) \iff (\forall i, i \in C \iff \exists c_2, c_2 \in s \land i \in c_2)$
	- __Partition__: $Partition(s,c) \iff Disjoint(s) \land ExhaustiveDecomposition(s,c)$

### Physical Composition
Objects can be made of other objects… the relation between an object and its parts is called ___meronymy___: some objects (___meronyms___) are part of a whole (___holonym___).

- PartOf: Part(Engine, Car)
	- Transitivity
	- reflexitivity
- BunchOf: BunchOf({Bread1, bread2})
	- a structure in the composition relation is missing

### Things & Stuff
- a category of objects that will include in its definition _only_ intrinsic properties is a substance; ==Stuff is the most general ___substance___ category==.
- a category of objects that will include in its definition _any_ extrinsic property is a count noun; ===Thing is the most general ___object___ category===.


# Reasoning over time
