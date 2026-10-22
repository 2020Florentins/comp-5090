# comp_project — Claude Code 指示

このフォルダで Claude Code を開いたチャット（実行役・監視役のどちらも）が、最初に読む前提。グローバル指示（~/.claude/CLAUDE.md）に加えて、これを守る。

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
