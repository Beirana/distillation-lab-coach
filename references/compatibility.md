# 兼容性与宿主适配 / Compatibility and host adapters

## 可移植核心 / Portable core

以下内容构成宿主无关的核心：

- `SKILL.md`：模式选择、实验契约、检查点、安全边界和完成标准。
- `references/`：阶段命令、文件含义、教师讲解与故障处理。
- `examples/`：可以复制或改写的调用示例。

核心不调用 Codex、OpenAI、Claude 或其他 Agent 宿主 API。实验命令属于公开课程仓库 `distill-course`；Agent 只负责解释、编排和核对证据。

The portable core consists of `SKILL.md`, `references/`, and `examples/`. It does not call a Codex, OpenAI, Claude, or other host API. Commands belong to the public `distill-course` repository; the agent explains, orchestrates, and verifies evidence.

## 可选宿主适配 / Optional host adapters

`agents/openai.yaml` 仅提供 Codex/OpenAI 界面的显示名称、简介和默认提示词。忽略或删除它不会改变核心流程。其他宿主可以加入自己的可选元数据，但不得削弱实验契约、硬门槛或凭据边界。

`agents/openai.yaml` provides optional Codex/OpenAI presentation metadata. Removing or ignoring it does not change the workflow. Other hosts may add adapters, but adapters must not weaken experiment gates or credential boundaries.

## 安装模型 / Installation model

把整个仓库复制或克隆到宿主能够发现的技能目录，并保持目录名、仓库名和 `SKILL.md` 中的 `name` 都为 `distillation-lab-coach`。

Copy or clone the whole repository into the host's skill directory. Keep the repository name, directory name, and the `name` field in `SKILL.md` aligned as `distillation-lab-coach`.

- Codex 用户级 / user: `~/.codex/skills/distillation-lab-coach/`
- Claude Code 用户级 / user: `~/.claude/skills/distillation-lab-coach/`
- Claude Code 项目级 / project: `.claude/skills/distillation-lab-coach/`
- 其他宿主 / other hosts: 使用该宿主文档指定的位置。

兼容性只有在实际测试技能发现、指令加载、终端与 SSH 权限以及审批行为后才能声明。目录格式兼容不代表每种宿主拥有相同的沙箱、联网或无人值守执行能力。

Claim host compatibility only after testing discovery, instruction loading, terminal and SSH permissions, and approval behavior. A compatible directory format does not imply identical sandboxing, networking, or unattended execution.

## 角色不是权限 / Modes are not authorization

“教师控场”“学生执行”等模式只改变解释和节奏，不授予额外系统权限。Agent 仍应遵守当前宿主的权限与审批机制。

Teacher and student modes change explanation and pacing only. They do not grant extra system permissions; the agent remains subject to the host's permission and approval model.
