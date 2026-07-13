# Skills 仓库

本目录维护 AI Agent 相关的 skills 集合，供本机各 Agent 框架（Hermes、Claude Code、Codex 等）共享使用。

## 目录结构

```
~/ws/skills/
├── README.md           # 本文件
├── superpowers/        # Superpowers 插件（GitHub 镜像）
└── (更多 skills 待添加)
```

## 已安装的 Skills

### Superpowers

- **来源**：https://github.com/obra/superpowers
- **版本**：v5.1.0（Claude Code 插件） / main 分支（本目录 git clone）
- **本地路径**：`~/ws/skills/superpowers/`
- **Skills 列表**（14 个）：
  - `brainstorming` - 需求头脑风暴
  - `dispatching-parallel-agents` - 并行子代理调度
  - `executing-plans` - 计划执行
  - `finishing-a-development-branch` - 开发分支收尾
  - `receiving-code-review` - 接收代码审查
  - `requesting-code-review` - 请求代码审查
  - `subagent-driven-development` - 子代理驱动开发
  - `systematic-debugging` - 系统化调试
  - `test-driven-development` - TDD 测试驱动开发
  - `using-git-worktrees` - Git worktree 使用
  - `using-superpowers` - Superpowers 引导
  - `verification-before-completion` - 完成前验证
  - `writing-plans` - 编写计划
  - `writing-skills` - 编写 Skills

## 安装方法

### 方式 1：Claude Code 插件安装（推荐）

```bash
# 安装 Claude Code CLI
sudo npm install -g @anthropic-ai/claude-code

# 安装 superpowers 插件
claude plugin install superpowers@claude-plugins-official

# 验证安装
claude plugin list
```

插件安装后位于 `~/.claude/plugins/cache/claude-plugins-official/superpowers/`。

### 方式 2：Git Clone（本目录维护）

```bash
# 克隆到 ~/ws/skills/ 维护一份源码
git clone https://github.com/obra/superpowers.git ~/ws/skills/superpowers

# 更新
cd ~/ws/skills/superpowers && git pull
```

### 方式 3：Hermes Agent 使用

Hermes Agent 的 skills 目录在 `~/.hermes/skills/`。如需在 Hermes 中使用 superpowers 的某个 skill：

```bash
# 复制单个 skill 到 Hermes skills 目录
cp -r ~/ws/skills/superpowers/skills/brainstorming ~/.hermes/skills/

# 或创建符号链接
ln -s ~/ws/skills/superpowers/skills/brainstorming ~/.hermes/skills/brainstorming
```

## 更新

```bash
# 更新本地 git clone
cd ~/ws/skills/superpowers && git pull

# 更新 Claude Code 插件
claude plugin update superpowers@claude-plugins-official
```

## 参考

- Superpowers GitHub: https://github.com/obra/superpowers
- Claude Code 插件市场: https://claude.ai/plugins
- Hermes Agent Skills 文档: https://hermes-agent.nousresearch.com/docs/reference/skills-catalog
