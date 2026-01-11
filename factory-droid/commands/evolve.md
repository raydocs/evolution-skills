---
description: 自我进化系统 - 将学到的知识保存到 skills 文件中
argument-hint: [tech] [pattern-name]
---

# 自我进化

将当前会话中学到的知识保存到 `~/.factory/skills/` 中。

## 输入

$ARGUMENTS

接受:
- 空: 回顾当前会话，提议要保存的内容
- `<tech>`: 指定技术栈 (如 flutter, swiftui, react)
- `<tech> <name>`: 直接添加指定的模式

示例:
- `/evolve` - 回顾会话并提议保存
- `/evolve flutter` - 保存 Flutter 相关的发现
- `/evolve flutter pull-to-refresh` - 保存下拉刷新模式

## 工作流

### Step 1: 识别值得保存的知识

回顾当前会话，问自己:
- 解决了什么非平凡的问题？
- 发现了什么可复用的模式？
- 有什么错误和解决方案值得记录？

### Step 2: 分类知识

确定目标文件:
```
~/.factory/skills/{tech}-patterns.md        # UI/组件模式
~/.factory/skills/{tech}-troubleshooting.md # 错误解决
~/.factory/skills/{tech}-architecture.md    # 架构模式
~/.factory/skills/{tech}-build.md           # 构建/部署
```

### Step 3: 检查现有内容

```bash
# 读取现有 skill 文件 (如果存在)
cat ~/.factory/skills/{tech}-patterns.md 2>/dev/null || echo "文件不存在，将创建"
```

### Step 4: 格式化内容

**模式格式**:
```markdown
<!-- evolution: {date} | source: {project} | trigger: solved-problem -->

## 模式 N: {名称}

{简要描述}

### 代码

\```{language}
// 代码
\```

### 使用场景

- 场景描述

### 注意事项

- 注意点
```

**问题解决格式**:
```markdown
<!-- evolution: {date} | source: {project} | trigger: fixed-error -->

### {错误消息}

**症状**: 开发者看到什么

**原因**: 为什么发生

**解决方案**:
\```{language}
// 修复代码
\```
```

### Step 5: 写入文件

如果文件不存在，创建带有头部的新文件:

```markdown
# {Tech} Patterns

此文件记录 {Tech} 开发中发现的可复用模式。

由自我进化系统自动维护。

---

```

然后追加新内容。

### Step 6: 确认

告诉用户:
```
✅ 已保存到 ~/.factory/skills/{tech}-patterns.md
   - 模式: {name}
   - 来源: {project}
```

## 快捷提示

在开发过程中，你可以说:

- "这个解决方案值得保存" → 触发进化
- "把这个加到 skills" → 触发进化
- "记住这个模式" → 触发进化

## 查看已保存的 Skills

```bash
ls -la ~/.factory/skills/
```

## 进化系统主文档

完整的自我进化系统说明见:
```bash
cat ~/.factory/skills/evolution.md
```
