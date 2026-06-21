# config-paper.md — 论文/算法类对象配置

> **适用对象**：学术论文、算法、研究综述、研究性博客文章。
> **加载时机**：当 SKILL.md 步骤 0 检测到分析对象为论文或算法时，使用 Read 工具加载本文件。

---

## 一、任务类型分流表

分流后，使用 Read 工具加载对应的 `tasks/task-*.md` 获取该类型的详细执行协议（Round 结构、子 agent 策略、pre-final gate、输出要求）。

| 任务类型 | 触发条件 | 最低执行深度 | config |
|---------|---------|------------|--------|
| 单对象概览 | 用户问"这篇论文/算法是什么/讲什么" | Round 1(A) → Final | `tasks/task-overview.md` |
| 单对象深度分析 | 用户问"创新点/设计决策""每个创新点的机制和证据" | Round 1(A+B+C) → Round 2(逐核心单元) → Final | `tasks/task-innovation.md` |
| 多对象对比分析 | 用户给出 ≥2 篇论文并要求逐一分析或对比 | Round 1(A+B+C) → Round 2(逐对象逐核心单元) → Verification → Final | `tasks/task-multi-compare.md` |
| 机制解释 | 用户问"X 为什么/怎么算/推导原理/公式来源" | 主 agent 直接分析 → Final（简单）；或 Round 1(A) → Final（需定位） | `tasks/task-mechanism.md` |
| 判定/评价 | 用户问"X 是否成立""实验是否支撑 claim""方法好不好" | Round 1(A+B) → Final（简单）；或 + Verification → Final（需外部证据） | `tasks/task-evaluation.md` |
| 领域全景 | 用户问"领域发展/趋势/生态" | Round 1(仓库+综述+最新进展) → Final | `tasks/task-domain.md` |
| 系统设计分析 | 用户问"系统怎么设计/架构/模块关系/数据流" | 主 agent 直接分析 → Final（简单）；或 Round 1(A+B) → Final（评估设计选择） | `tasks/task-system-analysis.md` |
| 混合型 | 多种任务类型的组合 | 取各类型最高要求的并集，在规划摘要中说明组合方式 | 加载所有涉及的 task-*.md |

分流结果必须记录在 research_state.md 顶部，后续每轮 checkpoint 时回查。
主 agent 不得在 checkpoint 时降级执行深度（如从"多对象对比分析"降为"领域全景"），除非用户明确同意。

---

## 二、Round 1 角色映射

| 角色 | 职责范围 |
|------|---------|
| A（结构理解） | 原文主线、章节结构、核心 claim、贡献列表、实验位置、作者声称的创新点 |
| B（对比与定位） | 相关方法缺陷、baseline 对比、同领域已有工作、作者说的创新相对谁成立 |
| C（理论与跨学科依据） | 核心机制的学术来源、跨学科理论支撑、反例边界、失败条件 |

- A 必须存在。B/C 按任务类型要求（参见分流表的最低执行深度）。
- 如果对象数量 ≥4，可以增设 D/E 等辅助角色按对象分组，但 A/B/C 三个证据维度不可省略。
- Round 1 拆分方式必须与分流表确定的最低执行深度一致，不允许用更粗的拆分替代要求的细粒度拆分。

---

## 三、跨领域概念映射——学科建议

根据论文创新点的类型，推荐的映射方向：

| 创新点类型 | 推荐映射领域（≥2） | 典型概念 |
|-----------|-----------------|---------|
| 涉及注意力/记忆/认知 | 认知科学 + 信息论 | 工作记忆容量限制、选择性注意、信息瓶颈 |
| 涉及编码/压缩/表示学习 | 信息论 + 统计学习 | 率失真理论、最小描述长度、变分推断 |
| 涉及反馈/迭代/自适应 | 控制论 + 优化理论 | 反馈环路、Lyapunov 稳定性、收敛性分析 |
| 涉及探索/利用/决策 | 决策理论 + 强化学习 | 信息价值(VOI)、探索-利用权衡、多臂赌机 |
| 涉及泛化/迁移/鲁棒性 | 统计学习理论 + 域适应 | PAC 界、Rademacher 复杂度、分布偏移 |
| 涉及搜索/规划/推理链 | 计算复杂性 + AI 规划 | NP-hard 归约、搜索空间结构、偏序规划 |
| 涉及生成/扩散/流 | 统计物理 + 微分几何 |  Langevin 动力学、流形假设、最优传输 |
| 涉及多智能体/协作/竞争 | 博弈论 + 社会心理学 | Nash 均衡、社会困境、群体决策偏差 |

如果某个创新点确实无法建立跨领域映射（如纯工程优化、纯超参数调优），必须写明"无合适映射"并给出不超过一句话的理由。

---

## 四、优先搜索来源

搜索时按下面优先级查证，不要只依赖泛搜索结果：

1. **通用学术检索和引用追踪**：
   - Google Scholar、Semantic Scholar、OpenAlex：综述、经典高被引论文、后续引用和相近概念。
   - DBLP：核对计算机科学论文的会议、年份、作者和正式出版记录。

2. **经典理论和基础概念**：
   - 优先找综述论文、教材、handbook、survey、tutorial、foundational paper。
   - 哲学和决策理论：Stanford Encyclopedia of Philosophy、Internet Encyclopedia of Philosophy、PhilPapers、PhilSci-Archive。
   - 认知科学：Cognitive Science Society / CogSci proceedings、Cognitive Science、Topics in Cognitive Science、Annual Review of Psychology、Trends in Cognitive Sciences。

3. **机器学习 / LLM / 数学论文**：
   - 预印本：arXiv 的 `cs.LG`、`cs.AI`、`cs.CL`、`stat.ML`、`math.OC` 等分类。
   - 机器学习顶会：NeurIPS proceedings、ICML / AISTATS / UAI 的 PMLR proceedings、ICLR / OpenReview。
   - NLP / LLM：ACL Anthology（ACL、EMNLP、NAACL、COLING、TACL、Computational Linguistics）。
   - AI 综合会议：AAAI、IJCAI。
   - 视觉/机器人/HCI：CVF Open Access、RSS、CoRL、ICRA / IROS、ACM CHI。

4. **认知科学 / 心理学 / 哲学 / 决策理论**：
   - 预印本：PsyArXiv、OSF Preprints、PhilPapers、PhilSci-Archive。
   - 决策/经济学：SSRN、Management Science、Operations Research、Decision Analysis、Journal of Risk and Uncertainty。

5. **浏览器接管检索**：
   - 如果顶会站点、OpenReview、Google Scholar、Semantic Scholar、ACL Anthology、PMLR、CVF 等页面需要交互式搜索、筛选、翻页或查看动态内容，可以使用浏览器接管工具。
   - 浏览器检索结果必须写入当前子任务对应的 `rounds/<轮次>/<子任务名>.md`：记录查询词、来源网站、URL、论文题名、作者、年份、venue、是否正式 proceedings、是否预印本。

6. **证据使用规则**：
   - 优先引用同行评审论文、正式 proceedings、综述、教材和经典理论；预印本只能作为"未同行评审的最新证据"。
   - 每条关键结论都要核对题名、作者、年份、venue / DOI / URL。
   - 不要把博客、百科、课程笔记当作最终证据；它们最多用于定位关键词和参考文献。
   - 如果论文只支持相邻概念，必须写成"基于文献的综合推断"，不能写成"文献直接支持"。

---

## 五、补充 Few-shot 示例

以下示例补充 SKILL.md 中的通用原则，展示论文/算法类对象的典型调研编排。

**例 P1**：用户问"这篇论文在干什么？"
- Round 1 可只派发 A。
  - A：原文结构理解，概括文章主线、章节结构、任务、输入输出、作者 claim。
- Checkpoint：如果问题只是 overview，A 足够就 resolved。
- Final：输出"对象—主线—结构—贡献"，不展开外部对照。

**例 P2**：用户问"这篇论文在干什么？创新点是什么？"
- Round 1 可按"原文理解 + 对照判断"拆。
  - A：原文结构理解，提取主线、claim、贡献列表和实验位置。
  - B：相关方法与 baseline 对比，判断作者说的创新相对谁成立。
- Checkpoint：如果创新点只是 standard 深度，A+B 已能覆盖 claim、对比对象、修改点和主要证据，可以 final；如果某个创新点缺机制或证据，再 promoted 到 Round 2。

**例 P3**：用户问"深入分析每个创新点，机制、证据和边界如何？"
- Round 1 先覆盖互相独立的证据来源。
  - A：原文结构理解。
  - B：相关方法与旧方法缺陷。
  - C：实验 / ablation / benchmark 是否支撑作者 claim。
  - D：理论依据、反例和失败边界（如果机制跨领域或争议较大）。
- Checkpoint：每个创新点都成为 answer unit；逐项判断是否需要进入 Round 2，而不是用总览结论直接替代细项分析。

**例 P4**：用户问"这篇综述是否覆盖完整，有没有遗漏方向？"
- Round 1 按 taxonomy 判断需要的外部参照拆。
  - A：原文 taxonomy 和章节结构。
  - B：该领域经典综述和代表性分类。
  - C：近三年新方向 / 最新论文。
  - D：遗漏方向、反例和边界。
- Checkpoint：只把会影响 taxonomy 判断的遗漏方向 promoted。

**例 P5**：用户问"这篇文章里的 X 机制为什么有效？"
- Round 1：先建立背景和定位。
  - A：原文结构理解，定位 X 在论文中的章节、模块、公式、实验和作者 claim。
  - B：X 的相关理论或近邻方法。
  - C：原文实验、ablation、引用是否直接支持 X。
- Checkpoint：X 本身默认 promoted；其他背景候选只在影响 X 判断时提升。
- Round 2：单独研究 X。
  - D：X 的因果链、替代解释、证据强度、失败边界。
- Final：围绕 X 输出"现象—机制—证据—边界"，不要用 Round 1 背景扫描直接替代 X 分析。

---

## 六、Section 4 补充 Few-shot（子 agent 查证编排）

以下示例展示论文类对象的典型子 agent 查证拆分。

- **问题**：多 agent role separation 是否能降低自我合理化？
  - A：查 LLM / multi-agent / evaluator-generator separation 论文；
  - B：查认知科学中的 confirmation bias、independent judgment、group decision；
  - C：查失败边界和反例：multi-agent 什么时候只是重复偏见。

- **问题**：工具调用为什么可以被看成 value of information？
  - A：查 decision theory / value of information 经典理论；
  - B：查 POMDP / active inference / information gain；
  - C：查 LLM tool-use / tool learning 论文中是否有直接支持。

- **问题**：某个 agent benchmark 的最新研究进展是什么？
  - A：查 arXiv / 预印本；
  - B：查 NeurIPS / ICML / ICLR / ACL / EMNLP proceedings；
  - C：查 GitHub / leaderboard / official docs。

- **问题**：某个机制是否真的被论文实验支持？
  - A：查论文原文和 ablation；
  - B：查后续复现、批评、失败案例；
  - C：核对 claim 和引用是否一致。

---

## 七、跨领域概念映射 Few-shot

如果问题是"AutoResearchClaw 中单 agent LLM 的连续推理是否会因为 autoregressive context-conditioning 产生路径依赖"，可按下面方向建立对应：

- **科学哲学**：证伪主义、Lakatos 研究纲领、Kuhn 范式转换、溯因推理，用于解释失败如何从"终止信号"变成"修正驱动力"。
- **机器学习 / LLM Agent**：Exposure Bias、Snowball Effect、Self-Refine、Reflexion、CRITIC、Process Reward Model，用于解释自回归条件化、误差累积、外部验证和过程级反馈。
- **软件工程**：SBFL、Delta Debugging、FTA、FMEA，用于对应失败定位、失败原因隔离、故障分解和预设失效模式。
- **AI 规划与搜索**：STRIPS / Sense-Plan-Act、HTN、MCTS 回溯、偏序规划，用于对应失败后的重新规划、回溯和替代路径搜索。
- **科学实践**：负面结果价值、可复现性危机、预注册，用于解释失败记录、复现检查和防止事后合理化。

映射表格式参考（在最终笔记中使用）：

| 创新点 | 对应的已有学术概念 | 概念来源领域 | 对应关系（如何映射） | 关键文献 | 支持类型 |
|--------|-----------------|------------|-----------------|---------|---------|
| 自回归上下文条件化 | Exposure Bias / Snowball Effect | 机器学习 | 模型在自身错误输出上继续条件化导致误差放大 | Bengio et al., 2015 | 相邻概念支持 |
| 失败驱动的假说修正 | 证伪主义 | 科学哲学 | 将失败预测转化为理论修正的驱动力 | Popper, 1959/1963 | 概念映射 |
