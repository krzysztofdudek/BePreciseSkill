<img width="1376" height="768" alt="Gemini_Generated_Image_pm4r2pm4r2pm4r2p" src="https://github.com/user-attachments/assets/47fb992b-c367-45e9-87af-58610b26e525" />

# Be Precise

**A Claude Code skill that makes your agent stop and ask instead of guessing.**

Plans are imprecise. Specs have gaps. The default agent fills those gaps from training data and ships. This skill changes the default: when the spec is silent on a hit case, contradicts itself, or tempts a workaround, the agent stops and asks.

The cost is more clarification rounds. The win is fewer wrong outcomes delivered confidently.

## Install

### Claude Code plugin (recommended)

Two slash commands inside Claude Code — first registers this repo as a marketplace, second installs the plugin from it:

```
/plugin marketplace add krzysztofdudek/BePreciseSkill
/plugin install be-precise@be-precise-marketplace
```

Restart Claude Code (or run `/plugin reload`). The skill activates automatically when the agent transitions from a plan or spec into implementation.

To upgrade later: `/plugin marketplace update be-precise-marketplace` then `/plugin install be-precise@be-precise-marketplace` again.

### Single-file drop-in (any agent)

The canonical skill body is `skills/be-precise/SKILL.md` in this repo (one file, frontmatter-tagged). Copy it into your agent's skill directory:

- **Claude Code (user-level):** `~/.claude/skills/be-precise/SKILL.md`
- **Claude Code (project-level):** `.claude/skills/be-precise/SKILL.md` in your repo
- **Codex / Cursor / other agents:** wherever your tool reads skills or instructions from

## What it looks like running

> **Step 4 of plan: implement the retry logic.**
>
> The plan says: *"On 5xx response, retry up to 3 times with exponential backoff."*
>
> Question: how should I handle 429 (rate-limited)? It's not 5xx but it's a retryable case. Three options:
> 1. Treat 429 like 5xx — retry with backoff.
> 2. Use the `Retry-After` header if present, otherwise backoff.
> 3. Skip 429 from retry logic — leave it to the caller.
>
> I'd go with (2). Want me to proceed?

That's the kind of interruption you want. Spec doesn't cover 429, agent doesn't guess, you spend ten seconds on a decision instead of finding it in the diff an hour later.

## When it fires

The skill activates the moment the agent moves from a plan, design doc, or spec into implementation. Once active, it pushes toward asking rather than guessing whenever:

- the spec is silent on a case the agent hit
- the spec contradicts itself
- the agent is about to add a fallback, exception, or TODO the spec didn't sanction
- the agent feels "this is probably fine" without certainty
- a test needs to be weakened or skipped to make code pass
- a framework or type system constraint forces a workaround

Full table in [`skills/be-precise/SKILL.md`](skills/be-precise/SKILL.md).

## Why this exists

Agents default to autonomous completion. Faced with a gap in the spec, they pick something. Faced with ambiguity, they resolve it silently. By the time you read the output, the decisions are already made and buried in the diff.

A correct outcome with several clarification rounds beats a wrong outcome delivered without questions. The user is reachable. The plan is the source of truth, not the agent's judgment.

## FAQ

**Won't this make the agent constantly stop?**
Only when the spec doesn't cover the case. The skill includes an explicit table — if the spec answers the question, proceed; if it doesn't, ask. Most well-written plans don't trigger many stops. Sparse plans trigger many. That's the point.

**Does this conflict with TDD / debugging / verification skills?**
No. This governs the *attitude* when moving from spec to code, not the process. TDD still tells you to write the test first; this tells you to ask when the test you'd write isn't covered by the spec.

**What if I'd rather the agent just ship something?**
Then don't install this skill. It's deliberately the opposite of "ship something." If you want a fast pass with deviations resolved silently, this skill is the wrong tool.

## License

MIT

## See also

**[Liaison](https://github.com/krzysztofdudek/LiaisonSkill)** — for requests phrased in business or product terms (not a precise spec). A five-phase protocol reads back intent in the user's language, holds consent gates for destructive operations, delivers exactly what was confirmed, and closes with user-executable verification steps. Liaison handles the human ↔ intent gap; be-precise handles the intent ↔ code gap.

**[Researcher Skill](https://github.com/krzysztofdudek/ResearcherSkill)** — once the agent knows what you actually want, point it at a measurable goal and let it iterate overnight. Same author, complementary skill.

**[Yggdrasil](https://github.com/krzysztofdudek/Yggdrasil)** — architecture rules in Markdown your agent can't ignore. A reviewer verifies every change and feeds violations back into the agent's loop before it moves on. Where be-precise asks *before*, Yggdrasil enforces *after*.

---

<div align="center">
  <img src="yggdrasil.svg" alt="Yggdrasil" width="150" />
</div>
