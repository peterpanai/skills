# Skills

这个仓库汇总了 Claude Code Agent Skills，用于扩展 AI 代理在开发、设计、文档、部署和自动化等场景下的能力。所有技能位于 [.agent/skills/](.agent/skills/) 目录。

## 什么是 Skill

Skill 是一组打包的指令和资源，当用户的任务匹配 skill 的描述时，Claude Code 会自动加载对应的 SKILL.md 来获取领域知识与工作流指引。每个 skill 是一个独立目录，包含 SKILL.md（必需）以及可选的脚本、参考文档、模板等资源。

## 安装

### 方式一：一键安装所有 Skills

克隆本仓库后，在项目根目录运行以下命令即可将 `.agent/skills/` 下的所有 skills 一键安装：

```bash
npx skills add --all
```

### 方式二：手动配置

将本仓库克隆后，在 Claude Code / OpenClaw 的配置中指向技能路径：

```json
{
  "skills": {
    "paths": [".agent/skills"]
  }
}
```

### 方式三：Claude Code 插件安装（Superpowers）

```bash
# 安装 Claude Code CLI
sudo npm install -g @anthropic-ai/claude-code

# 安装 superpowers 插件
claude plugin install superpowers@claude-plugins-official

# 验证安装
claude plugin list
```

插件安装后位于 `~/.claude/plugins/cache/claude-plugins-official/superpowers/`。

本仓库通过 Git Submodule 引入了 superpowers 源码，克隆时需带 `--recursive`：

```bash
git clone --recursive https://github.com/peterpanai/skills.git
# 或已有仓库初始化 submodule
git submodule update --init --recursive
```

## 目录结构

```
.agent/skills/skill-name/
├── SKILL.md            # 必需：YAML frontmatter + markdown 指令
├── LICENSE.txt          # 可选：技能许可证
├── metadata.json         # 可选：版本/组织/摘要元数据（Vercel Labs 风格）
├── AGENTS.md / README.md # 可选：技能说明
├── scripts/              # 确定性/重复性任务的可执行脚本
├── references/           # 按需加载的参考文档
├── assets/               # 产物中使用的文件（模板、图标、字体）
├── templates/            # 代码生成模板
├── examples/             # 示例输入/输出
├── rules/                # Lint 风格规则（Vercel Labs 风格）
└── <语言目录>/            # 如 python/、typescript/、go/

superpowers/              # Git Submodule（obra/superpowers）
├── skills/               # 14 个开发流程 skills
├── hooks/                # 会话启动钩子
└── docs/                 # 文档与计划
```

### SKILL.md 格式

每个 SKILL.md 包含 YAML frontmatter，其中 `name` 和 `description` 为必填字段。description 决定 skill 何时被触发，应明确说明触发条件与适用场景。

```yaml
---
name: skill-name
description: 触发条件与功能描述
license: 可选许可证信息
---
```

### 加载模型

Skills 采用三级渐进式披露：
1. **元数据**（name + description） - 始终在上下文中
2. **SKILL.md 正文** - skill 触发时加载
3. **捆绑资源** - 由 SKILL.md 按需引用加载

SKILL.md 正文保持在 500 行以内；超出时拆分到 `references/` 文件，并在 SKILL.md 中引用。

## 来源

| 仓库 | 链接 |
|------|------|
| Anthropic 官方 Skills | [anthropics/skills](https://github.com/anthropics/skills) |
| Vercel Labs 官方 Agent Skills | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) |
| ClawHub 社区 Skills | [clawhub.com](https://clawhub.com) |
| Darwin Skill（自优化框架） | [alchaincyf/darwin-skill](https://github.com/alchaincyf/darwin-skill) |
| Last30Days Skill（跨平台研究） | [mvanhorn/last30days-skill](https://github.com/mvanhorn/last30days-skill) |
| CrossFrame Skill Suite（中文结构诊断） | [xi-kari/crossframe-skill](https://github.com/xi-kari/crossframe-skill) |
| Humanizer（AI文本去痕迹） | [blader/humanizer](https://github.com/blader/humanizer) |
| Superpowers（开发流程技能框架） | [obra/superpowers](https://github.com/obra/superpowers) |
| MarkItDown（文件转 Markdown） | [microsoft/MarkItDown](https://github.com/microsoft/MarkItDown) |
| Obsidian Skills（Obsidian 笔记技能） | [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) |

## Skills 一览

### Anthropic

| 目录名 | 用途 |
|--------|------|
| `algorithmic-art` | 基于 p5.js 生成算法艺术（可复现种子、交互参数、生成式图形） |
| `brand-guidelines` | 将品牌规范（色彩、字体、视觉风格）应用到输出内容 |
| `canvas-design` | 生成高质量视觉设计产物（如 PNG、PDF 风格化内容） |
| `claude-api` | 构建、调试和优化 Claude API / Anthropic SDK 应用 |
| `doc-coauthoring` | 结构化协作文档编写流程，提升文档共创效率 |
| `docx` | 创建、读取、编辑和处理 Word（`.docx`）文档 |
| `frontend-design` | 构建高质量前端界面与设计实现 |
| `internal-comms` | 生成企业内部沟通内容（公告、通知、汇报类文本） |
| `mcp-builder` | 指导创建高质量 MCP（Model Context Protocol）服务 |
| `pdf` | 读取、生成、编辑与处理 PDF 文档 |
| `pptx` | 创建和处理 PowerPoint（`.pptx`）演示文件 |
| `skill-creator` | 创建、改进和评估新的 skills |
| `slack-gif-creator` | 制作适用于 Slack 的 GIF 动图 |
| `theme-factory` | 为文档、幻灯片、报告等产物快速应用主题风格 |
| `web-artifacts-builder` | 构建复杂的 HTML 交互产物（多组件页面 / Artifact） |
| `webapp-testing` | 基于 Playwright 的本地 Web 应用测试与交互验证 |
| `xlsx` | 创建和处理电子表格（`.xlsx`） |

### Vercel Labs

| 目录名 | 用途 |
|--------|------|
| `composition-patterns` | React 组合式组件设计，避免布尔参数膨胀，提升可复用性 |
| `deploy-to-vercel` | 将项目部署到 Vercel（预览部署、链接项目、团队作用域等） |
| `react-best-practices` | React / Next.js 性能优化（数据获取、渲染、包体积等） |
| `react-native-skills` | React Native / Expo 开发与性能实践 |
| `react-view-transitions` | React 视图过渡动画与路由过渡体验 |
| `vercel-cli-with-tokens` | 通过 Token 鉴权使用 Vercel CLI 进行部署和管理 |
| `web-design-guidelines` | 按 Web 设计规范审查 UI（可访问性、性能、交互体验） |

### Darwin Skill

| 目录名 | 用途 | 来源 |
|--------|------|------|
| `darwin-skill` | Skill 自优化框架--集成 Microsoft Research SkillLens 9 维评分 + SkillOpt 验证机制，自动评估和改进 SKILL.md 质量 | GitHub |

### Community

| 目录名 | 用途 | 来源 |
|--------|------|------|
| `last30day` | 跨平台研究引擎--聚合Reddit/X/YouTube/HN/Polymarket/Web 的最近30天讨论与趋势，生成基于真实参与度（点赞/投票/金额）的综合摘要 | GitHub |
| `crossframe-skill` | 中文结构诊断与成文技能组（12个技能）--处理关系、团队、组织、制度、公共争议、长期演化和思想文章等复杂问题，先做结构诊断再输出判断/答复/案例/文章 | GitHub |
| `humanizer` | AI 文本去痕迹工具--去除 AI 写作的典型特征（多余破折号、表情符号、引导句、过度粗体等），让文本听起来更像人类自然写作 | GitHub |

### Superpowers（Git Submodule）

> 路径：`superpowers/`（Git Submodule，指向 [obra/superpowers](https://github.com/obra/superpowers)）

| 目录名 | 用途 |
|--------|------|
| `brainstorming` | 需求头脑风暴，开发前先搞清楚做什么 |
| `dispatching-parallel-agents` | 并行子代理调度，多个独立任务同时推进 |
| `executing-plans` | 计划执行，按步骤推进开发 |
| `finishing-a-development-branch` | 开发分支收尾，合并前清理 |
| `receiving-code-review` | 接收代码审查，处理反馈 |
| `requesting-code-review` | 请求代码审查，提交前自检 |
| `subagent-driven-development` | 子代理驱动开发，拆分任务给子 agent |
| `systematic-debugging` | 系统化调试，4 阶段定位根因 |
| `test-driven-development` | TDD 测试驱动开发，RED-GREEN-REFACTOR |
| `using-git-worktrees` | Git worktree 使用，并行开发不冲突 |
| `using-superpowers` | Superpowers 引导，自动加载技能 |
| `verification-before-completion` | 完成前验证，确保交付质量 |
| `writing-plans` | 编写计划，结构化任务拆解 |
| `writing-skills` | 编写 Skills，创建新的 Agent 技能 |

**安装方式**：

```bash
# Claude Code 插件安装（推荐）
claude plugin install superpowers@claude-plugins-official

# 或通过本仓库 submodule
git clone --recursive https://github.com/peterpanai/skills.git
```

### Obsidian Skills（Git Submodule）

> 路径：`skills/obsidian-skills/`（Git Submodule，指向 [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills)）

| 目录名 | 用途 |
|--------|------|
| `defuddle` | 从网页提取干净的 Markdown 内容，去除广告和导航，节省 token |
| `json-canvas` | 创建和编辑 JSON Canvas 文件（.canvas），可视化画布、思维导图、流程图 |
| `obsidian-bases` | 创建和编辑 Obsidian Bases（.base 文件），数据库式笔记视图 |
| `obsidian-cli` | 通过 Obsidian CLI 读写笔记、搜索 vault、管理任务和属性 |
| `obsidian-markdown` | 编写 Obsidian 风格 Markdown（wikilinks、callouts、frontmatter、embeds） |

**安装方式**：

```bash
# Claude Code 插件安装
/plugin marketplace add kepano/obsidian-skills
/plugin install obsidian@obsidian-skills

# 或 npx skills
npx skills add https://github.com/kepano/obsidian-skills

# 或通过本仓库 submodule
git clone --recursive https://github.com/peterpanai/skills.git
```

### ClawHub

> 🔥 **写作与内容创作**
> 
> | 目录名 | 用途 |
> |--------|------|
> | `planning-with-files` | 类 Manus 任务规划，用持久化 Markdown 文件作为 AI 的"磁盘工作记忆"，适合复杂多步任务 |
> | `blog-seo-writer` | 技术博客写作+SEO优化，自动优化标题、关键词和内链，提升搜索排名 |
> | `explain-code` | 代码智能解析，将技术代码自动生成为通俗易懂的解释，适合博客配文 |
> | `markdown-converter` | Markdown 转多平台格式（HTML/PDF/微信公众号等），写博客日常必备 |
> | `html-markdown-hybrid` | HTML 与 Markdown 混合排版，支持表格、自定义样式等复杂格式 |
> | `web-scraping-tool-selection-strategy` | 网页数据抓取策略，为写作和研究收集素材 |
> | `beautiful-mermaid` | Mermaid 图表渲染（流程图/架构图/时序图），支持16种主题，导出 SVG/PNG |
>
> 🔧 **开发与部署**
> 
> | 目录名 | 用途 |
> |--------|------|
> | `china-cloud-deploy` | 国内云平台（阿里云/腾讯云等）一键部署，适合国内服务器环境 |
> | `china-seo-baidu` | 百度搜索引擎 SEO 优化，面向国内读者的技术博客必备 |
> | `database-operations` | 数据库操作与 SQL 优化，技术博客涉及数据库内容时生成规范的示例和文档 |

## 更新 Superpowers Submodule

```bash
# 更新本地 submodule 到上游最新
cd superpowers && git pull origin main && cd ..
git add superpowers
git commit -m "chore: update superpowers submodule"

# 更新 Claude Code 插件
claude plugin update superpowers@claude-plugins-official
```

## Tools（Git Submodule）

> 路径：`tools/`，详细说明见 [tools/README.md](tools/README.md)

| 工具 | 来源 | 用途 |
|------|------|------|
| MarkItDown | [microsoft/MarkItDown](https://github.com/microsoft/MarkItDown) | 文件转 Markdown（PDF/DOCX/PPTX/XLSX/HTML/Audio/YouTube） |

## 许可证

本仓库整体采用 MIT 协议，详见 [LICENSE](LICENSE)。各 skill 目录下的 LICENSE.txt 为其各自技能的原许可证。
