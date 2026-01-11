# 通用自我进化系统

此系统让 Droid 在开发过程中持续学习并改进知识库。

---

## 自动触发 vs 手动触发

**重要**: 某些机制应该自动触发，无需用户请求。

### 自动触发 (Droid 主动执行)

| 机制 | 何时自动触发 |
|------|-------------|
| **版本适配** | 会话开始时，检测项目依赖版本 |
| **个性化** | 首次生成代码时，扫描 2-3 个现有文件了解风格 |
| **自我修正** | 当 skill 建议的代码失败时立即触发 |
| **使用反馈** | 每次使用模式后（成功或失败） |

### 半自动触发 (Droid 提议，用户确认)

| 机制 | 何时提议 |
|------|---------|
| **自我进化** | 解决了值得记录的非平凡问题后 |

### 手动触发 (用户必须请求)

| 机制 | 触发短语 |
|------|---------|
| **自我验证** | "验证 skills" |
| **健康报告** | "显示模式健康报告" |

---

## 会话开始检查清单 (自动执行)

开始开发会话时，Droid 应自动:

```
1. [ ] 从 package.json/Cargo.toml/pubspec.yaml 等检测框架版本
      → 设置内部上下文: "使用 {框架} {版本}"

2. [ ] 扫描项目风格 (如果之前没做过)
      → 记录命名规范、模块结构

3. [ ] 应用版本特定和个性化的建议
      → 所有后续建议自动适配
```

**Droid: 静默执行这些。除非与用户问题相关，否则不要宣布。**

---

## 何时进化

当开发过程中发生以下任何情况时触发 skill 进化:

| 触发条件 | 目标文件 | 优先级 |
|---------|---------|-------|
| 发现新的 UI 组件模式 | {tech}-patterns.md | 高 |
| 学到新的架构技术 | {tech}-architecture.md | 高 |
| 解决了编译/运行时错误 | {tech}-troubleshooting.md | 高 |
| 找到布局/响应式方案 | {tech}-layout.md | 中 |
| 解决了构建/部署问题 | {tech}-build.md | 中 |
| 理清了核心概念 | {tech}-fundamentals.md | 低 |

其中 `{tech}` 是技术栈名称，如 `flutter`、`swiftui`、`react`、`rust` 等。

---

## 进化流程

### Step 1: 识别值得捕获的知识

问自己:
- 这是可复用的模式吗？(不是项目特定的)
- 花了很大力气才弄清楚吗？
- 会帮助其他开发者吗？
- 还没有记录在 skills 中吗？

### Step 2: 分类知识

```
UI 组件/Widget 模式  → ~/.factory/skills/{tech}-patterns.md
架构/状态管理模式    → ~/.factory/skills/{tech}-architecture.md
错误/调试解决方案    → ~/.factory/skills/{tech}-troubleshooting.md
布局/响应式设计      → ~/.factory/skills/{tech}-layout.md
构建/部署问题        → ~/.factory/skills/{tech}-build.md
核心概念/API         → ~/.factory/skills/{tech}-fundamentals.md
```

### Step 3: 格式化贡献

**统一元数据块** (所有条目通用):
```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: {what-happened} | tech: {tech} | category: {category} -->
<!-- feedback: count=0 | success=0 | failed=0 | last_used=YYYY-MM-DD -->
<!-- version: {framework} {constraint} | runtime: {runtime} | tool: {tool} -->
<!-- lifecycle: state=active | since=YYYY-MM-DD | reason={reason} -->
<!-- scope: level=project | projects={projectA,projectB} -->
<!-- revision: id={rev_id} | parent={rev_id} -->
<!-- correction: YYYY-MM-DD | was: {old} | now: {new} | reason: {why} -->
```
规则:
- `evolution` 必填
- 模式条目必须带 `feedback`，问题解决条目可选
- `version`/`correction` 按需添加
- `category` 取值: patterns / troubleshooting / architecture / layout / build / fundamentals
- `lifecycle` 标记生命周期状态；`scope` 标记通用性；`revision` 用于回滚链路

**模式格式**:
```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: {what-happened} | tech: {tech} | category: patterns -->
<!-- feedback: count=0 | success=0 | failed=0 | last_used=YYYY-MM-DD -->
<!-- version: {framework} {constraint} | runtime: {runtime} | tool: {tool} -->
<!-- lifecycle: state=active | since=YYYY-MM-DD | reason={reason} -->
<!-- scope: level=project | projects={projectA,projectB} -->
<!-- revision: id={rev_id} | parent={rev_id} -->

## 模式 N: [模式名称]

简要描述这个模式解决什么问题。

### 代码示例

\```{language}
// 代码
\```

### 使用场景

- 场景 1
- 场景 2

### 注意事项

- 注意点 1
```

**问题解决格式**:
```markdown
<!-- evolution: YYYY-MM-DD | source: {project} | trigger: fixed-error | tech: {tech} | category: troubleshooting -->
<!-- version: {framework} {constraint} | runtime: {runtime} | tool: {tool} -->
<!-- lifecycle: state=active | since=YYYY-MM-DD | reason={reason} -->
<!-- scope: level=project | projects={projectA,projectB} -->
<!-- revision: id={rev_id} | parent={rev_id} -->

### [错误类型/消息]

**症状**: 开发者看到什么

**原因**: 为什么发生

**解决方案**:
\```{language}
// 修复后的代码
\```

**相关版本**: {框架} {版本}
```

### Step 3.5: 去重与编号

- 模式编号: 读取目标文件中已有的 `## 模式 N:`，取最大 N + 1。
- 去重判定: 标题规范化（小写、去标点、空格折叠）相同视为同一模式；标题不同但核心步骤或代码签名一致时合并。
- 更新策略: 优先更新原条目并补充边缘情况；需要纠正时添加 `correction` 标记。

### Step 3.6: 条目生命周期

状态流转: `draft -> active -> deprecated -> removed`，异常时可进入 `quarantined`。

- `active` 才能默认推荐。
- `deprecated` 仅在用户明确要求或迁移提示时使用。
- `quarantined` 不推荐，需修复或回滚后恢复。
- `removed` 保留墓碑并指向替代条目。

元数据示例:
```markdown
<!-- lifecycle: state=active | since=YYYY-MM-DD | reason={reason} -->
```

### Step 3.7: 冲突解决与合并

当多个模式同时匹配时，按以下顺序选择:
1. tech + version 精确匹配
2. 采样窗口内成功率更高
3. `last_used` 更近
4. 与项目风格更一致

合并记录标记:
```markdown
<!-- merge: YYYY-MM-DD | from={old_id} | into={new_id} | reason={why} -->
```

变体处理: 在同一条目下追加 `Variants` 小节，不新增散乱文件。

### Step 4: 更新 Skill 文件

1. 读取目标 skill 文件 (如果存在)
2. 找到适当的部分
3. 按现有格式添加新内容
4. 确保没有重复内容
5. 如果文件不存在，创建它

### Step 5: 添加进化标记

```markdown
<!-- evolution: {date} | source: {project} | trigger: {what-happened} -->
```

将此注释放在你添加的新内容上方。

---

## Skill 文件位置

### 目录结构

```
~/.factory/skills/
├── evolution.md              # 本文件 - 主控制
├── flutter-patterns.md       # Flutter UI 模式
├── flutter-troubleshooting.md
├── flutter-layout.md         # Flutter 布局/响应式
├── flutter-fundamentals.md   # Flutter 核心概念/API
├── swiftui-patterns.md       # SwiftUI 模式
├── swiftui-troubleshooting.md
├── react-patterns.md         # React 模式
├── rust-patterns.md          # Rust 模式
└── project-styles/
    └── {project-name}.md     # 项目特定风格
```

### 创建新的 Skill 文件

当遇到新技术栈时，创建对应的文件:

```bash
# 示例: 创建 Vue 相关 skills
touch ~/.factory/skills/vue-patterns.md
touch ~/.factory/skills/vue-troubleshooting.md
```

文件开头模板:
```markdown
# {Tech} Patterns

此文件记录 {Tech} 开发中发现的可复用模式。

---

## 模式列表

(模式将在开发过程中自动添加)
```

---

## 自我修正 (Self-Correction)

当 skill 内容导致错误时，自动修正。

**自动触发: 是** - Droid 应在错误发生时自动修正 skills。

### 触发条件

```
用户遵循 skill 建议 → 代码编译/运行失败 → Droid 识别 skill 是错的
                                              ↓
                                    自动: 立即修正 skill
```

### Droid 自动行为

当 Droid 检测到 skill 中的代码导致错误时:

1. **识别** - "这个错误是因为 skill 建议是错的"
2. **修复用户代码** - 提供正确的解决方案
3. **更新 skill** - 自动编辑 skill 文件 (无需询问)
4. **通知用户** - "我也更新了 skills 以防止其他人遇到这个错误"

### 修正标记格式

```markdown
<!-- correction: {date} | was: {old-code-summary} | now: {new-code-summary} | reason: {why} -->
```

---

## 自动回滚 (Auto Rollback)

触发条件 (采样窗口内任一满足):
- `failed >= 3`
- 成功率 < 50%

回滚行为:
- 回退到最近 `active` 且成功率最高的 `revision`。
- 将当前条目标记为 `quarantined`。
- 追加回滚标记并记录原因。

回滚标记:
```markdown
<!-- rollback: YYYY-MM-DD | from_rev={r2} | to_rev={r1} | trigger={failed>=3} | reason={why} -->
```

revision 规则: `rev_id` 使用 `YYYYMMDD-<hash>`，每次内容变更必须更新 `revision`。

## 自我验证 (Self-Validation)

定期验证 skill 内容是否仍然准确。

### 验证触发

| 触发条件 | 动作 |
|---------|------|
| 用户说 "验证 skills" | 完整验证 |
| 使用 skill 代码编译失败 | 针对性验证 |
| 检测到新框架版本 | API 验证 |

### 验证检查清单

```markdown
## 验证报告

### 代码示例
- [ ] 所有代码示例可以编译
- [ ] 所有模式按文档工作

### API 准确性
- [ ] API 签名正确
- [ ] 方法名称正确

### 完整性
- [ ] 没有过时的模式 (无警告)
- [ ] 没有遗漏的常见用例
```

---

## 使用反馈 (Usage Feedback)

跟踪哪些模式有效，哪些导致问题。

**自动触发: 是** - Droid 应在使用模式后自动记录反馈。

### Droid 自动行为

使用 skills 中的任何模式后:

1. **静默跟踪** - 不要向用户宣布反馈记录
2. **成功时** - 更新标记: `success += 1`
3. **失败时** - 更新标记: `failed += 1`，然后触发自我修正
4. **定期** - 如果模式失败 3+ 次，主动建议修复

### 幂等更新规则

- pattern_id = 模式标题规范化结果（小写、去标点、空格折叠）。
- 为当前会话维护 `usage_set`，键为 `{pattern_id}:{outcome}:{target}`。
- 若键已存在，不重复计数；仅当计数发生变化时更新 `last_used`。
- `count` 必须等于 `success + failed`；不一致时标记为 "数据不一致" 并在健康报告提示。

### 采样窗口

- 默认窗口: 最近 90 天。
- 记录窗口内 `window_count/window_success/window_failed`，无窗口数据时回退到累计值并标记 "窗口数据缺失"。

### 反馈标记格式

在模式开头添加:

```markdown
<!-- feedback: count=5 | success=4 | failed=1 | last_used=2026-01-10 -->
## 模式 N: ...
```

### 健康报告

用户可以请求:

```
"显示模式健康报告"
```

输出:
```markdown
## 模式健康报告

| 模式 | 使用次数 | 成功 | 失败 | 成功率 | 状态 |
|------|---------|------|------|-------|------|
| Flutter 下拉刷新 | 10 | 9 | 1 | 90% | ✅ 健康 |
| SwiftUI 导航 | 5 | 3 | 2 | 60% | ⚠️ 需审查 |
| React useEffect 清理 | 3 | 0 | 3 | 0% | ❌ 可能损坏 |
```

| 成功率 | 动作 |
|-------|------|
| > 90% | 模式稳定，无需动作 |
| 70-90% | 审查模式的边缘情况 |
| 50-70% | 模式需要改进 |
| < 50% | 模式可能损坏，需修复或移除 |

### 健康报告计算规则 (确定性)

1. 条目识别: 仅统计以 `## 模式` 开头的条目。
2. 反馈绑定: 仅关联条目前 3 行内的 `feedback` 标记; 缺失则标记为未追踪。
3. 窗口优先: 使用 `window_*` 统计(90 天)，缺失则回退到累计并标记 "窗口数据缺失"。
4. 成功率: `window_count > 0` 时 `window_success / window_count`，否则为 N/A。
5. 关注规则: 成功率 <70% 或 `window_failed >= 3` 或 `last_used` 超过 90 天或未追踪。
6. 聚合: 技术栈优先来自 `tech` 元数据，缺失时由文件名推断。

---

## 版本适配 (Version Adaptation)

为不同框架版本提供特定指导。

**自动触发: 是** - Droid 应在会话开始时检测版本。

### Droid 自动行为

在任何开发会话开始时:

1. **读取配置文件** - 查找依赖版本
   - Flutter: `pubspec.yaml`
   - Node.js: `package.json`
   - Rust: `Cargo.toml`
   - Swift: `Package.swift` 或 Xcode 项目
   
2. **提取版本** - 记录主要框架版本

3. **设置上下文** - 记住这个用于所有后续建议

4. **静默适配** - 不要宣布，只使用正确的 API

5. **不匹配时警告** - 如果用户代码使用错误的 API，解释版本差异

### 版本特定内容格式

在 skill 文件中:

```markdown
### 某个功能

<!-- version: flutter>=3.0 -->
\```dart
// Flutter 3.0+ 的写法
\```

<!-- version: flutter<3.0 -->
\```dart  
// Flutter 3.0 之前的写法
\```
```

---

## 个性化 (Personalization)

使 skill 建议适应项目的编码风格。

**自动触发: 是** - Droid 应在首次生成代码时检测项目风格。

### Droid 自动行为

首次请求生成代码时:

1. **快速扫描** - 读取项目中 2-3 个现有文件
2. **记录模式** - 命名规范、模块结构、注释风格
3. **记住** - 存储在对话上下文中
4. **应用** - 所有生成的代码匹配项目风格
5. **静默** - 不要向用户宣布此分析

### 风格检测

| 方面 | 检测方法 | 适配 |
|-----|---------|------|
| 命名规范 | 扫描现有代码 | 匹配 snake_case vs camelCase |
| 代码组织 | 检查模块结构 | 建议匹配的模式 |
| 注释风格 | 读取现有注释 | 匹配文档风格 |
| 复杂度 | 计算每文件行数 | 建议适当的模式 |

### 项目风格档案

为每个项目保存风格到 `~/.factory/skills/project-styles/{project}.md`:

```markdown
# {Project} 风格档案

- **命名规范**: camelCase
- **模块组织**: feature-first
- **状态管理**: BLoC
- **注释风格**: 简洁
- **复杂度偏好**: 小组件
```

## 跨会话上下文存储

缓存文件: `~/.factory/skills/.evolution-context.json`

```json
{
  "schema_version": 2,
  "projects": {
    "{project_id}": {
      "root": "/abs/path",
      "tech_versions": { "flutter": "3.19.0" },
      "style_profile": { "naming": "camelCase", "structure": "feature-first" },
      "deps_signature": "{hash}",
      "updated_at": "YYYY-MM-DD",
      "pattern_stats": {
        "{pattern_id}": {
          "generality_level": "project",
          "source_projects": ["projectA", "projectB"],
          "window_days": 90,
          "window_count": 0,
          "window_success": 0,
          "window_failed": 0,
          "window_updated_at": "YYYY-MM-DD"
        }
      }
    }
  }
}
```

失效规则:
- 兼容: 缺失 `pattern_stats` 时默认 `generality_level=project`，`window_days=90`。
- 依赖文件签名变化（package.json/Cargo.toml/pubspec.yaml/Package.swift/xcodeproj）→ 重新检测版本
- project root 变化 → 新 project_id
- style_profile 超过 180 天未更新 → 重新扫描 2-3 个文件

---

## 自动进化提示

这些提示触发自我进化:

### 解决问题后

> "这个解决方案应该添加到 skills 供将来参考。"

### 创建组件后

> "这个组件模式可复用。让我添加到 {tech}-patterns。"

### 调试后

> "这个错误和修复应该记录到 {tech}-troubleshooting。"

### 完成功能后

> "回顾我学到的内容，如果适用就更新 skills。"

---

## 跨项目通用性

满足任一条件才可提升到全局 skills:
- 至少 2 个不同项目验证通过
- 明确的框架官方行为/稳定 API
- 有最小复现与通过步骤

否则保留在 `project-styles/{project}.md` 或条目内 `Variants`。

通用性标记:
```markdown
<!-- scope: level=project | projects={projectA,projectB} -->
```

## 质量指南

### 应该添加

- 通用、可复用的模式
- 常见错误及清晰解决方案
- 经过测试的效果/技术
- 平台特定的注意事项
- 性能优化

### 不应该添加

- 项目特定的代码
- 未验证的解决方案
- 重复内容
- 不完整的示例
- 没有理由的个人偏好

---

## 持续改进检查清单

每次开发会话后，考虑:

- [ ] 我发现了新的组件/模式吗？
- [ ] 我解决了棘手的问题吗？
- [ ] 我遇到并修复了令人困惑的错误吗？
- [ ] 我找到了更好的方式来组织代码吗？
- [ ] 我学到了关于构建/部署的东西吗？
- [ ] 这些会帮助其他开发者吗？

如果任何一个是肯定的，进化适当的 skill！

---

## 综合自我改进工作流

所有机制协同工作:

```
┌─────────────────┐
│   开发会话     │
└────────┬────────┘
         │
┌────────┴────────┬─────────────────┐
│                 │                 │
▼                 ▼                 ▼
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  个性化     │  │  使用模式   │  │  检测版本   │
│  建议       │  │             │  │             │
└─────────────┘  └──────┬──────┘  └─────────────┘
                        │
         ┌──────────────┼──────────────┐
         │              │              │
         ▼              ▼              ▼
   ┌──────────┐  ┌──────────┐  ┌──────────┐
   │  成功    │  │  失败    │  │  新模式  │
   │  +1      │  │  +1      │  │  发现    │
   └──────────┘  └────┬─────┘  └────┬─────┘
                      │              │
                      ▼              ▼
               ┌──────────┐  ┌──────────┐
               │  自我    │  │  自我    │
               │  修正    │  │  进化    │
               └──────────┘  └──────────┘
                      │              │
                      └──────┬───────┘
                             │
                             ▼
                      ┌─────────────┐
                      │   改进的    │
                      │   Skills    │
                      └─────────────┘
