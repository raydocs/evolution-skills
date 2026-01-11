# Factory Droid Evolution Skills

Self-evolving skill system for Factory Droid.

Factory Droid 的自我进化技能系统。

## Overview | 概览

This distribution targets Factory Droid. It installs the evolution skill spec and optional `evolution-agent` droid that can learn during development. Knowledge is stored under `~/.factory/skills`.

本套内容面向 Factory Droid。它安装自我进化规范与可选的 `evolution-agent` droid，用于开发中自动学习，知识存储在 `~/.factory/skills`。

## Installation | 安装

```bash
# Copy skills | 复制 skills
cp -r skills/* ~/.factory/skills/

# Copy commands | 复制 commands
cp -r commands/* ~/.factory/commands/

# Copy droid (optional) | 复制 droid (可选)
cp -r droids/* ~/.factory/droids/

# Optional: verify install | 可选: 验证安装
ls ~/.factory/skills
ls ~/.factory/commands
ls ~/.factory/droids
```

If you have existing skills, back them up before overwriting. Re-copying updates the installed files.

如果你已有自定义 skills，覆盖前请先备份。重复复制可用于更新。

## Directory Structure | 目录结构

Repo layout (files you copy):

```
skills/
├── evolution.md         # Canonical evolution spec
└── project-styles/
    └── _template.md     # Project style template

commands/
├── evolve.md            # /evolve command
└── skills-health.md     # /skills-health command

droids/
└── evolution-agent.md   # Optional evolution droid
```

Runtime skill storage (auto-maintained by the system):

```
~/.factory/skills/
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
2. Optional: install `evolution-agent`.
3. Start a dev session; version detection and style scan run silently.
4. Solve a non-trivial problem.
5. Run `/evolve` to capture the knowledge.
6. Use `/skills-health` for health stats.

1. 安装 skills 与 commands。
2. 可选: 安装 `evolution-agent`。
3. 开始开发会话; 版本检测和风格扫描会静默执行。
4. 解决一个非平凡问题。
5. 使用 `/evolve` 记录知识。
6. 用 `/skills-health` 查看健康统计。

```bash
/evolve
/evolve flutter pull-to-refresh
/skills-health
```

## Command Reference | 命令参考

### /evolve [tech] [pattern-name]

Accepts:
- empty: review the session and propose what to save
- `<tech>`: save knowledge for that tech
- `<tech> <name>`: add a specific pattern directly

支持:
- 空参数: 回顾会话并提议要保存的内容
- `<tech>`: 保存指定技术栈的知识
- `<tech> <name>`: 直接添加指定模式

Target mapping (by category):
- patterns -> `~/.factory/skills/{tech}-patterns.md`
- troubleshooting -> `~/.factory/skills/{tech}-troubleshooting.md`
- architecture -> `~/.factory/skills/{tech}-architecture.md`
- layout -> `~/.factory/skills/{tech}-layout.md`
- build -> `~/.factory/skills/{tech}-build.md`
- fundamentals -> `~/.factory/skills/{tech}-fundamentals.md`

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

## Automation and Droid Behavior | 自动化与 Droid 行为

Automatic (silent):
- Version detection at session start
- Style scan on first code generation
- Feedback tracking after pattern usage

自动触发(静默):
- 会话开始时检测版本
- 首次生成代码时扫描项目风格
- 使用模式后更新反馈统计

Semi-automatic:
- Propose saving after solving non-trivial problems

半自动:
- 解决非平凡问题后提议保存

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

Cache file: `~/.factory/skills/.evolution-context.json`

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

- Droid not loading: confirm `~/.factory/droids` exists and the droid is installed.
- Skills not updating: check file permissions and that you used `~/.factory` paths.
- Health report shows data inconsistent: fix `count` to equal `success + failed`.
- Duplicate patterns: normalize titles and merge entries instead of creating new ones.

- Droid 未加载: 确认 `~/.factory/droids` 存在且已安装 droid。
- Skills 未更新: 检查权限并确认使用的是 `~/.factory` 路径。
- 健康报告显示数据不一致: 修正 `count = success + failed`。
- 模式重复: 按标题规范化合并，不新增重复条目。

## See Also | 相关

- `skills/evolution.md`
- `droids/evolution-agent.md`
- `commands/evolve.md`
- `commands/skills-health.md`
- `skills/project-styles/_template.md`
