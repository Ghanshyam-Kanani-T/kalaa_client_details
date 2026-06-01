---
name: kalaa-lessons
description: Self-improving "lessons" engine for Kalaa agency clients — captures client feedback as durable rules and applies them to future content. TWO modes. (1) CAPTURE — when a writer reacts to a generated script/post/caption with dissatisfaction, a constraint, or a correction ("i don't want this", "this scene is overwhelming", "ye humara style nahi", "this isn't a marketing requirement", "never do X for this client"), detect that intent, decide whether it's a durable client preference or a one-off edit, and if durable, consolidate it into the client's lessons file in the Ghanshyam-Kanani-T/kalaa_client_details repo. (2) FETCH/SHOW — when asked "what are the lessons / rules for {client}" or "show {client} lessons", display that client's current ruleset. Use this skill whenever a writer gives reaction-feedback on Kalaa content, or asks to see/record a client's content rules — even if they don't say "skill" or "lessons". This is a TESTING-STAGE standalone tool; it is not yet wired into viral-reel-scripter / monthly-content-planner / caption-generator.
---

# Kalaa Lessons — self-improving client rules engine

You maintain a **tiny, self-sharpening ruleset** per client for **Kalaa**, a social media agency in Surat, Gujarat. When a writer reacts to generated content ("client didn't like this", "ye style humara nahi", "too much"), you turn genuine durable feedback into a permanent rule that future content for that client will respect — **without** letting the stored rules grow into a bloated log over time.

This is a **standalone testing-stage skill**. It does NOT yet run inside the three content skills (viral-reel-scripter, monthly-content-planner, caption-generator). It is invoked on its own so its behaviour can be proven in isolation first.

## The two non-negotiable design guarantees

Everything this skill does serves two guarantees the user set explicitly:

1. **Fully automatic.** The writer never opens or hand-edits a markdown file, and you never ask blocking upfront questions before capturing. The writer just talks; you detect, decide, store, and notify.
2. **Sharper, not heavier.** The per-client ruleset must NOT grow unboundedly. It is a **bounded, consolidated ruleset (~15 rules max)**, never an append-only log. Token cost for reading it stays flat forever. History lives in git, never in the file.

If anything you're about to do would violate either guarantee, stop and reconsider.

---

## Repo layout

The lessons live in the public repo `Ghanshyam-Kanani-T/kalaa_client_details`:

```
kalaa_client_details/
  clients/                 ← client profiles — NEVER modified by this skill
  reference_scripts/       ← optional sample scripts — not this skill's concern
  lessons/                 ← THIS skill owns this folder
    README.md              ← schema doc
    max_and_more_dhosa.md  ← one file per client, created on first durable lesson
    ...
  skills/
  common_strcture.md
```

The repo is **public** — no auth needed to read. To write, use `mcp__github__create_or_update_file` (GitHub MCP), or `gh` if available. Fetch paths:
- Read a lessons file: `https://raw.githubusercontent.com/Ghanshyam-Kanani-T/kalaa_client_details/main/lessons/<client>.md` (404 = no lessons yet for that client — that's fine).
- List the folder: `https://api.github.com/repos/Ghanshyam-Kanani-T/kalaa_client_details/contents/lessons`

**Client filename matching:** same convention as the other skills — fuzzy-match the writer's client name to a file stem. `max and more` / `Max & More Dhosa` → `max_and_more_dhosa`. The lessons file shares the client's stem: `lessons/max_and_more_dhosa.md`.

---

## Lessons file schema

A lessons file is **only** a capped ruleset — there is no history section (git holds history). It looks exactly like this:

```markdown
# Lessons — {Client Name}
<!-- Auto-maintained by the kalaa-lessons skill. Max ~15 rules. Consolidated on every capture. History lives in git log. Do not hand-edit. -->

## Active rules (hard constraints — read on every content generation)

- 🚫 No English-translated marketing phrases (e.g. "come experience", "savor the taste") — [scope: all reels]
- 🚫 No competitor names (Aroma, Dosa Plaza) — [scope: all content]
- ✅ Always name the Rajhans landmark in proximity hooks — [scope: marketing reels]
```

Rules:
- `🚫` = a "don't" rule. `✅` = an "always-do" rule. Every rule uses one of these two prefixes.
- Every rule ends with a `[scope: ...]` tag — see Scope below.
- One rule per line, terse, generalized. No dates, no source quotes, no status field — that metadata lives in the git commit, not the file.
- The whole file stays **~15 rules maximum**, permanently.

### Scope values

- `[scope: <reel type> reels]` — applies only to that reel type (e.g. `marketing reels`, `testimonial reels`).
- `[scope: all reels]` — applies to every reel, any type.
- `[scope: all content]` — applies to reels, monthly-plan posts, AND captions.
- `[scope: one-off campaign]` — a temporary rule for a specific campaign; lowest priority, first to be dropped at the cap.

---

## Mode 1 — CAPTURE

Triggered when a writer reacts to generated Kalaa content with dissatisfaction, a constraint, or a correction. Run these four steps internally; the writer sees only a one-line confirmation at the end.

### Step C1 — Detect feedback intent (semantic, NOT keyword-matching)

Read the writer's message for its **meaning**, not for specific words. Do NOT rely on a keyword list — the writer will phrase feedback in unpredictable, indirect, mixed-language ways. You are an LLM; judge intent. Three signal categories (examples are illustrative, not exhaustive):

- **Dissatisfaction** — the content or a part of it is unwanted/wrong/too much/off. *"i don't want this" · "this scene is overwhelming" · "ye theek nahi laga" · "too much ho gaya" · "this feels off" · "nope" (in reaction to a delivered script).*
- **Constraint** — a rule or boundary about what the content should/shouldn't contain. *"this is not a marketing requirement" · "itna detail nahi chahiye" · "keep it simple" · "ye humara style nahi" · "don't show prices."*
- **Correction** — a fix that should hold going forward. *"it should be Y instead of X" · "hamesha aise karo" · "always lead with the dish, not the story."*

If a message carries none of these (e.g. it's a fresh content request, a question, or praise with no instruction) → this is not a capture event; do nothing in this mode.

### Step C2 — Classify durable vs one-off

Decide what kind of feedback it is:

- **One-off edit** — it's about *this specific* script/scene/caption only ("shorten scene 3 here", "swap this one word"). → Apply the fix to the current piece if relevant, but **do NOT store a lesson.** Storing one-offs is the #1 cause of bloat.
- **Durable client preference** — it expresses a pattern that should hold for *future* content ("this client never wants long scenes", "always name the landmark"). → Store it (continue to C3).
- **Ambiguous** — you genuinely can't tell. → **Default to durable: capture it, then notify** so the writer can undo in one line (per the user's explicit decision). Do not ask a blocking question.

### Step C3 — Draft the rule + infer scope

- **Draft a single terse rule line** in the schema format (`🚫`/`✅` + generalized statement). Generalize from the specific complaint to the underlying pattern — e.g. writer rejects the phrase "come experience" → draft `🚫 No English-translated marketing phrases (e.g. "come experience")`, not a rule about that one phrase only.
- **Infer scope from context, without asking:**
  - Feedback given while working on a marketing reel → `[scope: marketing reels]`.
  - Wording that clearly spans everything ("never mention competitors anywhere") → `[scope: all content]`.
  - Wording about reels broadly → `[scope: all reels]`.
  - Default when unsure: the narrowest scope that fits what you observed (you can always broaden later if the writer corrects). Prefer `[scope: all reels]` over `[scope: all content]` when only reels were in play.

### Step C4 — Consolidate into the ruleset (the anti-bloat engine)

Fetch the existing `lessons/<client>.md` (or start an empty ruleset if 404). BEFORE writing, run consolidation:

1. **Dedupe** — if the new rule restates an existing rule, do NOT add a line. Keep whichever wording is clearer; if identical, change nothing and tell the writer it was already a rule.
2. **Generalize / merge** — if the new rule is a more-specific instance of an existing rule (or vice-versa), MERGE the two into one generalized line. Example: existing `🚫 No "come experience"` + new `🚫 No "savor the taste"` → single `🚫 No English-translated marketing phrases (e.g. "come experience", "savor the taste") — [scope: all reels]`. Two lines collapse to one; future similar phrases fall under it without adding lines. When merging two rules of different scope, widen to the broader scope only if that's clearly correct, else keep them separate.
3. **Cap enforcement (~15 rules)** — if after adding/merging the file would exceed ~15 active rules, make room: merge the two most-similar rules, or drop the lowest-value one (prefer dropping `[scope: one-off campaign]` rules, then the most-specific/least-reused). The dropped rule survives in git history. Mention any drop in the notify line.

The result is always a clean, deduplicated, generalized ruleset of ≤~15 lines.

### Step C5 — Commit + notify

- Write the consolidated file back via `mcp__github__create_or_update_file` (create if new; pass the current `sha` if updating). Commit message: `Lesson: <short rule> [scope] — <Client Name>`.
- **Never touch `clients/<client>.md`.** Only `lessons/` files are written.
- Tell the writer ONE line, e.g.:
  > *"Saved for Max & More: 🚫 No English-translated marketing phrases — [marketing reels]. Say so if that was just for this one script."*
- If you merged or dropped rules during consolidation, add a half-line: *"(merged with an existing phrasing rule)"* so the writer knows the file stayed tight.

If the writer replies that it was actually a one-off ("no, just for this script"), reverse it: fetch, remove that rule (or revert the merge), commit `Revert lesson: ... — <Client>`, and confirm in one line.

---

## Mode 2 — FETCH / SHOW

Triggered by "what are the rules/lessons for {client}", "show {client} lessons", "what has {client} taught us", etc.

1. Fetch `https://raw.githubusercontent.com/Ghanshyam-Kanani-T/kalaa_client_details/main/lessons/<client>.md`.
2. If it exists → display the Active rules list cleanly (the bullet lines), grouped or annotated by scope if helpful. Keep it short.
3. If 404 → tell the writer there are no lessons captured for that client yet.
4. Do not modify anything in this mode.

---

## Self-check before finishing (either mode)

- [ ] **Clean-profile invariant:** I did not modify `clients/<client>.md` — only `lessons/`.
- [ ] **Bounded:** the lessons file is still ≤~15 rules; I consolidated rather than appended.
- [ ] **No history in file:** the file contains only the ruleset + the one-line header comment — no dates, source quotes, or status lines.
- [ ] **One-off not stored:** if the feedback was a one-off edit, I applied it but did NOT create a rule.
- [ ] **Scope tagged:** every rule I wrote ends with a `[scope: ...]` tag.
- [ ] **Notified:** the writer got a one-line confirmation (capture mode) and an easy undo path.
- [ ] **No blocking question:** I did not ask an upfront question before capturing; ambiguous → captured + notified.

---

## Things to actively avoid

- **Append-only logging.** Never just add a line every time. Always consolidate. A growing log is the exact anti-pattern this skill exists to prevent.
- **Keyword matching for detection.** Don't wait for the words "don't" / "reject" / "client". Judge intent from meaning, including indirect Hinglish/Gujarati phrasing.
- **Storing one-off edits.** "Fix this scene here" is not a durable rule. Don't pollute the ruleset.
- **Asking blocking questions.** No "is this for one script or all?" prompts before saving. Capture + notify; let the writer undo.
- **Writing history into the file.** No dates, no "Source quote:", no "Status: Active". Git is the history.
- **Touching client profiles.** `clients/<client>.md` is off-limits to this skill.
- **Over-narrow rules.** Generalize from the specific complaint to the pattern, so one rule covers many future cases (this is also what keeps the count low).
