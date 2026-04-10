# Tutorial: Creating a New Skill Repository with GitHub Copilot CLI

# 教程：使用 GitHub Copilot CLI 创建新的技能仓库

---

> This tutorial is based on a real session that produced the `d365-fo-skills` repository.
> It covers the full lifecycle — from first prompt to a working, compliant skill system —
> and documents every issue that arose and how each was fixed.
>
> 本教程基于一次真实的会话，该会话创建了 `d365-fo-skills` 仓库。
> 内容涵盖从第一次提示到可用技能系统的完整生命周期，
> 并记录了过程中出现的每个问题以及修复方式。

---

## Table of Contents / 目录

**English**
1. [What This Tutorial Covers](#1-what-this-tutorial-covers)
2. [Prerequisites](#2-prerequisites)
3. [Step 1 — Invoke the Skill](#3-step-1--invoke-the-skill)
4. [Step 2 — Answer the Progressive Questions](#4-step-2--answer-the-progressive-questions)
5. [Step 3 — Review the Architecture Output](#5-step-3--review-the-architecture-output)
6. [Step 4 — Copilot Generates the Files](#6-step-4--copilot-generates-the-files)
7. [Step 5 — Add Domain Content](#7-step-5--add-domain-content)
8. [Common Issues and Fixes](#8-common-issues-and-fixes)
9. [Iterating on an Existing Repository](#9-iterating-on-an-existing-repository)
10. [Pre-Use Checklist](#10-pre-use-checklist)

**中文**
1. [本教程的内容](#1-本教程的内容)
2. [前提条件](#2-前提条件)
3. [第一步 — 激活技能](#3-第一步--激活技能)
4. [第二步 — 回答渐进式问题](#4-第二步--回答渐进式问题)
5. [第三步 — 审查架构输出](#5-第三步--审查架构输出)
6. [第四步 — Copilot 生成文件](#6-第四步--copilot-生成文件)
7. [第五步 — 添加领域内容](#7-第五步--添加领域内容)
8. [常见问题与修复](#8-常见问题与修复)
9. [迭代现有仓库](#9-迭代现有仓库)
10. [首次使用前的检查清单](#10-首次使用前的检查清单)

---

# English

---

## 1. What This Tutorial Covers

This tutorial walks you through using the `building-skill-systems` skill inside GitHub Copilot CLI to create a new, well-structured skill repository from scratch.

You will learn:
- how to invoke the skill and drive a design conversation
- what questions Copilot asks and why
- what the generated repository structure looks like
- how to add automation (hooks, agents, scripts) to your repo
- how to identify and fix the four most common structural issues

The example used throughout is the `d365-fo-skills` repository — a skill system for Dynamics 365 Finance & Operations development. But the process applies to **any domain**.

---

## 2. Prerequisites

Before starting, ensure:

1. **GitHub Copilot CLI is installed** — `copilot` command is available in your terminal
2. **You know your target domain** — what kind of work will this skill system support?
3. **You have a folder path in mind** — where will the new repository live? (e.g., `C:\Projects\my-skills`)
4. **Git is installed** — the skill system will be version-controlled from the start
5. **PowerShell is available** (Windows) — hook scripts use PowerShell

---

## 3. Step 1 — Invoke the Skill

Start a Copilot CLI session from any directory:

```bash
copilot
```

Then tell Copilot what you want. You do **not** need to invoke the skill manually — Copilot will detect the task and use `building-skill-systems` automatically. But you can also be explicit:

```
I want to create my own skill repository. Could you help me design it?
```

Or more explicitly:

```
Use the /building-skill-systems skill to help me create a new skill repository.
```

Copilot will respond with:

> "I'm using the building-skill-systems skill to design this skill repository as a complete system.
> I'll do this in steps so you don't need to provide everything at once..."

**What's happening:** The skill's `SKILL.md` is injected into Copilot's context, which gives it the full design methodology — progressive questioning, architecture-first thinking, and placeholder discipline.

---

## 4. Step 2 — Answer the Progressive Questions

The skill asks questions in small rounds. The **first round** establishes the repository shape:

| Question | Purpose |
|----------|---------|
| New repository or refactor of an existing one? | Determines Mode A (new) or Mode B (refactor) |
| What domain or task area? | Shapes the skill taxonomy |
| Who are the primary users? | Affects workflow philosophy |
| Lightweight skill pack or full workflow framework? | Determines structural scope |
| Which platforms should it support first? | Determines directory layout |
| Where should the new repository be created? | Gets the exact folder path |

### Example answers (from the real session)

```
1. New repository
2. Dynamics 365 Finance & Operations development
3. Small dev team
4. Full workflow framework
5. GitHub Copilot CLI only
6. C:\Projects\d365-fo-skills
```

After round 1, Copilot asks a **second round** of targeted follow-ups based on what you said. For a full framework, it asks:

- Should it include hooks?
- Should it include docs/specs/plans?
- Should it include tests?
- Should it include reviewer/agent roles?

### Tips for answering

- Be specific about domain — "D365 F&O development" is better than "software development"
- If unsure about scope, say "full workflow framework" — it's easier to remove than to add later
- For platform: if you only use GitHub Copilot CLI, say so — the layout will differ from Claude Code
- Always provide the full folder path — never assume a default

---

## 5. Step 3 — Review the Architecture Output

After you answer the questions, Copilot produces a **Skill System Brief** and a full architecture design. This includes:

- **Skill System Brief** — purpose, domain, users, platforms, scope, constraints
- **Directory tree** — the exact folder structure to create
- **Directory purpose map** — what each directory is for
- **Skill matrix** — all skills, their roles, triggers, and status (draft / placeholder)
- **Workflow design** — primary workflow + alternate branches
- **Platform integration matrix** — how each platform discovers and uses the skills
- **Testing matrix** — what gets tested, when, how
- **Placeholder vs complete content map** — what gets built now vs later

### What to check before proceeding

- [ ] Does the directory tree use `.github/skills/` (not root `skills/`) for Copilot CLI?
- [ ] Does it include `.github/hooks/` if you asked for hooks?
- [ ] Is there a bootstrap skill (`using-<system-name>`)?
- [ ] Does the skill matrix include at least one meta skill and one quality skill?
- [ ] Are placeholders clearly labeled (not implied as complete)?

If anything looks wrong, tell Copilot now — it is much easier to correct the design before file generation.

---

## 6. Step 4 — Copilot Generates the Files

Once you approve the architecture, tell Copilot to generate the files:

```
Looks good. Please generate the repository structure and starter files.
```

Copilot will:
1. Create the directory at your specified path and initialize a Git repo
2. Create all `SKILL.md` files with starter content
3. Create `README.md`, `CHANGELOG.md`, `.gitignore`, `COPILOT.md`
4. Create platform integration files (`.github/copilot-instructions.md`)
5. Create test scaffolding in `tests/`
6. Create docs structure
7. Make an initial commit

### What Copilot generates per skill

Each skill lives in its own directory with a `SKILL.md`:

```
.github/skills/<skill-name>/
└── SKILL.md
```

The `SKILL.md` has YAML frontmatter and a Markdown body:

```markdown
---
name: <skill-name>
description: What this skill does and when to use it.
---

# Skill Title

## When to Use

...

## Process

...
```

---

## 7. Step 5 — Add Domain Content

After the skeleton is generated, you add the real domain-specific content.

### Adding skills

Edit each `SKILL.md` to fill in:
- specific workflow steps for your domain
- checklists and quality gates
- examples and anti-patterns
- links to related skills

### Adding agents

Create agent files in `.github/agents/` with the `.agent.md` extension:

```markdown
---
name: my-agent
description: When and how to use this agent.
---

# Agent Name

## Role

...

## Instructions

...
```

### Adding hooks

Create a hooks config in `.github/hooks/`:

```json
{
  "version": 1,
  "hooks": {
    "sessionStart": [
      {
        "type": "command",
        "powershell": "./scripts/session-start.ps1",
        "cwd": ".github/hooks",
        "timeoutSec": 30,
        "comment": "Environment check on session open"
      }
    ]
  }
}
```

Create the script in `.github/hooks/scripts/session-start.ps1`. All hook scripts **must** read stdin:

```powershell
$ErrorActionPreference = "Stop"
$inputData = [Console]::In.ReadToEnd() | ConvertFrom-Json
# $inputData.source, $inputData.cwd, etc.
```

### Adding PowerShell scripts to skills

Skills can include runnable scripts. Place them in the skill directory alongside `SKILL.md`:

```
.github/skills/my-skill/
├── SKILL.md          ← instructions for Copilot
└── my-script.ps1     ← script Copilot can run
```

Reference the script from `SKILL.md`:

```markdown
When asked to do X, run `my-script.ps1` from this skill's directory.
```

---

## 8. Common Issues and Fixes

These four issues arose during the real session that created `d365-fo-skills`. You may encounter the same ones.

---

### Issue 1: Some questions don't support freeform answers

**Symptom:** Copilot asks you to choose from a fixed list (e.g., yes/no) but you need to give a different answer. There is no way to type freely.

**Why it happens:** The `ask_user` tool has an `allow_freeform` parameter. When it is set to `false`, users cannot type custom answers — they can only select from the provided choices.

**Fix:** If you notice this happening, tell Copilot:

```
Your questions should always allow me to type a custom answer, not just pick from a list.
```

Copilot will update its behavior. For repository maintainers, the fix is to ensure all `ask_user` calls use `allow_freeform: true` (which is the default).

**Status in this repo:** Fixed. The `building-skill-systems` SKILL.md now contains an explicit rule that all questions must allow freeform input.

---

### Issue 2: New repo folder path is never asked

**Symptom:** Copilot designs the architecture but creates files in the wrong location, or asks you to specify a path only after it has already started generating.

**Why it happens:** The original skill did not explicitly include "where should the new repository be created?" as a first-round question. Copilot skipped it or asked too late.

**Fix:** When you start a new repo session, explicitly state the path upfront:

```
I want to create a new skill repository at C:\Projects\my-skills.
```

Or, if Copilot has already started without asking, say:

```
Before generating any files, confirm the exact folder path with me.
```

**Status in this repo:** Fixed. Question 6 in Phase 1 now explicitly asks for the full folder path for Mode A (new repository) sessions.

---

### Issue 3: Skills and agents placed in root `skills/` instead of `.github/skills/`

**Symptom:** The generated repository has `skills/` and `agents/` at the repository root. When you run Copilot CLI from the repo directory, it cannot find the skills or agents.

**Why it happens:** GitHub Copilot CLI discovers skills from `.github/skills/` and agents from `.github/agents/` — not from root-level directories. Older design guidance (and some templates) used root-level `skills/` which is not the correct path for Copilot CLI.

**Fix (move the files):**

```powershell
# Create correct directories
New-Item -ItemType Directory -Path ".github\skills" -Force
New-Item -ItemType Directory -Path ".github\agents" -Force

# Move skills
Move-Item -Path "skills\*" -Destination ".github\skills\"

# Move agents
Move-Item -Path "agents\*" -Destination ".github\agents\"

# Remove old directories
Remove-Item -Path "skills" -Recurse
Remove-Item -Path "agents" -Recurse
```

Then update any references in `copilot-instructions.md` or skill files that point to the old paths.

**Verify:** After moving, run `copilot` from the repo root and type `/skills list`. You should see your skills listed.

**Status in this repo:** Fixed. The Phase 2 "GitHub Copilot CLI layout" tree now correctly shows skills and agents inside `.github/`.

---

### Issue 4: `hooks.json` has the wrong format

**Symptom:** Hooks do not fire. Or hooks fire but scripts fail. The Copilot CLI troubleshooter says "Hooks are not executing."

**Why it happens:** The generated `hooks.json` may use an outdated format that does not match the official GitHub Copilot CLI specification. The most common mistake is using `"hooks"` as an **array** instead of an **object**.

**Wrong format (generated by older guidance):**

```json
{
  "version": 1,
  "hooks": [
    {
      "event": "sessionStart",
      "script": "scripts/hooks/session-start.ps1"
    }
  ]
}
```

**Correct format (official spec):**

```json
{
  "version": 1,
  "hooks": {
    "sessionStart": [
      {
        "type": "command",
        "powershell": "./scripts/session-start.ps1",
        "cwd": ".github/hooks",
        "timeoutSec": 90,
        "comment": "Check environment and auto-launch VS"
      }
    ]
  }
}
```

**Key differences:**

| | Wrong | Correct |
|-|-------|---------|
| `"hooks"` type | Array `[...]` | Object `{...}` |
| Event key | `"event": "sessionStart"` | `"sessionStart": [...]` as object key |
| Script reference | `"script": "path"` | `"powershell": "path"` or `"bash": "path"` |
| Required `type` | Missing | `"type": "command"` required |
| Working directory | Missing | `"cwd": ".github/hooks"` — set this |
| Timeout | Missing | `"timeoutSec": 30` (default) |

**Hook script stdin fix:**

All hook scripts receive JSON input via stdin. Scripts that do not read it will miss context. Add this at the top of every hook script:

```powershell
$ErrorActionPreference = "Stop"
$inputData = [Console]::In.ReadToEnd() | ConvertFrom-Json
```

Then use the input fields relevant to that hook:

| Hook event | Key input fields |
|-----------|-----------------|
| `sessionStart` | `$inputData.source` (`"new"`, `"resume"`, `"startup"`) |
| `userPromptSubmitted` | `$inputData.prompt` |
| `preToolUse` | `$inputData.toolName`, `$inputData.toolArgs` (JSON string) |
| `postToolUse` | `$inputData.toolResult.resultType`, `$inputData.toolResult.textResultForLlm` |
| `errorOccurred` | `$inputData.error.message`, `$inputData.error.name` |

**File location fix:**

Hooks must also be in `.github/hooks/`, not at the repo root:

```
.github/
└── hooks/
    ├── hooks.json         ← config file here
    ├── logs/              ← add to .gitignore
    └── scripts/
        ├── session-start.ps1
        └── ...
```

**Add to `.gitignore`:**

```gitignore
# Hook audit logs — local only
.github/hooks/logs/
```

**Validate your JSON:**

```powershell
Get-Content ".github/hooks/hooks.json" | ConvertFrom-Json | ConvertTo-Json
```

If this fails, there is a JSON syntax error. Fix the syntax before testing.

**Status in this repo:** Fixed in both `d365-fo-skills` and `building-skill-systems`. The Phase 4 Platform Integration section now documents the full official hooks format.

---

## 9. Iterating on an Existing Repository

Once your repository is created, you will continue to add content over time. The same `building-skill-systems` skill supports **Mode B (refactor)**.

### Adding new skills

```
I want to add a new skill to my existing skill repository for [topic].
Use the /building-skill-systems skill to help me design it.
```

Or use the meta-skill in your own repo (e.g., `writing-d365-skills`) if you created one.

### Adding new hooks

1. Add the new entry to `.github/hooks/hooks.json` in the correct event array
2. Create the script in `.github/hooks/scripts/`
3. Add stdin reading to the script
4. Test by running `copilot` from the repo root

### Expanding agents

1. Create or edit the `.agent.md` file in `.github/agents/`
2. The agent file is a Markdown file with role, tool access, and instructions
3. Invoke with `/agent <name>` or by asking Copilot to use that agent

### Version tracking

Always commit your changes with descriptive messages:

```bash
git add .
git commit -m "Add <feature>: <brief description>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

Update `CHANGELOG.md` when adding significant content.

---

## 10. Pre-Use Checklist

Before declaring your skill repository ready to use, verify:

### Structure
- [ ] Skills are in `.github/skills/<name>/SKILL.md` (not root `skills/`)
- [ ] Agents are in `.github/agents/<name>.agent.md` (not root `agents/`)
- [ ] Hooks (if any) are in `.github/hooks/hooks.json` (not root `hooks.json`)
- [ ] Hook scripts are in `.github/hooks/scripts/`
- [ ] `.github/hooks/logs/` is in `.gitignore`

### Hooks format
- [ ] `"hooks"` is an object keyed by event name (not an array)
- [ ] Each hook entry has `"type": "command"`
- [ ] `"bash"` and/or `"powershell"` keys are used (not `"script"`)
- [ ] `"cwd"` is set to `".github/hooks"`
- [ ] `"timeoutSec"` is set appropriately for the script's expected duration
- [ ] All hook scripts read stdin JSON via `[Console]::In.ReadToEnd() | ConvertFrom-Json`

### Skill content
- [ ] Each `SKILL.md` has valid YAML frontmatter (`name`, `description`)
- [ ] The bootstrap skill (`using-<system-name>`) exists and routes to other skills
- [ ] `copilot-instructions.md` references correct paths (`.github/skills/`, `.github/agents/`)

### Testing
- [ ] At least 3 trigger prompts exist in `tests/skill-triggering/prompts/`
- [ ] At least one explicit-request prompt per core skill exists

### Verification
- [ ] Run `copilot` from the repo root
- [ ] Type `/skills list` — all your skills should appear
- [ ] Type `/agent` — all your agents should appear
- [ ] Start a session and verify the `sessionStart` hook fires (if configured)

---

---

# 中文

---

## 1. 本教程的内容

本教程将引导你使用 GitHub Copilot CLI 中的 `building-skill-systems` 技能，从零开始创建一个结构完整的技能仓库。

你将学到：
- 如何激活技能并驱动设计对话
- Copilot 会问哪些问题，以及背后的原因
- 生成的仓库结构是什么样的
- 如何向仓库中添加自动化内容（hooks、agents、脚本）
- 如何识别并修复四个最常见的结构问题

贯穿全文的示例是 `d365-fo-skills` 仓库——一个用于 Dynamics 365 Finance & Operations 开发的技能系统。但这个流程适用于**任何领域**。

---

## 2. 前提条件

开始之前，请确保：

1. **已安装 GitHub Copilot CLI** — 终端中可以使用 `copilot` 命令
2. **明确目标领域** — 这个技能系统将支持什么类型的工作？
3. **有明确的文件夹路径** — 新仓库将存放在哪里？（例如：`C:\Projects\my-skills`）
4. **已安装 Git** — 技能系统从一开始就需要版本控制
5. **PowerShell 可用**（Windows）— hook 脚本使用 PowerShell

---

## 3. 第一步 — 激活技能

从任意目录启动 Copilot CLI 会话：

```bash
copilot
```

然后告诉 Copilot 你想要做什么。你**不需要**手动激活技能——Copilot 会自动检测任务并使用 `building-skill-systems`。当然，你也可以明确指定：

```
我想创建自己的技能仓库，你能帮我设计吗？
```

或者更明确地：

```
请使用 /building-skill-systems 技能帮我创建一个新的技能仓库。
```

Copilot 将回应：

> "I'm using the building-skill-systems skill to design this skill repository as a complete system.
> I'll do this in steps so you don't need to provide everything at once..."
>
>（我正在使用 building-skill-systems 技能，将这个技能仓库作为一个完整的系统来设计。
> 我会分步进行，这样你不需要一次性提供所有信息……）

**背后发生了什么：** 技能的 `SKILL.md` 文件被注入到 Copilot 的上下文中，赋予了它完整的设计方法论——渐进式提问、架构优先思维和占位符规范。

---

## 4. 第二步 — 回答渐进式问题

技能会分小轮次提问。**第一轮**用于确定仓库的整体形态：

| 问题 | 目的 |
|------|------|
| 新建仓库还是重构现有仓库？ | 确定模式 A（新建）或模式 B（重构） |
| 领域或任务方向是什么？ | 影响技能分类体系 |
| 主要用户是谁？ | 影响工作流的设计哲学 |
| 轻量技能包还是完整工作流框架？ | 决定结构规模 |
| 首先支持哪个平台？ | 决定目录布局 |
| 新仓库创建在哪里？ | 获取确切的文件夹路径 |

### 示例回答（来自真实会话）

```
1. 新建仓库
2. Dynamics 365 Finance & Operations 开发
3. 小型开发团队
4. 完整工作流框架
5. 仅 GitHub Copilot CLI
6. C:\Projects\d365-fo-skills
```

第一轮之后，Copilot 会根据你的回答进行**第二轮**有针对性的追问。对于完整框架，通常包括：

- 是否需要 hooks？
- 是否需要 docs/specs/plans？
- 是否需要测试？
- 是否需要审阅者/agent 角色？

### 回答技巧

- 领域描述要具体——"D365 F&O 开发"优于"软件开发"
- 如果不确定规模，选"完整工作流框架"——删减比补充更容易
- 关于平台：如果你只使用 GitHub Copilot CLI，明确说明——目录结构会与 Claude Code 不同
- 始终提供完整的文件夹路径——不要假设会有默认值

---

## 5. 第三步 — 审查架构输出

回答完问题后，Copilot 会输出**技能系统简报**和完整的架构设计，包括：

- **技能系统简报** — 目的、领域、用户、平台、范围、约束
- **目录树** — 要创建的确切文件夹结构
- **目录用途映射** — 每个目录的作用
- **技能矩阵** — 所有技能、角色、触发条件和状态（草稿/占位符）
- **工作流设计** — 主工作流 + 备选分支
- **平台集成矩阵** — 每个平台如何发现和使用技能
- **测试矩阵** — 测什么、何时测、如何测
- **占位符与完整内容映射** — 现在构建什么、以后构建什么

### 推进前的检查项

- [ ] 目录树是否使用了 `.github/skills/`（而非根目录下的 `skills/`）？（Copilot CLI）
- [ ] 如果要求了 hooks，是否包含了 `.github/hooks/`？
- [ ] 是否有引导技能（`using-<system-name>`）？
- [ ] 技能矩阵是否至少包含一个元技能和一个质量技能？
- [ ] 占位符是否清晰标记（而非暗示已完成）？

如果有任何问题，现在就告诉 Copilot——在文件生成之前修正设计要容易得多。

---

## 6. 第四步 — Copilot 生成文件

审批架构后，告诉 Copilot 生成文件：

```
看起来不错。请生成仓库结构和初始文件。
```

Copilot 将会：
1. 在你指定的路径创建目录并初始化 Git 仓库
2. 创建所有 `SKILL.md` 文件并填写初始内容
3. 创建 `README.md`、`CHANGELOG.md`、`.gitignore`、`COPILOT.md`
4. 创建平台集成文件（`.github/copilot-instructions.md`）
5. 在 `tests/` 下创建测试脚手架
6. 创建文档结构
7. 提交初始 commit

### 每个技能的生成内容

每个技能位于自己的目录下，包含一个 `SKILL.md`：

```
.github/skills/<skill-name>/
└── SKILL.md
```

`SKILL.md` 包含 YAML 前置元数据和 Markdown 正文：

```markdown
---
name: <skill-name>
description: 这个技能的作用以及使用时机。
---

# 技能标题

## 使用时机

...

## 流程

...
```

---

## 7. 第五步 — 添加领域内容

骨架生成后，你需要添加真正的领域特定内容。

### 添加技能内容

编辑每个 `SKILL.md`，填写：
- 领域特定的工作流步骤
- 检查清单和质量关卡
- 示例和反模式
- 相关技能的链接

### 添加 Agents

在 `.github/agents/` 中创建 agent 文件，使用 `.agent.md` 扩展名：

```markdown
---
name: my-agent
description: 何时以及如何使用此 agent。
---

# Agent 名称

## 角色

...

## 指令

...
```

### 添加 Hooks

在 `.github/hooks/` 中创建 hooks 配置文件：

```json
{
  "version": 1,
  "hooks": {
    "sessionStart": [
      {
        "type": "command",
        "powershell": "./scripts/session-start.ps1",
        "cwd": ".github/hooks",
        "timeoutSec": 30,
        "comment": "会话开启时的环境检查"
      }
    ]
  }
}
```

在 `.github/hooks/scripts/session-start.ps1` 中创建脚本。所有 hook 脚本**必须**读取标准输入：

```powershell
$ErrorActionPreference = "Stop"
$inputData = [Console]::In.ReadToEnd() | ConvertFrom-Json
# 使用 $inputData.source、$inputData.cwd 等字段
```

### 向技能中添加 PowerShell 脚本

技能可以包含可运行的脚本。将脚本放在技能目录中，与 `SKILL.md` 并列：

```
.github/skills/my-skill/
├── SKILL.md          ← Copilot 的指令
└── my-script.ps1     ← Copilot 可以运行的脚本
```

在 `SKILL.md` 中引用脚本：

```markdown
当需要执行 X 时，从该技能目录运行 `my-script.ps1`。
```

---

## 8. 常见问题与修复

以下四个问题在创建 `d365-fo-skills` 的真实会话中出现过，你可能会遇到相同的问题。

---

### 问题 1：某些问题不支持自由输入

**症状：** Copilot 要求你从固定列表中选择（例如是/否），但你需要给出不同的答案。无法自由输入。

**原因：** `ask_user` 工具有一个 `allow_freeform` 参数。当它设为 `false` 时，用户无法输入自定义答案——只能从提供的选项中选择。

**修复方法：** 如果你发现这种情况，告诉 Copilot：

```
你的问题应该始终允许我输入自定义答案，而不只是从列表中选择。
```

Copilot 会更新其行为。对于仓库维护者，修复方法是确保所有 `ask_user` 调用使用 `allow_freeform: true`（这是默认值）。

**此仓库的状态：** 已修复。`building-skill-systems` SKILL.md 现在包含了明确规则，要求所有问题必须允许自由输入。

---

### 问题 2：从未询问新仓库的文件夹路径

**症状：** Copilot 设计了架构但在错误位置创建了文件，或者在已经开始生成后才询问路径。

**原因：** 最初的技能没有将"新仓库应创建在哪里？"明确列入第一轮问题。Copilot 会跳过这个问题或问得太晚。

**修复方法：** 开始新仓库会话时，提前明确说明路径：

```
我想在 C:\Projects\my-skills 创建一个新的技能仓库。
```

或者，如果 Copilot 已经在没有询问的情况下开始了，说：

```
在生成任何文件之前，请先和我确认确切的文件夹路径。
```

**此仓库的状态：** 已修复。第一阶段的第 6 个问题现在明确要求新建仓库（模式 A）会话提供完整的文件夹路径。

---

### 问题 3：技能和 agents 放在根目录 `skills/` 而非 `.github/skills/`

**症状：** 生成的仓库在根目录下有 `skills/` 和 `agents/` 目录。从仓库目录运行 Copilot CLI 时，无法找到技能或 agents。

**原因：** GitHub Copilot CLI 从 `.github/skills/` 发现技能，从 `.github/agents/` 发现 agents——而非根目录下的同名目录。旧版设计指导（以及某些模板）使用了根目录级别的 `skills/`，这对 Copilot CLI 来说是错误路径。

**修复方法（移动文件）：**

```powershell
# 创建正确的目录
New-Item -ItemType Directory -Path ".github\skills" -Force
New-Item -ItemType Directory -Path ".github\agents" -Force

# 移动技能
Move-Item -Path "skills\*" -Destination ".github\skills\"

# 移动 agents
Move-Item -Path "agents\*" -Destination ".github\agents\"

# 删除旧目录
Remove-Item -Path "skills" -Recurse
Remove-Item -Path "agents" -Recurse
```

然后更新 `copilot-instructions.md` 或技能文件中指向旧路径的所有引用。

**验证：** 移动后，从仓库根目录运行 `copilot`，输入 `/skills list`。你应该能看到技能列表。

**此仓库的状态：** 已修复。第二阶段的"GitHub Copilot CLI 布局"目录树现在正确地显示技能和 agents 位于 `.github/` 内部。

---

### 问题 4：`hooks.json` 格式错误

**症状：** Hooks 不触发。或者 hooks 触发但脚本失败。Copilot CLI 故障排除提示"Hooks are not executing"（hooks 未执行）。

**原因：** 生成的 `hooks.json` 可能使用了不符合 GitHub Copilot CLI 官方规范的旧格式。最常见的错误是将 `"hooks"` 写成**数组**而非**对象**。

**错误格式（旧版指导生成）：**

```json
{
  "version": 1,
  "hooks": [
    {
      "event": "sessionStart",
      "script": "scripts/hooks/session-start.ps1"
    }
  ]
}
```

**正确格式（官方规范）：**

```json
{
  "version": 1,
  "hooks": {
    "sessionStart": [
      {
        "type": "command",
        "powershell": "./scripts/session-start.ps1",
        "cwd": ".github/hooks",
        "timeoutSec": 90,
        "comment": "检查环境并自动启动 VS"
      }
    ]
  }
}
```

**关键差异：**

| | 错误 | 正确 |
|-|------|------|
| `"hooks"` 类型 | 数组 `[...]` | 对象 `{...}` |
| 事件键 | `"event": "sessionStart"` | `"sessionStart": [...]` 作为对象键 |
| 脚本引用 | `"script": "path"` | `"powershell": "path"` 或 `"bash": "path"` |
| 必填 `type` | 缺失 | 必须有 `"type": "command"` |
| 工作目录 | 缺失 | `"cwd": ".github/hooks"` — 必须设置 |
| 超时时间 | 缺失 | `"timeoutSec": 30`（默认值） |

**Hook 脚本标准输入修复：**

所有 hook 脚本通过标准输入接收 JSON 数据。不读取标准输入的脚本会丢失上下文。在每个 hook 脚本顶部添加：

```powershell
$ErrorActionPreference = "Stop"
$inputData = [Console]::In.ReadToEnd() | ConvertFrom-Json
```

然后使用对应 hook 的相关输入字段：

| Hook 事件 | 关键输入字段 |
|-----------|-------------|
| `sessionStart` | `$inputData.source`（`"new"`、`"resume"`、`"startup"`） |
| `userPromptSubmitted` | `$inputData.prompt` |
| `preToolUse` | `$inputData.toolName`、`$inputData.toolArgs`（JSON 字符串） |
| `postToolUse` | `$inputData.toolResult.resultType`、`$inputData.toolResult.textResultForLlm` |
| `errorOccurred` | `$inputData.error.message`、`$inputData.error.name` |

**文件位置修复：**

Hooks 还必须位于 `.github/hooks/`，而非仓库根目录：

```
.github/
└── hooks/
    ├── hooks.json         ← 配置文件在这里
    ├── logs/              ← 添加到 .gitignore
    └── scripts/
        ├── session-start.ps1
        └── ...
```

**添加到 `.gitignore`：**

```gitignore
# Hook 审计日志——仅本地保留
.github/hooks/logs/
```

**验证 JSON 格式：**

```powershell
Get-Content ".github/hooks/hooks.json" | ConvertFrom-Json | ConvertTo-Json
```

如果命令失败，说明存在 JSON 语法错误。在测试之前先修复语法。

**此仓库的状态：** 已修复，适用于 `d365-fo-skills` 和 `building-skill-systems`。第四阶段平台集成部分现在记录了完整的官方 hooks 格式。

---

## 9. 迭代现有仓库

仓库创建后，你将持续添加内容。同一个 `building-skill-systems` 技能支持**模式 B（重构）**。

### 添加新技能

```
我想在现有的技能仓库中为[主题]添加一个新技能。
请使用 /building-skill-systems 技能帮我设计它。
```

或者使用你自己仓库中的元技能（例如 `writing-d365-skills`）。

### 添加新 hooks

1. 在 `.github/hooks/hooks.json` 的相应事件数组中添加新条目
2. 在 `.github/hooks/scripts/` 中创建脚本
3. 在脚本中添加标准输入读取
4. 从仓库根目录运行 `copilot` 进行测试

### 扩展 Agents

1. 在 `.github/agents/` 中创建或编辑 `.agent.md` 文件
2. Agent 文件是包含角色、工具访问权限和指令的 Markdown 文件
3. 使用 `/agent <name>` 或要求 Copilot 使用该 agent 来调用它

### 版本跟踪

始终用描述性消息提交更改：

```bash
git add .
git commit -m "添加 <功能>: <简短描述>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

添加重要内容时更新 `CHANGELOG.md`。

---

## 10. 首次使用前的检查清单

在宣布技能仓库可以使用之前，请验证：

### 结构
- [ ] 技能位于 `.github/skills/<name>/SKILL.md`（不在根目录 `skills/`）
- [ ] Agents 位于 `.github/agents/<name>.agent.md`（不在根目录 `agents/`）
- [ ] Hooks（如有）位于 `.github/hooks/hooks.json`（不在根目录 `hooks.json`）
- [ ] Hook 脚本位于 `.github/hooks/scripts/`
- [ ] `.github/hooks/logs/` 已添加到 `.gitignore`

### Hooks 格式
- [ ] `"hooks"` 是以事件名为键的对象（不是数组）
- [ ] 每个 hook 条目都有 `"type": "command"`
- [ ] 使用了 `"bash"` 和/或 `"powershell"` 键（不是 `"script"`）
- [ ] `"cwd"` 设置为 `".github/hooks"`
- [ ] `"timeoutSec"` 根据脚本预期执行时长适当设置
- [ ] 所有 hook 脚本通过 `[Console]::In.ReadToEnd() | ConvertFrom-Json` 读取标准输入 JSON

### 技能内容
- [ ] 每个 `SKILL.md` 都有有效的 YAML 前置元数据（`name`、`description`）
- [ ] 引导技能（`using-<system-name>`）存在并路由到其他技能
- [ ] `copilot-instructions.md` 引用了正确的路径（`.github/skills/`、`.github/agents/`）

### 测试
- [ ] `tests/skill-triggering/prompts/` 中至少有 3 个触发提示
- [ ] 每个核心技能至少有一个显式请求提示

### 验证
- [ ] 从仓库根目录运行 `copilot`
- [ ] 输入 `/skills list`——所有技能应该显示出来
- [ ] 输入 `/agent`——所有 agents 应该显示出来
- [ ] 开始一个会话并验证 `sessionStart` hook 是否触发（如果已配置）
