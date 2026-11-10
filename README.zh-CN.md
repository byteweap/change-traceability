# 变更追溯

[English documentation](README.md)

一个用于维护软件重大变更业务文档追溯的 AI Skill，能够适配项目已有的文档规范和协作流程。它帮助 AI Agent 判断一次变更是否需要留档、需要多大程度的留档，并持续保持需求、决策、实现、验证、偏差、风险和剩余工作的记录与实际结果一致。

## 安装

将下面的内容粘贴到你的 Coding Agent：

```text
Install the change-traceability skill from https://github.com/byteweap/change-traceability
for me by running: npx skills add byteweap/change-traceability -g
```

也可以直接在终端执行：

```bash
npx skills add byteweap/change-traceability -g
```

CLI 会检测已安装的 Coding Agent，并将 Skill 安装到它们共享的全局 skills 目录。去掉 `-g` 可改为安装到当前项目。也可以手动复制或链接本仓库到 Agent 的 skills 目录，并确保 Skill 根目录下存在 `SKILL.md`。

## 使用教程

安装完成后，显式调用 Skill：

```text
$change-traceability
```

然后说明变更目标、约束条件，以及已有的 Issue 或验收标准。例如：

```text
使用 $change-traceability 实现失败支付重试功能。
先检查项目现有的文档约定，记录需求和技术决策，完成实现并执行相关检查，
最后报告验证证据、剩余风险和未执行的检查。
```

Skill 会先检查项目规则和文档结构，再复用已有的 `docs/`、ADR、RFC、Issue 或 Pull Request 约定。它会自动选择最低但充分的追溯级别，也可以明确指定 Level 0、Level 1、Level 2 或 ADR。

对于实质性变更，可以要求它在实施过程中持续同步记录：

```text
请在工作过程中持续同步追溯记录。更新进度、假设、偏差、决策和验证结果；
没有实际执行的检查不要标记为通过。
```

交付前确认最终报告包含追溯级别及理由、记录位置、实现变化、验收与验证证据、未执行检查、偏差、风险、剩余工作，并将代码审查状态与测试结果和业务验收分开说明。


## 为什么需要它

AI 辅助开发可能快速产出可运行的代码，却没有保留变更原因、明确的非目标、使用的假设、未验证的内容以及剩余风险。本 Skill 将追溯变成一种按风险分级、可重复执行的工作流，同时不会强迫所有项目采用同一套文档系统。

## 核心能力

- 按变更风险判断追溯要求。
- 支持 Level 0、Level 1、Level 2 和可选 ADR 记录。
- 发现并复用目标项目已有的文档约定。
- 指导需求、进度、决策、验收证据、偏差和风险记录。
- 明确暴露假设和范围变化。
- 区分验证证据、代码审查状态和业务验收。
- 在最终交付中明确报告文档位置、未执行检查和剩余风险。

## 不负责什么

本 Skill 不是：

- 项目管理系统；
- 产品需求或业务审批的替代品；
- 代码审查、安全审计或测试 Skill；
- 可以自行编造需求或验证结果的工具；
- 要求所有细小改动都创建文档的强制流程；
- 可以覆盖已有项目约定、另起一套文档体系的授权。

## 追溯模型

| 级别 | 适用场景 | 典型记录 |
|---|---|---|
| **Level 0** | 格式化、注释、错别字或简单的不改变行为的修改 | 不新增记录，但在最终报告中说明原因 |
| **Level 1** | 局部 Bug 修复或小范围内部行为调整 | 轻量变更记录 |
| **Level 2** | 新功能、流程或状态变化、公共接口、协议、数据、安全、迁移、跨模块或发布关键变更 | 完整业务追溯 |
| **ADR** | 技术方案存在多个可行选项，或具有较大影响、回滚条件 | ADR 风格的决策记录，可与 Level 1 或 Level 2 同时使用 |

如果同时满足多个条件，采用最高级别。用户明确要求留档时，最低使用 Level 1；用户要求完整追溯时，使用 Level 2。

## 文档工作流

1. 分类当前变更。
2. 选择最低但充分的追溯级别。
3. 检查项目规则、已有记录、模板以及 Issue/PR 约定。
4. 在实施前或实施过程中创建、更新相应记录。
5. 持续更新范围、假设、决策、进度、偏差、风险和剩余工作。
6. 将验收标准映射到具体验证证据。
7. 交付前检查文档是否完整、最新且没有未经验证的结论。
8. 报告文档位置、验证证据、限制和风险。

追溯是持续维护的记录，不是编码结束后才补写的一篇总结。

## 持续维护与交接

- 范围或验收标准变化时，先更新需求和变更记录，再实现调整后的行为；在有意义的阶段完成或重要验证结果变化时更新进度，不记录每次工具调用。
- 暂停或移交前，记录当前阶段、已验证范围、阻塞、相关链接和具体下一步，让接手者能依据记录和代码继续工作。
- 轻量记录同样包含目标、范围、明确的验收标准、实施进度和验证证据。根因分析用于缺陷修复，其他小范围行为调整无需编造根因。
- 复用已有文档索引：创建记录时增加入口，交付时同步结果，重新开启工作时修正过时的完成状态。详细进度保留在变更记录中，项目没有索引时不强制创建。

详细规则见[记录维护](SKILL.md#record-maintenance)、[进度模板](references/progress-template.md)、[轻量模板](references/lightweight-template.md)和[索引维护](references/documentation-discovery.md#existing-indexes)。这些改进保留原有目录优先级和基于风险的追溯分级。

## 文档语言规则

Skill 自带默认模板使用英文，以便跨项目复用。生成文档时按以下顺序确定语言：

1. 用户明确指定的语言；
2. 项目明确规定的文档语言；
3. 正在更新的已有文档语言；
4. 用户当前任务使用的语言。

模板只规定内容结构，不强制使用英文。除非用户或项目要求，不主动生成双语文档。

## 跨项目使用

本 Skill 可用于不同项目，但不要求所有项目采用相同的文档结构。它复用的是变更追溯方法，而不是强制复用文档路径、模板、术语、语言或协作流程。

## 项目适配

目标项目的规则、路径、模板、术语和协作流程优先级最高。创建文件前，应检查项目规则、`docs/`、`doc/`、`adr/`、`design/`、RFC、类似变更记录、PR 模板和 Issue 模板。如果项目没有既有约定，再使用 `references/` 中最小适用的默认模板。

## 自动创建主题子目录

对于没有适用项目约定的新主题，Skill 会指导 Agent 在写入记录前创建 `<docs-root>/<change-slug>/`。优先复用项目已有的文档根目录（例如 `doc/`）；没有文档根目录时才使用 `docs/`。仅有少量平铺的 Markdown 文件，不代表后续记录必须继续平铺。

```text
docs/
├── quick-match-waiting/
│   ├── requirements.md
│   ├── progress.md
│   └── decision.md       # 按需添加的 ADR
└── dev-rollout-capacity-fix/
    └── change.md
```

- Level 0 不创建目录或记录。
- Level 1 使用 `change.md`；Level 2 使用 `requirements.md` 和 `progress.md`。
- 需要决策记录时才添加 `decision.md`；已有 ADR 目录时沿用该位置。
- 同一功能继续实施或进行关联小修复时，若范围、历史和证据仍能清楚记录，则复用原主题；需要独立追踪目标、验收或回滚要求的变更单独建档，并链接相关功能记录。
- 复用主题不降低追溯级别，也不代表历史验证结果适用于新行为；不同主题重名时，使用 Issue 标识或描述性后缀区分。
- 历史平铺文件保留原位置，同一变更继续原地更新；迁移需要单独提出。
- 项目明确要求平铺文件或使用外部 Issue/PR 等流程时，优先遵守项目规则。
- 已有 `docs/features/` 等分类结构和 `decisions.md` 等文件名继续沿用；默认结构不在文档根目录与主题之间增加分类层。

Agent 会创建缺失的父目录、写入实际内容，并检查生成文件及本地链接。创建失败时必须报告路径和原因，不能静默退回平铺文件。本方案通过技能指令实现，不新增初始化脚本。完整规则及落位检查场景见[文档发现与落位](references/documentation-discovery.md)。

## 仓库结构

- `SKILL.md`：供 AI Agent 运行时读取的主要规则。
- `agents/openai.yaml`：可选的 OpenAI 界面元数据。
- `references/`：级别说明、文档发现规则、默认模板和证据格式。

## 验证

如果环境提供 Skill Creator 校验脚本，可以执行：

```bash
python3 /path/to/skill-creator/scripts/quick_validate.py /path/to/change-traceability
```

同时检查空白字符和仓库变更：

```bash
git diff --check
```

校验脚本只能检查打包结构和 frontmatter，不能判断追溯内容是否真正完整。

## 参考资料

- [追溯级别](references/traceability-levels.md)
- [文档发现与落位](references/documentation-discovery.md)
- [轻量追溯模板](references/lightweight-template.md)
- [完整需求模板](references/requirements-template.md)
- [进度模板](references/progress-template.md)
- [决策模板](references/decision-template.md)
- [验收证据](references/acceptance-evidence.md)
- [偏差与风险](references/deviation-and-risk.md)

## 许可证

本项目采用 [Apache License 2.0](LICENSE) 开源许可证。
