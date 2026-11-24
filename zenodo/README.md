# SMT-LIB 2025 additional verification certificates

This dataset supplements the [179-certificate record](https://doi.org/10.5281/zenodo.24416131). The cohort comprises all 337 non-incremental QF_DT (197) and QF_IDL (140) benchmarks in the [SMT-LIB 2025 release](https://doi.org/10.5281/zenodo.16740866) with a declared `:status unknown`.

| Evidence | SAT | UNSAT | Total |
| --- | ---: | ---: | ---: |
| Prior record | 40 | 139 | 179 |
| Additional certificates here | 11 | 147 | 158 |
| Complete cohort | 51 | 286 | 337 |

`SMTLIB2025_COMPLETE_337_SUMMARY.csv` links every verdict to its source record or this supplement and identifies the official input by benchmark path and SHA-256. The fields `declared_status` and `catalog_inferred_status` are preserved upstream metadata. `verified_status` records the independently checked result. These results are for the pinned 2025 inputs.

`CERTIFICATES.json` indexes the 158 additions and binds every witness to its input, method, archive and member checksum. The three witness archives contain 11 SAT assignments, 118 bounded trajectory refutations and 29 other structural refutations. The 29 comprise 13 graph-colouring, 8 Job Shop, 2 interval closure, 2 track capacity, 3 peg count and 1 wire-routing case. The verification sources are in `smtlib2025_verifier_source.tar.gz`.

## Recheck

You need Python 3.11 or later, a C++17 compiler and public releases of Z3 5.4.0, cvc5 1.4.2, VeriPB 3.0.2 and the CP2026 CakePB CNF frontend (`cake_pb_cnf`). See [third-party notices](THIRD_PARTY_NOTICES.md) for source links. On GNU/Linux, install the tools on your PATH or pass `--z3`, `--cvc5`, `--veripb` and `--cakepb` with their executable paths. The compiled C++ checker reads the serialized graph-colouring refutations.

Download both `QF_DT.tar.zst` and `QF_IDL.tar.zst` from the [official record](https://zenodo.org/records/16740866). Put the 11 files of this supplement under `bundle` and run the following from its parent directory. The verifier checks each needed original by its pinned SHA-256. The witness archives are opened by the verifier.

```sh
mkdir official verifier
tar --zstd -xf QF_DT.tar.zst -C official
tar --zstd -xf QF_IDL.tar.zst -C official
tar -xzf bundle/smtlib2025_verifier_source.tar.gz -C verifier
g++ -std=c++17 -O2 verifier/graph_tree_check.cpp -o verifier/graph-tree-check
python3 verifier/verify_smtlib2025_supplement.py --bundle bundle --checksums-only
python3 verifier/verify_smtlib2025_supplement.py --bundle bundle --official-inputs official \
  --tree-checker "$PWD/verifier/graph-tree-check" --report verification.json
```

The first command that invokes Python checks the 10 checksum entries and the 337/158 row counts. The full run must report `verified_sat: 11`, `verified_unsat: 147`, `failures: 0` and `complete_supplement: true`. The report path must be new and outside the bundle. `--workers 2` enables parallel verification if memory permits. `--cases case_NNN` restricts proof replay to selected supplement cases. To additionally check the 179 prior rows, download `SMTLIB2025_VERIFIED_179_CERTIFICATES.csv` from [the prior record](https://zenodo.org/records/24416131) and pass its path with `--baseline-certificates-csv`. The prior record has its own certificate archives and verification entry point.

The SAT checker validates the assignments against the official assertions with Z3 and cvc5. For UNSAT, Z3 and cvc5 check the obligations that bind the certificates to the official assertions. The terminal checks depend on the proof form. They include closure of finite state sets, an arithmetic contradiction, checked finite branching, and pseudo-Boolean proofs checked by VeriPB and CakePB. Only verification code and witnesses are distributed here. See [LICENSE](LICENSE) for the license by file type.
