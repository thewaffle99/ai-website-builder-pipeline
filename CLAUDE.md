# Website Rebuild — Orchestration Instructions

This project rebuilds a client website. Client-specific scope lives in `BRIEF.md` —
read it first, every session. Three sub-agents exist in `.claude/agents/`: `pm-agent`,
`dev-agent`, `qa-agent`. You (the main session) are the orchestrator — you invoke them
in sequence and manage the loop described below. Do not write application code
yourself; delegate to dev-agent. Do not write user stories yourself; delegate to
pm-agent. Do not review your own output; delegate to qa-agent.

## The loop, per batch

1. **Invoke pm-agent** with the batch name (e.g. "Header + Footer + Home"). It reads
   `BRIEF.md` and returns user stories with acceptance criteria.
2. **Invoke dev-agent** with those user stories. It builds the actual files in this
   repo and returns a self-check report.
3. **Invoke qa-agent** with the user stories + dev-agent's output. It returns PASS or
   FAIL with severity-tagged findings (BLOCKER/MAJOR/MINOR).
4. **If FAIL**: invoke dev-agent again with QA's findings. Repeat steps 2-3.
   **Cap at 5 rounds.** If round 5 still fails, stop and summarize the unresolved
   findings for me — do not keep guessing. This usually means the requirement itself
   is ambiguous, not that dev-agent needs another attempt.
5. **If PASS**: invoke pm-agent for final review (full batch, not just QA's checklist —
   PM checks scope completeness, SEO, and fidelity to BRIEF.md against the Definition
   of Done).
6. **If PM REJECTED**: invoke dev-agent with PM's findings, treat exactly like a QA
   fail, go back to step 3.
7. **If PM APPROVED**: batch is done. Tell me what shipped and move to the next batch
   in `BRIEF.md`'s sequence, or stop and wait for me if you've hit a natural
   checkpoint (end of a logical group of pages).

## Ground rules

- `BRIEF.md` is the only place client-specific facts should live. If any agent is
  about to invent client data (contact info, pricing, credentials, copy) that isn't in
  `BRIEF.md` or visible on the live reference site, that's a stop-and-flag moment, not
  a guess-and-continue moment.
- Keep me posted at the end of each batch (not each round) with a one-line status —
  I don't need a play-by-play of every QA bounce, just the outcome and anything that
  hit the round cap.
- If you ever find yourself about to write significant application code directly
  instead of delegating to dev-agent, stop and delegate instead — that's the whole
  point of this setup.

## Starting a new client project

1. Copy `briefs/BRIEF_TEMPLATE.md` to `BRIEF.md` in the project root.
2. Fill it in for the specific client/site (or ask me to do the discovery pass — I
   can fetch the reference site and draft a first version of BRIEF.md for you to
   review before any building starts).
3. Set `Brand Tone / Visual Direction` in `BRIEF.md` — dev-agent uses this to pick
   a matching file from `design-references/` before any layout work.
4. Run `claude` and say "Start the first batch from BRIEF.md."


## Agent Pipeline

1. **PM Agent** — scopes work from BRIEF.md, defines acceptance criteria
2. **Dev Agent** — implements
3. **QA Agent** — tests, loops back to Dev on failure
4. **PM Agent** — final review gate
5. **DevOps Agent** — manual trigger only. Run after PM final review passes, when ready to generate a client preview link.

DevOps is never auto-chained. Invoke it explicitly: "Use the devops subagent to deploy this for client preview."

The three agents in `.claude/agents/` and the `design-references/` folder don't change
between projects — only `BRIEF.md` does. This is what makes the setup reusable across clients.
