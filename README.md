# heat-kernel-unimodality

Reproducible Python code, exact rational certificates, and rigorous interval verification supporting the classification of temporal heat-kernel unimodality and residual bounds on finite upper half-plane graphs over prime-power rings.

**Repository:** https://github.com/dedeophd/heat-kernel-unimodality

## Companion paper

M. R. DeDeo, *Temporal unimodality of heat kernels on finite upper half-plane towers*.

The paper studies the middle-parameter upper half-plane graphs over $\mathbb Z/p^n\mathbb Z$, where $p$ is prime, $p\equiv3\pmod4$, and $n\ge1$, with nonsquare parameter $\delta=-1$.

Its main results classify temporal unimodality at every ring level and relate temporal reversals to sharp residual estimates for the normalized heat equation. Every coordinate heat profile is temporally unimodal exactly for $p\in\{3,7,11,19\}$. Every other admissible prime has a profile that exceeds equilibrium and subsequently falls below it, with the same density profile inherited throughout its ring tower.

The paper also proves a uniform residual coefficient bound of $648/55$ for Laplacian error. The difference between the sharp coefficient and the coordinate-peak expression is positive throughout each failing tower, while its supremum over ring levels is $O(p^{-1/2})$ as the prime increases.

## Fixed computational release

**Version:** 1.0.0  
**Release date:** October 19, 2026

Download [the version 1.0.0 computational supplement](Temporal_Heat_Certification_Supplement_v1.0.0.zip) and extract it. The archive is the fixed computational release; this repository README is separate from the files covered by its checksum manifest.

## Reproduce the verification

The release was tested with **CPython 3.12.14** and these pinned dependencies:

| Package | Version |
| --- | --- |
| SymPy | 1.14.0 |
| python-flint, including Arb | 0.9.0 |
| mpmath | 1.3.0 |

Open a terminal in the extracted `Temporal_Heat_Certification_Supplement_v1.0.0` directory and run:

```sh
python verify_release.py
python -m pip install -r requirements.txt
python -O run_all_checks.py
```

The first command checks the distributed files against `SHA256SUMS` using only the Python standard library. The final command runs nine verification jobs, stops at the first failure, and writes `output/Verification_Summary.json` together with individual JSON records and logs. A successful complete run reports nine passed jobs. Explicit mathematical checks remain active when Python is run with `-O`.

Check the archive **before** running the suite. Regeneration changes output files, including timing and environment metadata, so subsequent checksum differences can be expected. Use a fresh extraction when checking the original release again, and keep repository documentation outside the extracted release directory.

## Certificate coverage

| Assertion | Finite coverage | Verification method |
| --- | --- | --- |
| Positive generic field profiles | 14 profiles for $p=7,11,19$ | Exact polynomial arithmetic, rational root isolation, and rigorous interval bounds |
| Temporal failure at the two smallest failing primes | $p=23,31$ | Integer walk counts and rational heat-kernel enclosures |
| Remaining finite negative cases | All 69 admissible primes with $43\le p<841$ | Independent quotient construction from graph neighbors and rational verification |

The positive-profile verifier uses **384-bit Arb interval arithmetic**. It checks an initial Taylor bound, a complete compact-interval partition, and an infinite-tail bound. Exact algebra detects zero coefficients; required interval signs must be strictly separated from zero. Sampled plots are not used to certify global unimodality.

There are **71 negative certificates** in total. The input file `Prime_43_Bridge_Certificates.json` contains the 69 bridge records; the two additional certificates are regenerated in `output/Small_Prime_Failure_Certificates.json`. The number 43 in the historical input filename is the start of the bridge range, not the least failing prime, which is 23.

The other verification jobs check finite walk/curve identities, spherical trace normalizations, Jacobi-sum diagnostics, a Chen/Gauss identity, the extension-field boundary example over $\mathbb F_9$, and direct ring identities in five cases covering 20,282 vertices. One bridge certificate is also rechecked with the standard-library integer backend.

## Release contents

| File or directory | Purpose |
| --- | --- |
| `README.txt` | Detailed reproduction instructions |
| `PROOF_MAP.txt` | Connections between programs and manuscript claims |
| `RELEASE.json` | Version, coverage, and provenance metadata |
| `CITATION.cff` | Citation metadata for the fixed supplement |
| `SHA256SUMS` | Checksums of the distributed files |
| `requirements.txt` | Pinned dependencies |
| `verify_release.py` | Archive checksum verification |
| `run_all_checks.py` | Entry point for all nine jobs |
| `certify_small_prime_classification.py` | Positive profiles, failures at 23 and 31, and small-prime minorization checks |
| `certify_small_prime_unimodality.py` | Rigorous positive-profile certification |
| `generate_prime_bridge.py` | Rational negative-certificate producer |
| `verify_prime_bridge.py` | Independent negative-certificate verifier |
| `Prime_43_Bridge_Certificates.json` | Complete 69-record bridge input |
| `output/` | Included verification records, generated certificates, and logs |

Additional `verify_*.py` programs implement the arithmetic and structural diagnostics described in `PROOF_MAP.txt`.

## Optional regeneration of the bridge input

The main suite independently verifies the distributed bridge records. To generate a new set of all 69 records and then verify it, run from the extracted release directory:

```sh
python -O generate_prime_bridge.py --output output/Regenerated_Bridge_Certificates.json
python -O verify_prime_bridge.py output/Regenerated_Bridge_Certificates.json --output output/Regenerated_Bridge_Verification.json
```

Full bridge generation is substantially slower than verification and is not required for the nine-job check.

## Relationship to the proofs

Appendix B of the companion paper describes the certification criteria. The general arithmetic separation theorem, cohomological spatial-sign argument, analytic negative result for $p\ge841$, and passage to every ring level are proved in the paper. The positive case $p=3$ is handled analytically.

The finite diagnostics support those arguments but do not constitute formal verification of the infinite theorems. Prime-power rings and finite-field extensions are distinct settings; the $\mathbb F_9$ diagnostic does not extend the classification to all finite fields. The financial and continuum connections discussed in the paper are directions for further research.

## Citation

Please cite the companion paper for the mathematical results and version 1.0.0 of the supplement when using these programs or certificates.

```bibtex
@misc{DeDeoHeatKernelUnimodality2026,
  author       = {DeDeo, M. R.},
  title        = {Temporal heat certification: computational supplement},
  howpublished = {{GitHub} repository: {heat-kernel-unimodality}},
  year         = {2026},
  url          = {https://github.com/dedeophd/heat-kernel-unimodality},
  note         = {Version 1.0.0}
}
```
