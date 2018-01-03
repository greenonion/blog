+++
date = "2017-08-28T19:32:00+03:00"
tags = ["sicp"]
title = "SICP Solutions: Section 1.3.4"
+++

### Section 1.3.4 - Procedures as Returned Values

#### Exercise 1.40

`cubic` will create and return procedures that should be solved using Newton's
method:

```scheme
(define (cubic a b c)
  (lambda (x) (+ (cube x)
                 (* a (square x))
                 (* b x)
                 c)))
```

#### Exercise 1.41

Defining `double` is quite simple:

```scheme
(define (double f)
  (lambda (x) (f (f x))))
```

All the fun comes when trying to calculate something like
`((double (double double)) inc)`.

`(double double)` essentially returns a procedure that applies the original
prodecure 4 times. Therefore `(double (double double))` will apply what it
receives receives $4 * 4 = 16$ times. So if we start from $5$:

```scheme
> (((double (double double)) inc) 5)
; => 21
```

#### Exercise 1.42

```scheme
(define (compose f g)
  (lambda (x) (f (g x))))
```

#### Exercise 1.43

```scheme
(define (repeated f n)
  (if (= n 1)
      f
      (repeated (compose f f) (dec n))))
```

#### Exercise 1.44

Using the definition of `dx` from above:

```scheme
(define (smooth f)
  (lambda (x) (/ (+ (f (- x dx)) (f x) (f (+ x dx)))
                 3.0)))

(define (n-fold-smoothed f n)
  (repeated (smooth f) n))
```

#### Exercise 1.45

So the general function would be something like:

```scheme
(define (root-n x n damps)
  (fixed-point-of-transform
    (lambda (y) (/ x (expt y (- n 1))))
    (repeated average-damp damps)
    1.0))
```

The parameter `damps` controls the number of repeated average damps.
Experimenting a bit showed that with 2 average damps we can calculate up to the
7th root. 3 average damps lets us calculate up to the 31st root. Using 4 damps
we were able to calculate at least up to the 512th root with no problems.

#### Exercise 1.46

We first define `iterative-improve`. Note that it returns an anonymous procedure
that itself creates and executes a recursive procedure:


```scheme
(define (iterative-improve good-enough? improve)
  (lambda (guess)
    (define (iter last-guess)
      (if (good-enough? last-guess)
        last-guess
        (iter (improve last-guess))))
    (iter guess)))
```

Now we can implement `sqrt` and `fixed-point`:

```scheme
(define (sqrt x)
  ((iterative-improve
    (lambda (guess)
      (< (abs (- (square guess) x)) 0.001))
    (lambda (guess)
      (average guess (/ x guess))))
   1.0))

(define (fixed-point f first-guess)
  ((iterative-improve
    (lambda (guess)
      (< (abs (- guess (f guess))) tolerance))
    (lambda (guess) (f guess)))
   first-guess))
```

This concludes Chapter 1. Onwards to Chapter 2 then!

