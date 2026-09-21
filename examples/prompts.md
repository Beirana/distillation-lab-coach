# 调用示例

下面的路线按任务和节奏选择。任何使用者都可以调用全部路线，也都要遵守相同的实验门槛和权限边界。

## 本地没有技能：从云端开始

这一条先发给宿主，不假设 Skill 已被加载。当前仓库私有，需要本人已有授权或教师提供访问权限。

```text
请从 https://github.com/Beirana/distillation-lab-coach 获取完整技能，使用我已有的授权；无法访问时说明缺少什么。
然后按技能带我准备并完成一次 60 分钟蒸馏实验。连接信息和需要我操作的地方请与我沟通，先不要修改技能。
```

## 本机计算的课前评估

```text
使用 distillation-lab-coach。我想在自己的电脑上做整个实验，不使用远程 GPU。
请先只读检查实际系统、GPU 和已有环境，分别提出生成与训练方案，说明需要哪些适配。
不要先安装或改代码；我希望你根据本机条件选择环境工具，不必固定 uv 或 Conda。
```

## 课堂引导

```text
使用 $distillation-lab-coach 的课堂引导路线。
我的 SSH 主机别名是 autodl-course，run ID 是 smoke-group-03。
请先检查当前仓库、模型配置、已有 run 和 GPU，再从尚未完成的 smoke 阶段继续。
三个理解检查使用中文；答不上时提示一次并继续。
```

## 快速执行

```text
使用 $distillation-lab-coach，切换到快速执行路线。
继续当前 smoke run；每个阶段仍汇报输入、命令、输出、验证和产物路径。
理解检查同时给出问题与答案，实验门槛照常生效。
```

## 讲解展示

```text
使用 $distillation-lab-coach 的讲解展示路线。
当前刚完成教师示范生成。我现在应该展示哪个文件？
请给一段 30 秒中文讲解、一个展示入口和下一步的时间边界。
```

## 文件问答

```text
使用 $distillation-lab-coach 解释当前 run 的 generation_audit.jsonl。
请结合其中一条记录说明 question、gold、teacher_text 和 rejection_reasons，
并告诉我这个文件能证明什么、还不能支持什么结论。
```

## 双服务定性比较

```text
使用 $distillation-lab-coach 的讲解展示路线。
先检查原始 student base 和课前验证的 formal-500 merged 能否安全加载，再规划双服务演示。
两侧使用同一个问题，并分别判断回答格式和数值正确性。
显存不足时改成顺序启动或比较已有的同题预测，不结束其他人的进程。
```

## 故障诊断

```text
使用 $distillation-lab-coach 的故障诊断路线。
上一条命令失败了。请先确认当前阶段、真实退出码、已经存在的产物和日志，
再给最小修复动作。不要删除旧 run，也不要为了重跑伪造完成文件。
```

## 课后正式实验

```text
使用 $distillation-lab-coach 的课后正式路线。
我要使用一个新的正式 run 完成 500 条示范、训练前评测、LoRA 训练、导出和 dev 比较。
请先读取当前仓库说明并检查示范数据，再逐阶段执行；不要把课堂 smoke run 扩写成正式 run。
```

## 默认下载路线

```text
先使用 $distillation-lab-coach 判断模型是否已经完整存在。
完整则验证复用；确实缺失时，使用课程官方客户端下载并记录时间。
如果持续失败或停滞，请说明原因和备选方案，不要直接删除缓存或重下完整模型。
```

## 主动请求并行备选评估

```text
先使用 $distillation-lab-coach 判断模型是否已经完整存在。
我想评估并行备选；如果确实缺失，再检查当前来源是否满足 model-download-accelerator 的 Provider 资格合同及课程登记接口。
只报告适用性和缺口，不开始下载，也不宣称加速倍数。
```
