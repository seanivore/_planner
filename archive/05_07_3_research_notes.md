# Research Notes & Outlines

## Track 1: Email Configuration & Agentic Inbox

### SMTP Options for Sending via Gmail (Custom Domain)
Since Cloudflare Email Routing only handles *receiving*, you will need an SMTP service to *send* emails from your custom domain (`august.style`) directly within Gmail.

| Provider     | Free Tier                 | Pros                                                                                           | Cons                                                                 |
| ------------ | ------------------------- | ---------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Resend**   | 3,000 / month (100 / day) | You're already familiar with it (used in Everlastings). Best developer experience, modern API. | Daily limit of 100 might be low for heavy volume.                    |
| **Brevo**    | 300 / day                 | Generous daily limit, very reliable, includes marketing tools if needed.                       | UI is more geared towards marketing than pure developer SMTP.        |
| **SendGrid** | 100 / day                 | Industry standard, massive infrastructure, very reliable deliverability.                       | Free tier was reduced recently; can be strict on account approval.   |
| **Mailgun**  | Pay-as-you-go             | Highly customizable, great routing rules.                                                      | No permanent free tier anymore (only a 1-month trial of Foundation). |

**Recommendation:** Stick with **Resend**. It fits seamlessly into modern workflows, you already know the dashboard, and 100 emails/day is plenty for personal/networking outreach.

### Agentic Inbox Architecture
To implement a system like `cloudflare/agentic-inbox`:
1. **Trigger:** An email arrives at `august.style`. Cloudflare Email Routing intercepts it and triggers a **Cloudflare Worker**.
2. **Processing:** The Worker parses the email (headers, body, sender) and sends the payload to an LLM (e.g., Anthropic via API or Cloudflare's built-in Workers AI).
3. **Logic:** The LLM evaluates the email against your rules (e.g., "Is this a client inquiry?", "Is this spam?"). It can draft a response, summarize a long thread, or categorize the email.
4. **Action:** The Worker forwards the original email *plus* the LLM's summary/drafted response to your actual Gmail inbox. 

---

## Track 2: GitHub Repositories Snapshot

- **AutoHedge & Vibe-Trading:** These leverage AI agents (like Swarm) to perform market analysis and autonomous trading. Given your background in BTC derivatives, running a local instance of these connected to a paper-trading API could be a high-ROI experiment.
- **Camofox Browser:** A drop-in replacement for Puppeteer/Playwright that bypasses Cloudflare/bot detection. *Crucial for our data gathering in Track 4 (scraping esoteric databases).*
- **Claude Ads / Toprank:** Open-source Claude Code skills that run 250+ checks across ad platforms. You can literally drop these into a client project to generate an immediate, comprehensive audit of their digital marketing, providing instant value for your consulting business.

---

## Track 4: Jesse Michels (American Alchemy) Outreach Outline

**Objective:** Pitch a system-design solution that turns his massive data accumulation into an actionable, interconnected knowledge graph.

1. **The Hook (Shared Context)**
   - Briefly mention your background: Austin, ASD pattern recognition, and finding non-traditional value.
   - Validate his work: His ability to ask the hard questions softly, and his impressive historical knowledge surrounding Alchemy Networks and biological intelligence (Michael Levin).
2. **The Observation (The Data Problem)**
   - Identify that his podcast amasses incredible amounts of data—throwaway facts about "bio-plasma," "telepathy tapes," "Torsion," and missing physicists.
   - Highlight the gap: Traditional media operations treat this as content. But it is actually a fragmented database. Without proper aggregation, massive connections across decades of research fall through the cracks due to compartmentalization or simple human cognitive limits.
3. **The Solution (Your Pitch)**
   - Propose an AI-driven, RAG-based intelligence pipeline designed specifically for American Alchemy.
   - *How it works:* It ingests transcriptions of every episode, plus all the reference books (like Claude Swanson's). It uses agents to automatically flag cross-references (e.g., when a guest says "it's the only thing I could find," an agent independently verifies and maps it).
   - *The Value:* This tool becomes an "Anti-Compartmentalization Engine." It uncovers hidden through-lines, validates his intuitive pattern recognition, and identifies exact guests to interview next to close the loop on a theory.
4. **The Ask**
   - Propose a brief conversation. Offer to build a small prototype using a handful of his transcripts to prove the concept.

---

