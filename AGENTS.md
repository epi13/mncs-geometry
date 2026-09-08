# Agent and contributor contract

- Prefer `mncs-language` for implementation.
- Geometric robustness is a correctness property; do not patch failing predicates with unexplained epsilons.
- Separate approximate geometry from exact/adaptive predicate paths.
- State coordinate system, dimensionality, handedness and tolerance assumptions explicitly.
- Validate algorithms with degeneracies and adversarial configurations, not only typical cases.
- Record MNCS language/compiler/runtime blockers in `docs/LANGUAGE_PRESSURES.md`.
- Keep reusable geometry independent from rendering/game policy.
