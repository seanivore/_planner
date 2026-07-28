# Development Protocols

**Version**: v4.11.0 · **Last Updated**: 2026-07-22
**Purpose**: How we plan and build with agentic tools — planning to *exclusively executable*, proven by *fresh-instance gap review*. **Read THE CORE first**; the rest is machinery + reference.
**Syncing**: one canonical copy, propagated to every project via `filemgmt` (§ *Conventions → Syncing*).
**Changelog**: at the bottom of this doc (one line per version) + full history in git — this masthead stays lean on purpose.

---

## THE CORE — if you internalize nothing else, internalize this

Our whole method rests on one inversion of the old norm: **with today's tools, code is fast and cheap; the plan is where the work is.** ~90% of effort goes to planning, ~10% to building. Five things follow, and none are optional:

1. **Plan until the build doc is EXCLUSIVELY EXECUTABLE.** The agent who builds it needs *no prior context, no guessing, nothing looked up* — exact files, exact code, exact decisions, all written out. If the builder has to *decide* something, the plan failed; it could decide wrong.

2. **You cannot certify your own plan — fresh contexts do, under gate discipline.** A plan's author is the worst judge of its gaps (you fill them from memory without noticing). What the gate actually requires of a reviewer is a **fresh context + the full packet + a verbatim self-contained charge + an independent findings record** — not a particular window. Historically that meant a human-launched new session; since v4.8.0 a **B/C/D** pass may equally be an **orchestrator-spawned peer agent** run under the five guardrails in § *The Gap-Review Gate → Running the gate with peer agents*. **What is NEVER the gate:** reviewing the plan yourself, or "adopting the reviewer's perspective" — under pressure an instance will insist it can *be* the reviewer; it cannot — a reviewer that shares your conversation state or authorship is not fresh, however sincerely it role-plays independence. And **angle A additionally requires an environment that physically lacks the repo**: a spawned peer can be *told* not to look; only a no-repo tool *can't* — A stays external. (The pride fades fast: one fold of real findings and the instance turns eager to perfect the plan.)

3. **Your job as planner is to make the work maximally reviewable — not to judge whether it's ready.** Whether the gate runs is co-owned with the human and not yours to waive. Write everything out *so others can check it.*

4. **A residual gap always survives, and that's expected.** We *minimize* struggle; we don't chase a flawless doc. Gaps found in review are the process working, not a grade on your competence — which is exactly why the gate is safe to open wide.

5. **Don't build a big thing in one breath. Orchestrate.** Large work decouples into independently buildable chunks; an orchestrator delegates tightly-scoped tasks to fresh subagents. (Subagents for *parallel building* — distinct from the fresh-instance *review gate* in #2.)

Everything below is the machinery that makes these five real.

---

## How we work — the day-to-day

The operational orientation a fresh agent wants first: how a session actually runs. (The *why* is THE CORE above; the detailed mechanics are in the sections below.)

- **Start a project** from the two starter templates in `.agents/_TEMPLATES/`: copy `PROJECT_NAME.md` into the project as its living architecture doc (renamed for the project), and `README.md` as the base README. Never write either from scratch — they improve as the template is adapted, and `filemgmt` flows those improvements back to the fleet (§ *Conventions → Syncing*).
  - **Tend the memory.** Keep the project's auto-memory (`~/.claude/projects/<project>/memory/`) tended and trimmed. The two-orchestrator pattern — a planning thread, then a fresh execution thread — writes the best memories, because each writes for the next.
  - **Identify and maintain goals** for the project. These will be invaluable when it comes time for gap reviews and can be kept and expanded on throughout the project development at `assets/docs/GOALS.md`. 
  - **Never budget tokens.** You have 1M of context. Be context-smart by delegating large tasks to peer-agents, planning thoroughly ahead — but thorough, detail-oriented, pragmatic work beats speed, and corner-cutting is unacceptable. In Sean's words: *"There is no rush! If the plan for the session is large, just take your time, plan it out, delegate, and take it one thing at a time. You are not in a race and you are not trying to save up on tokens."*
  - **Skills are yours.** Globally-installed Agent Skills live at `~/.agents/skills` (not `~/.claude/skills`, where agents reflexively look and find nothing). Plan them in freely, and look up new ones whenever a task could use one — especially for design or a service we use. Smart surprises in the plan are welcome.
  - **CLI over MCP, always** MCP, often unreliable, incomplete, poorly documented. All CLI tool use is welcomed; always check their current docs before driving it. Available + logged-in: **Cloudflare** (edits DNS for `august.style` and `ckheals.com`), **Vercel**, **GitHub**, **Stripe**. If a useful CLI isn't installed, say so — the answer is undoubtedly, yes, and any needed keys or tokens can be arranged to increase usefulness of permission scope. Verify by driving the real thing (§ *Conventions → Agentic testing*).

- **Wrap a session** by assessing and updating any living docs to reflect reality: the architecture 'PROJECT_NAME.md' document, any active IMPLEMENT.md, the README.md, etc. Leave them current for the next thread. 
  - **Git is the progress log.** Commit often — typically per file — with descriptive, per-file detail; the history *is* the record of what happened (no separate session-log or build-report file). Push freely to `dev`; **never to `main`** without explicit sign-off (§ *Git Branching*).
  - **Tend the memory.** Keep the project's auto-memory (`~/.claude/projects/<project>/memory/`) tended and trimmed. The two-orchestrator pattern — a planning thread, then a fresh execution thread — writes the best memories, because each writes for the next.
  - **Git is the progress log.** Commit often — typically per file — with descriptive, per-file detail; the history *is* the record of what happened (no separate session-log or build-report file). Push freely to `dev`; **never to `main`** without explicit sign-off (§ *Git Branching*).
  - **Compaction is the handoff.** You write your own compact arg **when Sean asks** (he watches the context and requests it while it's fresh) — not proactively, and not into a file. Long is fine; context restarts at 0% after a compact, so carry everything forward. A worked model lives at `.agents/_EXAMPLES/COMPACT_ARG_example.md`.

---

## Development Philosophy (the why)

### Agentic coding in 2026 is 90% planning, 10% building (THIS IS NOT EXAGGERATION)

| Activity | Effort |
| -------- | ------ |
| Planning | 90%    |
| Building | 10%    |

Agents write thousands of correct lines in minutes and can one-shot a working app. The old high-cost "writing code" step is cheap now, so the value moved upstream to the plan. The old **debugging phase** — an unbounded, unpredictable "figure it out later" space — is precisely what good planning *replaces*: you move the uncertainty to *before* the code exists, where it's cheap to resolve and safe to review.

### The new failure mode: agents go down the wrong path

The risk today isn't slow typing — it's an agent confidently heading the wrong way on one small *unstated* thing and compounding it. Two consequences:
- **Whole rewrites often beat bug-hunting.** A clean re-generation from a correct spec is frequently faster and cleaner than patching; patched bugs compound in hard-to-trace ways and leave a codebase later agents can't reason about.
- **Specificity is everything.** The smallest unstated detail is where it breaks. Hence: exclusively executable.

### Question whether your training data is dated

LLMs are built to produce an answer in the moment, which can blind you to a gap in your own knowledge. Tech moves fast; the API you "know" may have changed. The discipline: don't *find the solution while building* — find it while **planning**, where there's room to verify against current docs and be reviewed. Prove current knowledge in the doc (link the real API docs). Never present training-data recall as fact.

### The Agentic Orchestration Paradox

The larger a task, the more an agent instinctively *shrinks* it to stay in its comfort zone — managing context, avoiding losing the thread. Unchecked, it tries to do a massive plan in one stream of consciousness and drops context / writes bugs. The only way to execute big work well is to make the agent a high-level **orchestrator** that aggressively delegates deterministic, tightly-scoped tasks to fresh subagents. Decouple complex plans into independent tracks; the orchestrator runs the pipeline, subagents do the typing.

---

## The Gap-Review Gate (the heart of the method)

"Exclusively executable" is a *claim*. The only way to know it's true is to have cold eyes try to break it before any code is written. This is the operational core of the 90/10 philosophy — without it, the philosophy stays aspirational and agents revert to "this looks fine, let me code."

### Two things people conflate — keep them apart

|              | In-session breadth pass                                | The fresh-instance gate                                               |
| ------------ | ------------------------------------------------------ | --------------------------------------------------------------------- |
| **What**     | the orchestrator spawns a subagent to review a chunk   | a fresh context reviews the full packet under gate discipline         |
| **Where**    | ad-hoc, orchestrator-framed prompt; findings inline    | A: a no-repo tool, human-launched · B/C/D: **disciplined peer agent** |
| **Good for** | breadth, first-pass cleanup while drafting             | *certifying* exclusively-executable before promotion                  |
| **Limit**    | orchestrator-framed = shares the session's blind spots | — this is the real gate                                               |

Breadth passes are useful early. **They are not the gate.** The line is no longer *who launches the reviewer* but *the discipline it runs under*: an ad-hoc subagent with an orchestrator-authored prompt is never the gate; a peer pass is gate-grade **only** under the five guardrails (verbatim block, new instance per pass, own findings file read raw by the human, every pass kept standing, human-kicked round). An orchestrator that "reviews its own plan" — or spawns reviewers outside those guardrails — has **not** passed the gate, however thorough it was.

### Access determines what a reviewer can even find

A reviewer can only catch what its *access* lets it see. Run A/B/C at least once — complementary, not redundant — and D per its artifact test (§ *Angles*):

| Angle                           | Access                         | The only thing it can truly test                                                                               |
| ------------------------------- | ------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| **A — Cold / self-containment** | the plan doc ONLY, **no repo** | Is it *actually* exclusively executable? Anywhere it must open a file, guess, or recall is a defect.           |
| **B — Fidelity**                | plan + repo                    | Do the quoted before/after blocks match the real code? Will edits apply cleanly?                               |
| **C — Integration**             | plan + repo + architecture doc | Does it fit the wider system (scoping, idempotency, resource caps, conventions, AR conflicts, stale pointers)? |
| **D — Contract seam**           | plan + repo + design addendum  | Does the payload contract line up surface↔endpoint (pre-design), or the UI honor the addendum (render)?        |

**A-Type Gap Reviews** were initially created to confirm product completeness; are there features missing or not completely planned based on the goals of the project. 

The **cold / no-repo** reviewer is the only one that can *prove* self-containment — it physically cannot fill a gap from the code, so wherever it gets stuck is exactly where your builder would have to guess. A subagent inside your session can never be angle A. Non-Claude tools are excellent at A (paste the doc, no repo). B, C, and D run as fresh-context instances in the repo — human-launched windows or orchestrator-spawned peer agents under the § *Running the gate with peer agents* guardrails.

### The loop

1. **Orchestrator** drafts/extends the IMPLEMENT (living; sessions refine it). Optional: in-session subagent passes for first-draft breadth.
2. **Run the gate:** fresh instances, one per angle, **a new instance per pass** (no context contamination). Each writes findings to a file (`vX_Y_Z_GAP_REVIEW_<angle>.md`); a no-filesystem tool prints the full file contents to paste in.
3. **Orchestrator folds** real findings into IMPLEMENT (the fold recorded in a descriptive git commit). A "gap" that's actually an architecture decision → pause, surface to the human (don't research it away). Watch for *plan drift* — fresh reviewers know only what the plan says, not what it's *for*, so they sometimes reshape the architecture; steer back or escalate.
4. **Repeat** until each angle's fresh pass returns nothing load-bearing.
5. **Stop conditions** (any): each angle finds only nitpicks (resolvable without research); OR an architecture decision surfaces (→ human); OR token/time budget hit (→ ask human).
6. **Gate clears → human approves → execute.** No code before this. The gate-cleared IMPLEMENT *is* the executable guide, run directly (the orchestrator parallelizing via subagent groupings). Deviations found while building are captured in descriptive git commits and fold back into the next IMPLEMENT round.

**One drafting session is never the finished plan.** A pass that finds *anything* means you're not done — loop until a *fresh* pass of each angle finds nothing load-bearing. Subagents surfacing gaps is **not** permission to stop; it's proof there's more to find. The 90/10 ratio is **literal, not hyperbole**: an exclusively-executable plan takes multiple rounds, usually across multiple sessions. (Drafting/refining with one agent over several rounds is normal and good — *then* the fresh-instance gate certifies it.)

**Re-run the breadth pass on every version-push, and expect non-monotonic convergence.** The cheap in-session subagent pass (owner-journey + integration, through the project lens) is worth running after *each* fold — not only while first-drafting. A fold can silently introduce the next bug, so the doc that just changed is exactly the doc to re-scan; it's near-free insurance against regressions the fold itself created. It stays **advisory** — never a substitute for the fresh-instance gate. And convergence is **non-monotonic**: a clean-looking pass can still surface something, and a fix can *create* the next finding (in the Everlastings v1.5 gate, an 8th-round data-loss bug was introduced by the 7th round's own fix). That's why the exit condition is *a fresh pass that finds nothing load-bearing* — never *it feels close*.

### The functionality-design pass (design the logic BEFORE you gate it)

**Gap review validates a plan; it does not design one.** When a slice carries genuinely novel functional logic — a flexible payment schedule, a Google-Docs-style collaborative editing model, anything where "how it should work" is itself unsettled — trying to evolve that logic *through gap-review folds* is using a proofreading tool to write the chapter. It reads as slow at best and convoluted chaos at worst: each reviewer sees a different corner, the folds pull in different directions, and the design never coheres. (Learned on contract-payments: the payment model was underspecified, so the gap-review loop *became* the place it got designed — exactly the friction this rule exists to prevent.)

So for such logic, run a **dedicated functionality-design pass** — a peer instance whose job is to design *how the feature works* and emit one focused design doc — and slot it into the loop at a fixed point:

1. Orchestrator drafts the IMPLEMENT + build-guide docs.
2. Two in-session breadth subagents (they will often *flag the need* for step 4 — an "this logic isn't designed yet" finding is the trigger).
3. Compact → fresh instance re-reads and self-gap-reviews.
4. **The functionality-design pass runs HERE** — after the fresh self-review has surfaced what's under-designed, before the certified A/B/C gate. It designs the novel logic; its output folds into the IMPLEMENT (and the architecture doc).
5. Fold those design outputs + any gap findings → re-run the breadth subagents → *then* the fresh-instance gate.

Boundaries: it is **distinct from the fresh-instance gate** (design vs. validate) and **distinct from the Claude Design collab** (functional logic vs. visual treatment — different axis; they can run in parallel on the same slice). It fires only for slices with real novel logic — infra/mechanical slices (stand up services, run migrations) don't need it. And the schema a novel feature will need must be made *accommodating* at its foundation slice even before the design pass runs, so the later design lands without a migration.

**Hunt your own gaps first — a pre-loop step, before even the breadth pass.** Before spinning up *any* review (the in-session breadth pass or the fresh-instance gate), take a deliberate step back and read the plan/IMPLEMENT clean yourself, grounding each gap by actually opening the reuse surface (not recalling it from memory), and fold what you find now. This is THE CORE #3 ("make the work maximally reviewable") applied early: the more the orchestrator surfaces on its own, the fewer rounds the loop needs and the faster it converges — a real lesson from the v1.5 gate, where skipping this (and the breadth pass) stretched the loop. It does **not** substitute for the gate (THE CORE #2): a self-read shares your blind spots, so it just clears the cheap-to-find gaps and lets cold eyes spend their value on the deep ones. **Non-negotiable: the gate is never the first reviewer.** Don't open a fresh-instance pass (or fire the courier tool) until real gap-hunting has happened — your own clean read-back, the breadth pass, ideally a day's distance. There are always gaps; the cheap ones get found by cheap means first, or you burn fresh-reviewer rounds (and human loop-time) re-discovering basics.

### Why this isn't the orchestrator's call — and why that's fine

Asking a plan's author to commission a hard critique of its own work is a conflict of interest — which is why the planner-confidence reflex ("it's ready," "I'll handle review myself") shows up *right here* and nowhere else in planning. So the decision to gate is **co-owned with the human and not waivable by the orchestrator.** And it's *safe*: a residual gap always survives (THE CORE #4). Found gaps are the method working. Make the plan maximally reviewable; don't defend it.

### The reviewer prompts — auto-generated, paste-ready, one self-contained block per angle

When the gate opens, the orchestrator **generates a `vX_Y_Z_REVIEW_PROMPTS.md`** — don't hand-write prompts each round, and don't make the human assemble pieces (the v1.5 loop burned several rounds before realizing the landmines had to be *inside* each prompt). One **fully self-contained, copy-paste block per angle**, each carrying everything the reviewer needs inline:

1. **The three-part review lens (all three, every angle).** A single lens shrinks the scope and reviewers skip whole files / the design — the hard v1.5/1.6 lesson. So:
   - **(a) the product North Star / thesis** — the *value* lens ("can the user actually do this, least-friction?"); a capability that reads "covered" but isn't truly drivable is a real gap. **The North Star is the *current build's*** — the problem this build/phase solves — not a fixed project-wide star; it evolves across phases, and an angle may tailor it (D's is design-flavored).
   - **(b) the broader mandate** — *the North Star is the primary functionality lens, NOT the filter for what counts as a gap.* Search the **whole build (the IMPLEMENT + every addendum), every element** — *"do not neglect the design addendum"* even if you're "not the design reviewer" (the value is many eyes on the *whole*, not tidy specialization) — for anything not truly **exclusively-executable**, any **unvalidated assumption**, and any **design-correctness** failure (the "columns-bug" class — a spec that *applies* cleanly but *renders wrong/incomplete*). Don't let the lens narrow what gets reviewed.
   - **(c) read-in-full, co-design, flag-don't-assert** — read ALL provided docs **end-to-end** and **don't ration tokens** (max effort; context is managed for you; grep/jump reading has produced real mis-diagnoses). **Co-design, don't just audit:** a gap is a gap whether it's a flaw in what we wrote OR something the build should address but omitted — don't assume it's mostly complete. **Flag-don't-assert:** when a finding depends on runtime/code you can't see, FLAG it *needs-verification* — never assert it "broken" (whole rounds of false alarms came from confident "broken" calls the code disproved; on a delta, prefer "I can't verify X" over "X is broken" — it cuts both ways). The orchestrator then **validates before folding**; only verified findings become landmines.
2. **The landmines = the "Settled — do not re-raise" ledger** (the project's hard-won truths — "validate against reality, not training data"). **Current-only + bounded: every entry is a *verified* prior-round finding; a superseding fold *replaces* its entry (never append a contradiction — no mixed truth); prune at each condense.** Append each newly-verified landmine as the loop runs. **Inline the ledger INSIDE every angle's block — deliberate duplication.** A block gets copied out of the prompts file alone; a shared top-of-file ledger has been missed at launch time more than once (by human *and* agent), and a review run without its landmines burns a whole round re-discovering settled truth. The flow: the ledger lives in the A block and updates there each A-loop round until A passes; when the B/C/D round is prepped, their blocks start from the last A-loop ledger; from then on **every fold updates every copy, identically** — the regeneration step is "update the ledger in EVERY block," never "update the ledger." (Everlastings' v1.5 A-loop grew it ~6→36, then v3.2 carried it to ~68; that accretion is what drives a build near-flawless.)
3. **The angle's specific charge** (below).
4. **The OUTPUT spec + the explicit instruction to WRITE the report** to `vX_Y_Z_GAP_REVIEW_<angle>.md` (a no-filesystem tool prints the full file to paste back). The reviewer changes nothing else.
5. **All docs delivered whole, and what to hand each angle.** The reviewer never opens or hunts a file it wasn't handed (angle A *can't*). For the cold-A web-UI run, **upload the doc FILES to the chat** (IMPLEMENT + every addendum + the living architecture doc as supplementary cross-check) — don't paste doc bodies into the prompt text; they run tens of thousands of tokens and uploads are how it's actually done. The prompt block itself carries only the instructions + inlined ledger, and **names the exact files being uploaded** so the reviewer knows its packet. Per angle: **A** = the uploaded docs only, **NO repo** (the absence is the point); **B/C/D** = the repo + the docs (Claude Code at 1M); **C** reads the architecture doc first; **D** reads the design research + feedback screenshots. For a **clean rebuild**, the *superseded* legacy architecture doc is handed to NO angle — it's concentrated stale truth and contaminates a cold read; its value was mined during planning. An **optional final cold-A holistic pass** on the B/C/D-clean docs (docs only) is welcome — optional even for peer-run loops (§ *Running the gate with peer agents*: peers are full fresh instances — equivalence, not compensation).
6. **No manufactured persona; the lines are counter-reflexes.** Lead with the *task + lens*, not "you are a senior engineer…" (the model gets the role from context — drop the cosplay; a surviving role line stays minimal/functional). And treat the loud, repeated instructions — *read in full, don't ration tokens, don't shrink scope, flag-don't-assert* — as **counter-reflex instrumentation**: each overrides a model instinct to do less (skim/grep, narrow-to-my-lane, assert-from-training, budget-tokens). **Keep them in every block; never trim them as "redundant"** — a tidy-up quietly reopens the exact narrowing they beat back.

**Name the settled base (for a delta build).** When the build is a *delta* on a shipped/tested system (most incremental work), every prompt states it: *"the current system (all of [the architecture doc] + the repo as it stands today) is built, tested, and live/approved — the fixed substrate; this build is a delta on top. Review the delta for gaps + whether it FITS the base; don't re-litigate, redesign, or flag settled/shipped behavior — a finding must be about what this build adds/changes, or a real conflict it creates with the base."* This stops reviewers manufacturing blockers out of proven code (the v1.6 loop explicitly warned against re-litigating the closed A-loop).

**Angles** (run A holistically/looped first or independently; B+C(+D) in parallel; a **new instance per pass**):
- **A — cold / no-repo (self-containment + completeness):** the docs only — the absence of a repo is the point; every place the builder would open a file, guess, recall a library, or decide.
- **B — fidelity (repo):** every CURRENT block byte-matches the tree; every NEW block applies cleanly + references only things that exist. Byte-check the **design addendum's** DECIDED blocks at the same bar (for render-tuned defaults, judge "concrete enough the builder never guesses," not whether it's final).
- **C — integration (repo + architecture):** read the living architecture doc first; system-fit gaps (scoping, idempotency, auth, resource caps, AR conflicts, stale pointers) **through the lens**, incl. design integration. **Boundary vs D: C audits the pipes** — whether a call can happen at all (routing/rewrites, auth/role gating, ordering, platform limits); what the call *carries* is D's. Report cross-lane findings anyway, tagged with the lane you think owns them.
- **D — interface-contract & design-correctness:** D's object across the build's life is the **contract seam** between frontend surfaces and the API. It has **two charges, chosen at prompt-generation time by artifact test — the reviewer never picks a mode**; each D block carries exactly one charge. **Contract charge** (fires when the packet has an API surface + a surfaces inventory but no executable-depth UI spec or pinned seam — a pre-design build): walk every surface × every endpoint **both directions** — every surface's data need traced to a providing endpoint, every endpoint's response to a consuming surface; orphans either way are findings; machine-called endpoints (webhooks, inbound email, crons, OAuth callbacks) are pre-exempted in the block or the first round burns on false orphans. Anchor completeness to artifacts *outside* the two docs being compared (the status machine, the notification/send inventory) — a need BOTH docs omit balances the matrix and sails through otherwise. Judge direction-only slices at direction depth (endpoint names + data ownership; depth findings against them are out of scope, stated in the block). Run the **handoff-executable test**: could the Claude-Design packet (`data-flow.md` + `data.js` mock + written UX descriptions per `CLAUDE_DESIGN_COLLAB.md`) be drafted from the docs alone with zero invented shapes? The verdict is **lane-scoped** — "CONTRACT READY" certifies doc-coherence + seam-draftability, NOT wiring; wiring is proven later at seam-pin (a scoped D re-open when `data-flow.md` lands), foundation-slice execution, and the first CD integration lap. **Render charge** (fires when executable-depth UI spec exists — DECIDED addendum blocks, CD output, or built UI): the design-correctness review — *renders right, accessible (reduced-motion/screen-reader), responsive, matches the design addendum*. Both artifact classes present: render charge, plus the contract walk scoped to not-yet-pinned surfaces. Neither present (genuinely UI-less build): D doesn't fire — fold design into B + C, **and then the prompts must say explicitly "the design addendum is always in scope; do not skip it"** (lumping-in silently dropped the design review in the first v1.6 round). **Boundary vs C: D audits the payload contract** (shapes, fields, ownership, mock fidelity, draftability); the pipes are C's. Contract-charge D may run **standalone** (no B/C alongside) — same packet, independent axis — provided it runs against a stable doc version (never launch mid-fold).

**Then:** fold findings → bump → **regenerate the prompts** (carrying the updated ledger) → repeat until each angle's fresh pass finds nothing load-bearing. **B/C/D close angle-by-angle, not in lockstep:** an angle that returned `READY TO BUILD` re-runs *only* when a fold lands in **its** lane — and is then **scoped + narrowed to exactly the folded changes** (told it passed, given the named change, asked to confirm *only* that, and told explicitly the narrow is *deliberate, not a violation* of the don't-shrink mandate — a finished reviewer left unscoped will keep digging and make something out of nothing). **The orchestrator assigns each finding's lane at fold time — lane assignment, not which reviewer reported it, drives re-opens** (reviewers report cross-lane findings tagged; the pipes/payload boundary between C and D decides ownership). A passed angle at max effort will always find one more "polish" nicety; reflexively folding those manufactures work + new seams. If a round's folds touch no lane, that angle **stays closed**; the 2-subagent breadth pass is the cross-lane backstop. Every `vX_Y_Z_GAP_REVIEW_<angle>.md` is kept standing (the rigor trail); the `REVIEW_PROMPTS` file is current-only (rename on bump). A full worked example: `assets/docs/archive/v3_2/v3_2_3_REVIEW_PROMPTS.md` (Everlastings' v3.2 management-features gate — richer than the older `v1_6/v1_6_2_REVIEW_PROMPTS.md`).

### The per-block skeleton (the generator fills this)

Run angle A in a no-repo tool; B/C/D in fresh repo instances.

```
You are a senior engineer doing a pre-build gap review. Effort: maximum. Do NOT change code or existing docs — your only output is your findings (write them to `[vX_Y_Z_GAP_REVIEW_<A|B|C|D>.md]`, or print the full file contents if you have no filesystem).

CONTEXT
- `[path to the IMPLEMENT]` is a packet a FRESH agent will execute against [this repo / from the doc alone], then test on [preview]. It is meant to be "exclusively executable": it embeds the exact current code and exact replacement for every edit, so the builder only LOCATES and APPLIES — never DISCOVERS or DECIDES.
- [Project landmines to respect — the hard-won truths, e.g. "the X integration's public docs are wrong for the loaded bundle; the real surface is Y." Give the reviewer these so it validates against reality, not training data.]

ANGLE — [pick one]
- A (cold / no-repo): you get ONLY the doc; its absence of a repo is the point. Find every place you'd have to open a file, guess, recall a library's behavior, or make a decision the doc didn't make for you.
- B (fidelity): open the files the doc edits; verify every before-block matches the repo and every after-block applies cleanly.
- C (integration): read `[architecture doc]` first; hunt system-fit gaps ([scoping], [idempotency], [resource caps], AR conflicts, stale pointers).
- D (contract seam — ONE charge per block, chosen by the generator's artifact test): contract charge — trace every surface↔endpoint both directions ([machine-called endpoints exempt]) + the handoff-executable test; OR render charge — the built/specced UI against the design addendum (renders right, accessible, responsive).

OUTPUT
- A gap list RANKED by how likely each is to derail the build: location, what's wrong/missing, the concrete fix.
- The single most important "if you fix one thing" insight.
- One-line verdict — three values: READY TO BUILD / NEEDS ANOTHER PASS / NEEDS ANOTHER PASS (NARROW). NARROW = "almost there, only a bounded area left" — load-bearing for planning: it triggers the end-game clean-up read and lets the next pass be scoped tight.
Be concrete: "Phase X mounts `[selector]` but Phase Y never defines it" beats "check the selectors."
```

### Why fresh instances (not re-used ones)

A reviewer who already saw draft v1 unconsciously fills gaps from memory instead of catching them. The cold-read reaction *is* the value. **A new instance per pass.** And different instances genuinely differ — different recall, different instincts, different paths — the same reason a team brings diverse people to a hard review.

### Running the gate with peer agents (B/C/D)

**Peer agents — deliberately NOT called "subagents"**: since the platform upgrade they are full agents, equal to the orchestrator — a **fresh, clean context** every pass (none of the orchestrator's conversation state), the **orchestrator session's model + effort**, and the full **1M context**. The rename marks the equivalence: these are the same fresh instances the human-launched terminal windows were. A gate round is only as strong as the session that fires it, so fire gate rounds from a max-effort session. (Angle A is unchanged by all of this — it stays the human's own no-repo run, pasted into Claude.ai.) That makes peers gate-grade for the repo angles, under five non-negotiable guardrails (each compensates for a specific thing the human-launched flow provided for free):

1. **The verbatim block IS the entire prompt.** Copy the angle's block out of `REVIEW_PROMPTS` byte-for-byte — nothing prepended, appended, or trimmed. (Generating the standing `REVIEW_PROMPTS` file at fold time is the mandated, human-visible step — § *The reviewer prompts*; the leak is ad-libbing at SPAWN time, off the audited artifact.) An orchestrator "tightening" a reviewer's prompt at launch is the conflict-of-interest leak, and the counter-reflex lines are the first thing a tidy-up deletes.
2. **New instance per pass** — unchanged from the human-launched rule.
3. **Independent findings transport.** The reviewer writes its own `vX_Y_Z_GAP_REVIEW_<angle>.md`; the human reads the FILE, never the orchestrator's summary of it; in chat the orchestrator quotes verdict lines verbatim, never paraphrased.
4. **Every launched pass is numbered and its report kept standing — none discarded** (a crashed pass is recorded as crashed). This kills verdict-shopping — re-rolling reviewers until one comes back clean; the standing rigor trail is the audit log the human can count.
5. **The human kicks off each round** — the co-owned gate decision (THE CORE #3) survives as the compact-arg + kick-off cadence (§ *Orchestrator hygiene*).

**Angle A stays external.** A peer can be *told* not to open the repo; only a no-repo tool physically can't — and that physical absence is A's entire value. **A peer-run B/C/D loop needs no compensating external pass** (Sean's call, 2026-07-21): peer agents are full agents — clean context per pass, the orchestrator's model and effort, 1M context — the SAME fresh instances the human-launched terminal windows were; what changed is who transports the prompt and the findings, and the five guardrails protect exactly that. The final cold-A holistic pass on the clean docs (§ *The reviewer prompts*, item 5) stays **optional**.

### End-game cleanup (the last stretch)

Two condensing passes shed the scaffolding as the loop converges — both exclusively-executable work, not optional polish:
- **Clean-up read (before the suspected last, NARROW, A pass).** Round-after-round surgical edits leave stray/outdated references; do a deliberate **end-to-end read** to catch them and condense the doc, then a breadth-subagent pass.
- **Build Guide Final Cuts (after every angle verdicts READY).** Strip what's the *wrong context* for the execution orchestrator — changelog, **provenance**, slipped-scope rationale, resolved-edges, owner-decision tags, gap-review framing, excessive prose — keeping the byte-exact anchors. Move substantial rationale to a sibling **`_RATIONALE.md`** with a *"don't read the rationale unless you must"* note in the IMPLEMENT. (Broad context genuinely helps an LLM, but as the guide nears ~100k tokens it shouldn't be *forced* — the same reason TESTING/DESIGN already live in addenda: the IMPLEMENT must *feel* manageable.) This cut drives a **MAJOR** bump (plan → execution); a mere recent-build delta bumps **MINOR**.

### Orchestrator hygiene — compact forward, keep memories

The Build-Guide orchestrator is long-lived across many rounds, so manage its context deliberately. **Immediately after** each fold + breadth pass + prompt-regeneration, **compact the session with a forward-looking note** — written as if the reviews it just prepped are already done — so a later `--resume` returns a correctly-compacted orchestrator (wait, and the compaction offered then can't be steered). Let the orchestrator author its own compact note (they do it well), and **keep memories** throughout — the loop is too long to hold in one context.

**The per-round cadence (peer-run gate loops):** each round ends with fold → the two breadth subagents → regenerate prompts → hand the human the **compact arg + a kick-off prompt** for the next round (templates in `.agents/GAP_REVIEW_WORKFLOW_PROMPTS.md`) — the human compacts and fires, which is guardrail 5. How many rounds to run between compacts is the orchestrator's judgment call; err toward one round per compact when a round also carries folds or protocol work.

---

## What "Exclusively Executable" Requires

The mechanical guarantees behind the claim. An executable plan that violates any of these isn't ready.

### Confirmed decisions only
The IMPLEMENT the executing agent reads contains **only confirmed decisions** — no alternatives, no "we could X or Y," no "TBD," no "Decision Dn" markers. Everything was discussed, researched, and locked earlier. If a subagent surfaces a decision-shaped question mid-build, that's a real plan bug: **stop, surface to the human, fix the plan, continue. Never decide on the agent's own.** This is what makes "exclusively executable" mechanically true — the agent has nothing to decide, so it can't decide wrong.

### No mixed truth
Never put a known-wrong fact and the right one in the same context — an LLM can't be trusted to pick the right one consistently. Remove or fix **before** the agent sees the plan: stale paths, superseded decisions, "bug noted but not patched," "carry forward from vX" redirects, "Phase 0 fixes these bugs" folded into forward spec. Code-level fixes happen in a **prep session**; only the post-fix world enters the next IMPLEMENT. BUG_REPORT / FEEDBACK / prior IMPLEMENTs are *historical archive* — their content folds in as standing behavior; the executing agent never reads them. **Fix or remove, then execute. Don't ask an LLM to debug your context.**

### Setup — human-hands items burn down DURING planning (the 3-minute rule)

**Setup is PLANNING'S work, never a future agent's.** The executable doc carries **zero "for the human" items scattered through its phases.** Route every human-hands task — a credential, a service dashboard, DNS, an account/plan check, a branch/domain assignment, a taste call, a real fork — by the to-do-list rule the moment it's found: **doable in ~3 minutes now → do it NOW, in this planning session** (and before assuming something needs the human at all, check whether a CLI or an agent can confirm or handle it — they usually can); anything bigger lands in **ONE named SETUP block at the top of the build guide** — never spread across the phases.

**The SETUP block is a burn-down list, not a hand-off.** The same planning sessions that author the guide also chip items off it — the human is present in those sessions anyway; they click/approve, the agent drives (most items turn out agent-runnable at an authenticated terminal with the human there). Mark each item ✓ with a date as it lands. Whatever remains at gate-clear is finished in one final human-present sitting. **Execution does not open while a SETUP item is outstanding — the block is EMPTY by kickoff.** The build guide's audience is the ORCHESTRATING AGENT; SETUP is the one part addressed to the human-present sessions, and by execution time it is a record, not a task list.

Two observed failure modes this kills (both real): (1) **the mid-build stall** — the executing orchestrator hits "need a Stripe key / a new Google Cloud project with scopes" and the whole flow freezes until the human can reach a terminal; (2) **the renamed-preflight anti-pattern** — the setup idea survives as a word while its items quietly spread back through the guide, or sit untouched waiting for "some agent later," which is worse than no setup block at all (contract-payments v8, observed). Neither mold works: not zero-items-by-scattering, not a list-in-name-only.

**Corollary — infrastructure that protects the build itself activates FIRST.** Anything whose absence degrades the build environment over time (the canonical case: a free-tier database keep-alive ping — the DB must stay warm through the whole build and debugging arc, not from first production push) is a day-one first-slice step, never deferred to the phase where it "belongs."

### No pass-through between execution chunks
When execution surfaces a gap/bug — even one affecting a later chunk — **do not silently patch the later chunk inline.** Finish the current scope, capture the gap (a descriptive git commit + a note to carry forward), and let a planning session fold it into the next IMPLEMENT round so the fix lands *once*, cleanly. A chunk carrying both the original and the corrected fact is mixed truth; inline pass-through erases the loop's value.

### The orchestrator's blueprint (parallel groundwork)
The gate-cleared IMPLEMENT hands the executing orchestrator a starting point for parallel work so it doesn't design it cold — the operational answer to the Orchestration Paradox:
- **Subagent groupings** per phase: what runs in parallel, what dependencies sequence it, what context each subagent needs.
- **Boundaries of delegation:** what the orchestrator does NOT delegate (gate decisions, branch state, commit cadence, escalation, verification reads).
- **Placeholders as decouplers:** strict `<!-- PLACEHOLDER: ... -->` conventions sever cross-team dependencies (frontend builds against a placeholder while backend builds the real thing). Treat groupings as a starting point, not a contract.

---

## The Artifact System

### Versioning — one internal counter, end to end

A three-part `vMAJOR.MINOR.PATCH` lives for the project's life, **starting at the first IMPLEMENT draft** and continuing through planning rounds into shipped releases. **Version numbers are internal artifacts that serve our work — not a customer-facing release counter.** One continuous track covers planning → rounds → ships.

The MAJOR / MINOR / PATCH bump table + the delimiter rules are catalogued in `.agents/DIRECTORY_PROTOCOL.md § Versioning` (the canonical numbering reference); the notes here are the *why* behind running one counter this way.

Higher bump resets lower to zero (`v3.1.5` → `v3.2.0`). **No change, no bump; the number tracks changes regardless of source.** A planning round that meaningfully revises the doc bumps it: `v5_0_0_IMPLEMENT` → `v5_0_1` (after feedback) → `v5_0_2` (after a cold pass) → `v5_0_3` (exclusively executable). When `v5_0_3` ships clean, the git tag is `v5.0.3` — *the same number.* Plan version IS ship version when nothing changed between them. **Addendums to an IMPLEMENT** (e.g. `…_ADDENDUM_DESIGN.md`) share its version and bump in lockstep — see *Addendums*.

*Why we don't reserve numbers for releases:* that's a UX gesture from when versions were dressed up for users; our lifecycle is 90% planning, so planning gets the same machinery. Users find a changelog fine without gap-free numbers. Each file's 2-line header says whether it's a planning revision or a ship artifact — **trust the header, not filename pattern-matching.**

**Long, multi-angle gate loops — demarcate phases with a MINOR bump.** The default above (each pass bumps PATCH) is right for a normal loop. When a gate runs many rounds across multiple angles and the per-version `docs/archive/vX_Y/` dir gets large, you MAY open each new *review phase* with a MINOR bump so each phase keeps its own directory and a self-contained patch-trail: the holistic A-loop lives in `vX.5.*`, the B/C round opens at `vX.6.0`, and execution opens the next MAJOR (`v(X+1).0.0`). (Everlastings v1.5: A ran in `v1_5/` as `v1.5.1…v1.5.9`, B/C opened at `v1_6/` `v1.6.0`, execution opens `v2.0.0`.) The number still tracks changes (no empty bumps); the minor boundary just keeps a long trail navigable, and the major boundary marks plan→execution (where the review scaffolding is finally shed). The phase boundary is a **copy** into the new minor dir (the living docs re-stamped at the new version); the old dir keeps its terminal state standing as the phase record — nothing is renamed across the boundary.

**Archiving IMPLEMENT updates during initial planning phases and folding in human surfaced feedback revisions — keep all old documents by COPYING the file first, then updating the new file with changes.** -- This is notably different from the very next rule. The logic is that in the beginning phases of planning and when human provides feedback during the process, then the actual original (for example when looking at v1_5_4_IMPLEMENT.md) is still extremely valuable context for future work, and should remain in the directory (right next to the new v1_5_5_IMPLEMENT.md in our example). Whereas the GAP REVIEW surfaced revisions have all the valuable information in the GAP_REVIEW meaning keeping all copies of the IMPLEMENT.md version in the directory isn't necessary. The next note emphasizes this. 

**Archiving GAP REVIEW surfaced revisions — current-only living docs; keep the records.** When GAP REVIEWS bump from revisions they surfaced, the **living planning docs** (the IMPLEMENT + its addendums + the review-prompt charters) are **current-only**: rename them to the new version (`git mv`, history preserved) so git + descriptive per-file commits hold the superseded patch and the dir doesn't fill with near-duplicates. **Terminal records are kept standing, never renamed:** the per-pass `vX_Y_Z_GAP_REVIEW_<angle>.md` findings, `BUG_REPORT`, and `FEEDBACK` logs — they're cited forward and are the visible rigor trail. **Two carve-outs:** (1) when a living doc is *condensed/whittled* (a big rewrite, not a normal fold), keep a diffable pre-condense copy (`…_2.md`) so the human's inline notes aren't silently lost; (2) *dead-end **decisions** within a doc* stay in context (anti-mixed-truth — a content rule, distinct from keeping old version *files*). This relaxation rides on consistent, descriptive commits; without them, prefer keeping copies. (Early drafts of this doc imagined keep-every-file as the norm before real flow was recorded — this is the reconciliation.) **Don't strand an empty directory:** when work *leaves* a `vX_Y/` dir for the next, drop a copy of that dir's final-state living docs so it isn't left empty (the standing records usually keep it populated; this just covers the case where a dir would otherwise empty out).

**Delimiters + tag hygiene** (dots except in filenames, pure-numeric git tags, re-point = delete-and-recreate) are catalogued in `.agents/DIRECTORY_PROTOCOL.md § Versioning`.

### As-built doc-sync — bring the architecture doc current with a FRESH agent

When a build's gate clears and execution completes, the living architecture doc (e.g. `EVERLASTINGS_STORE.md`) is brought current as a **distinct task run by a fresh agent** — never as the tired tail-end of the executing session. The failure it prevents: an executor whose context is nearly full summarizes the doc from memory, silently dropping/distorting details — stale lines then poison every future cold review (reviewers can only reason from the doc). The fresh agent reads the architecture doc **end-to-end, linearly, like a human** (no chunked/grep reads, so it coheres and contradictions surface), walks the executed IMPLEMENT and the git history as the change-source, and folds the changes in — with the **actual code as the tiebreaker** (cite `file:line`) wherever plan and doc disagree on behavior. Two landmines: the doc's top Status/Version header often drifts a release behind the code (treat it as suspect *first*), and stale `file:line` anchors survive code growth (re-open the file before trusting a cited line).

### The core documents (all in `docs/archive/vX_Y/`)

Canonical filenames + the directory tree live in `.agents/DIRECTORY_PROTOCOL.md`; this section carries the *method* each doc holds.

- **`vX_Y_Z_IMPLEMENT.md`** — the evolving plan for one initiative; the living roadmap (there is no separate MASTER). Iterates by revision; the highest-numbered one is active. **Through the gap-review loop the living plan is *current-only*: a revision bump renames it (`git mv`, history preserved) and git holds the superseded patches** — don't pile up near-duplicate standing copies (see *Versioning → Archiving a revision*). Once the gate clears, this same doc is **executed directly** — there is no separate frozen "BUILD" file. Header: `Initiative` + `Revision driven by`.
- **`vX_Y_Z_BUG_REPORT.md`** — bug log tied to a release; **kept standing** (a terminal record, cited later). *How bugs flow into versions:* a fix found on the fly during execution is captured in git and folds into the next IMPLEMENT round; a **provided bug list** that needs research/planning gets a **genuinely new** IMPLEMENT round (new planning ⇒ a new doc, not a rename of a prior one).
- **`vX_Y_Z_FEEDBACK.md`** — human review of an IMPLEMENT round; version-named, kept standing.
- **Unversioned sketch files** — notes whose actionable content migrates into the next IMPLEMENT, then move to `processed/`.

The session's progress lives in **git history** (frequent, descriptive per-file commits), not a session-log file; cross-session handoff is the **compact arg + tended memory** (§ *How we work*).

### Two routes — feature vs patch

- **Feature work** (default; architecture involved, genuine planning): one IMPLEMENT iterated 0→1→2→… through the gap-gate, then **executed directly from the gate-cleared IMPLEMENT** — the orchestrator parallelizing via subagent groupings across natural execution boundaries (a subsystem, a layer, a file cluster). How the work is split across subagents is a planning-time call, not a default.
- **Patch** (bug fix / trivial polish, root cause known, no architecture): fix → verify → ship; git is the record. The IMPLEMENT loop is skipped. This shortcut is *right* here and *wrong* for feature work.

> **The #1 trap (it stalled Thot): sizing feature work as patch work** — chopping it into tiny throwaway units because the IMPLEMENT was structured by version-milestone. Feature work is split by execution boundary, **never** by feature/version unit.

### Roadmap ≠ build queue (how to chunk without fragmenting)

The IMPLEMENT is the roadmap *and* the detailed plan, at two depths — keep them separate:
- **The roadmap is coarse direction.** It names where things are headed; it is NOT a list of build units, and a milestone is NOT a version to ship.
- **Only the imminent slice is detailed to executable depth.** Detailing the *entire* future to executable depth in one file is the opposite failure — a bloated, unbuildable IMPLEMENT. (Anti-pattern seen on Thot: a ~350KB IMPLEMENT that was all roadmap and nothing executable.)
- **A slice becomes ready-to-execute by readiness × execution boundary**, not by version. A slice can be *large* (a whole subsystem) when that's the coherent boundary.

Two project shapes, same machinery:
- **Plan-it-all** (e.g. a store launch): research/plan the whole product to gap-free, then **execute at one ship** — run the gate-cleared IMPLEMENT directly, the orchestrator parallelizing via subagent groupings (splitting into parallel subagent tracks if the ship is too big for one session). **Decide the arrangement when you can see the work.**
- **Incremental** (e.g. an app that grows): the IMPLEMENT carries the **full vision as direction** (so you build to accommodate what's coming), but you detail + gate + ship **one coherent slice at a time** by execution boundary. You do NOT invent a version number per feature to decide the slicing.

### Directory & master docs

The full directory tree — with a worked visual-flow example — lives in `.agents/DIRECTORY_PROTOCOL.md § Directory Example`.

The highest-numbered `vX_Y_Z_IMPLEMENT.md` IS the roadmap — there is no `IMPLEMENT_MASTER`. Two master docs live outside the archive:

| Doc                    | Role                                                          | Updated when                   |
| ---------------------- | ------------------------------------------------------------- | ------------------------------ |
| `docs/PROJECT_NAME.md` | architecture, current state, design system, pitfalls (living) | every non-trivial change ships |
| `.agents/DEV_RULES.md` | rules of engagement (this doc)                                | a convention is added/changed  |

**Starter templates — `.agents/_TEMPLATES/PROJECT_NAME.md` + `.agents/_TEMPLATES/README.md`.** Both ship in every project's `.agents/_TEMPLATES/` as ready-to-adapt starters, so you never write either from scratch: `PROJECT_NAME.md` is the seed for a project's living architecture doc (you copy it out and rename it for the project — e.g. it grew into `EVERLASTINGS_STORE.md`), and `README.md` is the base project README. They're kept in sync across projects via `filemgmt` (§ *Syncing*), and because they get refined naturally while being adapted in any one project, those improvements flow back to the fleet — so the templates only get better. When starting a new project, reach for these two first.

Reference content (schemas, glossary, diagrams) lives in `PROJECT_NAME.md` and `archive/resources/`, **not** in the IMPLEMENT — a forcing function that keeps PROJECT_NAME honest. Conflict resolution: a *past* IMPLEMENT vs PROJECT_NAME → **PROJECT_NAME wins** (it's living); the *current* IMPLEMENT vs PROJECT_NAME → **IMPLEMENT wins** (PROJECT_NAME was likely neglected — update it). **Dead-end *decisions* stay in context** so future agents read a live decision against what it replaced — a **content** rule (anti-mixed-truth), distinct from keeping old version *files* (those follow *Versioning → Archiving a revision*).

### Where information lives — route by scope (don't put cross-cutting facts in the wrong tier)

Five homes, each with a different scope and load behavior. Putting a fact in the wrong one means it either won't be there when you need it, or it bloats every unrelated session. Route by **scope** (who needs it) and **whether it must travel** (other machines/repos):

- **`~/.claude/CLAUDE.md`** (global, machine-local) — *about the human and how to work with them*: communication style, cognitive needs, writing mechanics, durable preferences. Auto-loads in **every** session on this machine. Keep it lean (always-on context). Does **not** sync to other machines/teammates. Right for "how to talk to the human," wrong for project facts or bulky data.
- **Per-project Claude auto-memory** (`~/.claude/projects/<project>/memory/`) — *facts about one project/codebase/client*. Auto-loads only in that project. Where project decisions, gotchas, and project-scoped feedback go.
- **`.agents/DEV_RULES.md` + synced `.agents/` docs** (this doc) — *cross-project build method/protocol*. Travels to every repo via `filemgmt`. The only tier that reliably reaches other machines/repos, so anything that must be portable protocol goes here — **not** in machine-local CLAUDE.md.
- **`.agents/PROJECT_LESSONS.md`** (in-repo, **not** fleet-synced) — *project-specific hard-won lessons* that live with the repo (travels via git with the repo, but isn't fanned out to the fleet).
- **The project's living architecture doc** (`PROJECT_NAME.md` → e.g. `EVERLASTINGS_STORE.md`) — the project's current-state architecture/spec.

Two routing traps: (1) cross-project but **not** protocol — measured calibration data, a reusable technique — has no perfect home; prefer a clearly-labeled synced reference doc over stuffing it into DEV_RULES (keep this doc pure method) or into machine-local CLAUDE.md (won't travel). (2) Always-on cost: anything in `~/.claude/CLAUDE.md` loads into *every* session including unrelated projects — put short durable facts there, not narrow or bulky content.

### IMPLEMENT template (the living roadmap)

```markdown
# v[X.Y.Z] Implementation Plan
**Initiative**: [what this initiative is]
**Revision driven by**: [initial draft / post-feedback / post-cold-review / lock-in review…]
**Required reading first**: docs/PROJECT_NAME.md · README.md · [archive/resources/* as applicable]
**If you find missing context**: PROJECT_NAME.md is living — confirm with the human and update it; don't paper over the gap here.

## Roadmap (coarse direction — NOT a build queue)
[Where this initiative is headed. Brief. Milestones are direction, not version-ships and not execution units.]

## Imminent slice — [name] (detailed to executable depth)
[The next coherent execution-boundary chunk: phases, file:line specifics, production-ready snippets, verification, rollback, subagent groupings. When this clears the gap-gate it's **ready to execute** — run directly, the orchestrator parallelizing via subagent groupings (§ *Two routes*); sized by execution boundary, not by version.]

## Later (direction only)
[Bulleted direction for what follows. Detail arrives as a slice approaches the gate — not before.]

## Cross-references
[Architecture/glossary → PROJECT_NAME.md · API/schemas → archive/resources/ · branching/versioning → DEV_RULES.md]
```

---

## Git Branching & Merging

`main` = production-ready, tagged releases only (tested, bug-free, public-ready). `dev` = persistent integration/testing branch. `feat/*` and `fix/*` = temporary, deleted after merge.

**New project init:**
```bash
cd ~/Development && mkdir <project> && cd <project>
cp -R /Users/seanivore/Development/_git_init/. .
git init && git add . && git commit -m "chore: initial commit"
git branch -M main
gh repo create <project> --public --source=. --remote=origin
git push -u origin main
git checkout -b dev
```

**Feature → dev preview → ship:**
```bash
# 1. Start a feature off latest main
git checkout main && git pull origin main && git checkout -b feat/<name>

# 2. Ship through dev first (ff-merge keeps history linear), push → Vercel auto-deploys the dev preview
git checkout dev && git merge --ff-only feat/<name> && git push origin dev
```
**🛑 PAUSE.** Tell the human it's live on the dev preview URL (in README / PROJECT_NAME) with a one-line summary. Do NOT ship to main until they sign off. Bug? Fix on `feat/*`, ff-merge to `dev`, push, ping again — production is untouched until step 3.

```bash
# 3. Ship to main (after: tests pass · human signed off · PROJECT_NAME current ·
#    package.json bumped if applicable · build succeeds)
git checkout main && git merge --ff-only dev && git push origin main
git tag vX.Y.Z && git push origin vX.Y.Z     # pure numeric tag, no suffix
```
After step 3, `dev` and `main` are at the same commit. The dev→main ff-merge model means dev is always at-or-ahead of main, never out of sync.

---

## Deployment — Vercel, Cloudflare & Preview Environments

Where the branching flow (above) meets the platforms. **Always check the current Vercel / Cloudflare CLI docs before running them — both change often; never drive them from training recall.**

- **Environments track branches:** pushing `dev` auto-deploys the **Vercel dev preview**; `main` is production. Verification runs on the real deployed preview URL, never `vercel dev` / localhost (§ *Git Branching*).
- **Preview protection OFF during development.** Turn Vercel **SSO / deploy-protection off** on the preview while building so agents reach every endpoint freely (or drive it with Claude-in-Chrome); turn it back on before anything public. The STRIPE-guide note ("use a browser already authenticated to the protected preview") is the *fallback* for when protection must stay on — not the default.
- **Set every scope's keys at once — once.** Seed **Development + Preview + Production together, in a single pass, each scope with its _correct_ value from the start**: Production gets **live** keys, Preview/Development get **test** keys. Never seed test keys into Production "temporarily" to swap at go-live — that's extra footwork and a guaranteed re-do (and the "so they can't be used by accident" rationale is a corner-cut, not a reason; least-privilege belongs in *which* keys a scope holds, not in deferring the setup). Set each scope right the first time — fewer touches, fewer errors. The only unavoidable later step is a value that literally does not exist yet (e.g. a live webhook secret minted only at go-live).
- **`.env.reference` is a durable RECORD, not a machine grep-source.** A **gitignored** doc holding *all* keys **grouped by prod / preview / dev**, so when Vercel periodically locks or drops env values they re-push from here and are **never reconstructed from scratch**. It exists to protect the human from lost keys — do **not** architect build commands around grepping values out of it inline per-command (that pattern is a workaround for agent shell-state quirks dressed up as a foundation, and it quietly usurps a human record into fragile plumbing). If a script needs values, read them once inside that one script; keep the record comprehensive and never delete from it.
- **DNS lives on Cloudflare, not Vercel.** The apex (`august.style`) is on **Cloudflare**; Vercel is authorized for the apex, so prod branches publish to a **subdomain**. Create/point the subdomain with the **Cloudflare CLI** using the token already in the shell. (Vercel used to *suggest* the Cloudflare DNS change and apply it for you; lately — likely because we drive the Vercel CLI — the agent edits Cloudflare DNS directly via its CLI, which the shell token authorizes.)

---

### Addendums — split a big plan, keep one build

A large IMPLEMENT can shed bulk into **addendums** — sibling docs it references, e.g. `vX_Y_Z_ADDENDUM_DESIGN.md` (the presentation layer) and `vX_Y_Z_ADDENDUM_TESTING.md` (the verification plan) — so the executable functionality plan stays scannable while design/test detail grows without entangling its byte-anchors.

- **One build, one task.** The IMPLEMENT + its addendums are a single execution task and a single source of truth; the IMPLEMENT lists its addendums in its required-reading.
- **Lockstep version.** Every addendum carries the **same version** as its IMPLEMENT and bumps with it (a v1.6.1 IMPLEMENT ⇒ v1.6.1 addendums) — a reader never diffs mismatched versions.
- **Always in review scope.** Every gap review (and the breadth pass) ingests the IMPLEMENT **and all its addendums** — no per-review reminder needed; "review the build" means all of them.
- **Same executable bar everywhere.** Design and tests are planned to *exclusively executable* exactly like functionality (the builder never discovers/decides). Design that depends on the live render ships a **concrete default + a render-tune note** and gets a **test + feedback pass** alongside functionality testing — "executable" means no guesswork, not "frozen, no feedback." **Real content is never a build/test gate:** build and test on production-grade placeholders that mimic the validated real-asset specs; real content arrives later (often by the client, post-handoff).
- **When to split.** Reach for an addendum when one concern would bloat the IMPLEMENT past easy scanning or tangle its anchors; keep tightly-coupled behavior (data model, handlers) in the IMPLEMENT itself.

---

## Design — the funnel, the language, and Claude Design

The interactive-design method now lives in four dedicated, fleet-synced docs in `.agents/_DESIGN/` (extracted from this doc so each is perfected on its own). This section is the index — go to the doc for the detail:

- **`.agents/_DESIGN/INTERACTIVE_DESIGN_LANGUAGE.md`** — the reusable aesthetic + interaction vocabulary (House taste, axes, technique library, product-archetype map, a divergence palette of named aesthetic "starts", the anti-slop craft bar). The WHAT you feed the funnel and hand to Claude Design. Reach for it to *name* a design direction.
- **`.agents/_DESIGN/INTERACTIVE_DESIGN_PLAYBOOK.md`** — how to wield the Language: the levers, a briefing checklist, where-to-research, the visual-feedback vocabulary, the paste-ready anti-slop acceptance checklist. Reach for it when briefing or iterating.
- **`.agents/_DESIGN/DESIGN_FUNNEL.md`** — the runnable, re-startable funnel that renders **multiple distinct named directions** to rank (no single grafted winner); ships the reusable Workflow script + spec template in `.agents/_DESIGN/`. Reach for it when the direction is open and you want rendered options.
- **`.agents/_DESIGN/CLAUDE_DESIGN_COLLAB.md`** — the three-phase Claude Design handoff/return protocol (initial new-UI handoff / mid-build gap-list / return-after-implementation 1:1 re-sync), the contract seam + PORTABLE/SEAM/SANDBOX-ONLY tags, and the "describe what you built" round-trip prompt. Reach for it when handing a chosen direction to CD.

Two integration facts stay here: CD's output is design that then gets **gap-reviewed like any other** via the **`D` angle's render charge** (design-correctness against `vX_Y_Z_ADDENDUM_DESIGN.md`; D's pre-design **contract charge** is what verified the seam was draftable before CD ever engaged — § *The Gap-Review Gate → Angles*); the `CD_HANDOFF` filename is registered in `.agents/DIRECTORY_PROTOCOL.md`.

---

## Conventions

- **Commits:** `type(scope): brief [vX.Y.Z]` + body bullets (`feat fix docs style refactor test chore`); word them to mirror the executed slice so history maps to plan.
- **Drift:** fix protocol drift you hit (whether or not you caused it); if unsure it's drift, confirm with the human first.
- **Research:** verify current docs while planning — never ship training-data recall as fact. `.agents/RESEARCH_PROTOCOL.md` is **only** for business-grade research (a formal business plan / market strategy); ordinary research for an IMPLEMENT build guide or a design funnel does **not** use it — that's just normal verify-as-you-go.
- **CLI tools first.** Reach for the CLI before any manual or human step — `gh`, `vercel`, `docker`, Cloudflare, `filemgmt`. If a CLI can do it, an agent drives the CLI; don't hand the human a task a command would do.
- **Shared commands live in `~/Development/scripts`.** Any terminal command used by more than one project belongs in the scripts repo (`git@github.com:seanivore/scripts.git`) and reaches `~/bin` through its installer — never authored in `~/bin` directly, never copied into a project repo, never symlinked out of one. `~/bin` is a single flat namespace, so a command is inherently *global*; what's per-project is the **data** it operates on, and that goes in a repo-local config the command reads (the `gate` → `<repo>/.agents/GATE_NOTES.md` pattern). Build new ones to `scripts/SCRIPT_STANDARD.md`, and run `scripts/install_all.sh` to restore the whole toolkit on a fresh machine. (The hard lesson: `uid` started inside one project, got copied into others, and ended as four byte-identical *broken* copies while the real implementation sat in a retired repo — it worked only for as long as that repo happened to still be on disk.)
- **Agentic testing by default.** Verify by *driving the real thing* — Claude-in-Chrome, throwaway `.js` scripts, actual end-to-end runs against the preview — not by asking the human to test or reasoning that it's untestable. (Pairs with § *What "Exclusively Executable" Requires → Setup — human-hands items burn down DURING planning*.)
- **Skills:** globally-installed Agent Skills live under `~/.agents/skills` (the `npx skills add …` install dir) — **not** `~/.claude/skills`, where agents reflexively look and find nothing. When a task needs a skill, or a subagent must locate one, check/pass that path explicitly.
- **Markdown tables ≤ ~100 cols.** Keep a table's total rendered width under ~100 monospace characters (any number of columns within that budget); past it, rows wrap and become unreadable in editors (and paste badly into Thot). If the data won't fit, drop the table for **grouped bullets** (one sub-list per row) — never a wider table. *Exception:* a doc read **only** by agents (e.g. a gate-cleared IMPLEMENT) may keep a wider table when it genuinely conveys structure better than bullets — never in human-facing docs (IMPLEMENT, README, FEEDBACK, PROJECT_NAME).
- **Prose soft-wraps; never hard-wrap a paragraph.** Write each paragraph/bullet as **one logical line** and let the editor wrap to the window — do not insert manual line breaks mid-paragraph. A single newline renders as a space, so hard-wrapping changes nothing visible but multiplies physical lines, and these docs get cited by line number constantly: a hard-wrapped paragraph spans many numbered lines, so a human pointing at "line 76" (the block) and an agent pointing at "line 81" (a sentence inside it) are both right about *different* lines — soft-wrapped, the whole paragraph is one number and they agree. **Never reflow inside a fenced ` ``` ` code block or a table** — those carry significant newlines / column alignment and *are* the anchors (the byte-exact CURRENT/NEW quotes). Reflowing prose is always safe; touching a fence or table is never. (Claude Code's doc output hard-wraps prose by habit — strip it.)
- **Human-formatted docs are opt-in — not the default.** Default formatting is dense (one logical line, delimiter-rich) — that's how agents ingest best. **Only** when Sean says **"human-formatted"** (or a doc is unmistakably Sean-only and agent-read by no one) switch to the cognitive-load-first layout in `.agents/HUMAN_FORMATTING.md` (foldable indentation, one-idea-per-line lists, `+`/`-` markers, colon-outside-bold). Never apply it to agent-read docs (IMPLEMENT / ADDENDUM / gap-review / reports). If unsure, stay dense and ask.
- **Testing:** the IMPLEMENT (or its `_ADDENDUM_TESTING`) carries the verification plan; results are recorded in git commits, not a separate report.
- **Media + services:** project media → `cdn.august.style` (Cloudflare R2); the upload flow (images → auto-`.webp`, video → `aws` to R2) is in `.agents/CDN_GUIDE.md`. Email → **Resend** (Cloudflare send/receive is the one we're evaluating).
- **Syncing:** one canonical copy → every project via `filemgmt -f ~/Development -r <path>/.agents/DEV_RULES.md` (new shared files: `-a`). `.agents/PROJECT_LESSONS.md` is per-project, not synced. The protocol dir is now **`.agents/`** (plural, the industry standard); `filemgmt` syncs to both `.agent/` and `.agents/` during the migration.

---

## Agent Quickstart

**Read first (no exceptions):** `docs/PROJECT_NAME.md` → `README.md` → the highest-numbered `vX_Y_Z_IMPLEMENT.md` (your executable guide if it's gate-cleared, your working plan if you're still planning) → `.agents/PROJECT_LESSONS.md` (skim).

**If you're planning, the gap-gate IS the job:** draft → fresh-context review (angles A/B/C, + D by artifact test) → fold → repeat → human approves → execute. **No code before the gate.**

**As you work:** commit often — per file, descriptive — so git holds the progress; if the plan is wrong, stop + surface + fold it into the next IMPLEMENT revision (don't silently patch); confirm via `git diff` before commit.

**Before closing:** update `PROJECT_NAME.md` if architecture changed; leave the git history as the session record (frequent, descriptive commits); write a compact arg only if Sean asks.

**You do NOT:**
- read past IMPLEMENTs / BUG_REPORT / FEEDBACK during a build (already folded into your gate-cleared IMPLEMENT);
- edit a current IMPLEMENT mid-build, or backfill an earlier one to fold findings — fold into the **next** revision (bump it; through the gap-review loop that's a rename — *Versioning → Archiving a revision*);
- write a separate "completion/walkthrough" file (the git history is the record);
- create archive dirs at major-version level (`v3_0/`, never `v3/`);
- put reference content in the IMPLEMENT (→ PROJECT_NAME / archive/resources);
- pass-through fixes between execution chunks;
- **self-certify the plan, or run ad-hoc in-session reviewers as the gate** — a peer pass counts ONLY under the five § *Running the gate with peer agents* guardrails (verbatim block as the entire prompt, new instance per pass, own findings file the human reads raw, every pass kept standing, human-kicked round); angle A is always external/no-repo.

---

## Changelog

One line per meaningful version; full history + rationale in git.

- **v4.11.0** (2026-07-22) — **Preflight renamed SETUP and reframed as a planning-time burn-down** (§ What "Exclusively Executable" Requires): the name "preflight" read as a pre-build task for some later agent; SETUP is planning's own work — the sessions that author the guide chip items off as they go (✓ + date), a final human-present sitting clears the remainder, and the block is EMPTY by execution kickoff. Same 3-minute rule, same two failure modes, same build-protecting-infra-first corollary as v4.10.0 — the change is who owns the list and when it empties.
- **v4.10.0** (2026-07-22) — **Preflight formalized** (§ What "Exclusively Executable" Requires — *No open human-action items* rewritten as *Preflight — no open human-action items mid-build (the 3-minute rule)*): every human-hands task routes by the 3-minute rule — trivial-now items are done during planning; everything else lands in ONE named PREFLIGHT block at the top of the build guide, executed as a human-present session BEFORE execution opens (human + orchestrator at an auth'd terminal; most items agent-runnable with the human present). The build guide's audience is the orchestrating agent; preflight is the one human-addressed part. Kills both observed failure modes: the mid-build stall on keys/dashboards, and the renamed-preflight anti-pattern where the word survives while the items spread back through the guide (contract-payments v8). Corollary: build-protecting infrastructure (e.g. a free-tier DB keep-alive) activates as a first slice, never at its "proper" phase. (Also trues up the masthead, which had lagged at v4.8.1 while the v4.9.0 changelog entry landed.)
- **v4.9.0** (2026-07-22) — **env-key discipline hardened** (§ Deployment), from Sean mid contract-payments v8 build: seed **every scope's keys at once, once** — Prod=live, Preview/Dev=test, each correct from the start (no test-in-prod-then-swap; the "so they can't be used by accident" rationale is a corner-cut, not a reason — least-privilege lives in *which* keys a scope holds, not in deferring the setup); and **`.env.reference` is a durable RECORD, not a machine grep-source** — it exists to protect the human from lost keys, so build commands must not be architected around inline per-command greps out of it (a shell-state workaround dressed as a foundation that quietly usurps a human record into fragile plumbing). Supersedes the old test-keys-in-all-scopes-until-go-live posture.
- **v4.8.1** (2026-07-21) — terminal cold-A reverted to **optional** for peer-run loops (v4.8.0 had made it mandatory as a compensating control; Sean's correction: peer agents are full agents — clean context per pass, orchestrator-grade model/effort, 1M context — the same fresh instances the human-launched windows were, so it's equivalence, not a loss needing compensation; the five guardrails cover what actually changed, prompt/findings transport — and peer-run B/C/D was already established practice before this doc caught up). What IS new with contract-payments v8.1: the first loop running the formalized two-charge angle D (the Claude Design integration) — it closed all three lanes in 3 rounds/9 passes with convergent cross-lane findings.
- **v4.8.0** (2026-07-21) — **peer-agent gate execution + two-charge angle D** (from the contract-payments v8 gate). B/C/D passes may now run as orchestrator-spawned fresh-context peer agents under five guardrails (verbatim block as the entire prompt; new instance per pass; reviewer writes its own findings file and the human reads it raw; every pass numbered and kept standing — no verdict-shopping; the human kicks off each round); THE CORE #2 + the conflation table amended to match — the gate's real requirements are fresh context + full packet + verbatim charge + independent findings transport, not the physical window; angle A stays external/no-repo, and the terminal cold-A holistic pass becomes MANDATORY for peer-spawned loops. Angle D redefined as **interface-contract & design-correctness**: two charges chosen at prompt-generation time by artifact test — the contract charge pre-design (surfaces × endpoints both directions, machine-called endpoints pre-exempted, completeness anchored outside the compared docs, the handoff-executable test, a lane-scoped CONTRACT READY verdict), the render charge once executable-depth UI spec exists — with the C/D boundary fixed as *pipes (C) vs payload contract (D)* and fold-time lane assignment driving angle re-opens. Plus the per-round compact-arg + kick-off cadence (§ Orchestrator hygiene) and minor-phase boundaries as copies (the old dir stays standing as the terminal phase record).
- **v4.7.0** (2026-07-20) — added **The functionality-design pass** (§ Gap-Review Gate → The loop): genuinely novel functional logic gets designed by a dedicated peer instance emitting a focused design doc, slotted after the fresh self-gap-review and before the certified A/B/C gate — because gap review *validates* a plan, it doesn't *design* one, and evolving novel logic through gap-review folds is convoluted chaos. Distinct from both the fresh-instance gate and the Claude Design (visual) collab; fires only for slices with real novel logic; the foundation-slice schema must be made accommodating up front so the later design lands without a migration. (Surfaced on contract-payments when the flexible payment model turned out to be underspecified mid-gate.)
- **v4.6.0** (2026-07-20) — gap-review prompt mechanics hardened from the contract-payments loop: the landmines ledger is now **inlined inside every angle's block** (deliberate duplication — a copied block must carry its landmines; the shared top-of-file ledger got missed at launch more than once), with the A block's copy maintained through the A loop and every copy updated identically on each fold; cold-A doc delivery is **file upload, not paste** (doc bodies run tens of thousands of tokens; the block names its uploaded packet); for clean rebuilds the superseded legacy architecture doc is handed to **no** reviewer (stale-truth contamination of a cold read).
- **v4.5.0** (2026-07-20) — added the **Shared commands** convention: every cross-project terminal command lives in `~/Development/scripts` and installs to `~/bin`, with per-project behaviour in a repo-local config (the `gate`/`GATE_NOTES.md` pattern) rather than a copied script. Written after `uid` was found broken across four repos — its real implementation lived in the retired `modular-agent-orchestrator` and its "guaranteed unique" claim measured a 99.85% collision rate. The scripts repo now carries `SCRIPT_STANDARD.md` (how commands are built) and `install_all.sh` (fresh-machine restore).
- **v4.4.0** (2026-07-10) — retired the `BUILD` / `TRACK_BUILD` / `BUILD_REPORT` / `SESH` file-types now that execution runs directly from the gate-cleared IMPLEMENT, git history is the progress log, and cross-session handoff is the compact arg + tended memory. Added a near-top **How we work** day-to-day section (start-from-templates, never-budget-tokens, skills, CLI-over-MCP, git-as-log, memory, compaction); simplified the as-built sync, the core-documents list, and feature-vs-patch routing; fixed the starter-template paths to `.agents/_TEMPLATES/`; added the **Media + services** convention pointing at the new `.agents/CDN_GUIDE.md`.
- **v4.3.0** (2026-07-09) — extracted the interactive-design method out of this doc into four dedicated, fleet-synced `.agents/` docs: `INTERACTIVE_DESIGN_LANGUAGE.md` (axes, technique library, archetype map, a divergence palette of named starts), `INTERACTIVE_DESIGN_PLAYBOOK.md` (levers, briefing + feedback vocabulary), `DESIGN_FUNNEL.md` (the re-runnable funnel + `_TEMPLATES/design_funnel.mjs`, now rendering **multiple distinct named directions** by default instead of one grafted winner), and `CLAUDE_DESIGN_COLLAB.md` (the three-phase handoff + the "describe what you built" round-trip). Replaced the inline `## Collaborating with Claude Design` section with a four-doc index stub — the start of DEV_RULES becoming an index.
- **v4.2.0** (2026-07-09) — added `## Deployment — Vercel, Cloudflare & Preview Environments` (preview-protection-off-in-dev, the `.env.reference` per-env key file, Cloudflare-CLI subdomains), `## Collaborating with Claude Design` (funnel → handoff/reply → mid-build, PORTABLE/SEAM, the `CD_HANDOFF` + reply-trio conventions, "more than one funnel winner"), `### No open human-action items`, and the *CLI-first* + *agentic-testing-by-default* conventions; renamed the protocol dir `.agent/` → `.agents/`; decluttered the masthead (this version-by-version changelog moved here off the top).
- **v4.1.0** — formalized the gap-review loop mechanics from the Everlastings v3.2 gate: the three-part review lens, flag-don't-assert + validate-then-fold, the "Settled — do not re-raise" landmines ledger, the trichotomy verdict, angle-by-angle close, end-game cleanup, orchestrator compact-forward hygiene, "the gate is never the first reviewer."
- **v4.0.13** — As-built doc-sync: a fresh agent derives the architecture doc from the build-adjusted IMPLEMENT + BUILD_REPORT with code as tiebreaker; `Doc impact:` phase annotations.
- **v4.0.12** — settled-base note for delta builds (don't re-litigate shipped code).
- **v4.0.11** — gap-review-prompts protocol: auto-generated, paste-ready `vX_Y_Z_REVIEW_PROMPTS.md`, one self-contained block per angle.
- **v4.0.10** — self-driven pre-loop gap pass; Skills live at `~/.agents/skills`.
- **v4.0.9** — human-formatted-docs convention (`.agents/HUMAN_FORMATTING.md`), opt-in only.
- **v4.0.8** — BUILD vs IMPLEMENT clarified; execution arrangement (one orchestrator vs split tracks) is a planning-time call.
- **v4.0.7** — Versioning → archiving a revision: living planning docs current-only, terminal records kept standing.
- **v4.0.6** — Addendums (`_ADDENDUM_DESIGN` / `_ADDENDUM_TESTING`), lockstep-versioned, always in review scope.
- **v4.0.5** — "Where information lives" — the five memory tiers, routed by scope.
- **v4.0.4** — breadth pass on every version-push; phase-demarcation versioning for long gate loops.
- **v4.0.3** — `.agents/PROJECT_NAME.md` + `.agents/README.md` starter templates.
- **v4.0.2** — prose soft-wrap convention.
- **v4.0.1** — Markdown-table width convention (≤ ~100 cols).
- **v4.0.0** — strategic restructure: re-centered on *exclusively executable* + *fresh-instance gap review*; cut boilerplate.
