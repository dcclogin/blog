---
title: The Sonnet of CPS
date: 2023-03-01 01:56:24
tags: CPS
---


This is derived from a CPSed interpreter.
If you understand so called 2-level lambda notations (in Olivier Danvy's old papers), then it's trivial.

```racket
(define cps
  (λ (expr C)
    (match expr
      [(? symbol? x) (C x)]
      [`(λ (,x) ,body)
       (C `(λ (,x k) ,(cps body (λ (v) `(k ,v)))))]
      [`(,rator ,rand)
       (cps rator
          (λ (v₀)
            (cps rand
               (λ (v₁)
                 (let ([v (gensym 'v)])
                   `(,v₀ ,v₁
                         (λ (,v) ,(C v))))))))]))) 
```
