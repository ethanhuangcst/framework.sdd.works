# Woodensword Skills Catalog

Editable skill tree for the woodensword workspace. Runtime install path: `~/.cursor/skills/` (see `Rules/source-of-truth.mdc` in the IDE catalog).

| Skill | Purpose |
| --- | --- |
| **atdd** | Acceptance criteria / Gherkin before build |
| **tdd** | Red-Green-Refactor; extends common-test-strategy |
| **webapp-testing** | Playwright + `with_server.py` for local UI |
| **frontend-design** | Distinctive UI/visual direction (not generic templates) |
| **Fullstack** | Next/React/Node/data layer delivery |
| **retrospective** | ADRs + `specs/knowledge/` after DoD |
| **agent-builder** | Agent loops, tools, subagents; `init_agent.py` scaffold |
| **mcp-server-patterns** | MCP tools/resources/transports (TypeScript SDK) |
| **rag-implementation** | RAG pipelines and vector search |
| **ai-architect-expert** | ML/LLM platform and MLOps design |
| **skill-creator** | Author, eval, and package skills |
| **skill-lookup** | Find/install skills from prompts.chat |

**Typical story flow:** atdd → tdd → (frontend-design / Fullstack for UI) → webapp-testing → dod → retrospective.

**Agent / platform work:** agent-builder → mcp-server-patterns (tools) → rag-implementation (knowledge) as needed.

Sync: `rsync -a woodensword/Skills/<name>/ ~/.cursor/skills/<name>/`
