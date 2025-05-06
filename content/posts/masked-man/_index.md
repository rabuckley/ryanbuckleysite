---
title: "The masked man misunderstanding"
description: "Why the masked man fallacy is not a fallacy."
date: 2023-01-20T12:00:00+00:00
layout: single
draft: true
---

Descartes' argument for dualism is [often boiled down to](https://www.jstor.org/stable/2253091) the following argument

```
I can doubt the existence of my body
I cannot doubt the existence of my self
---
My self is not my body
```

This seems to follow using [Leibniz's Law](https://plato.stanford.edu/entries/identity-indiscernible/):

> if, for every property _F_, object _x_ has _F_ if and only if object _y_ has _F_, then _x_ is identical to _y_.

or in other words:

> if x and y are distinct then there is at least one property that x has and y does not, or vice versa

This means the following argument is valid:

```
x has F
y does not have F
---
x is not y
```

And Descartes' argument also fits this form:

```
My body has the property that I can doubt its existence
My self does not have the property that I can doubt its existence
---
My body is not my self
```

Getting to the first premise is a straight-forward substitution of "my body" for `x` and "that I can doubt its existence" for `F`, which is a property of `x`.

Getting to the second premise takes a bit more work. The general form requires that `y` (in this case, "my self") does not have property `F`. The original argument instead contains a _second property_ (`F1`): "that I cannot doubt its existence". As long as you accept dubitability and non-dubitability are negations then `F1` is the same as `not F`, so if `y` has `F1` then `y` must also have `not F` by definition.

## The masked man fallacy

However, the following argument also has the same form:

```
I know who my father is
I do not know who the man in the mask is
---
My father is not the man in the mask
```

You do not know who the man in the mask is, so it seems fallacious to conclude that it cannot possibly be my father. This is the masked man fallacy.

In the general form:

```
My father has the property of being known to me
The man in the mask does not have the property of being known to me
---
My father is not the man in the mask
```

In this form, it still looks wrong to conclude that the man in the mask is not my father. The general form, however, looks valid.

The problem arises from the definition of "known". I "know" my father in the sense that I have a relationship with him. My knowledge of the man in the mask, however, is relating to my possession of _information_ about him.

Using the informational definition of "known" for both, knowing who my father is, and not knowing who the man in the mask is _are exclusive_. In other words, if I am unsure whether the man in the mask is my father, then _I do not know who my father is_, which contradicts the first premise.

Using the definition of "known" relating to relationship, the two premises are, again, exclusionary. I know my father, as in I have a father-son relationship with him. I do not know the man, as in I don't have a relationship with him. It does not matter who the man is, that he is wearing a mask, or anything else other than that I do not know him. Again, if my father were also the man in the mask then the premises would be contradictory.

To be clear, it absolutely _is_ fallacious to argue that you know your father socially, and don't know who the man in the mask is epistemically (or vice versa), because these are two different uses of the word are not exclusive.

### A poisonous coffee

Another example is:

```
I would like to drink this coffee
I would not like to drink poisoned coffee
---
This coffee is not poisoned coffee
```

Again, you may think that you cannot conclude that the coffee is not poisoned from a statement of preference. But you can.

The problem here, arises from mistaking the second premise to mean "I would not like to drink coffee that _I know_ to be poisoned". If that were the case, then the argument would absolutely be fallacious.

If this is how you read it then you can construct an argument equivalent to the first, adding knowledge as a condition:

```
I would not like to drink coffee if I know if it is poisoned and it is poisoned
I always know if coffee is poisoned
I would like to drink this coffee
---
This coffee is not poisoned
```

In a more general form, where

- `x` is "I would like to drink coffee"
- `y` is "I know if it is poisoned"
- `z` is "it is poisoned"

```
not x if y and z
y
x
---
not z
```

The condition `y`, "I know if it is poisoned", in premise one is _always_ satisfied by the second premise. Because the condition is always satisfied, the structure of the argument is the same _without the condition_, so the following (which is our original) argument is just as valid:

```
not x if y
x
---
not y
```

The masked man misunderstanding arises when the terms used are not understood _consistently_ throughout the premises. Knowing my father socially does not exclude him from being an epistemically unknown masked man, but knowing both epistemically excludes them from being the same person.
