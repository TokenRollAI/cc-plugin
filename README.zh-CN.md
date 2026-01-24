# TokenRoll Claude Code 插件

<div align="center">

**TokenRoll Claude Code Plugin: CC Coding 的最佳实践**

[![GitHub](https://img.shields.io/badge/GitHub-TokenRollAI%2Fcc--plugin-blue?logo=github)](https://github.com/TokenRollAI/cc-plugin)

[English](README.md) | [简体中文](README.zh-CN.md)

</div>

---

## 安装

### Step 1: 安装插件

```
# 添加 TokenRoll 插件市场
/plugin marketplace add https://github.com/TokenRollAI/cc-plugin

# 下载tr插件
/plugin install tr@cc-plugin
```

### Step 2: 配置系统提示词

将本仓库中的 `CLAUDE.example.md` 文件的全部内容，复制到您用户级的 `~/.claude/CLAUDE.md` 文件中。此文件包含了启用 Agent 和 Command 所必需的系统提示。

好了，现在你可以正常使用了。

### 更新插件

```
/plugin marketplace update https://github.com/TokenRollAI/cc-plugin
```

### (强烈推荐) 安装 CCR: 使用 GLM4.6 驱动 SubAgent

[参考](https://github.com/musistudio/claude-code-router)

```
npm install -g @musistudio/claude-code-router
```

在 `~/.claude-code-router/config.json` 中填写配置，参考如下：

```json
{
    "LOG": true,
    "LOG_LEVEL": "debug",
    "CLAUDE_PATH": "",
    "HOST": "127.0.0.1",
    "PORT": 3456,
    "APIKEY": "sk-apikey",
    "API_TIMEOUT_MS": "600000",
    "PROXY_URL": "http://127.0.0.1:7890",
    "transformers": ["Anthropic"],
    "Providers": [
        {
            "name": "claude",
            "api_base_url": "https://<BASE>/v1/messages",
            "api_key": "XXX",
            "models": ["claude-sonnet-4-5-20250929"],
            "transformer": { "use": ["Anthropic"] }
        },
        {
            "name": "glm",
            "api_base_url": "https://open.bigmodel.cn/api/anthropic/v1/messages",
            "api_key": "XXX",
            "models": ["glm-4.6"],
            "transformer": { "use": ["Anthropic"] }
        }
    ],
    "Router": {
        "default": "claude,claude-sonnet-4-5-20250929",
        "background": "claude,claude-sonnet-4-5-20250929",
        "think": "claude,claude-sonnet-4-5-20250929",
        "longContext": "claude,claude-sonnet-4-5-20250929",
        "webSearch": "claude,claude-sonnet-4-5-20250929"
    }
}
```

## 关于

由 **DJJ** 和 **Danniel** 为 TokenRoll 团队开发的强大 Claude Code 插件。本插件通过智能 Git 自动化、研究优先开发模式和创意构思工具，彻底改变你的开发工作流。

## 核心功能

### Skills（自动触发，带预取上下文）

| Skill | 触发词 | 描述 |
|-------|--------|------|
| `/investigate` | "什么是"、"X怎么工作"、"分析" | 文档优先的快速代码库调查 |
| `/commit` | "提交"、"保存更改"、"完成了" | 基于 git 历史生成提交信息 |
| `/update-doc` | "更新文档"、"同步文档" | 代码变更后更新 llmdoc |
| `/doc-workflow` | "文档工作流"、"如何写文档" | llmdoc 文档系统使用指南 |
| `/read-doc` | "了解项目"、"读文档" | 阅读 llmdoc 快速了解项目 |

### Commands（用户触发的工作流）

| 命令 | 描述 |
|------|------|
| `/tr:initDoc` | 为新项目初始化 llmdoc 文档系统 |
| `/tr:what` | 通过选项式问题澄清模糊请求 |
| `/tr:withScout` | 处理复杂任务：先调查后执行 |

### Agents（内部执行引擎）

| Agent | 描述 |
|-------|------|
| `worker` | 精确执行明确定义的计划 |
| `investigator` | 快速、无状态的代码库分析 |
| `recorder` | 创建和维护 llmdoc 文档 |
| `scout` | （内部）为 initDoc 进行深度调查 |

## 推荐使用流程

### 1. 初始化新项目

```bash
# 首次使用，为项目建立完整的文档系统
/tr:initDoc
```

### 2. 日常开发流程

```bash
# 快速代码库调查（自动触发的 skill）
/investigate "认证系统是怎么工作的？"

# 处理复杂任务，先调查后执行
/tr:withScout "分析现有代码架构，找出最佳集成点"

# 生成智能提交信息（自动触发的 skill）
/commit
```

### 3. 文档维护

```bash
# 代码变更后更新文档系统（自动触发的 skill）
/update-doc
```

### 4. 了解现有项目

```bash
# 阅读项目文档快速了解
/read-doc

# 获取文档工作流指导
/doc-workflow
```

---

<div align="center">

Made with ❤️ by DJJ & Danniel

</div>
