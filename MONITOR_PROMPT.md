# チャット-2（監視役）：1 回の監視の手順

オンコール（本人が起動）か、`/loop 2h MONITOR_PROMPT.md を読み、その手順で 1 回監視してください` で呼ばれる。**読み取り専用**。チャット-1 の作業には介入しない（MONITOR_REPORT.md と、必要なときの HALT.md を書くだけ）。

## 24 時間運用での追加の確認（RULES_24H.md）
- HEARTBEAT.txt の時刻：3 時間以上更新が無ければ「チャット-1 が止まっている」（HALT.md は作らない。本人に報告）
- 使用量：~/.claude.json の five_hour の使用率 U（配分はコンペ 4 割・研究 6 割。RULES_24H.md の 3）。コンペ側は U が 40% 以上で停止、30〜40% で節約。停止の領域なのに、HEARTBEAT が「停止中」でなく作業を続けていたら「注意」。`extra_usage.is_enabled` が true なら「要停止」
- Z890 の status.txt（`ssh Z890 cat /data/nakamura/comp/status/status.txt`）、watchdog.log の末尾、kaggle_ntl.txt（提出の状況）
- 直近 3 時間に failed/ に入ったジョブが、LOG.md に書かれているか（見落としの確認）
- 提出が、RULES_24H.md の時間帯（8:00〜22:00）と条件の外で行われていないか
- 夜間（22:00〜8:00）に、外部への書き込み（提出・公開・コメント）が行われていないか

## やること（毎回）

1. 読む：SCOPE.md（ずれの定義・範囲外）、NEXT.md、LOG.md の直近 5 件。
2. 実際の状態を確認する（読み取りのみ）：
   - `ssh Z890` で、queue/logs/runner.log の末尾、pending/ と failed/ の一覧、nvidia-smi、実行中のジョブのログの進み具合
   - `KAGGLE_API_TOKEN=$(cat /data/nakamura/.kaggle/access_token)` を使い、`kaggle competitions submissions name-that-llm`（提出の履歴と公開スコア）
   - Z890 の /data/nakamura/comp/ の外を、読み書きしていないか（LOG.md と照合）
3. SCOPE.md の「ずれ」1〜7 を、1 つずつ判定する。LOG.md の数値と、ジョブのログ・提出結果の数値を照合する（6 番）。
4. MONITOR_REPORT.md の先頭に、次の形で追記する（新しいものを上に）。

```
## YYYY-MM-DD HH:MM の監視
- 判定：問題なし ／ 注意 ／ 要停止
- ずれ 1〜7 のうち該当：（なければ「なし」）
- 根拠：（ファイル・行・数値）
- 状態：GPU（使用中／遊休 N 分）、キュー、提出枠（今日の残り）、Kaggle の最新スコア
- チャット-1 へのお願い：（あれば。1〜3 個）
```

5. **要停止**のとき（範囲外の操作、費用や規約に触れる可能性、削除、同じ失敗の反復など、実害が出うるもの）だけ、C:\Users\Nakamura\comp_project\HALT.md を作る。中身は、理由、根拠、再開の条件。それ以外は HALT.md を作らない（「注意」までは報告のみ）。
6. 本人への報告を 5 行以内で行う。

## やらないこと
- ジョブの停止・削除・再実行、提出、設定の変更、ファイルの編集（MONITOR_REPORT.md と HALT.md 以外）
- チャット-1 の代わりの作業
- 費用が発生する操作
