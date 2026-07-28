# Gap Review Loop

**Version**: v1.0.0 • **Last Updated**: 2026-07-28
**Purpose**: paring down the `DEV_RULES.md` document my moving the gap review protocol used to push a build guide to *exclusively executable* into its own dedicated document.
**Syncing**: one canonical copy, propagated to every project via `filemgmt` (§ *Conventions → Syncing*).
**Changelog**: at the bottom of this doc (one line per version) + full history in git — this masthead stays lean on purpose.

---

## THE FLOW

Takeing ideas, turning them into a "Build Guide", and going through all the necessary review passes to reach an exclusively executable plan. 

### Planning Start

1. **NEW PROJECT START OR PHASE** - a *high-level requirements* document identifies project goals; notes and/or details are provided from human for a loose plan to exist; any versioned files are all < v1 numbered, meaning ≤ v0.x.x. Continued build: an *update* plan describes the current state and how that should change in the next build guide creation, for the next phase of project development.
2. **IMPLEMENT.md begins** at ≥ v1 using the provided requirements and other details the first, or next major version numbered, IMPLEMENT.md document is created; the *goals* for the project, the 'north star' are always kept at the forefront of planning to ensure that all development is moving in the right direction; this helps each round of reviews to make sure that any missing functionality is identified as a gap avoiding something overlooked as being presumed to be left out by design. 
3. **DESIGNATED *Build Guide Orchestrator*** - the Claude Code thread that starts out building these build guide plans is renamed as the dedicated thread for this work. This maintains that purpose and the agent closes sessions by writing themselves /compact args and kick-off prompts for whatever phase is next in the overall Gap Review flow. They keep detailed *memory updates* through the process.
4. **DEEPENING THE PLAN SESSIONS** - the build guide orchestrator will continuing identifying areas of the plan that need deeper planning through *research, applying findings, surfacing important decisions*, and doing all that can be done to push the plan the being as complete as possible in these earlier planning stages.
5. **PLAN DEEPENING TRACKING** and other plan preparation tracking uses version numbers. Unless specifically stated by human, we can assume that each session that reaches completion for that session's goals in expanding the build plans drives the PATCH version number to increase by one. 
6. **PLAN DEEPENING COMPLETE** - when the orchestrator decides that they have gone through enough sessions building out the IMPLEMENT.md document then the formal build guide preparations in the next phase are to begin. 

### Pre-Gap Review

1. **DOCUMENT PREPARATIONS + CREATION** must first be completed by *separating out any addenda*, like ADDENDUM_TESTING.md or ADDENDUM_DESIGN.md, etc. as needed to keep the overall IMPLEMENT.md document from being overly cumbersome and digestible.
2. **SELF-GAP REVIEW** happens next; after the Build Guide Orchestrator had been compacted with appropriate arg message, that new instance completes the first pass of an agent looking for gaps in the plan. They *validate and fold in any of their findings*. Depending on the number of findings they should run this pass again until these "cheaper" rounds of reviews are exhausted. 
3. **ALWAYS RUN TWO BREADTH AGENTS** should be spawned to *review the build guide documents, which includes IMPLEMENT.md and the addenda*, every time they are updated significantly after findings are folded in. Just like the gap-reviews, any findings of the subagent breadth reviews should be validated and folded in, running these passes again until these "cheaper" rounds of reviews are exhausted.
4. **DRIVE VERSION NUMBER UPDATES** for *each loop completed*; these are typically just patch number increases, unless there is a larger functionality or other change in the planning. 
5. **COMPLETION DOCUMENT PREPARATIONS** include making sure that all of the IMPLEMENT.md and any ADDENDUM_x.md documents all have the *same version number*, for which the *minor value can be driven* up one so that the formal gap review documents all start in a new `assets/docs/archive/vX_X/...` sub-directory dedicated to those first gap type reviews. This should also include the *necessary REVIEW_PROMPTS.md in copy/paste ready format* with all necessary landmines, a clear list of the goals that have been collected over the span of starting and planning the project, and that start with paths directly to any documents that reviewer has available, if not the entire repository.

### Formal Gap Reviews

1. **A-TYPE GAP REVIEW** is the first gap review, and like all gap reviewers, is a new instance. This A-Type gap review is a check for *FUNCTIONALITY COMPLETENESS* and *SOUND LOGIC*, as it is a *COLD / SELF-CONTAINMENT* type review. This review is facilitated by human, run in the *CLAUDE.AI* workspace, has no access to the project repository, and is given only the build guide -- IMPLEMENT.md and the addenda -- and the architecture PROJECT_NAME.md document. This, and all GAP REVIEWERS deliver a `vX_X_X_GAP_REVIEW_{A/B/C/D}.md` when they are finished, providing the verdict of either NEEDS ANOTHER PASS or NEEDS ANOTHER NARROW PASS or READY TO BUILD. 
2. **VALIDATE AND FOLD IN FINDINGS** happens next when the *GAP_REVIEW_A.md is provided to the Build Guide Orchestrator*. After updating the build guide documents, they *run their two breadth review agents*, validating and folding in their findings as needed, as well. The agent should then update the REVIEW_PROMPTS.md as well as driving the version number by a patch value, assuming another A-TYPE GAP REVIEW is required. 
3. **LOOP AND CONSOLIDATE**: these A-Type Gap Reviews and the rest of the following loop tasks, continue and do not move on until a READY TO BUILD verdict is reached. When a number of A-type reviews have been completed and a "NEEDS ANOTHER PASS, BUT NARROW" verdict is reached, if it seems like the next A-Type Gap Review might be the last, then the *Build Guide Orchestrator does a document condense pass*. Typically this is after the build guide documents, meaning the IMPLEMENT.md and any addenda, have had quite a few surgical edits over time leaving behind stray references and mentions to previous version changes, with dated references often falling through the cracks as a result. Condensing things involves *removing the outdated notes, excessive context and any version changelog history* -- these are things that are not relevant when a plan is exclusively executable -- remove the excess archeology. Then run two breadth subagents, folding in changes. Then a straight read from end to end to be sure nothing is missing and everything flows the way it's supposed to. 
4. **GAP A READY TO BUILD VERDICT** will happen some time after this. The validation, folding in updates, and then two subagent breadth reviews are much the same as in previous loops. The only added task here is that, after the updates and breadth reviews are completed, the Build Guide Orchestrator drives the minor version number increase, copying the build guide documents into a new `assets/docs/archive/vX_X/...` sub-directory dedicated to the upcoming B/C/D-Type Gap Reviews, giving them a clean workspace. 
5. **B/C/D-TYPE GAP REVIEWS** are the next set of reviews that are this time run in parallel. Because the Build Guide Orchestrator is now able to spawn '*Peer Agents*' -- called that because they can be the same, or any, model, they share the same effort setting as the orchestrator, they have 1M context window and can be activated as a clean instance with no prior context -- that means *each of these three reviewers can be run by the Build Guide Orchestrator*; this was not always the case. All of the *B/C/D-Type Gap Reviews have access to all the build guides and the full repository*. The **B-Type** gap reviews are for fidelity and they check to make sure the quoted before/after blocks match the real code to ensure edits will apply cleanly. The **C-Type** gap reviews are for integration, and they check to make sure everything fits the wider system, scoping, idempotency, resource caps, conventions, AR conflicts, and checking for any stale pointers. Finally, the **D-Type** gap reviews are for the contract seam, making sure the payload contract line up surface <-> endpoint (pre-design), or the UI honor the addendum (render). 
6. **VALIDATE AND FOLD IN FINDINGS** happens after each B/C/D review loop until they all come up READY TO BUILD. *If one type is ready but not the others, the agent scopes down their REVIEW_PROMPTS.md* to make sure that, though that reviewer will still be in the subsequent loops until all three are complete, they know exactly what to be reviewing for and are aware that they had already passed a READY TO BUILD verdict. 
7. **LOOP UNTIL COMPLETION**: until with A-Type Gap Reviews, there is no document consolidation during the process that is strictly called for. Once all three, B/C/D-Type Gap Reviewers, report the READY TO BUILD verdict, after after the Build Guide Orchestrator has validated and folded in all their final findings, and even after the two breadth review sub-agents and finished, they then have the final build guide documents. 
8. **BUILD-GUIDE FINAL CUTS** are then to be made after every angle verdicts READY. Strip what's the *wrong context* for the execution orchestrator — changelog, *provenance*, slipped-scope rationale, resolved-edges, owner-decision tags, gap-review framing, excessive prose — keeping the byte-exact anchors. Move substantial rationale to a sibling **`_RATIONALE.md`** with a *"don't read the rationale unless you must"* note in the IMPLEMENT. Broad context genuinely helps an LLM, but as the guide nears ~100k tokens it shouldn't be *forced* — the same reason TESTING/DESIGN already live in addenda: the IMPLEMENT must *feel* manageable. This cut drives a *MAJOR* bump (plan → execution); a mere recent-build delta bumps *MINOR*.
9.  **BUILD GUIDES ARE NOW READY TO EXECUTE** and all organized for the fresh agent. They should include a recommended orchestration pattern, though the execution agent can just use that to make sure they're starting from an elevated place. 

---

## THE DOCUMENTS

### The "Build Guide" 

1. **`vX_X_X_IMPLEMENT.md`** - *where the plan lives*
2. **`vX_X_X_ADDENDUM_*.md`** - *additional scopes, constraints, and design specifications*

### Facilitatory 

3. **`vX_X_X_REVIEW_PROMPTS.md`** - *where the prompts for the review agents live and are updated*
4. **`vX_X_X_GAP_REVIEW_{A/B/C/D}.md`** - *where the results of the gap reviews live*

### Gap Review Completed

5. **`_RATIONALE.md`** - *where the 'true' rationale lives for the Build Orchestrator; only to be touched in absolute emergencies; no version number needed in the file name*
6. **`_KICKOFF_PROMPT.md`** - *where the prompt for the execution orchestrator lives; no version number needed in the file name*

---

## DETAILS 

## Orchestrator Hygiene 
*Compact forward, keep memories*

The Build-Guide orchestrator is long-lived across many rounds, so manage its context deliberately. **Immediately after** each fold + breadth pass + prompt-regeneration, **compact the session with a forward-looking note** — written as if the reviews it just prepped are already done — so a later `--resume` returns a correctly-compacted orchestrator (wait, and the compaction offered then can't be steered). Let the orchestrator author its own compact note (they do it well), and **keep memories** throughout — the loop is too long to hold in one context.

**The per-round cadence (peer-run gate loops):** each round ends with fold → the two breadth subagents → regenerate prompts → hand the human the **compact arg + a kick-off prompt** for the next round (templates in `.agents/GAP_REVIEW_WORKFLOW_PROMPTS.md`) — the human compacts and fires, which is guardrail 5. How many rounds to run between compacts is the orchestrator's judgment call; err toward one round per compact when a round also carries folds or protocol work.