# Architecture

## Layers

1. **Primitives** — coordinates, points, vectors, rays, planes, transforms and bounds.
2. **Predicates** — orientation, sidedness, incidence, intersection and containment with explicit robustness contracts.
3. **Topology** — polygons, meshes, adjacency and manifold/non-manifold representations.
4. **Algorithms** — hulls, triangulation, clipping, boolean operations and closest-point queries.
5. **Spatial structures** — grids, trees, BVHs and broad-phase indexing.
6. **Verification** — degeneracy corpus, exact/reference predicates and invariants.
7. **Adapters** — integration with physics, engine, FEM and visualization systems.

## First milestones

1. Core 2D/3D primitives and transforms.
2. Robust predicate suite.
3. Polygon/mesh representation.
4. Hull/triangulation/intersection corpus.
5. Spatial acceleration structures and CPU/GPU pressure tests.
