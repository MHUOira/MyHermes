# Workflow: 01_Quara-English-Research

## 1. 対象ファイル

- 重複確認ログ: `Hermes/cron/jobs/01_Quara-English-Research/logs/processed.log`（Google Drive）
- エラーログ: `Hermes/cron/jobs/01_Quara-English-Research/logs/error.log`（Google Drive）
- レポート出力先（Google Drive）: `Hermes/cron/jobs/01_Quara-English-Research/outputs/YYYY-MM-DD.md`
- ローカル保存先（同一構成）: `D:\MyWork\GPTQ\Hermes\cron\jobs\01_Quara-English-Research\outputs\YYYY-MM-DD.md`

## 2. 新規コンテンツの定義

- `processed.log` に記載されていない URL の記事
- または、タイトルが完全に一致しない記事

## 3. レポートの内容と書式（厳守）
Web検索ツールを使い、QuoraやRedditから新規のcuckoldry experience storiesをリサーチして報告してください。

見出しを「本日の新規事例」とし、以下の構造で報告：

■ 記事タイトル: [タイトル]

[URL]  
https://...

内容要約: （要約）
出典・特徴: （リサーチャーとしての評価）

## 4. 処理フロー（必ずこの順で実行）

1. `GAPI drive download` で `processed.log` を取得（失敗時はスキップ）
2. **重複チェックは `processed.log` の記録を踏まえて判断**
3. Web検索で新規事例を抽出
4. レポート内容を生成（指定書式厳守）
5. **ローカルにも同じ内容のレポートを保存**（フォルダ構成・命名則は Google Drive と同一）
6. `GAPI drive upload` でレポートをアップロード（**必ず実行**）
7. **ログに記述する事項と書式**:
   - フォーマット: `レポートID|URL|タイトル|処理日時`
   - 例: `report_20240617_001|https://...|タイトル|2026-06-17T15:30:00+09:00`
8. `processed.log` に追記してアップロード（**必ず実行**）

## 5. エラーハンドリング

- `processed.log` 取得失敗 → 重複チェックをスキップして続行
- **GAPI upload 失敗**:
  - **無条件にローカルファイルを残す**
  - エラーメッセージとローカルファイルへのリンクを `error.log` に記録
  - 可能な限り反復リトライ
  - 遂行できなかった場合はその経緯を `error.log` に記録
- その他エラー → 可能な限り継続し、ログに記録

## 6. ログ設計

- `processed.log`: 重複管理専用（成功したレポートのみ記録）
- `error.log`: エラー・リトライ履歴専用（別ファイルで管理）

## 7. 変更履歴

- 2026-06-17: パス修正（Google Drive パスを Hermes/ から開始する正しい形式に修正）
