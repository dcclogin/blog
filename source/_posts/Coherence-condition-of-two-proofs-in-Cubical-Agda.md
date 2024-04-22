---
title: Coherence condition of two proofs in Cubical Agda?
date: 2023-03-01 00:17:43
tags: HoTT
---


In Cubical Agda, there are multiple ways to prove a property of paths. For example, the most straightforward way to prove symmetricity is to use the primitive operator `~` on intervals:


```agda
≡-symm : ∀ {ℓ} {A : Type ℓ} {a b : A} → a ≡ b → b ≡ a
≡-symm {ℓ}{A}{a}{b} p = λ i → p (~ i)
```

But there are at least two more ways to prove the same thing. One you may consider is to use `hcomp`, and set two egdes of a square (2-d cube) to `refl`:

```agda
≡-symm₁ ≡-symm₂ ≡-symm₃ : ∀ {ℓ} {A : Type ℓ} {a b : A} 
                        → a ≡ b → b ≡ a
≡-symm₁ {ℓ}{A}{a}{b} p i = hcomp walls a
  where
    walls : ∀ j → Partial (∂ i) A
    walls j (i = i0) = p j
    walls j (i = i1) = a

≡-symm₂ {ℓ}{A}{a}{b} p i = hcomp walls (p i)
  where
    walls : ∀ j → Partial (∂ i) A
    walls j (i = i0) = p j
    walls j (i = i1) = p (~ j)

≡-symm₃ {ℓ}{A}{a}{b} p i = hcomp walls b
  where
    walls : ∀ j → Partial (∂ i) A
    walls j (i = i0) = b
    walls j (i = i1) = p (~ j)
```

You may also use *coercion*. The key is to construct a *type line* from `refl : a ≡ a` to something of type `b ≡ a`. Basically, you need to fix the right endpoint of a path, and let the left endpoint "slide" along some known path, and here it is `p : a ≡ b`:


![](symm-coe.png)

```agda
coe0→1 : ∀ {ℓ} (A : I → Type ℓ) → A i0 → A i1
coe0→1 A a = transp (λ i → A i) i0 a

≡-symm : ∀ {ℓ} {A : Type ℓ} {a b : A} → a ≡ b → b ≡ a
≡-symm {ℓ}{A}{a}{b} p = coe0→1 (λ i → p i ≡ a) refl
```

Now we have many syntactically different proofs of path symmetricity, can we ask what is the *coherence condition* of where these proofs are considered "equivalent"? What about other proofs like path transitivity?


### (Continued)

So far we know that:

1. The [CCHM](https://arxiv.org/pdf/1611.02108.pdf) paper shows that `transp` can be expressed as a special case of `comp`. 
   ```text
   Γ ⊢ (transpⁱ A a) = (compⁱ A [] a) : A(i1)
   ```

2. The [Cartesian Cubical Type Theory](http://www.cs.cmu.edu/~cangiuli/talks/defense.pdf) shows `hcom` can be expressed by `coe`.

3. The [Cubical Agda](https://github.com/agda/cubical/blob/master/Cubical/Foundations/CartesianKanOps.agda) shows `coe` and be implemented with `transp`.


To do:

- Prove (1) and (2) in Cubical Agda. 
