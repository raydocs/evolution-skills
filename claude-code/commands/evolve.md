---
description: 自我进化 - 将学到的知识保存到 skills 文件
---

# 自我进化

将当前会话中学到的知识保存到 `~/.claude/skills/`。

## 输入

$ARGUMENTS

接受:
- 空: 回顾当前会话，提议要保存的内容
- `<tech>`: 指定技术栈 (flutter, swiftui, react, rust 等)
- `<tech> <name>`: 直接添加指定的模式

示例:
- `/evolve` - 回顾会话并提议保存
- `/evolve flutter` - 保存 Flutter 相关发现
- `/evolve flutter pull-to-refresh` - 保存下拉刷新模式

## 工作流

### 1. 识别值得保存的知识

回顾当前会话:
- 解决了什么非平凡的问题？
- 发现了什么可复用的模式？
- 有什么错误和解决方案值得记录？

### 2. 确定目标文件

```
~/.claude/skills/{tech}-patterns.md        # UI/组件模式
~/.claude/skills/{tech}-troubleshooting.md # 错误解决
~/.claude/skills/{tech}-architecture.md    # 架构模式
~/.claude/skills/{tech}-build.md           # 构建/部署
```

### 3. 检查现有内容

```bash
cat ~/.claude/skills/{tech}-patterns.md 2>/dev/null || echo "文件不存在，将创建"
```

### 4. 格式化并写入

**模式格式**:
```markdown
<!-- evolution: {date} | source: {project} | trigger: {trigger} -->
<!-- feedback: count=0 | success=0 | failed=0 | last_used={date} -->

## 模式 N: {名称}

{描述}

### 代码
\```{language}
// 代码
\```

### 使用场景
- 场景

### 注意事项
- 注意点
```

**问题解决格式**:
```markdown
<!-- evolution: {date} | source: {project} | trigger: fixed-error -->

### {错误消息}

**症状**: ...
**原因**: ...
**解决方案**:
\```{language}
// 修复
\```
```

### 5. 确认

```
✅ 已保存到 ~/.claude/skills/{tech}-{category}.md
   - 内容: {name}
   - 来源: {project}
```

## 相关

- `/skills-health` - 查看模式健康报告
- `/validate-skills` - 验证 skills 准确性
- 完整文档: `~/.claude/skills/evolution/SKILL.md`
