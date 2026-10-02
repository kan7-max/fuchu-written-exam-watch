# 府中・本免学科試験のクラウド監視

GitHub Actionsが府中運転免許試験場の本免学科試験枠を確認します。空きが出たら公開のGitHub Issueを作り、空きの通知用コミットを1件だけプッシュします。リポジトリの「Email notifications for pushes」に登録したアドレスへ、GitHubからメールが届きます。PCを閉じても実行できます。予約は行いません。

GitHub標準の定期実行は最短5分で、実行時刻は保証されません。このリポジトリは5分間隔のベストエフォートで確認します。予約システム側の自動照会許可を確認できていないため、外部スケジューラによる毎分照会は設定していません。

## セットアップと確認

1. **Settings → Email notifications** に通知先のメールアドレスを登録します。GitHubは1リポジトリあたり最大2アドレスへ、プッシュ時のメールを送れます。
2. **Actions → Fuchu written exam slot watch → Run workflow** で `test_email` を有効にして実行すると、テストメール用の空コミットを1件だけプッシュします。受信を確認してください。
3. **Settings → Secrets and variables → Actions → Variables** に `ENABLE_WATCH=true` を登録します。定期監視が始まります。`run_watch` を有効にして手動確認もできます。

Gmail・iCloudのパスワードやメールAPIキーは不要です。GitHub Actions付属の `GITHUB_TOKEN` でIssue作成と通知用の空コミットを行います。

日付を絞る場合はワークフロー内の `START_DATE` と `END_DATE` に `YYYY-MM-DD` を設定します。未設定なら実行日から30日先までで、30日を超える日は通知しません。午前だけなら `TIME_OF_DAY: morning`、午後だけなら `TIME_OF_DAY: afternoon` とします。

## 運用上の注意

- リポジトリとIssueは公開です。空き枠の日時はIssueと通知用コミットのメッセージに表示されます。個人情報や認証情報は書き込みません。
- GitHubのプッシュ通知メールはリポジトリのEmail notifications設定、メールの振り分け、GitHub側の配信状況に依存します。[公式設定の説明](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/about-email-notifications-for-pushes-to-your-repository)
- GitHubの定期実行は遅延する場合があり、時刻どおりの通知は保証されません。公開リポジトリでは60日間リポジトリに活動がないと定期実行が無効になります。[スケジュールの仕様](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)
- 監視は受験条件に合う方だけが使用してください。[警視庁の学科試験案内](https://www.keishicho.metro.tokyo.lg.jp/menkyo/menkyo/web.html)
