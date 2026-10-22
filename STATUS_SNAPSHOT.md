# STATUS_SNAPSHOT（チャット-1 が毎回上書き。独立した確認ではない）
時刻：2026-10-22 16:56

## Z890 status.txt
time: 2026-10-22 16:52:05
gpu_util: 99%  gpu_mem_mb: 20408
job_running: 2  pending: 2  backlog: 0  gpu_idle_min: 0
disk_free(/data): 4.8T
--- runner.log (tail)
2026-10-22 14:18:35 done 83_gemma_v0_rr61
2026-10-22 14:18:35 start 84_gemma_v1_rr61_retry
2026-10-22 16:49:54 done 84_gemma_v1_rr61_retry
2026-10-22 16:49:54 start 85_mistral_v03_h0_poolv4
--- failed (latest 3)
82_gemma_v1_rr61.sh
73_mistral_v01_full_poolv1.sh
72_mistral_v01_h0_poolv1.sh

## watchdog.log（末尾 5 行）
2026-10-22 14:11:54 watchdog start pid 426901

## kaggle_ntl.txt（提出の履歴）
57503784 COMPLETE public: 0.563 | 2026-10-22 01:00 | 0.5*Qwen3-8B-Base + 1.0*OLMo2-7B + 0.5*Mistral-7B-v0.3 (two-
57472849 COMPLETE public: - | 2026-10-21 08:05 | Qwen3-8B-Base LoRA + OLMo2-7B LoRA (two-stage, pool_v1) + 0.
57469281 COMPLETE public: 0.544 | 2026-10-21 06:21 | DIAG: same as 57464891 (0.5I+B+0.5T) but WITHOUT topic assig
57464891 COMPLETE public: 0.534 | 2026-10-21 04:10 | 0.5*Qwen3-8B LoRA + 1.0*Qwen3-8B-Base LoRA (two-stage, pool_
57455202 COMPLETE public: 0.520 | 2026-10-20 22:49 | Qwen3-8B two-stage LoRA (pool_v1 -> host), merged; +0.5*TFID

## queue/failed（新しい 3 件）
82_gemma_v1_rr61.sh
73_mistral_v01_full_poolv1.sh
72_mistral_v01_h0_poolv1.sh
