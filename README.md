# quarant-results

[Quarant](https://github.com/SaeYoshizaki/Quarant)で、公開データセットを解析した結果の保管場所。

## 中身

`runs/` の各ファイルは、1 回の全データの解析。展開すると、キャプチャ 1 つごとにフォルダがあり、`events.jsonl`（検出したイベント）、`device_inventory.json`（機器の一覧と識別の根拠）、`findings.json`（機器 × 状態の要約）が入っている。各フォルダの 

| ファイル | Quarant | キャプチャ数 | 使った実験（Quarant の docs/EXPERIMENTS.md） |
|---|---|---|---|
| `runs/runs-fix4.tar.gz` | `106340f` | 1,084 | **最新**。E13（STARTTLS を入れたあとの測り直し） |
| `runs/runs-fix3.tar.gz` | `978b290` | 1,084 | E13（キープアライブ、空のアップロードの修正） |
| `runs/runs-fix2.tar.gz` | `203ccc9` | 1,084 | E13（本文の長さの修正。体重計の後退が見つかった版） |
| `runs/runs-fix.tar.gz` | `3ae8b2c` | 1,084 | E12（E11 の見逃しの修正、D23） |
| `runs/runs-d22.tar.gz` | `c357166` | 1,084 | E12 の修正前の基準（D22 を含む） |
| `runs/runs-e10.tar.gz` | `abf5d92` | 1,084 | E10、E11（見逃しの測定） |
| `runs/runs-e9.tar.gz` | `a4ea211` | 1,083 | E9 |
| `runs/runs-e5.tar.gz` | `02d1ac8` | 35 | E5 |
| `runs/runs-e4.tar.gz` | `b3b2e14` | 35 | E4 |
| `runs/runs-cic.tar.gz` | 記録なし | 1,032 | E6〜E8（CIC IoT 2022） |
| `runs/runs-active.tar.gz` | `21978b3` | 1 | E9（CIC IoT 2022 の 5-Active） |
| `runs/runs-flood.tar.gz` | `203ccc9` | 79 | E13（CIC IoT 2022 の 1-Flood での誤反応の確認）。フォルダ名に `._` を含む 79 個は macOS の付随ファイルで、中身は空 |
| `runs/runs-e1.tar.gz` | 記録なし | 約 30 | E1（最初の検証。UNSW 2016、IoT-23） |
| `ground-truth/gt-e10.tar.gz` | — | 1,084 | E11 の正解（Zeek 9.0.0 と生パケットの検索。名前だけで、値は含まない） |

キャプチャの名前の付け方（フォルダ名）：`unsw-16-*`（UNSW IoT traces 2016）、`unswatk-18-*`（UNSW IoT attack traces 2018）、`iot23-*`（IoT-23）、`cic-1-power-*`、`cic-2-idle-*`、`cic-3-interactions-*`、`cic-4-scenarios-*`、`cic-5-active-*`、`cic-6-rtsp-*`（CIC IoT 2022）。

## そのほかの作業ファイル

- `fork-work/`：分岐したセッション（機器カテゴリの評価 C1〜C3、I5 の照合の確認、パーサの差分検査など）の作業ファイルと解析結果。90MB ずつに分けてある。つなげてから展開する：

  ```bash
  cat fork-work/fork-work.tar.gz.part-* | tar -xzf -
  ```

  S2（Zeek、Suricata との比較）の出力 `fork-work/step2/` は含めていない（下記）。
- `standards/e3-analysis.tar.gz`：E3（TS 103 701 の試験ケースの分類）の作業ファイル。規格の PDF は含めていない（ETSI から入手できる）。

## 含めていないもの

- **S2 の Zeek と Suricata の生のログ**（`fork-work/step2/`）：Quarant と違い、URL のトークンやパスワードなどの値を伏せずに記録しているため。比較の結果は Quarant の `docs/assessment/tool-comparison.md` にある。
- **元の PCAP**：大きさ（合計 30GB 以上）と、各データセットの利用条件のため。配布元から取り直せる。
  - UNSW IoT traces：https://iotanalytics.unsw.edu.au/iottraces.html
  - UNSW IoT attack traces：https://iotanalytics.unsw.edu.au/attack-data.html
  - IoT-23：https://www.stratosphereips.org/datasets-iot23
  - CIC IoT Dataset 2022：https://www.unb.ca/cic/datasets/iotdataset-2022.html
