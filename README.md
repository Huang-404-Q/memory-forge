# Memory Forge

[![GitHub stars](https://img.shields.io/github/stars/Huang-404-Q/memory-forge)](https://github.com/Huang-404-Q/memory-forge/stargazers)
[![MIT License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

**A repository-aware meta-skill that turns project knowledge into focused, maintainable Claude Code skills.**

一个面向代码仓库的元 skill，用来把项目知识锻造成多个聚焦、可维护的 Claude Code 项目记忆 skill。

---

## 🤔 为什么需要这个？

Claude Code 很强大，但它不知道你的项目：

- Redis 怎么连？数据库怎么查？工具用法每次要解释
- 代码规范、命名约定、日志规则散落在代码里，没人记得住
- 新会话要重复解释项目结构，效率低下
- 多人协作时，规范不一致，沟通成本高

**问题不是 Claude 记不住，而是项目知识没有被结构化。**

---

## 🎯 它能做什么？

Memory Forge 不是另一个"项目记忆文件"，而是一个 **工程化的项目知识组织器**：

- **自动分析项目** - 扫描代码、配置、文档，识别值得复用的知识单元
- **生成聚焦的 skills** - 每个 skill 只解决一个问题（topic + purpose + memoryType）
- **可维护的结构** - 把散落的项目知识拆成多个独立的、互相引用的 SKILL.md
- **避免重复** - 自动检测已有 skills，避免重复生成

---

## ⚡ 快速开始

### 安装

```bash
# 方式一：Git Submodule（推荐）
git submodule add https://github.com/Huang-404-Q/memory-forge.git .claude/skills/memory-forge

# 方式二：直接下载
mkdir -p .claude/skills/memory-forge
curl -sL https://raw.githubusercontent.com/Huang-404-Q/memory-forge/main/SKILL.md \
  -o .claude/skills/memory-forge/SKILL.md
```

### 使用

在 Claude Code 中，直接说：

```
请帮我生成项目记忆
```

或指定主题：

```
请帮我生成 Redis 记忆
请帮我生成数据库查询记忆
请帮我生成测试规范记忆
```

---

## 📖 核心概念

### 记忆单元 = topic + purpose + memoryType

每个生成的 skill 都是围绕这三个维度组织的：

| 字段 | 说明 | 示例 |
|------|------|------|
| `topic` | 主题 | Redis、数据库、日志 |
| `purpose` | 用途 | query（查询）、convention（规范） |
| `memoryType` | 分类 | structure、convention、tool、workflow |

### memoryType 分类

| 类型 | 用途 | 示例 |
|------|------|------|
| `structure` | 项目结构、模块职责 | project-architecture |
| `convention` | 代码规范、命名规范 | logging-convention |
| `tool` | 工具用法、调试脚本 | redis-query |
| `workflow` | 工作流程、发布流程 | bugfix-workflow |

---

## 💡 适用场景

| 场景 | 说明 |
|------|------|
| 新项目初始化 | 生成项目架构记忆，快速建立上下文 |
| 项目重构 | 沉淀规范记忆，保持重构一致性 |
| 多人协作 | 统一项目规范，新人快速上手 |
| 工具集成 | Redis、数据库等工具用法沉淀 |
| 流程标准化 | bugfix、release 流程规范化 |

---

## 📄 文档

详细文档：[SKILL.md](SKILL.md)

---

## 📝 License

MIT License