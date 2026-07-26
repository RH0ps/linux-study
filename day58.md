# Linux学習記録 Day58
## disk_monitor.shの動作テスト追加とmacOS対応
## 実践内容
- `disk_monitor.bats` に動作テストを追加した。
- `disk_monitor.sh` が正常終了することを確認するテストを追加した。
- `metrics/disk.prom` が生成されることを確認するテストを追加した。
- `disk.prom` に `disk_usage_percent` が含まれていることを確認するテストを追加した。
- `disk_monitor.sh` を実行したところ、macOSでは `flock` コマンドが存在しないため、ロック処理で終了していることを確認した。
- `disk_monitor.sh` のロック処理を修正し、`flock` が利用できる環境ではロックを行い、利用できない環境ではロックをスキップするよう改善した。
- 修正後に `disk_monitor.sh` を実行し、`metrics/disk.prom` が正常に生成されることを確認した。
- Batsで全テストを実行し、23件すべて成功することを確認した。

## 間違えたこと
- `disk.prom` が作成されない原因をテストコードの問題だと考えていたが、実際は `flock` が存在しないためスクリプトが途中で終了していた。
- `disk_monitor.sh` がメトリクスを出力していないと思っていたが、コードを確認すると `disk.prom` を生成する処理は実装されていた。

## 学んだこと
- テストが失敗した場合は、テストコードだけでなく実際のスクリプトの動作も確認することが重要である。
- `command -v` を利用すると、コマンドの有無を判定できる。
- macOSとLinuxでは利用できるコマンドが異なるため、環境差を考慮した実装が必要である。
- Batsでは動作確認だけでなく、生成されるファイルや内容まで自動で検証できる。

## 覚えたこと・コマンド
```bash
# ディスク監視スクリプト実行
bash disk_monitor.sh

# metricsディレクトリ確認
ls -l metrics

# ファイル検索
find . -name "disk.prom"

# メトリクス関連の処理を検索
grep -n "disk.prom\|metrics\|prom\|echo\|printf" disk_monitor.sh

# コマンドの存在確認
command -v flock

# Batsテスト実行
bats tests
```