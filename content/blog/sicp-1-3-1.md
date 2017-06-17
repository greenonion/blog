+++
date = "2017-06-17T11:15:15+03:00"
tags = ["sicp"]
title = "SICP Solutions: Section 1.3.1"
+++

## Section 1.3 - Formulating Abstractions With Higer-Order Procedures

### Section 1.3.1 - Procedures as Arguments

#### Exercise 1.29

Here is the implementation of Simpson's Rule. Note that we defined `h` as a
procedure with no arguments since we haven't learned about `let` yet.

```scheme
(define (integral-simon f a b n)
  (define (h) (/ (- b a) n))
  (define (y k) (* (f (+ a (* k (h))))
                   (cond ((or (= k 0) (= k n)) 1)
                         ((even? k) 2)
                         (else 4))))
  (* (sum y 0 inc n)
     (/ (h) 3.0)))

```

And running it we can see that it converges faster than the previous solution:

```scheme
> (integral-simon cube 0.0 1 100)
0.24999999999999992
> (integral-simon cube 0.0 1 1000)
0.2500000000000003
```

#### Exercise 1.30

```scheme
(define (sum-iter term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (+ result (term a)))))
  (iter a 0))
```

#### Exercise 1.31

`product` is very similar to sum:

```klipse-eval-scheme
(define (inc n) (+ 1 n))

(define (product term a next b)
  (if (> a b)
      1
      (* (term a)
         (product term (next a) next b))))

;; We can use it:

(define (factorial n)
  (product identity 1 inc n))

(define (wallis n)
  (define (term a) (if (even? a)
                       (/ (+ a 2) (+ a 1))
                       (/ (+ a 1) (+ a 2))))
  (* (product term 1.0 inc n) 4))

;; Let's try
(wallis 100)
```

And we can write the iterative version:

```scheme
(define (product term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (* result (term a)))))
  (iter a 1))
```

#### Exercise 1.32

This is a recursive version of `accumulate`:

```scheme
(define (accumulate combiner null-value term a next b)
  (if (> a b)
      null-value
      (combiner (term a)
                (accumulate combiner null-value term (next a) next b))))
```

We can use it to build `sum` and `product`:

```scheme
(define (sum term a next b)
  (accumulate + 0 term a next b))

(define (product term a next b)
  (accumulate * 1 term a next b))
```

And we can write the iterative version:

```scheme
(define (accumulate combiner null-value term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (combiner result (term a)))))
  (iter a null-value))

```

#### Exercise 1.33

We just skip the `combiner` if the predicate is not satisfied:

```scheme
(define (filtered-accumulate f combiner null-value term a next b)
  (if (> a b)
      null-value
      (if (f a)
          (combiner
           (term a)
           (filtered-accumulate f combiner null-value term (next a) next b))
          (filtered-accumulate f combiner null-value term (next a) next b))))
```

Then we can define the procedure that generates the sum of squares of primes
between `a` and `b`, and the procedure that generates the product of relative
primes less than `n`:

```scheme
(define (sum-square-primes a b)
  (filtered-accumulate prime? + 0 square a inc b))

(define (product-relative-primes n)
  (define (relative-prime? a) (= (gcd a n) 1))
  (filtered-accumulate relative-prime? * 1 identity 1 inc (- n 1)))
```

