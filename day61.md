# Linux学習記録 Day61
## log_rotate.sh のBatsテスト追加とログ設定の修正
## 実践内容

- tests/log_rotate.bats を新規作成
- log_rotate.sh の存在確認・実行権限・Bash構文チェックを追加
- log_rotate.sh の実行テストを追加
- rotate.log が生成されることを確認するテストを追加
- テスト実行時に log_rotate.sh が失敗する原因を調査
- LOG_FILE が source より後に定義されていたため、source の前に定義するよう修正
- 重複していた source と LOG_FILE の定義を整理
- bash -n による構文確認を実施
- bats tests/log_rotate.bats を実行し、全5件のテスト成功を確認
- bats tests を実行し、全41件のBatsテスト成功を確認

## 間違えたこと

- LOG_FILE を定義する前に lib/log.sh を読み込んでいたため、`LOG_FILE is not set` エラーが発生した
- log_rotate.sh の修正時に source と LOG_FILE の定義を重複して記述してしまった
- ShellCheckコメントだけが残った状態になっていたため、不要な記述を整理した

## 学んだこと

- set -u を使用している場合、source する前に必要な変数を定義しておく必要がある
- 共通ライブラリを利用するスクリプトでは、変数定義と読み込み順序が重要である
- Batsでは構文確認だけでなく、スクリプト実行や生成ファイルの確認までテストできる
- テストを追加しながら不具合を修正することで、スクリプトの信頼性を高められる
- 全41件のBatsテストが成功し、プロジェクト全体のテスト環境を維持できた
