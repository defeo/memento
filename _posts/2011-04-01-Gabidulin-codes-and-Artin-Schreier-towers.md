---
layout: note
tags: linear_code finite_field tower_of_extensions
---

### From a mail to Antonia on her paper \cite{wachter+afanassiev+sidorenko11}
Troughout your paper you repeatedly use three fundamental
operations in $$\FF_{q^m}$$: addition, multiplication and Frobenius
(any iterate of the _q_-th power, not just the first!). Using
low-complexity normal bases, addition and Frobenius are optimal,
but multiplication is not. My feeling is that the suboptimality of
the symbolic product comes from the suboptimality of
multiplication.

There's not many constructions yielding (quasi-)optimal algorithms
for all of the three operations mentioned above; part of my
research focuses exactly on providing this type of
construction. In my ISSAC '09 paper and in Chapter 6 of my PhD
thesis, I give a construction having these properties, based on
the theory of Artin-Schreier extensions.

Briefly, the idea is to start from a base field $$\FF_q$$ of
characteristic _p_, and then iteratively build some canonical
degree p extensions to represent the field $$\FF_{q^m}$$ with $$m=p^k$$
for some integer _k_. It turns out that, using this construction,
we have the following complexity for the three elementary
operations:

- Addition: _m_ operations over $$\FF_q$$. (Indeed it uses an ordinary
representation on a vector space basis)
- Multiplication: $$M(m \log q)$$ operations over $$\FF_p$$, where
$$M(n)$$ is the cost of multiplying two polynomials of degree at
most _n_ with coefficients in $$\FF_p$$. If $$m\log q$$ is large
enough, $$M(n)$$ can be taken to be the FFT complexity $$n\log n \log\log n$$.
If you want to count in number of operations over
$$\FF_q$$, you can estimate this complexity to be $$O(M(m))$$.
- Frobenius: $$O(pmk^3 + pmk\log^2 q + pkM(m\log q))$$ operations
over $$\FF_p$$, which is in $$\tilde{O}(pM(m))$$ operations in $$\FF_q$$,
ignoring logarithmic factors. This estimate hides some
precomputations, but these precomputations are in
$$\tilde{O}(pM(m))$$ too.

Of course, this construction is limited by the characteristic of
the base field, thus it is not possible to represent $$\FF_{q^m}$$
for an arbitrary _m_.  Furthermore, the complexity depends
strongly on _p_, thus it is most interesting to take $$p=2$$;
fortunately, I guess that the case $$p=2$$ is also the most
relevant for coding theory.

In practice, I think this might yield fast encoding/decoding of
Gabidulin codes over $$\FF_{q^m}$$, where $$q=2^d$$ and $$m=2^k$$ for
some integers _d_ and _k_. For convenience, let me take $$q=2$$,
then the previous complexity bounds are:

- Addition: denoted by $$A(m)$$, in _m_ operations over $$\FF_2$$;
- Multiplication: denoted by $$M(m)$$, in $$m\log m\log\log m$$
operations over $$\FF_2$$;
- Frobenius: denoted by $$F(m)$$, in $$O(mk^3 + kM(m))$$ operations
over $$\FF_2$$, or $$\tilde{O}(M(m))$$ ignoring logarithmic factors.

Using my construction as a black-box, the symbolic product of
two linearized polynomials of degree at most _n_ costs

$$\begin{equation*}
n^2 F(m) + n^2 M(m) + n(n-1)A(m)
\qquad\text{or}\qquad
\tilde{O}(n^2 M(m))
\end{equation*}$$

using the naive algorithm. This is slightly better than the
algorithms in section 4.1 and 4.2 of your WCC paper when $$\log m \ll n \ll m$$.
The constants involved in the $$\tilde{O}$$ are
relatively high, though, and I don't expect this approach to be
practically faster than yours for reasonably sized codes.
Note, however, that my results are quite efficient and beat
magma for $$m=2^{10}$$ already, thus it seems reasonable to try to
improve these bounds. For example, combining this with the
baby-step-giant-step algorithm of yours
\cite{wachter+sidorenko+bossert10}, might improve somehow the
bounds (at least theoretically).

But these complexities are still far from the quasi-optimal
$$\tilde{O}(m^2)$$ I'd expect for symbolic products. Some FFT-like
technique might help here.

"About the WCC paper: If the field supports the FFT, I think
it is also possible to do that transform with the complexity
of a FFT over the field $$\\F_{q^m}$$ because it is actually only a
multiplication of a vector with a Hankel matrix."

I do not quite get your point. As far as I know, DFT matrices
are relied to Toeplitz matrices, not Hankel ones. I do not see
how having some specific primitive roots of unity may help you
doing the _q_-transorm step; but maybe you attribute to "FFT" a
broader meaning than I do.

Indeed, I looked for an FFT-like algorithm to do the
_q_-transform, but I couldn't find any. The matrix of the
_q_-transform is

| $$b^{[0]}$$   | . . . | $$b^{[m-1]}$$ |
| .             |       | .             |
| .             |       | .             |
| $$b^{[m-1]}$$ | . . . | $$b^{[m-2]}$$ |

Let _A_ be a matrix, denote by $$A^{[i]}$$ the matrix obtained by
raising each entry of A to the power $$[i]$$. Then, assuming
$$m=2^k$$, the matrix above has the block structure

| $$A^{[0]}$$   | $$A^{[m/2]}$$ |
| $$A^{[m/2]}$$ | $$A^{[m]}$$   |

Let $$f = f_0 + f_1(X^{[m/2]})$$ be a linearized polynomial, we
want to compute $$Af$$, this is equivalent to compute 

$$\begin{gather*}
A f_0 + A^{[m/2]} f_1\\
A^{[m/2]} f_0 + A^{[m]} f_1
\end{gather*}$$

Each of the matrices _A_, $$A^{[m/2]}$$, $$A^{[m]}$$ looks like a
_q_-transform matrix, so we can compute recursively the four
matrix-vector product above, but this has a quadratic
complexity. To have an FFT-like complexity, we should compute
only two of the matrix-vector product recursively, and deduce
the two others by some magic formula, but unfortunately I don't
see any magic formula, unless $$f_0$$ and $$f_1$$ have coefficients
in $$\FF_q$$ (but this case is not really interesting).

Maybe I overlooked something and it is still possible to do an
FFT-like algorithm by decomposing the matrix of the q transform
in a smarter way, or by doing a slightly different
transform. I'll keep looking at this problem and I'll tell you
if I come up with something.

"However, for the fast multipoint evaluation the step of
multiplying the matrices over $$\FF_q$$ remains the same."

I bet that once the q-transform problem is solved, this one
will follow.  Actually, the approach I was trying above, should
work for the multipoint evaluation as well (or, rather, as
badly), because I wasn't using any property of the normal
basis.

### Some more thoughts on the symbolic product of linearized polynomials

$$\newcommand{\qT}{\,q\!\mathrm{T}}$$
$$\newcommand{\rev}{\mathrm{rev}}$$
$$\DeclareMathOperator{\Tr}{Tr}$$
$$\DeclareMathOperator{\Mult}{M}$$
$$\DeclareMathOperator{\FFrob}{F}$$
Indeed! The _q_-transform is just plain product! Let $$f=\sum_i f_i x^i\in\FF_{q^m}[x]$$
be a polynomial of degree less than _m_ and let
$$L_f$$ be the linearized polynomial associated to _f_. The
_q_-transform of $$L_f$$ with respect to $$\beta\in\FF_{q^m}$$, denoted
by $$\qT(L_f;\beta)$$, is

$$\begin{gather}
\qT(L_f;\beta) = (F_j)_{j<m} = (L_f(\beta^{[j]}))_{j<m} =
\left(\sum_{i} f_i \beta^{[i+j]}\right)_{j<m}.
\end{gather}$$

Equivalently, define the polynomial $$B = \sum \beta^{[i]} x^i\in\FF_{q^m}[x]$$,
then the _q_-transform of $$L_f$$ is

$$\begin{gather}
\qT(L_f;\beta) = B \rev_m f \mod x^m-1,
\end{gather}$$

where $$\rev$$ is the usual reciprocal operator on polynomials:

$$\begin{gather}
\rev_m f = x^m f(1/x) = \sum_i f_{i} x^{m-i}.
\end{gather}$$

Indeed (recall that $$\beta^{[m]} = \beta$$)

$$\begin{gather}
B \rev_m f \bmod x^m-1 = \sum_j \sum_i f_i \beta^{[j+i-m \bmod m]} x^j = \sum_j L_f(\beta^{[j]}) x^j.
\end{gather}$$

If $$\beta$$ is $$\FF_q$$-normal, then the _q_-transform with respect
to $$\beta$$ is an isomorphism of $$\FF_q$$-vector spaces. Let
$$\tilde{\beta}$$ be dual to $$\beta$$, then the _q_-transform with
respect to $$\tilde{\beta}$$ is the inverse of the _q_-transform with
respect to $$\beta$$, indeed

$$\begin{gather}
\qT(\qT(L_f;\beta);\tilde{\beta}) = \tilde{B} \rev_m (B \rev_m f) = \tilde{B} f \rev_m B \mod x^m-1
\end{gather}$$

and

$$\begin{multline}
\tilde{B} \rev_m B = \sum_j \sum_i \beta^{[i]} \tilde{\beta}^{[j+i-m \bmod m]} x^j =\\
\sum_j \sum_i (\beta\tilde{\beta}^{[j]})^{[i]} x^j = \sum_j \Tr(\beta\tilde{\beta}^{[j]}) x^j = 1 \mod x^m-1.
\end{multline}$$

Now, given two linearized polynomials $$L_f$$ and $$L_g$$ of degree at
most _m_, we want to compute their symbolic product $$L_h = L_f \otimes L_g$$.
It can be computed using a _q_-transform as

$$\begin{gather}
L_h(\beta^{[i]}) = L_f(L_g(\beta^{[i]})),
\end{gather}$$

thus if $$G = (G_0, \ldots, G_{m-1})$$ is the _q_-transform of _g_,
by computing the evaluations $$L_f(G_0), \ldots, L_f(G_{m-1})$$ and
then applying the inverse _q_-transform, we obtain $$L_h$$. Hence we
need a fast algorithm to do multi-point evaluation of linearized
polynomials.

Just as a remark, observe that if _g_ has coefficients in $$\FF_q$$,
then $$G_i = G_0^{[i]}$$, thus computing the evaluations is just a
_q_-transform with respect to $$G_0$$ (notice that $$G_0$$ need not be
normal). Then we have

$$\begin{multline}
L_h = \qT(\qT(L_f; G_0);\tilde{\beta}) = \tilde{B} \rev_m (G \rev_m f) =\\
\tilde{B} \rev_m (B (\rev_m g) (\rev_m f)) = f g \tilde{B} \rev_m B = fg \mod x^m-1
\end{multline}$$

thus the symbolic product of $$L_f \otimes L_g$$ is just the
linearized of the ordinary product $$fg$$, a result that is well
known.

Now, in the generic case where _g_ has coefficients in $$\FF_{q^m}$$,
the multi-point evaluation is somewhat harder. We can use the
technique of \cite{wachter+afanassiev+sidorenko11}. Let $$G_i = \sum_j \gamma_{i,j}\beta^{[j]}$$
be the representation of $$G_i$$ on
the normal basis generated by $$\beta$$, then by linearity

$$\begin{gather}
L_f(G_i) = \sum_j \gamma_{i,j} L_f(\beta^{[j]}). 
\end{gather}$$

Hence the multipoint evaluation is given by the matrix-vector
product

$$\begin{gather}
\Gamma \qT(L_f) = 
\begin{pmatrix}
\gamma_{0,0} & \cdots & \gamma_{0, m-1}\\
\vdots & & \vdots\\
\gamma_{m-1,0} & \cdots & \gamma_{m-1,m-1}
\end{pmatrix}
\begin{pmatrix}
L_f(\beta^{[ 0 ]})\\
\vdots\\
L_f(\beta^{[m-1]})
\end{pmatrix}
\end{gather}$$

By expanding each $$L_f(\beta^{[j]})$$ over an arbitrary
$$\FF_q$$-basis of $$\FF_{q^m}$$, the previous matrix-vector product
becomes a product of $$m\times m$$ matrices with coefficients in
$$\FF_q$$, which can be performed in $$m^\omega$$ operations. 

It remains to compute the matrix $$\Gamma$$. Let $$G_i = (g_{i,0},\ldots,g_{i,m-1})$$
be the representation of $$G_i$$ over
the standard basis of $$\FF_{q^m}$$, let _B_ be the change-of-basis
matrix from the standard basis to the normal basis generated by
$$\beta$$, then the matrix $$\Gamma$$ can be computed as

$$\begin{gather}
\Gamma = GB =
\begin{pmatrix}
g_{0,0} & \cdots & g_{0, m-1}\\
\vdots & & \vdots\\
g_{m-1,0} & \cdots & g_{m-1,m-1}
\end{pmatrix}
B
\end{gather}$$

which can be computed in $$m^\omega$$ operations too.

Summarizing, the symbolic product $$L_f \otimes L_g$$ can be
computed as follows.

Precomputations:

1. Select a normal element $$\beta$$;
2. Compute $$\beta^{[i]}$$ for $$0 < \beta < m$$;
3. Form the polynomial $$B = \sum_i \beta^{[i]} x^i$$;
4. Compute $$1/\rev_m B \bmod x^m - 1$$, this gives the polynomial $$\tilde{B}$$;
5. Form the change-of-basis matrix from the normal basis generated
by $$\beta$$ to the standard basis, this is the matrix having
$$\beta^{[i]}$$ in the _i_-th column;
6. Compute the change-of-basis matrix from the standard basis to
the normal basis by inverting the previous matrix.

Computations:

1. Compute $$\qT(L_g;\beta)$$ and $$\qT(L_f;\beta)$$;
2. Compute the matrix $$\Gamma$$ as $$\qT(L_g)B$$;
3. Compute $$H = \Gamma\qT(L_f)$$;
4. Compute $$\qT(H; \tilde{\beta})$$.

I write $$\Mult(m)$$ for the cost of multiplying two elements of
$$\FF_{q^m}$$, $$\FFrob(m)$$ for the cost of computing $$x^{[i]}$$ for any
$$x\in\FF_{q^m}, 0<i<m$$. Then the complexity of the precomputations
is:

1. Depends on the setting, but ususally cheap;
2. $$(m-1)\FFrob(m)$$;
3. Free;
4. This is an XGCD in $$\FF_{q^m}[x]$$, which can be done in
$$O(\Mult(m^2)\log m)$$ in many cases using Kronecker substitution;
5. Free;
6. $$m^\omega$$.

And the complexity of the computations is:

1. $$O(\Mult(m^2))$$ in many cases using Kronecker substitution;
2. $$m^\omega$$;
3. $$m^\omega$$.
4. $$O(\Mult(m^2))$$, as before.

In the setting of my paper on Artin-Schreier towers
\cite{df+schost09}, $$\Mult(m)$$ and $$\FFrob(m)$$ are both in
$$\tilde{O}(m)$$, thus the cost $$m^\omega$$ of mutliplying matrices
dominates.

From a practical point of view, I don't think this algorithm would
be effective when _q_ is not prime, because I intentionally hid
the fact that a full "push-down" to $$\FF_q$$ is needed in order to
work with matrices with coefficients in $$\FF_q$$. This doesn't
affect the overall complexity, but is somewhat expensive in
practice. On the other hand, when $$q=2$$ I think that the algorithm
would be quite fast, after the precomputations are done (however,
it would take a quite big _m_ to beat the one based on optimal
normal bases).

I still think that there must be a smarter way of doing the
multi-point evaluation, achieving the $$\tilde{O}(m^2)$$ complexity. If
this smarter way exists, then I guess that the algorithm would be
quite fast in practice too!
