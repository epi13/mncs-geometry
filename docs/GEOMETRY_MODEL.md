# Geometry ownership and model

What `mncs-geometry` owns, what it consumes, and the semantic model
actually implemented. Status: foundational 2D f64 slice realized in
MNCS (point/vector, bounds, segments, affine transforms); see the
module headers for exact contracts.

## Ownership

- **`mncs-numeric`** owns numeric representation, conversion, and
  scalar contracts. Geometry consumes `mncs.numerics.scalar_float`
  (`sqrt_value`, `approx`, `fmin`/`fmax`) and never re-implements
  numeric semantics. No numeric trait, conversion policy, overflow
  policy, or approximate-comparison vocabulary lives here.
- **`mncs-math`** owns reusable mathematical functions and
  algorithms. Geometry is deliberately not built on `mncs-math`:
  it is mid-campaign with unstable interfaces, and this slice needs
  nothing beyond Numeric plus the `sin`/`cos` language intrinsics.
  Rotation consumes the intrinsics directly (bit-exact across
  backends through the shared host libm, as pinned by the numerics
  corpus); when Math stabilizes, geometry does not need redesign to
  keep working, and any future Math consumption (special functions,
  decompositions) composes at the call site.
- **`mncs-test`** owns verification: the suites are Profile 0.18
  `test` declarations run through `mncs test` by
  `scripts/run_tests.py`, with stable test-case identities and
  revision-bound JSON evidence.
- **Forge/Debug/Doctor/RAVEL**: tests execute through the canonical
  `mncs test` pathway (Forge-consumable envelopes); failures carry
  geometric input/context in structured assertion records for Debug;
  Doctor checks source conformance. Geometry builds no
  orchestration, diagnosis, or policy of its own.

## Semantic model

- `Point2` (location) and `Vec2` (displacement) are distinct record
  types with directional operations: point − point → vector,
  point + vector → point. Point + point does not exist.
- `Bounds2` always satisfies lo ≤ hi: `make_bounds` validates at
  construction and reports violations as `OptBounds.Empty`, a
  first-class value rather than a degenerate box. Containment is
  boundary-inclusive.
- `Orient` (Ccw/Cw/Collinear) is the exact sign of the edge cross
  product — no epsilon band. `SegIntersect` classifies disjoint
  (`None`), single-point contact (`Point`, crossing or endpoint
  touch), and collinear overlap (`Overlap` with inner endpoints).
  Degenerate segments intersect as the points they are.
- `Affine2` applies M·p + t to points and M·v to vectors
  (translation never moves a displacement). Inversion is validated
  (`Inv2.Singular` on zero determinant, never a divide-by-zero).

## Exact vs approximate

Exact (only exact binary64 operations): translation, scaling by
exact factors, dot/cross products, length-squared,
distance-squared, orientation signs, containment, union,
intersection intervals, Cramer's-rule crossing points on exact
inputs. Approximate (explicit tolerances at the call site):
lengths, normalization, rotations, inverse round-trips, and the
`point_approx`/`vec_approx` helpers. Geometry defines no global
epsilon; near-degenerate configurations classify by computed sign
(pinned by test, documented in `segment2`).

## Dimensionality and coordinate types

The slice is concrete 2D f64. Generic coordinate arithmetic is not
supported by the current language (MNE120: type parameters carry no
arithmetic capability — see `docs/LANGUAGE_PRESSURES.md`), so a
generic `Point<T>` would duplicate every operation per concrete
type with no semantic gain. Integer-coordinate exact geometry,
N-dimensional generics, coordinate-frame phantom types, and 3D
primitives are recorded future work, not attempted here: the
pressure must be real (a generic arithmetic capability), not
worked around by duplication.

## Degeneracy contract

Zero vectors have no direction (`Norm2.Degenerate`); zero-length
segments measure and intersect as points; singular transforms do
not invert; empty bounds are values, not violations. Division
appears only behind validated guards (nonzero length-squared,
nonzero determinant, nonzero projection denominator with an
unreachable-but-checked fallback that returns `None` rather than a
NaN).
