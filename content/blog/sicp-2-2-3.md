+++
tags = ["sicp"]
date = "2017-06-17T12:16:56+03:00"
title = "SICP Solutions: Section 2.2.3"
draft = true
+++

### Section 2.2.3 - Sequences As Conventional Interfaces

#### Exercise 2.37

It is suprisingly how simple this is.

```klipse-eval-scheme
(define (accumulate op initial sequence)
  (if (null? sequence)
      initial
      (op (car sequence)
          (accumulate op initial (cdr sequence)))))

(define (accumulate-n op init seqs)
  (if (null? (car seqs))
      ;; nil is no longer part of Scheme so
      '()
      (cons (accumulate op init (map car seqs))
            (accumulate-n op init (map cdr seqs)))))

(define v (list 1 2))
(define m (list (list 1 2) (list 3 4)))

(define (dot-product v w)
  (accumulate + 0 (map * v w)))

(define (matrix-*-vector m v)
  (map (lambda (row) (dot-product row v)) m))

(define (transpose mat)
  (accumulate-n cons '() mat))

(define (matrix-*-matrix m n)
  (let ((cols (transpose n)))
    (map (lambda (row) (matrix-*-vector cols row)) m)))

(matrix-*-matrix m m)
```
