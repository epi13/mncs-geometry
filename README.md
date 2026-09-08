# mncs-geometry

Robust machine-native computational geometry for MNCS.

`mncs-geometry` provides reusable geometric primitives and algorithms while deliberately pressuring `mncs-language` on numerical robustness, topology, spatial data structures, generic coordinate systems, memory layout and CPU/GPU execution.

## Initial scope

- points, vectors, lines, rays, planes and transforms
- robust orientation/intersection predicates
- polygons, polyhedra and meshes
- convex hulls and triangulation
- spatial indexes, BVHs and acceleration structures
- topology-aware mesh operations
- collision/query geometry shared with other MNCS systems
- exact/adaptive predicates and verification cases

## Repository layout

- `docs/ARCHITECTURE.md`
- `docs/rfcs/0001-foundation.md`
- `docs/LANGUAGE_PRESSURES.md`
- `AGENTS.md`
