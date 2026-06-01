# 面向 AI 协作的项目工作空间：一个关于下一代软件工程目录结构的开放提案

## 背景

传统软件开发通常围绕“人”来组织协作。

一个项目往往由产品、设计、前端、后端、测试、运维等角色共同完成。每个角色使用自己的工具、文档和工作方式：

- 产品维护 PRD、业务规则、会议纪要、需求变更记录；
- 设计维护原型、交互说明、视觉稿、流程图；
- 前端维护页面、组件、状态管理、接口调用；
- 后端维护服务、接口、领域模型、数据库和任务；
- 测试维护测试计划、测试用例、缺陷复盘、造数说明和自动化测试。

在传统模式下，仓库通常只是“代码仓库”。

大量产品背景、业务约束、历史决策、测试思路和设计细节分散在文档工具、聊天记录、会议纪要、网盘、Issue、Wiki 或人的脑子里。

这在纯人工协作时代还能勉强运转，但在 AI coding agent 时代会暴露出新的问题：

1. AI 只能看到代码，却看不到完整业务上下文；
2. AI 容易误读过期资料、历史方案或草稿；
3. AI 经常不知道哪些需求、测试、接口、页面之间有关联；
4. 不同角色反复向 AI 补充背景，造成重复沟通和上下文浪费；
5. 产品、测试、设计如果被迫改用 Markdown 或工程化格式，会增加很大组织阻力；
6. AI coding agent 往往更偏向开发角色，而忽略产品、设计、测试等角色同样需要 AI 上下文。

因此，我提出一个开放问题：

> 在 AI 参与软件开发越来越深入的时代，项目仓库是否应该从“代码仓库”升级为“面向 AI 协作的项目工作空间”？

---

## 核心想法

这个项目工作空间不是简单地把所有资料塞进一个大仓库。

它的核心思想是：

> 人类继续用自己习惯的方式维护资料；\
> AI 不直接消费混乱的人类资料；\
> 通过 Context Compiler 将产品、设计、测试、代码等资料编译成 AI 可读的结构化上下文；\
> 不同角色的 AI Agent 基于各自的角色视图进行协作；\
> Claude Code、Codex、Cursor 等 AI Agent 基于这些上下文、项目级 Skill、MCP 和 Agent 规则进行工作。

换句话说：

```text
产品资料
设计资料
测试资料
前端代码
后端代码
自动化测试
历史缺陷
运行日志
   ↓
Context Compiler
   ↓
/工作区/AI/领域上下文
/工作区/AI/角色视图
   ↓
产品 Agent / 设计 Agent / 前端 Agent / 后端 Agent / 测试 Agent / Reviewer Agent
   ↓
需求分析、设计校验、编码、测试、评审、交付
```

这个模式的目标不是替代产品、设计、测试或开发，而是减少低效的上下文转述，让 AI 更可靠地参与多角色工程协作。

---

## 一句话定义

> 面向 AI 协作的项目工作空间，是一种以项目目标为边界、以角色视图为入口、以 Context Compiler 为转换层的协作结构。它保留产品、设计、开发、测试等角色的原生工作方式，同时将各角色资料和工程实现编译成适合不同角色 Agent 使用的 AI 上下文。

---

## 核心不是“让其他角色服务开发”

这个提案很容易被误解成：

> 产品、设计、测试把资料整理好，是为了让开发 Agent 更好写代码。

这只是其中一个场景，但不是全部。

更完整的理解是：

> 每个角色都维护自己的原始资料；\
> 每个角色也都可以使用 AI 来理解其他角色的工作成果；\
> Context Compiler 负责把其他角色的信息编译成当前角色可理解、可行动的 AI 上下文视图。

例如：

### 当开发角色使用 AI 时

产品、设计、测试资料会被编译成有助于编码的上下文：

```text
/工作区/AI/角色视图/for-frontend.generated.md
/工作区/AI/角色视图/for-backend.generated.md
```

前端 Agent 需要理解：

- 当前需求；
- 页面流程；
- 交互规则；
- API 契约；
- 测试验收标准；
- 组件复用建议。

后端 Agent 需要理解：

- 业务规则；
- 领域模型；
- API 契约；
- 前端调用方；
- 幂等性和一致性要求；
- 测试风险和验收点。

### 当产品角色使用 AI 时

前端、后端、设计、测试等模块也应该被编译成有助于产品理解项目状态的上下文：

```text
/工作区/AI/角色视图/for-product.generated.md
```

产品 Agent 可以回答：

- 哪些需求已经实现？
- 哪些需求仍未覆盖？
- 当前代码中有哪些页面、接口、数据模型与某个需求相关？
- 测试是否覆盖了关键验收标准？
- 最近代码变更可能影响哪些产品能力？
- 当前实现和 PRD 是否存在不一致？
- code graph / service graph / page graph 如何映射到产品语义？

### 当设计角色使用 AI 时

设计 Agent 可以通过前端代码、页面图谱、组件图谱、产品上下文理解：

- 当前页面实现状态；
- 哪些组件已经存在；
- 哪些交互已经实现；
- 设计稿和代码实现是否存在差异；
- 哪些设计变更会影响前端、后端或测试；
- 某个用户流程是否被完整支持。

### 当测试角色使用 AI 时

测试 Agent 可以通过产品、设计、前端、后端和历史缺陷上下文理解：

- 某个需求的验收标准；
- 哪些页面流程需要回归；
- 哪些 API 和服务被影响；
- 哪些历史缺陷可能复发；
- 哪些需求没有自动化测试覆盖；
- 如何生成测试数据；
- 本次代码变更的风险矩阵。

因此，AI 上下文不是单向的：

```text
产品 / 设计 / 测试 → 开发
```

而是多向的：

```text
产品 ↔ 设计 ↔ 前端 ↔ 后端 ↔ 测试
        ↓
   Context Compiler
        ↓
   Role-specific AI Context
```

---

## 设计原则

### 1. 人类资料保持原生形态

产品、设计、测试等人工维护目录不限制格式。

它们可以是：

- Markdown
- Word
- PDF
- PPT
- Excel
- XMind
- 图片
- Figma 导出物
- 流程图
- 接口集合
- HAR 抓包
- 日志
- 会议纪要
- 缺陷复盘

原因很简单：

> 不能为了让 AI 舒服，而强迫整个组织改变已有工作习惯。

AI-native 工作空间应该降低协作成本，而不是增加产品、设计、测试同学的负担。

---

### 2. AI 默认读取编译后的上下文

普通 AI Agent 默认不直接读取人工资料目录。

例如，普通开发 Agent 不应该直接读取：

```text
../产品
../设计
../测试
```

它应该优先读取：

```text
工作区/AI
```

只有以下情况才允许回溯人工资料：

1. 用户明确要求查看原始资料；
2. `/AI` 上下文缺失或冲突；
3. 当前任务是生成或修复 AI 上下文；
4. 当前 Agent 是 Context Compiler Agent；
5. 人工审核要求追溯来源。

---

### 3. AI 上下文是编译产物，不是人工维护资料

`/工作区/AI` 下的内容由工具生成，默认不手工修改。

例如：

```text
/工作区/AI/产品/requirements.generated.md
/工作区/AI/测试/acceptance-map.generated.md
/工作区/AI/后端/api-contracts.generated.md
/工作区/AI/角色视图/for-backend.generated.md
/工作区/AI/角色视图/for-product.generated.md
```

如果生成内容有错，应该回到人工资料或 Context Compiler 规则中修正，然后重新生成，而不是直接改 generated 文件。

---

### 4. 领域上下文和角色视图要分开

`/AI` 目录至少应该包含两类内容：

```text
领域上下文：某个领域当前是什么状态？
角色视图：某个角色为了完成自己的工作，需要从其他领域理解什么？
```

例如：

```text
/工作区/AI/产品
/工作区/AI/设计
/工作区/AI/前端
/工作区/AI/后端
/工作区/AI/测试
```

这些是领域上下文。

```text
/工作区/AI/角色视图/for-product.generated.md
/工作区/AI/角色视图/for-designer.generated.md
/工作区/AI/角色视图/for-frontend.generated.md
/工作区/AI/角色视图/for-backend.generated.md
/工作区/AI/角色视图/for-test.generated.md
/工作区/AI/角色视图/for-reviewer.generated.md
```

这些是角色视图。

领域上下文回答：

> 产品、设计、前端、后端、测试各自当前是什么状态？

角色视图回答：

> 某个角色为了完成自己的工作，需要理解其他领域的哪些内容？

---

### 5. 尽量使用官方支持的 AI 工具机制

这个工作空间不希望发明一套完全孤立的插件系统。

因此，Claude Code、Codex 等工具已经支持的项目级能力，应该尽量按官方方式使用：

- Claude Code：

  - `CLAUDE.md`
  - `.claude/settings.json`
  - `.claude/skills/*/SKILL.md`
  - `.claude/agents/*.md`
  - `.claude/commands/*.md`
  - `.mcp.json`

- Codex：

  - `AGENTS.md`
  - `.codex/config.toml`
  - `.codex/agents/*.toml`
  - `.agents/skills/*/SKILL.md`

`/AI` 只负责存放生成给 AI 使用的上下文，不负责替代官方插件、Skill、MCP 或 Agent 机制。

---

### 6. Skill 是方法，MCP 是工具，AI 上下文是资料

这三者需要清晰分开：

| 类型       | 作用                                      | 推荐位置                                           |
| -------- | --------------------------------------- | ---------------------------------------------- |
| AI 上下文   | 告诉 Agent 当前项目是什么、有哪些需求、接口、测试、风险         | `/工作区/AI`                                      |
| 角色视图     | 告诉某个角色应该如何理解其他角色的信息                     | `/工作区/AI/角色视图`                                 |
| Skill    | 告诉 Agent 某类任务应该怎么做                      | `.claude/skills`、`.agents/skills`              |
| MCP      | 给 Agent 提供可调用工具，如文档查询、代码搜索、GitLab、上下文编译 | `.mcp.json`、`.codex/config.toml`、`scripts/mcp` |
| Subagent | 定义特定职责的 Agent，如后端实现、测试分析、上下文编译          | `.claude/agents`、`.codex/agents`               |

---

## 当前建议目录结构

下面是当前提案中的一个初始目录结构。

它不是最终答案，而是一个供社区讨论的起点。

```text
项目根/
  README.md

  /产品
    /需求文档
    /业务规则
    /用户调研
    /会议纪要
    /竞品分析
    /原型截图
    /历史归档

  /设计
    /原型
    /交互说明
    /视觉规范
    /流程图
    /页面截图
    /历史归档

  /测试
    /测试计划
    /测试设计
    /测试用例
    /缺陷复盘
    /造数说明
    /环境说明
    /接口集合
    /抓包记录
    /历史归档

  /工作区
    README.md

    AGENTS.md
    CLAUDE.md
    CODEX.md

    .mcp.json

    /.claude
      settings.json
      settings.local.json

      /skills
        /context-reading
          SKILL.md
        /impact-analysis
          SKILL.md
        /context-compiler
          SKILL.md
        /requirement-change-sync
          SKILL.md
        /frontend-page-implementation
          SKILL.md
        /backend-api-implementation
          SKILL.md
        /regression-scope-analysis
          SKILL.md
        /mr-review
          SKILL.md
        /security-review
          SKILL.md

      /agents
        product-context-compiler.md
        design-context-compiler.md
        test-context-compiler.md
        frontend-implementer.md
        backend-implementer.md
        reviewer.md

      /commands
        generate-ai-context.md
        review-mr.md
        analyze-impact.md

    /.codex
      config.toml

      /agents
        product-context-compiler.toml
        design-context-compiler.toml
        test-context-compiler.toml
        frontend-implementer.toml
        backend-implementer.toml
        reviewer.toml

    /.agents
      /skills
        /context-reading
          SKILL.md
        /impact-analysis
          SKILL.md
        /context-compiler
          SKILL.md
        /requirement-change-sync
          SKILL.md
        /frontend-page-implementation
          SKILL.md
        /backend-api-implementation
          SKILL.md
        /regression-scope-analysis
          SKILL.md
        /mr-review
          SKILL.md
        /security-review
          SKILL.md

    /AI
      /全局
        workspace-index.generated.md
        source-manifest.generated.json
        traceability.generated.md
        open-questions.generated.md
        deprecated-context.generated.md
        context-health.generated.md

      /产品
        context.generated.md
        requirements.generated.md
        business-rules.generated.md
        glossary.generated.md
        product-decisions.generated.md
        open-questions.generated.md

      /设计
        context.generated.md
        page-flows.generated.md
        interaction-rules.generated.md
        component-map.generated.md
        design-decisions.generated.md
        open-questions.generated.md

      /前端
        context.generated.md
        page-map.generated.md
        component-map.generated.md
        route-map.generated.md
        state-management.generated.md
        api-usage.generated.md
        frontend-risks.generated.md
        code-graph.generated.json
        dependency-map.generated.md

      /后端
        context.generated.md
        architecture.generated.md
        api-contracts.generated.md
        domain-model.generated.md
        database-map.generated.md
        service-map.generated.md
        backend-risks.generated.md
        code-graph.generated.json
        dependency-map.generated.md

      /测试
        context.generated.md
        acceptance-map.generated.md
        risk-matrix.generated.md
        test-cases.generated.md
        test-data-guide.generated.md
        regression-scope.generated.md
        known-bugs.generated.md

      /角色视图
        for-product.generated.md
        for-designer.generated.md
        for-frontend.generated.md
        for-backend.generated.md
        for-test.generated.md
        for-reviewer.generated.md

      /报告
        2026-05-29-上下文生成报告.md

    /前端
      /web
      /admin
      /mobile
      /shared-ui

    /后端
      /services
      /api-gateway
      /jobs
      /shared

    /testing
      /integration
      /e2e
      /contract
      /fixtures
      /data-factory

    /scripts
      /context-compiler
        package.json
        src/

      /mcp
        /context-compiler-mcp
        /code-search-mcp
        /gitlab-mcp

      generate-ai-context.ts
      validate-context.ts
      check-traceability.ts
      sync-skills.ts

    /infra
      docker-compose.yml
      k8s/
      terraform/

    /.context-cache
      /产品
      /设计
      /测试
      /前端
      /后端
```

---

## 为什么把 `/工作区` 单独拿出来？

这个设计中，Claude Code、Codex 等 AI coding agent 默认打开：

```text
项目根/工作区
```

而不是打开项目根目录。

这样做的原因是：

1. 防止普通 Agent 默认扫描外层人工资料；
2. 把 AI 运行环境和工程源码放在一个明确边界内；
3. 让 `/产品`、`/设计`、`/测试` 成为 Context Compiler 的输入，而不是普通 Agent 的直接上下文；
4. 更容易配置 Claude/Codex 的项目级规则、MCP、Skills 和 Subagents；
5. 更容易在未来支持不同 AI 工具接入。

整体关系如下：

```text
项目根/
  /产品      人类维护的产品资料
  /设计      人类维护的设计资料
  /测试      人类维护的测试资料

  /工作区    Claude / Codex / Cursor 等 AI Agent 的工作目录
    /AI      编译后的 AI 上下文
    /前端    前端源码
    /后端    后端源码
```

---

## `/AI` 目录的作用

`/工作区/AI` 是多角色 AI 上下文目录。

它不是人工资料目录，也不是只给开发 Agent 使用的目录。

它应该由 Context Compiler 从产品、设计、前端、后端、测试等资料中生成，服务不同角色的 AI 协作需求。

例如：

- 产品角色使用 `/AI/角色视图/for-product.generated.md` 理解当前代码实现、测试覆盖和需求落地状态；
- 设计角色使用 `/AI/角色视图/for-designer.generated.md` 理解页面、组件、交互实现和设计差异；
- 前端角色使用 `/AI/角色视图/for-frontend.generated.md` 理解需求、设计、接口和测试验收；
- 后端角色使用 `/AI/角色视图/for-backend.generated.md` 理解业务规则、接口契约、前端调用方和测试风险；
- 测试角色使用 `/AI/角色视图/for-test.generated.md` 理解产品验收、设计流程、前后端实现和回归范围；
- Reviewer 角色使用 `/AI/角色视图/for-reviewer.generated.md` 进行 MR 风险评估、测试覆盖检查和需求一致性检查。

因此，`/AI` 目录包含两类核心内容：

1. 领域上下文，例如 `/AI/产品`、`/AI/设计`、`/AI/前端`、`/AI/后端`、`/AI/测试`；
2. 角色视图，例如 `/AI/角色视图/for-product.generated.md`、`for-backend.generated.md`、`for-test.generated.md`。

领域上下文回答：

> 某个领域当前是什么状态？

角色视图回答：

> 某个角色为了完成自己的工作，需要从其他领域理解什么？

---

## 角色视图示例

### `/AI/角色视图/for-product.generated.md`

给产品角色使用。

来源可以包括：

- `/AI/前端`
- `/AI/后端`
- `/AI/测试`
- `/AI/设计`
- code graph
- service graph
- API graph
- test coverage graph

内容可以包括：

- 需求实现状态；
- 页面与需求映射；
- 接口与需求映射；
- 当前能力边界；
- 已知技术约束；
- 测试覆盖情况；
- 最近变更影响的产品能力；
- 未实现或实现不一致的需求；
- 代码结构如何映射到产品能力。

### `/AI/角色视图/for-designer.generated.md`

给设计角色使用。

来源可以包括：

- `/AI/产品`
- `/AI/前端`
- `/AI/测试`
- 页面图谱
- 组件图谱

内容可以包括：

- 页面清单；
- 用户流程；
- 组件复用情况；
- 当前交互实现约束；
- 已上线页面状态；
- 设计稿与实现差异；
- 设计变更可能影响的前端模块和测试范围。

### `/AI/角色视图/for-frontend.generated.md`

给前端角色使用。

来源可以包括：

- `/AI/产品`
- `/AI/设计`
- `/AI/后端`
- `/AI/测试`

内容可以包括：

- 当前需求；
- 页面流程；
- 交互规则；
- API 契约；
- 验收标准；
- 相关测试；
- 组件复用建议；
- UI 风险点。

### `/AI/角色视图/for-backend.generated.md`

给后端角色使用。

来源可以包括：

- `/AI/产品`
- `/AI/前端`
- `/AI/测试`
- service graph
- database graph

内容可以包括：

- 业务规则；
- 领域模型；
- API 契约；
- 前端调用方；
- 幂等性和一致性要求；
- 数据库影响；
- 测试验收点；
- 风险边界。

### `/AI/角色视图/for-test.generated.md`

给测试角色使用。

来源可以包括：

- `/AI/产品`
- `/AI/设计`
- `/AI/前端`
- `/AI/后端`
- 历史缺陷
- code graph

内容可以包括：

- 验收标准；
- 页面路径；
- API 流程；
- 风险矩阵；
- 测试数据建议；
- 回归范围；
- 代码变更影响面；
- 历史缺陷复发风险。

### `/AI/角色视图/for-reviewer.generated.md`

给 Reviewer 使用。

来源可以包括：

- `/AI/产品`
- `/AI/设计`
- `/AI/前端`
- `/AI/后端`
- `/AI/测试`
- MR diff
- code graph
- test coverage

内容可以包括：

- 本次变更关联需求；
- 受影响页面、接口、服务、测试；
- 缺少测试覆盖的风险；
- 需求和实现不一致之处；
- 代码复杂度或安全风险；
- 是否违反 generated 文件规则；
- 是否误读或越权读取人工资料。

---

## Context Compiler 是什么？

Context Compiler 是这个提案中的关键组件。

它负责读取人工资料目录：

```text
../产品
../设计
../测试
```

以及必要时读取工程源码：

```text
/前端
/后端
/testing
```

然后生成：

```text
/工作区/AI
```

它应该具备以下能力：

1. 多格式解析：PDF、PPT、Word、Excel、XMind、图片、Markdown、HTML、JSON；
2. OCR 和截图理解；
3. 需求抽取；
4. 业务规则抽取；
5. 术语抽取；
6. 设计流程和页面关系抽取；
7. 测试点和测试用例抽取；
8. 代码结构扫描；
9. code graph / dependency graph / service graph 生成；
10. 接口契约生成；
11. 冲突检测；
12. 过期信息识别；
13. 追踪关系生成；
14. 按角色生成不同 AI 视图；
15. 输出生成报告，供人审核。

它的定位类似：

```text
人工资料 + 工程实现 = 源码
Context Compiler = 编译器
/AI = 编译产物
Agent = 运行时消费者
```

---

## 推荐的 Agent 访问规则

普通 Agent 默认允许读取：

```text
/AI
/前端
/后端
/testing
/scripts
/infra
```

普通 Agent 默认禁止读取：

```text
../产品
../设计
../测试
../**/历史归档/**
```

原因是人工资料中可能包含：

- 历史方案；
- 草稿；
- 会议临时结论；
- 过期设计；
- 未确认需求；
- 未结构化描述；
- 敏感信息；
- 不适合直接给 AI 消费的上下文。

只有 Context Compiler Agent 或明确授权任务可以读取人工资料目录。

---

## 推荐接入的 AI 工具能力

这个工作空间可以接入当前 AI coding 生态中的几类能力。

### 1. Project Skills

项目级 Skill 用于沉淀“如何完成某类任务”。

例如：

```text
context-reading
impact-analysis
context-compiler
requirement-change-sync
frontend-page-implementation
backend-api-implementation
regression-scope-analysis
mr-review
security-review
```

Skill 不是上下文资料，而是任务方法。

例如 `backend-api-implementation` 应该告诉 Agent：

1. 先读哪些 AI 上下文；
2. 如何确认需求 ID；
3. 如何查接口契约；
4. 如何修改后端代码；
5. 如何补测试；
6. 如何输出风险和待确认问题。

而 `requirement-change-sync` 应该告诉产品 Agent：

1. 如何读取 `for-product.generated.md`；
2. 如何理解当前代码实现状态；
3. 如何判断需求是否已有对应页面、接口和测试；
4. 如何提出需求变更的影响范围；
5. 如何生成待开发、待设计、待测试的事项。

---

### 2. MCP Servers

MCP 用于给 Agent 提供工具能力，例如：

- 最新技术文档查询；
- 代码语义搜索；
- GitLab MR 查询和评审；
- 项目上下文编译；
- 影响分析；
- 测试数据生成。

推荐接入的 MCP 类型：

```text
context7             查询最新库文档
code-search          语义代码搜索
context-compiler     编译产品/设计/测试/代码到 /AI
gitlab               查询 MR、Issue、Pipeline
```

---

### 3. Code Graph / Knowledge Graph

AI Agent 不应该每次都全仓库扫描。

更合理的是生成代码图谱和项目图谱，例如：

```text
/AI/前端/component-map.generated.md
/AI/前端/route-map.generated.md
/AI/后端/service-map.generated.md
/AI/后端/domain-model.generated.md
/AI/全局/traceability.generated.md
```

这能让不同角色的 Agent 更快理解项目结构和影响范围。

例如：

- 开发 Agent 用 code graph 理解实现位置；
- 产品 Agent 用 code graph 理解某个需求是否已经落地；
- 测试 Agent 用 code graph 判断回归范围；
- Reviewer Agent 用 code graph 检查 MR 影响面。

---

### 4. Multi-format Document Parsing

由于人工资料不限制格式，Context Compiler 必须支持多格式输入。

它可以集成：

- PDF parser
- DOCX parser
- PPTX parser
- XLSX parser
- XMind parser
- OCR
- 图片理解
- Markdown parser
- HTML parser

这部分能力决定了这个工作空间能否真正低成本落地。

---

## 一个典型工作流：开发角色

### 场景：产品修改了退款需求，后端需要实现

1. 产品同学更新：

```text
/产品/需求文档/退款流程需求说明.pdf
/产品/会议纪要/退款需求评审纪要.docx
```

2. 运行 Context Compiler：

```bash
cd 工作区
npm run ai:context
```

3. 工具生成：

```text
/工作区/AI/产品/requirements.generated.md
/工作区/AI/产品/business-rules.generated.md
/工作区/AI/测试/acceptance-map.generated.md
/工作区/AI/角色视图/for-backend.generated.md
/工作区/AI/报告/上下文生成报告.md
```

4. 后端 Agent 读取：

```text
/AI/角色视图/for-backend.generated.md
/AI/产品/requirements.generated.md
/AI/后端/api-contracts.generated.md
/AI/测试/acceptance-map.generated.md
```

5. 后端 Agent 修改代码：

```text
/后端
/testing
```

6. Reviewer Agent 根据 `/AI` 和代码变更进行 MR 评审。

---

## 一个典型工作流：产品角色

### 场景：产品想确认某个需求当前是否已经完整落地

1. Context Compiler 扫描代码、测试和设计上下文：

```text
/工作区/前端
/工作区/后端
/工作区/testing
/工作区/AI/设计
/工作区/AI/测试
```

2. 生成产品角色视图：

```text
/工作区/AI/角色视图/for-product.generated.md
```

3. 产品 Agent 可以基于这个角色视图回答：

```text
RQ-REFUND-001 部分退款是否已经实现？
哪些页面支持了它？
哪些接口支持了它？
哪些测试覆盖了它？
还有哪些缺口？
最近一个 MR 会不会影响这个需求？
```

4. 如果发现需求和实现不一致，产品可以发起：

```text
需求澄清
需求变更
设计补充
开发任务
测试补充
```

而不是通过会议反复追问每个角色。

---

## 一个典型工作流：测试角色

### 场景：测试需要判断某次代码变更的回归范围

1. Context Compiler 读取：

```text
/工作区/AI/产品
/工作区/AI/设计
/工作区/AI/前端
/工作区/AI/后端
/工作区/后端 code graph
/工作区/前端 code graph
/测试/缺陷复盘
```

2. 生成测试角色视图：

```text
/工作区/AI/角色视图/for-test.generated.md
/工作区/AI/测试/regression-scope.generated.md
/工作区/AI/测试/risk-matrix.generated.md
```

3. 测试 Agent 可以回答：

```text
这次改动影响哪些功能？
哪些页面需要回归？
哪些接口需要回归？
哪些历史缺陷可能复发？
哪些测试用例需要新增或更新？
是否缺少自动化测试？
```

---

## 这个提案想解决什么？

### 1. 减少跨角色重复沟通

以前很多会议本质上是同步上下文。

现在上下文可以被结构化沉淀、生成、追踪和审查。

---

### 2. 降低 AI coding 的幻觉和误读

Agent 不直接读混乱原始资料，而是读经过编译和校验的上下文。

---

### 3. 保留传统团队工作方式

产品、设计、测试不需要立刻改变自己的工作工具。

他们只需要把资料放到正确位置，并审核生成报告。

---

### 4. 让需求、设计、代码、测试可追踪

例如：

```text
RQ-REFUND-001
  → PAGE-REFUND-DETAIL
  → API-REFUND-CREATE
  → SERVICE-order-service
  → TC-REFUND-003
```

这种追踪关系非常适合 AI 使用，也非常适合 MR Review、回归分析和影响分析。

---

### 5. 为多 Agent 协作打基础

未来不同 Agent 可以承担不同职责：

- Product Context Compiler
- Design Context Compiler
- Test Context Compiler
- Frontend Implementer
- Backend Implementer
- Reviewer
- Security Reviewer
- Regression Analyzer
- Product Analyst
- Design Reviewer

它们不再凭临时 prompt 工作，而是基于统一工作空间、项目级 Skill、MCP 和 AI 上下文协同。

---

### 6. 让每个角色都能使用 AI 理解其他角色的成果

产品不需要读代码也能理解实现状态。

测试不需要逐个问开发也能理解变更影响。

设计不需要翻源码也能理解组件和页面实现。

开发不需要反复开会也能理解需求、设计和测试验收。

这才是面向 AI 协作的项目工作空间最重要的价值。

---

## 当前仍然不确定的问题

这个提案还不成熟，很多问题需要讨论。

### 1. `/工作区` 是否是最合适的命名？

可能的名字包括：

```text
/工作区
/工程区
/ai-workspace
/workspace
/dev
```

哪一个更适合全球开发者？

---

### 2. 人工资料是否应该完全放在仓库中？

对于公司内部项目，可能存在隐私、合规和大文件问题。

是否应该支持：

- Git LFS；
- 外部文档链接；
- 私有对象存储；
- 只存 manifest，不存原文；
- 只在本地编译，不提交原始资料？

---

### 3. `/AI` 生成产物是否应该提交到 Git？

可能有两种模式：

- 提交 `/AI`，让团队共享同一份 AI 上下文；
- 不提交 `/AI`，每个人本地生成；
- 只提交部分核心上下文；
- CI 自动生成并作为 artifact。

这需要更多真实项目验证。

---

### 4. 普通 Agent 是否应该完全禁止读取人工资料？

当前建议是默认禁止，但允许例外。

实际项目中，可能需要更细的权限模型。

---

### 5. Context Compiler 应该做到多强？

最低可用版本可能只支持 Markdown、PDF、DOCX、XLSX。

生产级版本可能需要支持：

- OCR；
- 图片理解；
- XMind；
- PPT；
- Figma；
- 接口平台；
- GitLab；
- Jira；
- Confluence；
- 飞书/Notion/语雀等。

---

### 6. Skill 是否应该跨工具统一？

Claude Code、Codex、Cursor、OpenCode、Gemini CLI 对 Skill、Agent、MCP 的支持方式不同。

是否需要一个跨工具的 Project Skill 标准？

---

### 7. AI 上下文的质量如何评估？

可能需要 `context-health.generated.md` 记录：

- 覆盖率；
- 冲突数量；
- 未确认问题；
- 过期资料；
- 需求到测试的追踪完整度；
- 代码影响分析可信度；
- 不同角色视图的可用性；
- 角色视图是否过期；
- code graph 与源码是否同步。

---

### 8. 角色视图应该如何生成？

不同角色需要不同粒度的信息。

需要讨论：

- 产品视图是否应该包含代码细节？
- 测试视图是否应该包含完整 code graph？
- 设计视图如何表达设计稿与实现差异？
- 后端视图是否应该包含前端调用链？
- Reviewer 视图是否应该融合 MR diff、测试覆盖和需求追踪？
- 是否需要支持自定义角色？

---

## 欢迎讨论

这个仓库不是为了宣布一个标准，而是提出一个问题：

> AI coding agent 时代，软件项目的目录结构和协作方式应该如何变化？

我希望和社区一起讨论：

- 这个目录是否合理？
- `/工作区` 的边界是否清晰？
- `/AI` 是否应该提交？
- Context Compiler 应该如何设计？
- 角色视图应该如何定义？
- Claude Code、Codex、Cursor 等工具应如何共同支持项目级上下文？
- Skill、MCP、Subagent 是否应该有更统一的跨工具标准？
- 产品、设计、测试如何在尽量不改变工作习惯的情况下参与 AI-native 工程协作？
- 如何让产品、设计、开发、测试都能通过 AI 理解其他角色的工作成果？

欢迎提交 Issue、Proposal、PR 或替代目录结构。
