# Lessons folder

This folder holds **auto-maintained, per-client rule files** captured by the `kalaa-lessons` skill. Each file is a small, consolidated ruleset of client preferences learned from writer feedback — applied to future content so the same mistake isn't repeated.

**Do not hand-edit these files.** They are written automatically by the skill. If a rule is wrong, tell the skill in plain language ("that competitor rule for Max & More is wrong, drop it") and it will fix + re-commit.

## One file per client

`lessons/<client>.md` — the stem matches the client's profile in `clients/`. Example: `clients/max_and_more_dhosa.md` ↔ `lessons/max_and_more_dhosa.md`. A file is created only when the first durable lesson for that client is captured.

## Two design guarantees

1. **Fully automatic** — captured from natural writer feedback; nobody edits these by hand, and the skill asks no blocking questions.
2. **Sharper, not heavier** — each file is a **bounded ruleset (~25 rules max)**, never an append-only log. New lessons are *consolidated* (deduped + generalized) into existing rules, so the file stays small forever and token cost stays flat. **History lives in git** (`git log <file>`), never inside the file.

## File schema

```markdown
# Lessons — {Client Name}
<!-- Auto-maintained by the kalaa-lessons skill. Max ~25 rules. Consolidated on every capture. History lives in git log. Do not hand-edit. -->

## Active rules (hard constraints — read on every content generation)

- 🚫 No English-translated marketing phrases (e.g. "come experience", "savor the taste") — [scope: all reels]
- 🚫 No competitor names (Aroma, Dosa Plaza) — [scope: all content]
- ✅ Always name the Rajhans landmark in proximity hooks — [scope: marketing reels]
```

- `🚫` = a "don't" rule · `✅` = an "always-do" rule. Every rule uses one prefix.
- Each rule ends with a `[scope: ...]` tag.
- No dates / source quotes / status lines in the file — that's what git is for.

## Scope tags

| Tag | Applies to |
|---|---|
| `[scope: <type> reels]` | Only that reel type (e.g. `marketing reels`, `testimonial reels`) |
| `[scope: all reels]` | Every reel, any type |
| `[scope: all content]` | Reels + monthly-plan posts + captions |
| `[scope: one-off campaign]` | Temporary, lowest priority; first dropped at the cap |

## How rules get used

**Stage A (current):** the standalone `kalaa-lessons` skill captures and shows rules. It is being proven in isolation.

**Stage B (later):** once validated, the three content skills (`viral-reel-scripter`, `monthly-content-planner`, `caption-generator`) will read the matching `lessons/<client>.md` in their Step 1 and treat every in-scope rule as a hard constraint — same level as a profile's "Topics to avoid".
