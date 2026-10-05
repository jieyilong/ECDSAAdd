# Exact slim-cleanup point checkpoint

Source checkpoint: `60debd8dd7afc6bb67777dd3b685de6cc1987ade`, including the inverse record-alignment and active-support integration repairs after `5ad000f`.

## Verification status

The exact 511-Toffoli cleanup, its phase/state restoration, both 1,024-Toffoli forward/inverse field kernels, inverse record alignment, active support and physical relabeling have passed remote Lean checks. The complete selected point circuit is undergoing the fixed-source full build and transitive axiom audit in `slim-offset-full-point-v3`. This branch is not a newly promoted full checkpoint until that attempt passes.

The prior promoted full circuit is `d6d6b4f`: 2,227,651 T, 1,565,127 measurements and ≤1,899 logical sites. Its full build and all 1,336 axiom checks passed. All changes here preserve the original public controlled point-addition specification, arbitrary measurement records, incoming phase, controls and full work restoration. No approximation, shortened GCD horizon or sampled correctness replacement is used.

## Integrated resource theorems

| Stage | Q allocation ceiling, including residents | Toffolis | Measurements |
| --- | ---: | ---: | ---: |
| 1. Coordinate differences | ≤1,036 | 2,046 | 2,046 |
| 2. Skywalk-GCD division | ≤1,899 | 1,058,625 | 727,623 |
| 3. Prepare X workspace | ≤1,036 | 1,023 | 1,023 |
| 4. Measured streamed square | ≤1,297 | 99,902 | 99,382 |
| 5. Forward multiplication | ≤1,899 | 1,058,626 | 727,624 |
| 6. Output recovery | ≤1,036 | 2,301 | 2,301 |
| Six-stage subtotal | ≤1,899 | 2,222,523 | 1,559,999 |
| Input/corner classification | ≤1,034 | 4,104 | 4,104 |
| Complete finite-addend point addition | ≤1,899 | 2,226,627 | 1,564,103 |

These are static instruction counts and certified logical-site allocation ceilings. Q is not a separately measured exact peak-live count. The infinity addend emits an empty program. The ≤1,297 Q and <600K T per arithmetic-stage targets remain open.

The cleanup's final offset carry is computed and erased but never consumed. Removing it saves one Toffoli and one measurement per field update. All 512 updates in each arithmetic stage remain, saving 512 T/M per stage and 1,024 T/M in the complete circuit, with no claimed Q reduction.

## Evidence and integration repairs

All Lean execution occurred on the CPU pod. Accepted component attempts include `offset-slim-chain-v3` (13s), `offset-slim-full-proof-v1` (8s), `offset-slim-forward-proof-v1` (74s), `offset-slim-inverse-proof-v1` (5s), `offset-slim-rename-v1` (5s), `offset-slim-inverse-integrated-v2` (6s), and `slim-active-support-v3` (8s). Inline axiom checks use only `propext`, `Classical.choice` and `Quot.sound`; their time is included in compilation.

Full attempt v1 failed after 160s because inverse recovery still used the old 512-measurement alignment. The exact repair uses 511 recovery measurements and 253 padding records to align with the 764-record original reference. Full attempt v2 failed after 226s because one active-support theorem still referred to the old cleanup. The new cleanup's support is now included in the old emitted program's support, without admitting descriptor-only ghost sites or widening the allocation. Both failed attempts are retained; neither is credited as full verification.

Attempt v3 contains 756 fixed source/configuration/script hashes and checks all selected public transitive axioms. The protected `ControlledPointAddSpec.lean`, `PointAddSpec.lean` and `AffineFormula.lean` files are byte-identical to the previous checkpoint.
