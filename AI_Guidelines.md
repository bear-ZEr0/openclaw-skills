# 🤖 AI 助手开发准则与行为要求 (Guidelines & Preferences)

> 💡 **文档说明**：
> 本文档由 User 维护和更新。它定义了在这个 Gym Agent 项目中，AI 助手必须严格遵守的做事风格、开发流闭环和文件管理习惯。AI 每次执行任务必须参照这里的核心原则。

## 🎯 核心行为守则

1. **里程碑必须提交 (Github Sync)**：
   - 每完成一个小章节（User Story），或者完成了一个重要阶段任务（Epic），必须主动将代码 commit 并 push 到远程 GitHub 仓库，确保进度不丢失。
   - Commit Message 请保持规范（如 `feat:` / `fix:` / `docs:` 等）。

2. **踩坑与反思沉淀 (Troubleshooting Logging)**：
   - **优先查阅 (Doc-First Research)**：遇到报错、环境 Bug 或是执行卡点时，**必须第一时间查询 `docs/踩坑与反思记录(Troubleshooting Log).md` 和 `docs/Agent_Handover_Context(项目交接快照).md`**。如果文档中已有记录，严禁重复自行排查，必须直接采用记录中的方案以节省时间。
   - **记录义务 (Logging Duty)**：处理完新的报错或卡点后，必须将“遇到什么问题 -> 为什么发生 -> 怎么解决的 -> 后续如何规避”这段反思，写入并追加到 `docs/踩坑与反思记录(Troubleshooting Log).md` 中。

3. **透明化文档管理 (Docs Visibility)**：
   - 任何需要长期跟进、更新的项目记录文件（包括需求文档、敏捷拆解计划、任务追踪清单、开发准则等），都必须放置在项目的 `docs/` 目录下。
   - 严禁只存储在 AI 的隐藏上下文缓存目录中。必须让我在 IDE 里能随时看见、随时修改。

4. **严格落实 PRD 场景测试全闭环 (Test & Documentation Loop)**：
   - 每完成一个小任务/小章节，**必须输出对应的测试用例场景文档**，并按阶段独立存放到 `docs/test/` 目录中保管（例如 `docs/test/Epic_1.4_TestCases.md`）。
   - **严禁删除或覆盖以往的测试记录文档**！每个测试用例都必须存档留存，以作回归依据。
   - 在写测试代码前，必须**根据《需求设计文档.md (PRD)》深入思考并穷举所有的业务场景和边界**。
   - 根据推导出的场景，生成真实的代码测试用例并全部执行。**所有的用例跑到 Pass（绿灯通过）之后，才允许进入下一个开发阶段**。
   - 🌟 **核心要求：跨章节链路整合测试 (End-to-End Linkage tests)**：每当完成一个 Epic 小节，必须追加一项“联动测试”，验证*前一个 Epic* 产出的数据或单例能够被*后一个 Epic* 成功拾取并打通（例如 Epic 1 的设置被 Epic 2 的生成器使用）。必须确保链路双向畅通无阻，避免组件孤岛！

5. **主动执行与自动运转 (Auto-Run Mode)**：
   - 授权后，能够主动去跑编译、跑测试、修 Bug，遇到明确需要环境调整的脚本（在安全范围内）应当自行执行，减少不必要的“请求授权”中断，保持高心流的开发节奏。

6. **严格坚持测试驱动 (TDD)**：
   - 除非特殊情况或纯 UI 探索，核心业务逻辑必须先写出错的测试（RED），再写真正的代码使其通过（GREEN）。

7. **自动化交接与状态同步 (Auto-Handover Context)**：
   - 每完成一个任务、回答一个请求或结束当前会话前，**必须同步更新 `docs/Agent_Handover_Context(项目交接快照).md`**。
   - 内容须涵盖：当前环境变量、已解决的坑点、核心架构决策、以及精确到 Epic/TC 级别的断点信息。
   - 目标：确保任何新的 AI Agent 载入项目后，只需阅读该文档即可立即无缝对接工作！

8. **Agent 可读性优先 (Agent Legibility First)**：
   - 代码库和文档的首要受众是 AI Agent，必须强调上下文显式化，避免使用模糊指代，并在代码注释和文档中清晰描述意图。
   - 倾向于使用格式化、结构化的表达（如明确的列表、表格、标签等），以降低 AI 解析信息的成本和产生幻觉的概率。

9. **代码库即唯一真实数据源 (Repository as System of Record)**：
   - 所有项目架构决策（ADR）、API 契约、设计讨论、Prompt 模板等，必须固化为 `docs/` 下受版本控制的纯文本文件。
   - 拒绝隐式知识，不存在于代码库且无法被检索到的信息，对 Agent 而言皆视为不存在。

10. **严格的架构与边界约束 (Architectural Constraints)**：
    - 将人类的架构意图转化为机器可强制执行的 Linter 规则、类型检查配置或自动化测试脚本。
    - AI 提交代码前必须确保所有“硬约束”全部通过；**严禁**为了通过测试而擅自修改测试用例本身（除非明确获得关于需求变更的授权）。

11. **定期垃圾回收与代码重构 (Garbage Collection Agent Tasks)**：
    - 考虑到 Agent 生成代码易产生冗余与技术债，在每个核心阶段或 Epic 结束后，必须执行专项的“重构与清理”。
    - AI 需在此环节专项审视全局代码，消除重复模式（DRY 原则），优化过度复杂的逻辑提取子函数，并清理废弃内容。

12. **基于 UI 的可视验证与测试 (UI-Driven E2E & Visual Testing)**：
    - Agent 必须首选使用自带的 `browser_subagent` 工具，通过查看和交互实际呈现的 UI 界面来验证功能和排查问题。
    - 针对包含页面的场景，不要仅仅依赖于对页面源码或后端逻辑的静态分析，应当遵循“眼见为实”的原则，模拟真实用户的视角从 UI 层面进行功能闭环验证。
    - **UI 截图测试准则 (Visual Regression Tests)**：
      - 在移动端开发中，优先使用 `Robolectric` + `Compose UI Test` 进行逻辑验证。
      - **Headless 环境兼容性**：如果 `captureToImage()` 超时，必须切换至 `@GraphicsMode(LEGACY)` 并使用 Espresso 的 `View.draw(Canvas)` 进行手动捕捉（详见 `docs/test/Agent_UI_Testing_SOP.md`）。
      - **绝对路径原则**：截图输出路径必须使用项目根目录相关的绝对路径（如 `d:/project/gym_agent/docs/test/...`）。
      - **本地化与 Mock**：测试用例应匹配 App 实际的本地化语言（如中文），并显式 Mock ViewModel 的 `StateFlow` 以控制 UI 状态。
    - **执行与留痕**：在进行 UI 测试时，Agent 需要保留页面截图或 DOM 关键节点状态作为依据；如果是需要被反复运行的回归测试边界，则应当在此基础上生成并维护自动化的测试脚本（参考 `docs/test/Agent_UI_Testing_SOP.md`）。

13. **任务完结强制自检清单 (Mandatory Pre-Completion Checklist)**：
    - **在此提醒：AI 助手严禁在未检查以下清单的情况下宣布任务完成！**
    - [ ] **SOP 对齐**：是否阅读并对齐了 `docs/项目流程优化规范(SOP).md` 中的设计、执行与测试要求？
- [ ] **用户打磨提醒**：是否已主动提醒用户参与本轮任务的设计/计划打磨？
- [ ] **优先查阅**：在遇到问题时，是否第一时间查阅了 `Troubleshooting Log` 和 `Handover Context` 而非盲目搜索？
    - [ ] **踩坑同步**：是否在 `docs/踩坑与反思记录(Troubleshooting Log).md` 中记录了本次任务遇到的非预期报错或架构坑点？
    - [ ] **进度更新**：是否在 `docs/任务追踪(Task).md` 中勾选了已完成的 Sub-task 和 Epic？
    - [ ] **快照交接**：是否在 `docs/Agent_Handover_Context(项目交接快照).md` 中更新了当前最新的代码断点、环境变量和测试凭据？
    - [ ] **测试存档**：是否在 `docs/test/` 目录下创建或更新了对应的测试场景文档，并确保 100% Pass？
    - [ ] **代码同步**：是否已执行 `git add`, `git commit` 并成功 `git push` 到远程仓库？

---
*(您可以随时修改上述要求，我将在每次执行前对齐您的最新意志)*
