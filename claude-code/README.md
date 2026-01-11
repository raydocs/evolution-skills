# Claude Code Evolution Skills

Self-evolving skill system for Claude Code.

Claude Code 的自我进化技能系统。

## Overview | 概览

This package installs an evolution skill for Claude Code. It captures reusable patterns, troubleshooting fixes, and architecture notes into `~/.claude/skills`, and keeps usage metadata updated during development.

该包为 Claude Code 安装自我进化技能。它把可复用模式、问题修复和架构经验保存到 `~/.claude/skills`，并在开发过程中维护使用元数据。

## Installation | 安装

```bash
# Copy skills | 复制 skills
cp -r skills/* ~/.claude/skills/

# Copy commands | 复制 commands
cp -r commands/* ~/.claude/commands/

# Optional: verify install | 可选: 验证安装
ls ~/.claude/skills
ls ~/.claude/commands
```

If you have existing skills, back them up before overwriting. Re-copying updates the installed files.

如果你已有自定义 skills，覆盖前请先备份。重复复制可用于更新。

## Directory Structure | 目录结构

Repo layout (files you copy):

```
skills/
├── evolution/
│   └── SKILL.md         # Canonical evolution spec
└── project-styles/
    └── _template.md     # Project style template

commands/
├── evolve.md            # /evolve command
├── skills-health.md     # /skills-health command
└── validate-skills.md   # /validate-skills command
```

Runtime skill storage (auto-maintained by the system):

```
~/.claude/skills/
├── {tech}-patterns.md
├── {tech}-troubleshooting.md
├── {tech}-architecture.md
├── {tech}-layout.md
├── {tech}-build.md
├── {tech}-fundamentals.md
└── project-styles/
    └── {project}.md
```

## Quick Start | 快速开始

1. Install skills and commands.
2. Start a dev session; version detection and style scan run silently.
3. Solve a non-trivial problem.
4. Run `/evolve` to capture the knowledge.
5. Use `/skills-health` for health stats.
6. Use `/validate-skills` when you suspect drift.

1. 安装 skills 与 commands。
2. 开始开发会话; 版本检测和风格扫描会静默执行。
3. 解决一个非平凡问题。
4. 使用 `/evolve` 记录知识。
5. 用 `/skills-health` 查看健康统计。
6. 需要时用 `/validate-skills` 做准确性验证。

```bash
/evolve
/evolve flutter pull-to-refresh
/skills-health
/validate-skills
```

## Command Reference | 命令参考

### /evolve [tech] [name]

Accepts:
- empty: review the session and propose what to save
- `<tech>`: save knowledge for that tech
- `<tech> <name>`: add a specific pattern directly

支持:
- 空参数: 回顾会话并提议要保存的内容
- `<tech>`: 保存指定技术栈的知识
- `<tech> <name>`: 直接添加指定模式

Target mapping (by category):
- patterns -> `~/.claude/skills/{tech}-patterns.md`
- troubleshooting -> `~/.claude/skills/{tech}-troubleshooting.md`
- architecture -> `~/.claude/skills/{tech}-architecture.md`
- layout -> `~/.claude/skills/{tech}-layout.md`
- build -> `~/.claude/skills/{tech}-build.md`
- fundamentals -> `~/.claude/skills/{tech}-fundamentals.md`

Numbering and dedupe:
- Next number = max existing `## 模式 N:` + 1
- Dedupe by normalized title (lowercase, remove punctuation, collapse spaces)

### /skills-health

Generates a health report from `feedback` markers across all skills.

从所有 skills 文件的 `feedback` 标记生成健康报告。

Deterministic rules (summary):
- Count only entries starting with `## 模式`
- Bind `feedback` within the first 3 lines above the entry
- Prefer 90-day window stats (`window_*`) when present
- Attention when success rate < 70%, `window_failed >= 3`, `last_used` > 90 days, or missing tracking

Status labels:
- Healthy / 健康
- Good / 良好
- Needs review / 需审查
- Problem / 有问题

### /validate-skills

Validates code blocks, API accuracy, version compatibility, and completeness.

验证代码块、API 准确性、版本兼容性与完整性。

Validation marker:
```markdown
<!-- validated: YYYY-MM-DD | status: pass|issues | by: claude -->
```

## Core Workflow (Automation) | 核心工作流（自动化）

Session start (silent):
- Detect versions from `package.json`, `Cargo.toml`, `pubspec.yaml`, `Package.swift`
- Load relevant `~/.claude/skills/{tech}-*.md`

会话开始(静默):
- 从 `package.json` / `Cargo.toml` / `pubspec.yaml` / `Package.swift` 检测版本
- 加载对应的 `~/.claude/skills/{tech}-*.md`

First code generation (silent personalization):
- Scan 2-3 project files for naming, structure, and comment style
- Update `~/.claude/skills/project-styles/{project}.md`

首次生成代码(静默个性化):
- 扫描 2-3 个项目文件，识别命名/结构/注释风格
- 更新 `~/.claude/skills/project-styles/{project}.md`

After using a saved pattern:
- Update `feedback` counters and `last_used` using idempotent rules

使用已保存的模式后:
- 按幂等规则更新 `feedback` 计数与 `last_used`

Self-correction:
- If a suggested pattern fails, fix user code and update the skill with a `correction` marker

自我修正:
- 若建议导致失败，修复用户代码并添加 `correction` 标记

## Metadata Format | 元数据格式

Canonical metadata block (summary):

```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: {what-happened} | tech: {tech} | category: {category} -->
<!-- feedback: count=0 | success=0 | failed=0 | last_used=YYYY-MM-DD -->
<!-- version: {framework} {constraint} | runtime: {runtime} | tool: {tool} -->
<!-- lifecycle: state=active | since=YYYY-MM-DD | reason={reason} -->
<!-- scope: level=project | projects={projectA,projectB} -->
<!-- revision: id={rev_id} | parent={rev_id} -->
<!-- correction: YYYY-MM-DD | was: {old} | now: {new} | reason: {why} -->
```

Optional markers used when needed:
- `merge`: record merged patterns
- `rollback`: record automatic rollback events

## Lifecycle / Rollback / Scope Rules | 生命周期 / 回滚 / 通用性规则

Lifecycle states: `draft -> active -> deprecated -> removed`, plus `quarantined`.

生命周期状态: `draft -> active -> deprecated -> removed`，异常时可进入 `quarantined`。

Rollback triggers (windowed):
- `failed >= 3` OR success rate < 50% within the sampling window

回滚触发(采样窗口):
- `failed >= 3` 或成功率 < 50%

Rollback behavior:
- Revert to best recent active revision
- Mark current as `quarantined` and append `rollback` marker

回滚行为:
- 回退到最近表现最好的 active 版本
- 标记为 `quarantined` 并写入 `rollback` 标记

Scope promotion (cross-project):
- Promote to global only after 2+ projects or stable official API validation

通用性提升:
- 至少 2 个项目验证或官方稳定 API 证明后才提升到全局

## Health Report Semantics | 健康报告语义

Deterministic rules (short):
- Count only `## 模式` entries
- Bind `feedback` within 3 lines above entry
- Prefer 90-day window stats (`window_*`), else fall back and mark missing window data
- Flag attention when success rate < 70%, `window_failed >= 3`, `last_used` older than 90 days, or untracked

确定性规则(摘要):
- 仅统计以 `## 模式` 开头的条目
- 只绑定条目前 3 行内的 `feedback` 标记
- 优先使用 90 天窗口统计(`window_*`)，缺失则回退并标记
- 成功率 < 70%、`window_failed >= 3`、`last_used` 超过 90 天或未追踪时提示关注

## Context Cache | 上下文缓存

Cache file: `~/.claude/skills/.evolution-context.json`

```json
{
  "schema_version": 2,
  "projects": {
    "{project_id}": {
      "tech_versions": {},
      "pattern_stats": {}
    }
  }
}
```

## Examples | 示例

Pattern entry skeleton:
```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: solved-problem | tech: flutter | category: patterns -->
<!-- feedback: count=0 | success=0 | failed=0 | last_used=YYYY-MM-DD -->
<!-- lifecycle: state=active | since=YYYY-MM-DD | reason=stable -->
<!-- scope: level=project | projects={projectA} -->
<!-- revision: id=20260111-abcd | parent=20260101-1234 -->

## 模式 12: Pull to refresh

短描述。
```

Troubleshooting entry skeleton:
```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: fixed-error | tech: flutter | category: troubleshooting -->
<!-- version: flutter >= 3.0 | runtime: dart | tool: build -->
<!-- lifecycle: state=active | since=YYYY-MM-DD | reason=fixed -->
<!-- scope: level=project | projects={projectA,projectB} -->
<!-- revision: id=20260111-efgh | parent=20260101-5678 -->

### MissingPluginException

**症状**: ...
**原因**: ...
**解决方案**: ...
```

Project style profile excerpt:
```markdown
# Project Style Profile | 风格档案

- **Project Name | 项目名称**: ...
- **Tech Stack | 技术栈**: ...
- **Framework Version | 框架版本**: ...
```

## Troubleshooting | 故障排查

- Skills not found: ensure `~/.claude/skills` and `~/.claude/commands` exist, then re-copy.
- Health report shows data inconsistent: fix `count` to equal `success + failed`.
- Wrong API usage: update `version` metadata and add a `Variants` subsection.
- Duplicate patterns: normalize titles and merge entries instead of creating new ones.

- 找不到 skills: 确认 `~/.claude/skills` 与 `~/.claude/commands` 存在并重新复制。
- 健康报告显示数据不一致: 修正 `count = success + failed`。
- API 版本不匹配: 更新 `version` 并添加变体说明。
- 模式重复: 按标题规范化合并，不新增重复条目。

## See Also | 相关

- `skills/evolution/SKILL.md`
- `commands/evolve.md`
- `commands/skills-health.md`
- `commands/validate-skills.md`
- `skills/project-styles/_template.md`
