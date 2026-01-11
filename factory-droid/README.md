# Factory Droid Evolution Skills

Self-evolving skill system for Factory Droid.

Factory Droid 的自我进化技能系统。

## Installation | 安装

```bash
# Copy skills | 复制 skills
cp -r skills/* ~/.factory/skills/

# Copy commands | 复制 commands
cp -r commands/* ~/.factory/commands/

# Copy droid (optional) | 复制 droid (可选)
cp droids/* ~/.factory/droids/
```

## Files | 文件

```
skills/
├── evolution.md         # Main evolution skill
└── project-styles/
    └── _template.md     # Project style template

commands/
├── evolve.md            # /evolve command
└── skills-health.md     # /skills-health command

droids/
└── evolution-agent.md   # Evolution agent droid
```

## Usage | 使用

| Command | Description |
|---------|-------------|
| `/evolve` | Save learned knowledge |
| `/evolve flutter` | Save Flutter knowledge |
| `/skills-health` | Show health report |

## Droid | 专家角色

Use `evolution-agent` droid for automatic learning during development.

使用 `evolution-agent` droid 在开发过程中自动学习。

## Automatic Behaviors | 自动行为

- Version detection at session start
- Style personalization on first code generation
- Self-correction when code fails
- Usage tracking after using patterns
