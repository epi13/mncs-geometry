# mncs-geometry

Foundational computational geometry for MNCS, realized directly in
the current language.

`mncs-geometry` provides 2D geometric primitives with explicit
exact/approximate contracts: points vs displacement vectors as
distinct types, validated bounds, orientation predicates without
epsilon bands, rich segment-intersection classification, and
validated affine transforms. Robustness is a correctness property:
degenerate cases (zero vectors, zero-length segments, singular
transforms, empty bounds, collinear configurations) are first-class
values with defined semantics, never accidental divide-by-zero or
NaN behavior.

## Ownership

- Numeric representation, conversion, scalar contracts: `mncs-numeric`
  (consumed, never duplicated).
- Mathematical functions/algorithms: `mncs-math` (deliberately not
  a dependency yet; see `docs/GEOMETRY_MODEL.md`).
- Verification: `mncs-test` via `scripts/run_tests.py`.
- See `docs/GEOMETRY_MODEL.md` for the full boundary, exact vs
  approximate discipline, and dimensionality/coordinate decisions.

## Repository layout

- `src/geometry/` — the library (`.mncs` only, no host semantics)
- `tests/native/` — in-language contracts via `mncs test`
  (25 `test` declarations)
- `scripts/run_tests.py` — canonical runner with revision-bound
  JSON evidence
- `docs/` — architecture, geometry model, pressure ledger, RFC

## Quick start

Requires sibling checkouts `../mncs-language` (or
`MNCS_LANGUAGE_DIR`), `../mncs-numerics` (or `MNCS_NUMERICS_DIR`),
and `../mncs-test` (or `MNCS_TEST_DIR`); a prebuilt CLI binary is
used when present, else `cargo run`.

```bash
python3 scripts/run_tests.py         # native contracts via mncs test
```

`MNCS_LIBRARY_PATH` is set to `src/` plus the consumed libraries
by the runner; set it yourself for direct CLI use (geometry
resolves as `mncs.geometry.*`).

## Status

Foundational 2D f64 slice: point/vector semantics, bounds,
orientation, segment intersection (crossing/parallel/coincident/
tangent/degenerate), point-segment distance, affine transforms
(translate/scale/rotate/compose/validated inverse) — 25 native
contract tests green. One language pressure filed (G-001, generic
numeric arithmetic). See `docs/ARCHITECTURE.md`,
`docs/GEOMETRY_MODEL.md`, `docs/LANGUAGE_PRESSURES.md`.
