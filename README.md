# 🧬 Evolution Skills | 自我进化技能系统

[English](#english) | [中文](#中文)

---

<a name="english"></a>
## English

### What is this?

A self-evolving skill system for AI coding assistants. During development, the AI automatically:

- 📚 **Learns** reusable patterns and saves them
- 🔧 **Self-corrects** when suggested code fails
- 📊 **Tracks** which patterns work well
- 🎯 **Adapts** to project coding style
- 🔄 **Adapts** to framework versions

### Supported Tools

| Tool | Directory |
|------|-----------|
| Claude Code | `claude-code/` |
| Factory Droid | `factory-droid/` |

### Quick Install

#### Claude Code

```bash
# Copy skills
cp -r claude-code/skills/* ~/.claude/skills/

# Copy commands
cp -r claude-code/commands/* ~/.claude/commands/
```

#### Factory Droid

```bash
# Copy skills
cp -r factory-droid/skills/* ~/.factory/skills/

# Copy commands
cp -r factory-droid/commands/* ~/.factory/commands/

# Copy droid (optional)
cp factory-droid/droids/* ~/.factory/droids/
```

### Usage

#### Automatic Behaviors (No action needed)

| Behavior | Trigger |
|----------|---------|
| Version Detection | Session start |
| Style Personalization | First code generation |
| Self-Correction | When suggested code fails |
| Usage Tracking | After using saved patterns |

#### Manual Commands

| Command | Description |
|---------|-------------|
| `/evolve` | Save learned knowledge |
| `/evolve flutter` | Save Flutter-specific knowledge |
| `/skills-health` | Show pattern health report |
| `/validate-skills` | Validate skills accuracy |

#### Natural Language Triggers

- "This solution is worth saving"
- "Add this to skills"
- "Remember this pattern"

### How It Works

```
┌─────────────────┐
│   Development   │
│     Session     │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌───────┐
│Success│ │Failure│
│  +1   │ │  +1   │
└───┬───┘ └───┬───┘
    │         │
    │    ┌────┴────┐
    │    ▼         │
    │ ┌───────┐    │
    │ │ Self- │    │
    │ │Correct│    │
    │ └───┬───┘    │
    │     │        │
    └──┬──┴────────┘
       │
       ▼
┌─────────────┐
│   Propose   │
│  Save New   │
│  Knowledge  │
└─────────────┘
       │
       ▼
┌─────────────┐
│  Improved   │
│   Skills    │
└─────────────┘
```

### File Structure

```
~/.claude/skills/  (or ~/.factory/skills/)
├── evolution/
│   └── SKILL.md              # Main control document
├── {tech}-patterns.md        # UI/component patterns
├── {tech}-troubleshooting.md # Error solutions
├── {tech}-architecture.md    # Architecture patterns
└── project-styles/
    └── {project}.md          # Project style profiles
```

### Quality Guidelines

#### Should Save ✅
- Generic, reusable patterns
- Common errors with clear solutions
- Verified techniques
- Performance optimizations

#### Should NOT Save ❌
- Project-specific code
- Unverified solutions
- Duplicate content
- Incomplete examples

---

<a name="中文"></a>
## 中文

### 这是什么？

一个用于 AI 编程助手的自我进化技能系统。在开发过程中，AI 会自动：

- 📚 **学习** 可复用的模式并保存
- 🔧 **自我修正** 当建议的代码失败时
- 📊 **跟踪** 哪些模式有效
- 🎯 **适配** 项目编码风格
- 🔄 **适配** 框架版本

### 支持的工具

| 工具 | 目录 |
|-----|------|
| Claude Code | `claude-code/` |
| Factory Droid | `factory-droid/` |

### 快速安装

#### Claude Code

```bash
# 复制 skills
cp -r claude-code/skills/* ~/.claude/skills/

# 复制 commands
cp -r claude-code/commands/* ~/.claude/commands/
```

#### Factory Droid

```bash
# 复制 skills
cp -r factory-droid/skills/* ~/.factory/skills/

# 复制 commands
cp -r factory-droid/commands/* ~/.factory/commands/

# 复制 droid (可选)
cp factory-droid/droids/* ~/.factory/droids/
```

### 使用方法

#### 自动行为 (无需操作)

| 行为 | 触发时机 |
|-----|---------|
| 版本检测 | 会话开始 |
| 风格个性化 | 首次生成代码 |
| 自我修正 | 建议的代码失败时 |
| 使用跟踪 | 使用已保存的模式后 |

#### 手动命令

| 命令 | 说明 |
|-----|------|
| `/evolve` | 保存学到的知识 |
| `/evolve flutter` | 保存 Flutter 相关知识 |
| `/skills-health` | 显示模式健康报告 |
| `/validate-skills` | 验证 skills 准确性 |

#### 自然语言触发

- "这个解决方案值得保存"
- "把这个加到 skills"
- "记住这个模式"

### 工作原理

```
┌─────────────────┐
│    开发会话     │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌───────┐
│ 成功  │ │ 失败  │
│  +1   │ │  +1   │
└───┬───┘ └───┬───┘
    │         │
    │    ┌────┴────┐
    │    ▼         │
    │ ┌───────┐    │
    │ │ 自我  │    │
    │ │ 修正  │    │
    │ └───┬───┘    │
    │     │        │
    └──┬──┴────────┘
       │
       ▼
┌─────────────┐
│   提议保存   │
│   新知识     │
└─────────────┘
       │
       ▼
┌─────────────┐
│   改进的    │
│   Skills    │
└─────────────┘
```

### 文件结构

```
~/.claude/skills/  (或 ~/.factory/skills/)
├── evolution/
│   └── SKILL.md              # 主控制文档
├── {tech}-patterns.md        # UI/组件模式
├── {tech}-troubleshooting.md # 错误解决
├── {tech}-architecture.md    # 架构模式
└── project-styles/
    └── {project}.md          # 项目风格档案
```

### 质量标准

#### 应该保存 ✅
- 通用、可复用的模式
- 常见错误及清晰解决方案
- 经过验证的技术
- 性能优化

#### 不应该保存 ❌
- 项目特定的代码
- 未验证的解决方案
- 重复内容
- 不完整的示例

---

## License

MIT

## Contributing

PRs welcome! Please follow the existing format when adding new patterns.
