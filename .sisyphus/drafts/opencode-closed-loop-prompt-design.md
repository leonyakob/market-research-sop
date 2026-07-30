# Draft: opencode闭环Prompt设计

## Requirements (confirmed)
- 用户希望为品牌策划全案 SOP 项目设计一套可复用的“闭环 Prompt”。
- 当前痛点：每次新开 session 都要重复说明“要做什么、已经做了什么、接下来做什么、注意事项是什么”。
- 目标之一：在完成一个模块后，让 opencode 基于品牌策划全案框架与 `交付文档/` 里已有文件名，判断下一个模块，并自动产出“开启下一个模块设计”的 Prompt。
- 当前聚焦范围：先只设计“第一个大模块：市场研究”相关 Prompt，不扩展到其他大模块。
- 用户已提供一个用于 1.2 竞争分析模块的历史 Prompt 作为参考样例。
- 用户当前请求：先设计“闭环 Prompt 系统结构”，再基于它产出优化版 Prompt。
- “品牌策划全案框架”以 `~/github/market-research-sop/交付文档/1-品牌策划全案SOP框架.md` 为准，且文档内为完整模块清单。
- “自动生成下一个 Prompt”的触发时机：用户手动输入“为下一个模块生成 Prompt”后才触发。
- 期望交付形态：一套 Prompt 体系，而不是单条 Prompt。

## Technical Decisions
- 当前任务属于 Prompt 设计与流程规划，不是直接实施自动化。
- 优化目标应兼顾：可复用性、模块差异化、对 OpenCode/Prometheus 工作流的适配性。
- Prompt 体系应拆分为：总控层、模块类型层、实例层。
- “下一个模块识别”应以框架清单顺序 + `交付文档/` 现有文件名为判断依据。
- 当前仅覆盖“市场研究”大模块（1.1-1.5）的 Prompt 模板与闭环逻辑。

## Research Findings
- `~/github/market-research-sop/交付文档/1-品牌策划全案SOP框架.md` 已确认包含完整模块顺序。
- 市场研究模块顺序为：1.1 行业研究 → 1.2 竞争分析 → 1.3 消费者研究 → 1.4 趋势分析 → 1.5 机会分析。

## Open Questions
- 优化版 Prompt 最终是否需要内置“生成 Prompt 文件到 ~/github/market-research-sop/Prompt/”这一步的明确指令。
- 市场研究内部是否还需要进一步区分子类型（如数据优先型、综合分析型）来使用不同 Prompt 子模板。

## Scope Boundaries
- INCLUDE: 理解需求、识别缺失信息、设计闭环 Prompt 系统结构、产出市场研究范围内的优化版 Prompt。
- EXCLUDE: 立即实现自动识别逻辑、修改业务文件、生成非 markdown 交付物。
