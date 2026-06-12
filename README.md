# Memory Forge

一个用于在 Claude Code 中生成和管理项目级记忆技能的工具。

## 概述

Memory Forge 用来把项目中的结构信息、工程规范、工具用法和执行流程沉淀成可复用的项目级 skill。

它会读取当前项目的代码、配置、文档、脚本、测试和已有 `.claude/skills/`，识别值得长期复用的记忆单元，并为这些记忆单元创建或更新对应的 `SKILL.md`。

## 快速开始

### 安装方式

#### 方式一：Git Submodule（推荐）

使用 git submodule 引用你的 skill，保持独立更新：

```bash
cd your-project
git submodule add https://github.com/Huang-404-Q/memory-forge.git .claude/skills/memory-forge
```

更新 skill：
```bash
git submodule update --remote .claude/skills/memory-forge
```

#### 方式二：Git Clone

克隆到本地后复制到项目中：

```bash
# 克隆到本地
git clone https://github.com/Huang-404-Q/memory-forge.git

# 复制到你的项目
mkdir -p your-project/.claude/skills/memory-forge
cp memory-forge/SKILL.md your-project/.claude/skills/memory-forge/
```

#### 方式三：直接下载

```bash
mkdir -p .claude/skills/memory-forge
curl -sL https://raw.githubusercontent.com/Huang-404-Q/memory-forge/main/SKILL.md \
  -o .claude/skills/memory-forge/SKILL.md
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