# RFC 0001: Robust geometry foundation

Status: Draft

## Principles

- Topological decisions must not depend on arbitrary floating-point accidents.
- Predicate APIs communicate robustness/precision contracts.
- Degenerate inputs are defined behavior, not undefined edge cases.
- Coordinate/frame types should prevent accidental mixing where practical.
- Data layouts should support scalar, SIMD and heterogeneous execution without changing geometric meaning.
- High-level algorithms are built on verified predicates rather than duplicated tolerance logic.

## Pressure objectives

Generic numeric types, const/dimension parameters, exact/adaptive arithmetic, SIMD-friendly structs, recursive/spatial data structures, enums and topology representations, ownership of graph-like meshes, robust error/result types, CUDA layouts and optimization-sensitive predicates.
