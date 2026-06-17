# Harness Agent 相关论文调研 (2025–2026)

> 调研日期: 2026-06-13

## 概述

"Harness Engineering"（Harness 工程）已成为 2026 年 AI Agent 领域最活跃的研究方向之一。核心理念是：**Coding Agent = AI Model(s) + Harness**。Harness 是包裹模型的一切——规则文件、skills、MCP server、hooks、sub-agent、插件、反馈循环。其关键原则是：*每当 agent 犯错，就工程化地修复它，使其不再发生。*

---

## 1. 基础理论与概念定义

### What Makes a Harness a Harness: Necessary and Sufficient Conditions for an Agent Harness
- **arXiv**: `2606.10106` | 2026-06-08
- **作者**: Sanderson Oliveira de Macedo
- **贡献**: 首次给出 "agent harness" 的严格构成性定义，将其与 agent framework、SDK、IDE 插件、eval harness、orchestrator 等概念明确区分。追溯了从 test harness 到现代 LLM agent harness 的谱系，并将定义应用于真实系统（Claude Code、Codex CLI、Aider、Cline、OpenHands、SWE-agent）。

### AI Harness Engineering: A Runtime Substrate for Foundation-Model Software Agents
- **arXiv**: `2605.13357` | 2026-05-13
- **作者**: Hailin Zhong, Shengxin Zhu
- **贡献**: 将 harness 形式化为具有 11 项组件职责的运行时基板（任务规范、上下文选择、工具访问、可观测性、验证、权限、熵审计等）。提出四级成熟度阶梯（H0–H3）和基于 trace 的评估协议。

### Code as Agent Harness: Toward Executable, Verifiable, and Stateful Agent Systems
- **arXiv**: `2605.18747` | 2026-05-18
- **机构**: UIUC、Meta、斯坦福（42 位作者，102 页综述）
- **一作**: 宁徐瑛（Xuying Ning），UIUC CS 博士，2026 Siebel Scholar
- **核心观点**: **代码本身是 Harness 的核心媒介**——可执行、可检查、有状态。从三个层次展开：
  1. **Harness Interface（接口层）**: 代码连接推理、行动和环境建模
  2. **Harness Mechanisms（机制层）**: 规划、记忆、工具使用，通过 Plan-Execute-Verify 循环组织
  3. **Scaling the Harness（多 Agent 扩展层）**: 共享仓库、测试、执行状态作为协作基底
- **GitHub**: [Awesome-Code-as-Agent-Harness-Papers](https://github.com/YennNing/Awesome-Code-as-Agent-Harness-Papers)
- **HuggingFace**: [papers/2605.18747](https://huggingface.co/papers/2605.18747)

---

## 2. 自动化 Harness 生成与自我改进

### The Last Harness You'll Ever Build
- **arXiv**: `2604.21003` | 2026-04-22 (v3: 2026-05-01)
- **作者**: Haebin Seong et al.
- **贡献**: 双层元学习框架
  - **Level 1 — Harness Evolution Loop**: Worker Agent、Evaluator Agent、Evolution Agent 三者迭代优化单一任务的 harness
  - **Level 2 — Meta-Evolution Loop**: 跨多种任务优化进化蓝图本身
  - 目标：在新领域实现零人工 harness 工程

### AutoHarness: Improving LLM Agents by Automatically Synthesizing a Code Harness
- **arXiv**: `2603.03329` | 2026-02-10
- **作者**: Xinghua Lou et al.（Google）
- **贡献**: Gemini-2.5-Flash 通过环境反馈的迭代优化自动合成代码 harness。在 145 个 TextArena 游戏中阻止了所有非法移动；code-policies 在策略合规性上优于 Gemini-2.5-Pro 和 GPT-5.2-High。

### Adaptive Auto-Harness: Sustained Self-Improvement for Agentic System Deployment on Open-Ended Task Streams
- **arXiv**: `2606.01770` | 2026-06-01
- **作者**: Zewen Liu et al.
- **贡献**: 解决静态 harness 在开放式任务流上的脆弱性问题。将 oracle harness 差距分解为 **evolution loss** 和 **adaptation loss**。引入有状态多 agent evolver + harness tree + 求解时路由 + 人工干预钩子。

---

## 3. Harness 诊断、修复与安全

### HarnessFix: From Failed Trajectories to Reliable LLM Agents
- **arXiv**: `2606.06324` | 2026-06-04
- **作者**: Mengzhuo Chen et al.
- **贡献**: 基于 trace 的诊断修复框架。将执行轨迹编译为 Harness-aware Trace Intermediate Representation (HTIR)，将失败归因到具体 harness 层（ETCLOVG: Execution, Tool, Context, Lifecycle, Orchestration, Verification, Governance），生成限定范围的补丁。在 SWE-Bench、Terminal-Bench 2.0、GAIA、AppWorld 上取得 15.2%–50% 的提升。

### Auditing Agent Harness Safety (HarnessAudit)
- **arXiv**: `2605.14271` | 2026-05-14
- **作者**: Chengzhi Liu et al.
- **贡献**: 审计完整执行轨迹（而非仅最终输出）的边界合规性、执行保真度和系统稳定性。引入 HarnessAudit-Bench：8 个领域 210 个任务，嵌入了安全约束。关键发现：任务完成度与安全执行之间存在错位；违规随轨迹长度累积；多 agent 协作扩大了安全风险面。

---

## 4. Harness 架构与可学习组件

### HarnessBridge: Learnable Bidirectional Controller for LLM Agent Harness
- **arXiv**: `2606.12882` | 2026-06-11
- **作者**: Xiaoxuan Wang et al.
- **贡献**: 将 agent-环境接口参数化为可学习的双向投影（observation projection + action projection），通过指令微调训练。在 Terminal-Bench 2.0 和 SWE-bench Verified 上匹配或超越专用 harness，同时减少 token 使用量和轨迹长度，可跨模型尺寸泛化。

### Recursive Agent Harnesses (RAH)
- **arXiv**: `2606.13643` | 2026-06-11
- **作者**: Elias Lumer, Sahil Sen, Kevin Paul, Vamse Kumar Subbiah
- **贡献**: 将 harness 递归形式化——父 agent 生成带有完整文件系统工具、代码执行和规划能力的子 agent harness。在长上下文推理（Oolong-Synthetic，最多 4M token）上评估：GPT-5 71.75% → 81.36%；Claude Sonnet 4.5 达 89.77%。连接了 Anthropic 的动态工作流和递归语言模型（RLM）。

---

## 5. 领域特定 Harness

### AutoMegaKernel: A Statically-Checked Agent Harness for Self-Retargeting Megakernel Synthesis
- **arXiv**: `2606.09682` | 2026-06-08
- **作者**: Jaber Jaber et al.
- **贡献**: 将 HuggingFace Llama 系列模型编译为持久协作 CUDA megakernel，具有静态死锁/竞态自由认证。可 agent 驱动的自动研究循环用于自我改进。已开源。

---

## 主题总结

| 主题 | 代表论文 |
|------|---------|
| **自动化 Harness 生成** | AutoHarness, The Last Harness You'll Ever Build, Adaptive Auto-Harness |
| **Harness 诊断与修复** | HarnessFix, HarnessAudit |
| **形式化与定义** | AI Harness Engineering, What Makes a Harness a Harness |
| **可学习/神经 Harness 组件** | HarnessBridge |
| **递归/多 Agent Harness** | Recursive Agent Harnesses, Adaptive Auto-Harness |
| **安全与审计** | HarnessAudit |
| **领域特定（内核、游戏）** | AutoMegaKernel, AutoHarness |

---

## 关键洞察

2026 年的研究社区正围绕一个共识收敛：**harness（而非仅仅是模型）是决定 agent 可靠性、安全性和能力的关键因素**。OpenAI 在 2026 年初披露的内部 harness 工程实践（100 万+行 agent 生成代码，~1500 个 PR，5 个月内零手动编写代码行）以及 LangChain 仅通过 harness 改进从 52.8% → 66.5%（Terminal-Bench 2.0）的跃升，进一步巩固了这一范式。

---

*数据来源: arXiv, HuggingFace Papers, GitHub*
