---
layout: note
tags: isogeny elliptic_curve transposition_principle pairing
---

Suppose you have a straight-line program to evaluate an isogeny
$$\phi$$. How do you evaluate its dual $$\hat{\phi}$$ ?

An easy way: if you have enough points of order prime to the degree,
evaluate $$\phi$$ on them, multiply them by the degree, then
interpolate.

And in general? What if the straight-line program is sublinear (which
actually happens very often in cryptography)? One example is the
evaluation of the dual of the composite degree isogeny in my paper
with Jao and Plût, although I think that in that case it is trivial to
evaluate the dual isogeny by step-by-step interpolation.

The Weil and other pairings give some interesting bilinear forms on
the curve, which may be leveraged by the usual transposition principle
techniques. Would this work and be efficient? The Jao-Soukharev
isogeny evaluation algorithm might be a good target.