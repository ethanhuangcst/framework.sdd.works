# Project constants

Seed for `<workspace>/.cursor/project-constants.md`. Agents read the project copy. This file is the seed shipped with the template pack.

Defaults shared by agents and skills. Not Scrum vocabulary (that is `sdd-scrum-guide.md`). Not this project's live paths (those are in `artifacts-map.md` under the artifacts root).

Do not copy this file into the artifacts root. Do not overwrite an existing `<workspace>/.cursor/project-constants.md`.

## Keys

| Key | Value |
| --- | --- |
| `site_home` | `https://framework.sdd.works/` |
| `instructions_url` | `https://framework.sdd.works/instructions` |
| `artifacts_root` | `specs` |
| `agents_dir` | `agents` |
| `skills_dir` | `skills` |
| `rules_dir` | `rules` |
| `workflows_dir` | `workflows` |

`artifacts_root` is the default folder name for a new project. A project may use another name. After it exists, read the live root from that project's `artifacts-map.md`.

`agents_dir`, `skills_dir`, `rules_dir`, and `workflows_dir` are folders under the user Cursor root (`~/.cursor` on Cursor).

## Default folders under `artifacts_root`

Created for a new project when a skill says to. Omitted if the project already has a different map.

| Key | Folder |
| --- | --- |
| `adr_dir` | `adr` |
| `knowledge_dir` | `knowledge` |

## Skills

Folder names under `skills_dir`. Agents look up a key here. Do not hard-code the folder name in an agent prompt.

| Key | Skill folder |
| --- | --- |
| `skill_atdd` | `sdd-atdd` |
| `skill_tdd` | `sdd-tdd` |
| `skill_start_project` | `sdd-new-project` |
| `skill_update_project` | `sdd-update-project` |
| `skill_refine_pb` | `sdd-refine-pb` |
| `skill_plan_sprint` | `sdd-plan-sprint` |
| `skill_update_status` | `sdd-update-status` |
| `skill_retrospective` | `sdd-retrospective` |
| `skill_close_sprint` | `sdd-close-sprint` |
| `skill_audit_artifacts` | `sdd-audit-artifacts` |
| `skill_update_specs` | `sdd-update-specs` |
| `skill_implement_feature` | `sdd-implement-feature` |

## Rules

File names under `rules_dir`.

| Key | Rule file |
| --- | --- |
| `rule_dod` | `sdd-dod.mdc` |
| `rule_incremental_delivery` | `sdd-incremental-delivery.mdc` |
| `rule_realtime_status` | `sdd-realtime-status.mdc` |

## Workflows

Names under `workflows_dir`. Empty until a workflow is planned.

| Key | Workflow |
| --- | --- |
| — | — |
