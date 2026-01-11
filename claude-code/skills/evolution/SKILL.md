---
name: evolution
description: 通用自我进化系统 - 在开发过程中自动学习并改进知识库。自动检测版本、适配项目风格、跟踪模式成功率、解决问题后提议保存知识。任何开发会话都应加载此 skill。
---

# 通用自我进化系统

此 skill 让 Claude 在开发过程中持续学习并改进知识库。

---

## 自动触发规则

**重要**: 作为 Claude，你应该在适当时机自动执行这些行为。

### 自动 (无需用户请求)

| 机制 | 何时触发 |
|------|---------|
| **版本适配** | 会话开始，检测 package.json/Cargo.toml/pubspec.yaml |
| **个性化** | 首次生成代码，扫描 2-3 个现有文件 |
| **自我修正** | 当你建议的代码失败时立即触发 |
| **使用反馈** | 每次使用已保存的模式后 |

### 半自动 (你提议，用户确认)

| 机制 | 何时提议 |
|------|---------|
| **自我进化** | 解决非平凡问题后，问用户是否保存 |

### 手动 (用户请求)

| 机制 | 触发短语 |
|------|---------|
| **验证 skills** | "验证 skills" / "validate skills" |
| **健康报告** | "skills 健康报告" / "pattern health report" |

---

## 会话开始检查清单

开始任何开发会话时，静默执行:

1. **检测技术栈版本**
   - Flutter: 读取 `pubspec.yaml`
   - Node.js: 读取 `package.json`
   - Rust: 读取 `Cargo.toml`
   - Swift: 检查 Xcode 项目或 Package.swift
   - 设置内部上下文记住版本

2. **扫描项目风格** (如果没做过)
   - 读取 2-3 个现有代码文件
   - 记录: 命名规范、模块结构、注释风格

3. **加载相关 skills**
   - 读取 `~/.claude/skills/{tech}-*.md`
   - 如果不存在，准备创建

**不要宣布这些操作**，除非与用户问题直接相关。

---

## Skill 文件结构

```
~/.claude/skills/
├── evolution/
│   └── SKILL.md              # 本文件
├── {tech}-patterns.md        # UI/组件模式
├── {tech}-troubleshooting.md # 错误解决
├── {tech}-architecture.md    # 架构模式
├── {tech}-build.md           # 构建/部署
└── project-styles/
    └── {project}.md          # 项目风格档案
```

其中 `{tech}` 是: flutter, swiftui, react, vue, rust, python 等。

---

## 进化触发条件

当发生以下情况时，考虑保存知识:

| 触发 | 目标文件 | 优先级 |
|-----|---------|-------|
| 发现新 UI 模式 | {tech}-patterns.md | 高 |
| 解决编译/运行错误 | {tech}-troubleshooting.md | 高 |
| 学到架构技术 | {tech}-architecture.md | 高 |
| 找到布局方案 | {tech}-layout.md | 中 |
| 解决构建问题 | {tech}-build.md | 中 |

### 进化前的自我检查

问自己:
- ✅ 这是可复用的模式吗？(不是项目特定的)
- ✅ 花了很大力气才弄清楚吗？
- ✅ 会帮助未来的开发吗？
- ✅ 还没记录在 skills 中吗？

如果都是肯定的，提议给用户:
> "这个解决方案可能值得保存到 skills。要我添加吗？"

---

## 内容格式

### 模式格式

```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: {what-happened} -->
<!-- feedback: count=0 | success=0 | failed=0 | last_used=YYYY-MM-DD -->

## 模式 N: {名称}

{一句话描述这个模式解决什么问题}

### 代码

\```{language}
// 完整可运行的代码示例
\```

### 使用场景

- 场景 1: 具体描述
- 场景 2: 具体描述

### 注意事项

- 版本要求: {framework} >= {version}
- 其他注意点
```

### 问题解决格式

```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: fixed-error -->

### {错误消息或类型}

**症状**: 开发者看到什么现象

**原因**: 为什么会发生 (技术解释)

**解决方案**:
\```{language}
// 修复代码
\```

**相关版本**: {framework} {version}
```

---

## 自我修正流程

当你建议的代码导致错误时:

1. **识别**: "这个错误是因为我之前的建议有问题"

2. **修复用户代码**: 提供正确的解决方案

3. **更新 skill 文件**:
   ```
   找到错误的内容
   添加修正标记
   更新为正确内容
   ```

4. **通知用户**:
   > "我也更新了 skills 文件，这样以后不会再犯同样的错误。"

### 修正标记

```markdown
<!-- correction: YYYY-MM-DD | was: {旧内容摘要} | now: {新内容摘要} | reason: {原因} -->
```

---

## 使用反馈跟踪

当使用 skills 中的模式时:

### 成功时
- 心里记录: success += 1
- 更新 feedback 标记

### 失败时
- 心里记录: failed += 1
- 触发自我修正流程
- 更新 feedback 标记

### 反馈标记

```markdown
<!-- feedback: count=10 | success=8 | failed=2 | last_used=2026-01-10 -->
```

### 健康判断

| 成功率 | 状态 | 动作 |
|-------|------|------|
| > 90% | ✅ 健康 | 无需动作 |
| 70-90% | 🔶 良好 | 可选审查 |
| 50-70% | ⚠️ 需审查 | 检查边缘情况 |
| < 50% | ❌ 问题 | 需修复或移除 |

---

## 版本适配

### 检测版本

会话开始时静默检测:

```
Flutter: pubspec.yaml → flutter sdk 版本
React: package.json → react 版本
Swift: Xcode 项目 → iOS deployment target
Rust: Cargo.toml → edition, dependencies
```

### 版本特定内容

在 skill 文件中标记版本:

```markdown
<!-- version: flutter>=3.0 -->
\```dart
// Flutter 3.0+ 写法
\```

<!-- version: flutter<3.0 -->
\```dart
// Flutter 3.0 之前写法
\```
```

### 版本不匹配警告

如果用户代码使用了旧版 API:
> "注意：你使用的是 {old-version} 的写法，项目是 {new-version}。正确写法是..."

---

## 个性化

### 首次代码生成时

静默扫描项目:
1. 读取 2-3 个现有代码文件
2. 记录命名规范、模块结构、注释风格
3. 后续代码生成匹配项目风格

### 项目风格档案

保存到 `~/.claude/skills/project-styles/{project}.md`:

```markdown
# {Project} 风格档案

- **命名规范**: camelCase / snake_case
- **模块组织**: feature-first / layer-first
- **状态管理**: BLoC / Provider / Redux
- **注释风格**: 详细 / 简洁
- **复杂度偏好**: 小组件 / 大组件
```

---

## 质量标准

### 应该保存 ✅

- 通用、可复用的模式
- 常见错误及清晰解决方案
- 经过验证的技术
- 平台特定注意事项
- 性能优化

### 不应该保存 ❌

- 项目特定的代码
- 未验证的解决方案
- 重复内容
- 不完整的示例
- 无理由的个人偏好

---

## 进化提示语

在开发过程中，这些短语触发进化:

- "这个解决方案值得保存"
- "把这个加到 skills"
- "记住这个模式"
- "以后可能还会用到"

---

## 持续改进检查清单

每次开发会话后，考虑:

- [ ] 发现了新的组件/模式吗？
- [ ] 解决了棘手的问题吗？
- [ ] 遇到并修复了令人困惑的错误吗？
- [ ] 找到了更好的代码组织方式吗？
- [ ] 学到了构建/部署相关的东西吗？

如果有，提议保存！

---

## 命令

用户可用的命令:

- `/evolve [tech] [name]` - 保存知识
- `/skills-health` - 健康报告
- `/validate-skills` - 验证准确性
