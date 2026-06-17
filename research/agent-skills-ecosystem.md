# Agent Skills 生态系统调研 (2025–2026)

> 调研日期: 2026-06-13

## 概述

2025–2026 年，围绕 Claude Code 的 agent skills 生态系统爆发式增长。Skills 作为可移植的 `SKILL.md` 文件（含 YAML frontmatter + 渐进式加载），已成为跨运行时（Claude Code、Codex、Gemini CLI、Antigravity、Cursor、Windsurf）的标准扩展机制。

---

## 1. 生态规模

| 生态组件 | 规模 |
|----------|------|
| alirezarezvani 社区 skills 仓库 | 205+ skills，9 个领域 |
| richfrem/agent-plugins-skills | 11 plugins，137 skills，56 sub-agents |
| wshobson/agents catalog | 182 agents，149 skills |
| agent-harness-kit (npm) | 33 skills，10 review sub-agents |

---

## 2. 核心开源项目

### 2.1 Harness — 元技能 / Agent 团队工厂
- **仓库**: [revfactory/harness](https://github.com/revfactory/harness)
- **定位**: Meta-skill，将领域描述转化为 agent 团队 + skills
- **架构模式**: 6 种——Pipeline, Fan-out/Fan-in, Expert Pool, Producer-Reviewer, Supervisor, Hierarchical Delegation
- **效果**: +60% 质量提升（49.5 → 79.3），15/15 胜率，−32% 方差

### 2.2 Loop Skill Family
- **仓库**: [FUY25/Loop](https://github.com/FUY25/Loop)
- **定位**: 将循环定义为 **trigger + work + verification + memory**，含 6 个专门 skills：
  - `loop-scan` — 从会话历史中发现可重复工作
  - `loop-generate` — 生成完整循环规范
  - `loop-verify` — 将手动验证编码为项目检查
  - `loop-run` — 通过正确机制执行循环（cron, hook, GH Actions）
  - `loop-status` — 维护和标记过期/失败的循环

### 2.3 Claude Self-Improving Skills
- **仓库**: [UniM0cha/claude-self-improving-skills](https://github.com/UniM0cha/claude-self-improving-skills)
- **灵感**: Nous Research 的 Hermes Agent
- **循环**: Detect → Distill → Validate → Rediscover
  - **Detect**: Stop hook 检测复杂工作
  - **Distill**: 子 agent 创建/补丁 `SKILL.md` 文件
  - **Validate**: 验证 hook 检查并回滚坏编辑
  - **Curator**: 30–90 天不活跃后归档过期 skills
  - **Rediscover**: 下次会话自动发现和复用

### 2.4 AceForge — 自进化技能引擎
- **仓库**: [sudokrang/aceforge](https://github.com/sudokrang/aceforge)
- **定位**: 12 阶段流水线
  - Observe → Detect → Generate（双模型 LLM）→ Validate（23 种对抗变异检查）→ Score → Approve（人工把关）→ Deploy → Evolve（SRLR 蒸馏，500/2000/5000 里程碑）→ Retire → Propagate → Compose → Validate
- **特点**: 研究支撑、人工审批、无自动部署

### 2.5 Agent Harness Kit
- **npm**: [agent-harness-kit](https://www.npmjs.com/package/agent-harness-kit)
- **定位**: 生产级套件，含 33 skills、10 review sub-agents、6 语言结构强制执行
- **特性**: 成本护栏、JSON 特性追踪、SQLite 操作状态

### 2.6 spec-driven-tdd — 多 Harness 技能包
- **仓库**: [strelov1/spec-driven-tdd](https://github.com/strelov1/spec-driven-tdd)
- **定位**: 将 OpenSpec 规划与 Superpowers TDD/simplify/review 融合为单一交付循环
- **特性**: npm 安装器，多 harness 融合

### 2.7 claudenv
- **npm**: [claudenv](https://www.npmjs.com/package/claudenv)
- **定位**: 一键项目设置，自动发现和配备 skills 的自扩展 harness

---

## 3. 架构分层

生态系统收敛于以下分层架构：

```
L3 元工厂层 (Meta-Factory)
    ├── Harness (revfactory) — 领域→团队+skills 工厂
    ├── Archon — 多 agent 编排
    └── meta-harness — harness 的 harness
        ↓ 生成
L2 跨 Harness 工作流层
    └── ECC 标准化 — 跨运行时 skill 兼容
        ↓ 标准化
L1 执行层 (Execution)
    ├── Skills (SKILL.md)
    ├── Hooks (生命周期钩子)
    ├── MCP Servers (工具扩展)
    ├── Sub-agents (隔离执行)
    └── Loops (循环/定时任务)
        ↓ 运行于
Runtime 层
    ├── Claude Code
    ├── Codex CLI
    ├── Gemini CLI → Antigravity (2026-06-18 消费者版本关停)
    └── Cursor / Windsurf
```

---

## 4. 关键时间线

| 时间 | 事件 |
|------|------|
| 2026 年初 | OpenAI 发布 harness 工程论文（100 万+行，零手动代码） |
| 2026-02 | Paradime 发布 Claude Code harness 工程综合指南 |
| 2026-03 | LangChain 展示仅靠 harness 改进：52.8% → 66.5% |
| 2026-05 | richfrem v1.4 — MAF 合成、混合运行时、SQLite 控制平面 |
| 2026-05 | Agent Harness Kit 0.22.0 发布 |
| 2026-05 | UIUC/Meta/Stanford "Code as Agent Harness" 102 页综述 |
| 2026-06 | UniM0cha self-improving skills 插件发布 |
| 2026-06 | spec-driven-tdd 多 harness 技能包发布 |
| 2026-06 | Gemini CLI 消费者版宣布关停（6/18），转向 Antigravity |

---

## 5. Skill 文件规范

每个 skill 目录遵循标准结构：

```
skill-name/
├── SKILL.md          # 必需: YAML frontmatter + markdown 指令
├── LICENSE.txt        # 可选: 许可证
├── scripts/           # 确定性/重复性任务的可执行代码
├── references/        # 按需加载到上下文的文档
├── assets/            # 输出中使用的文件（模板、图标、字体）
├── templates/         # 代码生成模板
├── examples/          # 示例输入/输出
└── <语言特定目录>/    # 如 python/, typescript/, go/
```

### SKILL.md Frontmatter 格式

```yaml
---
name: skill-name
description: 触发条件与功能描述（这是主要的触发机制）
license: 可选的许可证信息
---
```

### 渐进式加载模型（三级）

1. **元数据**（name + description）— 始终在上下文中
2. **SKILL.md 正文** — skill 触发时加载
3. **捆绑资源** — 按需从 SKILL.md 引用加载

建议 SKILL.md 正文控制在 500 行以内，超出部分拆分到 `references/` 目录。

---

## 6. 基础设施层

| 组件 | 功能 |
|------|------|
| **Plugin marketplaces** | `marketplace.json` 发现、依赖控制、allowlist |
| **跨运行时支持** | Claude Code, Codex, Gemini CLI, Antigravity, Cursor, Windsurf |
| **Skills 可移植性** | `SKILL.md` 作为标准可移植单元 |
| **claudenv** | 一键项目设置，自扩展 harness |
| **Agent Harness Kit** | 生产级套件，SQLite 操作状态 |

---

## 7. 模式共识

整个生态系统的共识是：

1. **Harness 质量比模型选择更重要** — 对生产结果的影响更大
2. **小、可组合的 skills 是最佳实践** — 链式组合为 plan → do → verify → commit 循环
3. **渐进式披露保持上下文精简** — 仅在需要时加载详细信息
4. **Sub-agents 隔离昂贵工作** — 保护主上下文窗口
5. **闭环自改进是终局** — Detect → Distill → Validate 模式解决 skill 质量衰减

---

## 8. 值得关注的趋势

- **Skills 质量危机**: 56% 的 skills 从未被调用，劣质 skills 导致负面性能
- **自进化 Skills**: EvolveR、SkillRL、Socratic-SWE 展示了 agent 自我改进的可行路径
- **认知对齐**: 自我提炼的 skills 优于外部教师蒸馏的 skills（在 ~3B 参数以上）
- **Harness 即代码**: 代码作为 harness 的核心媒介，贯穿整个执行循环
- **递归 Harness**: RAH 通过父 agent 生成子 agent harness 取得显著收益

---

*数据来源: GitHub, npm, arXiv, 36氪, Paradime, skywork.ai*
