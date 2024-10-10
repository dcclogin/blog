---
title: From bastardized relativism to the failed absolute
date: 2024-08-07 17:44:19
tags:
  - Philosophy
  - Logics
---

*There are only (relative) opinions, no (absolute) truths.* 

It is the prevailing ideology nowadays. But does itself count as an ‘opinion’ or ‘truth’? [Auguste Comte](https://en.wikipedia.org/wiki/Auguste_Comte) from 19th century regards it as an absolute truth: *Nothing is absolute, except the fact that everything is relative.* [Quentin Meillassoux](https://en.wikipedia.org/wiki/Quentin_Meillassoux) has another version: *The laws of nature have no necessity, except that it is necessary that they be contingent.*

Not surprisingly, this **Universal-Exception Model** serves as the framework of contemporary (vague, 1st ordered) democracy -- everything can be decided by votes and parliament, except the democratic apparatus/mechanism itself (which is to much degree guaranteed by ‘constitution’ or ‘consitutive exception’). For some reason, I will call this model another name ‘non-reflexive relativism’ or ‘bastardized relativism’.

If we otherwise, unlike Comte, regard it as an opinion: *Nothing is absolute, including this line itself.* Then we will have a **Non-All Model**, where no totalizing exception can be found. I will call this model another name ‘reflexive relativism’ or ‘inconsistent relativism’. Why ‘inconsistent’? Here I refer to the very fundamental lesson of formal logic that in order to maintain ‘consistency’ or ‘meaning’, there should be at least one exception (usually refered to as *false*, or non-appearance, something stands for nothing). 

One can immediately relate to formula of sexuation by [Jacques Lacan](https://en.wikipedia.org/wiki/Jacques_Lacan). With proto set theoretic symbols, ‘proto’ as it is not axiomatic, we can express these two models as follow:

- non-reflexive relativism/set: `X ∉ X`, e.g. `X = {opinion_1, opinion_2, opinion_3}`.
- reflexive relativism/set: `X ∈ X`, e.g. `X = {opinion_1, opinion_2, opinion_3, X}`.

### Detour 1: set theory and two laws of classical logic

In set theory, given two sets `A` and `B`, there are only two possibilities in terms of the relation `∈`: either `A ∈ B` or `¬(A ∈ B)`. In other words, it is an absolute partition with respect to immanence/belonging. There is no uncanny/spooky ‘bi-location’ or ‘non-location’:
- bi-location: *both* `A ∈ B` *and* `¬(A ∈ B)` -- violation of *law of non-contradiction*.
- non-location: *neither* `A ∈ B` *nor* `¬(A ∈ B)` -- violation of *law of excluded middle*.
- both bi-location and non-location: violation of both laws.

Assuming the classical negation in these formulae, the first two cases lead to paraconsistent logic and intuitionistic logic, respectively. In some sense, we can say the intrinsic logic of set theory is the classical one.

### Detour 2：reformulating the paradox

If `A` and `B` are identical, then the only two possibilities in terms of `∈` become either `X ∈ X` or `X ∉ X`.
Given a set `X`, it is either a non-reflexive set or a reflexive set. The *absolute* partition guarantees no bi-location or non-location cases.

The question arises: *Is the set of all non-reflexive sets reflexive or non-reflexive?*

If we name the interesting ‘set of all non-reflexive sets’ `NR`, and define the corresponding predicate `P(X) = X ∉ X`, we then have the following reasoning chain:

```text
X ∈ NR ⇔ P(X) = X ∉ X
```

Substituting `X` with `NR`:

```text
NR ∈ NR ⇔ NR ∉ NR
```

The formal contradiction indicates the very location of `NR` can not be consistently expressed with the reflexive-or-not partition. In other words, `NR` is an *indivisible remainder* of such partition. `NR` is the real *Other* from the perspective of the classical logic intrinsic to set theory.

As [Alain Badiou](https://en.wikipedia.org/wiki/Alain_Badiou) well articulated: *The Other emerges from an impasse of the Same.* Here Same refer to the Order claiming that every set can be safely categorized/reduced. 

There are lots of consequences following the paradox. The most ‘formal’ one says that you cannot pass directly from a predicate (`P(X) = X ∉ X`) to its extension (`X ∈ NR`), one corollary of which leads to the [axiom of separation](https://en.wikipedia.org/wiki/Zermelo%E2%80%93Fraenkel_set_theory#3._Axiom_schema_of_specification_(or_of_separation,_or_of_restricted_comprehension)) in ZFC set theory.

I would like to insist on the consequence called ‘failure of absolute partition’ (named by myself). Failure to measure (the property of) the Absulute epistemologically. Here I would also risk referring to [Kantian](https://en.wikipedia.org/wiki/Immanuel_Kant) agnosticism with respect to the Thing-in-itself.

### Failed absolute and beyond

There is nontheless something that serves as the ‘Whole’ background/framework. That is the *Absolute* (`A`), i.e. *the set of all sets* (in Lacan's term, the Big Other, or the Other of Other). The Absolute qua set of all sets has to include itself, thus reflexive (`A ∈ A`) and *Non-All*.

The absolute partition above is ultimately a partition of the Absolute. The very fact that the Absolute has a determinate position (categorized as a reflexive set) while `NR ∈ A` doesn't, is the index of a *failed absolute*, since the absolute is transcended by its element/part. The move from ‘failure of absolute partition’ to *failed absolute* mimics the move from Kant to Hegel.

The dialectic suggests not only a rethinking of logics other than the classical one (as logicians and computer scientists did in 20th century), but also a rethinking of topology (the problem of immanence).

Personally I would risk trying to formulate logics able to account for something like ‘`A` appears in the guise of `B`’, ‘spy of `A` on `B`’ at an ontological level.