# Memory Forge

一个用于在 Claude Code 中生成和管理项目级记忆技能的工具。

## 概述

Memory Forge 用来把项目中的结构信息、工程规范、工具用法和执行流程沉淀成可复用的项目级 skill。

它会读取当前项目的代码、配置、文档、脚本、测试和已有 `.claude/skills/`，识别值得长期复用的记忆单元，并为这些记忆单元创建或更新对应的 `SKILL.md`。

## 快速开始

### 安装

1. 克隆此仓库
2. 将 `SKILL.md` 文件复制到你的项目 `.claude/skills/memory-forge/` 目录下

```bash
mkdir -p .claude/skills/memory-forge
cp SKILL.md .claude/skills/memory-forge/
```

### 使用

在 Claude Code 中，当你想生成项目记忆时，可以这样使用：

```
请帮我生成项目记忆
```

或指定具体主题：

```
请帮我生成 Redis 记忆
```

## 核心概念

### 记忆单元

Memory Forge 的最小生成单位是 **记忆单元**，由三部分组成：

- `topic` - 记忆主题（Redis、数据库、日志等）
- `purpose` - 记忆用途（query、reader、convention 等）
- `memoryType` - 记忆分类（structure、convention、tool、workflow）

### memoryType 分类

| 类型 | 用途 | 示例 |
|------|------|------|
| `structure` | 沉淀项目结构、模块职责 | project-architecture |
| `convention` | 沉淀代码规范、命名规范 | logging-convention |
| `tool` | 沉淀查询工具、调试脚本 | redis-query |
| `workflow` | 沉淀任务流程、发布流程 | bugfix-workflow |

## 文档

详细文档请查看 [SKILL.md](SKILL.md)

## License

MIT