## A brief history

Scheme was created during the 1970s at the MIT AI Lab and released by its developers, Guy L. Steele and Gerald Jay Sussman.

Scheme programming language is the language used in the famous "wizard book" SICP (
[Structure and Interpretation of Computer Programs](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book.html)) taught in MIT.
Many people have learned Scheme when they learned SICP. Scheme is suitable for teaching programming because of its simplicity.

We strongly recommend this book for you to learn Scheme and CS foundations. However, the Scheme implementation in LambdaChip may not contain the complete
syntax and APIs, because we concern about the firmware size and performance in embedded system.

## The Scheme standard specification

The standardized specification of Scheme is called RnRS which stands for "Revised n Report on the Algorithmic Language Scheme".
[The latest standard is R7RS which is the 7th version](https://small.r7rs.org/attachment/r7rs.pdf).

The R7RS separates it to two language specifications, the large language and the small language.
The r7rs-small language was designed for teaching & research, and its minimalism is suitable for embedded system.
The Scheme on LambdaChip aims to be r7rs-small compatible, and it's extended as a super-set of r7rs-small to control peripherals.

## Prefix expression

Scheme uses prefix expression that the operators are written before their operands:
```scheme
(func x y)
(+ 1 2)
(display "hello world\n")
```
It's easy to understand, the leftmost element in the parenthesis must be a function. In this article, we always call the operator a "function".

## Lambda is your best friend

Functional programming is all about functions. In Scheme, a function can be defined with *lambda*:
```scheme
(define add (lambda (x y) (+ x y)))

;; Usually, we can write it in short:
(define (add x y) (+ x y))
```

In functional programming, function is the first-class citizen, so you can pass a function as an object:
```scheme
(define lst (map (lambda (x) (* x 2)) '(1 2 3 4 5)))
(display lst)
```
Either you can return a function as an object:
```scheme
(define (func x)
  (lambda (y) (+ x y)))
```
And you can call apply it as a function:
```scheme
(define z (func 123))

(display (z 5))
;; => 128
```
The object returned by **func** is the so-called **closure**, which is an object storing a function together with an environment. In this, case the value of x was bound into the closure object, basically, it's stored into the environment of the closure when it's captured. So that's why it will plus the bound x (the value is 5) with the argument 123, which is bound to y.

## To Be Continued ...
