# Task 8 Evidence, Final Draft Check

## Check 1, Baseline carries forward long-term rules

- **Target section**: `二、结构规则（Structure Rules）`, `三、版本管理规则（Versioning Rules）`, `六、GitHub 同步规则`, `八、SOP v3.0 执行规则`, `十、思考逻辑梳理规范`
- **Pass criteria**: Final candidate must preserve the accepted long-term constraints from task 6 and task 7.
- **Status**: PASS
- **Exact references**:
  - `- 采用扁平化交付结构，所有 SOP 文件统一存放在 \`交付文档/\` 目录，不创建模块子目录。`
  - `- 同一模块在 OpenCode 工作区只保留一个当前主文件，不保留版本后缀文件。`
  - `- 每次对 \`交付文档/\` 下 SOP 主文件做出有效内容修改后，必须执行 Git commit 并 push 到 GitHub。`
  - `- 所有 SOP 模块的数据获取必须通过 \`.sop/config/data-acquisition.yaml\` 配置。`
  - `- 触发条件：每次 SOP 模块交付完成后，必须同步产出思考逻辑梳理文档。`
- **Verification note**: The final draft keeps the single-master-file policy, Git history policy, data-acquisition routing, and thinking-logic output rule.

## Check 2, Module mapping remains explicit and complete

- **Target section**: `十一、模块输入分类规范`
- **Pass criteria**: The final candidate must keep the numbered module mapping table and the dual-input rules.
- **Status**: PASS
- **Exact references**:
  - `### 11.1 数据优先`
  - `### 11.2 资料优先`
  - `### 11.3 数据 + 资料双输入`
  - `- 1.1 行业研究`
  - `- 2.1 品牌定位`
  - `- 7.1 上市计划`
  - `- 必须在章节前置声明中同时标注所需数据和所需资料`
  - `- 数据获取和资料引用必须分别明确来源`
- **Verification note**: The module-number mapping is still explicit and the dual-input handling remains actionable.

## Check 3, Execution note blocks direct AGENTS.md edits

- **Target section**: `十四、执行说明`
- **Pass criteria**: The final candidate must say user approval is required before editing root `AGENTS.md`.
- **Status**: PASS
- **Exact references**:
  - `## 十四、执行说明`
  - `当前文件仅为最终候选草案。`
  - `用户确认后，方可由其他 agent 编辑根目录 \`AGENTS.md\`。`
- **Verification note**: The handoff note is explicit and prevents unsanctioned edits to the root file.

## Check 4, Internal consistency and scope are preserved

- **Target section**: Entire draft
- **Pass criteria**: The candidate must stay concise, internally consistent, and limited to packaging and handoff scope.
- **Status**: PASS
- **Exact references**:
  - `项目采用：模块化结构 + Git/GitHub 历史保留 + 单主文件工作区模式。`
  - `版本演进信息必须内嵌在 SOP 文件附录中管理，不创建独立 README 文件，不创建 v1/v2/v3 并行文件。`
  - `当前文件仅为最终候选草案。`
- **Verification note**: The draft does not introduce new scope and stays aligned with the package-only task.

## 补充说明

当前阶段仅输出候选草案，不修改根目录 AGENTS.md。
