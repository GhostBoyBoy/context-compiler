# AI-Collaborative Project Workspace: An Open Proposal for the Next Generation of Software Engineering Directory Structures

## Background

Traditional software development is usually organized around humans.

A project is typically completed by multiple roles working together: product, design, frontend, backend, QA, operations, and others. Each role uses its own tools, documents, and working style:

- Product maintains PRDs, business rules, meeting notes, and requirement change records;
- Design maintains prototypes, interaction notes, visual designs, and flow diagrams;
- Frontend maintains pages, components, state management, and API calls;
- Backend maintains services, APIs, domain models, databases, and jobs;
- QA maintains test plans, test cases, bug retrospectives, test data notes, and automated tests.

In the traditional model, a repository is usually just a “code repository”.

A large amount of product context, business constraints, historical decisions, testing rationale, and design detail is scattered across document tools, chat logs, meeting notes, shared drives, issues, wikis, or people’s heads.

This can still work in a purely human collaboration model, but it creates new problems in the era of AI coding agents:

1. AI can see the code, but not the complete business context;
2. AI can easily misread outdated materials, old proposals, or drafts;
3. AI often does not know how requirements, tests, APIs, and pages relate to each other;
4. Different roles repeatedly explain context to AI, creating duplicated communication and context waste;
5. Forcing product, QA, or design teams to convert everything into Markdown or engineering-friendly formats creates significant organizational friction;
6. AI coding agents are often biased toward developer workflows, while product, design, and QA roles also need AI context.

Therefore, I want to raise an open question:

> As AI becomes increasingly involved in software development, should a project repository evolve from a “code repository” into an “AI-collaborative project workspace”?

---

## Core Idea

This project workspace is not about simply dumping every piece of material into one large repository.

The core idea is:

> Humans continue maintaining materials in the way they are already comfortable with;  
> AI does not directly consume messy human-facing materials;  
> A Context Compiler compiles product, design, QA, code, and other materials into structured AI-readable context;  
> AI Agents for different roles collaborate through role-specific views;  
> Claude Code, Codex, Cursor, and other AI Agents work based on those contexts, project-level Skills, MCP tools, and Agent rules.

In other words:

```text
Product materials
Design materials
QA materials
Frontend code
Backend code
Automated tests
Historical bugs
Runtime logs
   ↓
Context Compiler
   ↓
/workspace/AI/domain context
/workspace/AI/role views
   ↓
Product Agent / Design Agent / Frontend Agent / Backend Agent / QA Agent / Reviewer Agent
   ↓
Requirement analysis, design validation, coding, testing, review, delivery
```

The goal of this model is not to replace product, design, QA, or development roles. The goal is to reduce low-value context relaying and make AI participation in multi-role engineering collaboration more reliable.

---

## One-Sentence Definition

> An AI-collaborative project workspace is a collaboration structure bounded by a project goal, entered through role-specific views, and transformed through a Context Compiler. It preserves the native working styles of product, design, development, and QA roles, while compiling their materials and engineering implementation into AI context suitable for different role-based Agents.

---

## This Is Not About Making Other Roles Serve Development

This proposal can easily be misunderstood as:

> Product, design, and QA organize their materials so that developer Agents can write code better.

That is only one scenario, not the full picture.

A more complete interpretation is:

> Each role maintains its own original materials;  
> Each role can also use AI to understand the work produced by other roles;  
> The Context Compiler compiles information from other roles into AI context views that are understandable and actionable for the current role.

For example:

### When a Developer Role Uses AI

Product, design, and QA materials are compiled into context that helps with coding:

```text
/workspace/AI/role-views/for-frontend.generated.md
/workspace/AI/role-views/for-backend.generated.md
```

A frontend Agent needs to understand:

- Current requirements;
- Page flows;
- Interaction rules;
- API contracts;
- Test acceptance criteria;
- Component reuse suggestions.

A backend Agent needs to understand:

- Business rules;
- Domain models;
- API contracts;
- Frontend callers;
- Idempotency and consistency requirements;
- Testing risks and acceptance points.

### When a Product Role Uses AI

Frontend, backend, design, and QA modules should also be compiled into context that helps the product role understand project status:

```text
/workspace/AI/role-views/for-product.generated.md
```

A product Agent can answer:

- Which requirements have already been implemented?
- Which requirements are still uncovered?
- Which pages, APIs, and data models in the codebase are related to a given requirement?
- Do tests cover the key acceptance criteria?
- Which product capabilities may be affected by recent code changes?
- Are there inconsistencies between the current implementation and the PRD?
- How do the code graph, service graph, or page graph map to product semantics?

### When a Design Role Uses AI

A design Agent can understand the following through frontend code, page graphs, component graphs, and product context:

- Current page implementation status;
- Which components already exist;
- Which interactions have already been implemented;
- Whether there are differences between design files and code implementation;
- Which design changes may affect frontend, backend, or QA;
- Whether a user flow is fully supported.

### When a QA Role Uses AI

A QA Agent can use product, design, frontend, backend, and historical bug context to understand:

- Acceptance criteria for a requirement;
- Which page flows need regression testing;
- Which APIs and services are affected;
- Which historical bugs may recur;
- Which requirements lack automated test coverage;
- How to generate test data;
- The risk matrix of the current code change.

Therefore, AI context is not one-directional:

```text
Product / Design / QA → Development
```

It is multi-directional:

```text
Product ↔ Design ↔ Frontend ↔ Backend ↔ QA
        ↓
   Context Compiler
        ↓
   Role-specific AI Context
```

---

## Design Principles

### 1. Human Materials Keep Their Native Form

Human-maintained directories for product, design, QA, and similar roles should not restrict file formats.

They may contain:

- Markdown
- Word
- PDF
- PowerPoint
- Excel
- XMind
- Images
- Figma exports
- Flowcharts
- API collections
- HAR captures
- Logs
- Meeting notes
- Bug retrospectives

The reason is simple:

> We should not force the entire organization to change its existing working habits just to make AI comfortable.

An AI-native workspace should reduce collaboration cost, not add burden to product, design, and QA teams.

---

### 2. AI Reads Compiled Context by Default

A normal AI Agent should not directly read human-facing material directories by default.

For example, a normal development Agent should not directly read:

```text
../product
../design
../qa
```

It should first read:

```text
workspace/AI
```

Only the following cases should allow backtracking into original human materials:

1. The user explicitly asks to inspect original materials;
2. `/AI` context is missing or conflicting;
3. The current task is to generate or repair AI context;
4. The current Agent is a Context Compiler Agent;
5. A human review requires source tracing.

---

### 3. AI Context Is a Compiled Artifact, Not Human-Maintained Material

Content under `/workspace/AI` is generated by tools and should not be manually edited by default.

For example:

```text
/workspace/AI/product/requirements.generated.md
/workspace/AI/qa/acceptance-map.generated.md
/workspace/AI/backend/api-contracts.generated.md
/workspace/AI/role-views/for-backend.generated.md
/workspace/AI/role-views/for-product.generated.md
```

If generated content is wrong, the original human material or Context Compiler rules should be fixed, and the generated files should be regenerated. The generated files themselves should not be manually patched.

---

### 4. Domain Context and Role Views Should Be Separated

The `/AI` directory should contain at least two types of content:

```text
Domain context: What is the current state of a domain?
Role view: What does a role need to understand from other domains in order to do its work?
```

For example:

```text
/workspace/AI/product
/workspace/AI/design
/workspace/AI/frontend
/workspace/AI/backend
/workspace/AI/qa
```

These are domain contexts.

```text
/workspace/AI/role-views/for-product.generated.md
/workspace/AI/role-views/for-designer.generated.md
/workspace/AI/role-views/for-frontend.generated.md
/workspace/AI/role-views/for-backend.generated.md
/workspace/AI/role-views/for-qa.generated.md
/workspace/AI/role-views/for-reviewer.generated.md
```

These are role views.

Domain context answers:

> What is the current state of product, design, frontend, backend, and QA?

Role views answer:

> What does a given role need to understand from other domains to complete its work?

---

### 5. Use Official AI Tooling Mechanisms Whenever Possible

This workspace should not invent a completely isolated plugin system.

Therefore, project-level capabilities already supported by tools like Claude Code and Codex should be used in their official form whenever possible:

- Claude Code:

  - `CLAUDE.md`
  - `.claude/settings.json`
  - `.claude/skills/*/SKILL.md`
  - `.claude/agents/*.md`
  - `.claude/commands/*.md`
  - `.mcp.json`

- Codex:

  - `AGENTS.md`
  - `.codex/config.toml`
  - `.codex/agents/*.toml`
  - `.agents/skills/*/SKILL.md`

`/AI` is responsible only for storing generated AI context. It should not replace official plugin, Skill, MCP, or Agent mechanisms.

---

### 6. Skills Are Methods, MCPs Are Tools, AI Context Is Material

These concepts should remain clearly separated:

| Type | Purpose | Recommended Location |
|---|---|---|
| AI Context | Tells the Agent what the current project is, including requirements, APIs, tests, and risks | `/workspace/AI` |
| Role View | Tells a role how to understand information from other roles | `/workspace/AI/role-views` |
| Skill | Tells an Agent how to perform a certain class of task | `.claude/skills`, `.agents/skills` |
| MCP | Provides callable tools to an Agent, such as documentation lookup, code search, GitLab, or context compilation | `.mcp.json`, `.codex/config.toml`, `scripts/mcp` |
| Subagent | Defines a specialized Agent role, such as backend implementer, QA analyst, or context compiler | `.claude/agents`, `.codex/agents` |

---

## Current Suggested Directory Structure

The following is an initial directory structure for this proposal.

It is not a final answer. It is a starting point for community discussion.

```text
project-root/
  README.md

  /product
    /requirements
    /business-rules
    /user-research
    /meeting-notes
    /competitor-analysis
    /prototype-screenshots
    /archive

  /design
    /prototypes
    /interaction-notes
    /visual-guidelines
    /flowcharts
    /page-screenshots
    /archive

  /qa
    /test-plans
    /test-design
    /test-cases
    /bug-retrospectives
    /test-data-notes
    /environment-notes
    /api-collections
    /har-captures
    /archive

  /workspace
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
        qa-context-compiler.md
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
        qa-context-compiler.toml
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
      /global
        workspace-index.generated.md
        source-manifest.generated.json
        traceability.generated.md
        open-questions.generated.md
        deprecated-context.generated.md
        context-health.generated.md

      /product
        context.generated.md
        requirements.generated.md
        business-rules.generated.md
        glossary.generated.md
        product-decisions.generated.md
        open-questions.generated.md

      /design
        context.generated.md
        page-flows.generated.md
        interaction-rules.generated.md
        component-map.generated.md
        design-decisions.generated.md
        open-questions.generated.md

      /frontend
        context.generated.md
        page-map.generated.md
        component-map.generated.md
        route-map.generated.md
        state-management.generated.md
        api-usage.generated.md
        frontend-risks.generated.md
        code-graph.generated.json
        dependency-map.generated.md

      /backend
        context.generated.md
        architecture.generated.md
        api-contracts.generated.md
        domain-model.generated.md
        database-map.generated.md
        service-map.generated.md
        backend-risks.generated.md
        code-graph.generated.json
        dependency-map.generated.md

      /qa
        context.generated.md
        acceptance-map.generated.md
        risk-matrix.generated.md
        test-cases.generated.md
        test-data-guide.generated.md
        regression-scope.generated.md
        known-bugs.generated.md

      /role-views
        for-product.generated.md
        for-designer.generated.md
        for-frontend.generated.md
        for-backend.generated.md
        for-qa.generated.md
        for-reviewer.generated.md

      /reports
        2026-05-29-context-generation-report.md

    /frontend
      /web
      /admin
      /mobile
      /shared-ui

    /backend
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
      /product
      /design
      /qa
      /frontend
      /backend
```

---

## Why Separate `/workspace`?

In this design, AI coding agents such as Claude Code and Codex open:

```text
project-root/workspace
```

instead of opening the project root.

This has several benefits:

1. It prevents normal Agents from scanning outer human-facing material directories by default;
2. It places the AI runtime environment and engineering source code inside a clear boundary;
3. It makes `/product`, `/design`, and `/qa` inputs to the Context Compiler instead of direct context for normal Agents;
4. It makes it easier to configure project-level rules, MCP tools, Skills, and Subagents for Claude/Codex;
5. It makes future support for different AI tools easier.

The overall relationship looks like this:

```text
project-root/
  /product    Human-maintained product materials
  /design     Human-maintained design materials
  /qa         Human-maintained QA materials

  /workspace  Working directory for Claude / Codex / Cursor and other AI Agents
    /AI       Compiled AI context
    /frontend Frontend source code
    /backend  Backend source code
```

---

## The Role of the `/AI` Directory

`/workspace/AI` is a multi-role AI context directory.

It is not a human-facing material directory, and it is not only for developer Agents.

It should be generated by the Context Compiler from product, design, frontend, backend, QA, and related materials, serving different AI collaboration needs for different roles.

For example:

- A product role uses `/AI/role-views/for-product.generated.md` to understand current implementation status, test coverage, and requirement delivery state;
- A design role uses `/AI/role-views/for-designer.generated.md` to understand pages, components, interaction implementation, and design differences;
- A frontend role uses `/AI/role-views/for-frontend.generated.md` to understand requirements, design, APIs, and test acceptance criteria;
- A backend role uses `/AI/role-views/for-backend.generated.md` to understand business rules, API contracts, frontend callers, and testing risks;
- A QA role uses `/AI/role-views/for-qa.generated.md` to understand product acceptance criteria, design flows, frontend/backend implementation, and regression scope;
- A Reviewer role uses `/AI/role-views/for-reviewer.generated.md` to evaluate MR risk, test coverage, and requirement consistency.

Therefore, the `/AI` directory contains two core categories of content:

1. Domain context, such as `/AI/product`, `/AI/design`, `/AI/frontend`, `/AI/backend`, `/AI/qa`;
2. Role views, such as `/AI/role-views/for-product.generated.md`, `for-backend.generated.md`, `for-qa.generated.md`.

Domain context answers:

> What is the current state of a domain?

Role views answer:

> What does a role need to understand from other domains in order to complete its work?

---

## Role View Examples

### `/AI/role-views/for-product.generated.md`

For the product role.

Possible sources include:

- `/AI/frontend`
- `/AI/backend`
- `/AI/qa`
- `/AI/design`
- code graph
- service graph
- API graph
- test coverage graph

Possible contents include:

- Requirement implementation status;
- Page-to-requirement mapping;
- API-to-requirement mapping;
- Current capability boundaries;
- Known technical constraints;
- Test coverage status;
- Product capabilities affected by recent changes;
- Requirements that are not implemented or inconsistent with implementation;
- How code structure maps to product capabilities.

### `/AI/role-views/for-designer.generated.md`

For the design role.

Possible sources include:

- `/AI/product`
- `/AI/frontend`
- `/AI/qa`
- page graph
- component graph

Possible contents include:

- Page list;
- User flows;
- Component reuse status;
- Current interaction implementation constraints;
- Released page status;
- Differences between design and implementation;
- Frontend modules and QA scope affected by design changes.

### `/AI/role-views/for-frontend.generated.md`

For the frontend role.

Possible sources include:

- `/AI/product`
- `/AI/design`
- `/AI/backend`
- `/AI/qa`

Possible contents include:

- Current requirements;
- Page flows;
- Interaction rules;
- API contracts;
- Acceptance criteria;
- Related tests;
- Component reuse suggestions;
- UI risk points.

### `/AI/role-views/for-backend.generated.md`

For the backend role.

Possible sources include:

- `/AI/product`
- `/AI/frontend`
- `/AI/qa`
- service graph
- database graph

Possible contents include:

- Business rules;
- Domain models;
- API contracts;
- Frontend callers;
- Idempotency and consistency requirements;
- Database impact;
- Test acceptance points;
- Risk boundaries.

### `/AI/role-views/for-qa.generated.md`

For the QA role.

Possible sources include:

- `/AI/product`
- `/AI/design`
- `/AI/frontend`
- `/AI/backend`
- Historical bugs
- code graph

Possible contents include:

- Acceptance criteria;
- Page paths;
- API flows;
- Risk matrix;
- Test data suggestions;
- Regression scope;
- Code change impact surface;
- Historical bug recurrence risk.

### `/AI/role-views/for-reviewer.generated.md`

For the Reviewer role.

Possible sources include:

- `/AI/product`
- `/AI/design`
- `/AI/frontend`
- `/AI/backend`
- `/AI/qa`
- MR diff
- code graph
- test coverage

Possible contents include:

- Requirements related to the current change;
- Affected pages, APIs, services, and tests;
- Risks caused by missing test coverage;
- Inconsistencies between requirements and implementation;
- Code complexity or security risks;
- Whether generated file rules are violated;
- Whether the Agent misread or overstepped into human-facing material directories.

---

## What Is the Context Compiler?

The Context Compiler is the key component of this proposal.

It reads human-facing material directories:

```text
../product
../design
../qa
```

and, when needed, engineering source code:

```text
/frontend
/backend
/testing
```

Then it generates:

```text
/workspace/AI
```

It should support at least the following capabilities:

1. Multi-format parsing: PDF, PowerPoint, Word, Excel, XMind, images, Markdown, HTML, JSON;
2. OCR and screenshot understanding;
3. Requirement extraction;
4. Business rule extraction;
5. Glossary extraction;
6. Design flow and page relationship extraction;
7. Test point and test case extraction;
8. Code structure scanning;
9. Code graph / dependency graph / service graph generation;
10. API contract generation;
11. Conflict detection;
12. Outdated information detection;
13. Traceability generation;
14. Role-specific AI view generation;
15. Generation reports for human review.

Its role is similar to:

```text
Human materials + engineering implementation = source code
Context Compiler = compiler
/AI = compiled artifacts
Agent = runtime consumer
```

---

## Recommended Agent Access Rules

A normal Agent is allowed to read by default:

```text
/AI
/frontend
/backend
/testing
/scripts
/infra
```

A normal Agent is forbidden by default from reading:

```text
../product
../design
../qa
../**/archive/**
```

Human-facing materials may contain:

- Historical proposals;
- Drafts;
- Temporary meeting conclusions;
- Outdated designs;
- Unconfirmed requirements;
- Unstructured descriptions;
- Sensitive information;
- Context that is not suitable for direct AI consumption.

Only a Context Compiler Agent or an explicitly authorized task may read human-facing material directories.

---

## Recommended AI Tooling Capabilities

This workspace can integrate several kinds of capabilities from the current AI coding ecosystem.

### 1. Project Skills

Project-level Skills capture “how to perform a class of task”.

For example:

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

A Skill is not context material. It is a task method.

For example, `backend-api-implementation` should tell an Agent:

1. Which AI contexts to read first;
2. How to confirm the requirement ID;
3. How to inspect API contracts;
4. How to modify backend code;
5. How to add or update tests;
6. How to output risks and open questions.

And `requirement-change-sync` should tell a product Agent:

1. How to read `for-product.generated.md`;
2. How to understand current implementation status;
3. How to determine whether a requirement already has corresponding pages, APIs, and tests;
4. How to propose the impact scope of a requirement change;
5. How to generate follow-up items for development, design, and QA.

---

### 2. MCP Servers

MCP provides callable tools to Agents, such as:

- Latest technical documentation lookup;
- Semantic code search;
- GitLab MR lookup and review;
- Project context compilation;
- Impact analysis;
- Test data generation.

Recommended MCP types:

```text
context7             latest library documentation lookup
code-search          semantic code search
context-compiler     compile product/design/QA/code into /AI
gitlab               query MRs, issues, and pipelines
```

---

### 3. Code Graph / Knowledge Graph

An AI Agent should not scan the whole repository every time.

A better approach is to generate code graphs and project graphs, such as:

```text
/AI/frontend/component-map.generated.md
/AI/frontend/route-map.generated.md
/AI/backend/service-map.generated.md
/AI/backend/domain-model.generated.md
/AI/global/traceability.generated.md
```

This helps Agents for different roles understand project structure and impact scope more quickly.

For example:

- A development Agent uses the code graph to understand implementation locations;
- A product Agent uses the code graph to understand whether a requirement has landed;
- A QA Agent uses the code graph to determine regression scope;
- A Reviewer Agent uses the code graph to inspect MR impact.

---

### 4. Multi-format Document Parsing

Because human materials do not restrict file formats, the Context Compiler must support multi-format inputs.

It may integrate:

- PDF parser
- DOCX parser
- PPTX parser
- XLSX parser
- XMind parser
- OCR
- Image understanding
- Markdown parser
- HTML parser

This capability determines whether the workspace can be adopted with low organizational friction.

---

## A Typical Workflow: Development Role

### Scenario: Product Changes a Refund Requirement, Backend Needs to Implement It

1. Product updates:

```text
/product/requirements/refund-flow-requirement.pdf
/product/meeting-notes/refund-requirement-review.docx
```

2. Run the Context Compiler:

```bash
cd workspace
npm run ai:context
```

3. The tool generates:

```text
/workspace/AI/product/requirements.generated.md
/workspace/AI/product/business-rules.generated.md
/workspace/AI/qa/acceptance-map.generated.md
/workspace/AI/role-views/for-backend.generated.md
/workspace/AI/reports/context-generation-report.md
```

4. The backend Agent reads:

```text
/AI/role-views/for-backend.generated.md
/AI/product/requirements.generated.md
/AI/backend/api-contracts.generated.md
/AI/qa/acceptance-map.generated.md
```

5. The backend Agent modifies code:

```text
/backend
/testing
```

6. The Reviewer Agent reviews the MR based on `/AI` and code changes.

---

## A Typical Workflow: Product Role

### Scenario: Product Wants to Confirm Whether a Requirement Has Fully Landed

1. The Context Compiler scans code, tests, and design context:

```text
/workspace/frontend
/workspace/backend
/workspace/testing
/workspace/AI/design
/workspace/AI/qa
```

2. It generates the product role view:

```text
/workspace/AI/role-views/for-product.generated.md
```

3. A product Agent can answer based on this role view:

```text
Has RQ-REFUND-001 partial refund been implemented?
Which pages support it?
Which APIs support it?
Which tests cover it?
What gaps remain?
Will the latest MR affect this requirement?
```

4. If inconsistencies between requirement and implementation are found, product can initiate:

```text
Requirement clarification
Requirement change
Design supplement
Development task
QA supplement
```

instead of repeatedly asking each role in meetings.

---

## A Typical Workflow: QA Role

### Scenario: QA Needs to Determine the Regression Scope of a Code Change

1. The Context Compiler reads:

```text
/workspace/AI/product
/workspace/AI/design
/workspace/AI/frontend
/workspace/AI/backend
/workspace/backend code graph
/workspace/frontend code graph
/qa/bug-retrospectives
```

2. It generates the QA role view:

```text
/workspace/AI/role-views/for-qa.generated.md
/workspace/AI/qa/regression-scope.generated.md
/workspace/AI/qa/risk-matrix.generated.md
```

3. The QA Agent can answer:

```text
Which features are affected by this change?
Which pages need regression testing?
Which APIs need regression testing?
Which historical bugs may recur?
Which test cases need to be added or updated?
Is automated test coverage missing?
```

---

## What Does This Proposal Try to Solve?

### 1. Reduce Repeated Cross-role Communication

Many meetings are essentially context synchronization.

Now context can be structured, generated, traced, and reviewed.

---

### 2. Reduce AI Coding Hallucination and Misreading

Agents do not directly read messy original materials. They read compiled and validated context.

---

### 3. Preserve Traditional Team Workflows

Product, design, and QA teams do not need to immediately change their tools.

They only need to place materials in the right location and review generation reports.

---

### 4. Make Requirements, Design, Code, and Tests Traceable

For example:

```text
RQ-REFUND-001
  → PAGE-REFUND-DETAIL
  → API-REFUND-CREATE
  → SERVICE-order-service
  → TC-REFUND-003
```

This kind of traceability is useful for AI, MR review, regression analysis, and impact analysis.

---

### 5. Build a Foundation for Multi-Agent Collaboration

In the future, different Agents can take different responsibilities:

- Product Context Compiler
- Design Context Compiler
- QA Context Compiler
- Frontend Implementer
- Backend Implementer
- Reviewer
- Security Reviewer
- Regression Analyzer
- Product Analyst
- Design Reviewer

They no longer work from temporary prompts. They collaborate through a unified workspace, project-level Skills, MCP tools, and AI context.

---

### 6. Let Every Role Use AI to Understand the Work of Other Roles

Product does not need to read code to understand implementation status.

QA does not need to ask developers one by one to understand change impact.

Design does not need to inspect source code manually to understand components and page implementation.

Developers do not need repeated meetings to understand requirements, design, and test acceptance criteria.

This is the most important value of an AI-collaborative project workspace.

---

## Open Questions

This proposal is still immature, and many questions need discussion.

### 1. Is `/workspace` the Best Name?

Possible names include:

```text
/workspace
/engineering
/ai-workspace
/dev
```

Which one is most suitable for global developers?

---

### 2. Should Human-facing Materials Be Stored Entirely in the Repository?

For internal company projects, there may be privacy, compliance, and large-file concerns.

Should we support:

- Git LFS;
- External document links;
- Private object storage;
- Manifest-only storage without original files;
- Local-only compilation without committing original materials?

---

### 3. Should Generated `/AI` Artifacts Be Committed to Git?

Possible modes include:

- Commit `/AI`, so the team shares the same AI context;
- Do not commit `/AI`; each developer generates it locally;
- Commit only selected core context;
- Generate it in CI and publish it as an artifact.

More real-world validation is needed.

---

### 4. Should Normal Agents Be Completely Forbidden From Reading Human-facing Materials?

The current suggestion is “forbidden by default, allowed by exception”.

Real projects may require a more granular permission model.

---

### 5. How Powerful Should the Context Compiler Be?

A minimum viable version may support only Markdown, PDF, DOCX, and XLSX.

A production-grade version may need to support:

- OCR;
- Image understanding;
- XMind;
- PowerPoint;
- Figma;
- API platforms;
- GitLab;
- Jira;
- Confluence;
- Lark / Notion / Yuque and similar tools.

---

### 6. Should Skills Be Unified Across Tools?

Claude Code, Codex, Cursor, OpenCode, and Gemini CLI support Skills, Agents, and MCP in different ways.

Do we need a cross-tool Project Skill standard?

---

### 7. How Should AI Context Quality Be Evaluated?

A `context-health.generated.md` file may need to track:

- Coverage;
- Number of conflicts;
- Open questions;
- Outdated materials;
- Requirement-to-test traceability completeness;
- Code impact analysis confidence;
- Usability of different role views;
- Whether role views are outdated;
- Whether code graphs are synchronized with source code.

---

### 8. How Should Role Views Be Generated?

Different roles need different levels of detail.

Questions to discuss:

- Should the product view include code details?
- Should the QA view include the full code graph?
- How should the design view express differences between design files and implementation?
- Should the backend view include frontend call chains?
- Should the Reviewer view combine MR diff, test coverage, and requirement traceability?
- Should custom roles be supported?

---

## Discussion Welcome

This repository is not meant to declare a standard. It is meant to ask a question:

> In the age of AI coding agents, how should software project directory structures and collaboration models evolve?

I hope the community can discuss:

- Is this directory structure reasonable?
- Is the `/workspace` boundary clear?
- Should `/AI` be committed?
- How should the Context Compiler be designed?
- How should role views be defined?
- How should Claude Code, Codex, Cursor, and other tools jointly support project-level context?
- Should Skills, MCP, and Subagents have a more unified cross-tool standard?
- How can product, design, and QA participate in AI-native engineering collaboration without changing their working habits too much?
- How can product, design, development, and QA all use AI to understand each other’s work?

Issues, proposals, PRs, and alternative directory structures are welcome.

