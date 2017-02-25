+++
date = "2017-02-25T01:06:25-06:00"
title = "Personal Notes from Lecture 1B"
categories = ['Notes']
tags = ['Notes']

+++

# 02. Procedures and Processes

> Sussman enters...

### What is it that programmers do?

Design processes which achieve particular goals. Such as finding the square roots of numbers, or anything else. The way a programmer does this is by constructing spells. These are built out of instructions and expressions. In order for a programmer to do this effectively, they have to understand the relationship between the particular tasks that he writes, and the behavior of the process that they're attempting to control.

What we're going to attempt to do in this lecture is establish that connection in the clearest way possible. Particularly, to understand how patterns (of procedures and expressions) cause particular patterns of execution (particular behaviors) from the processes.

Simple?

``` scheme
;; a simple program... compute two squares
(DEFINE (SOS X Y)
  (+ (SQ X) (SQ Y)))

(DEFINE (SQ X)
  (* X X))

;; (SOS 3 4) ----> 25
```

**_How does that happen?_**

If we're going to understanding processes as a mapping, from procedures to behavior, we'll need to construct a formal (or semi-formal) mechanical model, whereby we understand how an actual machine could do this. In fact, this is an engineering model.

Enter the first model we'll be working with:

## The Substitution Model

For our purposes, this model will work in the short term. Like other kinds of models, it is more of an allegory, not so much based on actual fact, as it is something which points our thinking in a way which is more or less correct. Eventually, we'll need a more accurate model which will expose more detail.


### Kinds of Expressions
_things like..._
- Numbers
- Symbols
- Lambda Expressions *
- Definitions
- Conditionals
- Combinations

Lambdas, definitions and conditionals are going to be ignored for now. They are special forms, with particular rules for each. However, we will learn a general rule for _every_ case. Many will just evaluate to themselves.

With the model we'll be given, the symbols will disappear.

So, we really just need to get at how **_Combinations_** are defined.

```raw
        ## SUBSTITUTION RULE ##

To evaluate an application
  Evaluate the operator to get procedure
  Evaluate the operands to get arguments
  Apply the procedure to the arguments
    Copy the body of the procedure
      substituting the arguments supplied
      for the formal parameters of the
      procedure.
    Evaluate the resulting new body.

```

> If we don't understand something, we should be very mechanical and apply this rule to the procedure we are evaluating.


#### An example (sum of squares)

``` scheme
(SOS 3 4)
```

What does that mean?

- SOS :: some procedure
- 3 :: a number, but we can't repeat it (it's abstract). 3 is the numerical representation.
- 4 :: also a number

We substitute, `3` for `X` and `4` for `Y` within the _body_ for the procedure, `SOS`, as it is defined:
``` scheme
(+ (SQ X) (SQ Y))
```

The body corresponds to a combination, namely, Addition.

Thus, the code example would _reduce_ (a reduction step) to:
``` scheme
(+ (SQ 3) (SQ 4))
```

Next, we have to evaluate this. First, the operands and then the procedure. The order doesn't matter. We'll then apply the procedure (`+`) -- we're not going to open up the `+` procedure and look at it, just trust that the answer will be the result of applying as an operator.

These procedures will then further be substituted, but this time according to the formal definition of the `SQ` procedure.

``` scheme
(+ (SQ 3) (* 4 4))
```

Again, we could open up the `*` operation, but we won't and just consider it _primitive_. We could note here that at any level of detail, if you were to look inside of this machine (the Scheme Machine), you would see multiple levels below a given procedure (machine code, byte code, etc...).

> The key to understanding complicated things is to learn what _not_ to look at, and what _not_ to think, and what _not_ to compute.

``` scheme
(+ (SQ 3) 16)
(+ (* 3 3) 16)
(+ 9 16)
;; 25
```

We should look at this substitution model _religiously_. It will come up again and again.

Since order does not matter what order we substitute formal parameters for actual operands, and body definitions with actual expressions, we could devise another model. Indeed their are 'normal order' models which do this. However, since the Scheme Machine works by evaluating the procedure to get the operator (line 2) and then evaluating the arguments to get the operands (line 3), we'll work the problems in that way.

In the long wrong, there are some reasons why you might pick one order over another.

### Rules for Conditionals

``` scheme
(IF <predicate>
    <consequent>      ;; if the predicate is true
    <alternative>)    ;; if the predicate is false
```
As a formal definition:

```raw
To evaluate an IF expression
  Evaluate the predicate expression
    if it yields TRUE
      evaluate the consequent expression
    otherwise
      evaluate the alternative expression
```

Let's illustrate this in the context of a particular program.

``` scheme
;; Sum of X and Y, done by Peano Arithmetic

(DEFINE (+ X Y)
  (IF (= X 0)
    Y
    (+ (-1+ X) (1+ Y)))) ;; -1+ == decrement operator; 1+ == increment
```

## Segment 2!

We now have a reasonable mechanical model for _how_ a program built out of procedures and expressions "evolves" a process. Let's now try to build up an intuition about how _particular_ programs evolve particular processes, and their shape.

#### Visualizing

Can we imagine a resulting image, just by looking at the components of which it is made? Can you imagine what a final image will look like, given on the facts about particular film, aperture and lighting conditions?

> I have so many pens in my pocket.

#### "Test strips"

Examining a range of items with a few of variants that might give us a contrast to study.

``` scheme
;; === Peano Arithmetic ===
;; Two ways to add whole numbers:

(define (+ x y)
  (if (= x 0)
    y
    (+ (-1+ x) (1+ y))))

(define (+ x y)
  (if (= x 0)
      y
      (1+ (+ (-1+ x) y))))

```

These two programs are very similar, the only different being **_where_** we've placed the increment. Two illustrate these, we'll rewrite these two programs while we also _evole_ the process.

``` scheme
(define (+ x y)
  (if (= x 0)
      y
      (+ (-1+ x) (1+ y))))

;; -> (+ 3 4)
(+ (-1+ 3) (1+ y))
;; -> (+ 2 5)
(+ (-1+ 2) (1+ 5))
;; -> (+ 1 6)
(+ (-1+ 1) (1+ 6))
;; -> (+ 0 7)
;; -> 7

```

For the other program:

``` scheme
(define (+ x y)
  (if (= x 0)
      y
      (1+ (+ (-1+ x) y))))

;; (+ 3 4)
;; (1+ (+ (-1+ 3) 4))
(1+ (+ 2 4))            ;; apply the + operation again
(1+ (1+ (+ 1 4)))       ;; ...and again
(1+ (1+ (1+ (+ 0 4))))  ;; Ah! we can just return 4 as the consequent
(1+ (1+ (1+ 4)))
(1+ (1+ 5))
(1+ 6)
;; 7
```

**These two processes have very different shapes.**

The first one, we might notice, has a directed-ness that feels somewhat straight. Like an arrow pointing downward. The right boundary does not vary particularly.

The second almost looks like a pyramid turned sideways. The increments sort of fan out, and the recede later, as we build up and then evaluate operations.

We can think of these steps as occurring in time. And the number of steps as being approximate to the amount of time that they take to execute. Conversely, there is a dimension of space analogous to the width, or amount of memory that the process is going to take up.


#### Linear **Iteration** process

- Time = O(n)
- Space = O(1)

The `time` of the process is on the _Order of x_, it is proportional to X by some constant proportionality (and we're not concerned what that proportionality is). The `space` complexity is constant -- it is proportional to `1`.

Thus this tells us a bound, (any machine could perform this in constant space). This _algorithm_ represented by this procedure is executable in constant space.

The model is ignoring certain things, like bigger numbers take up more space... and some other things.


#### Linear **Recursion** process

Our example was that second program. It turns out that the time complexity grows as a proportion of its input, rather than the number of items.

- Time = O(x)
- Space = O(x)

> What's the essence of this matter? This matter is not so obvious. Maybe their are other models for which we can talk about the differences between iterative and recursive processes. The two examples we gave are both recursive definitions. They both refer to the thing being defined within the process itself, but they lead to differently shaped processes. Note that there is nothing special about the fact that the definition is recursive, that leads to a recursive process.

## Another model: Bureaucracy

> Let's say there's someone called GJS ()
