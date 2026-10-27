# Gemma 4 の提出手順の下書き（S-022、M2-4）

作成：2026-10-24。**アカウントが復旧するまで、提出はできない**（Kaggle の異議申し立ての審査中）。この文書は、復旧の直後に、迷わず進めるための下書き。HARNESS_README.md（競技のデータ）と、スターターのノートブック（gemma4/start/）から読み取った内容で、Kaggle の公式ページで確認していない部分は「未確認」と書く。

## 提出物の要件（HARNESS_README の第 2・3 節）
- ディレクトリの構成：ルートに agent.yaml が**ちょうど 1 つ**（agent.yml、root_agent.yaml、root_agent.yml も可）。任意で eval_config.yaml、prompts/*.md、configs/*.yaml、sub_agents/*.yaml、adapters/<名前>/（adapter_config.json と adapter_model.safetensors）。
- 制約：シンボリックリンク禁止、パスの `..` 禁止、宣言的な YAML のみ（Python のエントリポイント無し）。展開後の合計が **3 GiB 以下**、ファイル数 1 万以下、YAML 1,000 個以下、YAML 1 つ 50 MiB 以下。許可される拡張子は .yaml .yml .md .txt .py（スキルのスクリプト）.json .safetensors。アダプタ名は `[A-Za-z0-9][A-Za-z0-9._-]*`。
- 単一モデル：すべてのエージェントが宣言するベースモデルは、1 種類だけで、gemma-4-31b-it-qat-w4a16-ct、gemma-4-12b-it-qat-w4a16-ct、gemma-4-e4b-it-qat-w4a16-ct、gemma-4-e2b-it-qat-w4a16-ct のいずれか。
- thinking_config：vLLM で LoRA を使うときは、省略する（README の注意）。

## 手元の検証（済み、2026-10-24）
- gemma4/validate_sub.py（スターターのノートブックの検証セルと同じ手順：symlink の検査、validate_directory、宣言モデルの検査、zip の作成と、サイズ・拡張子の検査）を、subs/v0_base と subs/v2_fast に実行：**どちらも通過**（宣言モデルは 31B 1 種類、zip は 9 ファイル、展開後 5〜6 KiB、agent.yaml がルート）。
- 使い方：`cd /data/nakamura/comp/gemma4; . .venv_harness/bin/activate; python validate_sub.py subs/<提出物>`。

## 提出の流れ（スターターのノートブックから。**未確認**：Kaggle の公式ページ）
1. スターターのノートブック（ryanholbrook/getting-started-gemma-4-developer-agent。GPU、インターネット無し、データセット metric/gemma-4-developer-agent-wheelhouse、モデル 4 種、競技 gemma-4-developer-agent を入力に持つ）に倣い、提出物のディレクトリから submission.zip（ルートに agent.yaml）を作る。
2. ノートブックを非公開で実行し、出力に submission.zip を残す（kaggle kernels push）。
3. `kaggle competitions submit gemma-4-developer-agent -k <ノートブック> -f submission.zip -v <版> -m "<説明>"`（コード競技の形式。**未確認**）。採点は、非公開の約 120 件で、競技側が実行する。
4. 提出は 08:00〜22:00 の日中、1 日 2 回まで（私たちの規則。Kaggle の上限は別）。M2-4 の条件：ハーネスの検証を通ったものだけ。

## 提出物の選択（P-007 の判断待ち）
- v0_base（基準）と v2_fast（プロンプトにツールの規則を足した版）は、どちらも検証を通過している。手元の評価では、正解パッチが通る 41 件で v0 が 15 件（並行 1）、v2_fast は並行 3 の 61 件の評価で 16 件（v0 は 15 件）。差はノイズの範囲で、どちらが良いかは判別できない（LOG 2026-10-23 21:43）。本人の判断（P-007）まで、v0_base を既定とする。

## 追記（2026-10-28）：最小カーネル案
- スターターの推論を省き、提出物の作成だけを行う最小カーネルを、gemma4/sub_kernel_v0/ に作成し、手元で検証した（LOG 2026-10-28 の S-041）。朝、push → complete → submit。受理されない場合は、スターターに近い形に戻す。
