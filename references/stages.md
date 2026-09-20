# 学生执行阶段 / Student execution stages

以下命令以远端项目 `/root/distill-course`、工作根 `/root/distill-work` 为默认值。实际仓库说明或环境变量不同时，先报告差异，不凭历史路径覆盖当前配置。

课堂引导模式只使用三个软检查，分别位于阶段 1、3、5。学生回答一句话、指出一个字段或做出二选一判断即可。第一次答不上时给提示并解释，不重复追问。快速执行模式显示问题与答案，但不停下来等待。

## 0. 连接与契约 / Connection and contract

先通过已经配置好的 SSH 主机别名连接。检查：

```bash
cd /root/distill-course
git status --short
git rev-parse HEAD
python scripts/verify_bundle.py
python scripts/course.py preflight
python -m json.tool configs/course.json
nvidia-smi
```

验证：仓库完整；GPU 可用；教师和学生模型配置符合本课；没有与本组实验冲突的任务。

快速定位：教师、学生和数据设置位于 `configs/course.json`。课堂中 2 分钟仍未连通，切到教师备用实例或并组。

## 1. 创建 smoke 示范数据 / Create smoke demonstrations

运行前确认新的 `<run-id>` 不存在：

```bash
export COURSE_DATA_ROOT=/root/distill-work
test ! -e "/root/distill-work/runs/<run-id>"
python scripts/run_stage.py generate --run <run-id> --mode smoke
```

验证：生成阶段正常结束；run 的 `dataset/train.json` 包含八条保留示范；`generation_audit.jsonl` 保留尝试和拒绝依据。

软检查 1：给出一条 audit 记录，请学生指出“教师答案是否进入训练”应该看哪个字段。可接受回答：看 `rejection_reasons` 是否为空，并结合解析、结束状态等记录。答不上时直接在当前记录中高亮字段并解释。

生成超过课堂截止时间时，保留日志并切换到教师准备的 `smoke01` 证据。

## 2. 训练前基线 / Pre-training baseline

```bash
python scripts/run_stage.py eval --run <run-id> --model before --split dev
```

验证：`eval_dev/before_summary.json` 与 `before_predictions.jsonl` 存在且属于同一 run。本阶段 smoke 只取少量开发题。

一句话解释：before 是训练前坐标原点；五题只能做流程和定性观察，不能代表稳定准确率。无需停下来考问。

## 3. 生成并检查训练配置 / Build and inspect training config

```bash
python scripts/course.py make-config --run <run-id>
```

先展示 `train_config.yaml`，再训练。至少指出：学生模型路径、数据路径、`finetuning_type`、LoRA rank、alpha、target modules、训练步数和输出路径。

硬门槛：训练前必须实际展示并核对 `train_config.yaml` 的模型、数据、输出路径和 smoke 步数。发现路径或模式错误时停止，不用课堂节奏覆盖配置问题。

软检查 2：二选一提问——“LoRA 训练时，基础模型完全不参与计算，还是参与前向计算但其原权重冻结？”学生选后给出解释。答不上不阻塞训练，只把状态记为 `hinted`。

## 4. 训练与导出 / Train and export

若当前实例需要独立训练环境，以仓库说明和已验证环境为准设置 `COURSE_TRAIN_ENV`。不要沿用不确定的历史路径。

```bash
python scripts/course.py train --run <run-id>
python scripts/course.py export --run <run-id>
```

验证：`train_complete.json` 记录完成状态；adapter 目录和 `adapter_config.json` 存在；导出阶段正常结束；`merged_manifest.json` 与 merged 模型属于当前 run。

一句话解释：adapter 是依赖匹配基座的增量参数，merged 是合并后的完整模型。loss 下降只能说明训练目标在当前数据上变化，不等于正确率提高。无需停下来考问。

## 5. 训练后比较 / Post-training comparison

```bash
python scripts/run_stage.py eval --run <run-id> --model student --split dev
```

并排读取 `before_summary.json`、`student_summary.json` 和对应 predictions。确认同一 run、同一 split 和同一评测协议。

软检查 3：让学生在“流程跑通”和“能力提升”中选择本次 smoke 能证明哪一个，并用一句话说原因。答不上时明确答案是前者，因为八条数据、两步训练和少量开发题不提供稳定效果证据。

## 6. 正式结果讲解 / Explain the full-run results

课堂 smoke 到此停止，不运行 test。教师可展示已经冻结的 500 条实验结果：历史留档 dev100 为 before 4%、student 62%、teacher 90%，test300 为 2%、48%、84%。必须说清这是已验证归档结果，不是本组 smoke 刚跑出的结果。

若要在课后执行正式 500 条路径，重新读取当前仓库 README 和学生跟课手册，使用新 run，并在人审教师示范后再训练。不要把正式流程作为课堂 smoke 的自动延续。
