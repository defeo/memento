---
layout: note
tags: linear_code transposition_principle
---

A very trivial observation. Let $$C\subset\FF_q^n$$ be a linear code
of length $$n$$ and dimension $$k$$. Its _generating matrix_ $$G$$ is the
$$n\times k$$ matrix such that $$C$$ is the span of its rows. Its
_parity check matrix_ $$H$$ is the $$n\times(n-k)$$ matrix such that
$$c\in C\Leftrightarrow cH^T=0$$. In particular $$GH^T=0$$.

If $$f\in\FF_q^k$$ is a plain word, it is customary to encode it as
$$c=f^TG$$. On the receiver side, if $$y=c+e$$ is a received codeword
(eventually containing an error $$e$$), most decoding algorithms
compute the _syndrome_ $$s=yH^T=eH^T$$; if $$s=0$$, no error correction
is needed, otherwise the value of $$s$$ may be used to find the value
of the error $$e$$.

The orthogonal space of $$C$$, denoted by $$C^\bot$$, is called the
_dual code_ of $$C$$. Its generating matrix is the parity check
matrix of $$C$$, and _vice-versa_. Then, the _transposition
principle_ says that any encoding algorithm for $$C$$ yields an
algorithm to compute syndromes of $$C^\bot$$ having the same
complexity, and _vice-versa_.

Obviously, this doesn't say much on decoding, because decoding
linear codes is a (highly) non-linear mapping (and it is much
harder than simply computing syndromes). I doubt this observation
is of any interest, except for pointing out some dumb literature.
