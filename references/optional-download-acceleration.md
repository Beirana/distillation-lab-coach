# 可选模型下载加速 / Optional model-download acceleration

本实验可选配合 [model-download-accelerator](https://github.com/Beirana/model-download-accelerator)，但它不是强制依赖，也不自动取代 `distill-course` 的 `download-models`。

The lab may optionally use [model-download-accelerator](https://github.com/Beirana/model-download-accelerator), but it is not a required dependency and does not automatically replace the course downloader.

## 决策顺序 / Decision order

1. **模型已完整存在？ / Models already complete?** 先按课程仓库清单或加载检查验证。完整就复用，不下载。
2. **课堂是否即将开始？ / Is class about to start?** 大模型传输和性能试验应在课前完成；课堂中缺模型直接切准备好的实例。
3. **Provider 是否已经支持？ / Is the provider supported?** 下载 Skill 当前主要面向公开 Hugging Face 兼容来源。ModelScope 尚未获得公开支持声明。
4. **来源能否满足资格合同？ / Can the source satisfy the provider contract?** 至少需要固定 revision、完整清单、稳定直链、已探测的 Range 行为、完整性依据和限流规则。
5. **本机基础是否具备？ / Are local prerequisites present?** 检查磁盘、文件系统、网络出口、`aria2c` 以及宿主执行权限。
6. **基准是否公平？ / Is the benchmark fair?** 对同一固定 revision 和冷文件比较普通方案与并行方案；记录持续吞吐、总时间、失败、重试、HTTP 错误和最终完整性。

任一关键条件不满足时，回退课程官方客户端或准备好的课堂实例。不要为了宣称“加速”而降低校验要求。

Fall back to the official course client or a prepared classroom host whenever a key condition is missing. Never weaken integrity checks merely to claim acceleration.

## 与当前课程的边界 / Boundary with the current course

`distill-course` 当前教师和学生模型通过 ModelScope 路线获取。公开版 `model-download-accelerator` 明确没有宣称 ModelScope 支持。因此现阶段的正确集成方式是：

- 在 README 中列为可选伴随 Skill；
- 先运行适用性探查，不直接执行传输；
- 只有 Provider 合同满足并经过实际验证后，才允许生成并行下载计划；
- 保留课程原有 `download-models` 作为默认和回退路径。

The course currently obtains its teacher and student models through ModelScope. The public accelerator does not claim ModelScope support. Keep it optional, assess applicability first, and preserve `download-models` as the default and fallback until the provider contract has been implemented and tested.

## 后续实测记录 / Future benchmark record

```text
Provider / mirror:
Repository and immutable revision:
Files and total bytes:
Machine, region, disk, and network:
Official-client wall time:
Accelerated wall time:
Sustained throughput (not warm-cache peak):
HTTP 403/429/reset counts:
Retry count and unfinished files:
Byte-level verification:
Semantic/index verification:
Measured improvement and limitations:
```

在填完这些数据前，不在本 Skill 中写入具体加速倍数。

Do not publish a numerical speedup in this skill before this record has been completed with measured evidence.
