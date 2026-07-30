# Linux学習記録 Day59
## backup.sh の自動テスト追加（Bats）

- backup.sh 用の Bats テストファイル（tests/backup.bats）を新規作成
- setup() を追加し、ROOT パスを共通化
- backup.sh の存在確認テストを追加
- backup.sh の実行権限確認テストを追加
- backup.sh の Bash 構文チェックを追加
- backup.sh が正常終了することを確認するテストを追加
- backup.prom が生成されることを確認するテストを追加
- backup_success_total メトリクスが出力されることを確認するテストを追加
- backup_failure_total メトリクスが出力されることを確認するテストを追加
- DRY_RUN=true で正常終了することを確認するテストを追加
- index.html のバックアップファイルが作成されることを確認するテストを追加
- docker-compose.yml のバックアップファイルが作成されることを確認するテストを追加

## テスト実行・動作確認

- backup.bats 単体で実行し、全10テスト成功を確認
- bats tests を実行し、全33テスト成功を確認
- backup.sh の既存機能を壊していないことを確認
- backup.sh の Prometheus メトリクス生成を確認
- バックアップファイル生成を自動テストで検証できるよう改善

## 学んだこと

- Bats の setup() を利用して共通処理をまとめる方法
- run を利用したコマンド実行結果の検証方法
- ファイル生成を find コマンドで確認する方法
- メトリクスファイルの内容を grep で検証する方法
- Bash スクリプトの自動テストを段階的に追加する流れを学んだ
- bats tests に新しいテストを追加すると、自動的に CI のテスト対象にも含まれることを確認した

# bats tests実行結果
```bash
1..10
ok 1 backup.sh exists
ok 2 backup.sh is executable
ok 3 backup.sh syntax
ok 4 backup.sh runs successfully
ok 5 backup.prom is created
ok 6 backup.prom contains backup_success_total
ok 7 backup.prom contains backup_failure_total
ok 8 backup.sh supports DRY_RUN
ok 9 backup directory contains index backup
ok 10 backup directory contains docker-compose backup
r.h@ishikawarihonoMacBook-Air ops-lab % bats tests
1..33
ok 1 backup.sh exists
ok 2 backup.sh is executable
ok 3 backup.sh syntax
ok 4 backup.sh runs successfully
ok 5 backup.prom is created
ok 6 backup.prom contains backup_success_total
ok 7 backup.prom contains backup_failure_total
ok 8 backup.sh supports DRY_RUN
ok 9 backup directory contains index backup
ok 10 backup directory contains docker-compose backup
ok 11 cpu_monitor.sh exists
ok 12 cpu_monitor.sh is executable
ok 13 cpu_monitor.sh syntax
ok 14 disk_monitor.sh exists
ok 15 disk_monitor.sh is executable
ok 16 disk_monitor.sh syntax
ok 17 disk_monitor.sh runs successfully
ok 18 disk.prom is created
ok 19 disk.prom contains disk_usage_percent
ok 20 health_check.sh exists
ok 21 health_check.sh is executable
ok 22 health_check.sh syntax
ok 23 memory_monitor.sh exists
ok 24 memory_monitor.sh is executable
ok 25 memory_monitor.sh syntax
ok 26 ops help exits successfully
ok 27 ops version exits successfully
ok 28 unknown command returns error
ok 29 unknown command shows message
ok 30 help contains backup command
ok 31 process_monitor.sh exists
ok 32 process_monitor.sh is executable
ok 33 process_monitor.sh syntax
```
