---
title: Overdetermination of the symbol =
date: 2024-04-22 15:36:02
---


The symbol `=` is seen in every corner of logics, mathematics, and programming languages. Let us take a tour on how this specifc symbol is used and referred to, and see how "overdetermined" it currently is. Starting from basic formal logic - specifically the Law of Identity, loaded with Mathematical categories like equivalence relations, isomorphisms, and so on, finally toward practices and theories in and about programming languages.

### Determination of `=` in logics

In classical formal logic, the [Law of Identity](https://plato.stanford.edu/entries/identity/) says:

{% katex '{"displayMode": true}' %} 
A = A  
{% endkatex %}

***Remarks***
- There is no further determination of `A` except it's an empty letter thats holds an intentionality toward some "logical proposition", and has *dual occurrences*.
- There is a short-circuit between the arbitrariness of the letter and the universally-quantifiability of the proposition.
- Intentionality toward *other* letters (potentially `B`, `C`, `D` and so on) is not explicit.
- In addition to holding the identity of `A`, `=` reflects an internal tension of `A`. 

There is already a minimum level of contradiction in the dual occurrences of `A`. Questions arise naturally: What's the *difference* between the `A` in lhs and the `A` in rhs? If `A` is self-consistent (or self-persistent), then for what and by what it has to identify with itself?

Such identity is *free* (with no cost). There is no locativity and linearity, let alone temporality and further physicality. More about Identity is discussed in another [post](./negativity).


### Determination of `=` in mathematics

The most familar and basic determination of `=` in mathematics is as "equivalence relation", which satisfies:

- **Reflexivity**: {% katex %} A = A {% endkatex %}
- **Symmetricity**: {% katex %} A = B {% endkatex %} iff {% katex %} B = A {% endkatex %}.
- **Transitivity**: if {% katex %} A = B {% endkatex %} and {% katex %} B = C {% endkatex %}, then {% katex %} A = C {% endkatex %}.

For now let us suspend any references to foundations of mathematics like Set Theory.

### Determination of `=` in PL

In modern programming languages, we can write programs like:

```haskell
let f = \x -> (x, x) in
  let a = 42 in
    let r = f a in
      r
```

Where `=` intuitively means "binding" or "assignment". Clearly, none of three (reflexivity, symmetricity, transitivity) are applicable to the `=` above. This is despite tons of "features" of PL that keep overdetermining the symbol `=`.




### Intrusion of locativity

The triad `(A before, A after, identification/alienation of A)`. 

Minimum level of spatiality (or temporality, causality?).

See [Jean-Yves Girard]().

(under construction)

### As postulate, or *alsob*

Ad hoc postulation. Reduction to a common third. 

(under construction)

### As exchange, or transaction

Exchange of commodity. Linear Logic.

(under construction)

### The dialectic of identity

(under construction)


### References

1. https://www.youtube.com/watch?v=0mrC0kIr_aQ