# quarant-results

[Quarant](https://github.com/SaeYoshizaki/Quarant)で、公開データセットを解析した結果の保管場所。

## 中身

`runs/` の各ファイルは、1 回の全データの解析。展開すると、キャプチャ 1 つごとにフォルダがあり、`events.jsonl`（検出したイベント）、`device_inventory.json`（機器の一覧と識別の根拠）、`findings.json`（機器 × 状態の要約）が入っている。

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

<!-- キャプチャの名前の付け方（フォルダ名）：`unsw-16-*`（UNSW IoT traces 2016）、`unswatk-18-*`（UNSW IoT attack traces 2018）、`iot23-*`（IoT-23）、`cic-1-power-*`、`cic-2-idle-*`、`cic-3-interactions-*`、`cic-4-scenarios-*`、`cic-5-active-*`、`cic-6-rtsp-*`（CIC IoT 2022）。） -->

## 2026-10-25 に追加：OSS を組み込んだ版（Quarant-oss）

[Quarant-oss](https://github.com/SaeYoshizaki/Quarant-oss)（Quarant に Zeek と Suricata を組み込んだ版）の解析結果。まとめは Quarant-oss の `docs/REPORT-2026-10.md`。大きいファイルは 90MB ずつに分けてある。つなげてから展開する：

```bash
cat runs/runs-oss1.tar.gz.part-* | tar -xzf -
```

| ファイル | Quarant-oss | 中身 | 使った実験（Quarant-oss の docs/EXPERIMENTS.md） |
|---|---|---|---|
| `runs/runs-oss1.tar.gz.part-*` | `46138a4` | 全データ（UNSW 2016 の 20 日、CIC IoT 2022 の 1,050 キャプチャ、YourThings 2018 の 4 日）。キャプチャごとに `q/`（Quarant 単体）、`z/`（Zeek 経由）、`m/`（重ねたもの）。`manifest.txt` に元のキャプチャの SHA-256 | `docs/assessment/integration-rates.md` |
| `runs/runs-oss1-iotraffic.tar.gz.part-*` | `46138a4` | UNSW-IoTraffic（27 台、機器ごとのキャプチャ）。同じ形 | 同上 |
| `countermeasure/o6-o7.tar.gz.part-*` | `f073e74`〜`46138a4` | 対策の試算の最初の版（YourThings 3/20 の Zeek 経由、3/20 の計画を 4/12 に当てたもの）。外から始まった接続の取り違えがある前の数字で、O8 で測り直した。同梱の `plan-0320.json`、`plan-0412.json` は `6c81b1f` で作り直したもの | O6、O7 |
| `countermeasure/o8.tar.gz.part-*` | `7b1f606` | 普通のルーター、一律の対策、Quarant の比較（YourThings、3/20 で学習、4/12 に当てる）。条件ごとの解析結果（`m/`）と `filter.stats.json`（止めたパケット、上位の宛先）、許可リスト | O8 |
| `countermeasure/o9.tar.gz.part-*` | `6633222` | UNSW-IoTraffic で、2016 年 10 月で学習し、1〜5 か月後の週に当てたもの。期間ごとの計画、許可リスト、条件ごとの結果、`effect.md` | O9 |
| `runs/runs-yt5.tar.gz.part-*` | Quarant `682144f` | YourThings 4/10〜12 の以前の解析 | E14、O1 |
| `ground-truth/gt-yt.tar.gz.part-*` | — | YourThings の見逃しの正解（Zeek と生パケットの検索。名前だけで、値は含まない） | E14 |

キャプチャ（PCAP）と、`flows.jsonl`、`observations.jsonl` は含めていない。

## そのほかの作業ファイル

- `fork-work/`：分岐したセッション（機器カテゴリの評価 C1〜C3、I5 の照合の確認、パーサの差分検査など）の作業ファイルと解析結果。90MB ずつに分けてある。つなげてから展開する：

  ```bash
  cat fork-work/fork-work.tar.gz.part-* | tar -xzf -
  ```

  S2（Zeek、Suricata との比較）の出力 `fork-work/step2/` は含めていない（下記）。
- `standards/e3-analysis.tar.gz`：E3（TS 103 701 の試験ケースの分類）の作業ファイル。規格の PDF は含めていない（ETSI から入手できる）。
- **元の PCAP**：
  - UNSW IoT traces：https://iotanalytics.unsw.edu.au/iottraces.html
  - UNSW IoT attack traces：https://iotanalytics.unsw.edu.au/attack-data.html
  - IoT-23：https://www.stratosphereips.org/datasets-iot23
  - CIC IoT Dataset 2022：https://www.unb.ca/cic/datasets/iotdataset-2022.html
