---
title: "SICP Solutions: Section 2.2.1"
date: 2018-03-09T20:06:52+02:00
tags: ["sicp"]
draft: false
---

## Section 2.2 - Hierarchical Data and the Closure Property

### Section 2.2.1 - Representing Sequences

#### Exercise 2.17

```scheme
(define (last-pair items)
    (if (null? (cdr items))
        items
        (last-pair (cdr items))))
```

#### Exercise 2.18

This is the iterative version:

```scheme
(define (reverse items)
    (define (reverse-iter i l)
      (if (null? i)
          l
          (reverse-iter (cdr i) (cons (car i) l))))
    (reverse-iter items nil))
```

We can use `append` to create a recursive version:

```scheme
(define (reverse items)
    (if (null? items)
        nil
        (append (reverse (cdr items)) (list (car items)))))
```

But this will be more expensive because `append` will iterate over each subset.

#### Exercise 2.19

The three procedures are really simple actually, just renaming the
list-operating procedures.

```scheme
(define (first-denomination coin-values)
  (car coin-values))

(define (except-first-denomination coin-values)
  (cdr coin-values))

(define (no-more? coin-values)
  (null? coin-values))
```

The order of list `coin-values` does not affect the answer produced by `cc`.
This is because the procedure is not based on any assumption regarding the order
of the elements.

#### Exercise 2.20

```scheme
(define (same-parity . i)
    (define (iter-parity f l)
      (cond ((null? l) l)
            ((f (car l)) (cons (car l) (iter-parity f (cdr l))))
            (else (iter-parity f (cdr l)))))
    (cond ((null? i) nil)
          ((even? (car i)) (iter-parity even? i))
          ((odd? (car i)) (iter-parity odd? i))))
```

#### Exercise 2.21

```scheme
(define (square-list items)
  (if (null? items)
      nil
      (cons (* (car items) (car items))
            (square-list (cdr items)))))

(define (square-list items)
  (map (lambda (x) (* x x)) items))
```

#### Exercise 2.22

Louis' solution is essentially how we implemented `reverse` for exercise 2.18.
Each iteration creates something like `(cons 9 (cons 4 (cons 1 nil)))`,
which is exactly how lists are implemented.

Unfortunately the second solution is no good either, because it is the inverse
of how lists are implemented, and results into something like
`(cons (cons (cons nil 1) 4) 9)`. The order is correct, but this isn't how
Scheme structures lists from `cons` cells.

#### Exercise 2.23

```scheme
(define (for-each proc items)
  (cond ((null? items) nil)
        (else (proc (car items))
              (my-for-each proc (cdr items)))))
```

