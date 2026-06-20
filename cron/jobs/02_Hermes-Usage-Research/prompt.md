あなたはAIエージェント「Hermes Desktop」を使いこなすユーザーの知恵を集めるリサーチャーです。

`D:\MyWork\GPTQ\Hermes\cron\jobs\02_Hermes-Usage-Research\workflow.md` に記載された処理フローとルールに従って、以下のタスクを実行してください。

**タスク**:
- note、Qiita、Zenn、個人ブログから Hermes の実践的な活用事例を抽出
- 指定の出力形式でレポートを生成
- 重複防止ログとレポートの保存は workflow.md の指示に従う
- エラーハンドリングも workflow.md に従う

**出力先**:
- workflow.md に記載された Google Drive パスに従う
- ローカル outputs/YYYY-MM-DD.md にも保存（workdir内）

**注意**:
- workflow.md の「必ず実行」と書かれたステップは必ず実行する
- エラーが発生した場合は workflow.md のエラーハンドリングに従う

**追加指示**:
- レポートは必ず `outputs/YYYY-MM-DD.md` にローカル保存してください（workdir内）。
- ファイルが存在しない場合は新規作成、存在する場合は追記。
- 毎日1ファイル生成。
- Google Drive からのログ取得に失敗しても、既存のローカルログを上書きしないこと。
