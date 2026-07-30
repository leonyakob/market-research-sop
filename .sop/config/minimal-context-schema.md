# 跨 Session 最小上下文模型 - Schema 草表

**版本**: v1.0  
**生成日期**: 2026-04-20  
**基于**: 市场研究模块 1.1-1.5 实际结构分析  

---

## Schema 总览

本 Schema 定义 4 类卡片结构，用于跨 Session 最小上下文继承：

| 卡片类型 | 数量 | 用途 | 创建时机 |
|---------|------|------|---------|
| **Card 1: Project State Card** | 每个 Session 1 张 | 任务状态、进度、冻结决策 | 新 Session 开始时 |
| **Card 2: Module Dependency Card** | 每个模块 1 张 | 上下游依赖关系 | 模块完成时 |
| **Card 3: Output Interface Card** | 每个模块 1 张 | 输出定义、消费者、状态 | 模块完成时 |
| **Card 4: Normalization Bridging Card** | 按需创建 | 派生字段语义桥接 | 检测到 normalized 字段时 |

---

## Card 1: Project State Card（项目状态卡）

### 用途
当前 Session 的任务状态、进度、冻结决策。

### 必含字段

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|--------|------|------|------|------|
| `framework_file` | string | ✅ | 全案框架文件路径 | `~/github/market-research-sop/交付文档/1-品牌策划全案SOP框架.md` |
| `completed_modules` | array[string] | ✅ | 已完成模块列表 | `["1.1", "1.2", "1.3", "1.4", "1.5"]` |
| `current_target_module` | string | ✅ | 当前目标模块 | `"2.1"` |
| `current_task_type` | string | ✅ | 任务类型 | `design_sop` / `optimize_sop` / `review_sop` |
| `current_phase` | string | ✅ | 当前阶段 | `question_design` / `sop_generation` / `review` |
| `frozen_decisions` | array[string] | ❌ | 已冻结决策 | `["市场研究模块采用双模板体系"]` |
| `do_not_repeat` | array[string] | ❌ | 不应重复做的事 | `["不要重新确认框架顺序"]` |
| `last_session_summary` | string | ❌ | 上一轮核心结论(50字内) | `"1.5完成，准备进入2.1"` |

---

## Card 2: Module Dependency Card（模块依赖卡）

### 用途
定义模块的上下游依赖关系，明确必须承接哪些上游输出。

### 必含字段

| 字段名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `module` | string | ✅ | 模块编号，如 `"2.1"` |
| `module_name` | string | ✅ | 模块名称，如 `"品牌定位"` |
| `module_type` | string | ✅ | 模块类型：`数据研究型` / `对比/判断型` / `双输入型` / `战略判断型` |
| `upstream` | array[object] | ✅ | 上游依赖列表（见下） |
| `downstream` | array[string] | ✅ | 下游消费者模块列表 |

### upstream_item 结构

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|--------|------|------|------|------|
| `module` | string | ✅ | 上游模块编号 | `"1.5"` |
| `relation` | string | ✅ | 关系强度：`strong` / `conditional` / `contextual` | `"strong"` |
| `relation_rationale` | string | ✅ | 为什么有这个依赖 | `"提供优先级机会清单、机会-定位映射方向"` |
| `required_consumables` | array[string] | ✅ | 必须承接的上游输出对象ID | `["priority_opportunities", "opportunity_positioning_mapping"]` |
| `read_level` | string | ✅ | 最低读取层级：`L0` / `L1` / `L2` | `"L1"` |

### Read Level 定义

- **L0**: 只读依赖摘要（模块元数据）→ 适合快速依赖识别
- **L1**: 读取接口定义（输出接口卡）→ 适合问题设计
- **L2**: 补读桥接/规则信息 → 适合接口契约设计

---

## Card 3: Output Interface Card（输出接口卡）

### 用途
精确定义模块输出什么、给谁、状态如何。**最关键的一张卡**。

### 必含字段

| 字段名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `module` | string | ✅ | 模块编号 |
| `formal_outputs` | array[object] | ✅ | 正式输出对象列表（一定有） |
| `conditional_outputs` | array[object] | ❌ | 条件输出对象列表（不一定有） |
| `watch_outputs` | array[object] | ❌ | 观察/降级输出 |
| `interface_meta` | object | ✅ | 接口元信息 |

### formal_output_item 结构

| 字段名 | 类型 | 必填 | 说明 | 来源 |
|--------|------|------|------|------|
| `output_id` | string | ✅ | 机器可读的输出ID | SOP第九章接口定义 |
| `output_name` | string | ✅ | 人类可读的输出名称 | SOP第九章接口定义 |
| `consumers` | array[string] | ✅ | 消费此输出的下游模块 | SOP第九章下游交接定义 |
| `status` | string | ✅ | 状态：`canonical`（正式输出恒为此） | - |
| `semantics` | string | ✅ | 字段语义定义 | SOP第九章或输出格式章节 |
| `schema_location` | string | ❌ | Schema定义位置（如有） | SOP第九章 |

### interface_meta 结构

| 字段名 | 类型 | 必填 | 说明 | 来源 |
|--------|------|------|------|------|
| `storage_path` | string | ✅ | 输出存储路径 | `~/.sop/context/{模块号}-output.json` |
| `interface_definition_location` | string | ✅ | 接口定义在SOP中的位置 | `{文件名}:{行号范围}` |
| `provisional_note` | string | ❌ | 前瞻性接口说明（如有） | SOP第九章provisional标注 |

---

## Card 4: Normalization Bridging Card（语义桥接卡）

### 用途
解决"字段语义分散定义"问题。当检测到 `derived` / `normalized` / `conditional` / `provisional` 字段时创建。

### 必含字段

| 字段名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `module` | string | ✅ | 下游模块编号（谁需要理解这个桥接） |
| `derived_inputs` | array[object] | ✅ | 派生/归一化输入字段列表（见下） |
| `critical_warnings` | array[string] | ❌ | 下游模块必须知道的风险提示 |

### derived_input_item 结构

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|--------|------|------|------|------|
| `field` | string | ✅ | 字段名 | `"social_cultural_signals"` |
| `source_module` | string | ✅ | 来源模块 | `"1.1"` |
| `source_type` | string | ✅ | 来源类型：`native` / `normalized` / `derived` / `conditional_normalized` | `"normalized"` |
| `authoritative_origin` | array[string] | ✅ | 权威来源位置（可能有多个） | `["1.1正文PEST:264-268", "1.3归一化说明:575-579"]` |
| `fallback` | string | ❌ | 降级处理规则 | `"若C1不可用，返回空数组并标unavailable"` |
| `read_level_required` | string | ✅ | 理解此字段需要的最低读取层级：`L1` / `L2` | `"L2"` |

### source_type 定义

- **native**: 原生接口字段，单点定义
- **normalized**: 正文提取后归一化，非原生接口字段
- **derived**: 派生计算得出
- **conditional_normalized**: 条件输出 + 归一化

---

## 1.1-1.5 实际卡片实例

### 1.1 行业研究

**Module Dependency Card:**
```yaml
module: "1.1"
module_name: "行业研究"
module_type: "数据研究型"
upstream: []  # 无上游
downstream: ["1.2", "1.3", "1.4", "1.5", "2.1"]
```

**Output Interface Card:**
```yaml
module: "1.1"
formal_outputs:
  - output_id: "market_size_data"
    output_name: "市场规模数据"
    consumers: ["1.2", "2.1"]
    status: "canonical"
    semantics: "历史+预测市场规模时间序列"
    source: "1.1-行业研究SOP.md:325-330 (表1)"
  
  - output_id: "industry_lifecycle"
    output_name: "行业生命周期阶段"
    consumers: ["1.2", "2.1"]
    status: "canonical"
    semantics: "行业所处生命周期阶段判断"
    source: "1.1-行业研究SOP.md:278-281"
  
  - output_id: "ksfs"
    output_name: "关键成功因素"
    consumers: ["1.2", "1.5", "4.1"]
    status: "canonical"
    semantics: "带优先级排序的KSFs清单"
    source: "1.1-行业研究SOP.md:415-433"
  
  - output_id: "opportunities_risks"
    output_name: "机会点与风险点"
    consumers: ["1.5", "2.1"]
    status: "canonical"
    semantics: "带数据支撑的机会与风险清单"
    source: "1.1-行业研究SOP.md:445-455"
  
  - output_id: "strategic_recommendations"
    output_name: "战略建议选项"
    consumers: ["2.1"]
    status: "canonical"
    semantics: "基于行业分析的战略建议"
    source: "1.1-行业研究SOP.md:576-584"

interface_meta:
  storage_path: "~/.sop/context/1.1-output.json"
  interface_definition_location: "1.1-行业研究SOP.md:568-593"
```

**注意**: 1.1 没有单独的 Bridging Card，但它的 `social_cultural_signals` 被 1.3 在下游标注为 `normalized`。

---

### 1.2 竞争分析

**Module Dependency Card:**
```yaml
module: "1.2"
module_name: "竞争分析"
module_type: "对比/判断型"
upstream:
  - module: "1.1"
    relation: "strong"
    relation_rationale: "需要1.1输出的市场规模数据、行业生命周期阶段、KSFs、机会风险"
    required_consumables: ["market_size_data", "industry_lifecycle", "ksfs", "opportunities_risks"]
    read_level: "L1"
downstream: ["1.3", "1.5", "2.1", "4.1", "渠道策略"]
```

**Output Interface Card:**
```yaml
module: "1.2"
formal_outputs:
  - output_id: "competitors"
    output_name: "竞品集与入选依据"
    consumers: ["1.5", "2.1", "4.1"]
    status: "canonical"
    source: "1.2-竞争分析SOP.md:677-680"
  
  - output_id: "strategic_groups"
    output_name: "战略群组结果"
    consumers: ["1.5", "2.1"]
    status: "canonical"
    source: "1.2-竞争分析SOP.md:682"
  
  - output_id: "positioning_map_data"
    output_name: "定位图数据"
    consumers: ["1.5", "2.3"]
    status: "canonical"
    source: "1.2-竞争分析SOP.md:683"
  
  - output_id: "competitor_matrix"
    output_name: "竞品对标矩阵"
    consumers: ["1.5", "2.1", "4.1"]
    status: "canonical"
    source: "1.2-竞争分析SOP.md:684"
  
  - output_id: "competitive_opportunities"
    output_name: "竞争机会清单"
    consumers: ["1.5", "2.1"]
    status: "canonical"
    source: "1.2-竞争分析SOP.md:690"

conditional_outputs:
  - output_id: "sentiment_insights"
    output_name: "用户口碑洞察"
    consumers: ["1.3", "2.1"]
    status: "conditional"
    appears_when: "C1条件满足，有足够用户评价数据"
    source: "1.2-竞争分析SOP.md:694-696"
  
  - output_id: "supply_chain_signals"
    output_name: "供应链能力信号"
    consumers: ["4.1", "供应链策略"]
    status: "conditional"
    appears_when: "C2条件满足"
    source: "1.2-竞争分析SOP.md:697"
  
  - output_id: "financial_signals"
    output_name: "财务与资本信号"
    consumers: ["1.5", "投资决策"]
    status: "conditional"
    appears_when: "C3条件满足"
    source: "1.2-竞争分析SOP.md:698"
  
  - output_id: "competitive_dynamics"
    output_name: "竞品动态监测点"
    consumers: ["1.4", "1.5"]
    status: "conditional"
    appears_when: "C4条件满足"
    source: "1.2-竞争分析SOP.md:699"

interface_meta:
  storage_path: "~/.sop/context/1.2-output.json"
  interface_definition_location: "1.2-竞争分析SOP.md:667-758"
```

---

### 1.3 消费者研究

**Module Dependency Card:**
```yaml
module: "1.3"
module_name: "消费者研究"
module_type: "数据研究型"
upstream:
  - module: "1.1"
    relation: "strong"
    relation_rationale: "需要1.1输出的社会文化信号、KSFs、行业阶段"
    required_consumables: ["social_cultural_signals", "ksfs", "industry_lifecycle"]
    read_level: "L2"  # 因为 social_cultural_signals 是 normalized
  
  - module: "1.2"
    relation: "conditional"
    relation_rationale: "需要1.2条件输出C1的口碑数据、竞争机会ID"
    required_consumables: ["sentiment_insights", "competitive_opportunities"]
    read_level: "L2"  # 因为 conditional 输出

downstream: ["1.4", "1.5", "2.1"]
```

**Output Interface Card:**
```yaml
module: "1.3"
formal_outputs:
  - output_id: "segmentation_metadata"
    output_name: "细分元数据"
    consumers: ["1.4", "1.5", "2.1"]
    status: "canonical"
    source: "1.3-消费者研究SOP.md:200-206"
  
  - output_id: "evidence_inputs"
    output_name: "证据输入摘要"
    consumers: ["1.4", "1.5"]
    status: "canonical"
    source: "1.3-消费者研究SOP.md:207-226"
  
  - output_id: "segments"
    output_name: "细分人群数组"
    consumers: ["1.5", "2.1", "产品策略"]
    status: "canonical"
    semantics: "3-5类人群完整画像（定义层+核心层+扩展层+决策层）"
    source: "1.3-消费者研究SOP.md:228-287"
  
  - output_id: "segment_comparison"
    output_name: "人群对比矩阵"
    consumers: ["1.5", "2.1"]
    status: "canonical"
    source: "1.3-消费者研究SOP.md:289-292"
  
  - output_id: "downstream_handoff"
    output_name: "下游交接包"
    consumers: ["1.4", "1.5", "2.1"]
    status: "canonical"
    semantics: "模块化交接数据结构"
    source: "1.3-消费者研究SOP.md:293-311"

interface_meta:
  storage_path: "~/.sop/context/1.3-output.json"
  interface_definition_location: "1.3-消费者研究SOP.md:565-605"
  provisional_note: "以下字段为前瞻性下游交接 Schema，用于保证 1.3 输出的数据类型可覆盖 1.4 / 1.5 / 2.1 的潜在输入需求"
```

**Normalization Bridging Card:**
```yaml
module: "1.3"
derived_inputs:
  - field: "social_cultural_signals"
    source_module: "1.1"
    source_type: "normalized"
    authoritative_origin:
      - "1.1 正文 PEST 社会文化部分 (1.1-行业研究SOP.md:264-268)"
      - "1.3 接口归一化说明 (1.3-消费者研究SOP.md:575-579)"
    read_level_required: "L2"
  
  - field: "top_switching_triggers"
    source_module: "1.2"
    source_type: "conditional_normalized"
    authoritative_origin:
      - "1.2 条件输出 C1 (1.2-竞争分析SOP.md:694-699)"
      - "1.3 接口归一化说明 (1.3-消费者研究SOP.md:577-579)"
    fallback: "若C1不可用，返回空数组并标 unavailable"
    read_level_required: "L2"

critical_warnings:
  - "此模块下游交接字段并非全部来自上游 formal outputs"
  - "不可把 derived 字段误当作 canonical 原生字段"
```

---

### 1.4 趋势分析

**Module Dependency Card:**
```yaml
module: "1.4"
module_name: "趋势分析"
module_type: "数据研究型"
upstream:
  - module: "1.1"
    relation: "strong"
    relation_rationale: "需要1.1输出的PEST社会文化信号作为趋势输入"
    required_consumables: ["social_cultural_signals"]
    read_level: "L2"
  
  - module: "1.2"
    relation: "conditional"
    relation_rationale: "需要1.2条件输出C4的动态监测"
    required_consumables: ["competitive_dynamics"]
    read_level: "L2"
  
  - module: "1.3"
    relation: "strong"
    relation_rationale: "需要1.3输出的下游交接包"
    required_consumables: ["downstream_handoff.for_1_4_trend_analysis"]
    read_level: "L1"

downstream: ["1.5", "2.1", "2.3", "5.2", "5.3"]
```

**Output Interface Card:**
```yaml
module: "1.4"
formal_outputs:
  - output_id: "candidate_objects"
    output_name: "趋势候选对象列表"
    consumers: ["internal"]
    status: "canonical"
    source: "1.4-趋势分析SOP.md:623-627"
  
  - output_id: "validated_trends"
    output_name: "正式趋势主表"
    consumers: ["1.5", "2.1", "2.3", "5.2", "5.3"]
    status: "canonical"
    semantics: "经有效性验证的正式趋势（短期+中期）"
    source: "1.4-趋势分析SOP.md:627-629"
  
  - output_id: "long_term_watchlist"
    output_name: "长期观察池"
    consumers: ["1.5", "2.x"]
    status: "canonical"
    semantics: "潜力待验证的长期趋势"
    source: "1.4-趋势分析SOP.md:630"
  
  - output_id: "downstream_handoff"
    output_name: "下游交接包"
    consumers: ["1.5", "2.x"]
    status: "canonical"
    semantics: "模块化交接数据结构"
    source: "1.4-趋势分析SOP.md:643-656"

conditional_outputs:
  - output_id: "conflict_register"
    output_name: "冲突登记表"
    consumers: ["review_only"]
    status: "conditional"
    appears_when: "存在多源冲突"
    source: "1.4-趋势分析SOP.md:639"
  
  - output_id: "evidence_gap_register"
    output_name: "证据缺口登记表"
    consumers: ["follow_up"]
    status: "conditional"
    appears_when: "关键证据不完整"
    source: "1.4-趋势分析SOP.md:640"
  
  - output_id: "channel_content_signals"
    output_name: "渠道/内容信号包"
    consumers: ["5.2", "5.3"]
    status: "conditional"
    appears_when: "C3条件满足"
    source: "1.4-趋势分析SOP.md:641"

interface_meta:
  storage_path: "~/.sop/context/1.4-output.json"
  interface_definition_location: "1.4-趋势分析SOP.md:612-728"
```

---

### 1.5 机会分析

**Module Dependency Card:**
```yaml
module: "1.5"
module_name: "机会分析"
module_type: "对比/判断型"
upstream:
  - module: "1.1"
    relation: "strong"
    relation_rationale: "提供行业机会点与风险点、KSFs、社会文化信号"
    required_consumables: ["industry_opportunities", "ksfs", "social_cultural_signals"]
    read_level: "L2"  # social_cultural_signals 需要 L2
  
  - module: "1.2"
    relation: "strong"
    relation_rationale: "提供竞争机会清单、定位图空白区域、战略群组结果"
    required_consumables: ["competitive_opportunities", "positioning_map_gaps", "strategic_groups"]
    read_level: "L1"
  
  - module: "1.3"
    relation: "strong"
    relation_rationale: "提供消费者机会假设、细分人群"
    required_consumables: ["underserved_segments", "high_growth_segments", "high_pain_low_solution", "opportunity_hypotheses"]
    read_level: "L1"
  
  - module: "1.4"
    relation: "strong"
    relation_rationale: "提供趋势机会领域、高优先级趋势、观察池机会项、趋势风险标记"
    required_consumables: ["trend_opportunity_areas", "high_priority_trends", "watchlist_items", "trend_risk_flags"]
    read_level: "L1"

downstream: ["2.1", "2.2", "2.3"]
```

**Output Interface Card:**
```yaml
module: "1.5"
formal_outputs:
  - output_id: "candidate_opportunities"
    output_name: "机会候选对象列表"
    consumers: ["internal"]
    status: "canonical"
    source: "1.5-机会分析SOP.md:812"
  
  - output_id: "screening_results"
    output_name: "机会筛选结果表"
    consumers: ["internal"]
    status: "canonical"
    source: "1.5-机会分析SOP.md:813"
  
  - output_id: "validated_opportunities"
    output_name: "正式机会主表"
    consumers: ["2.1", "2.2", "2.3"]
    status: "canonical"
    semantics: "经筛选与优先级排序后的正式机会对象"
    source: "1.5-机会分析SOP.md:814"
  
  - output_id: "opportunity_watchlist"
    output_name: "观察池"
    consumers: ["2.x"]
    status: "derived"
    semantics: "潜力待验证的机会项"
    source: "1.5-机会分析SOP.md:815"
  
  - output_id: "downstream_handoff"
    output_name: "下游交接包"
    consumers: ["2.1", "2.2", "2.3"]
    status: "canonical"
    semantics: "模块化交接数据结构（for_2.1/for_2.2/for_2.3）"
    source: "1.5-机会分析SOP.md:827-844"

conditional_outputs:
  - output_id: "conflict_register"
    output_name: "冲突登记表"
    consumers: ["review_only"]
    status: "conditional"
    source: "1.5-机会分析SOP.md:824"
  
  - output_id: "evidence_gap_register"
    output_name: "证据缺口登记表"
    consumers: ["follow_up"]
    status: "conditional"
    source: "1.5-机会分析SOP.md:825"

interface_meta:
  storage_path: "~/.sop/context/1.5-output.json"
  interface_definition_location: "1.5-机会分析SOP.md:796-941"
```

---

## Schema 使用指南

### 何时创建哪张卡

| 触发条件 | 创建卡片 | 创建者 | 包含内容 |
|---------|---------|--------|---------|
| 每个新 Session 开始 | Project State Card | Prometheus 或用户 | 当前任务状态、冻结决策、不重复项 |
| 模块完成时 | Module Dependency Card | Prometheus | 上下游模块、关系类型、必须承接的输出、读取层级 |
| 模块完成时 | Output Interface Card | Prometheus | 正式输出、条件输出、观察输出、接口元信息 |
| 检测到 normalized/conditional 字段时 | Normalization Bridging Card | Prometheus | 派生字段定义、权威来源、降级规则、关键警告 |

### Read Level 协议

```
L0: 只读依赖摘要（适合快速依赖识别）
  └─ 读取：Project State Card + Module Dependency Card

L1: 读取接口定义（适合问题设计）
  └─ 读取：+ Output Interface Card

L2: 补读桥接/规则信息（适合接口契约设计）
  └─ 读取：+ Normalization Bridging Card + 必要时少量规则摘要
```

### 稳定性保障

这套模型通过以下机制保证跨模型稳定：

1. **Project State Card** → 保证任务上下文一致
2. **Module Dependency Card** → 保证依赖关系一致
3. **Output Interface Card** → 保证接口理解一致
4. **Normalization Bridging Card** → 保证语义理解一致
5. **Read Level Protocol** → 保证读取深度可控

### 核心原则

> **不要问 AI "你觉得要不要补读上游内容"；**  
> **要告诉 AI "在什么条件下必须补读到哪一层"。**

---

## 版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| v1.0 | 2026-04-20 | 初始版本，基于 1.1-1.5 实际结构分析 |
