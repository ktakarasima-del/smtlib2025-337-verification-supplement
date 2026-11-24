# SMT-LIB 2025 verification supplement

This dataset provides machine-checkable witnesses for 158 additional SMT-LIB 2025 benchmarks and a unified outcome table for all 337 non-incremental QF_DT and QF_IDL benchmarks with a declared `:status unknown`. The canonical release is the [Zenodo dataset](https://zenodo.org/records/24492181), DOI [10.5281/zenodo.24492181](https://doi.org/10.5281/zenodo.24492181).

| Evidence | SAT | UNSAT | Total |
| --- | ---: | ---: | ---: |
| Prior release | 40 | 139 | 179 |
| This supplement | 11 | 147 | 158 |
| Complete cohort | 51 | 286 | 337 |

The [complete summary CSV](zenodo/SMTLIB2025_COMPLETE_337_SUMMARY.csv) records each official benchmark path, SHA-256, published catalogue status, verified outcome and certificate source. The [certificate index](zenodo/CERTIFICATES.json) identifies the 158 witnesses in the supplement. The [earlier record](https://doi.org/10.5281/zenodo.24416131) provides the first 179 certificates.

[Dataset documentation](zenodo/README.md) · [日本語の説明](zenodo/README.ja.md)

## Download the archives

All archives are available from the canonical Zenodo record.

- [11 SAT assignments](https://zenodo.org/records/24492181/files/smtlib2025_additional_11_sat_models.tar.gz?download=1)
- [118 bounded trajectory refutations](https://zenodo.org/records/24492181/files/smtlib2025_additional_118_trajectory_proofs.tar.gz?download=1)
- [29 structural refutations](https://zenodo.org/records/24492181/files/smtlib2025_additional_29_structural_proofs.tar.gz?download=1)
- [Verification source code](https://zenodo.org/records/24492181/files/smtlib2025_verifier_source.tar.gz?download=1)

## Verify the release

Download the complete release on Zenodo, or add the four archives to the `zenodo` folder of a local copy of this repository. Then run the following from the repository root.

```sh
mkdir verifier
tar -xzf zenodo/smtlib2025_verifier_source.tar.gz -C verifier
python3 verifier/verify_smtlib2025_supplement.py --bundle zenodo --checksums-only
```

The expected counts are 10 checksum entries, 337 summary rows and 158 supplemental certificate entries. For full proof and model replay, obtain the official inputs from the [SMT-LIB 2025 release](https://doi.org/10.5281/zenodo.16740866) and follow the [verification instructions](zenodo/README.md#recheck).

## License and citation

The author's contributions to the generated data and documentation are licensed under CC BY 4.0. The verification source is MIT-licensed. See [LICENSE](zenodo/LICENSE), the [third-party notices](zenodo/THIRD_PARTY_NOTICES.md) and [citation metadata](CITATION.cff).
