---
title: Walsh-Hadamard Transform 
date: 2024-04-22 15:37:43
---

## Hadamard matrix

*Definition*: a square matrix whose entries are either +1 or -1, and whose rows are mutually orthogonal.

*Properties*: {% katex %} H H^T = n I_n {% endkatex %}

Sylvester's construction for the `2^n * 2^n` case:

{% katex %}
H_{2^0} = [1]

\quad

H_{2^1} =
    \begin{bmatrix}
        1 & 1 \\
        1 & -1
    \end{bmatrix}

\quad

H_{2^{n+1}} = 
    \begin{bmatrix}
        H_{2^n} & H_{2^n} \\
        H_{2^n} & -H_{2^n}
    \end{bmatrix}

\quad
or
\quad

H_{2^{n+1}} = H_{2^1} \otimes H_{2^n}
{% endkatex %}


[Hadamard conjecture](https://en.wikipedia.org/wiki/Hadamard_matrix#Hadamard_conjecture) for the `n = 4*k` case:
- its existence is still an open question.
- the smallest `n` for which no Hadamard matrix is presently known is 668.


### Hadamard gate for qutrits

From several places ([1] [2] [3]) I found the Hadamard gate for qutrits:

{% katex %}

H_3 = 
    \begin{bmatrix}
        1 & 1 &  1 \\
        1 & \omega & \omega^2 \\
        1 & \omega^2 & \omega
    \end{bmatrix}

\qquad

\omega = e^{\frac{2i\pi}{3}}
{% endkatex %}


It is not a Hadamard matrix, since {% katex %}
H_3 H_3^T = 3 \begin{bmatrix}        
        1 & 0 & 0 \\
        0 & 0 & 1 \\
        0 & 1 & 0
    \end{bmatrix}
\neq
3 I_3
{% endkatex %}

*Question*: Why is this matrix chosen to be the THadamard gate?


## fWHT examples


### Boolean function `AND`

I borrowed from these [slides](https://www.cs.rice.edu/~zz59/Zhi-Wei-Zhang/slides/LAPIS200520.pdf).

| `x1` | `x2` | `f(x1, x2) = x1 AND x2` |
| :--: | :--: | :--: |
| 1  | 1  | 1  |
| 1  | -1 | 1  |
| -1 | 1  | 1  |
| -1 | -1 | -1 |

The column vector can be used to represent the boolean functions. 4 basis functions are:

|       | `1` | `x2` | `x1` | `x1⋅x2` |
| :--: | :--: | :--: | :--: | :--: |
| `1`  | 1    | 1    | 1    | 1    | 
| `x2`  | 1   | -1   | 1    | -1   |
| `x1`  | 1   | 1    | -1   | -1   |
| `x1⋅x2` | 1 | -1   | -1   | 1    |

 
The fWHT for `AND` is:

{% katex %}

H_4 ⋅ f_{\land}

=
\begin{bmatrix}        
        1 & 1 & 1 & 1 \\
        1 & -1 & 1 & -1  \\
        1 & 1 & -1 & -1 \\
        1 & -1 & -1 & 1
\end{bmatrix}
⋅
\begin{bmatrix}
1 \\ 1 \\ 1 \\ -1
\end{bmatrix}
=
\begin{bmatrix}
3-1 \\ 3-1 \\ 3-1 \\ 1-3
\end{bmatrix}

{% endkatex %}


The `AND` function is thus "analyzed" into the following one:

{% katex %}
f(x_1, x_2) = \frac{1}{4} [(3-1)⋅1 + (3-1)⋅x_2 + (3-1)⋅x_1 + (1-3)⋅x_1 x_2]
{% endkatex %}


### Boolean function `NOT`

| `x` | `f(x) = NOT(x)` |
| :--: | :--: |
| 1  | -1 |
| -1 | 1  |

The column vector can be used to represent the boolean functions.

{% katex %}
H_2 ⋅ not =
    \begin{bmatrix}
        1 & 1 \\
        1 & -1
    \end{bmatrix}
    ⋅
    \begin{bmatrix}
    -1 \\ 1
    \end{bmatrix}
    =
    \begin{bmatrix}
    +1-1 \\ -1-1 
    \end{bmatrix}
    =
    \begin{bmatrix}
    0 \\ -2 
    \end{bmatrix}    
{% endkatex %}

So `NOT` can be analyzed into:

{% katex %}
not(x) = \frac{1}{2} [(1-1)⋅1 + (-1-1)⋅x] = -x
{% endkatex %}


Similarly, for `ID(x) = x`， `constF(x) = F`, and `constT(x) = T`, we have:

{% katex %}
H_2 ⋅ id = 
    \begin{bmatrix}
    0 \\ 2 
    \end{bmatrix} 

\qquad

id(x) = \frac{1}{2} [0⋅1 + 2⋅x] = x
\newline
{% endkatex %}


{% katex %}
H_2 ⋅ constF = 
    \begin{bmatrix}
    2 \\ 0 
    \end{bmatrix} 

\qquad

constF(x) = \frac{1}{2} [2⋅1 + 0⋅x] = 1
\newline
{% endkatex %}


{% katex %}
H_2 ⋅ constT = 
    \begin{bmatrix}
    -2 \\ 0 
    \end{bmatrix} 

\qquad

constT(x) = \frac{1}{2} [(-2)⋅1 + 0⋅x] = -1
{% endkatex %}


The composition of `not` and `id` under the other basis is (?):
{% katex %}
\begin{bmatrix} 0 \\ -2 \end{bmatrix} 
\odot
\begin{bmatrix} 0 \\ 2 \end{bmatrix}
=
\begin{bmatrix} proj_2(0,0) \\ -2 × 2 \end{bmatrix}
=
\begin{bmatrix} 0 \\ -4 \end{bmatrix}
{% endkatex %}

The composition of `not` and `constF` under the other basis is (?):
{% katex %}
\begin{bmatrix} 0 \\ -2 \end{bmatrix} 
\odot
\begin{bmatrix} 2 \\ 0 \end{bmatrix}
=
\begin{bmatrix} proj_2(0,2) \\ -2 × 0 \end{bmatrix}
=
\begin{bmatrix} 2 \\ 0 \end{bmatrix}
{% endkatex %}

The composition of `constT` and `constF` under the other basis is (?):
{% katex %}
\begin{bmatrix} -2 \\ 0 \end{bmatrix} 
\odot
\begin{bmatrix} 2 \\ 0 \end{bmatrix}
=
\begin{bmatrix} proj_2(-2,2) \\ 0 × 0 \end{bmatrix}
=
\begin{bmatrix} 2 \\ 0 \end{bmatrix}
{% endkatex %}

### Typed function `swap` (`+` type)

```text
swap :: 1 + 1 -> 1 + 1
| inl () = inr ()
| inr () = inl ()
```

| `x` | `f(x) = swap x` |
| :--: | :--: |
| `inl ()` | `inr ()` |
| `inr ()` | `inl ()` |

Two basis functions are:
```text
constL :: 1 + 1 -> 1 + 1
| inl () = inl ()
| inr () = inl ()

id :: 1 + 1 -> 1 + 1
| inl () = inl ()
| inr () = inr ()
```


The corresponding fWHT is (`l` for `inl` & `r` for `inr`):

{% katex %}
H_2 ⋅ swap =
    \begin{bmatrix}
        l & l \\
        l & r
    \end{bmatrix}
    ⋅
    \begin{bmatrix}
    r \\ l
    \end{bmatrix}
    =
    \begin{bmatrix}
    l⋅r+l⋅l \\ l⋅r+r⋅l 
    \end{bmatrix}
{% endkatex %}


*Question*: What does this mean?

Analogous to vectors:
- `l` and `r` are two vectors whose angle are π.
- `|l| = |r|` should hold.
- `⋅` is dot product.
- `+` is scalar sum.


Analyzed into:
{% katex %}
swap(x) = \frac{1}{2|l||r|} [(l⋅r+l⋅l)⋅l + (l⋅r+r⋅l)⋅x]
{% endkatex %}

if we generalize the type for `swap` a little:
```text
swap :: A + A -> A + A
| inl a = inr a
| inr a = inl a
```

There is no significant difference except the pattern-matched value `a : A`:
- `inl` and `inr` are like unit vectors with angle π.
- what does `a : A` mean here? a scalar?


### Fully generalized `swap`

```text
swap :: A + B -> B + A
| inl a = inr a
| inr b = inl b
```

| `x` | `f(x) = swap x` |
| :--: | :--: |
| `inl a` | `inr a` |
| `inr b` | `inl b` |


Two basis functions would be:
```text
b0(f) :: A + B -> B + A
| inl a = inl (f a)
| inr b = inl b
    where 
        f :: A -> B

b1(f,g) :: A + B -> B + A
| inl a = inl (f a)
| inr b = inr (g b)
    where 
        f :: A -> B
        g :: B -> A
```

This is a bit strange since they are parameterized.

The corresponding fWHT is:

{% katex %}
H_2 ⋅ swap =
    \begin{bmatrix}
        l(f(a)) & l(f(a)) \\
        l(b) & r(g(a))
    \end{bmatrix}
    ⋅
    \begin{bmatrix}
    r(a) \\ l(b)
    \end{bmatrix}
{% endkatex %}

It breaks the symmetricity. Impose `b = f(a)`?


### THadamard example

Come up with a function whose type is `3 -> 3` (similar to `NOT` whose type is `2 -> 2`):

| `x` | `f(x)` |
| :--: | :--: |
| {% katex %} 1 {% endkatex %} | {% katex %} \omega {% endkatex %} |
| {% katex %} \omega {% endkatex %} | {% katex %} \omega^2 {% endkatex %} |
| {% katex %} \omega^2 {% endkatex %} | {% katex %} 1 {% endkatex %} |

It's a one-cycle permutation. The 3 basis functions are:

| `x` | `x^0` | `x^1` | `x^2` | 
| :--: | :--: | :--: | :--: |
| {% katex %} 1 {% endkatex %} | {% katex %} 1 {% endkatex %} | {% katex %} 1 {% endkatex %} | {% katex %} 1 {% endkatex %} |
| {% katex %} \omega {% endkatex %} | {% katex %} 1 {% endkatex %} | {% katex %} \omega {% endkatex %} | {% katex %} \omega^2 {% endkatex %} |
| {% katex %} \omega^2 {% endkatex %} | {% katex %} 1 {% endkatex %} | {% katex %} \omega^2 {% endkatex %} | {% katex %} \omega {% endkatex %} |

The fWHT for this function `f` is: 

{% katex %}
H_3 ⋅ f =
    \begin{bmatrix}
        1 & 1 &  1 \\
        1 & \omega & \omega^2 \\
        1 & \omega^2 & \omega
    \end{bmatrix}
    ⋅
    \begin{bmatrix}
    \omega \\ \omega^2 \\ 1
    \end{bmatrix}
    =
    \begin{bmatrix}
    \omega + \omega^2 + 1 \\ \omega + 1 + \omega^2 \\ \omega + \omega + \omega
    \end{bmatrix}
{% endkatex %}

This function `f` is analyzed into:

{% katex %}
f(x) = \frac{1}{3} [(\omega \oplus \omega^2 \oplus 1)⋅1 + (\omega \oplus 1 \oplus \omega^2)⋅x + (\omega \oplus \omega \oplus \omega) ⋅ x^2]
{% endkatex %}

What does this mean?


[1]: https://dergipark.org.tr/tr/download/article-file/838937
[2]: https://pennylane.ai/qml/demos/tutorial_qutrits_bernstein_vazirani/
[3]: https://arxiv.org/pdf/2003.04879.pdf