# Task 7 Evidence, Boundary Audit

## Check 1, Effective-change definition is present

- **Target section**: `三、版本管理规则（Versioning Rules）`
- **Pass criteria**: Effective change must be defined with concrete, testable triggers.
- **Status**: PASS
- **Exact references**:
  - `4. 有效内容修改定义：对 SOP 文件内容产生实质性变更的操作，必须满足以下任一类：`
  - `- 新增、删除、修改章节内容`
  - `- 调整分析框架或方法论`
  - `- 更新数据获取路径、来源、参数或引用方式`
  - `- 修改输出格式、判断标准或质量检查标准`
- **Verification note**: The draft defines a closed trigger list for effective changes.

## Check 2, Push failure handling is explicit

- **Target section**: `六、GitHub 同步规则` and `十二、异常边界处理`
- **Pass criteria**: Push failure must preserve local commit and direct the user to inspect network, permissions, or branch conflicts.
- **Status**: PASS
- **Exact references**:
  - `5. push 失败时，必须保留本地 commit，并明确提示用户检查网络、权限或分支冲突。`
  - `1. Push 失败处理：`
  - `- 保留本地 commit`
  - `- 明确提示用户手动处理，或确认后重试`
- **Verification note**: The failure path is spelled out in both policy and boundary sections.

## Check 3, Unrelated changes are handled before SOP actions

- **Target section**: `十二、异常边界处理`
- **Pass criteria**: Changes outside `交付文档/` must pause SOP work and ask the user to stash or commit them.
- **Status**: PASS
- **Exact references**:
  - `2. 工作区无关改动处理：`
  - `- 检测到 \\`交付文档/\\` 外的文件改动时，先暂停 SOP 相关操作`
  - `- 提示用户先 stash 或提交无关改动`
- **Verification note**: The draft blocks progress until unrelated changes are handled.

## Check 4, Dual-input missing is a stop condition

- **Target section**: `十一、模块输入分类规范` and `十二、异常边界处理`
- **Pass criteria**: If a dual-input module lacks data or资料, the draft must stop and request the missing source.
- **Status**: PASS
- **Exact references**:
  - `3. 双输入缺失处理：`
  - `- 检测到双输入模块缺少数据或资料任一来源时，必须暂停`
  - `- 明确提示用户补充缺失来源`
  - `- 必须在章节前置声明中同时标注所需数据和所需资料`
- **Verification note**: The stop rule and the dual-input requirement are both explicit.

## Check 5, User rejection blocks edits

- **Target section**: `十二、异常边界处理`
- **Pass criteria**: User rejection must cancel edits and keep current state unchanged.
- **Status**: PASS
- **Exact references**:
  - `4. 用户拒绝确认处理：`
  - `- 用户拒绝优化方案或修改计划时，不执行任何修改`
  - `- 保留当前状态`
  - `- 结束本次修改流程`
- **Verification note**: The branch is terminal and non-destructive.
