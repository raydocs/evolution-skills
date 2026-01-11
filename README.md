# Evolution Skills

Self-evolving skills for AI coding assistants.

AI 编程助手的自我进化技能系统。

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Package: Claude Code](https://img.shields.io/badge/Package-Claude%20Code-black.svg)](claude-code/README.md)
[![Package: Factory Droid](https://img.shields.io/badge/Package-Factory%20Droid-black.svg)](factory-droid/README.md)

[English](#english) | [中文](#中文)

---

## English

### What It Is

Evolution Skills is a documentation-first skill system for AI coding assistants. It ships structured specs and command templates that help capture reusable patterns, track their health, and keep guidance aligned with project style and framework versions.

Packages:
- Claude Code: `claude-code/`
- Factory Droid: `factory-droid/`

### Why It Matters

- Preserve hard-won solutions as reusable patterns.
- Self-correct when suggestions fail.
- Track pattern health with deterministic rules.
- Adapt to project style and version constraints.

### Feature Highlights

| Capability | What it does | Where to learn more |
|---|---|---|
| Pattern capture | Save new patterns with `/evolve` | `claude-code/README.md`, `factory-droid/README.md` |
| Health reporting | Aggregate `feedback` markers via `/skills-health` | `claude-code/README.md`, `factory-droid/README.md` |
| Validation | Validate skills content with `/validate-skills` | `claude-code/README.md` |
| Version adaptation | Choose guidance based on detected versions | `claude-code/README.md`, `factory-droid/README.md` |
| Style profiling | Align output to project conventions | `claude-code/README.md`, `factory-droid/README.md` |

### Quick Start

#### Claude Code

```bash
cp -r claude-code/skills/* ~/.claude/skills/
cp -r claude-code/commands/* ~/.claude/commands/
```

Then run:
```bash
/evolve
/skills-health
/validate-skills
```

#### Factory Droid

```bash
cp -r factory-droid/skills/* ~/.factory/skills/
cp -r factory-droid/commands/* ~/.factory/commands/
cp -r factory-droid/droids/* ~/.factory/droids/   # optional
```

Then run:
```bash
/evolve
/skills-health
```

### Command Overview

| Command | Claude Code | Factory Droid | Purpose |
|---|---|---|---|
| `/evolve` | Yes | Yes | Save learned knowledge |
| `/skills-health` | Yes | Yes | Show health report |
| `/validate-skills` | Yes | No | Validate skills accuracy |

### Use Cases

- Capture a verified troubleshooting fix after resolving a build failure.
- Standardize a UI pattern across multiple projects.
- Keep guidance aligned with framework version upgrades.
- Detect stale patterns using health signals.

### Architecture

```
Dev Session
  |
  | triggers: /evolve, errors, usage
  v
Selection + Dedupe + Metadata
  |
  +--> ~/.claude/skills   (Claude Code)
  +--> ~/.factory/skills  (Factory Droid)
  |
  v
Health + Feedback Updates
```

This repo provides the specs and command templates. The assistant runtime applies them and writes entries as Markdown with metadata blocks.

### Project Layout

```
claude-code/
  skills/
  commands/
factory-droid/
  skills/
  commands/
  droids/
```

Runtime storage lives under `~/.claude/skills` or `~/.factory/skills`. See the package READMEs for full details.

### Documentation

- `claude-code/README.md`
- `factory-droid/README.md`

### License

MIT

### Contributing

PRs welcome. Please follow the existing format and keep Claude and Factory versions aligned.

---

## 中文

### 这是什么

Evolution Skills 是面向 AI 编程助手的文档型技能系统。它提供规范化的技能说明与命令模板，用于沉淀可复用模式、追踪健康度，并与项目风格和框架版本保持一致。

套件目录:
- Claude Code: `claude-code/`
- Factory Droid: `factory-droid/`

### 价值

- 将解决方案沉淀为可复用模式。
- 建议失败时自动修正。
- 用确定性规则跟踪模式健康度。
- 适配项目风格与版本约束。

### 核心特性

| 能力 | 作用 | 了解更多 |
|---|---|---|
| 模式保存 | 通过 `/evolve` 保存新模式 | `claude-code/README.md`, `factory-droid/README.md` |
| 健康报告 | 通过 `/skills-health` 汇总 `feedback` | `claude-code/README.md`, `factory-droid/README.md` |
| 验证 | 通过 `/validate-skills` 验证内容 | `claude-code/README.md` |
| 版本适配 | 根据检测到的版本选择指导 | `claude-code/README.md`, `factory-droid/README.md` |
| 风格适配 | 匹配项目编码习惯 | `claude-code/README.md`, `factory-droid/README.md` |

### 快速开始

#### Claude Code

```bash
cp -r claude-code/skills/* ~/.claude/skills/
cp -r claude-code/commands/* ~/.claude/commands/
```

然后执行:
```bash
/evolve
/skills-health
/validate-skills
```

#### Factory Droid

```bash
cp -r factory-droid/skills/* ~/.factory/skills/
cp -r factory-droid/commands/* ~/.factory/commands/
cp -r factory-droid/droids/* ~/.factory/droids/   # 可选
```

然后执行:
```bash
/evolve
/skills-health
```

### 命令一览

| 命令 | Claude Code | Factory Droid | 说明 |
|---|---|---|---|
| `/evolve` | 是 | 是 | 保存学到的知识 |
| `/skills-health` | 是 | 是 | 显示健康报告 |
| `/validate-skills` | 是 | 否 | 验证 skills 准确性 |

### 使用场景

- 解决构建失败后沉淀可复用修复方案。
- 跨项目统一 UI 模式。
- 随框架版本升级保持指导一致。
- 通过健康信号识别过时模式。

### 架构

```
开发会话
  |
  | 触发: /evolve, 错误, 使用反馈
  v
选择 + 去重 + 元数据
  |
  +--> ~/.claude/skills   (Claude Code)
  +--> ~/.factory/skills  (Factory Droid)
  |
  v
健康统计 + 反馈更新
```

该仓库提供规范与命令模板，运行时由助手应用并以带元数据的 Markdown 保存。

### 项目结构

```
claude-code/
  skills/
  commands/
factory-droid/
  skills/
  commands/
  droids/
```

运行期内容保存于 `~/.claude/skills` 或 `~/.factory/skills`，细节见各套件 README。

### 文档

- `claude-code/README.md`
- `factory-droid/README.md`

### 许可证

MIT

### 贡献

欢迎 PR。请保持 Claude 与 Factory 两套文档一致。
