# 01_Quara-English-Research

Quora / Reddit から「寝取られ体験談」を英語で収集し、日本語訳を付与する調査レポート用フォルダ。

## 目的
- 毎日4:30に自動実行されるcronジョブの出力管理
- 英語原文 + 日本語逐語訳のレポートを日付ごとに保存
- 過去の調査結果の蓄積と参照

## 運用ルール
- 出力ファイル: `outputs/YYYY-MM-DD.md`
- エラーログ: `logs/` に日付付きで保存
- プロンプトは `prompt.md` に最新版を保存
- レポートは `research/quora-cuckoldry-reports/` へ月次で移動・まとめ可能

## 変更履歴
- 2026-06-14: 初版作成（フォルダ名を 01_Quara-English-Research に変更）