# comp_project — Claude Code 指示

このフォルダで Claude Code を開いたチャット（実行役・監視役のどちらも）が、最初に読む前提。グローバル指示（~/.claude/CLAUDE.md）に加えて、これを守る。

## 【最優先・例外なし】/loop の具体的指示の書式（本人の命令。10/24・10/28 に 3 回、11/1 に 4 回目の「また崩れている」。この節は他のどの手順よりも優先する）
1. 次の指示は必ず NEXT_PROMPT.md に、**7 行**で書く：1 行目 `/loop <LOOP_PROMPT.md のパス> を読み、その手順で、次の指示を 1 ステップ進めてください。`、2〜7 行目に【状況】【スコープとの距離】【妥当性・貢献度】【今回の指示】【その次の候補】【起きたときの確認】をこの順に 1 回ずつ。**600〜800 字**（Python の len）。空行・他の見出し（【測定】など）・固定の注意・長い経過は書かない。
2. 【今回の指示】は **1 件だけ**、必ずこの形：`ID（中スコープ ID）題：入力＝… → 出力＝… → 成功基準＝…。理由：…`（例：`S-086（M1-4）…`、`W152（M6）…`）。確認リストや段落にしない。
3. 【状況】は `M/D HH:MM。` で始め、時刻は `date`（手で書かない）。**起床のたびに（進捗の無い起床、背景処理の通知への応答、保険の cron での再開も含めて）今の状態で書き直す**。古い NEXT_PROMPT を使い回さない。
4. 書いたら必ず `python C:\Users\Nakamura\comp_project\check_next_prompt.py` を実行し、**OK が出るまで直す**。OK の内容だけを ScheduleWakeup の prompt にそのまま渡す。
5. 本人に見える文章を出すときは、**毎回例外なく、末尾に NEXT_PROMPT.md と同一の `/loop …`（7 行）をコードブロック 1 つで付ける**（通知への短い応答、1 行の報告も含む）。省けるのは、cron の保険が求める「ループは稼働中です（…）」だけの 1 行の応答のみ。
6. 保険の cron で再開するとき、汎用の 1 行（「LOOP_PROMPT.md を読み、その手順で 1 ステップ進めてください」）で /loop を呼ばない。NEXT_PROMPT.md を今の状態で書き直して検査を通し、その内容の /loop で進める。

## 最初にやること
1. README.md（構成と役割）→ SCOPE.md（大中小スコープ・範囲外・ずれの定義）→ NEXT.md → LOG.md の末尾、の順に読む
2. 実行役なら LOOP_PROMPT.md、監視役なら MONITOR_PROMPT.md の手順に従う
3. HALT.md があれば、何もせず止まる（削除は本人だけ）

## 前提（記憶が別フォルダにあるため、ここに転記）
- 目的：Z890（RTX 5090、`ssh Z890`、ユーザー shogo の共用アカウント、他の人は使わない）で、Kaggle のコンペ（Name That LLM、Gemma 4 Developer Agent）の賞・実績を狙う
- 作業場所：Z890 の /data/nakamura/comp/（name-that-llm/、gemma4/、models/、queue/、logs/）。この外は読み書きしない
- ジョブは Z890 のキュー（/data/nakamura/comp/queue/pending/ に `NN_名前.sh` を置く。1 本ずつ順に実行）。GPU は 1 枚なので、常駐サーバー（vLLM）はジョブの中で起動して必ず止める
- Kaggle：ユーザー名 natakumi。トークンは Z890 の /data/nakamura/.kaggle/access_token（`KAGGLE_API_TOKEN=$(cat ...)` で渡す）
- **費用が発生するものは一切禁止**（有料 API、クラウド GPU、参加費、購入）
- 削除・上書き・移動は、本人の確認を取る。患者データ・gait_reextract には触れない
- Kaggle の Join・規約への同意・公開は、本人だけが行う
- 詳細な数値・手順：C:\Users\Nakamura\comp_handoff\NIGHT_RUNBOOK.md、メモリ（~/.claude/projects/C--Users-Nakamura/memory/comp-5090-prize-hunt.md。**別フォルダのチャットからは自動では見えない**ので、必要なら直接読む）

## git（2026-10-22 追加）
- このフォルダは git 管理（private、`github.com/2020Florentins/comp-5090`）。実行役は、毎回の最初に pull、最後に push する（RULES_24H.md の「13」）。強制 push、`git add -A`、git config の変更はしない。トークン・鍵・重み・データはリポジトリに入れない。

## 応答
日本語（です・ます調）。前後に短い要約（論点／結論／ネクスト）。共感トークや冗長な復唱はしない。
