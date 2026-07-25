# Linux学習記録 Day57
## Batsを用いたテスト環境の構築・CLIテスト追加
## 実践内容
- Bats（Bash Automated Testing System）を使用したテスト環境を構築した。
- `tests` ディレクトリを整備し、各監視スクリプトのテストを作成した。
- `health_check.bats` を実行場所に依存しないよう、`$BATS_TEST_DIRNAME` を利用したパス指定へ修正した。
- `disk_monitor.sh`、`cpu_monitor.sh`、`memory_monitor.sh`、`process_monitor.sh` に対して、以下の3項目をテストした。
  - ファイルの存在確認
  - 実行権限の確認
  - Bash構文チェック
- `ops.bats` を作成し、CLIの動作テストを追加した。
  - `ops help`
  - `ops version`
  - 存在しないコマンド実行時の終了コード
  - エラーメッセージ表示
  - helpに`backup`コマンドが表示されるか
- `ops` コマンドのUnknown command処理に `exit 1` を追加し、異常終了時に適切な終了コードを返すよう改善した。
- すべてのテストを実行し、合計20件すべて成功することを確認した。

## 間違えたこと
- `tests` ディレクトリ内で `bats tests` を実行し、`tests/tests` を探してしまいエラーになった。
- `health_check.bats` のパスを `../health_check.sh` に変更したが、プロジェクトルートから実行すると正しく参照できなかった。
- `ops back` が動かない原因をコードだと思っていたが、実際はDocker Desktopが起動していなかった。
- `ops.bats` で文字列を完全一致（`== "backup"`、`== "Unknown command"`）で比較していたため、実際の複数行出力と一致せずテストが失敗した。
- Unknown commandで終了コードが0になっており、異常終了として扱われていなかった。

## 学んだこと
- Batsでは `run` を使用して終了コードや標準出力をテストできる。
- `"$BATS_TEST_DIRNAME"` を利用すると、実行場所に依存しないテストを作成できる。
- CLIは正常系だけでなく、異常系（存在しないコマンドなど）のテストも重要である。
- Bashではエラー時に `exit 1` を返すことで、CIやテストから異常終了を判定できる。
- 出力内容をテストするときは、完全一致ではなく部分一致（ワイルドカード）を利用した方が実際のCLI出力に対応しやすい。

## 覚えたこと・コマンド
```bash
# Bats実行
bats tests

# 現在のディレクトリのテスト実行
bats .

# Bash構文チェック
bash -n ファイル名

# Dockerコンテナ一覧
docker ps


# Docker Desktop起動（macOS）
open -a Docker

# テストファイル内でプロジェクトルートを取得
ROOT="$(cd "$BATS_TEST_DIRNAME/.." && pwd)"

# Bashで異常終了を返す
exit 1
```
