---
layout: note
tags: elliptic_curve sage
---

### Elliptic Curves

  - `sage.schemes.elliptic_curves.ell_wp` contains a function
        named `solve_linear_differential_system`. This seems a
        duplication of
        `sage.ring.power_series_ring_element.PowerSeries.solve_linear_de`.

  - Where shall I put modular polynomials?

  - In `sage.schemes.elliptic_curves.ell_generic` the method
	`multipilcation_by_m_isogeny` computes wrong maps.

  - The following code

	~~~ python
          sage: E = EllipticCurve('11a1')
          sage: phi = E.isogeny(None, E, 4, normalized=2)
          sage: phi.rational_maps()
	~~~

	is utterly slow!
