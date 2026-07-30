# Task 6 Evidence, Optimize Flow

## Check 1, Optimized SOP flow is explicit

- **Target section**: `五、行为规则（AI 执行逻辑）`
- **Pass criteria**: `优化当前 SOP` must require scope clarification, plan output, user confirmation, then edit.
- **Status**: PASS
- **Exact references**:
  - `## 五、行为规则（AI 执行逻辑）`
  - `### 1. 当用户说：“优化当前 SOP”`
  - Bullet sequence under that subsection
- **Verification note**: The flow now has four required steps before modification, and it explicitly blocks immediate editing.

## Check 2, No direct edit before confirmation

- **Target section**: `五、行为规则（AI 执行逻辑）`
- **Pass criteria**: The rule must say editing happens only after user confirmation.
- **Status**: PASS
- **Exact references**:
  - `- 等待用户确认`
  - `- 仅在用户确认后修改当前主文件`
- **Verification note**: The confirmation gate is written as a required step, not a note.

## Check 3, Plan first, then execute

- **Target section**: `五、行为规则（AI 执行逻辑）`
- **Pass criteria**: The rule must force the agent to output a plan before any edit.
- **Status**: PASS
- **Exact references**:
  - `- 输出优化方案或修改计划`
  - `- 等待用户确认`
- **Verification note**: The edit step is downstream of the plan step.
