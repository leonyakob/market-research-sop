# Task 6 Evidence, Edge Cases

## Check 1, Push failure handling is explicit

- **Target section**: `六、GitHub 同步规则`
- **Pass criteria**: Push failure must preserve local commit and prompt the user to inspect network, permissions, or branch conflicts.
- **Status**: PASS
- **Exact references**:
  - `## 六、GitHub 同步规则`
  - `5. push 失败时，必须保留本地 commit，并明确提示用户检查网络、权限或分支冲突。`
- **Verification note**: The rule uses imperative handling, not a vague reminder.

## Check 2, Unrelated workspace changes are blocked before action

- **Target section**: `十二、异常边界处理`
- **Pass criteria**: Unrelated changes must pause SOP work and ask the user to stash or commit them first.
- **Status**: PASS
- **Exact references**:
  - `2. 工作区无关改动处理：`
  - `- 检测到 \`交付文档/\` 外的文件改动时，先暂停 SOP 相关操作`
  - `- 提示用户先 stash 或提交无关改动`
- **Verification note**: The rule explicitly pauses rather than auto-fixing.

## Check 3, Dual input missing is a stop condition

- **Target section**: `十二、异常边界处理` and `十一、模块输入分类规范`
- **Pass criteria**: If a dual input module lacks either data or资料, the agent must pause and ask for the missing source.
- **Status**: PASS
- **Exact references**:
  - `3. 双输入缺失处理：`
  - `- 检测到双输入模块缺少数据或资料任一来源时，必须暂停`
  - `- 明确提示用户补充缺失来源`
  - `11.3 数据 + 资料双输入`
- **Verification note**: The stop rule and the module class both appear in the draft.

## Check 4, User rejection blocks edits

- **Target section**: `十二、异常边界处理`
- **Pass criteria**: If the user rejects the plan, no modification may happen and the current state must remain intact.
- **Status**: PASS
- **Exact references**:
  - `4. 用户拒绝确认处理：`
  - `- 用户拒绝优化方案或修改计划时，不执行任何修改`
  - `- 保留当前状态`
  - `- 结束本次修改流程`
- **Verification note**: The handling is explicit and terminal.

## Check 5, Restore flow uses Git history and version history entry

- **Target section**: `五、行为规则（AI 执行逻辑）` and `三、版本管理规则（Versioning Rules）`
- **Pass criteria**: Restore must use Git/GitHub history, restore to current main file, and append a version history entry.
- **Status**: PASS
- **Exact references**:
  - `- 使用 Git / GitHub 历史定位目标版本`
  - `- 使用 Git / GitHub 历史恢复旧版本到当前主文件`
  - `- 在当前主文件“版本历史”章节新增回溯记录`
  - `6. 恢复规则：使用 Git / GitHub 历史恢复旧版本到当前主文件后，必须在该文件“版本历史”章节新增一条记录，并执行 Git commit 与 push。`
- **Verification note**: The restore sequence now ties history lookup, restore, and logging together.
