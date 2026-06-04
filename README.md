# Skills

这个仓库汇总了 Claude Code Agent Skills，用于扩展 AI 代理在开发、设计、文档、部署和自动化等场景下的能力。所有技能位于 [.agent/skills/](.agent/skills/) 目录。

## 什么是 Skill

Skill 是一组打包的指令和资源，当用户的任务匹配 skill 的描述时，Claude Code 会自动加载对应的 SKILL.md 来获取领域知识与工作流指引。每个 skill 是一个独立目录，包含 SKILL.md（必需）以及可选的脚本、参考文档、模板等资源。

## 安装

将本仓库克隆后，在 Claude Code 的 settings.json 中配置技能路径：

```json
{
  "skills": {
    "paths": [".agent/skills"]
  }
}
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
1. **元数据**（name + description） — 始终在上下文中
2. **SKILL.md 正文** — skill 触发时加载
3. **捆绑资源** — 由 SKILL.md 按需引用加载

SKILL.md 正文保持在 500 行以内；超出时拆分到 `references/` 文件，并在 SKILL.md 中引用。

## 来源

| 仓库 | 链接 |
|------|------|
| Anthropic 官方 Skills | [anthropics/skills](https://github.com/anthropics/skills) |
| Vercel Labs 官方 Agent Skills | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) |

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

## 许可证

本仓库整体采用 MIT 协议，详见 [LICENSE](LICENSE)。各 skill 目录下的 LICENSE.txt 为其各自技能的原许可证。
