# Question 1 (Prolog)
Develop `inOddPosition(L1, L2)` that is true when L2 is the list containing the elements of the list L1 that are positioned in odd positions. 
For example:
```
?- inOddPositions([2, 4, 6, 8, 10, 12], X)
yes, X = [2, 6, 10]

?- inOddPositions([42], X)
yes, X = [42]

?- inOddPositions([], X)
yes, X = []

?- inOddPositions([1, 2, 3], [1, 3]])
yes

?- inOddPositions([1, 2, 3], [1])
no
```

## Answer 
Simplest solution:
```
inOddPositions([], []).
inOddPositions([H | L], [H | L2]) :- !, notOdd(L, L2).
notOdd([], []).
notOdd([_ | L], L2) :- !, inOddPositions(L, L2).
```
Less intuitive solution:
```
inOddPositions([], []).
inOddPositions([H | L], [H | L2]) :- 
    length(L, Y),
    0 is mod(Y, 2), !,
    inOddPositions(L, L2).
inOddPositions([_ | L], L2) :- 
    inOddPositions(L, L2).
```


# Question 2
The candidate is invited to describe the predicates/terminology used in the definition of the Event Calculus Framework.

## Answer 

# Question 3
Within the terminological approach towards the representation of concepts/categories and individuals, the candidate is invited to illustrate the notions of:
- Disjointness over a set S of categories (S = {c1, c2, ..., cn}, where c1,...,cn are categories)
- Exhaustive decomposition of a category c into a set S of categories. 
- Partition of a category c into a set of categories S
The candidate is invited to illustrate the notions through a simple example. 

## Answer 
- __Disjointness__: $Disjoint(s) \iff \forall c_1, c_2. c_1 \in s \land c_2 \in S \land c_1 \ne c:2 \implies c_1 \cap c_2 = \emptyset$
- __Exhaustive Decomposition__: $ExhaustiveDecomposition(s) \iff (\forall i, i \in C \iff \exists c_2, c_2 \in s \land i \in c_2)$
- __Partition__: $Partition(s,c) \iff Disjoint(s) \land ExhaustiveDecomposition(s,c)$

Examples:
1) Animal and Vegetables are two disjointed categories. 
2) Professor and Students contain all elements of the categories "People in a class"
3) Professor and Students are also partitions of "People in a class", since a student can't be a professor and viceversa. 
