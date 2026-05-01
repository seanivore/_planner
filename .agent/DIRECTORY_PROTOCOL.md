# Directory Protocol 

## Overview 

These are filename with versioning standards that keep project directory structure organized by design. From top to bottom, the files are chronologically in their lifespan.  

## Filename Conventions 

  1. `UPDATE_v(+1)_0_0.md` — UPDATE
  2. `v1_0_0_DEV_PLANNING.md` — DEV_PLANNING
  3. `v1_0_0_IMPLEMENT.md` — IMPLEMENT
  4. `v1_(+1)_0_FEEDBACK.md` — FEEDBACK
  5. `v1_1_0_SESSION_DEV.md` — SESSION_DEV
  6. `v1_1_(+1)_BUGS.md` — BUGS

### New No Impact Filetype 

Created specifically for human to keep notes and tasks organized before the next session. Thus far, theses are not items that would require a version update. If at any point there is something in them that would require one, then the filetype and name will change accordingly. 

  + `vX_Y_Z_PREP.md` — this keeps it right after the relevant implementation plan, using the same version number. Because of its neutral nature — mostly just a need to keep notes somewhere handy — it isn't integrated into the directory example below. You can imagine that it could be  placed anywhere and have no effect. It is just for personal reference. 

### Alternate Filetype for Simpler Projects

Created specifically for projects that do not require a larger IMPLEMENT.md file at that time or a larger formalized flow; for example, in the case of simple portfolio website updates. 

  + `vX_Y_Z_UPDATE_PLAN.md` — is used in some projects that do not need a larger IMPLEMENT.md file at that time. In these cases, a FEEDBACK.md document is presented to Claude Code to begin the session, and then the agent creates a session plan designed to achieve all the FEEDBACK.md goals, or goals simply communicated by chat. Upon approval, that plan is immediately executed during that same session. The UPDATE_PLAN.md document is that exact same session plan file being followed as a to-do checklist for that session.

## Directory Example 

docs/
├── archive/                           # Find current doc by looking to highest version 
│   ├── images/                        # Used for feedback and planning
│   ├── resources/                     # Two examples; provided for planning at some point
│   │   ├── {SERVICE}_FULL_LLM.txt     # Commonly found in service technical documentation
│   │   └── {SERVICE}_API_DOCS.md      # Many services have agent skills or MCPs with details instead  
│   ├── v1_0/                          # New subdirectory for every MAJOR update (only filenames use underscore not dot)
│   │   ├── UPDATE_v1_0_0.md           # Human build sketch needing research and planning
│   │   ├── v1_0_0_DEV_PLANNING.md     # Agent's session plan writing implementation guide
│   │   ├── v1_0_0_IMPLEMENT.md        # First implementation guide requires many gap-hunting iterations
│   │   └── v1_0_0_SESSION_DEV.md      # Agent's orchestration plan from executing implementation build
│   └── v1_1/                          # New subdirectory for every MINOR update (vMAJOR.MINOR.PATCH)
│       ├── v1_0_0_FEEDBACK.md         # Reflects reviewed version, drove v1.0.0 to v1.1.0 version change
│       ├── v1_1_0_DEV_PLANNING.md     # Research, updating implementation guide to fill in gaps, iterative process 
│       ├── v1_1_0_IMPLEMENT.md        # Implementation guide is updated living document
│       ├── v1_1_0_SESSION_DEV.md      # Agent's orchestration execution plan when building
│       ├── v1_1_1_BUGS.md             # Bug logs expected behavior versus actual, steps to reproduce; drove v1.1.0 to v1.1.1
│       ├── v1_1_1_SESSION_DEV.md      # Exploring bugs, details on findings helpful for future, then applying fixes 
│       └── v1_1_1_DEV_PLANNING.md     # No new bugs found; notes dev pushed to main branch with v1.1.1 tag
├── research/                          # Below is by no means meant to be absolutely comprehensive or exhaustive 
│   ├── 1_DEEP/                        # Any research needed to define market opportunity, strategic positioning
│   │   ├── business-viability/        # Tech feasibility, service-needs, pricing strategy, backed by market analysis
│   │   ├── competitive-landscape/     # Zoomed in evaluation of rival strategy, strength, weakness
│   │   ├── funding/                   # What opportunities are out there; provide adjacent proof
│   │   ├── market-analysis/           # Broader industry environment, user needs, market size, industry trends
│   │   ├── target-demographics/       # Who you are building for specifically, adjacent opportunity in tiers
│   │   ├── RESEARCH_REVIEW.md         # Feedback on where to look more, what to focus on later
│   │   └── RECOMMENDATIONS.md         # Agent candid thoughts on opportunity based on research
│   ├── 2_FOCUS/                       # Human guided drill-down; focus on moat, realistic finance projections, etc. 
│   │   ├── RESEARCH_FINALIZATION.md   # Report on research and finalization planning 
│   │   └── PRODUCT_OPPORTUNITY.md     # Overview assessment of the project's market placement and more
│   └── 3_FINAL/                       # Polished results ready to be seen by the world whenever and wherever 
│       ├── BUSINESS_PLAN.md           # After drafts, includes 'best practice' business plan essentials 
│       ├── EXEC_SUM_investors.md      # Overview framed specifically for investors 
│       ├── EXEC_SUM_users.md          # Overview framed for any other audience the project might need
│       └── EXECUTIVE_SUMMARY.md       # Use 'best practice' research for current, industry specific framework
├── PROJECT_NAME.md                    # Architecture, technical documentation, context priming agent helper 
├── BRAND.md                           # Voice in various situations, design guide, palettes, etc. 
└── BUSINESS_PLAN.md                   # Any other important docs to keep top level minimal but helpful 