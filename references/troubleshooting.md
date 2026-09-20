# 故障诊断与课堂切换 / Troubleshooting and classroom fallback

先确定当前阶段、最后一条实际命令、退出码和已有产物。保留原始日志，不以重新运行覆盖证据。

| 现象 / Symptom | 先检查 / Inspect | 处置 / Action | 课堂截止 / Cutoff |
|---|---|---|---|
| SSH 无法连接 | 主机别名、实例是否开机、平台给出的地址/端口、首次指纹 | 不索要私钥或密码；让学生本人修正连接，必要时并组 | 2 分钟 |
| `verify_bundle` 失败 | 当前 Git 状态、缺失文件、仓库来源 | 停止实验；不在未知工作区继续，切教师备用实例 | 立即 |
| 教师/学生配置不符 | `configs/course.json` 与当前 Git commit | 报告差异并停止；不静默修改模型 | 立即 |
| 模型目录缺失 | `/root/distill-work/models/{teacher,student}` 与数据盘链接 | 课堂不等待大下载；切准备好的实例，课后修复资产 | 1 分钟 |
| `Run ID exists` | 对应 run 目录和 request | 保留旧 run，生成新的简单 run ID | 立即 |
| 生成/评测退出码为 2 | 当前 stage 的 supervisor、日志和目标产物 | 说明进程回收未正常通过；即使产物存在也不称完全成功，不重跑同名 run | 3 分钟 |
| GPU 显存被占用 | `nvidia-smi`、当前任务 supervisor、启动命令 | 只识别当前任务创建的进程；不宽泛清理其他任务 | 2 分钟 |
| make-config 前置检查失败 | 当前 run 的生成完成记录和数据哈希 | 按错误定位缺失前置，不手写伪造完成文件 | 2 分钟 |
| 训练失败 | `train.log`、环境前缀、配置路径、磁盘 | 保存日志；不课堂重装环境，切归档 adapter/结果 | 3 分钟 |
| 导出失败 | `export.log`、基座、adapter、磁盘 | 不把 adapter 冒充完整模型；切归档 merged | 2 分钟 |
| 结果与教师展示不同 | run、split、模型角色、commit、配置、解码设置 | 先判断是否可比；真实差异如实记录，不追求改成同一个数字 | 3 分钟 |

## 诊断汇报格式 / Diagnostic report format

```text
阶段：
最后命令与退出码：
已经存在的有效产物：
直接证据：
目前不能确定的事项：
建议的最小修复：
课堂是否切备用材料：
```

## 不得用的捷径 / Prohibited shortcuts

- 删除现有 run 后原名重跑。
- 手工创建 `*_complete.json` 让后续检查通过。
- 把少数预测样例当作准确率。
- 根据 test 结果修改训练配置后再次测试。
- 为了赶时间静默切换模型、数据或评分规则。
- 用 `killall`、宽泛 `pkill` 或模糊进程名清理整台机器。
