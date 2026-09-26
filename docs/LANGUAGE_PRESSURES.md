# MNCS language pressure ledger

Each item records workload, observed behavior, required semantic, reproducer, owner, workaround and closure verification.

## Index

| ID | Pressure | Severity | Status |
|----|----------|----------|--------|
| G-001 | No generic numeric arithmetic (MNE120): type parameters carry no arithmetic capability | capability | confirmed, designed around (concrete f64 slice) |

## G-001 — No generic numeric arithmetic

- **Severity:** capability (blocks generic-coordinate geometry).
  **Status:** confirmed, designed around. **Non-blocking** for the
  f64 slice.
- **Workload:** a generic `Point<T>` / `Vec<T>` with `add`, `dot`,
  `scale` shared across f64, i64, and future exact domains.
- **Repro:** `fn vadd<T>(a: T, b: T) -> (result: T) { return a + b; }`
  called at `(2, 3)` in a 0.18 module.
- **Expected:** arithmetic over a numeric-constrained `T`, or a
  diagnostic naming the missing constraint kind.
- **Actual:** single precise `MNE120` (arithmetic operands must
  have an integer type; the generic parameter carries no arithmetic
  capability). The message is accurate — the capability does not
  exist — so this is a missing-capability pressure, not a
  diagnostic pressure.
- **Why it matters (bounded):** without it, every coordinate domain
  duplicates the full operation surface (the `mncs-numeric` P-009
  situation). Geometry stays concrete f64 rather than duplicating
  itself per domain.
- **Workaround:** concrete f64 primitives (this repository).
- **Desired behavior:** a numeric capability/constraint usable in
  generic signatures, so one geometric implementation serves all
  numeric domains with domain-appropriate exact/approximate
  behavior selected by the constraint.
- **Acceptance criteria:** the `vadd<T>` reproducer elaborates at
  concrete integer and float instantiations.
- **Regression test:** this ledger entry plus the native geometry
  suites (which gain generic instantiations when it lands).
- **Depends on:** none. Dual of `mncs-numeric` P-009.
