## 2026-04-09 Seed
- 仓库中的 `.sop/context/*` 目前仅在 SOP 文档中被引用，尚未落地真实目录/文件。Prompt 只能引用现有交付文档与配置文件，不实现 context。

## 2026-04-09 Task 1 - Working tree hygiene
- 当前 working tree 存在较多未跟踪文件（`git status` 有很多 `??`）。后续进行 git 操作时必须**显式 add 指定文件**，避免 `git add .` 误加入垃圾文件。
