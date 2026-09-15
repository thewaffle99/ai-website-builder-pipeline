# BRIEF — [Client Name]

Fill this in before starting a build. This is the only file that should contain
client-specific facts — the agents in `.claude/agents/` are generic and read this
file for context.

## Reference site
- Live URL: [url]
- Type of engagement: [ ] Live-site migration (preserve URLs/SEO equity)
                       [ ] Reference rebuild (placeholder content, inspired-by)
                       [ ] Net-new build (no reference site, brief is the spec)
- CMS/stack of the original (if known): [e.g. WordPress + Elementor]

## Target stack
- [ ] Default (Next.js + Tailwind + shadcn/ui) — use dev-agent's defaults
- [ ] Override: [specify framework/stack if different]

## Brand
- Primary colors: [hex values, or "match reference site — extract from live CSS"]
- Fonts: [names, or "match reference site"]
- Logo: [source file location, or "use placeholder pending client asset"]
- Tone/voice: [e.g. professional, playful, technical]
- Brand Tone / Visual Direction: [one of: "clean/editorial", "warm/organic", "premium minimal",
  "bold/energetic", "consumer-warm" — or describe your own. Dev-agent uses this to pick a
  matching file from design-references/ as a visual starting point.
  Suggested mappings: clean/editorial → linear.app, stripe | warm/organic → clay, notion |
  premium minimal → superhuman, apple | bold/energetic → nike, spotify |
  consumer-warm → starbucks, airbnb]

## Site map
List every page/route in scope, grouped logically. Example:

1. Home
2. About / Team (template: [bio fields if repeated per-person])
3. Services / Products (template: [fields if repeated])
4. Contact
5. Legal (Privacy, Terms, etc.)

## Global components
- Header: [nav structure, CTA, phone/contact display if applicable]
- Footer: [links, contact info, social/legal links]
- Repeated templates: [list any page type that repeats — team bios, products,
  blog posts, service pages — and the data fields each instance needs]

## Known data (source of truth — dev-agent must use exactly, never approximate)
- Business name / legal name:
- Address(es):
- Phone/fax:
- Email:
- Hours:
- Key claims/credentials/certifications (if regulated industry):
- External links that must NOT be rebuilt, only linked (e.g. payment processors,
  booking systems, third-party portals):
- [Any other facts that must be exact]

## SEO targets
- Primary schema type: [LocalBusiness | Organization | Product | Article | etc.]
- Target keywords/local SEO focus: [if known]
- Redirect map required: [ ] Yes — see redirects.csv  [ ] No — net-new build

## Regulated industry flags (if applicable)
[ ] Legal   [ ] Medical   [ ] Financial   [ ] Other: ___
If checked: all factual claims, disclaimers, and credentials require exact sourcing,
never paraphrasing. Flag this explicitly to dev-agent and qa-agent.

## Batch sequence (PM will draft this, but note any client priorities here)
[e.g. "Homepage and Contact need to ship first — client has a launch event on X date"]

## Notes / constraints
[Anything else: hosting target, timeline, content still pending from client, etc.]
