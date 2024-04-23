---
title: Transfinity, stream, and recursion
date: 2023-08-02 12:01:36
tags:
  - Philosophy
  - Mathematics
  - Zizek
---

As you might see, this cannot be a worse title. But recently I was inspired by Zizek's interpretation on Kant's transcendental objects and Lacan's *objet petit a*, in his book *Less Than Nothing*, to make these many things connected in a gradual sense.

To begin with, one will assume a counting from zero. With 'set representation', one can write down:
```text
{0, 1, 2, ...}
```

You might think it's the set of natural number, the aleph-naught, or something similar. It's not yet informative to that extent. Symbolically, one can argue that `...` is too fuzzy. It nontheless means the "constant possibility of `+1`", as if the magical infinity "spits out" one but itself still remain intact.
```text
{...}
{0, ...}
{0, 1, ...}
{0, 1, 2, ...}
```

It's fascinating to think there is a "point of impossibility", an inaccessable `X` beyond all finite ones, lying at the 'end' of the series:
```text
{0, 1, 2, ..., X}
```
The impossible/inaccessable `X` within the set can be viewed as a 'transcendental one perceived empirically'.

Now we have a pair of ideas opposite to each other:

- the constant possibility of adding one
- the impossibility to access the ultimate element `X`

What if there is a **short-circuit** between them? What if the impossible `X` itself is the constant possibility of adding one, represented as `{...}` (and any other representations with `...` inside like `{0, 1, 2, ...}`)?

With one more step forward, one can write this down carefully:
```text
{0, 1, 2, ..., {0, 1, 2, ...}}
```

It's exactly a counting from zero to one. One can continue adding the symbol `X` in the nested set, and unfold it further:
```text
{0, 1, 2, ..., {0, 1, 2, ..., X}}
{0, 1, 2, ..., {0, 1, 2, ..., {0, 1, 2, ...}}}
```

The "constant possibility of adding one" is now inscribed into the level where the unfolding goes on. To make it more concise, if one takes `{0, 1, 2, ..., X}` as a totality of "constant possibility of adding one", and `=` as the short-circuit operation:
```text
X = {0, 1, 2, ..., X}
```

It is the transfinity.

### Stream

A simple variant of the above result would be like:
```text
X = (1, X)
X = (1, (1, X))
X = (1, (1, (1, X)))
```

It's a stream of `1`s. In Racket, with `delay` or `lambda`, one can define such a stream:
```scheme
(define 1s (cons 1 (delay 1s)))
(define 1s (cons 1 (lambda () 1s)))
```

Stream is no more than transfinity. It's a "constant possibility of unfolding", a stream of empirical objects, by definition. But one may not easily relate an arbitrary stream to the impossibility of reaching a ultimate element (transcendental object) within itself. One can regard the stream itself as an 'external framework', or computationally, a 'generator' of all positive and empirical objects.


### Fixpoint and recursion

Another more general variant of the above result is something called 'fixed point' or 'fixpoint' (recall 'point of impossibility'):
```text
X = F(X)
```
where `F` is some mathematical or computational function. Here the transfinity lies in, at the most obvious level, the constant possibility of applying the function `F`:
```text
X = F(X)
X = F(F(X))
X = F(F(F(X)))
```

One can say it's too general and has nothing informative. There is nontheless one tiny tweak to be done to make it powerful - what if `X` is also indexed by another variable, that is, `X` is some `g(x)`?
```text
g(x) = F(g(x))
```
This is a recursion without 'termination conditions', with `F` as the 'wrapper' of the recursive call to `g`. To make it practical, one needs to specify the very data type of `x` and create at least one termination condition, for example:
```text
g(0) = 0
g(x+1) = F(g(x))
```
Racket version:
```scheme
(define (g x)
  (cond
    [(zero? x) 0]
    [else (F (g (sub1 x)))]))
```

A recursion is a 'quasi-transifinity' with an empirical 'ultimate object'. It always returns half-way during the impossible journey to the transfinite.


Question:
- How is this story related to Y combinator (fixpoint combinator)?
- How to explain the computational power of recursive functions?
- Relation to co-induction?


### More foundational symbolic operations: toward Lacan, and beyond Cantor

**Gaze** and **Silence** are two symbolic mechanisms familiar to continental schools. To show them properly, I have to refer to Lacanian theory (the Big Other and the objet petit a). There are two ways to illustrate them, one in set-theoretical style, another in categorical way (you can have Yoneda at hands).

Suppose we have a (finite) collection/set, say `{0, 1, 2, 3}`. The critical idea here is: identity is not free - i.e. one cannot justify `0`'s identity `0 = 0` simply by its own **presence**. Instead, it's very identity should be obtained through it's **absence** and **difference** from *other* elements - i.e. one can say `0 = {1, 2, 3}`, `1 = {0, 2, 3}`...

Note the italic *other* here. The **Otherness** seems to be more explicit and tangible in category theory. But let me first introduce the Big Other under set theory - `A` is defined as:

```text
A = {0, 1, 2, 3, A}
```

In other words, `A` is something like the set itself, while it's also part of the set. It has the ability to unfold itself unboundedly (**repetition**). Compared to any "usual others" like `0`, `A`'s identity is indeed a self-referential tautology (the Master's Signifier). One can easily imagine its political counterpart - people with various identities "issued" by a special agency, who pretends to be just a "usual element". One should not miss a dialectical tension here - only by degrading itself into the commonplace can it impose an effective rule:

```text
A = {0, 1, 2, 3}
```

This is an imaginary-idiot tyrant which is clearly not the case for modernity. But rather, it's a trick played everywhere - call someone a tyrant, and others gaze it out, then they fight within the constitutive-illusion.

Now, one cannot help but ask the question: how does the Big Other successfully "hide" itself? The answer is - by objet petit a - `a` is defined as:

```text
a = {a, A}
```

It says: `A` has its own master, which is `a`. Just like `A` can repeatedly unfold itself, so does `a` here. As a result, `A` is as common as the usual others. But is this true? Unfolding of `a` only generates a dull sequence of `A` which is suspicious enough. 

Here the proper catch is: where does this mysterious `a` come from? Is it really something behind `A`? The answer is: from the commonplace elements - i.e. one element from `{0, 1, 2, 3}` is "sacrificed" to play the role as seemingly transcendental `a`. One should recall Hegel's famous line "essence is the appearance *qua* appearance", here the essence is the illusionary signal that there is an `a` beyond `A`.

To sum up, **Gaze** is the `=` operation in `0 = {1, 2, 3}`, `1 = {0, 1, 2}`...(Probably a good question to think about: what is the gaze from the Big Other?)


In category theory, the Yoneda Embedding shows *the gaze proper*:

```text
X ↦ Hom(_,X)
```

This says an object "itself" is no more than an empty letter. Its determination is by the set of all arrows (gazes) coming from *other* objects.


Questions:
  - How to locate the Big Other in a category?
  - How about the objet petit a?

