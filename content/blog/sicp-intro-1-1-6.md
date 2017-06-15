+++
date = "2017-06-15T19:39:51+03:00"
title = "SICP Solutions: Introduction and section 1.1.6"
tags = ["sicp"]
draft=false
+++

# Introduction

I have been studying [Structure and Interpretation of Computer Programs](https://mitpress.mit.edu/sicp/) for
about one and an half months now. As everyone familiar with the book will know,
at least half of the work (and the knowledge!) is in its several exercises. I
started documenting my process, writing down all solutions, but wasn't sure
whether I wanted to post them here. At first I wasn't even sure I'd actually go
through with the book, but as I'm reaching section 2.2.4 I am amazed by it and
not intending to stop soon. Still, this doesn't mean I'll make it to the end,
and I'm not stressing about how long it will take. I'm here for the long haul.
It also goes without saying that you shouldn't believe everyhting you read: some
of these solutions might be incomplete or just plain wrong.

On a slightly practical note: I have been using [Racket](https://racket-lang.org) with it's [SICP
collection](compatibility layer) for the exercises. I considered using [Clojure](https://clojure.org) (which I'm
already familiar with) but decided against it, mainly because I wanted to follow
the text as closely as I could. Scheme is a very syntactically simple language
and it is no coincidence the authors chose it to teach programming.

Finally, I have used the awesome [Klipse](https://github.com/viebel/klipse) Clojurescript plugin to make all
Scheme code samples interactive. This means you can change any value and see the
changes reflected on the result space!

There are various solutions for the SICP exercises on the web, but these are
mine.

# Chapter 1

## Section 1.1.6

### Exercise 1.1

- 10
- 12
- 8
- 3
- 6
- nil
- nil
- 19
- #f
- 4
- 16
- 6
- 16

### Exercise 1.2

```klipse-eval-scheme
  (/ (+ 5 4 (- 2
               (- 3
                  (+ 6
                     (/ 4 5)))))
     (* 3 (- 6 2) (- 2 7))
```

### Exercise 1.3

```klipse-eval-scheme
(define (greatest-sum-squares a b c)
  (if (> a b)
  (+ (* a a)
  (if (> b c) (* b b) (* c c)))
  (+ (* b b)
  (if (> a c) (* a a) (* c c)))))

(greatest-sum-squares 2 3 4)
```

### Exercise 1.4

When the `if` statement is evaluated, the operatior is chosen based on whether
`b` is positive or negative, and results into calculating `a + |b|`.

### Exercise 1.5

Using applicative-order evaluation the evaluation never terminates, because as
it is trying to evaluate the operands, the algorithm falls into an infinite
loop.

Using normal-order evaluation, the algorithm does not try to evaluate `(p)`
until it actually needs it. However, the computation terminates before that, as
the `if` special form evaluates the first branch and returns `0`.
