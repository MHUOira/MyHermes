# Workflow: 02_Hermes-Usage-Research

## 1. 対象ファイル

- 重複確認ログ: `Hermes/cron/jobs/02_Hermes-Usage-Research/logs/processed.log`（Google Drive）
- エラーログ: `Hermes/cron/jobs/02_Hermes-Usage-Research/logs/error.log`（Google Drive）
- レポート出力先（Google Drive）: `Hermes/cron/jobs/02_Hermes-Usage-Research/outputs/YYYY-MM-DD.md`
- ローカル保存先（同一構成）: `D:\\MyWork\\GPTQ\\Hermes\\cron\\jobs\\02_Hermes-Usage-Research\\outputs\\YYYY-MM-DD.md`

## 2. 新規コンテンツの定義

- `processed.log` に記載されていない URL の記事
- または、タイトルが完全に一致しない記事

**processed.log のエントリ形式（厳守）**
各行: `YYYY-MM-DD|URL|記事タイトル`（例: 2026-06-19|https://note.com/...|Hermes Desktopを業務で使ってみた）
重複判定は URL と タイトル の完全一致で行う。

## 3. レポートの内容と書式（厳守）
Web検索ツールを使い、note、Qiita、Zenn、または個人の技術・ガジェットブログから、Hermes Desktop（またはHermes Agent）の実践的な活用事例をリサーチして上記で定義された新規コンテンツの内容を報告してください。

[検索ルールの厳守]
- "Hermes Desktop" または "Hermes Agent" を必須キーワードとする
- 「使ってみた」「自動化」「プロンプト」「連携」「便利」「業務効率化」などの実践寄りキーワードを組み合わせる
- 国内記事を優先

見出しを「本日の注目活用事例」とし、以下の構造で報告：

■ 記事タイトル: [タイトル]

[URL]  
https://...

活用内容・工夫点: （要約）
ここが素晴らしい: （リサーチャーとしての評価）

## 4. 処理フロー（必ずこの順で実行）

1. processed.log の存在を確認する
   - ローカルに存在する場合はそのまま使用
   - 存在しない場合は空の processed.log をローカルに作成

2. Google Drive API で最新の processed.log を取得（存在する場合のみ上書き）
   - 取得に失敗してもローカルファイルを優先

3. processed.log の内容を確認して、既に存在するレポートの URL を一覧化

4. Web検索で新規事例を抽出（processed.log に記載されていないもの）

5. 既存レポートとの重複・競合をチェック

6. レポート内容を生成（指定書式厳守）

7. **ローカル保存を最優先で実行**
   - レポートを `D:\\MyWork\\GPTQ\\Hermes\\cron\\jobs\\02_Hermes-Usage-Research\\outputs\\YYYY-MM-DD.md` に保存
   - processed.log に新しいエントリを追記（ローカル）

8. Google Drive API でレポートと processed.log をアップロード（**必ず実行**）

## 5. エラーハンドリング（各失敗モードごとの具体的なフォールバック — 必ずこのルールに従う）

**processed.log 関連**
- Google Drive からの processed.log 取得/ダウンロードに失敗した場合:
  - ローカルに既存の `processed.log` が存在すれば、それを優先して使用（決して上書きしない）
  - ローカルにも存在しない場合は、空の processed.log をローカルに新規作成
  - `error.log` に「[timestamp] processed.log 取得失敗: [エラーメッセージ全文]」を追記
- processed.log の内容解析に失敗した場合（フォーマット不正など）:
  - 空として扱い、処理を続行（新規作成扱い）

**Google Drive アップロード関連**
- レポートまたは processed.log の Google Drive API アップロードに失敗した場合:
  - ローカル保存はすでに完了しているため、ファイルをそのまま残す（非破壊優先）
  - `error.log` に「[timestamp] GAPI upload 失敗: [エラーメッセージ全文] [対象ファイル: フルパス]」を追記
  - 以降の実行でもローカルファイルを優先し、手動介入は最小限に

**検索・コンテンツ取得関連**
- Web検索、note/Qiita/Zenn/技術ブログ取得、ブラウザ操作に失敗した場合:
  - 最大3回までリトライを試行
  - それでも失敗した場合は `error.log` に記録
  - 利用可能なデータのみでレポートを生成（新規0件として扱うことを許可）

**ローカルファイル書き込み関連**
- `outputs/YYYY-MM-DD.md` または processed.log のローカル書き込みに失敗した場合:
  - 即時中止し、critical error として扱う
  - `error.log` に「[timestamp] LOCAL WRITE FAILED: [詳細]」を追記
  - ユーザーに手動確認を促すメッセージを出力

**新規コンテンツ0件の場合**
- レポートファイルは作成し、見出し「本日の注目活用事例: 0件」と記載
  - processed.log への新規エントリ追記は行わない

**その他予期しないエラー**
- 可能な限り処理を継続
- すべてのエラーは `error.log` にタイムスタンプ付きで記録
- 重大なエラー以外は cron を停止させない

## 6. ログ設計

- `processed.log`: 重複管理専用（成功したレポートのみ記録）
- `error.log`: エラー・リトライ履歴専用（別ファイルで管理）

## 7. 変更履歴

- 2026-06-17: パス修正（Google Drive パスを Hermes/ から開始する正しい形式に修正）
- 2026-06-17: processed.log の取得失敗時に既存ファイルを上書きしないロジックを追加
- 2026-06-17: ローカル保存を Google Drive 成否に関わらず最優先で実行するよう変更
- 2026-06-17: processed.log の存在確認を最初に行い、初回作成を明示する手順に再構成
- 2026-06-19: エラーハンドリングを大幅強化（各失敗モードごとの具体的なフォールバックを追加、processed.log のエントリ形式を明記、local write failure と 0件ケースを追加）。cron-job-workflow-design skill のテンプレートにさらに適合させるため。