# Workflow: 01_Quara-English-Research

**デバッグ用追加指示（原因特定のため一時的に追記）**:
- 今回の実行において、利用可能なツール（関数）の一覧をすべてリストアップして記述してください。
- `workflow.md` の読み込みに成功したかどうか、またそこに記載された重複ログファイルが正常に読み込めたかを明記してください。
- 実際にどのようなキーワードでWeb検索（Quora/Reddit等）を試みたか、その検索クエリとヒット件数を履歴として出力してください。
- 新規情報が0件だった場合でも、上記のデバッグ情報（ツール一覧、検索ログ、読み込み成否）を必ず `Response` に含めて出力し、決して `[SILENT]` で終わらせないでください。
- web_extractで失敗した場合には、その理由を記録してください。今はsomething wrongとしか出ていないので原因が特定できない。詳しい理由を出力してください。
- browser経由での記事取得にフォールバックしたか？しなかったのであればその理由を記録して。
 【最重要・強制デバッグモード（SILENT完全禁止）】

- この実行では以下の情報を必ず最終Responseに含めること。0件・エラー時も例外なく出力せよ。絶対にSILENTにしない。
    必須出力項目:
    1. 利用可能なツール（関数）の一覧（全ツール名）
    2. workflow.md の読み込み成否 + 読み込んだ主なルール確認
    3. 実行した全検索クエリと各々のヒット件数（web_search の履歴）
    4. 各主要ステップの成否（processed.log取得、browser_navigate、web_extract、GAPI upload、ローカル保存）
    5. 新規抽出件数と、0件だった場合の理由
    6. ローカルファイル書き込みの成否 + 書き込んだファイルパス
    7. 発生したエラーの詳細（web_extract失敗時などはエラーメッセージ全文）

    上記を最後に「実行サマリーブロック」として verbatim で出力すること。

## 1. 対象ファイル

- 重複確認ログ: `Hermes/cron/jobs/01_Quara-English-Research/logs/processed.log`（Google Drive）
- エラーログ: `Hermes/cron/jobs/01_Quara-English-Research/logs/error.log`（Google Drive）
- レポート出力先（Google Drive）: `Hermes/cron/jobs/01_Quara-English-Research/outputs/YYYY-MM-DD.md`
- ローカル保存先（同一構成）: `D:\MyWork\GPTQ\Hermes\cron\jobs\01_Quara-English-Research\outputs\YYYY-MM-DD.md`

## 2. 新規コンテンツの定義

- `processed.log` に記載されていない URL の記事
- または、タイトルが完全に一致しない記事

## 3. レポートの内容と書式（厳守）

Web検索ツールを使い、QuoraやRedditから新規のcuckoldry experience storiesをリサーチしてください。

**必ず以下の手順で精査・記述すること（これを最優先とする）**:
1. 各記事の本文を可能な限り取得する
2. 主要なエピソード部分を**原文のまま長めに抜粋**する（要約だけで終わらせない）
3. 抜粋した原文に対して**逐語訳（直訳）**を付ける
4. 感情の変化・時間経過・身体的描写・屈辱要素・心理描写を丁寧に分析する

見出しを「本日の新規事例」とし、**各事例ごとに以下の構造を厳守**して報告：

■ 記事タイトル: [タイトル]

**ソース**:
[URL]  
https://...（実際の記事URLをここに記載。research-report-output-formatting skill のルールに従い、[URL] の後に半角スペース2つを入れて改行し、URLを記載）

**原文（抜粋）:**
（ここに実際の投稿本文から重要な部分を長めに引用。短く切りすぎず、情景が伝わる程度の長さを確保）

**逐語訳（直訳）:**
（上記の原文をできるだけ逐語的に日本語に訳す。意訳を避け、直訳を優先）

**内容要約:**
（感情の推移、時間経過、身体的・心理的描写、屈辱や興奮のポイントを交えて詳細にまとめる。表面的な要約は禁止）

**出典・特徴:**
（投稿の特徴や価値、リサーチャーとしての評価。なぜこの事例が注目に値するかを述べる）

**注意事項（厳守）**
- 要約だけで終わらせず、必ず「原文＋逐語訳」をセットで入れる
- 分析は「何が起こったか」だけでなく、「どのように感じたか」「身体的・感情的にどう変化したか」を重視する
- 短い要約ではなく、読者がその場の雰囲気や心理を想像できるレベルで記述する

## 4. 処理フロー（必ずこの順で実行）

1. processed.log の存在を確認する
   - ローカルに存在する場合はそのまま使用
   - 存在しない場合は空の processed.log をローカルに作成

2. Google Drive API で最新の processed.log を取得（存在する場合のみ上書き）
   - 取得に失敗してもローカルファイルを優先

3. processed.log の内容を確認して、既に存在するレポートの URL を一覧化

4. Web検索で新規事例を抽出（processed.log に記載されていないもの）
### コンテンツ取得の優先順位:
(1) browser_navigate + browser_snapshot（full=true） を第一選択とする（Quora / Reddit 共通）  
(2) web_extract は補助的に使用（または Quora/Reddit では使用しない）  
(3) どうしても browser が不安定な場合は web_extract にフォールバック  
### 具体的な取得手順:
(1) web_search で URL を取得  
(2) 各 URL に対して browser_navigate を実行  
(3) browser_snapshot（full=true）でページ内容を取得  
(4) 取得した内容から「主要なエピソード部分を原文のまま長めに抜粋」できるかを判断  
(5) 十分な長文が取れなかった場合は、browser_scroll (direction=down)や browser_snapshot (full=true)を再度実行して追加取得を試みる  
(6) (5)の実行は3回まで繰り返す。
### 失敗時の扱い:
(1) browser_navigate / browser_snapshot が失敗した場合：最大3回までリトライ  
(2) それでも長文抜粋が取れない場合は error.log に詳細を記録し、0件として扱うことを許可  
(3) Reddit は web_extract が完全にサポート外のため、最初から browser_navigate を使用する  

5. 既存レポートとの重複・競合をチェック

6. レポート内容を生成（指定書式厳守）
   - 上記「3. レポートの内容と書式」に記載された手順を厳守
   - 各事例ごとに「原文抜粋 → 逐語訳 → 詳細分析」の3層構造を必ず守る

7. **ローカル保存を最優先で実行**
   - レポートを `D:\MyWork\GPTQ\Hermes\cron\jobs\01_Quara-English-Research\outputs\YYYY-MM-DD.md` に保存
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


**ローカルファイル書き込み関連**
- `outputs/YYYY-MM-DD.md` または processed.log のローカル書き込みに失敗した場合:
  - 即時中止し、critical error として扱う
  - `error.log` に「[timestamp] LOCAL WRITE FAILED: [詳細]」を追記
  - ユーザーに手動確認を促すメッセージを出力

**新規コンテンツ0件の場合**
- レポートファイルは作成し、見出し「本日の新規事例: 0件」と記載
  - processed.log への新規エントリ追記は行わない
  - Quoraジョブの場合のみ、デバッグ情報を含めて出力（[SILENT] は使用禁止）

**その他予期しないエラー**
- 可能な限り処理を継続
- すべてのエラーは `error.log` にタイムスタンプ付きで記録
- 重大なエラー以外は cron を停止させない

**processed.log のエントリ形式（厳守）**
各行: `YYYY-MM-DD|URL|記事タイトル`（例: 2026-06-19|https://quora.com/...|My cuckoldry experience...）
重複判定は URL と タイトル の完全一致で行う。

## 6. ログ設計

- `processed.log`: 重複管理専用（成功したレポートのみ記録）
- `error.log`: エラー・リトライ履歴専用（別ファイルで管理）

## 7. 変更履歴

- 2026-06-17: パス修正（Google Drive パスを Hermes/ から開始する正しい形式に修正）
- 2026-06-17: processed.log の取得失敗時に既存ファイルを上書きしないロジックを追加
- 2026-06-17: ローカル保存を Google Drive 成否に関わらず最優先で実行するよう変更
- 2026-06-17: processed.log の存在確認を最初に行い、初回作成を明示する手順に再構成
- 2026-06-17: レポート書式を大幅強化（原文長め抜粋＋逐語訳＋詳細分析を必須化）
```