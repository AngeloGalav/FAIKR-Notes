# What is an Ontology?
An ___ontology___ is a formal, explicit ===description of a domain of interest===.
- __formal__: it should be described using a language with clear and non-ambiguous semantics.
- __explicit__: the information should be derivable in a finite time with a sound procedure.
- __description__: it should provide us information… more interesting, the information it provides defines the goal of the ontology itself.
- __domain__: such description should be related to some topic of interest, and in turn it should be related to the ontology goal.

Us humans use ontologies all the time. In general, we tend to organize our knowledge through 2 notions:
- Categories
- Objects 

Essentially:
- there can be categories which are more or less general
- categories  can also be organized hierarchically

On the other hand, objects:
- can be instances of categories
- can belong to more categories at the same time.

# Upper ontologies 
As we've have seen, categories can be more or less general. 
An ___upper ontology___ is an ontology that focuses ==on the largest, more general, more high level, most general purpose domain==.

At least two characteristics of upper ontologies: 
- they ___should be applicable to (almost) any special domain___ 
- we should be able to combine different general concepts ==without incurring in inconsistencies== (e.g., time and space concepts should work together…)

Roughly speaking, at least four different approaches to upper/general ontologies:
1. Ontologies created by philosophers/logician/AI researchers
2. Ontologies created by automatically extract knowledge from already well-structured datasets
3. Ontologies created by text documents (i.e. NLP)
4. By using crowd-sharing information

## Categories and objects: how to represent them?
For objects: it is safe to assume there always exists ==a way to identify each of them, a __unique name__==. 
As for categories, we have two possible alternatives:

1. ___categories as predicates___ e.g.: Car is a predicate that represents the category of cars; Car(aa123bb) means that the vehicle with plate aa123bb is a car.
2. through reification: ==categories are objects as well==.

In categories, we can also express these concepts:
![[other_properties_categories.png]]

Other relations:
- ___Disjointness___: every animal could not be a vegetable, and the opposite. We would say that Animals and Vegetables are two disjointed categories (although they are subclass of LivingThing).
![[categories_disjointness.png]]
- ___Exhaustive Decomposition___: Subcategories might "cover" all the possible instances of the parent category. 
	- E.g.: Every one in this classroom is a student, or a professor (or both). "Student" and "Professor" categories provide an exhaustive decomposition of the category "People in this classroom".
![[ExhaustiveDecomposition.png]]
- ___Partition___: If a category can be decomposed in more categories such that: 
	- they form an exhaustive decomposition 
	- they are disjointed 
Then we have a partition.
Formally, given a category c, and a set s of categories, s is a partition of c if:
![[partition.png]]

# Physical Composition
Objects can be made of other objects… the relation between an object and its parts is called ___meronymy___: some objects (___meronyms___) are part of a whole (___holonym___).
E.g.: a car is composed of an engine, four wheels, five seats,…

_Is there any structural relation between the parts, and between the parts and the whole (other than the composition itself)?_ •
-  yes: the relation is usually named ___PartOf___ 
	- E.g.: PartOf(engine123,car_aa123bc), PartOf(cylinder1345, engine123)
- no: the relation is usually named ___BunchOf___
	- E.g.: BunchOf({Bread1, Bread2, Bread3})

#### PartOf
The PartOf relation enjoys some properties:
- transitivity 
- reflexivity always holds.
![[properties_of_partof.png]]
#### BunchOf 
The _BunchOf_ relation aims to define objects in terms of composition of other countable objects, however, ===a structure in the composition relation is missing===.

We don’t use the _Set_ paradigm because it’s a mathematical concept and properties in general cannot be propagated to the Set concept
- i.e. the weight of the box of nails is given by the sum of the nails contained in the box… the weight property of each nail has a relation with the weight of the box. The weight of each nail is constrained within the weight of the box. 

## Measures
Among the many types of properties, objects can have measures, and specifically:
- ___quantitative measures___: something that can be measured in a quantitative manner, w.r.t. some unit
	- usually represented using a _unit function_ that takes in input a number. E.g.: length(table1) = cm(80).
	- properties expressed in quantitative measures can/usually propagate upwards/downwards in the PartOf/BunchOf relations. E.g.: the total weight of a car is given by the sum of the weights of its parts. 
	
- ___qualitative measures___: something that can be measured using terms, and these terms come with an total/partial order relation. E.g.: this lessons is {devastatingly boring, just boring, puzzling, amusing, fantastic}.
	- they do not propagate downward/upwardE.g.: two boring lessons in the same day do not make the day twice boring 
	- the presence of an order relation however allow their use in object/category definitions

What if we really need to make quantitative reasoning over qualitative measures?
<span style="color:lime">Fuzzy Logic</span> provides a (many-values-truth) semantics to qualitative measures.

### Things or Stuff?
Till here, categories and objects were about things that can be individuated, or distinct from each other. Whether explicitly or implicitly:
- There is some individuation criteria
- mainly referred to the PartOf/BunchOf relation
- mainly related to the substance of the object itself
- logical minimization criteria

There are objects in our real world that defy the individuation criteria. In English: • count nouns:
- car, apple, human 
- mass nouns: water, air, butter 

In the Cyc ontology, and Russel&Norvig as well, they adopt the distinction between: 
- Things 
- Stuff

- some properties are referred to the substance of the object. E.g.: the water boils at 100°C Such properties are named ___intrinsic___, and they are retained even when some division is applied.
- some properties refer to the object, and to the object structure relation with its parts. E.g.: the weight of professor's car change if we take out a wheel. Such properties are named ___extrinsic___.

- a category of objects that will include in its definition _only_ intrinsic properties is a substance; ==Stuff is the most general ___substance___ category==.
- a category of objects that will include in its definition _any_ extrinsic property is a count noun; ===Thing is the most general ___object___ category===.