# Loop Agent 相关论文调研 (2025–2026)

> 调研日期: 2026-06-13

## 概述

2025–2026 年，Agent Loop 研究正从 **ad-hoc 迭代式 agent 循环** 向 **结构化、可验证、受治理的架构** 转变。核心趋势包括：DAG 执行图取代简单循环、控制论稳定性预算、符号化治理层、事件溯源审计轨迹。

---

## 1. 基础理论：Agent Loop 的批判与重构

### From Agent Loops to Structured Graphs: A Scheduler-Theoretic Framework for LLM Agent Execution
- **arXiv**: `2604.11378` | 2026-04
- **作者**: Hu Wei
- **核心洞察**: 将主流的 "Agent Loop" 范式定性为 **单一就绪单元调度器**，存在三个结构性弱点：
  1. 隐式依赖（implicit dependencies）
  2. 无界恢复循环（unbounded recovery loops）
  3. 可变执行历史（mutable execution history）
- **解决方案 — SGH (Structured Graph Harness)**: 将控制流从隐式上下文提升为显式静态 DAG，实现三层分离（规划、执行、恢复）。70 系统调研，形式化节点状态机，具有终止保证。

### From Agent Loops to Deterministic Graphs: Execution Lineage for Reproducible AI-Native Work
- **arXiv**: `2605.06365` | 2026-05
- **作者**: Josh Rosen, Seth Rosen
- **核心洞察**: 引入 **execution lineage**——将 AI 原生工作表示为带稳定中间边界的 DAG 产物计算，支持基于身份的复现（identity-based replay）。
- **关键结果**: DAG 复现能精确保持最终 memo，零无关分支污染 vs. 基于循环的基线方法。

### When the Loop Forgets the Why: Recursive AI, Loop Engineering, and the Case for Continuity Architecture
- **Zenodo**: `20621972` | 2026-06
- **核心洞察**: 引入 **递归漂移（recursive drift）** 作为失败模式——循环在局部能力增强的同时，却丧失了对原始任务意义的问责。提出 "continuity architecture" 作为必要的治理层。
- **框架**: Mnemosyne AI Continuity Framework

---

## 2. 长周期 Loop 的稳定性与控制

### AICL: A Control-Loop Architecture for Stable Long-Horizon LLM Agents
- **Zenodo**: `17835680` | 2025-12
- **核心洞察**: 将 agentic 推理形式化为 **闭环过程**，包含四个组件：
  1. 结构化规划（structured planning）
  2. 探针驱动监控（probe-driven monitoring）
  3. 事件驱动编排（event-based orchestration）
  4. 量化稳定性预算（quantitative stability budgets）
- **开源实现**: [CyberLoop](https://github.com/roackb2/cyberloop)（TypeScript）

### Structured Cognitive Loop with a Governance Layer (SCL)
- **arXiv**: `2511.17673` | 2025
- **核心洞察**: SCL 将认知分为五个阶段：Retrieval → Cognition → Control → Action → Memory（R-CCAM）。引入 **Soft Symbolic Control** 作为治理层，对概率推理施加符号约束。
- **关键结果**: 零策略违规，消除冗余工具调用，完整的决策可追溯性。

---

## 3. 递归多 Agent 系统

### Recursive Multi-Agent Systems (RecursiveMAS)
- **arXiv**: `2604.25917` | 2026-04
- **作者**: Xiyuan Yang et al.（12 位作者）
- **核心洞察**: 将递归/隐式循环缩放从单模型扩展到 **多 agent 协作**。将整个多 agent 系统建模为统一的隐空间递归计算，通过轻量级 **RecursiveLink** 模块连接。
- **关键结果**: 9 个基准测试平均准确率提升 **+8.3%**（数学、科学、医学、搜索、代码），推理加速 **1.2×–2.4×**，token 减少 **34.6%–75.6%**。

---

## 4. 自主研究与改进 Loop

### Autonomous Research Loops: An LLM-Agent Framework for End-to-End ML Experimentation, Manuscripting, and Self-Evaluation
- **ACM**: `10.1145/3802133.3802134` | 2026-05
- **作者**: Alicem Koyun（Westcliff University）
- **核心洞察**: 端到端框架，自动化完整的 ML 研究生命周期——假设生成、代码编写、实验执行、手稿准备，以及通过 reviewer agent 进行 **多轮自反思同行评审**。
- **成本**: ~$15/paper；Claude Sonnet 3.5 获得最高质量评分（假设 8.7、代码成功率 87%、手稿 8.9）。

### Regimes: An Auditable, Held-Out-Gated Improvement Loop
- **arXiv**: `2606.10241` | 2026-06
- **作者**: Yohei Nakajima
- **核心洞察**: 展示 **事件溯源 agent 运行时** 将受控改进转化为一流工作流。Regimes 循环诊断失败、在流水线接缝处提出修复、仅通过静态检查、沙箱执行、样本内评估和 **留出验证** 后才升级。
- **发现**: 在 LongMemEval-S 上，主要失败模式不是检索而是 **reconciliation**（证据存在但阅读者回答错误）。

### Agentic Auto-Scheduling: ComPilot — LLM-Guided Loop Optimization
- **arXiv**: `2511.00592` | PACT 2025
- **核心洞察**: 闭环交互——LLM 向编译器提出代码变换，根据合法性/加速反馈迭代优化。**无需微调**。
- **关键结果**: 几何平均加速 **2.66×**（单次运行）和 **3.54×**（best-of-5），可与 SOTA Pluto 多面体优化器竞争。

---

## 5. Self-Improving Skills Loop（自改进技能循环）

2026 年的一个关键研究方向是 **agent 技能库的质量问题**。研究表明 56% 的 agent skills 从未被调用（SkillsBench），劣质 skills 在 84 个基准任务中的 16 个上产生负面性能。解决方案是 **自改进循环**。

### 核心模式: Detect → Distill → Validate

| 阶段 | 描述 |
|------|------|
| **Detect** | 识别 agent 执行轨迹中的模式、差距、失败或成功 |
| **Distill** | 从轨迹中提取结构化的可复用 skills/guardrails/指导 |
| **Validate** | 在部署前验证 skills 的正确性、安全性和实际改进效果 |

### 代表工作

| 项目/论文 | 来源 | 特点 |
|-----------|------|------|
| **SIRI** (2606.02355) | arXiv 2026-06 | discover→validate→distill；ALFWorld 0.908→0.930 |
| **Socratic-SWE** (2606.07412) | arXiv 2026-06 | 从 trace 蒸馏 skills，SWE-bench Verified 50.40% |
| **HERO** (2606.11559) | arXiv 2026-06 | 后见之明增强反思，利用环境观察作为局部对齐反馈 |
| **MMG2Skill** (2606.01993) | arXiv 2026-06 | 从 web guides 蒸馏 → 编译为可执行 skills，+12.8~25.3pp |
| **EvolveR** | ICML 2026 | 从成功/失败轨迹蒸馏 "认知技能"，自我提炼优于外部教师蒸馏 |
| **SkillRL** (2602.08234) | arXiv 2026-02 | 7B 模型通过递归轨迹蒸馏超越 GPT-4o 41% |

---

## 6. 领域特定 Loop 架构

### LoopRAG: A Closed-Loop Multi-Agent RAG Framework
- **MDPI Buildings** | 2026-01
- **核心洞察**: 将 PDCA（Plan–Do–Check–Act）闭环优化机制应用于 RAG，创建四阶段流水线，动态 prompt 重配置和异构知识融合。
- **结果**: 90% 上下文召回率，72% 响应相关性，88% 答案准确率。

---

## 主题总结

| 主题 | 代表工作 |
|------|---------|
| **Agent Loop 批判与重构** | SGH, Execution Lineage, Continuity Architecture |
| **闭环稳定性与治理** | AICL, SCL, CyberLoop |
| **递归多 Agent 缩放** | RecursiveMAS |
| **自主研究循环** | Autonomous Research Loops, Regimes/ActiveGraph |
| **自改进 Skills Loop** | SIRI, Socratic-SWE, EvolveR, SkillRL |
| **领域特定 Loop** | LoopRAG（建筑）, ComPilot（编译器） |

---

## 关键洞察

1. **从 Loop 到 Graph**: 学术界正系统性地批判简单的 agent 循环，主张用显式 DAG + 稳定中间边界取代之
2. **治理层是必要的**: 无论是符号控制、稳定性预算还是 continuity architecture，纯粹的开放式循环无法保证长期可靠性
3. **递归是杠杆**: RecursiveMAS 和 RAH 展示了递归结构在质量和效率上的双重收益
4. **Skills 自进化**: Detect → Distill → Validate 循环是解决 skill 库质量问题的关键路径；认知对齐比教师蒸馏更有效

---

*数据来源: arXiv, ACM DL, Zenodo, MDPI, ICML 2026*
