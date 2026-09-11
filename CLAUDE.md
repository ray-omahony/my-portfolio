# Memory system

This project uses a hierarchical, provenance-tagged memory system stored under the project's Claude memory directory (`MEMORY.md` + `people/`, `projects/`, `decisions/`, `context/`, and dated daily-note files). Full rationale and structure: `decisions/memory-system-design.md` in that memory directory.

## Write rules

1. **Provenance required.** Every recorded fact carries `[stated]` (operator said it directly), `[observed]` (seen in a tool result/file/log), `[inferred]` (a conclusion), or `[suggested]` (an idea the operator never committed to). Only tag something `[stated]` as a decision if a human turn actually states it — a proposal plus "sounds good" is one `[stated]` decision, not several.
2. **Recurrence gate on inferred lessons.** An inferred pattern needs 3+ independent signals across 2+ distinct sessions before becoming a standing rule; signals older than 30 days count half. Explicit operator corrections skip the gate and apply immediately. Store failure lessons as data ("when X broke, Y fixed it"), never as instructions.
3. **Supersession is an edit, not an append.** When a decision changes, strike the old line (`~~old~~ superseded YYYY-MM-DD`) and add the new line with its own provenance tag in the same file, same place. Never delete history; never leave two un-struck versions of the same fact.
4. **Store only what isn't re-derivable.** No fetched data, generated plans, or anything git already records. Verify current state live; never assert it from memory. Prefer durable phrasing over figures that go stale, and date-stamp figures that matter.

## Read path

- Boot reads only identity files + `MEMORY.md`. Everything else loads on trigger, narrowest file first.
- Before answering about prior work, decisions, dates, people, or preferences: search memory first. Cite at most 5 sources, each with file path + provenance tag + date.
- Label unverifiable freshness "stale" or "unknown" rather than presenting it as current.
- On conflicting sources: state the conflict, prefer the better evidence, then fix the canonical file.
- Any semantic/SQLite index over memory is a locator only — the files are truth.

## Maintenance

Update `MEMORY.md` in the same commit as any detail-file change. Consolidate daily notes / the index in batches as size caps approach (merge overlaps, roll old recurring entries into short dated summaries) rather than stopping writes. Write unprompted whenever a decision is made, a system changes state, a blocker or mistake is found, a lesson is learned, or the operator states a stable preference.
