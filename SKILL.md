------

## name: memory-forge description: Use this skill when the user asks to create, update, initialize, organize, or deduplicate project-level memory skills under `.claude/skills/`. This skill reads existing skills and project evidence, identifies focused memory units, and generates maintainable `SKILL.md` files organized by topic, purpose, and memoryType.

# Memory Forge

你是当前项目的 **记忆锻造师**。

Memory Forge 用来把项目中的结构信息、工程规范、工具用法和执行流程沉淀成可复用的项目级 skill。

它会读取当前项目的代码、配置、文档、脚本、测试和已有 `.claude/skills/`，识别值得长期复用的记忆单元，并为这些记忆单元创建或更新对应的 `SKILL.md`。

------

## 一句话原则

先理解用户要沉淀什么，再检查已有 skill 是否覆盖，然后基于项目证据生成或更新具体的项目记忆 skill。

------

# 核心概念

## 记忆单元

Memory Forge 的最小生成单位是 **记忆单元**。

一个记忆单元由三部分组成：

```text
topic + purpose + memoryType
```

| 字段         | 含义                                                         |
| ------------ | ------------------------------------------------------------ |
| `topic`      | 记忆主题，例如 Redis、数据库、日志、测试、项目架构、发布     |
| `purpose`    | 记忆用途，例如 query、reader、convention、workflow、architecture、index |
| `memoryType` | 记忆分类，只能是 `structure`、`convention`、`tool`、`workflow` |

生成 skill 时，名称根据 `topic` 和 `purpose` 决定。

------

## memoryType 分类

| memoryType   | 用途                                             | 通用示例                                |
| ------------ | ------------------------------------------------ | --------------------------------------- |
| `structure`  | 沉淀项目结构、模块职责、目录落点、能力索引       | `project-architecture`、`api-route-map` |
| `convention` | 沉淀代码规范、命名规范、日志规范、测试规范       | `logging-convention`、`test-convention` |
| `tool`       | 沉淀查询工具、读取工具、调试脚本、项目内工具用法 | `redis-query`、`database-query`         |
| `workflow`   | 沉淀任务流程、修 bug 流程、发布流程、审查流程    | `bugfix-workflow`、`release-workflow`   |

------

# 适用场景

当用户表达以下意图时使用本 skill：

- 生成项目记忆
- 初始化项目记忆
- 整理项目记忆
- 更新已有项目记忆
- 合并重复 skill
- 检查 `.claude/skills/` 是否重复
- 把某个项目规则沉淀成 skill
- 把某个工具用法沉淀成 skill
- 把某个流程沉淀成 skill
- 生成 Redis、数据库、日志、测试、架构、发布等相关记忆

------

# 工作流程

## Phase 1：理解用户意图

先判断用户想做什么。

### 全量初始化

用户说：

- 生成项目记忆
- 初始化项目记忆
- 整理项目记忆

处理方式：

1. 扫描已有 `.claude/skills/`
2. 扫描项目中的关键代码、配置、文档和脚本
3. 找出适合沉淀的记忆单元
4. 输出候选生成计划
5. 等用户确认后再生成或更新

全量初始化时不要直接写文件，先给出候选清单。

------

### 指定生成

用户说：

- 生成 Redis 记忆
- 生成数据库查询记忆
- 生成日志规范记忆
- 生成测试规范记忆
- 生成发布流程记忆

处理方式：

1. 识别用户指定的主题
2. 判断这个主题对应的用途
3. 查看已有 skill 是否已经覆盖
4. 读取相关项目证据
5. 输出生成或更新计划
6. 用户确认后执行

如果用户只说了大主题，但用途不明确，先列出可能方向让用户确认。

例如用户说“生成 Redis 记忆”，可以先判断它更像：

- Redis 查询工具
- Redis 缓存使用规范
- Redis key 命名规范
- Redis 分布式锁使用流程

------

### 更新已有

用户说：

- 更新某个 skill
- 补充某个记忆
- 合并重复 skill
- 这个 skill 过期了

处理方式：

1. 读取目标 skill
2. 重新查看项目证据
3. 判断需要补充、替换、合并还是跳过
4. 输出更新计划
5. 用户确认后执行

------

## Phase 2：扫描已有 skills

读取：

```text
.claude/skills/*/SKILL.md
```

重点查看每个已有 skill 的：

- `name`
- `description`
- 适用场景
- 核心职责
- 工作流程
- 元信息
- 维护约定

扫描的目的，是判断当前用户要生成的记忆是否已经被已有 skill 覆盖。

------

## Phase 3：判断是否已有覆盖

Memory Forge 的去重判断保持简单：

> 直接看已有 skill 的 `description`、适用场景和核心职责，判断它是否已经能处理当前用户请求。

判断结果分为五种：

| 决策     | 含义                                              |
| -------- | ------------------------------------------------- |
| `create` | 没有已有 skill 覆盖，需要新建                     |
| `update` | 已有 skill 覆盖主题，但内容不完整或过期，需要更新 |
| `merge`  | 已有相近 skill，合并进去比新建更合适              |
| `skip`   | 已有 skill 已经能处理该需求，跳过                 |
| `ask`    | 边界不清楚，需要用户确认                          |

判断时关注三个问题：

1. 用户现在要生成的记忆，已有 skill 是否已经会处理？
2. 如果新建一个 skill，是否会和已有 skill 触发场景冲突？
3. 合并到已有 skill 是否比新建更清楚？

如果已有 skill 的 `description` 已经清楚覆盖当前需求，就优先 `skip` 或 `update`。

如果只是同一大类，但主题和用途不同，可以新建。

------

## Phase 4：收集项目证据

生成或更新 skill 前，需要读取当前项目中的相关证据。

证据可以来自：

- 源码
- 配置文件
- README
- docs
- scripts
- tests
- `.claude/skills/`
- `.claude/commands/`
- 构建配置
- 部署配置
- 项目已有流程文件

读取证据的目标不是写通用教程，而是确认当前项目真实存在什么结构、规则、工具或流程。

如果证据不足，先说明缺少什么，不要强行生成。

------

## Phase 5：识别记忆单元

根据用户需求和项目证据，整理出具体的记忆单元。

每个记忆单元使用以下结构表示：

```yaml
skillName: <skill-name>
topic: <topic>
purpose: <purpose>
memoryType: structure | convention | tool | workflow
action: create | update | merge | skip | ask
reason: <why>
```

示例：

```yaml
skillName: redis-query
topic: redis
purpose: query
memoryType: tool
action: create
reason: 项目中存在 Redis 使用方式，且当前没有 Redis 查询相关 skill。
```

------

## Phase 6：输出写入计划

创建、更新或合并前，先输出计划。

计划需要包含：

- skill 名称
- 目标路径
- topic
- purpose
- memoryType
- 决策动作
- 使用到的项目证据
- 将生成或修改的文件
- 是否包含 `references/`
- 是否包含 `scripts/`
- 是否需要用户确认

涉及以下行为时，需要用户确认：

- 更新已有 skill
- 合并已有 skill
- 覆盖已有文件
- 新增脚本
- 删除内容
- 移动文件
- 写入数据库、配置中心、环境相关内容

------

## Phase 7：生成或更新 skill

默认路径：

```text
.claude/skills/<skill-name>/SKILL.md
```

内容较长时，可以拆分：

```text
.claude/skills/<skill-name>/
├── SKILL.md
└── references/
    └── xxx.md
```

需要脚本时，可以使用：

```text
.claude/skills/<skill-name>/
├── SKILL.md
├── scripts/
│   └── xxx.py
└── references/
    └── xxx.md
```

工具类 skill 如果包含脚本，必须写清楚安全约束。

数据库、配置中心、环境信息相关工具默认只读。

------

## Phase 8：生成后检查

生成或更新后检查：

- frontmatter 是否存在
- `name` 是否和目录名一致
- `description` 是否准确
- skill 名称是否表达 topic 和 purpose
- 是否包含维护约定
- 是否包含元信息
- 是否没有写入敏感信息
- Markdown 代码块是否闭合
- `references/` 或 `scripts/` 中提到的文件是否存在
- 是否和已有 skill 产生明显重复

检查通过后再报告完成。

------

# 生成的 skill 基础结构

每个生成的 `SKILL.md` 至少包含：

```markdown
---
name: <skill-name>
description: <说明什么时候触发，具体到 topic 和 purpose>
---

# <Skill Title>

## 一句话原则

...

## 适用场景

...

## 适用边界

...

## 核心规则

...

## 工作流程

...

## 项目依据

- ...

## 维护约定

| 变更内容 | 需要同步更新 |
|---|---|
| ... | ... |

## 元信息

- lastUpdated: <YYYY-MM-DD>
- memoryType: structure | convention | tool | workflow
- topic: <topic>
- purpose: <purpose>
- sourceFiles:
  - <file path>
```

------

# 不同 memoryType 的内容要求

## structure

需要说明：

- 覆盖范围
- 模块职责
- 关键文件
- 目录组织
- 核心入口
- 修改相关功能时应该先看哪里

------

## convention

需要说明：

- 适用范围
- 核心规则
- 代码生成约束
- 修改时需要同步检查什么
- 检查清单

------

## tool

需要说明：

- 工具用途
- 使用方式
- 命令格式
- 脚本路径
- 参数含义
- 安全约束
- 失败处理

------

## workflow

需要说明：

- 触发条件
- 流程步骤
- 每步输入
- 每步动作
- 每步输出
- 检查点
- 失败回退策略

------

# 命名规则

Skill 名称规则：

1. 使用小写英文。
2. 单词之间用 `-` 分隔。
3. 名称体现具体 topic 和 purpose。
4. 名称清晰表达该 skill 解决的问题。
5. 同 topic + purpose 的 skill 优先更新或合并。

推荐格式：

```text
<topic>-<purpose>
```

或：

```text
<scope>-<topic>-<purpose>
```

通用示例：

```text
redis-query
database-query
logging-convention
test-convention
project-architecture
api-route-map
bugfix-workflow
release-workflow
```

------

# 安全规则

1. 创建或更新前先输出计划。
2. 覆盖已有文件前需要用户确认。
3. 删除已有内容前需要用户确认。
4. 新增脚本前需要用户确认。
5. 数据库、配置中心、环境信息相关内容默认只读。
6. 不写入密钥、token、密码、生产连接信息。
7. 信息不足时说明缺少哪些证据。

------

# 维护约定

Memory Forge 生成的每个 skill 都必须包含 `维护约定` 章节。

该章节用于说明：当项目中与本 skill 相关的代码、配置、脚本、文档或流程发生变化时，需要同步更新本 skill，保证项目记忆和真实项目保持一致。

生成的 skill 中统一加入以下内容：

```
## 维护约定

当本 skill 依赖的相关代码、配置、脚本、文档或流程发生变化时，需要同步更新本 skill。

需要重点检查：

- 本 skill 记录的规则是否仍然准确
- 本 skill 提到的文件路径是否仍然存在
- 本 skill 描述的流程是否仍然可执行
- 本 skill 中的示例是否仍然符合当前项目
- 本 skill 的适用场景是否需要调整

更新时应修改：

- `lastUpdated`
- 相关规则说明
- 相关文件路径
- 相关示例
- 相关注意事项
```



# 报告格式

## 候选计划

```text
Memory Forge 识别到以下候选记忆单元：

1. <skill-name>
   topic: <topic>
   purpose: <purpose>
   memoryType: <memoryType>
   建议动作: create / update / merge / skip / ask
   说明: <为什么需要这个 skill>

请确认是否继续生成或更新。
```

## 新建完成

```text
✅ 已生成 <skill-name>
📁 位置：.claude/skills/<skill-name>/
🧠 memoryType：<structure | convention | tool | workflow>
🏷️ topic：<topic>
🎯 purpose：<purpose>
📝 包含内容：<summary>
```

## 更新完成

```text
✅ 已更新 <skill-name>
📁 位置：.claude/skills/<skill-name>/
🧠 memoryType：<structure | convention | tool | workflow>
🔄 更新章节：<sections>
```

## 跳过

```text
ℹ️ 已有 <skill-name> 覆盖该记忆单元，跳过
📁 位置：.claude/skills/<skill-name>/
📌 跳过原因：已有 skill 的 description 和职责范围已覆盖当前需求
```

## 证据不足

```text
⚠️ 暂不生成 <skill-name>
原因：项目证据不足

还需要查看：
- <file or directory>
- <file or directory>
```

## 需要确认

```text
⚠️ 该记忆单元边界不清楚，需要确认

候选处理方式：
1. 新建 <skill-name>
2. 合并到 <existing-skill>
3. 更新 <existing-skill>
4. 暂不处理

请确认采用哪种方式。
```

------

# 质量检查

最终报告前检查：

-  是否扫描了已有 `.claude/skills/`
-  是否看过已有 skill 的 description 和职责范围
-  是否识别了具体记忆单元
-  是否明确了 topic
-  是否明确了 purpose
-  是否明确了 memoryType
-  是否基于项目证据生成
-  是否输出了写入前计划
-  是否没有写入敏感信息
-  是否包含维护约定
-  是否验证了生成结果

------

# 默认执行流程

```text
接收用户指令
  -> 判断全量初始化 / 指定生成 / 更新已有
  -> 扫描已有 .claude/skills/
  -> 判断已有 skill 是否覆盖
  -> 收集项目证据
  -> 识别具体记忆单元
  -> 确定 topic、purpose、memoryType
  -> 决策 create / update / merge / skip / ask
  -> 输出计划
  -> 用户确认后写入
  -> 生成后检查
  -> 报告结果
```