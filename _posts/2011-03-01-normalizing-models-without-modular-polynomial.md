---
layout: note
tags: isogeny elliptic_curve 
---

Here's a stupid idea. Let $$E$$ and $$\hat{E}$$ be non-normalized
$$\ell$$-isogenous curves, then there is a curve $$\hat{E}_u$$ isomorphic
to $$\hat{E}$$ and such that the isogeny is normalized. To find $$u$$,
apply BMSS in $$\FF_q(c)$$. More precisely, set $$\hat{E}_c : y^2 = x^3 +c^4ax + c^6b$$ and proceed as in BMSS to develop the rational fraction
at infinity; stop at a truncation degree slightly larger (one
coefficient is enough) than what is needed by BMSS.  Then, if one
specialises $$c=u$$ in the power series, a gap must happen after $$\ell$$
steps of the rational fraction reconstruction, while this is very
unlikely for any other specialisation of $$c$$. Since $$u$$ is unknown, do
the rational fraction reconstruction in $$\FF_q(c)$$, consider the
leading coefficient of the remainder where the gap must happen, and
equate it to zero: $$u$$ must be one of the roots of the equation (most
likely, there will only be $$u$$ and $$-u$$).

Unfortunately, I think that this algorithm uses a cubic number of
operations in $$\FF_q$$. In fact, the coefficient of the power series
should be polynomials of degree $$O(\ell)$$ in $$c$$, thus, after $$\ell$$
steps of Euclid's algorithm, the polynomials involved will have
coefficients of degree $$O(\ell^2)$$ in $$c$$. Thus the space needed is
$$O(\ell^3)$$ elements of $$\FF_q$$, without even considering the time
complexity (which should be $$\tilde{O}(\ell^3)$$).
