# 调用示例 / Invocation examples

## 学生课堂引导 / Guided student run

```text
使用 distillation-lab-coach 的课堂引导模式。
我的 SSH 主机别名是 autodl-course，run ID 是 smoke-group-03。
请先检查当前仓库、模型配置和 GPU，再分阶段执行 smoke。
三个微检查使用中文；答不上时提示一次并继续。
```

```text
Use distillation-lab-coach in classroom-guided mode.
My SSH host alias is autodl-course and my run ID is smoke-group-03.
Inspect the repository contract, model configuration, and GPU before running the smoke workflow in stages.
Ask the three soft checks in Chinese; give one hint and continue if I do not know.
```

## 快速执行 / Fast execution

```text
使用 distillation-lab-coach，切换到快速执行模式。
继续当前 smoke run；每个阶段仍汇报输入、命令、输出、验证和产物路径。
软检查同时给出问题与答案，硬门槛照常停止。
```

## 教师控场 / Teacher control

```text
使用 distillation-lab-coach 的教师控场模式。
学生刚完成教师示范生成。我现在应该展示哪个文件？
请给我一段 30 秒中文口播、一个展示入口和下一步的时间截止点。
```

## 文件问答 / File explanation

```text
使用 distillation-lab-coach 解释当前 run 的 generation_audit.jsonl。
请结合其中一条记录说明 question、gold、teacher_text 和 rejection_reasons，
并告诉我这个文件能证明什么、不能证明什么。
```

## 可选下载能力探查 / Optional download assessment

```text
先使用 distillation-lab-coach 判断模型是否已经完整存在。
如果确实缺失，再检查当前来源是否满足 model-download-accelerator 的 Provider 资格合同。
只报告适用性和缺口，不开始下载，也不宣称加速倍数。
```
