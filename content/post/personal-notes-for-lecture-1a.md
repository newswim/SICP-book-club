+++
date = "2017-02-25T00:41:02-06:00"
title = "Personal Notes from Lecture 1A"
categories = ['']
tags = ['lectures']

+++

### Some wild build up to 'What is it that programmers are doing?'

- It is intended to help us learn how to think

---

Some time passes, I pick up notes around 0:35:00

### A General Framework (for learning a computer language)

- What are the primitive elements?

- What are the means of combination?

>>>> What are the ways of putting things together?

- What are the means of abstraction?

>>>> How do we take those primitive things and
     build something complicated with them?


### _Some primitive data in LISP..._

``` scheme
3
```

The number 3.

However, if we're being very pedantic, it's not even the number Three itself, it's some Platonic idea of 'what Three means'.

Still, it's a 3.

``` scheme
17.4
```

Some representation of 'Seventeen Point Four'

``` scheme
5
```

``` scheme
+
```

A name for the primitive method of adding things.

A name for Plato's concept of how you add things.

#### What is the sum of these numbers?

``` scheme
( + 3 17.4 5 ) ;; -> 25.4
```

This is called a **Combination**.

A Combination consists, in general, of applying an operator to some opperands.

``` scheme
( + 3 17.4 5 )
  ^ ^-^----^
   \     \_____
    operator   \
                operands
\_____________/
  combination
```
> Note: I'm using ` scheme` for code highlighting.. (it's a lisp after all)


From these principles, we have enough to form more complex combinations.

The reason that we can get this complexity, is because the operands themselves can be combinations.

``` scheme
( + 3 ( * 5 6 ) 8 2 ) ;; -> 43
```

#### Little bit on Syntax

LISP uses **prefix** notation, meaning the operator is written to the left of the operands. It is also _fully parenthesized_, and the parenthesis are there to make expressions written in the language less ambiguous.

In mathematics, parentheses are often used to mean grouping, and it's okay to sometimes leave them out, however in Lisp, you cannot leave out parenthesis, and you can not arbitrarily add them.

Putting in parenthesis always means precisely "this is an operation." (eg. apply an operator to operands).

Really, we're writing trees.

``` scheme
;; it's hard to make ascii trees.

        +       ;; operator
       / \
      /   \
     /     \
    1       *
   / \     / \
  0   +   9   +
 /   / \     / \
2   1   7   8   8

```

### This is called `tree accumulation`

>>> Really whats going on are we're writing trees, and parentheses are just a way to write a two-dimensionally structure as a linear character string.

### Let's ASK LISP !!

``` scheme
(+ 3 4 8)
;; 15
```

HOLY SHIT!

``` scheme
(+ (* 3 (+ 7 13.5)) 4)
;; 54.5
```

my god.

``` scheme
;; indentation, "pretty printing"

(+ (* 3 5)
   (* 47
      (- 20 6.8))
   12)
;; 647.4 (or something)
```

Wow. Well, those are the primitives, eg. the Means of Combination. Let's move on to...

### The Means of **Abstraction**

We use `DEFINE`,

``` scheme
(DEFINE A (+ 5 5))

(+ A A) ;; -> 625

(DEFINE B (+ A (* 5 A)))
B ;; -> 150

(* A (/ B 5)) ;; -> 55
```

Let's say we want to name the general term for, 'multiplying a number by itself' -- how would this be written?

``` scheme
(DEFINE (square x)
  (* x x))

;; use it in a sentence..

    TO   SQUARE SOMETHING
(DEFINE (square x)
  (* x x))
   ^ ^ ^
MULT IT BY ITSELF
```

sort of yes!

> A notation for defining a procedure

However, it's still kind of confusing. It's not totally clear that you're _naming something_.

``` scheme
(DEFINE SQUARE (LAMBDA (x) (* x x)))
```

Here, we're clearly naming something `SQUARE`.
- `Lambda` is Lisp's way of saying 'make a procedure'

In general, we'll be using the more concise, first form. However try to keep in mind that to the Lisp interpreter, there is no difference between the two, and in fact, what we're asking semantically is closer to the second form.

``` scheme
(define square
  (lambda (x) (* x x) ))
    ;; MAKE A PROCEDURE
          ;; WITH THE ARGUMENT 'X'
              ;; THAT RETURNS THE RESULT OF MULTIPLYING X BY X
```

In this 1986 lecture, the author defines this ability
for an interpreter to accept either form, with one
being more human-readable as... (drum roll)

### Syntactic Sugar

"Having somewhat more convenient surface forms for typing something"

``` scheme
(DEFINE (SQUARE X) (* X X))   ;; -> SQUARE
(SQUARE 1001)                 ;; -> 1002001
(SQUARE (+ 5 7))              ;; -> 144
(+ (SQUARE 3) (SQUARE 4))     ;; -> 25

;; We can also use it in something more complicated.

(SQUARE (SQUARE (SQUARE 1001)))     ;; 10050010002...

;; And, if we just ask the definition..

SQUARE
;; #[COMPOUND-PROCEDURE SQUARE]
```

#### Two more examples of defining

``` scheme
(define (average x y)
  (/ (+ x y) 2))

(define (mean-square x y)
  (average (square x)
           (square y)))
```

Having defined `average` and `square`, we can start talking about the 'mean-square' of something.

Having defined `SQUARE`, it can be used _AS IF_ it were a primitive. It doesn't matter whether it was something built into the language, or something that we ourselves have defined.

This is a key thing in Lisp.

> The things you construct have all the power and flexibility as if they were primitives.

``` scheme
+
;; #[COMPOUND-PROCEDURE +]
```

You should not be able to tell, in general, between things that are built in, and things that are compound. Why? Because the things that are compound have been wrapped in an abstraction.

### Example: Absolute Value Function

This is what's called a *Case Analysis*

```math
          { -x  for  x < 0 }
abs(x) =  {  0  for  x = 0 }
          {  x  for  x > 0 }
```

``` scheme
(DEFINE (ABS X)
  (COND ((< X 0) (- X))       ;; clauses for COND
        ((= X 0) 0)
        ((> X 0) X)))
```

- `-` is a subtraction OR negation operator (depending on # args)

``` scheme
(define (abs x)
 (if (< x 0)
     (- x)
     x))
```

This form is essentially the same. Either can be thought of as syntactic sugar.

### Which/When of `DEFINE`

You might use `DEFINE` slightly differently depending on whether or not you're defining a procedure.

``` scheme
(DEFINE (SQUARE X) (* X X))
;; or
(DEFINE SQUARE (LAMBDA (X) (* X X)))
```

The first is a kind of special, but probably more common case, for defining procedures. The second can be thought of as 'Define the _symbol_ `SQUARE`'

Yup. // TODO: wtf.

NOTES:
- are there any structural differences between the two?
- are there any concerns to do with memory, performance, etc.?
- is the first one a Lambda function?

> A little more on that whole thing...

``` scheme
;; this is sort of -definition- vs -expression-

(define a (* 5 5))

(define (d) (* 5 5))

;; when used, results..

a       ;; -> 25
d       ;; -> compound-procedure d
(d)     ;; -> 25 (same as evocation with 0 args)
(a)     ;; -> error , (25 is not an operator that can be applied)

(* a a) ;; -> 25

```

### The Heron of Alexandria example

```raw
TO FIND AN APPROXIMATION TO SQRT(X)
- Make a guess  (G)
- Improve the guess by averaging G and X/G
- Keep improving the guess until it is good enough
- Use 1 as an initial guess
```

A method of successive averaging. Let's write it in Lisp!

but wait.. what it is mean 'to try a guess for the square root of x'?

``` scheme
(DEFINE (TRY X)
  (IF (GOOD-ENOUGH? GUESS X)
  GUESS
  (TRY (IMPROVE GUESS X) X)))
```

The next part states, in order to compute square roots..

``` scheme
(DEFINE (SQRT X) (TRY 1 X))
```

Ok but, how is a guess good enough? How do we improve a guess?

``` scheme
(define (improve guess x)
    (average guess (/ x guess)))

(define (good-enough? guess x)
    (< (abs (- (square guess) x))
    .001))
```

- To improve, we average the guess with the quotient of dividing `x` by the `guess`.
- To check if it's good enough, one way is to ask, 'when I take this guess and square it, then subtract x, do I get a small enough number left over?'

#### Let's look at the structure of that a little bit...

- `SQRT` - some type of module, that is defined in terms of `TRY`, etc.

```raw
            `SQRT`
              |
            `TRY`
            /    \
    GOOD-ENOUGH?  IMPROVE
       /     \        \
     ABS    SQUARE    AVG
```

But wait.. `TRY` is defined in terms of `GOOD-ENOUGH?` and `IMPROVE`, but also in terms of `TRY` again. It's definition includes its own implementation, which may make a geometer gag.

But in a way, it must know what Trying is, in order to define what it is doing.

We can write down what this means..

``` scheme
(SQUARE 2)
..
(TRY 1 2)
..
(TRY (IMPROVE 1 2) 2) ;; average of 1 and 2/1 -> 1.5

...
(TRY 1.5 2)
..
(TRY (AVERAGE 1.5 (/ 2 1.5)) 2)
..
(TRY 1.3333 2)
...
(TRY 1.41667 2)
...
;; -> 1.41421568
```

this is constantly _reducing_, in a sense.

Good enough.

#### Recursive Definition (is what that is)

- Allows for infinite computations that go on until something is true.

``` scheme
(define (sqrt x)
    (define (improve guess)
        (average guess (/ x guess)))
    (define (good-enough? guess)
        (< (abs (- (square guess) x))
        .001))
    (define (try guess)
        (if (good-enough? guess)
            guess
            (try (improve guess))))
    (try 1))
```

- `improve`, `good-enough?` and `try` are all packages inside of the `SQRT` module.

This is called _Block Structure_.

#### Summarize

- What are we doing?
> Expressing imperative knowledge.

|                      | Procedures              | Data   |
|----------------------|-------------------------|--------|
| Primitive Elements   | + * < =                 | 4 15.6 |
| Means of Combination | () composition  COND IF |        |
| Means of Abstraction | DEFINE                  |        |


#### COMING UP!

- _How it is_... that you make a link between the procedure and the processes within the machine.
- _How it is_... that you use the power of Lisp to talk not only about little computations, but about more general, conventional methods of doing things.


> tree accumulation.

## `COND`

### Logical Operators

``` scheme
(define (square-sum-larger a b c)
	(cond ((and (< a b) (< a c)) (+ (* b b) (* c c)))
        ((and (< b a) (< b c)) (+ (* a a) (* c c)))
        (else (+ (* a a) (* b b)))))

(square-sum-larger 1 2 3)
;; 13
(square-sum-larger 13 6 4)
;; 205
```


``` scheme
; Original Guess-And-Square method of finding Square Roots

(define (square x)
  (* x x))

(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x)
                 x)))

(define (good-enough? guess x)
  (< (abs (- (square guess) x)) 0.001))

(define (average x y)
  (/ (+ x y) 2))

(define (improve guess x)
  (average guess (/ x guess)))

(define (sqrt x)
  (sqrt-iter 1.0 x))

(sqrt 8)

```
