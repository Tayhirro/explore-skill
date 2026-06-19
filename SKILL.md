---
name: explore-skill
description: 深入研究论文、topic、系统设计或创新点的分析 skill。用于表层理解、问题框定、三阶段多轮调研、第一性原理分析、设计哲学拆解、因果机制分析、子 agent 调研、学术依据查证、笔记化压缩，以及在需要时提炼迁移启发或研究机会。
---

# Skill：论文研究/分析 

## 目标

研究一篇论文、一个 topic、一个系统设计或一个创新点时，让 AI 帮我完成：

1. 表层理解；
2. 第一性原理分析；
3. 创新点拆解；
4. 因果机制分析；
5. 独立子问题调研；
6. 学术依据查证；
7. 笔记化压缩；
8. 迁移到自己的研究方向和系统设计。

核心思路：

> 复杂任务先做问题框定，再做聚焦调研，最后综合验证；
> 主 agent 负责建立结构、抽象问题、设计每轮子任务；
> 子 agent 负责清空上下文、独立调研、查证学术依据；
> 子 agent 只上报证据、缺口和后续候选问题，不创建子子 agent；
> 主 agent 负责 checkpoint、调度下一轮、综合、压缩、改写成自己的研究笔记。

---

## 1. 先做表层理解

收到用户给出的论文、topic、系统名、创新点或研究材料后，先直接解释基本内容，避免一开始过度抽象。

不要把本节写成“推荐提问”返回给用户。用户已经提出研究请求时，AI 应直接按下面维度完成表层理解；如果材料不足，先基于已有信息分析，并明确标出还需要补充什么。

执行维度：

```md
先解释【论文 / topic / 系统名】大概在做什么，不要直接上升到哲学层。

必须回答：

1. 它研究的问题是什么？
2. 它输入什么、输出什么？
3. 它的核心流程是什么？
4. 它和已有方法相比有什么明显不同？
5. 它最重要的模块有哪些？
6. 每个模块在流程中承担什么作用？
```

目的：

> 先建立直观理解，知道系统在做什么、怎么做、模块之间怎么连接。

---

## 2. 做通用深层分析

完成表层理解后，继续分析底层结构。

AI 应直接使用下面的框架分析当前对象。

执行维度：

```md
从通用深层结构分析这个对象，不要停留在模块描述或表面总结。

按下面维度选择性分析；不是每个任务都要填满所有维度。

1. 对象与边界：
   - 当前真正要研究 / 解释 / 判定 / 映射的对象是什么？
   - 它不研究什么？哪些内容只是背景或辅助材料？

2. 核心问题：
   - 它要回答什么问题？
   - 这个问题属于内容理解、机制解释、判定评价、概念映射、方法创新，还是混合型？

3. 依据与可信度：
   - 结论凭什么成立？
   - 依据来自论文原文、实验、ablation、benchmark、理论证明、综述、经典理论、外部工具、复现结果，还是综合推断？
   - 哪些是文献直接支持，哪些只是合理类比或主 agent 的综合判断？

4. 路径 / 机制 / 组织方式：
   - 如果是机制解释：因果链是什么？
   - 如果是综述或文章理解：它如何组织领域、章节、taxonomy、观点和证据？
   - 如果是系统分析：模块之间如何连接，信息如何流动？
   - 如果是判定问题：判定标准如何作用到证据上？
   - 如果是概念映射：源概念如何映射到目标理论？

5. 形式化视角：
   - 是否可以形式化为 MDP、POMDP、Bayesian inference、tree search、bandit、optimization、credit assignment、information gain、constraint satisfaction 等？
   - 如果形式化会造成误导，就明确说明不强行形式化。

6. 方法论 / 认识论 / 设计哲学视角（条件触发）：
   - 只有当问题涉及创新点、机制解释、研究范式、问题定义、证据标准、系统设计取舍或迁移启发时，才启用这一视角。
   - 方法论：它提供了什么分析流程、建模方式、实验组织方式或问题求解路径？
   - 认识论：它把什么视为可观察信息、有效证据、可验证 claim 或可信解释？
   - 设计哲学：它优先保留什么、牺牲什么、把复杂性转移到哪里、如何重新定义问题空间？
   - 如果当前任务只是内容概括、事实解释、来源核对或普通结构梳理，不要强行上升到方法论 / 认识论 / 哲学层。

7. 适用范围与失败边界：
   - 结论在什么条件下成立？
   - 什么反例、数据分布、任务设置、评估标准或理论前提会让它失效？

8. 后续价值（可选）：
   - 只有当任务需要时，才分析它对理解论文、构造系统、设计实验、迁移应用或发现研究机会有什么启发。
```

目的：

> 从“它在说什么”进入“它研究什么、凭什么成立、如何组织推理、在哪里有效、还能迁移到哪里”。

---

## 3. 从用户问题推进到“问题—调研—机制映射”

先由主 agent 判断当前问题更适合什么分析框架，再决定使用什么字段。不要把所有问题都强行归入固定类型，也不要把所有问题都写成“旧方法缺陷 / 本质缺陷”。下面类型只是常见参考；如果用户的问题是混合型，就组合使用相应字段。只有在问题和证据清楚之后，才把它映射到论文中的具体机制。

执行流程：

```md
1. 判断分析框架：
   - 由主 agent 根据用户问题自行判断，不要求输出固定分类名。
   - 常见参考：
     - 创新点 / 方法改进 / 系统对比：例如“这篇文章的创新点是什么”“它改了哪里”“解决了什么痛点”“有什么值得学习和迁移”，使用“旧方法缺陷、本质缺陷、设计哲学、修改点、机制与迁移价值”；如果该创新改变了问题定义、证据标准、建模方式或实验组织方式，再加入方法论 / 认识论视角。
     - 内容 / 结构解释：例如“这篇文章在干什么”“这篇综述讲了什么”“这个系统由哪些模块组成”，使用“研究对象、文章结构、核心问题、模块关系、主线论证”。
       - 这类任务的主对象是原文内容本身，不是相关背景知识。必须先解释文章讲了什么、怎么组织、作者想回答什么问题，再决定是否需要外部调研。
     - 机制解释：例如“为什么 X 会发生”“X 为什么有效”，使用“待解释现象、因果机制、理论依据”。
     - 判定 / 评价：例如“X 是否是 Y”“这个创新点重要吗”“实验是否支撑 claim”，使用“待判定命题、判定标准、支持证据、反例边界、结论强度”。
     - 概念映射：例如“X 能否形式化为 POMDP / VOI / belief update”，使用“源概念、目标理论、映射关系、映射限制”。

2. 选择通用分析字段：
   - 按当前问题选择必要字段，不需要保留无关字段。
   - 对象与边界：当前真正研究 / 解释 / 判定 / 映射的对象是什么？哪些内容不在范围内？
   - 核心问题：当前要回答的是内容理解、机制解释、判定评价、概念映射、方法创新，还是混合问题？
   - 依据与可信度：判定标准、解释框架、证据来源是什么？哪些是直接支持，哪些是综合推断？
   - 路径 / 机制 / 组织方式：文章如何组织论证，系统如何流动信息，机制如何产生效果，概念如何映射？
   - 方法论 / 认识论 / 设计哲学：只在它能解释创新、机制、证据标准、问题定义或迁移价值时使用；不要把普通技术细节拔高成哲学结论。
   - 适用范围与失败边界：结论在什么条件下成立，什么反例或前提变化会让它失效？

3. 拆成需要查证的问题：
   - 拆分不是强制动作。只有当问题本身包含多个独立子命题、多个章节 / 模块、多个证据链，或需要外部查证时，才拆成多个问题。
   - 每个问题必须来自明确缺陷、文章结构 / 模块关系、待解释现象、待判定命题或概念映射关系，不是孤立模块名。
   - 不要在缺少依据时先写确定性结论。

4. 判断每个问题是否需要子 agent 调研：
   - 是否开启子 agent 不取决于问题类型，而取决于是否需要拆分材料或外部证据。
   - 对原文内容 / 结构解释：如果文章很短、结构简单，主 agent 直接解释即可，不要为了使用多 agent 而强行派发子 agent；如果是长论文、综述、多模块系统或跨学科材料，需要启动子 agent 时，必须保留一个贯穿全文的 A 子 agent 负责整体概括，覆盖文章主线、章节 / taxonomy、核心 claim 和整体结构。其余 B/C/D 子 agent 按需启动，可以按章节、模块、观点簇、学科背景、证据链或外部 retrieval 拆分。最终由主 agent 汇总各子 agent 的结构化结果，形成对整篇文章的整体概括和必要补充。
   - 对机制解释、判定 / 评价、概念映射类问题：如果涉及深层机制、学术依据、跨学科理论、外部论文支持、争议判断或反例边界，先按第 4、5 节开启独立子 agent。
   - 子 agent 只查证自己负责的章节、模块、理论依据、证据、适用范围、边界、反例和不确定部分。
   - 子 agent 不能自行创建子子 agent；如果发现需要继续深入的问题，只能写成 follow-up candidate 返回给主 agent。

5. 综合调研结果：
   - 区分“文献直接支持”和“基于文献的综合推断”。
   - 去掉泛泛解释，只保留能支撑内容理解、判定、机制解释或概念映射的结论。

6. 建立分析映射：
   - 文章中的哪个模块、算法、流程或实验设置回应了这个问题？
   - 如果是创新点类问题：该机制如何体现第 2 节的设计哲学？
   - 如果是内容 / 结构解释：该模块 / 章节在整篇文章结构中承担什么作用？
   - 如果是机制解释、判定 / 评价、概念映射类问题：该机制如何支撑解释、判定、评价或理论映射？
   - 如果是机制问题，再说明该机制如何改变系统行为。
   - 结论适用于什么范围？有哪些边界、反例或不确定部分？

7. 处理后续候选问题：
   - 子 agent 返回的 follow-up candidate 不自动等于下一轮任务。
   - 主 agent 必须先 triage：合并重复候选、判断影响范围、决定是否提升为下一轮调研任务。
   - 被提升的问题进入新的 round 或新的问题研究目录；未提升的问题标记为 deferred / dropped，不继续展开。
```

本节输出不是最终笔记，而是主 agent 和子 agent 共享的结构化中间结果。具体可以理解成一组临时字段，例如 `design_philosophy_summary`、`causal_issue_cards`、`subagent_research_tasks`、`evidence_mapping`，用于决定下一步 action、是否派发子 agent、以及最后如何压缩成笔记。

### 3.1 三阶段问题分析与多轮调研

复杂研究任务默认使用三阶段流程，而不是无限递归。该流程吸收 problem framing、Double Diamond、active retrieval、self-reflective RAG、attribution revision 和 verification planning 的思想：先发散建立问题空间，再收敛定义真正要回答的单元，再用证据缺口触发后续检索、验证和修订。子 agent 不能创建子子 agent；每轮都由主 agent 创建任务、读取结果、做 checkpoint。

```md
启动前：User Question Decomposition / 回答焦点表
- 在派发 Round 1 前，主 agent 必须先把用户原始问题拆成最终需要回答的几个回答焦点，并写入 `research.md`。
- 回答焦点不是任务类型标签，而是最终答案必须覆盖的用户问题，例如“论文在干什么”“创新点是什么”“逐个创新点的机制和证据”“某个 claim 是否成立”“有什么可迁移启发”。
- 每个回答焦点都要记录回答深度：overview / standard / deep。
  - overview：用户只要概括或快速理解，Round 1 通常可以满足。
  - standard：用户要解释清楚主要依据和边界，Round 1 后必须过 Evidence Sufficiency Gate。
  - deep：用户明确要求“深入 / 每个 / 逐项 / 多视角 / 证据 / 机制 / 公式原理 / 计算过程 / 各模型 / 各场景 / 是否成立 / 值得学习”，相关 answer units 默认需要 Round 2 聚焦调研，除非 evidence ledger 已证明 Round 1 足够。
- `research.md` 中至少记录：
  - focus_id：
  - 用户原始问法片段：
  - 回答焦点：
  - 回答深度：overview / standard / deep
  - 需要覆盖的 answer units：
  - 最低证据要求：
  - 当前状态：unresolved / resolved / weak / needs_round2 / deferred

Round 1：Orientation / Problem Framing
- 目标：理解对象、背景、旧方法、理论语境和证据来源，让主 agent 建立 problem representation。
- 如果输入是论文、综述或文章，第一个子任务必须是 A：原文结构理解，覆盖文章主线、章节 / taxonomy、核心 claim、关键模块、作者贡献和证据组织方式。
- 其他子任务按证据来源、学科背景、相关工作、理论依据、实验来源或失败边界拆分。
- PDF 解析、全文抽取、通读原文或把公式摘出来，只是材料获取，不等于完成 Round 1。只要用户显式使用本 skill 且问题包含“研究深入 / 详细公式原理 / 各模型 / 各场景 / 逐项解释”等深度信号，必须先建立 `research.md`、回答焦点表和 Round 1 子任务清单，再进入 checkpoint。
- Round 1 可以形成初步答案草稿，但不能只凭“整体上看起来够了”直接结束；如果已经派发子 agent，必须先进入 checkpoint。

Round Checkpoint：每轮结束后的主 agent 收敛动作
- 主 agent 必须读取本轮所有子 agent 输出，更新 `research.md` 中子任务状态。
- 收集所有 `followup_candidates` 到 `tmp/followup_candidates.md`。
- 更新 answer units 状态表。answer unit 是最终答案必须单独回应的对象，可以是创新点、机制、claim、模块、实验、baseline 对比、taxonomy 节点、理论映射、失败边界、研究机会、可迁移设计原则或用户指定的任意概念。
- 更新回答焦点表：把 Round 1 发现的 answer units 映射回启动前拆出的回答焦点，逐项判断每个焦点是否已经能回答、还缺什么、是否需要 Round 2。
- 更新 `tmp/evidence_ledger.md`：为每个 answer unit 记录原文 claim、外部对照、支撑证据、反证 / 边界、证据质量和缺口。
- 每个 answer unit 必须标记 promoted / resolved / weak / deferred / dropped，并写明理由。
- promoted：需要进入下一轮聚焦调研。
- resolved：已满足对应 Evidence Sufficiency Gate，且证据账本中有可审计的直接证据、明确综合推断依据和边界说明，不需要再开子 agent。
- weak：只能确认作者 claim、相邻概念支持或弱证据支持，最终可以回答，但必须降级表述，不能写成强结论。
- deferred：重要但超出当前用户问题或当前预算。
- dropped：不影响最终答案。
- 不能把“没有更多候选问题”当成 resolved；resolved 必须有证据账本中的足够性理由。

Evidence Ledger 记录格式：
- answer_unit_id：
- 回答焦点：
- 回答深度：overview / standard / deep
- 最低证据要求：
- 原文 claim / 用户指定对象：
- 对比对象 / baseline / 旧痛点：
- 具体机制 / 修改点 / 判定标准：
- 支撑证据：来源、实验、ablation、理论、综述或外部资料；
- 反证 / 替代解释 / 失败边界：
- 证据质量：direct / partial / adjacent / speculative；
- 缺口：
- 状态：promoted / resolved / weak / deferred / dropped；
- 状态理由：

Evidence Sufficiency Gate：判断是否足够，不用主 agent 的主观自信
- 先看回答焦点表，再看证据。证据“够不够”必须相对于用户问法和回答深度判断，而不是孤立地套同一个标准。
- 对内容 / 结构理解：至少要有对象、主线、章节 / 模块结构、核心 claim 和原文边界。
- 对普通“创新点是什么 / 贡献是什么”：至少要有原文 claim、对比对象或旧痛点、具体修改点、机制解释、主要支撑证据和边界；缺 baseline、缺机制、缺证据或缺边界时，默认 promoted 或 weak，而不是 resolved。
- 对“深入分析每个创新点 / 每个机制 / 多视角求证 / 是否真的成立 / 值得学习什么”：每个创新点必须成为独立 answer unit，并默认进入 Round 2 做多视角分析，覆盖旧方法对比、机制链、实验或 ablation、理论依据、反例边界和可迁移条件；只有 evidence ledger 明确证明这些维度在 Round 1 已足够时，才能不进入 Round 2。
- 缺组件级 ablation、直接 baseline 对照、反例边界或外部对照，不自动等于失败；它表示该回答焦点的结论强度需要被检查。若用户只是 overview，可降级说明后 resolved；若用户要求 deep，则默认 needs_round2 或 weak，不能强行 resolved。
- 对机制解释：至少要有待解释现象、因果链、关键变量、直接或间接证据、替代解释和失败边界。
- 对公式计算 / 模型推导解释：至少要有公式所在章节或表格、适用信道 / 场景 / 模型、变量定义与单位、物理或统计来源、计算步骤、参数如何获得、公式之间的继承关系、假设条件和失效边界。长 PDF、survey 或多模型材料中，不能只凭主 agent 全文抽取后的公式列表直接 final；必须先把公式族映射成 answer units，并经过 checkpoint。
- 对判定 / 评价：至少要有待判定命题、判定标准、支持证据、反证或缺失证据、结论强度。
- 对概念映射：至少要有源概念、目标理论、可映射部分、不能映射部分和支持类型。
- 如果某个 answer unit 影响最终答案，但证据账本只记录了单来源、作者自述、弱相关文献或未核查引用，不能仅凭主 agent 的总结标记 resolved。
- 如果用户问题涉及创新点、机制、贡献、评价或迁移判断，而 Round 1 后所有关键 answer units 都被标记为 resolved，必须视为 early-stop risk。主 agent 必须先执行 stop-challenge，逐项检查“是否只是作者 claim、是否缺少 baseline / ablation / 反例、是否缺外部对照、是否只是背景材料很充实”，然后才能决定 final、weak 或 promoted。

反早停检查：最终前的停止理由审计
- 当主 agent 准备停止时，先生成 `tmp/stop_challenge.md`，逐条写出“如果这个答案是错的，最可能错在哪里、缺了什么证据、哪个 answer unit 被过早 resolved”。
- 如果存在“缺少单独 ablation / 缺少 baseline / 缺少反例但整体证据足够”这类表述，stop-challenge 必须判断它应当是 resolved、weak，还是需要在 Round 2 补证。
- 如果 stop-challenge 发现新的高影响缺口，必须 promoted 到 Round 2；如果只是 claim 表述或引用支持关系需要核查，放入 Round 3 final check。
- 如果缺口存在但不会改变当前用户问题的答案，可以标记 weak / deferred，并在最终笔记中降级说明。
- Stop-Challenge 不是要求无限调研，而是要求停止理由可审计：停止必须基于证据账本和状态表，不基于“感觉已经够了”。

Checkpoint 1：Problem Representation
- 发生在 Round 1 后。
- 目标：把背景扫描收敛成 problem representation 和 answer units。
- 不允许用 Round 1 的背景材料直接替代最终答案，除非所有关键 answer units 都通过 Evidence Sufficiency Gate，并在 `tmp/evidence_ledger.md` 中有状态理由。

Checkpoint 2：Answer Unit Resolution
- 发生在 Round 2 后。
- 目标：判断每个 promoted answer unit 是否已经有足够证据、边界和结论强度。
- 如果仍缺关键证据且会改变最终答案，继续在 Round 2 内补证；如果关键 claim 需要核查，进入 Round 3 final check；如果证据不足但可以降级回答，标记 weak；如果证据已足够，标记 resolved。

Checkpoint 3：Final Gate
- 发生在 Round 3 或最终输出前。
- 目标：确认没有 pending 子任务、没有未完成的 promoted answer unit、没有未处理的 unsupported claim。
- 只有通过 final gate，才允许写最终笔记。

Round 2：Focused Investigation
- 目标：围绕 promoted answer units 深入调研，不再泛泛补背景。
- 复用第 4、5 节的子 agent 启动和提示词模板。
- 可以按创新点、机制链、claim-evidence、实验 / ablation、taxonomy 分支、理论映射、系统设计选择或迁移原则拆分。
- 子 agent 只回答自己负责的 answer unit，并继续返回证据、缺口、边界和 follow-up candidates。

Round 3：Final Check / Synthesis / Deliver
- 目标：综合、证据分级、必要时做 claim 核查和反早停检查，然后输出最终笔记。
- 区分文献直接支持、综合推断和不确定项。
- 如果存在 promoted 但未完成的 answer unit，不允许最终输出。
- 如果仍有关键 claim 缺少证据，优先降级为 weak；只有会改变最终答案的高影响缺口，才回到 Round 2 做一次定向补证。
- 最终笔记按用户原始提问的回答焦点组织，不按 Round 1 / Round 2 / 子 agent 输出顺序组织。
- 如果任务包含创新点、机制、贡献、评价或迁移判断，最终前必须执行反早停检查；可以由主 agent 写 `tmp/stop_challenge.md`，也可以派独立 Round 3 子 agent 检查。

轮次收敛限制：
- Round 1 可以发散，只负责建立问题空间和候选 answer units。
- Round 2 只能处理 promoted answer units，不再新增大背景或新主题；新发现问题只有影响最终答案时才允许进入本轮补证。
- Round 3 不再做大范围调研，只做 evidence ledger、claim 核查、反早停检查、降级和最终综合。
- 如果 Round 3 发现高影响缺口，最多回退到 Round 2 做一次定向补证；补证后必须重新进入 Round 3，不能无限循环。
```

`followup_candidates` 只是 checkpoint 的输入，不是自动递归机制。主 agent 不因为候选问题存在就无限展开；它只提升会改变最终答案质量、证据强度、边界判断或迁移结论的 answer unit。

派生问题的文件结构默认保持扁平：所有问题目录都放在同一个 `tmp/` 下，不强制创建多层嵌套目录。用 `research.md` 中的 `parent_issue`、`derived_from` 和 `source_subagent_output` 记录依赖关系，避免路径和上下文管理复杂化。

三阶段 few-shot：

```md
派发编队原则：
- 先把用户问题落到回答焦点表，再根据每个焦点的深度、独立证据链和材料复杂度组织子任务。
- 每个子 agent 应承担一个清楚的判断维度，例如原文理解、方法对照、实验核查、理论边界或迁移风险；没有独立判断价值的方向不单独派发。
- 论文 / 综述 / 文章类任务需要派发时，优先建立“原文结构理解”；外部对照、实验支撑、反例边界、迁移判断等方向按问题需要加入。
- Checkpoint 用 answer units 和 evidence ledger 判断下一轮，不用背景调研替代目标问题本身的分析。

例 0：用户问“这篇短文章大概讲什么？”
- 启动前回答焦点表：
  - F1：文章大意，depth=overview。
- 主 agent 直接完成表层理解；如果读完发现文章很长、结构复杂或用户追加深度要求，再补建 Round 1。

例 0a：用户问“一篇论文在干什么？”
- 启动前回答焦点表：
  - F1：论文在做什么，depth=overview。
- Round 1 可只派发 A。
  - A：原文结构理解，概括文章主线、章节结构、任务、输入输出、作者 claim。
- Checkpoint：如果 A 已满足 F1 的最低证据要求，直接 resolved。
- Final：输出“对象—主线—结构—贡献”，不展开外部对照。

例 0b：用户问“这篇论文在干什么？创新点是什么？”
- 启动前回答焦点表：
  - F1：论文在做什么，depth=overview。
  - F2：创新点是什么，depth=standard。
- Round 1 可按“原文理解 + 对照判断”拆。
  - A：原文结构理解，提取主线、claim、贡献列表和实验位置。
  - B：相关方法与 baseline 对比，判断作者说的创新相对谁成立。
- Checkpoint：如果 F2 只是 standard，A+B 已能覆盖 claim、对比对象、修改点和主要证据，可以 resolved；若某个创新点缺机制、证据或边界，再 promoted 到 Round 2。

例 0c：用户问“深入分析每个创新点，机制、证据和边界如何？”
- 启动前回答焦点表：
  - F1：论文在做什么，depth=overview。
  - F2：每个创新点的机制、证据和边界，depth=deep。
- Round 1 先覆盖互相独立的证据来源。
  - A：原文结构理解。
  - B：相关方法与旧方法缺陷。
  - C：实验 / ablation / benchmark 是否支撑作者 claim。
  - D：理论依据、反例和失败边界（如果机制跨领域或争议较大）。
- Checkpoint：每个创新点都成为 answer unit；逐项判断是否需要进入 Round 2，而不是用总览结论直接替代细项分析。

例 0d：用户问“这篇综述是否覆盖完整，有没有遗漏方向？”
- 启动前回答焦点表：
  - F1：综述 taxonomy 如何组织，depth=overview。
  - F2：覆盖是否完整，depth=deep。
  - F3：遗漏方向与研究机会，depth=deep。
- Round 1 按 taxonomy 判断需要的外部参照拆。
  - A：原文 taxonomy 和章节结构。
  - B：该领域经典综述和代表性分类。
  - C：近三年新方向 / 最新论文。
  - D：遗漏方向、反例和边界。
- Checkpoint：只把会影响 F2/F3 判断的遗漏方向 promoted。

例 0e：用户问“这个系统设计能迁移到我的项目吗？”
- 启动前回答焦点表：
  - F1：原系统如何设计，depth=overview。
  - F2：是否可迁移，depth=standard 或 deep，由用户是否要求深入决定。
- 如果项目背景已经清楚，Round 1 可按“原系统理解 + 迁移判断”拆：
  - A：原系统结构、模块、数据流、约束。
  - B：迁移条件、风险、维护成本和失败边界。
- 如果用户项目背景不清楚，先列假设或请求必要约束，再决定是否需要外部对照。

例 1：用户问“这篇论文每个创新点是什么、解决什么痛点、机制和证据如何？”
- 启动前回答焦点表：
  - F1：论文在做什么，depth=overview，最低证据要求=原文主线、模块、任务和输出。
  - F2：每个创新点是什么，depth=deep，最低证据要求=每个创新点的 claim、旧痛点、修改点、机制、证据、边界。
  - F3：创新点是否值得学习 / 可迁移，depth=deep，最低证据要求=适用条件、风险和可迁移原则。
- Round 1 Orientation：
  - A：原文结构理解，提取主线、claim、贡献列表和实验位置。
  - B：相关方法与旧方法缺陷，查 baseline 和已有方法。
  - C：理论依据与外部证据，查机制来源和反例边界。
- Checkpoint：抽取 I1/I2/I3 等创新点作为 answer units，并映射到 F2/F3。F1 通常可 resolved；F2/F3 因为 depth=deep，默认把 I1/I2/I3 promoted 到 Round 2，除非 evidence ledger 已逐项证明足够。
- 回答焦点：文章在做什么；创新点是什么；创新点的证据和边界。
- Round 2 Focused Investigation：
  - D：I1 的旧方法痛点、修改点、机制、实验支持和边界。
  - E：I2 的旧方法痛点、修改点、机制、实验支持和边界。
  - F：I3 的旧方法痛点、修改点、机制、实验支持和边界。
- Round 3：证据分级后写最终创新点笔记。

例 1b：用户问“这篇论文在干什么？创新点是什么？”
- 启动前回答焦点表：
  - F1：论文在做什么，depth=overview。
  - F2：创新点是什么，depth=standard。
- Round 1 Orientation 后：
  - 如果原文结构、作者 claim、主要 baseline 对比和核心证据已经清楚，F1 和 F2 可以 resolved，并在 evidence ledger 中说明证据强度。
  - 如果某个创新点只有作者 claim、缺少 baseline / 机制 / 证据 / 边界，标记 weak 或 promoted 到 Round 2。
  - 不要因为 F1 已经 resolved，就默认 F2 resolved。

例 2：用户问“这篇文章里的 X 机制为什么有效？”
- Round 1 Orientation：
  - A：原文结构理解，定位 X 的章节、模块、公式、实验和作者 claim。
  - B：X 的相关理论或近邻方法。
  - C：原文实验、ablation、引用是否直接支持 X。
- Checkpoint：X 是主要 answer unit；背景候选只有影响 X 判断时才 promoted。
- 回答焦点：X 是什么；X 为什么有效；证据和边界是什么。
- Round 2 Focused Investigation：
  - D：X 的因果链、替代解释、证据强度、失败边界。
- Round 3：围绕 X 输出“现象—机制—证据—边界”，不要用 Round 1 背景扫描替代 X 分析。

例 2b：用户上传长 PDF / survey，问“各信道、各场景和各模型下的某个指标怎么计算？研究深入，解释详细公式原理。”
- 启动前回答焦点表：
  - F1：论文如何组织这些信道 / 场景 / 模型，depth=overview。
  - F2：每一类公式如何计算，depth=deep。
  - F3：公式背后的物理、统计或建模原理是什么，depth=deep。
- Round 1 Orientation：
  - A：原文结构与公式定位，按章节 / taxonomy 提取所有相关公式、表格和模型名称。
  - B：基础物理模型和确定性模型，解释几何、能量守恒、路径损耗、反射或传播项的来源。
  - C：统计 / 经验 / 测量拟合模型，解释参数如何估计、随机变量代表什么、适用场景是什么。
  - D：特殊场景或扩展模型，例如遮挡、天气、移动性、RIS / ISAC / 多径等，只在原文确实覆盖时派发。
- Checkpoint：把公式族建成 answer units，例如 LOS 公式、NLOS / 多径公式、统计拟合公式、特殊环境修正公式；每个公式族都要记录变量、单位、假设、计算步骤、参数来源和边界。
- Round 2 Focused Investigation：对仍不清楚的公式族逐项补证，不要用“已读完整篇 PDF”替代公式级解释。
- Round 3：做公式一致性检查，确认最终笔记按“场景 / 模型 -> 公式 -> 变量 -> 原理 -> 适用范围”组织，而不是按 PDF 页码堆公式。

例 3：用户问“这篇综述的 taxonomy 是否合理，有哪些可学习的研究机会？”
- Round 1 Orientation：
  - A：原文结构理解，提取 taxonomy、章节主线、覆盖范围。
  - B：关键子领域或代表论文检索。
  - C：taxonomy 外部对照，查其他综述或经典分类。
- Checkpoint：抽取争议分类、遗漏方向和研究机会作为 answer units。
- 回答焦点：taxonomy 如何组织；它是否合理；可以学习什么研究机会。
- Round 2 Focused Investigation：按 promoted answer units 补查遗漏领域、反例或替代 taxonomy。
- Round 3：输出 taxonomy 结构、合理性、边界和研究机会。

例 4：用户问“这个系统设计有什么值得迁移到我的项目？”
- Round 1 Orientation：
  - A：原系统结构理解，模块、数据流、约束和目标。
  - B：相近系统或工程模式对照。
  - C：失败边界、成本、维护和可扩展性依据。
- Checkpoint：抽取可迁移设计原则、不可迁移前提和风险点作为 answer units。
- 回答焦点：系统如何设计；哪些原则可迁移；适用条件和风险是什么。
- Round 2 Focused Investigation：只研究会影响迁移判断的原则或风险。
- Round 3：输出“可迁移原则—适用条件—改造方式—风险边界”。
```

当任务需要派发子 agent、跨多轮继续、材料较长或需要避免上下文丢失时，主 agent 应在当前研究对象所在目录或本次笔记输出目录下新建 `tmp/` 文件夹，把这些字段保存为中间草稿文件，例如 `tmp/research_state.md`、`tmp/evidence_mapping.md`、`tmp/evidence_ledger.md`、`tmp/followup_candidates.md`、`tmp/stop_challenge.md`。

每个需要子 agent 独立调研的 answer unit / 子问题，都必须在 `tmp/` 下单独建立或复用一个问题研究目录，不能只把结果写进最终笔记。例如：

```text
tmp/
  research_state.md
  evidence_mapping.md
  evidence_ledger.md
  followup_candidates.md
  stop_challenge.md
  01-单agent自我合理化问题研究/
    research.md
    rounds/
      round1/
        A-文章主线与结构概括.md
        B-机器学习与LLM论文.md
        C-认知科学与心理学.md
        D-哲学与决策理论.md
      round2/
        E-核心answer-unit机制分析.md
        F-失败边界补证.md
      round3/
        claim-check.md
        stop-challenge.md
  02-工具调用value-of-information问题研究/
    research.md
```

目录名使用“编号 + 问题短名 + 问题研究”，例如 `tmp/01-单agent自我合理化问题研究/research.md`。每个问题目录只维护一个主 agent 工作文件 `research.md`，它只合并原本会拆开的 `task.md` 和 `subagent_tasks.md`：包括问题背景、任务边界、需要查证 / 不需要查证的范围、round1 / round2 / round3 的子任务清单和状态。不要再额外生成问题级 `task.md`、`subagent_tasks.md`。

如果某个问题由上一轮子 agent 的 `followup_candidates` 派生而来，在该问题的 `research.md` 顶部记录：

```md
parent_issue:
derived_from:
source_subagent_output:
promotion_reason:
status:
```

子 agent 只读取 `research.md` 中分配给自己的任务小节，并把调研结果写到对应轮次目录，例如 `rounds/round1/A-机器学习与LLM论文.md`。各轮结果不要写进 `research.md`；`research.md` 只负责记录任务拆分和状态。主 agent 最终交付时，要读取 `research.md` 和 `rounds/` 下的结果文件，把有价值的部分合并进第 7 节的最终笔记格式，并清楚标明哪些内容来自文献直接支持、哪些是综合推断。

只有在用户需要看到阶段性结果时，才把它整理成“阶段性分析摘要”输出。不要额外生成“问题卡 / 因果问题卡 / 问题研究卡”；这些结构容易和子 agent 提示词模板重复。

如果需要派发子 agent，主 agent 只需要在对应问题目录下维护一个 `research.md`，但必须在这个文件里写清楚 sub tasks：

1. 在 `research.md` 的“任务边界”小节写总体背景、主问题、需要查证和不需要查证的范围。
2. 在 `research.md` 的“User Question Decomposition / 回答焦点表”小节拆解用户原始问题，记录每个回答焦点、回答深度、最低证据要求和当前状态。
3. 在 `research.md` 的“Subtasks / 子任务清单”小节列出每一轮、每个子任务、负责方向、状态和输出文件路径。
4. 在 `research.md` 的“Round 1：Orientation 子任务”小节列出每个子 agent 的任务和范围。
5. 如果输入是论文、综述或文章，Round 1 的第一个子任务必须是“原文结构理解”，用于概括文章主线、章节 / taxonomy 和作者 claim。
6. 如果进入后续轮次，继续在同一个 `research.md` 里追加“Round 2：Focused 子任务”或“Round 3：Final Check 子任务”清单；补证、引用核查和反早停检查都写在对应轮次下，不单独创建新轮次。
7. 在 `research.md` 的“Answer Units / 回答单元状态”小节记录每个 answer unit 的状态：promoted / resolved / weak / deferred / dropped，并链接到对应回答焦点和 `tmp/evidence_ledger.md` 中的证据账本。
8. 在 `research.md` 的“Follow-up Candidates / 后续候选问题状态”小节记录每个候选问题的状态：promoted / resolved / weak / deferred / dropped。

`research.md` 是该问题的唯一主 agent 工作文件；它保存任务和 subtasks，不保存各轮调研结果。子 agent 不再新建独立任务文件或新的问题研究目录，只把结果写入 `rounds/<轮次>/<子任务名>.md`。

输出结构：

```md
## 分析框架与通用骨架摘要

- **分析框架：**
  - 主 agent 自行判断；常见参考包括：创新点 / 改进、内容 / 结构解释、机制解释、判定 / 评价、概念映射、混合型。

- **回答焦点表：**
  - 按用户原始问题拆出最终必须回答的焦点。
  - 每个焦点记录回答深度、最低证据要求、支撑 answer units 和当前状态。

- **通用分析字段：**
  - 按当前问题选择必要字段；不要保留无关字段。
  - 常见字段：对象与边界；核心问题；依据与可信度；路径 / 机制 / 组织方式；形式化视角；适用范围与失败边界；后续价值（可选）。

- **核心判断：**
  - ...
```

子 agent 任务写法：

```md
## 子 agent 任务：<角色>-<方向短名>

- 轮次：round1 / round2 / round3
- 角色：orientation / focused-investigation / final-check
- 独立问题：
- 为什么需要单独查证：
- 需要查：
- 不需要查：
- 证据要求：
- 工作文件：`tmp/<问题研究目录>/research.md`
- 任务位置：`research.md` 中对应的 `Round X / <角色>-<方向短名>` 小节
- 输出位置：`tmp/<问题研究目录>/rounds/<轮次>/<角色>-<方向短名>.md`
```

目的：

> 把“创新点、解释问题、判定问题或概念映射问题”变成可查证的问题拆解，先查证关键依据，再落到机制映射。

---

## 4. 每一轮复用的子 agent 启动协议

第 3.1 节决定当前处于 Round 1、Round 2 还是 Round 3。本节只负责每一轮如何开启一个或多个子 agent。不要把子 agent 启动协议单独理解成新的研究流程；它是三轮流程里的执行层。补证、引用核查和反早停检查只是轮内动作，不再作为独立轮次或独立目录。

子 agent 提示词采用“通用模板 + Round Overlay”。通用模板规定独立调研、证据要求、落盘位置和输出结构；Round Overlay 只说明本轮目标和禁止事项。不要为每一轮维护完全不同的长提示词。

适合开启子 agent 的情况：

```md
1. 上下文污染：当前解释可能受到上下文污染。
2. 学术依据：某个 answer unit 需要证明设计有效性、机制有效性、概念映射或证据强度，需要引用论文、综述或经典理论。
3. 涉及跨学科内容：哲学、认知科学、计算神经科学、决策理论、POMDP、active inference 等跨学科内容。
4. 需要区分“文献直接支持”和“综合推断”。
5. 一个 round 中存在多个相互独立、可互相校验的证据链。
6. 主 agent 准备停止，但关键 answer unit 只由单一来源、作者自述、弱相关文献或未核查引用支撑，需要 Round 3 独立检查。
```

如果同一个 round 涉及多个求证方向，不要强行交给一个子 agent 完成。按“能产生相互独立证据链”的原则拆成多个同级子 agent：

```md
Round 1：Orientation
- 目标：广泛覆盖不同领域和来源，先把可能相关的原文结构、理论、论文、证据、反例找出来。
- 启动方式：主 agent 按问题、来源或学科创建若干同级 orientation 任务；子 agent 只执行 `research.md` 中分配给自己的任务小节。
- 拆分决策：
  - 主 agent 不预设固定拆分方式，而是按“哪种拆分能产生相互独立、可互相校验的证据链”来决定。
  - 如果输入是论文、综述或文章，Round 1 的第一个子任务必须是“原文结构理解”：先概括原文本身讲什么、章节 / taxonomy 如何组织、作者 claim 是什么、哪些内容只是背景、哪些 claim 需要外部查证。
  - 原文结构理解不做外部概念扩展，不把任务改写成“查某个相关领域是什么”；外部背景知识只能作为其他 Round 1 子任务或 Round 2 证据缺口。
  - 默认按证据链 / 问题方向拆，例如理论依据、机器学习论文、认知科学机制、失败边界。
  - 如果任务重点是找最新论文、确认 venue、核对顶会录用或追踪最新进展，优先按检索来源拆。
  - 如果已有初步证据但某一块薄弱，优先把它标成 answer unit 或 evidence gap，等待 Round 2 定向处理。
  - 每个子 agent 的文件名必须体现拆分依据，例如 `A-LLM工具调用论文.md`、`B-value-of-information经典理论.md`、`C-失败边界与反例.md`。
- Few-shot：
  - 问题：多 agent role separation 是否能降低自我合理化？
    - A：查 LLM / multi-agent / evaluator-generator separation 论文；
    - B：查认知科学中的 confirmation bias、independent judgment、group decision；
    - C：查失败边界和反例：multi-agent 什么时候只是重复偏见。
  - 问题：工具调用为什么可以被看成 value of information？
    - A：查 decision theory / value of information 经典理论；
    - B：查 POMDP / active inference / information gain；
    - C：查 LLM tool-use / tool learning 论文中是否有直接支持。
  - 问题：某个 agent benchmark 的最新研究进展是什么？
    - A：查 arXiv / 预印本；
    - B：查 NeurIPS / ICML / ICLR / ACL / EMNLP proceedings；
    - C：查 GitHub / leaderboard / official docs。
  - 问题：一篇 O-ISAC 综述文章在讲什么？
    - A：原文结构理解，概括文章主线、章节结构、taxonomy 和作者 claim；
    - B：查作者使用的关键背景概念；
    - C：查论文中重点机制或技术路线的外部依据。
  - 问题：某个机制是否真的被论文实验支持？
    - A：查论文原文和 ablation；
    - B：查后续复现、批评、失败案例；
    - C：核对 claim 和引用是否一致。
- 输出：每个 Round 1 子 agent 只返回本领域的来源列表、关键证据、初步 takeaway、证据不足点和候选 answer units。

Round 1 后 checkpoint：主 agent Triangulation
- 目标：合并 Round 1 结果，判断哪些结论被多个独立来源支持，哪些只是单来源或类比推断。
- 动作：
  - 去重来源；
  - 合并相同证据；
  - 记录冲突结论；
  - 标出证据缺口；
  - 形成 Round 2 只允许处理的 promoted answer units。

Round 2：Focused Investigation
- 目标：只针对 checkpoint 提升的 answer units 或证据缺口补查，不重复覆盖已有结论。
- 启动方式：主 agent 根据 promoted answer units 创建一个或多个 focused 任务；补证也写成 Round 2 任务，不单独开补证轮次。
- 输出：每个 Round 2 子 agent 必须说明 answer unit 是否 resolved / weak / deferred / dropped；如果无法补上直接证据，要明确写“未找到直接证据”，不能用弱相关材料硬补。
- 收敛限制：Round 2 不允许新增大背景、新领域综述或与用户问题无关的新 answer unit；新发现问题只有会改变最终答案质量时才继续处理。

Round 3：Final Check
- 目标：检查关键 claim 是否真的被引用来源支持，并防止主 agent 因为 Round 1/2 材料充实而过早停止。
- 启动方式：主 agent 抽取最重要的 5-10 条 claim 和所有关键 answer units，派一个或多个 Round 3 子 agent 做 evidence sufficiency、claim 核查或反早停检查。
- 独立性要求：Round 3 子 agent 只拿 answer units、证据账本、关键 claim 和用户原始问题，不拿主 agent 的完整叙事段落，避免顺着主 agent 的解释继续合理化。
- 输出：对 claim 给出 SUPPORTED / PARTIAL / UNSUPPORTED；对 answer unit 给出 resolved / weak / needs round2 / deferred / dropped，并说明是否需要改写、降级为综合推断、标为 weak 或删除。
- 收敛限制：Round 3 不再大范围调研；如果发现高影响缺口，最多回退到 Round 2 做一次定向补证。
```

多轮子 agent 查证时，主 agent 必须为每个子 agent 指定清楚的轮次、角色、查证范围、排除范围和落盘目录。目录放在同一个问题研究目录下，例如：

```text
tmp/01-单agent自我合理化问题研究/
  research.md
  rounds/
    round1/
      A-机器学习与LLM论文.md
      B-认知科学与心理学.md
      C-哲学与决策理论.md
      D-经典理论与综述.md
    round2/
      E-answer-unit机制分析.md
      F-失败边界补证.md
      G-直接实验支持补证.md
    round3/
      claim-check.md
      stop-challenge.md
```

每个子 agent 只能回答自己负责的证据链，不要替其他子 agent 综合结论，也不要跳到下一轮任务。如果发现范围外但重要的问题，只能写成 `followup_candidates`。主 agent 在第 6 节统一读取这些结果，合并重复证据，处理相互冲突的结论，并标明哪些结论是直接支持、哪些只是综合推断。

子 agent 的目标：

> 独立查证，不接着当前笔记继续补全文本。

---

## 5. 子 agent 调研提示词模板

主 agent 每次派发子 agent 时，使用“通用要求 + 背景 + Round Overlay + 落盘位置 + 调研任务 + 输出结构”生成提示词。不要把整个提示词模板包进一个大代码块；否则模板内部标题会破坏本文档的大纲层级。

### 5.1 通用要求

- 不要依赖任何先前对话结论；
- 必须基于学术知识、论文、综述和可靠资料；
- 给出引用来源；
- 区分“文献直接支持”和“基于文献的综合推断”；
- 不要编造论文或概念；
- 明确自己属于哪一轮：`round1` / `round2` / `round3`；
- 必须遵守下面的 `Round Overlay / 本轮重点`；
- 调研完成后，把结果写入对应的 `tmp/【编号-问题短名】问题研究/rounds/【轮次】/` 目录。
- 不要创建子子 agent，不要新建问题研究目录，不要擅自进入下一轮调研。
- 如果发现当前任务之外仍需深入的问题，只能在输出末尾写入 `Follow-up Candidates / 后续候选问题`；如果没有，明确写“无”。
- 如果角色是 Round 3 final-check，只能根据主 agent 提供的 answer units、证据账本、关键 claim 和原始问题判断，不要接收或续写主 agent 的完整最终答案草稿。

### 5.2 背景

- 我正在研究：【topic / paper / system】
- 当前关注的任务是：【写清楚背景任务：可以是文章 / 综述在干什么、某个 claim 是否成立、某个机制为什么有效、某个概念能否映射，或某类来源检索】

### 5.3 Round Overlay / 本轮重点

主 agent 必须选择一个 overlay，并删除无关 overlay。

#### round1 / Orientation

- 目标：建立背景、原文结构、相关方法、理论语境和候选 answer units。
- 重点：说明对象在做什么、作者 claim 是什么、已有方法或背景是什么、哪些内容可能成为后续 answer units。
- 禁止：不要直接写最终结论；不要把外部背景扩展成主任务；不要替主 agent 决定最终 answer units 状态。

#### round2 / Focused Investigation

- 目标：只研究一个 promoted answer unit；如果任务是补证，也必须围绕这个 answer unit 的明确证据缺口展开。
- 重点：回答该单元的对象、机制 / 结构 / 判定标准、直接证据、综合推断、边界和反例；如果找不到直接证据，明确写“未找到直接证据”并建议降级。
- 禁止：不要泛泛补背景；不要覆盖其他 answer units；不要把相邻概念支持写成文献直接支持；不要创建新的大范围调研主题。

#### round3 / Final Check

- 目标：核查关键 claim 是否被引用来源支持，并检查主 agent 是否过早停止。
- 重点：逐条 claim 输出 SUPPORTED / PARTIAL / UNSUPPORTED；逐个关键 answer unit 判断 resolved / weak / needs round2 / deferred / dropped，并说明理由。
- 禁止：不要新增大范围调研；不要引入未核查的新 claim；不要重写最终答案；不要把“看起来合理”当成证据足够。

### 5.4 落盘位置

请按下面位置读取任务并写入结果：

```text
    tmp/【编号-问题短名】问题研究/
      research.md
      rounds/
        round1/
          A-【领域或来源】.md
        round2/
          D-【answer-unit短名】.md
        round3/
          claim-check.md
          stop-challenge.md
```

- `research.md`：由主 agent 维护，保存任务边界和 subtasks 清单；子 agent 只读取其中分配给自己的任务小节。
- `rounds/round1/`：保存第一轮不同领域 / 来源 orientation 子 agent 的结果。
- `rounds/round2/`：保存第二轮围绕 promoted answer units 的聚焦调研和定向补证结果。
- `rounds/round3/`：保存第三轮关键 claim 的引用核查、evidence sufficiency 和反早停检查结果。
- 子 agent 不修改 `research.md`，不创建新的问题目录，只写对应 `rounds/<轮次>/<子任务名>.md`。

### 5.5 调研任务

- 我想知道：【写一个明确、边界清楚、可独立完成的调研任务】

例如：
- 这篇综述主要在综述什么，它的 taxonomy 如何组织？
- 这篇论文的某个模块在整体方法中承担什么作用？
- 某个 claim 是否真的被论文实验或引用文献支持？
- 为什么单 agent 连续推理容易形成自我合理化？
- 多 agent role separation 是否能降低认知偏差？
- 工具调用为什么可以被看成 value of information？
- X 能否被形式化为 POMDP / VOI / belief update？
- 某个领域有哪些关键论文、经典理论、反例或失败案例？

### 5.6 需要重点调查的方向

请重点分析：

1. 当前任务的对象、范围和不需要覆盖的内容；
2. 原文、论文、综述、经典理论或外部资料中的直接证据；
3. 如果涉及机制解释，说明因果链和可能的替代解释；
4. 如果涉及内容 / 结构理解，说明文章主线、章节 / 模块关系、taxonomy 或观点组织方式；
5. 如果涉及判定 / 评价，说明判定标准、支持证据、反证和结论强度；
6. 如果涉及概念映射，说明源概念、目标理论、可映射部分和不能映射的部分；
7. 区分哪些结论是文献直接支持，哪些属于综合推断；
8. 说明结论的适用范围、边界、反例或不确定部分。

### 5.7 优先搜索来源

搜索时按下面优先级查证，不要只依赖泛搜索结果：

1. 通用学术检索和引用追踪：
   - Google Scholar、Semantic Scholar、OpenAlex：用于找到综述、经典高被引论文、后续引用和相近概念。
   - DBLP：用于核对计算机科学论文的会议、年份、作者和正式出版记录。

2. 经典理论和基础概念：
   - 优先找综述论文、教材、handbook、survey、tutorial、foundational paper。
   - 哲学和决策理论优先查 Stanford Encyclopedia of Philosophy、Internet Encyclopedia of Philosophy、PhilPapers、PhilSci-Archive。
   - 认知科学优先查 Cognitive Science Society / CogSci proceedings、Cognitive Science、Topics in Cognitive Science、Annual Review of Psychology、Trends in Cognitive Sciences。

3. 机器学习 / LLM / 数学论文：
   - 预印本：arXiv 的 `cs.LG`、`cs.AI`、`cs.CL`、`stat.ML`、`math.OC` 等分类。
   - 机器学习顶会：NeurIPS proceedings、ICML / AISTATS / UAI 的 PMLR proceedings、ICLR / OpenReview。
   - NLP / LLM：ACL Anthology，重点看 ACL、EMNLP、NAACL、COLING、TACL、Computational Linguistics。
   - AI 综合会议：AAAI、IJCAI。
   - 如果问题涉及视觉、机器人、HCI 或数据挖掘，再查 CVF Open Access、RSS、CoRL、ICRA / IROS、ACM CHI、KDD。

4. 认知科学 / 心理学 / 哲学 / 决策理论：
   - 认知科学和心理学预印本：PsyArXiv、OSF Preprints。
   - 哲学和科学哲学预印本：PhilPapers、PhilSci-Archive。
   - 决策、经济学和管理科学：SSRN、Management Science、Operations Research、Decision Analysis、Journal of Risk and Uncertainty、Theory and Decision、Games and Economic Behavior。

5. 浏览器接管检索：
   - 如果顶会站点、OpenReview、Google Scholar、Semantic Scholar、ACL Anthology、PMLR、CVF 等页面需要交互式搜索、筛选、翻页或查看动态内容，可以使用 Kimi WebBridge skill 或 QoderWork 浏览器插件接管浏览器。
   - 适用场景：确认论文是否正式录用、核对 venue / 年份 / 作者、追踪引用链、查看 OpenReview 评审与最终版本、在顶会 proceedings 中按关键词逐站搜索。
   - 使用 Kimi WebBridge 时，先检查 `~/.kimi-webbridge/bin/kimi-webbridge status`；健康后用 `navigate` / `snapshot` / `click` / `fill` 完成检索，优先读页面文本快照，必要时截图或保存 PDF，任务结束后关闭会话。
   - 使用 QoderWork 浏览器插件时，按插件的浏览器接管流程打开官方来源页面，执行站内搜索、筛选和结果核对，并记录最终 URL。
   - 浏览器检索结果必须写入当前子任务对应的 `rounds/<轮次>/<子任务名>.md`：记录查询词、来源网站、URL、论文题名、作者、年份、venue、是否正式 proceedings、是否预印本。
   - 不要绕过付费墙、验证码或访问控制；不要读取、暴露或记录 cookie、token、账号信息、私有页面内容。

6. 证据使用规则：
   - 优先引用同行评审论文、正式 proceedings、综述、教材和经典理论；预印本只能作为“未同行评审的最新证据”。
   - 每条关键结论都要核对题名、作者、年份、 venue / DOI / URL。
   - 不要把博客、百科、课程笔记当作最终证据；它们最多用于定位关键词和参考文献。
   - 如果论文只支持相邻概念，必须写成“基于文献的综合推断”，不能写成“文献直接支持”。

### 5.8 输出结构

请按以下结构输出；按任务性质选择字段，不要为了填模板强行写“机制”。

#### 1. 任务定义与边界

说明本次子 agent 负责什么、不负责什么，以及使用了哪些主要材料或来源。

#### 2. 核心发现

根据任务类型选择性展开：

- 内容 / 结构理解：它在讲什么，主线、章节、模块、taxonomy 或观点如何组织；
- 判定 / 评价：待判定命题、判定标准、支持证据、反证、结论强度；
- 机制解释：现象、因果链、替代解释、机制作用；
- 概念映射：源概念、目标理论、映射关系、映射限制；
- 来源检索：关键来源列表、检索路径、每个来源能支持什么。

#### 3. 证据与引用

给出关键论文、综述或经典理论。

#### 4. 和已有学术概念的对应（可选）

如果有帮助，按学科或理论来源分组，把当前任务和已有概念建立对应关系。不要只罗列名词，必须说明：

- 概念的核心思想；
- 它和当前任务 / 问题 / 机制 / 判定标准 / 文章结构的对应关系；
- 关键文献或理论来源；
- 这种对应是文献直接支持，还是基于相邻概念的综合推断。

可用表格结构：

| 领域 | 概念 | 核心思想 | 与当前任务的对应 | 关键文献 | 支持类型 |
|---|---|---|---|---|---|
| 科学哲学 | 证伪主义 | 失败预测推动理论修正 | 将失败从终止信号转化为修正驱动力 | Popper, 1959/1963 | 概念映射 |
| 机器学习 / LLM Agent | Exposure Bias / Snowball Effect | 自回归模型在错误上继续条件化会放大误差 | 解释连续推理中的路径依赖和错误累积 | Bengio et al., 2015 | 相邻概念支持 |

Few-shot：如果问题是“AutoResearchClaw 中单 agent LLM 的连续推理是否会因为 autoregressive context-conditioning 产生路径依赖”，可按下面方向建立对应：

- 科学哲学：证伪主义、Lakatos 研究纲领、Kuhn 范式转换、溯因推理，用于解释失败如何从“终止信号”变成“修正驱动力”。
- 机器学习 / LLM Agent：Exposure Bias、Snowball Effect、Self-Refine、Reflexion、CRITIC、Process Reward Model，用于解释自回归条件化、误差累积、外部验证和过程级反馈。
- 软件工程：SBFL、Delta Debugging、FTA、FMEA，用于对应失败定位、失败原因隔离、故障分解和预设失效模式。
- AI 规划与搜索：STRIPS / Sense-Plan-Act、HTN、MCTS 回溯、偏序规划，用于对应失败后的重新规划、回溯和替代路径搜索。
- 科学实践：负面结果价值、可复现性危机、预注册，用于解释失败记录、复现检查和防止事后合理化。

#### 5. 适用范围 / 成立条件

说明当前结论在什么范围内成立：

- 如果是机制解释：说明机制依赖的前提、任务设置、数据分布或系统条件；
- 如果是文章 / 综述理解：说明结论只覆盖哪些章节、领域、时间范围、文献集合或作者定义；
- 如果是判定问题：说明判定标准适用的前提和结论强度；
- 如果是概念映射：说明哪些部分能映射，哪些部分不能映射。

#### 6. 边界 / 反例 / 不确定部分

说明哪些情况会让结论失效、变弱或需要降级：

- 反例、失败案例、冲突证据；
- 原文没有覆盖的范围；
- 只存在间接证据或相邻概念支持的部分；
- 需要主 agent 后续综合判断的部分。

#### 7. Evidence Sufficiency / 证据是否足够

对子 agent 自己负责的 answer unit 给出停止建议：

- resolved：本任务范围内证据足够，可以进入最终综合；
- weak：可以回答，但必须降级为作者 claim、弱证据、相邻概念支持或综合推断；
- needs round2：缺少明确证据、反例、baseline、实验、理论、跨领域依据，或需要围绕该 answer unit 再做聚焦调研；
- needs final-check：关键 claim、引用支持关系或 early-stop risk 需要在 Round 3 单独核查；
- dropped：和当前用户问题关系弱。

不要替主 agent 决定最终答案；这里只报告证据状态和缺口。

#### 8. Follow-up Candidates / 后续候选问题

如果发现当前任务之外仍需进一步研究的问题，按下面格式返回；如果没有，写“无”。

```md
Candidate 1：<问题短名>

- 来源：来自哪个章节、模块、claim、实验、文献或当前子任务发现；
- 父任务：当前子 agent 负责的问题；
- 新问题：
- 为什么当前任务不能直接回答：
- 需要进一步查证什么：
- 建议拆分方向：
- 可能证据来源：
- 对最终结论的影响：
- 优先级：high / medium / low
```

候选问题只是给主 agent triage 的输入，不是下一轮任务，也不是已验证结论。

#### 9. 可写进笔记的总结段

给出一段学术化中文总结，可以直接放进研究笔记。总结时按任务性质压缩：

- 内容 / 结构理解：压缩为“对象—主线—结构—贡献”；
- 判定 / 评价：压缩为“命题—标准—证据—结论强度—边界”；
- 机制解释：压缩为“现象—机制—证据—适用范围—边界”；
- 概念映射：压缩为“源概念—目标理论—映射关系—映射限制”；
- 来源检索：压缩为“来源格局—关键证据—缺口—后续建议”。

要求：
- 语言严谨；
- 避免泛泛而谈；
- 如果是机制问题，要明确因果链；如果不是机制问题，不要强行写因果链；
- 不要把推测写成已有结论；
- 需要引用时给出来源。

---

## 6. 主 agent 综合子 agent 结果

子 agent 每一轮返回调研结果后，主 agent 都必须执行本节作为 checkpoint。若当前不是最终轮，执行到 answer units、evidence ledger 和 final-check 状态更新后，决定进入下一轮或停止；不要直接写最终笔记。

执行要求：

1. 逐个读取 `tmp/*问题研究/` 下的 `research.md` 和 `rounds/`；
2. 先读取 `research.md` 中的“User Question Decomposition / 回答焦点表”；如果没有，必须先补建，不允许直接综合；
3. 先合并 `rounds/round1/`：去重来源，按领域整理证据，记录初步共识和冲突；
4. 如果输入是论文、综述或文章，先读取 Round 1 中的“原文结构理解”子任务，并把文章主线、章节 / taxonomy、作者 claim、证据组织方式和原文边界作为后续综合的骨架；外部 retrieval 只能补充或校验原文理解，不能替代“文章讲了什么”；
5. 收集所有子 agent 输出中的 `Follow-up Candidates / 后续候选问题`，追加或更新到 `tmp/followup_candidates.md`；
6. 建立或更新 answer units 状态表：合并重复候选，标记来源，判断它是否是最终答案必须单独回应的对象；
7. 将 answer units 映射到回答焦点表：逐个焦点判断“是否能回答、回答是否够深、缺哪些 answer units 或证据、是否需要 Round 2”；
8. 建立或更新 `tmp/evidence_ledger.md`：逐个 answer unit 记录原文 claim、支撑证据、反证 / 边界、证据缺口、所属回答焦点和状态理由；
9. 只把 promoted answer units 提升为下一轮任务；提升时创建或更新对应 `research.md`，记录 `parent_issue`、`derived_from`、`source_subagent_output`、`promotion_reason` 和输出目录；
10. 对 medium / low 候选标记为 deferred / dropped / resolved；如果它被保留为 answer unit 但证据不足，再标记为 weak，不要为了完整性继续派发；
11. 执行 pre-final gate：
   - 必须确认用户问题对应的 answer units 已出现在 `tmp/followup_candidates.md` 或 `research.md` 的 answer units 状态表中；
   - 必须确认每个回答焦点都有支撑材料、回答深度、最低证据要求、状态理由，且知道最终放在哪个标题下回答；
   - 如果某个回答焦点是 deep，相关关键 answer units 不能只靠 Round 1 背景扫描 resolved；必须有 Round 2 聚焦调研，或在 evidence ledger 中明确说明 Round 1 为什么已经满足 deep 标准；
   - 每个 answer unit 必须有 promoted / resolved / weak / deferred / dropped 状态和理由；
   - 每个关键 answer unit 必须在 `tmp/evidence_ledger.md` 中有 evidence sufficiency 记录；
   - 如果用户问题涉及创新点、机制、贡献、评价或迁移判断，最终前必须生成 `tmp/stop_challenge.md` 或 `rounds/round3/stop-challenge.md`；
   - 如果 Round 1 后所有关键创新点 / 机制 / 贡献 answer units 都被标记 resolved，必须把它当作 early-stop risk 处理；没有 stop-challenge 记录时，不允许 final；
   - 如果存在 promoted 但未完成的下一轮任务，不允许写最终笔记；
   - 如果 Round 1 子任务输出文件已存在，但 `research.md` 仍为 pending，必须先更新状态；
   - 如果没有创建 `tmp/followup_candidates.md`，必须说明没有候选问题和 answer units；否则不允许直接 final。
12. 再检查是否需要 Round 2 定向补证：如果某个回答焦点的最低证据要求未满足，或某个关键结论只有单来源、缺少反例、缺少最新论文、缺少跨学科支持，必须标成 promoted，并在 `rounds/round2/` 下派子 agent 补查；
13. 合并 `rounds/round2/` 中的定向补证结果：只吸收真正补上缺口的证据；无法补上的地方保留为不确定项或 weak；
14. 抽取最重要的 5-10 条 claim，在 `rounds/round3/` 下派子 agent 检查引用是否真的支持 claim，并执行反早停检查；
15. 根据 Round 3 final-check 结果处理 claim：
   - SUPPORTED：可以保留为文献直接支持；
   - PARTIAL：必须改写为更弱、更准确的表述；
   - UNSUPPORTED：删除、降级为综合推断，标为 weak，或继续标为未证实；
16. 更新 `research.md` 中的子任务状态、follow-up candidate 状态、answer units 状态、回答焦点表、evidence ledger、反早停检查和 final-check 状态；不要把子 agent 原始结果复制进 `research.md`；
17. 区分“文献直接支持”和“综合推断”；
18. 改写成研究笔记风格；
19. 根据回答焦点压缩：每个焦点用短标题和研究笔记式段落回答，保留该焦点需要的对象、依据、路径 / 机制 / 映射、边界和启发；创新点、机制解释、概念映射或迁移启发类焦点可以加入方法论 / 认识论 / 设计哲学视角，内容概括、事实解释或证据核对类焦点不要强行加入；
20. 使用更学术、严谨的中文表达；
21. 保留关键术语，去掉泛泛总结；
22. 把可复用内容合并进最终笔记结构；
23. 保留 `tmp/*问题研究/` 作为可追溯的子问题研究档案，不要用最终笔记覆盖或替代它们。

目的：

> 把调研材料转化成自己的知识结构。

---

## 7. 最终笔记格式

最终笔记只负责综合和压缩，不替代 `tmp/*问题研究/` 下的子问题调研文件；每个问题的检索记录、证据和完整调研结论仍应保留在对应目录中。

最终组织方式由主 agent 根据用户原始提问、材料结构和子 agent 的实际拆分决定。内部调研按 rounds、answer units 和子 agent 管理；最终笔记按用户真正关心的回答焦点组织。不要按 Round 1 / Round 2 / 子 agent 输出顺序写最终答案，也不要把每个焦点机械套成同一组字段。

回答焦点不是口语化小标题，而是最终答案中的分析单元。每个焦点都必须保留研究笔记的证据链和分析骨架，只是把无关字段删掉：

- 标题直接回应用户问法，例如“这篇文章讲什么”“核心创新点”“为什么这个机制有效”“证据是否支撑 claim”“可以迁移到哪里”。
- 每个焦点用 1-3 段严谨中文表述；必要时用短列表或表格，但不要把段落拆成碎片化字段。
- 段落必须能看出“对象 / 命题是什么、依据来自哪里、结论强度如何、边界在哪里”；如果是机制或创新，还要写清楚路径、机制或修改点。
- 已经在 Round 1 resolved 的内容可以简洁回答；经过 Round 2 或 Round 3 final-check 的焦点必须写清楚证据强度、反例或不确定部分。
- 如果不同焦点共享同一批背景材料，只在第一次需要时概括，不要重复堆背景。
- 如果某个焦点的证据不足，直接标明“不确定 / 只支持相邻概念 / 需要后续查证”；不要用顺滑文字掩盖证据缺口。

分析视角按需嵌入，不作为默认栏目：

- 内容 / 结构理解：压缩为“对象—主线—结构—贡献”。
- 创新点 / 方法改进：压缩为“旧痛点—本质缺陷—修改点—机制—证据—边界”。
- 机制解释：压缩为“现象—因果链—关键变量—证据—适用范围”。
- 判定 / 评价：压缩为“命题—判定标准—支持证据—反例—结论强度”。
- 概念映射：压缩为“源概念—目标理论—映射关系—不能映射部分”。
- 方法论 / 认识论 / 设计哲学：只在它能解释创新、机制、证据标准、问题定义、验证方式或迁移价值时写。不要把普通技术细节拔高成哲学结论。
- 形式化视角：只在形式化能提升解释力时写；如果会误导，明确说明不适合形式化。

输出格式：
```md
## 【Paper / Topic Name】

本质：
- 一句话本质

## 通用分析骨架

用很短篇幅交代整篇材料的对象与边界、核心问题 / 主线、主要证据来源和可信度。这里不是复制子 agent 的摘要，而是给后续回答焦点建立共同上下文。

## <回答焦点 1：按用户问法生成标题>

先直接回答该焦点，再把对象、证据来源、结论强度、路径 / 机制 / 映射、适用范围和边界压缩进连续段落。需要引用时在段落中说明来源，并区分文献直接支持、综合推断和不确定部分。

如果该焦点涉及创新点或机制，可以嵌入方法论 / 认识论 / 设计哲学分析，例如：它改变了什么问题定义、观察方式、证据标准、建模假设或系统取舍。若当前焦点只是内容概括或事实核对，不写这一层。

## <回答焦点 2：按用户问法生成标题>

继续回答另一个焦点。不要重复前文背景；直接补充该焦点特有的证据、分析和边界。若它来自某个子 agent 的结论，要综合子 agent 输出，而不是照抄其原文。

## 变化 / 创新 / 贡献总结（按任务选择）

如果用户关心创新、方法改进、系统设计或研究贡献，用“从 A 走向 B”的方式总结关键变化。每条变化都要对应到原文机制、实验证据、理论依据或综合推断，不能只写抽象赞美。

## 后续价值（按任务选择）

如果用户关心系统设计或方法迁移，写可迁移原则、适用条件、改造方式和风险。如果用户关心科研选题，写可以继续研究的问题。如果当前任务只是内容解释、证据判定或概念澄清，且没有明显后续价值，可以省略本节。
```

---
