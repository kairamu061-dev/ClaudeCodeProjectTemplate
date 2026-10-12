# Project Structure

## Directory Layout

```
docs/
├── index.md               # Required — list of feature areas and sub-items
├── project_overview.md    # Required — overall project overview
├── glossary.md            # Required — domain-specific terminology
├── tags.md                # Required — inline tag definitions and schema
└── {feature-area}/        # One directory per feature area
    ├── overview.md
    ├── spec.md
    ├── design.md
    ├── tasks.md
    ├── test-cases.md
    ├── dev-notes.md
    └── {sub-item}/        # Created as needed per split rules
        └── ...

templates/                 # Document templates (all copied by /add-feature)
├── overview.md
├── spec.md
├── design.md
├── tasks.md
├── test-cases.md
└── dev-notes.md

issues/                    # Bug ticket management
├── index.md               # Ticket list and state legend
├── templates/
│   └── BUG-template.md
└── tickets/
    └── BUG-{NNN}.md        # Added sequentially as bugs are reported

tests/                     # Automated tests (see docs/testing.md for the line)

tmp/                       # Not committed. Working area
├── 確認/                   # Verification requests written by the agent
│   └── old/               #   archived after each round
└── フィードバック/           # Reports from the verification session
    └── old/               #   archived after each round
```

---

## Skills

| Skill | Use |
|---|---|
| `/add-feature <path>` | Create a feature area or sub-item: directory, templates, index.md row |
| `/verify-request <title> [--run <id>] [--workflow <name>]` | Generate a verification request. **Measures the CI artifacts itself** (sizes, SHA256) so they cannot be mis-transcribed, warns on a dirty tree or a HEAD that differs from the run, and carries over the previous round's open items. Without `--run`, picks the latest green run on the current branch. Requires `gh`, `git`, `awk` |

---

## Document Templates

### project_overview.md
- **Purpose & Background**: The problem this project solves and why it exists
- **Scope**: What will and will not be built
- **Tech Stack Overview**: Technologies used and rationale
- **Overall Architecture**: High-level system structure
- **Constraints**: Technical, environmental, and resource limitations

### overview.md (per feature area)
- **Purpose & Background**: Why this feature is needed
- **Scope**: What will and will not be built
- **Constraints**: Technical and resource limitations
- **Definition of Done**: What constitutes completion

### spec.md
- **Feature List**: Enumeration of provided features
- **Screens & User Flow**: Step-by-step user interactions
- **Screen / State Details**: Display content, actions, and transitions
- **Error Cases**: Behavior for abnormal conditions
- **Out of Scope**: Things intentionally not handled

### design.md
- **Tech Selection**: Technologies used and rationale
- **Architecture**: Component structure
- **Data Structures**: Key data models and schemas
- **Interfaces**: API and function interfaces
- **Dependencies**: External libraries and services

### tasks.md
- **Task List**: Checkbox format
- **Dependencies**: Ordering constraints between tasks
- **Status**: Todo / In progress / Done

### test-cases.md
- Every row in spec.md's feature list names its test-case IDs in the テストケース column,
  or says 「無し」 explicitly. An empty cell is an oversight; 「無し」 is a decision —
  and a feature with an empty cell is invisible to a port/rewrite coverage table
- E2E rows carry a **対照 (control)** column. Where no control exists, say so:
  an item that cannot fail cannot be evidence

### dev-notes.md
- **Implementation Decisions**: Why a particular approach was chosen
- **Issues & Resolutions**: Problems encountered and how they were solved
- **Deviations from Design**: Differences from the design document and reasons
- **Future Work**: Current limitations and items to address later
- **Requests to User**: Recorded when skills, permissions, or information are lacking
- Keep a **"how we verify"** section here. The verification method compounds faster than
  the code does, and is the part most worth carrying to the next project

### Verification request (.claude/skills/verify-request/template.md)
- Lives inside the skill, not in `templates/`, so `/add-feature` does not copy it
- Header's artifact table is **generated**, never typed
- **測ったこと / 推測していること** — unmarked claims are not allowed
- A **対照** column per item, a table of every `0` expectation and its control,
  and a section listing items that have no control at all

### docs/testing.md
- The line drawn on day one: what runs in CI without the target environment,
  what needs real hardware and therefore stays in the manual loop, and why

### Bug ticket (issues/tickets/BUG-{NNN}.md)

Created following `issues/templates/BUG-template.md`.
