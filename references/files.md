# 文件含义速查 / File map

回答“这个文件是什么”时，先说明它属于哪个阶段，再说明它能证明什么和不能证明什么。

| 文件或目录 / File | 含义 / Meaning | 可验证 / Supports | 不能单独证明 / Does not prove |
|---|---|---|---|
| `configs/course.json` | 模型、数据版本、划分规模、随机种子和提示词等实验契约 | 本次计划使用什么 | 远端模型实体已经完整下载 |
| `request.json` | 当前 run 的任务请求 | run 名和模式 | 阶段已经成功完成 |
| `generation_audit.jsonl` | 教师生成尝试、解析结果和拒绝原因 | 为什么保留或拒绝某条示范 | 教师推理过程全部正确 |
| `dataset/train.json` | 实际进入训练的教师示范 | 训练数据内容与条数 | 数据没有偏差或泄漏 |
| `dataset/dataset_info.json` | 训练数据的来源与摘要 | 数据从哪里来、如何形成 | 原始数据许可之外的结论 |
| `eval_dev/before_predictions.jsonl` | 原始学生在开发题上的逐题输出 | 训练前具体行为 | 完整基准能力 |
| `eval_dev/before_summary.json` | 训练前聚合指标和设置 | before 的同协议摘要 | 蒸馏后的效果 |
| `train_config.yaml` | LoRA 训练的实际配置 | 模型、数据、rank、alpha、target、步数 | 配置一定合理或训练一定成功 |
| `train.log` | 训练过程日志 | 进度、loss、报错和耗时 | loss 下降等于答案正确率提高 |
| `train_complete.json` | 训练阶段完成记录 | 完成状态、配置指纹和耗时 | 导出模型可加载 |
| `adapter/` 或 `adapter-lite/` | LoRA 增量参数及配置 | 增量产物存在 | 可以脱离匹配基座独立使用 |
| `export_config.yaml` | 合并导出的配置 | adapter 与基座如何组合 | 合并已经成功 |
| `merged_manifest.json` | 合并模型文件清单与哈希 | 当前导出包含哪些文件 | 模型生成质量 |
| `merged/` | 可重新加载的完整学生模型 | 导出实体存在 | 评测已经完成 |
| `eval_dev/student_predictions.jsonl` | 训练后学生逐题输出 | 与 before 做定性同题比较 | 少数样例代表总体效果 |
| `eval_dev/student_summary.json` | 训练后开发集摘要 | student 的同协议指标 | 教师上界或最终测试结果 |
| `environment/process-*/supervisor.json` | 推理子进程的退出和回收状态 | 进程是否正常结束 | 模型答案正确 |
| `frozen.json` | 正式 run 在最终测试前冻结的配置与文件指纹 | 测试前契约是否锁定 | 测试集从未在历史实验中使用 |

## 四个容易混淆的词 / Four commonly confused terms

- **gold**：数据集给出的参考最终答案，用来核对教师或学生回答。
- **teacher_text**：教师模型生成的完整文本，可能正确也可能错误。
- **adapter**：LoRA 学到的增量权重，体积小但依赖对应基座。
- **merged**：把 adapter 合入学生基座后的完整可加载模型。
