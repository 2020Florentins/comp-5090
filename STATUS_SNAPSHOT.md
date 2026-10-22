# 状況の写し（チャット-1 が毎回上書き。クラウドの監視用。独立した確認ではない）
時刻：2026-10-23 06:02 JST（使用率：監視 04:48 のライブ値 11%、その後の消費は小。実測なし）

## Z890
status.txt 05:42:51：gpu_util 99%、gpu_mem 22570 MiB、job_running 2（実体は学習 1 本）、gpu_idle_min 0、disk_free 4.8T
runner.log 末尾：90a_olmo2_full_poolv4 完了（05:51）→ 90b_qwen3_8bbase_full_poolv4 開始（05:51、約 08:20 完了予定）
pending：90b（実行中）、90c_sub10_t4sim（06:00 に投入）、91_mistral_v03_h0_mergedv2（承認済み）
failed（新しい 3 件、LOG に記載済み）：82_gemma_v1_rr61、73_mistral_v01_full_poolv1、72_mistral_v01_h0_poolv1

## Kaggle（name-that-llm）
最新の公開スコア 0.563（57503784、2026-10-22 01:00）。今日（10/23）の提出：0 回（夜間は提出しない）

## 今回の主な動き
- 90a（OLMo 2 全データ、pool_v4）完了。90c（T4 相当の手元テスト）を投入。データセット置き場（Mistral・OLMo 2）はローカルに作成済み。アップロードは 08:00 以降
