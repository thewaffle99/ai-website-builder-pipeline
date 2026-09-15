# Website Rebuild Agent System — Reusable Across Clients

## What changed from the jmarinolaw-specific version

The three agents (`pm-agent`, `dev-agent`, `qa-agent`) used to have client-specific
content — addresses, site maps, SEO targets — baked directly into their role
definitions. That meant a new client meant editing the agents themselves, which
defeats the point of having a reusable system.

**Now the split is:**
- **`.claude/agents/*.md`** — defines *process and judgment*. Identical across every
  client project. Never edit these per-engagement.
- **`BRIEF.md`** — defines *what this specific project is*. Reference URL, site map,
  brand, known data, SEO targets, regulated-industry flags. This is the only file
  that changes per client.

All three agents are instructed to read `BRIEF.md` first, every session, before doing
any work.

## File structure

```
your-project/
├── CLAUDE.md                          ← orchestration loop (generic, reusable)
├── BRIEF.md                           ← THIS project's client facts (you create this)
├── briefs/
│   ├── BRIEF_TEMPLATE.md              ← blank template for new clients
│   └── EXAMPLE_jmarinolaw_BRIEF.md    ← worked example (Jesse Marino engagement)
├── design-references/                 ← 74 real-brand DESIGN.md files (MIT licensed)
│   ├── clay.md                        ← warm/organic — good for fitness, wellness
│   ├── cal.md                         ← clean scheduling UI — booking-heavy sites
│   ├── superhuman.md                  ← premium minimal — high-end service clients
│   ├── notion.md                      ← illustration-rich, approachable
│   ├── linear.app.md                  ← clean/editorial — professional services
│   ├── stripe.md                      ← clean/editorial — professional services
│   ├── nike.md                        ← bold/energetic — consumer brands
│   ├── starbucks.md                   ← consumer-warm — local/lifestyle businesses
│   ├── airbnb.md                      ← consumer-warm — local/lifestyle businesses
│   └── ... (65 more brands)
└── .claude/
    └── agents/
        ├── pm-agent.md                ← generic, reusable
        ├── dev-agent.md               ← generic, reusable
        └── qa-agent.md                ← generic, reusable
```

**Design references** are plain-text design token docs (colors, typography, spacing,
visual tone) extracted from real company sites. Dev-agent reads them before any layout
work to ground UI generation in real design language instead of generic defaults.
Set `Brand Tone / Visual Direction` in `BRIEF.md` to tell dev-agent which reference to use.
Source: [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — MIT licensed.

## Starting a new client project

1. Set up a fresh project folder (or reuse this one's `.claude/agents/` and
   `CLAUDE.md` — they don't change).
2. Copy `briefs/BRIEF_TEMPLATE.md` to `BRIEF.md` in the project root.
3. Fill it in — or ask me to do a discovery pass: give me the reference site URL and
   I'll fetch it, crawl the key pages, and draft a first version of `BRIEF.md` for you
   to review and correct before any building starts. That's exactly what I did
   manually for jmarinolaw.com before this conversion — `EXAMPLE_jmarinolaw_BRIEF.md`
   is what that output looks like.
4. Run `claude`, confirm `/agents` shows all three loaded, and say:
   **"Start the first batch from BRIEF.md."**

## Why this split matters in practice

- **Agency reuse**: every new law firm, dentist, restaurant, or SaaS landing page
  client is a new `BRIEF.md`, not a new agent system. This is the productization
  path for an AI Web Studio — one engineered process, infinite clients.
- **Auditability**: if a build goes wrong, you can tell immediately whether it's a
  *process* problem (fix the agent) or a *scope* problem (fix the brief). Mixing them
  made every bug ambiguous to diagnose.
- **Safer defaults for regulated industries**: the agents have a generic
  "regulated industry — never approximate known data" rule. `BRIEF.md` is where you
  flag *which* industries apply per client, so the rule activates appropriately
  without being hardcoded to law firms specifically.

## The loop itself (unchanged)

PM reads `BRIEF.md` → writes user stories → Dev builds → QA reviews → loop Dev/QA on
fail (capped at 5 rounds) → PM final review against `BRIEF.md`'s Definition of Done →
loop again if PM rejects → SHIPPED. Full detail in `CLAUDE.md`.
