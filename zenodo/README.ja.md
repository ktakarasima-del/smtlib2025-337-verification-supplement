# SMT-LIB 2025 追加検証証跡

本データセットは[既刊179件](https://doi.org/10.5281/zenodo.24416131)の補足です。対象は[SMT-LIB 2025公式リリース](https://doi.org/10.5281/zenodo.16740866)に収録された非増分のQF_DT 197件とQF_IDL 140件で、元の宣言が `:status unknown` の全337件です。

| 証跡の出所 | SAT | UNSAT | 合計 |
| --- | ---: | ---: | ---: |
| 既刊 | 40 | 139 | 179 |
| 今回の追加 | 11 | 147 | 158 |
| 全体 | 51 | 286 | 337 |

`SMTLIB2025_COMPLETE_337_SUMMARY.csv` は、全件の公式パス、原本SHA-256、判定、証跡の出所を記録しています。`declared_status` と `catalog_inferred_status` は公式側のメタデータで、`verified_status` が検証済み判定です。`CERTIFICATES.json` から追加158件の原本、検証形式、該当アーカイブと内部ファイルのSHA-256を確認できます。

証跡はSAT代入11件、有限軌跡の反証118件、構造を利用した反証29件です。29件の内訳は彩色13、Job Shop 8、区間の閉包2、トラック容量2、ペグ数3、配線1です。公開した再検証コードは `smtlib2025_verifier_source.tar.gz` に含まれます。

## 再検証

[公式レコード](https://zenodo.org/records/16740866)から `QF_DT.tar.zst` と `QF_IDL.tar.zst` を取得してください。Python 3.11以上、C++17コンパイラ、Z3 5.4.0、cvc5 1.4.2、VeriPB 3.0.2、CakePB CP2026のCNFフロントエンドを使います。コマンドと配置手順は[英語README](README.md#recheck)、各ツールの入手先は[第三者表記](THIRD_PARTY_NOTICES.md)を参照してください。

完全再生の合格条件はSAT 11件、UNSAT 147件、失敗0件です。チェックサム一覧は公開ファイルのうち自身を除く10ファイルをカバーします。既刊179件の集計との照合には、既刊の `SMTLIB2025_VERIFIED_179_CERTIFICATES.csv` を `--baseline-certificates-csv` に指定できます。既刊分の証跡自体は既刊の検証手順で再生できます。

SAT代入とUNSAT証跡の元式への対応をZ3とcvc5で確認します。UNSATの反証形式に応じて、有限状態の閉包、算術矛盾、有限分岐、またはVeriPBとCakePBによる擬ブール反証を検査します。生成した証跡、集計、文書はCC BY 4.0、配布する検証ソースはMITです。第三者の権利は[第三者表記](THIRD_PARTY_NOTICES.md)を確認してください。
