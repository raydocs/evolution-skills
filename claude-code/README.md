# Claude Code Evolution Skills

Self-evolving skill system for Claude Code.

Claude Code 的自我进化技能系统。

## Installation | 安装

```bash
# Copy skills | 复制 skills
cp -r skills/* ~/.claude/skills/

# Copy commands | 复制 commands
cp -r commands/* ~/.claude/commands/
```

## Files | 文件

```
skills/
├── evolution/
│   └── SKILL.md         # Main evolution skill
└── project-styles/
    └── _template.md     # Project style template

commands/
├── evolve.md            # /evolve command
├── skills-health.md     # /skills-health command
└── validate-skills.md   # /validate-skills command
```

## Usage | 使用

| Command | Description |
|---------|-------------|
| `/evolve` | Save learned knowledge |
| `/evolve flutter` | Save Flutter knowledge |
| `/skills-health` | Show health report |
| `/validate-skills` | Validate accuracy |

## Automatic Behaviors | 自动行为

- Version detection at session start
- Style personalization on first code generation
- Self-correction when code fails
- Usage tracking after using patterns
