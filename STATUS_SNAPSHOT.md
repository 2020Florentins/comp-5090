# 状況の写し（チャット-1 が毎回上書き。クラウドの監視用。独立した確認ではない）
時刻：2026-10-23 04:07 JST（使用率は実測なし。監視 02:50 の 8% は 72 分前で無効、キャッシュも古いため 35% とみなし節約）

## Z890 status.txt（03:57:45）
gpu_util 99%、gpu_mem 20264 MiB、job_running 2（実体は学習の python 1 本）、pending 2（その後 1 本追加）、backlog 1、gpu_idle_min 0、disk_free 4.8T
runner.log 末尾：89_qwen3_8b_base_h0_poolv4 完了（03:46）→ 90a_olmo2_full_poolv4 開始（03:46、約 06:00 完了予定）

## pending
90a（実行中）、90b_qwen3_8bbase_full_poolv4（04:05 に投入）、91_mistral_v03_h0_mergedv2（承認済み）

## failed（新しい 3 件、いずれも LOG に記載済み）
82_gemma_v1_rr61、73_mistral_v01_full_poolv1、72_mistral_v01_h0_poolv1

## Kaggle（name-that-llm の提出）
57503784 COMPLETE public 0.563（2026-10-22 01:00）、57469281 0.544、57464891 0.534、57455202 0.520
今日（10/23）の提出：0 回（夜間のため提出しない）

## 今回の主な動き
- 89（Qwen3-8B-Base、pool_v4、検証グループ 0）：0.5593（pool_v1 版 0.5315、+0.028）。3 モデルとも +0.01 以上のため、S-010 を満たした
- 90b を投入。S-018（08:00 以降：重みのアップロード、sub9、手元テスト、条件を満たせば日中に提出）を NEXT に追加
