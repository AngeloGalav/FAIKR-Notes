Usually, we communicate each other using Natural Language. Natural language is subject, by its own nature, to ==ambiguity==. 

We represent sentences using symbols. 
To establish a link between ideas and symbols, we use ___interpretations___.

An <span style="color:yellow">interpretation</span> $I$ is a pair (D, I) where: 
- D is any set of objects, called domain 
- I is a mapping called the <u>interpretation mapping</u>, from the (non logical) symbols to elements and relations over D. 

Given a set of sentences, ==their truth value will change depending on the interpretation we will give to the symbols==: there is a strict relation between the notion of interpretation and the truth value.

### The idea of Logical Consequence
To avoid the problem of the many different interpretations of symbols, we will restrict, and derive from the notion of truth (something more related to philosophy) the notion of ___Logical Consequence___. 

Let $\Gamma = F_1 ... F_n$ be a set of sentences (hypotheses or premises), and $F$ be a sentence (conclusion).

$F$ is a ___logical consequence___ of $\Gamma$, written $\Gamma \vDash F$, if for any possible different interpretation, ==it is always true that if all the formulas in are true (in the world under examination) then also is true==.

### Propositional Logic 
Propositional logic is the simplest logic: it illustrates basic ideas using propositions $P_i$ 
- Each $P_i$ is referred to as an atom or atomic formula.
- Atoms Pi can be connected together by connectives, such as:
$$
\lor, \land, \rightarrow, ¬, \leftrightarrow, \bot
$$
A ___well-formed formula___ is: 
- an atomic formula $P_i$
- if A and B are well-formed formulas, then A $\land$ B, A $\lor$ B, A $\rightarrow$ B, and ¬A are well-formed formulas.

Each atom $P_i$ can only be either true or false.
