# 使用するファイルの所在を既定する。

# パスの設定
## workdirが設定されている場合
カレントを.と表記して、.をルートと見做す。
## workdirが未設定の場合
カレント：`D:\MyWork\GPTQ\Hermes\cron\jobs\03_Test-Report-Workflow\`
以下の.をカレントと見做す。

# ファイルの設定
- 重複確認ログ: `.\logs\processed.log`
- エラーログ: `.\logs\error.log`
- デバッグログ: `.\logs\debug.log`
- レポート出力先: `.\outputs\YYYY-MM-DD.md`
