# Development Protocols

**Version**: 3.2.0
**Last Updated**: 2026-04-27
**Purpose**: Inform agents on our standardized project structure, documentation and development workflows.
**Syncing**: Sync any updates to all `.agent/DEV_RULES.md` files using `frdoc` (see § *Syncing This Document*).

---

## How This Works

These protocols work because they (1) **maintain lifespan documents** for future reference, (2) **agents follow standards** that maintain organization, and (3) agents take initiative **correcting protocol drift**. This is how you support each other which makes us an effective team.

### Correcting Issues

If you encounter inconsistencies, drift, or protocol violations, take action to correct the issues. 

  A. When creating your session plan, integrate a cleanup step for the drift you observed. This is done whether or not you created the drift.
  B. If you discover issues while following your session plan, pause and explain to human what needs cleanup for immediate approval.

Do not assume you'll be able to come back later. Don't assume someone else will fix it. Don't assume that human already knows.

**Your assistance in maintaining these protocols is greatly appreciated.**

---

## Development Philosophy

**Planning should take 10X longer than implementation** 

For big-picture full project planning, write and iterate the implementation plan document. Beware of dated agent training data with a bias towards figuring things out as you build; today's modern tools encourage modern workflows.

Implementation guide planning should involve:

  - Clear and concise documentation
  - Thorough research ensuring no assumptions 
  - Proving all knowledge is up to date 
  - Breaking down details into actual code
  - Many instances/eyes on implementation plans finding gaps 
  - Revision loops, closing gaps until plans are solid 

### Core Principle 

Project implementation guide preparation occurs in a research and planning loop until the plan becomes *EXCLUSIVELY EXECUTABLE*. This means that the agent following the plan to build requires no prior context, no guessing or looking anything up. If something needed to be figured out, it all should have been accounted for during planning. 

### Why Work This Way 

  * **The Challenge**: By design, LLMs expect answers come during active work. This stream of thought can blind one from recognizing gaps in knowledge, or considering if training data might now be out of date.

  * **The Solution**: This can be a strength, if handled during planning. Move "finding solutions" thinking out of active building, and into planning where there is opportunity for revisions and review.

  * **Why This Matters**: Even with extensive planning, something will fall through the cracks. This minimizes those instances helping to prevent time spent debugging later in the process. 

### Novel Benefits

  * **Concurrent Documentation**: Documentation is being created for multiple features simultaneously. 

  * **Parallel Development**: Becoming aware of exactly what must be done before acting allows for more work in parallel, which is certainly not something much training data accounts for. 

### Using Modern Tools 

Historic method of debugging placed code as extremely high-value, time consuming, exhaustive cognitive work. We have to question these workflows given an agent can write thousands of lines of lines of code, even an entire functional app, in a single output that just takes minutes to write. 

Agents can "go down the wrong path" when producing output. This means that whole rewrites of features or functions can be more effective than the historic methods of debugging, even if it makes the seasoned, pre-AI developers uncomfortable. 

Eliminating bugs by hunting-and-patching things together can result in a functioning application, but it creates a codebase that wasn't explicitly planned and now will be difficult to document. Agents work best from specifics where accuracy regarding the smallest details matters. 

Most significantly, agents have to work inside a context window, which doesn't bode well for trying to sort through patches and code adjustments. 

**Modern tools require modern workflows.**

---

## Versioning & Naming Conventions

### 1. Standard Version Number Format

A three-part semantic version is used: `vMAJOR.MINOR.PATCH` (e.g., `v3.1.2`). This is used throughout the life of the project, versioning from the first implementation guide document, updating it with each revision, and then matching the version numbers with the codebase itself allowing future feature updates to continue smoothly. 

| Position           | Bumps when                                                                |
| ------------------ | ------------------------------------------------------------------------- |
| **MAJOR** (first)  | Architectural rewrite, deployment-target change, breaking external change |
| **MINOR** (second) | New feature, capability shift, breaking-but-internal change               |
| **PATCH** (third)  | Bug fix, doc-only update, micro-tweak that doesn't change feature surface |

**Bumping a higher position resets lower positions to zero.** `v3.1.5` → next minor is `v3.2.0`, not `v3.2.5`.

A patch or minor bump is justified for docs-only changes if the changes were substantial and constitute a new artifact (e.g., a state snapshot, a feedback round closing). Trivial commit-message-level doc edits don't need a bump.

Updating docs-only examples: 

  * **Major**: Changing architecture (SwiftUI → PWA) 
  * **Minor**: Adjusting a feature (local only -> synched across devices)
  * **Patch**: Expanding implementation details (general steps -> production-ready code snippets) or (referencing a service -> actual API documentation or SDK usage)

Development example: 

  * **Major**: SwiftUI → Web rewrite; PWA → native shell
  * **Minor**: URL/anchor click handling; multi-window persistence
  * **Patch**: Single CSS fix for proper line numbering; correcting a typo in `THOT_APP.md`

### 2. Writing Version Delimiters

**Important**: Filenames are the only place that use underscores. Dots in filenames have caused git/tooling issues historically. Underscores are the necessary exception, and *only* in filenames.

| Where                                                   | Delimiter        | Example                      |
| ------------------------------------------------------- | ---------------- | ---------------------------- |
| Version strings (prose, headers, package.json, READMEs) | `.` (dot)        | `v3.1.2`                     |
| Git tags                                                | `.` (dot)        | `git tag v3.1.2`             |
| Commit messages                                         | `.` (dot)        | `fix(persistence): [v3.1.2]` |
| **Filenames only**                                      | `_` (underscore) | `v3_1_2_IMPLEMENT.md`        |

### 3. Git Tags

**Git tags use pure version numbers only. No suffixes. No extra labels for context.**

  - ✅ `v3.1.2`
  - ❌ `v3.1.2-single-draftpad`
  - ❌ `v3.1.2-fix`

Human-readable release labels go in:

  - The **commit message body** (`feat: … [v3.1.2]\n\nSingle persistent main draftpad fallback`)
  - The **GitHub Release** description (created from the tag, optional)

**Important**: If a tag needs to be re-pointed (a release was retracted and re-issued at a new commit), **never create `v3.1.2-v2` or similar**. Instead, delete the old tag and recreate it at the new commit.

### 4. Canonical Directory Structure

All planning documentation is created, updated, and remains forever in the `docs/archive`. The active feature plans or current and up-to-date implementation plan will always be the highest numbered version in the `docs/archive/vX_X` directory. This minimizes future document shuffling breaking any internal or external links to implementation plans, referenced paths, etc. 

**MUST have one subdirectory per MINOR versions, created based on needs and quantity of documents and revisions.** 

```plaintext
docs/
├── archive/
│   ├── images/                ← screenshots, diagrams (project-wide)
│   ├── resources/             ← reference, like tech docs used in a planning session
│   ├── v1_0/                  ← all v1.0.x artifacts
│   ├── v1_1/                  ← all v1.1.x artifacts
│   ├── v2_0/                  ← all v2.0.x artifacts
│   ├── v2_1/                  ← all v2.1.x artifacts
│   ├── v2_2/                  ← all v2.2.x artifacts
│   ├── v2_3/                  ← all v2.3.x artifacts
│   └── v3_0/                  ← all v3.0.x artifacts
├── research/                  ← (optional) only if active research is required — see Non-Archive Doc Directories below
├── planning/                  ← (optional) only if active non-implementation planning is required — see Non-Archive Doc Directories below
├── UPDATE_MAP.md              ← strategic roadmap (high-level only)
└── PROJECT_NAME.md            ← master architecture/state doc (living)
```
**Note**: The `docs/` path is canonical. Older drafts that referenced `assets/docs/` were inaccurate to actual repo structure.

#### Non-Archive Doc Directories

Only create `docs/research` or `docs/planning` if there is active, ongoing research or planning that the project requires. This is common, for example, when creating a business plan that requires data on market positioning, competition, etc. They should be maintained, moving older versions to `docs/archive/` as new versions are created as research is consolidated and redrafted into final documents. In these cases you might also need additional directories such as `docs/presentations`. In all cases, these additional directories should be archived and emptied of contents when they are no longer needed, moving the final results of those directories to more prominent, permanent locations, such as `docs/BUSINESS_PLAN.md` or for creative work for a client, `docs/BRAND.md`. Any documents containing things like API details or technical information for services that were pulled to inform the creation of implementation planning, should move to `docs/archive/resources/` and remain there. Otherwise, these directories need not exist.

#### Root Doc Files

The `docs/UPDATE_MAP.md` document is important when it is necessary for agents working on the project to see where the project is headed, so that we can build accordingly, setting ourselves up for success along the way, rather than having to double back and make larger changes in the future. 

### 5. Archive File Naming Protocol

We have five document types, with six filename variations. 

The filenames and formatting have been specifically constructed so that an alphabetical sort of all files in a version's directory reads like the chronological lifecycle of that version. A `ls` of any version subdirectory reads like a build log of what happened, in the order it happened.

**Each file type in a typical lifespan order might look like the following outline.**

  1. Human provides **`UPDATE_vX_Y_Z.md`**, a sketch or detailed outline of a major version proposal that now needs detailed planning. 
  
  2. Human provides Claude Code in Plan Mode the `UPDATE_vX_Y_Z.md` document, and Claude writes a plan for how to address all that is needed for the update, from architecture decisions that require discussions, to research online for best-practice methods. Claude creates an 'ephemeral session container' for the plan for human to review. When human approves the plan, Claude starts executing it immediately, and human grabs a markdown copy of that session plan and saves it as **`vX_Y_Z_DEV_PLANNING.md`** in the docs/archive/vX_X/ directory. Often, part of this plan will involve starting to write an IMPLEMENT.md document, which serves as a blueprint for implementation through the life of the proposed scope which may cross a number of versions, as they are created on the fly. Other times, a DEV_PLANNING session will be details on how Claude will approach investigating bug reports and then applying fixes, and so on. "DEV_PLANNING" does not imply either specifically planning or building, it is merely the plan the agent created *for that session* immediately executed as a guide or a to-do list for that session. Keeping this allows agents to look back and identify if anything fell through the cracks when trying to fix a bug or address how to implement a new feature improvement. 
  
  3. Agent creates and works with human to iteratively update and refine an **`vX_Y_Z_IMPLEMENT.md`**, which would be produced during a session and thus could be tracked using Claude Code's Plan Mode output which can be found in `.claude/plans/...` or later added by human to the version directory as a DEV_PLANNING document type or a SESSION_DEV document type. 
  
  4. Human, agent, or another subagent can provide **`vX_Y_Z_FEEDBACK.md`** documents to Claude Code to start a session. The feedback will be from a review of a build, perhaps testing, or evaluation of the ongoing iterative process of creating the IMPLEMENT.md file. The filename of FEEDBACK.md will reflect the version that the feedback is regarding. To keep the lifespan organization of files, a v1_1_0_FEEDBACK.md would be about a v1.1.0 document or build, but is placed in the version directory it drive the creation of, which would be v1_2_0_FEEDBACK.md in this example. To address a detailed FEEDBACK.md file, the agent might create a Claude Code Plan Mode 'ephemeral session container' that will immediately be presented to the human for approval and then followed to address all of the feedback according to the plan. If substantial, and particularly if the feedback requires and update to the IMPLEMENT.md document, the human will typically take the markdown copy of the session's plan for record keeping and save it as a DEV_PLANNING.md or a SESSION_DEV.md. If the feedback is not overly details, it might not require saving the session's plan to address the feedback, as the FEEDBACK.md document itself would suffice as documentation. 
  
  5. When human presents Claude Code Plan Mode with a completed and ready to build **`vX_Y_Z_IMPLEMENT.md`** document, Claude will assess the implementation plan and create an 'ephemeral session container' that details how they will process through the implementation guide. This might include things like spawning various subagents that work in parallel, the specific files that will be changed along with how they'll be changed, and information on how the session's work will be validated. While Claude Code will immediately begin executing this plan in the same session they created it, the human will grab a copy of it in markdown form and save it to the project directory so that it follows the IMPLEMENT.md document in chronological order by placing it in the same directory as the IMPLEMENT.md it is based on, and with the naming convention **`vX_Y_Z_SESSION_DEV.md`**. Again, it is *important to understand that SESSION_DEV and DEV_PLANNING are both plans that the agent creates FOR THAT SESSION and then immediately executes the plan*. They are named differently specifically to keep the file lifespan order within the project directories consistent with the chronological order of file creation, and human adds them to the project directory simply for historic record keeping. Claude Code and other agents would be able to access the same plan to look back at in a future session or to continue if it was not finished from `.claude/plans/...` or similar. 
  
  6. When human is reviewing a build and has feedback specifically in the form of bugs, they create a bug log and save it with the name **`vX_Y_Z_BUGS.md`** in the same directory as the build it is reporting on. On occasion, if the human is reporting bug issues directly to the agent in the chat, the agent will create the bug log document themselves and add it to the version directory. The same session that human started with Claude Code by providing the BUGS.md file, Claude will, as usual, create a plan that addresses how exactly they plan on exploring the bug, diagnosing the issue, and then fixing the problem. This same session the human will review this session plan and the the agent immediately begins executing it. As in previous sessions, the human will take a copy of the session plan and save it as a SESSION_DEV.md or DEV_FEEDBACK.md in the docs/archive/vX_X/ directory. This way if the fix doesn't work and it is being diagnosed again but by a different agent in the future, they can simply look back at the BUG.md file and the fill immediately preceding it to see exactly what had been attempted when Claude initially tried to fix the issue. 

#### Working Chronological Example 

Noted "human" for informal documents and "agent" for either 'ephemeral session containers' created of their to-do list plan for that specific session, in the case of DEV_PLANNING and SESSION_DEV, or a formal document created by agent with human during one of these planning sessions. 

Included is what might have occurred to require that session plan or implementation guide update, along with notation for when an update drove a version number change. 

```
docs/
├── archive/
│   ├── v3_0/
│   │   └── UPDATE_v3_0_0.md        # Human intro plan 
│   ├── v3_1/
│   │   ├── v3_1_0_DEV_PLANNING.md  # Agent's session plan involving research and notable expansion of the scope driving v3.0.0 -> v3.1.0
│   │   └── v3_1_0_IMPLEMENT.md     # Implementation guide that was started during the DEV_PLANNING session
│   ├── v3_2/
│   │   ├── v3_1_0_FEEDBACK.md      # Human review of implementation guide gaps in v3.1.0, driving v3.1.0 -> v3.2.0 
│   │   ├── v3_2_0_FEEDBACK.md      # Human found additional, smaller feedback for the plan, driving v3.2.0 -> v3.2.1 
│   │   ├── v3_2_1_DEV_PLANNING.md  # Agent received the feedback docs, created, executed this plan to address gaps
│   │   ├── v3_2_1_IMPLEMENT.md     # Implementation guide that was created during the DEV_PLANNING session 
│   │   ├── v3_2_2_DEV_PLANNING.md  # Human communicated gaps via chat; Agent planned, executed this session, driving v3.2.2 -> v3.2.3
│   │   ├── v3_2_3_DEV_PLANNING.md  # Plan created by agent planning another session to fix smaller gaps driving v3.2.3 -> v3.2.4
│   │   └── v3_2_4_IMPLEMENT.md     # Implementation guide that was created/updated during the two previous recorded session plans
│   ├── v3_3/
│   │   ├── v3_2_4_FEEDBACK.md      # Human reviewed v3.2.4 plan and suggested larger build change driving v3.2.4 -> v3.3.0
│   │   ├── v3_3_0_DEV_PLANNING.md  # Agent's plan of immediately executed session created new v3.3.0 plan integrating human's feedback
│   │   ├── v3_3_0_IMPLEMENT.md     # Implementation guide created during v3.3.0 session reflected in DEV_PLANNING plan 
│   │   ├── v3_3_0_SESSION_DEV.md   # Reviewing the implementation guide, this is agents plan to start execution
│   │   ├── v3_3_1_BUGS.md          # Human reviewed build, found issues, drove v3.3.0 -> v3.3.1
│   │   ├── v3_3_1_DEV_PLANNING.md  # Looking at bugs, agent created and immediately executed this plan that details fixes
│   │   └── v3_3_1_SESSION_DEV.md   # Agent needed second session for bugs, this is the plan they created and executed that session
│   └── v4_0/
│       ├── v3_3_1_FEEDBACK.md      # Human has notable feature updates driving v3.3.1 -> v4.0.0
│       ├── v4_0_0_DEV_PLANNING.md  # Provided with feedback, agent created and executed this plan updating the guide 
│       ├── v4_0_1_DEV_PLANNING.md  # This is a plan showing that agent had subagents audit the guide this session, driving v4.0.0 -> v4.0.1
│       ├── v4_0_2_DEV_PLANNING.md  # Plan immediately executed that fixed more audit issues, driving v4.0.1 -> v4.0.2 
│       └── v4_0_2_IMPLEMENT.md     # Copy of the implementation plan that was being updated in the previous few sessions, reflected in those plans 
├── BRAND.md                        # Voice, palette, etc. for branding 
└── PROJECT_NAME.md                 # Master architecture, context primer, general all-purpose document, not an implementation plan
```

**In almost all cases, human will be placing these files into their directories, with the exception of the IMPLEMENT.md guides that human comes to agent with feedback to use adjusting the plan during planning or development sessions; during these sessions, as reflected in the plan, the agent copies the old implementation plan and then updates it according to the evolving needs presented. Otherwise, agent will be presented with relative paths to all relevant documents when they start any session, at which point the agent creates a plan to address whatever is in those documents or communicated by the human.**

### 6. File Lifecycle (Read the Tree Above)

The Working Chronological Example tree in § 5 *is* the lifecycle. Read it top-to-bottom: each file's comment annotates which kind of session produced it and which version-bump it drove. The directory listing under each minor version reads in the order things actually happened.

**Nothing is deleted.** Superseded plans stay in the archive as historical record — including the ones that turned out wrong. Future agents need to see the dead ends to understand the live decisions.

### 7. Master Documents

Three master documents live outside the archive. Each has a single, explicit role.

| Doc                    | Role                                             | Updated when                          |
| ---------------------- | ------------------------------------------------ | ------------------------------------- |
| `docs/PROJECT_NAME.md` | Single truth source; current states architecture | Every non-trivial change ships        |
| `docs/UPDATE_MAP.md`   | Strategic roadmap; plan now by knowing future    | Direction shift; milestone finished   |
| `.agent/DEV_RULES.md`  | Rules of engagement; this document               | When a convention is added or changed |

  * If a fact in archive doc, from a **PAST** implementation guide version, conflicts with `PROJECT_NAME.md`, **`PROJECT_NAME.md` wins** (it's living; passed versions in archive are is frozen).

  * If a fact in archive doc, from the **CURRENT VERSION** implementation guide, conflicts with `PROJECT_NAME.md`, **archive doc wins** (The PROJECT_NAME.md was neglected after planning.)

---

## Session Document Handling

A `vX_Y_Z_SESSION_DEV.md` or `vX_Y_Z_DEV_PLANNING.md` file is the live working document for the session — marked off as work completes, not written as a report at the end. The human files the plan into the archive after a session; recognize one when you see it and treat it as the in-conversation plan you already have.

### During the Session

  - Mark checkboxes in `SESSION_DEV.md` as work completes — **as it happens**, not at the end.
  - If something unexpected comes up that changes the plan, note it inline at that step.
  - If a step turns out wrong and a new plan is needed, **stop and write a new `vX_Y_Z+1_DEV_PLANNING.md`**. Don't edit the IMPLEMENT.md silently mid-build.

### Footer Sections (Added at Session End)

Append these sections to the bottom of `SESSION_DEV.md` before closing the session:

  - **`## Session Notes`** — anything unexpected, undocumented surprises, or things that ought to be in `PROJECT_NAME.md` or `IMPLEMENT.md` but weren't.
  - **`## Picked Up From / Stopped At`** — exact resumption pointer for the next session. File:line, branch state, what's tested vs untested.
  - **`## Open Threads For Next Session`** — questions, deferred work, things the human or next agent needs to act on.

These footers replace what other systems call "post-session walkthroughs" or "completion reports." Keeping them inline at the bottom of `SESSION_DEV.md` means everything for that session lives in one file, in the right place.

---

## Git Branching Merging Protocol

We use a persistent `dev` branch for ongoing development integration, testing, and debugging.

We use `main` branch as strictly reserved for **production-ready, tagged releases**, that were **tested, are bug-free**, and ready for the **public to download** the application and use the digital product.

### Branch Structure

  - `main` — Production-ready code only. No direct commits; only fast-forward merges from stable releases.
  - `dev` — The primary integration branch for all ongoing development.
  - `feat/*` or `fix/*` — Temporary branches for specific features or bugfixes. Deleted after merge.

### 0. Initializing a New Project

When starting a brand new repository, you must create the `main` branch first before setting up the `dev` environment:

```bash
# 1. Initialize git and make the first commit
git init
git add .
git commit -m "chore: initial commit"

# 2. Assign the primary branch as main (if not default)
git branch -M main

# 3. Create and switch to the development branch immediately
git checkout -b dev
```

### 1. Starting a New Feature

Always branch off the latest `main` to ensure a clean slate:

```bash
git checkout main
git pull origin main
git checkout -b feat/your-feature-name
```

### 2. Ready To Ship Updates Merged To Main

#### Pre-flight checklist (before any of the steps below)

  1. All tests passing.
  2. `docs/PROJECT_NAME.md` reflects current architecture (per § *Master Documents*).
  3. `vX_Y_Z_SESSION_DEV.md` complete with footer sections (per § *Session Document Handling*).
  4. Version bumped in `package.json` if applicable.
  5. Build succeeds (`npm run build` or equivalent).

When the checklist is clean and the feature is tested, use this 5-step process to keep tags and remote repositories perfectly synced. **Tag is clean numeric only** — no suffixes (see § *Versioning § 3*):

```bash
# 1. Move to the production branch
git checkout main

# 2. Fast-forward main to your feature branch state
git merge --ff-only feat/your-feature-name

# 3. Push the clean update to remote main
git push origin main

# 4. Tag the release with a clean numeric version
git tag vX.Y.Z

# 5. Push the version tag to the remote
git push origin vX.Y.Z
```

### 3. Syncing the Development Branch

After successfully releasing a feature to `main`, keep `dev` up to date with the latest production state so parallel features don't drift:

```bash
git checkout dev
git merge main
git push origin dev
```

---

## Exclusively Executable Implementation Plans

Details from our **development philosophy**.

### Requirements for Every Update Document

#### 1. Research Phase Complete

  - All APIs/libraries documented with current best practices
  - Always find API documentation that is current; tech is moving faster than ever
  - Browser/platform compatibility confirmed
  - Performance implications understood
  - Alternative approaches evaluated
  - **Critical**: Don't presume you know how something works; verify with documentation and show proof in document 

#### 2. Architecture Decisions Documented

  - Why this approach over alternatives
  - What trade-offs were made
  - What constraints influenced the decision
  - Future update map considerations: work smarter not harder, this isn't a race

#### 3. Implementation Details Specified

  - Exact file changes needed 
  - Use production ready code snippets, NEVER PLACEHOLDERS, we have no hygiene policy 
  - Dependencies to add with their specific versions
  - Configuration changes required are finely detailed 
  - Test cases defined, planned, documented 
  - Edge cases identified, planned, documented 

#### 4. Common Pitfalls Identified

  - Known issues with this approach
  - Edge cases to handle
  - Rollback strategy if needed
  - Performance considerations
  - Write what a new instance with no context must know 
  - What might you have changed if you didn't know how development progressed 

### The Gap-Finding Loop

The point of "Exclusively Executable" is that the plan can be built without further clarification. The only way to *know* it's there is to have many cold sets of eyes try to find gaps before any code is written.

This is the operational layer of the *Planning takes 10× implementation* philosophy. Without it, the philosophy stays aspirational and agents revert to "this seems fine, let me code." With it, the philosophy becomes a process.

#### Process

  1. **Orchestrator** (the Claude instance you're working with directly) drafts initial `vX_Y_Z_DEV_PLANNING.md` and a draft `vX_Y_Z_IMPLEMENT.md`.

  2. Orchestrator spawns review subagents in alternating modes:

     - **Steered review** — orchestrator picks 3–5 categories specific to the project type (examples: "auth flow edge cases, payment failure modes, schema migration safety, file-system permission boundaries"). The subagent receives the plan plus those categories and reports gaps within them.
     - **Cold review** — subagent receives the plan with no categories, only the instruction *"find every gap, ambiguity, contradiction, or unverified claim."* The cold reviewer often finds things the steered ones miss.

     **Why both:** Steered reviews go deep on known risk areas. Cold reviews catch the unknown unknowns. We have observed non-Claude reviewers (e.g., a human's ChatGPT instance during product planning) catch significant gaps that focused passes had glossed over. Both kinds of pass have value.

  3. Orchestrator integrates real findings into a new `vX_Y_Z+1_DEV_PLANNING.md`, documenting which gaps were addressed and how (architectural decisions, research conducted, scope changes).

  4. Repeat — alternate steered and cold passes, **fresh subagent each pass** (no context contamination from prior reviewers).

  5. **Stop conditions** (any one):
     - Two consecutive passes return only nitpicks (define "nitpick" as: orchestrator can resolve without research).
     - An architecture-level decision surfaces — pause, surface to human, wait for direction.
     - Token / time budget reached — pause, ask human whether to continue.

  6. Human reviews architecture decisions and approves the final `vX_Y_Z_IMPLEMENT.md` before any code is written.

#### Why Fresh Subagents Matter

A reviewer who has already seen draft v1 of the plan unconsciously fills in gaps from memory rather than catching them. The cold-read reaction is the reviewer's whole value. **Always spawn a new instance per pass.**

#### Orchestrator's Role During Loops

Maintain context. The orchestrator is the one who notices when a reviewer's "gap" is actually an architecture-level decision masquerading as a detail (and therefore needs human input, not more research). The orchestrator also prevents *plan drift* — fresh reviewers don't know what the plan was for, only what it says, so they sometimes propose changes that subtly reshape the architecture. Steer them back, or surface to human.

### Template Structure

The template below is intentionally lean. Reference content does **not** live in `IMPLEMENT.md` — it lives in `docs/PROJECT_NAME.md` (architecture, glossary, current state) and `docs/archive/resources/` (API specs, schemas, integration docs pulled from external sources during planning). 

This is a deliberate forcing function: by sending the reader *out* of `IMPLEMENT.md` for context, we keep `PROJECT_NAME.md` honest and updated.

```markdown
# v[X.Y.Z] Implementation Plan

**Feature**: [name]
**Date**: [start date]
**Branches**: [feat/fix branch names]
**Required reading first**:
  - `docs/PROJECT_NAME.md`
  - `README.md`
  - [other archive/resources/* docs as applicable]

**Architecture decisions made in planning**: link to the `vX_Y_Z_DEV_PLANNING.md` rounds that produced this plan.

**If you find missing context as you read this**: `PROJECT_NAME.md` is meant to be living. Confirm with the human and update it; don't paper over the gap inside this document.

---

## Pre-flight Checklist
[Things that must be true before starting — env vars set, branch cut, deps installed, services up]

## Phase 0: Setup
[Concrete commands and file changes]

## Phase 1: [first executable chunk]
[Step-by-step. Each step file:line specific. Code snippets are production-ready, no placeholders.]

## Phase 2: [second executable chunk]
[…]

## Phase N: [final integration / wire-up]

## Verification
[How to confirm each phase worked — end-to-end and per-phase]

## Rollback
[Per-phase rollback strategy]

---

## Cross-references — NOT IN THIS DOC, find here:

- Tech stack summary, glossary, architecture diagrams → `docs/PROJECT_NAME.md`
- API schemas, integration docs, third-party service references → `docs/archive/resources/`
- Branch / merge / tag protocol → `.agent/DEV_RULES.md` § Git Branching
- Versioning, file naming → `.agent/DEV_RULES.md` § Versioning & Naming Conventions
- Session document handling → `.agent/DEV_RULES.md` § Session Document Handling
```

If you find yourself wanting to embed a tech-stack summary, schema, or glossary in `IMPLEMENT.md` because "the reader will want it here" — they will, but the right move is to put it in `PROJECT_NAME.md` and link to it. That keeps the system honest.

---

## Context Management Strategy

**Problem**: Agents working on specific features don't need the entire roadmap in their context because it's noise and wastes tokens. This is why we create exclusively executable implementation plans; it avoids any need to attempt to understand the build in entirety. That happens during planning only.

**Solution**:

  1. One single implementation plan exists, saved by version number in `docs/archive/vX_X/vX_Y_Z_IMPLEMENT.md`.
  2. In-flight work happens in `docs/archive/vX_X/vX_Y_Z_SESSION_DEV.md` (see § *Session Document Handling* for the full mechanics).
  3. Agent should read the master architecture document at `docs/PROJECT_NAME.md` and the project `README.md`.
  4. Agent does not need to read anything else unless referenced in their `IMPLEMENT.md` or in-flight `SESSION_DEV.md`.
  5. **Do NOT read `UPDATE_MAP.md`**: this is too much information that is COMPLETELY unnecessary, already accounted for in the `IMPLEMENT.md`.
  6. Mark off checkboxes on the in-flight `SESSION_DEV.md` plan as work is completed (see § *Session Document Handling* for footer protocol).
  7. After completion create detailed `commit` messages.
     - These should encompass the same grouping and wording as on the implementation guide used.
     - This ensures that we can look back and easily find and fix any bugs or necessary changes.

---

## Parallel Development Workflow

### Enterprise Company Best Practices 

This is how they do it and how we can too. This section is for context only; it does not need to be memorized, it should be used when creating implementation plans, where the actual parallel tracks need to be clearly defined. 

**1. Multiple Agents on Different Branches Simultaneously**

  - Agent A: `feat/paired-delimiters` (v3.0.0)
  - Agent B: `feat/file-operations` (v3.0.0)
  - Agent C: `research/custom-highlighter` (future)

**2. Merge Conflict Prevention**

Before starting parallel work:

  1. List all files each feature will modify.
  2. Check for overlaps.
  3. If overlap exists, sequence the work or refactor to separate concerns.

  Example:
  - Feature A modifies `src/editor.ts` (keymap section)
  - Feature B modifies `src/editor.ts` (extensions array)
  - Decision: Merge A first, B rebases and adds changes.

**3. Integration Protocol**

  1. Complete feature A, merge to `dev`.
  2. Feature B rebases on updated `dev`.
  3. Resolve conflicts (should be minimal if planned well).
  4. Run full test suite after each merge.
  5. Update roadmap with completion status.

**4. Detailed Change Logs**

Each agent's `vX_Y_Z_SESSION_DEV.md` documents:

  - What changed (file-by-file)
  - Why it changed
  - Git diff confirmation
  - Test results
  - Any unexpected discoveries

### Best Practices

**During parallel work:**

  - Each agent maintains a detailed change log.
  - Test in isolation on the feature branch.
  - Document any unexpected discoveries.
  - Don't merge until fully tested.

**Merging finished updates:**

  - Merge in order of completion.
  - Second feature rebases before merging.
  - Confirm no regressions.
  - Update documentation.

---

## Agent Documentation Standards

Every agent working on an update must:

  1. **Start with research** (even if it seems simple)
     - Read official documentation
     - Check current best practices
     - Verify browser/platform compatibility
     - Don't rely on training data alone

  2. **Document assumptions** and verify them
     - "I assume X works this way" → verify with docs
     - "This should be compatible with Y" → test it
     - "Performance should be fine" → measure it

  3. **Create before/after examples** for testing
     - What should work before the change
     - What should work after the change
     - Edge cases to test

  4. **Confirm changes via `git diff`** before marking complete
     - Review every changed line
     - Verify no unintended changes
     - Check for debug code or comments left behind

  5. **Update master architecture doc** if architecture changes
     - Keep `docs/PROJECT_NAME.md` current
     - Document new patterns or conventions
     - Update architecture diagrams if needed

  6. **Append session footers to `SESSION_DEV.md`** before closing the session
     - `## Session Notes` — surprises, undocumented findings, things that ought to be in PROJECT_NAME.md
     - `## Picked Up From / Stopped At` — resumption pointer
     - `## Open Threads For Next Session` — deferred work, questions
     - See § *Session Document Handling* for full protocol.

---

## Research Phase Best Practices

> **Note**: This section is provisional. A more complete research framework — extracted from the data-edger market-research cycle (`research/{1_DEEP, 2_FOCUS, 3_FINAL, QUALITATIVE}`) and the everlastings-website rounds-of-feedback pattern — is planned for a future revision of this document.

### When Research Is Needed

  - New API or library being introduced
  - Unfamiliar technology or pattern
  - Multiple implementation approaches possible
  - Performance implications unclear
  - Browser/platform compatibility unknown

### Research Deliverables

  1. **API/Library Documentation Summary** — what it does, how to use it, browser/platform support, known issues
  2. **Approach Comparison** — list 2–4 viable approaches; pros/cons; recommendation with reasoning
  3. **Implementation Sketch** — rough code outline, key integration points, dependencies
  4. **Risk Assessment** — what could go wrong, mitigation strategies, rollback plan

### Research Document Template

```markdown
# Research: [Topic]

## Question
[What we're trying to figure out]

## Findings
[What we learned from documentation, testing, research]

## Approaches Considered
### Approach A: [Name]
- Pros: ...
- Cons: ...
- Complexity: Low/Medium/High

### Approach B: [Name]
- Pros: ...
- Cons: ...
- Complexity: Low/Medium/High

## Recommendation
[Which approach and why]

## Implementation Sketch
[Rough code outline or integration points]

## Risks & Mitigation
[What could go wrong and how to handle it]
```

---

## Testing Standards

Before merging any feature:

  1. **Functionality tests** — core feature works, edge cases handled, error states graceful
  2. **Regression tests** — existing features still work, no unintended side effects, performance not degraded
  3. **Cross-browser/platform tests** (if applicable) — Chrome, Safari, Firefox; desktop, tablet, mobile
  4. **Documentation updated** — README if user-facing, technical docs if architecture changed, comments for complex logic

---

## Common Pitfalls to Avoid

### 1. Presuming Knowledge

- ❌ "This API probably works like this..."
- ✅ "Let me check the documentation to confirm..."

**Why**: Training data ages fast. Verify before you build.

### 2. Skipping Research Phase

- ❌ "This seems simple, I'll just implement it"
- ✅ "Let me research best practices first, even if it seems simple"

**Why**: Simple features often have hidden complexity. 30 minutes of research can save hours of debugging.

### 3. Not Documenting Decisions

- ❌ Implement feature, merge, move on
- ✅ Document why you chose this approach over alternatives

**Why**: Future developers (including future agents) need to understand the reasoning to maintain or extend the feature.

### 4. Ignoring Edge Cases

- ❌ "It works for the happy path"
- ✅ "I've tested error states, empty inputs, edge cases"

**Why**: Edge cases cause bugs in production. Handle them upfront.

### 5. Not Confirming Changes

- ❌ "I made the changes, should be good"
- ✅ "Let me review the git diff to confirm every change is intentional"

**Why**: Unintended changes cause subtle bugs. Always review diffs.

---

## Commit Message Standards

### Format

```
type(scope): brief description

Longer explanation if needed.

- Bullet points for multiple changes
- Reference issues: Fixes #123
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Formatting, no code change
- `refactor`: Code restructuring, no behavior change
- `test`: Adding tests
- `chore`: Maintenance (deps, config)

### Examples

```
feat(editor): add paired delimiter behavior

- Auto-insert closing brackets/quotes
- Skip over auto-inserted characters
- Wrap selection with delimiters
- Delete pairs when empty

Fixes #42
```

```
fix(highlighting): inline code markers now match content color

The tick marks for inline code were not adopting the red-orange
color. Fixed by adjusting CSS cascade order in theme.ts.

Fixes #38
```

---

## Syncing This Document

This file is intended to be the same across projects. To propagate updates from one canonical copy out to every other project:

```bash
frdoc -n /path/to/canonical/.agent/DEV_RULES.md \
      -s ~/Development \
      -r .agent/DEV_RULES.md
```

`frdoc` (find-replace-doc) lives at `~/Development/scripts/frdoc`. Run `frdoc -h` for full help. The same script can be used to sync any canonical doc across projects (for example, a master `BRAND.md`).

**Note**: Project-specific learnings live in `.agent/PROJECT_LESSONS.md`, which is **not** synced — every project keeps its own incident history.

---

## Agent Quickstart

The minimum protocol for any session, in order. If something here is unclear, that means this document failed — flag it.

### Read first (no exceptions)

  1. `docs/PROJECT_NAME.md` — context primer
  2. `README.md` — project status, deploy URL, commands
  3. Your assigned `docs/archive/vX_Y/vX_Y_Z_IMPLEMENT.md` — what to build
  4. `.agent/PROJECT_LESSONS.md` — incidents that shaped this project's protocols (skim)

### As you work

  5. Mark `SESSION_DEV.md` checkboxes live, not at the end.
  6. If the `IMPLEMENT.md` plan is wrong: stop, write `vX_Y_Z+1_DEV_PLANNING.md`. Don't silently patch.
  7. Confirm changes via `git diff` before commit. Every line intentional.

### Before closing the session

  8. Append `## Session Notes`, `## Picked Up From / Stopped At`, `## Open Threads For Next Session` to `SESSION_DEV.md`.
  9. If architecture changed: update `docs/PROJECT_NAME.md`. Future you depends on this.
  10. Commit with a message matching `IMPLEMENT.md` grouping (see § *Commit Message Standards*).

### What you do NOT do

  - Don't read `docs/UPDATE_MAP.md` during execution. It's noise.
  - Don't edit `vX_Y_Z_IMPLEMENT.md` mid-build. Write a new `_DEV_PLANNING.md` instead.
  - Don't write a separate "completion" or "walkthrough" file. Footers go in `SESSION_DEV.md`.
  - Don't create archive directories at the major-version level only (no `archive/v3/` — only `archive/v3_0/`, `archive/v3_1/`, etc.).
  - Don't write `IMPLEMENT.md` reference content (schemas, glossaries, architecture diagrams). Send the reader to `PROJECT_NAME.md` or `archive/resources/`.
  - Don't file `~/.claude/plans/<name>.md` into `docs/archive/`. The human handles that — they choose the version and filing path.

If you do these ten things and avoid those six, the protocol is satisfied.
