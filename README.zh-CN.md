# TokenRoll Claude Code 插件

<div align="center">

**llmdoc + SubAgent RAG: 解决 Context Floor 问题**

[![GitHub](https://img.shields.io/badge/GitHub-TokenRollAI%2Fcc--plugin-blue?logo=github)](https://github.com/TokenRollAI/cc-plugin)

[English](README.md) | [简体中文](README.zh-CN.md)

</div>

---

## 问题：Context Floor

在严肃的面向生产的环境中，AI Coding Agent 面临一个根本性的挑战：**它们并不真正了解你的代码库**。它们通过 CLAUDE.md + 大量阅读代码文件来感知当前环境，这导致：

- 在达到足够上下文之前，不断地调用工具
- Token 消耗高，信息密度低
- 到达上下文就绪的时间（TTCR）太慢

我们把"满足 Agent 解决需求的 context 的丰富度"称之为 **Context Floor**。

### 现有方案的不足

| 方案 | 工具调用 | Token 占用 | 信息密度 | 效果 |
|------|---------|-----------|---------|------|
| LSP MCP | 高 | 中等 | 高 | 好，但慢 |
| ACE / RAG | 低 | 低 | 稀疏 | 关联性差 |
| Agentic RAG (Explorer) | 中等 | 低 | 高 | 好，但 TTCR 难以忍受 |

## 我们的方案：llmdoc + SubAgent RAG

**足够快、信息密度足够高、主 Agent Token 占用足够少、信息和任务存在强关联且有效。**

### llmdoc

一个在设计之初就用来解决 AI 快速获取高密度信息 + 人类可读性的文档系统。

脱胎于 [Diataxis](https://diataxis.fr/)，针对 LLM 检索进行优化：

```
llmdoc/
├── index.md          # 入口 - 永远首先阅读
├── overview/         # "这个项目是什么？" - 必须全部阅读
├── guides/           # "如何做 X？" - 分步指南
├── architecture/     # "它是怎么工作的？" - LLM 检索地图
└── reference/        # "X 的具体细节是什么？" - API 规范、约定
```

**核心设计原则：**
- 利用 Agent 能够快速批量 Read 的能力
- 文档中保留最关键的文件路径 + 负责的模块说明
- 项目概览 + 架构 + 通过主题串联的 guides + reference

示例：[TokenRoll/minicc/llmdoc](https://github.com/TokenRollAI/minicc/tree/main/llmdoc)

### SubAgent RAG

主要做两件事情：
1. **调研**：基于 llmdoc + 现有的代码文件，调研拆解后的任务作为前置条件
2. **记录**：在完成了编码任务之后，自动的更新维护 llmdoc

---

## 快速开始

### Step 1：安装插件

```bash
# 添加 TokenRoll 插件市场
/plugin marketplace add https://github.com/TokenRollAI/cc-plugin

# 安装 tr 插件
/plugin install tr@cc-plugin
```

### Step 2：配置系统提示词

将 [`CLAUDE.example.md`](CLAUDE.example.md) 的内容复制到 `~/.claude/CLAUDE.md` 文件中。

**就这样。** 配置完成后，所有行为自动激活：

- Agent 会**永远优先阅读 llmdoc**
- 调研使用**文档优先的方法**
- 完成编码任务后，Agent 会**询问是否更新文档**
- 所有 skill 根据上下文自动触发

### 更新插件

```bash
/plugin marketplace update https://github.com/TokenRollAI/cc-plugin
```

---

## 工作原理

### 自动行为（无需命令）

配置 `CLAUDE.example.md` 后，这些行为**始终激活**：

| 行为 | 效果 |
|------|------|
| **文档优先** | Agent 在任何操作前先阅读 `llmdoc/` |
| **智能调研** | 使用 `investigator` agent 而非通用探索 |
| **选项式编程** | 不会直接下结论；通过问题呈现选择 |
| **文档维护提示** | 编码后询问是否更新文档 |

### 可用 Skills（自动触发）

这些 skill 根据你的提示自动激活：

| Skill | 触发词 | 描述 |
|-------|--------|------|
| `/investigate` | "什么是"、"X怎么工作"、"分析" | 快速代码库调查 |
| `/commit` | "提交"、"commit" | 生成提交信息 |
| `/update-doc` | "更新文档"、"同步文档" | 更新 llmdoc |
| `/read-doc` | "了解项目"、"读文档" | 阅读 llmdoc 概览 |

### 命令（需要精确控制时）

| 命令 | 描述 |
|------|------|
| `/tr:initDoc` | 为新项目初始化 llmdoc |
| `/tr:withScout` | 复杂任务：先深度调研，再执行 |
| `/tr:what` | 通过结构化问题澄清模糊请求 |

---

## 推荐工作流

### 新项目

```bash
# 初始化文档系统
/tr:initDoc
```

### 日常开发

自然对话即可，系统会处理其余的：

```
"认证系统是怎么工作的？"
# -> 自动触发 /investigate，优先读取 llmdoc

"添加一个用户资料的 API 端点"
# -> 读取 llmdoc，调研，实现，询问是否更新文档

"commit"
# -> 自动触发 /commit，生成智能提交信息
```

---

## 成本与效果

**诚实评估**：这套方案大概用 **1.5 倍的价钱**完成了从 85 分到 90 分的效果提升。

- 简单项目：效果一般
- 复杂项目：收益显著
- 生产级代码库（10万+ 行）：效果出色

在我们的线上业务后端（约 10 万行代码）：
- 需求完成成本：**$1-5 / 功能**
- 人类介入：**大大降低**
- 输出质量：**Review 后稍作修改即可放心交付**

---

## 内部 Agents

| Agent | 用途 |
|-------|------|
| `worker` | 精确执行明确定义的计划 |
| `investigator` | 快速、无状态的代码库分析 |
| `recorder` | 创建和维护 llmdoc 文档 |
| `scout` | 为 initDoc 进行深度调查 |

---

<div align="center">

由 **DJJ** 和 **Danniel** 为 TokenRoll 团队精心打造

</div>
