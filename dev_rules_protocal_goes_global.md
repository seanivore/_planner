Wow okay I might be able to get around to shortening this, but FYI at the bottom Gemini ended up giving us a head start. I am only hesitating shortening this down because it would require me taking a lot of time to figure out what is needed still and what isn't so that we can improve on the boilerplate setup Gemini gave me based on my main files that I wanted to make global instead of project-specific. Other than the length, I'm happy it happened this way because, with a starting point, the end result should be much better. I'm franky very surprised by how easily it was able to strip down DEV_RULES considering how much that file had been updated and just made a mess over time with agents sort of agonizing over it. 

Though before spitting out the started files it did provide this regarding updating the DEV_RULES to make them global: 

  > "Create the directory structure ~/.agents/skills/dev-process/ and save the clean system files provided into ~/.agents/AGENTS.md, ~/.agents/personality.md, ~/.agents/DEVELOPER_PROFILE.md, and ~/.agents/skills/dev-process/SKILL.md respectively. Ensure you clean out old .agent paths and update our global VS Code/Cursor settings.json file-watcher rules so the language_ser process doesn't wipe them."

Okay, now my original document, which will end in final context on how Gemini helped. 

---

# Project Rule Operational Update

I'd like to adjust my `.agent/DEV_RULES.md` and other important file distribution strategy. 

## Current System

  Regardless of what `~/Development/<project-dir>` I'm working in, the system is set up so that we can keep updating the `.agent/DEV_RULES.md` document as a living document being reflecting specifics and a workflow that is being perfected over time. This file, and others, all exist in duplicates within the `~/Development/<project-dir>/.agent/...` directory. To make this work we currently use a custom script terminal command `filemgmt` to distribute any updates to `DEV_RULES.md`, any other important resources or document within the `.agent/...` directory, as well as adding new documents to those directories — the command updates or adds those files to every single `~/Development/<project-dir>/.agent/...` folder that exists. 

### Why Update Now

  Recently I asked a Claude Code agent to use a Skill I had installed "globally". The agent wasn't able to locate the file. I investigated and found that this is because the standard for Skills that people create and have setup so that you can select what agentic development tool you use, like Claude Code, Cursor, and others, uses the same format. In this case it was looking for the skill I globally installed using the HyperFrame provided command `npx skills add heygen-com/hyperframes`. 
  
  I found this was because globally installed Skilled, of which there are currently 20, are all installed here: `/Users/seanivore/.agents/skills/...`
  
  Upon exploring further I found that even if installed a Skill was (accidentally) installed just for one project (as I attempt to only install them globally), are all also now placed in a `~/Development/<project-dir>/.agents/skills/...` directory. This is a very recent shift, and frankly, a much welcomed one that people have been asking for on Twitter for a while to get rid of the collection of `.cursor/CURSOR.md`, `.claude/CLAUDE.md`, and others, in their project directories. 
  
  I let this discovery sit in my notes and my mind for a while, already pretty sure that I would want to move my own `DEV_RULES.md` et al, documents, to the `.agents/...` instead of `.agent/...` directory. Then one day an agent got confused by the plural and singular directories and I decided it was time. 

### Opportunistic Considerations

  1. **RUNAROUND REDIRECTS IN EVERY PROJECT MUST BE ACCURATE**
     I'm now more familiar with the location and usage of these "global" directories and files. That makes one question if this might be a clue towards a less error-prone rules document flow than  my current setup. 
    
     My current setup also includes: 
    
     `/Users/seanivore/Development/<project-dir>/.cursor/CURSOR.md` that only says "@.agent/AGENTS.md"
     `/Users/seanivore/Development/<project-dir>/.claude/CLAUDE.md` that only says ""@.agent/AGENTS.md and @.agents/" 
    
     And then: 
     `/Users/seanivore/Development/<project-dir>/.agent/AGENTS.md` with the actual information and 
     `/Users/seanivore/Development/everlastings-website/.claude/skills/` with any project-specific skills.
    
     *Can't we just direct them to read a single copy in a global location?*
  
  2. **REDIRECTS FOR ITEMS THAT GET LOST IN SPECIFIC PROJECTS**
     I also *just* discovered that, where I can find a project-specific installed skill, it appears that the installer also throw symlinks in the `.claude/skills/` directory that, when you select "open original" takes you to `.agents/skills/`. 
  
     The example where I discovered this was, when I looked here: 
  
     `/Users/seanivore/Development/everlastings-website/.claude/skills/...`, I saw all the contents had folders icons that showed it was a symlink directory. Opening their original takes you here: 
  
     `/Users/seanivore/Development/everlastings-website/.agents/skills/...`, while like, fine, but I just really don't like these project-specific skills because who wants to hunt down a skill that you vaguely remember from a past project but don't know what past project it was used in to find and then accurately remember what it was called. 
  
     *Symlink aside, can we easily collect all the skills and place them in a global location?*

## Proposed System

Here's my thoughts on a updated ideal system, how to manage it, and what required changes we'd need to complete to achieve this. 

### Creating New Projects

I'd like to keep my system for when I set up a new project directory. I have a specific folder that I use a terminal command flow to copy the contents out of in the right order to ensure that my secret files are always already in the folder and added to the .gitignore file before git is even initialized. 

`/Users/seanivore/Development/_git_init/` 

Other than every current project in `~/Development/`, this is also where we'll want to update the core agent files to redirect agents where they need to go to read our actual rules and other essential files. 

### Organizational Considerations 

**One organizational focus: Length**

Yesterday I asked an agent to merge our test-complete changes on a dev branch to a main branch. I asked that they please make sure that they're using the `DEV_RULES.md` protocol, because I know that it is specific about using FF and keeping the main branch otherwise clean. 

Starting the task I saw the agent comment: "Looks like `DEV_RULES.md` is very large. Let me find the specific part about git branch management."

They completed the task fine, but it points towards a potential problem:

  1. They already should always be reading DEV_RULES when they start but they clearly didn't
  2. DEV_RULES is specifically, literally, the rules for agent development and shouldn't need to be read in any other method than in full

Not reading in full means important specifics could be missed because targeting and only reading a specific portion assumes we somehow are able to create a perfect document that will never accidently leave details slightly out of the expected location. Not reading the full thing when starting the project is, presumably, also because of the size. 

I let agents update the document over time and stopped handling it partially myself because LLMs write and consume written information in a very different format than I do which is really difficult for me to get through, often resulting in spending hours reformating while editing a document. 

**Another focus: Decentralize and simplify**

Part of the problem is that the `~/Development/<project-dir>/.agent/...` directory currently includes the following. 

```
├── 2026_MOBILE_DESIGN_SPECS.md
├── AGENTS.md
├── apple-ui-design
│   ├── art-ipad-navigation.jpeg
│   ├── ios-viewport-measurements.jpeg
│   ├── ipados-sidebar-landscape-expand-hidden.jpeg
│   ├── ipados-sidebar-portrait-expand-hidden.jpeg
│   ├── ipados-tab-bar-landscape-portrait.jpeg
│   └── macos-sidebar-expand-hidden.jpeg
├── CDN_MEDIA_UPLOAD.md
├── CLAUDE_DESIGN_ANIM_SITE.md
├── CLAUDE_DESIGN_HYPERFRAMES_SKILL_SUB.md
├── DEV_RULES.md
├── EMOTION_DRIVEN_COPYWRITING.md
├── GAP_REVIEW_WORKFLOW_PROMPTS.md
├── HUMAN_FORMATTING.md
├── HYPERFRAMES_LLM.txt
├── HYPERFRAMES_MOTION_PRINCIPLES.md
├── HYPERFRAMES_TRANSITIONS.md
├── HYPERFRAMES_TYPOGRAPHY.md
├── PROJECT_NAME.md
├── README.md
├── RESEARCH_PROTOCOL.md
└── STRIPE_CUSTOM_CHECKOUT_GUIDE.md
```

The current intended starting place was DEV_RULES but that isn't even clear anymore. The README file seems like a good target, but even there, that actual file is technically a template for new project directories. 

The conundrum with this is that, to shorten the DEV_RULES into something an agent won't hesitate to read before starting any session, and won't hesitate to read in full, we need to take out parts and place them in other files so that it becomes a bit more of a directory ensuring we can make sure they get all the MUST READ information in there in full while directing them to all the information that they might only need in certain situations in other files, but the directory is a mess of information. 

**The move forward**

Keeping in mind that we'll be placing these files in the global location, therefore only needing one version of everything, and just having files that redirect in all product directories, we can plan out how to organize these files into subfolders that make sense, we can decide how to rename files so that they know what must be read first, and we can shorten our DEV_RULES according to our needs. 

### Location Considerations

**Are there global directory challenges?**

I started to do this myself the other day by placing a document that was made for me in a global directory for `/human-resources/`. I am noticing today that the file remains, but the directory keeps losing it's name. 

Is there some kind of system or script that is controlling this directory that we need to know about? 

`'/Users/seanivore/.agents/untitled folder/CDN_UPLOAD_GUIDE.md'`

The fact that it changed it to a directory name using a space make me even more concerned. 

Regardless of if this 'agents' folder is the best place for my human-resources, this behavior is making me question if we'll be able to add my other files to this folder. 

If there is some kind of controller, then, well (A) I'm just curious wtf how does that work I've never encountered this before and it is so weird that the user has no idea, and (B) can we control it or adapt it, but if not (C) are we able to find out what it's rules are so that we can understand what we'll be able to move into this directory? 

### The Ideal

  1. We might need to adjust this plan based on your updating my understanding of certain files. For instance, I was going to say maybe `DEV_RULES.md` should go in the `AGENTS.md` file, but I recall agents calling that the "personality" file in the past. Is that it's purpose and are there other purposes we should adhere to to make our new system as intuitive for an LLM as possible? 

  2. We can either place the `PROJECT_NAME.md` and `README.md` templates in a `/Users/seanivore/.agents/templates/` directory, since the templates inform what exactly should be on these files. Or we could just keep them in the my aforementioned `/git_init/` directory I use to start new project directories to always be copied. The only difference is deciding where we need to go when we want to update these templates. In either case we can just make a note in whatever the core read-first file is (with the DEV_RULES details) tells them that they need to update these two files on the first session and keep them as the source of truth, updated after every session that would have involved making changes to the project that would require updating these files.

  3. Otherwise, we can have all those files placed into sub-directories and labeled as whatever type of resources they are. Then wherever our main read-first file is we just include an index of sorts about where these resource files are and what they are.

  4. Then we search all `~/Development/<project-dir>/` directories for skills, and move those skills to `/Users/seanivore/.agents/skills/...`. I don't think we need symlinks at all in the Claude project specific skill location since they'll be global.

  5. Update all of our `~/Development/<project-dir>/.<agentic-tool>/<tool-rules.md>` files to point to the GLOBAL location of our DEV_RULES type files. This would include `.cursor/CURSOR.md`, `.claude/CLAUDE.md`, and the project local `.agents/AGENTS.md`. 

## What's Next

I need feedback on the plan, some questions answered, and then the DEV_RULES.md file assessed in a very categorical way so that we can make sure we know exactly what types of chunks of information it contains, allowing us to make sure all that needs to be in there is where it needs to be. 

This will also involve an update making sure the protocol isn't stale anywhere and an all-around simplification. My struggle here has been that sometimes I don't want them to be overly strict depending on the project, or I run into issues where they're overly strict about the wrong rules. Like being obsessed with the semantics and filenames, rather than the purpose of the protocol. 

### Note From Web Search Gemini, Re: Self-Fixing .agents/ Directory Rules

I searched why it kept removing the name of my "human-references" folder, it gave me a command to find the culprit, I explained what I'm trying to plan for you to help me with, and ask it give a rpely to fill you in on what is going on and how to adjust the system to work with our new extension of the system. Here is what they sent. 

```
Here is a comprehensive briefing document tailored for an AI agent to consume. It outlines the current system conflict, the structural goals, and the precise steps required to transition your rules system globally.
------------------------------
## Technical Brief: Global Developer Rules Migration (.agent/ to .agents/)## Objective
Migrate a custom developer rules system from legacy project-specific .agent/ directories into a single, unified global system under ~/.agents/. Ensure the new setup seamlessly integrates with the language_ser background daemon without triggering its aggressive auto-cleanup routine. [1] 
------------------------------
The user's local projects (360-design, everlastings-website) are actively being managed by an IDE-bound language server background daemon (language_ser).

   1. The Ghost Deletions: When manual directories are created inside .agents/, the background Language Server treats them as cache drift or invalid states and purges them.
   2. Schema Enforcement: language_ser strictly expects structured formats following the agents.md specification. It monitors and maps files like SKILL.md, skills.json, and directories like workflows/ or skills/. [2, 3, 4, 5] 
   3. Naming Confusion: The system is migrating away from a legacy .agent/ naming scheme to match the newer industry standard .agents/, which is currently causing context fragmentation and confusion among LLMs.

------------------------------

* Single Source of Truth: Eliminate all local .agent/ and .agents/ project folders to prevent rule fragmentation.
* Global Precedence: Establish ~/.agents/ as the host for universal workflows, developer identities, and coding rules.
* Daemon Compliance: Structure custom rules so they match the schema language_ser expects, preventing the daemon from auto-deleting them. [6] 

------------------------------
When you take over this task, execute the changes across these distinct areas:
## 1. Configure IDE Exclusions (Silence the Cleanup)
Before generating or moving files, update the global IDE configurations (e.g., Cursor or VS Code settings.json) to prevent language_ser from altering custom structures during development:

"files.watcherExclude": {
  "**/.agents/**": true,
  "**/_agents/**": true
},"search.exclude": {
  "**/.agents/**": true,
  "**/_agents/**": true
}

## 2. Standardize Folder Structure to Match agents.md
To safely include custom additions in the global ~/.agents/ directory without clashing with the language server, format files exactly into the schema it scans for.

* Rewrite freeform markdown rules into structured SKILL.md documents.
* Place them inside subdirectories under ~/.agents/skills/. [7] 

Target Global Directory Layout:

~/.agents/
├── AGENTS.md                  # Global registry file declaring your profiles
├── workflows/                 # Standard orchestration files
│   └── dev-rules.json         
└── skills/                    # Custom instructions mapped as agent skills
    ├── global-code-style/
    │   └── SKILL.md           # Your DEV_RULES content adapted into a Skill
    └── workflow-automation/
        └── SKILL.md

## 3. Refactor Legacy Content

* Scan all local directories inside the Development/ path for existing .agent/ or .agents/ folders.
* Consolidate their contents. Extract project-agnostic guidelines into global skill files.
* Add an AGENTS.md file to the root of ~/.agents/ to formally register the directories, ensuring background utilities recognize them as valid configurations. [8, 9] 

## 4. Provide Clean Prompt Context for LLMs
Because LLMs get confused between .agent/ and .agents/, explicitly inject system-level steering text at the top of the new global rule files: [10] 

# SKILL: Global Developer Rules<!-- CRITICAL CONTEXT: The legacy '.agent/' directory has been deprecated. 
     All global developer rules, identities, and workflows now reside exclusively 
     in the standard '.agents/' directory. Do not reference or look for '.agent/'. -->

------------------------------
Would you like me to generate a template starter for your global AGENTS.md file so your agent knows exactly how to format the registry?
```

I then did have it set up starter files for a few you'll already see in `~/.agents/`. When it went to completely change things too much (started suggesting the gap review go in skills which I rather keep for just downloaded skills and have our rules at top level), I asked that it provide new copies with all the context it had, so that you could have a starting point and just be reviewing and improving upon Gemini's work. 

Just in case the `language_ser` changed things in the `~/.agents./` directory before we got to it and got to adapting the language_ser rules to fit our needs, I added copies of the starer files it created from our files in the `~/.agent/...` directory, which we will be deleting everywhere after this admin project is complete. 

Here's a note we should address so that it doesn't happen for any future settings, too. It came up when starting this `claude --resume` session. 

  > Ignoring 6 permissions.allow entries from .claude/settings.json: this workspace has not been trusted. Run Claude Code interactively here once and accept the trust dialog, or set projects["/Users/seanivore/Development/get-paid"].hasTrustDialogAccepted: true in /Users/seanivore/.claude.json.