# チャット-1（実行役）：1 回の手順

`/loop LOOP_PROMPT.md を読み、その手順で 1 ステップ進めてください` で呼ばれる。毎回、次の順に行う。

## -1. 24 時間運用のルール（RULES_24H.md が、この手順より優先）
- 毎回の最初に RULES_24H.md の「3. 使用量の配分と上限」と「3-2 自動再開」を実行する（配分はコンペ 4 割・研究 6 割。共通の使用率 U が 30% 以上で節約、40% 以上で停止して、リセットで自動再開。キャッシュが古いときは U を 35% とみなす）。起動のたびに、使用率の前後を LOG.md に書く。モデルと effort は「12」に従う。
- 毎回の最初に `git pull --no-rebase`、毎回の最後に STATUS_SNAPSHOT.md と HEARTBEAT.txt を上書きして、`*.md` と HEARTBEAT.txt だけを指定して add、commit、`git push origin master`（RULES_24H.md の「13」。`git add -A` と強制 push は禁止、MONITOR_REPORT.md と HALT.md は編集しない）。
- ジョブを出すときは、RULES_24H.md の「8. ジョブの出し方」に従う（一時ファイルに書き、構文確認し、mv。入れたら 2 分以内に runner.log を確認）。
- 起動のたびに、Z890 の status.txt（`ssh Z890 cat /data/nakamura/comp/status/status.txt`）と failed/ の新しい失敗を確認する。

## 0. 止まる条件の確認（最初に）
- C:\Users\Nakamura\comp_project\HALT.md があれば、何もせず「HALT.md があるので停止」と報告して、ループを止める。HALT.md は削除しない（本人だけが削除する）。

## 1. 読む
- SCOPE.md（範囲外とずれの定義を含む）、NEXT.md の先頭の未完了の項目、LOG.md の末尾 3 件、MONITOR_REPORT.md（あれば最新）。
- MONITOR_REPORT.md に指摘があれば、NEXT.md の先頭より先に対応する（対応できないものは LOG.md に理由を書く）。

## 2. 状態を確認する（読み取りのみ）
- `ssh Z890` で、ジョブキューの状態（queue/logs/runner.log、pending/）、GPU（nvidia-smi）、実行中のジョブの進み具合を見る。
- Kaggle の提出（`kaggle competitions submissions name-that-llm`）。トークンは `KAGGLE_API_TOKEN=$(cat /data/nakamura/.kaggle/access_token)`。

## 3. 進める
- ジョブの完了待ちなら、結果を確認して、小ステップを完了にする。待ちが残るなら、ScheduleWakeup で、待つ時間に見合った間隔（実行中のジョブの残り時間に近い値）で次の呼び出しを予約する。短い間隔で空回りしない。
- 実行できる小ステップがあれば、実行する。GPU が空いていて、NEXT.md に GPU を使う項目があれば、キューに入れる（ジョブの末尾でサーバーなどの常駐プロセスを必ず止める）。
- 範囲外の操作は、どれも行わない（費用、削除、Kaggle の Join・規約・公開、患者データなど）。迷ったら行わず、LOG.md に書いて止める。

## 4. 終わるときに必ず行う
1. LOG.md に追記（形式は LOG.md の末尾）。結論は、ジョブのログの数値と合っていること。
2. NEXT.md を更新：完了した項目に印をつけ、次の小ステップを 1〜3 個生成する。各項目に、SCOPE.md の中スコープ ID を必ずつける。IDに紐づけられない項目は作らない。
3. 10 行以内の報告（やったこと、数値、次、止まっているもの）。

## 5. 止める
- スコープ内に実行できる小ステップが無いとき、同じエラーが 3 回続いたとき、費用や規約に触れる可能性があるときは、ループを止めて、理由を報告する（ScheduleWakeup は予約せず、`stop` する）。

## 提出のルール（SCOPE.md の M1-4・M2-4）
- NTL：OOF が現在の提出（0.608）より +0.01 以上、かつ T4 相当の制限での手元テストを通ったときだけ。1 日 2 回まで。
- Gemma 4：ハーネスの検証を通った提出物だけ。
