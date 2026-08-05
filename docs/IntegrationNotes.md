# PR 27-29 Reconstruction Notes

## Immutable upstream points

| State | Commit |
| --- | --- |
| PR #27 merged baseline | `1a624a9` |
| PR #28 original head | `8f73a3b` |
| PR #28 upstream merge | `6538695` |
| PR #29 head | `d739396` |

PR #28 and PR #29 were independently authored from the same PR #27 baseline.
PR #28 was merged first, leaving PR #29 with conflicts in agent, camera, sensor,
loader, rendering, thermal, and world modules.

## Reconstruction procedure

1. Rehearsed PR #29's two commits on the PR #27 merge.
2. Merged the original PR #28 head afterward to enumerate real conflicts.
3. Treated PR #28 as authoritative in overlapping thermal and IR code.
4. Created the final integration branch from upstream's clean PR #28 merge.
5. Replayed PR #29 while preserving its functional fixes:
   - Far clipping range correction
   - `orientation` spelling correction
   - Removal of duplicate agent position/orientation initialization
   - EO camera offset/angle configuration
6. Preserved PR #28's configurable IR loader and added #29 mount names as
   compatibility fallbacks rather than replacing the loader.
7. Excluded `logs/buffer/buffer.ppm`, a generated screenshot artifact included
   in PR #29 that contained no source behavior.
8. Replayed post-#28 thermal rendering, controls, target, and legend commits.
9. Applied code-quality and documentation changes as a separate final commit.

## Conflict policy

- Thermal equations, material values, sanity artifacts, and ThermalBody
  behavior from PR #28 remain semantically unchanged.
- PR #29 fixes are retained where they correct independent rendering behavior.
- Compatibility code is explicit and documented instead of duplicating loaders.
- Generated runtime buffers are not source-controlled as integration changes.

This separation makes review straightforward: original feature commits remain
identifiable, and refactoring/documentation can be assessed independently.
