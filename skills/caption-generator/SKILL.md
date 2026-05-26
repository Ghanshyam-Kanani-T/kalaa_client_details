---
name: caption-generator
description: Generates ready-to-publish Instagram + Facebook captions (plus a bonus story version) for a Kalaa agency client's single post or reel. Pulls the client's profile from the Ghanshyam-Kanani-T/kalaa_client_details GitHub repo, or — crucially — picks up context automatically when a reel script or monthly planner was just generated in the same conversation, so the user doesn't have to re-explain the post. Produces one hook line + short body + specific CTA + exactly 5 hashtags for Instagram and exactly 3 for Facebook, in the client's preferred language. Use this skill whenever the user mentions writing a caption, generating a caption, captioning a post or reel, needing copy for a post, or simply says "caption for {client}" — even if they don't say "skill". This is a single-caption tool, not a bulk generator; for monthly content use monthly-content-planner, for reel scripts use viral-reel-scripter.
---

# Caption Generator — Kalaa

You are writing **one social media caption** — for a single post or reel — for a client of **Kalaa**, a social media agency in Surat, Gujarat. The output is three things in one go: an Instagram caption, a Facebook caption, and a short story version. Then you offer variants.

A "ready-to-publish" caption is one the social media manager can copy-paste, swap the IG handle if needed, and hit post — without rewriting. That is the bar.

## Why specificity matters

Captions die from two failure modes:

1. **Boring hooks.** "Check out our latest…", "We are happy to announce…", "Aaj hum laaye hain…" — these get the caption collapsed into "…more" before the reader engages. The hook line is the **only** line guaranteed to show in the feed. It has to earn the tap. Use a curiosity gap, a number, a contradiction, an emotion, or a real customer's words. Not a corporate preamble.

2. **Generic everything.** "#food" is dead. "Visit us today" is not a CTA. "Best restaurant in town" is something every restaurant says. Replace generic with specific: name the dish, name the neighborhood, name the timing, name the offer terms. If a competing client could paste your caption and it would still make sense, it's too generic.

The second-biggest design rule: **short**. Instagram users don't read essays. Three to six body lines. Anything longer is a blog post hiding in a caption.

---

## Workflow

### Step 1 — Identify the client (or pick up context)

**Context-aware path (preferred when applicable):** If the same conversation already produced a reel script (via `viral-reel-scripter`) or a monthly plan (via `monthly-content-planner`), the **client** and the **specific piece of content** are already known. Use them. Don't re-ask "which client?" or "what's the post about?" — that wastes the user's time and signals you weren't paying attention. Note: context tells you *who* and *what*, but it never tells you the **caption language** or **hashtag language** — those are always asked in Step 2 regardless of context.

Examples of context that's already in your hand:
- *"Write caption for the Burj Khalifa Dhosa reel"* → you just scripted it; the client is Max & More, the topic is the new product launch, the language is Gujarati.
- *"Caption for post #8 from the June plan"* → you just generated that planner; pull post #8's topic + theme + date.
- *"Caption for this"* → look at the most recent script/plan in the conversation and use that.

**Fresh-start path:** If there's no prior context, the user typically names the client: *"Caption for Shreeji Interior"*, *"Write a caption for Max & More post"*. Extract the client name, fuzzy-match to a file in the `clients/` folder of `Ghanshyam-Kanani-T/kalaa_client_details`.

| User says | Filename |
|-----------|----------|
| `max and more` | `max_and_more_dhosa.md` |
| `shreeji interior` | `shreeji_interior.md` (or similar) |
| `the diamond client` | (ambiguous — ask which one) |

List the directory, then fetch the matched file. See [References → Fetching files from the repo](#references). The repo is private; if the user's PAT isn't configured, surface the error — don't guess.

**If no match:** Show the writer the list of clients in the repo and ask them to pick.

### Step 2 — Ask caption details

There are five questions in total. **Two of them are mandatory every single time — they are never skipped, never defaulted, never inferred from context, even when the client file or a previous skill output in the conversation suggests an answer.** The other three are skippable when context already answers them.

**Always ask, every time (no exceptions):**

1. **Caption language?** Hindi · Gujarati · Hinglish · Gujlish · English.
2. **Hashtag language?** Same as caption · English only · Mix.

These are mandatory because the same client routinely wants different languages for different posts — a Father's Day post might run in pure Gujarati while a product-launch post for the same client runs in Hinglish. The client file's "Language Preference" field is a *default for the brand overall*, not a per-post decision. Defaulting silently from the file means the wrong language ships half the time. Ask, always.

**Ask only if context doesn't already answer:**

3. **What's the post / reel about?** Skip if a prior skill output in this conversation (reel script, monthly planner) already tells you what the content is.
4. **Platform?** Instagram · Facebook · Both. Default: Both. Skip if the user named the platform.
5. **Specific CTA?** Optional — DM us, visit today, call now, link in bio, save this, tag a friend. If they don't specify, pick the one that fits the post type.

**Conditionally ask (only when the client file doesn't already have it):**

6. **Full address + phone number** for the contact block. First check the client file's Business Info section for a `Full Address` and `Phone Number`. If both are present and look complete, use them silently — don't ask. If either is missing, or the `Location` field only has a neighborhood-level string like "Katargam, Surat" with no street/landmark, ask the user in the same turn as the mandatory two language questions: *"What's the full address and phone number for the contact block?"* Once provided, suggest they get added to the client's file in the repo so future captions don't need to re-ask. If the client has no physical storefront (e.g., digital marketing, remote-only), confirm with the user and skip the contact block for this post.

If the user pre-answered any of the mandatory two in their opening message ("Gujarati caption with English hashtags for the Father's Day post"), then the answer is given and you don't need to re-ask — but if they didn't, ask. Inferring from the client file's default *is not the same as them telling you*.

**Best practice when asking:** Bundle the mandatory two (and any of #3–5 you also need) into a single turn — don't drip-feed questions one by one. One concise message with the questions, wait for the reply, then generate.

### Step 3 — Generate the caption

Output three blocks in this order: Instagram, Facebook, Story. Then offer variants.

---

**INSTAGRAM CAPTION**

Structure:
- **Hook line** — line 1, the only line guaranteed to show in feed before "…more". Make it earn the tap. No greeting, no setup. A question, a number, a contradiction, a quote, an emotion. In the client's language.
- **Body** — 3 to 6 short lines, conversational, in the client's brand voice. Include the key message. One idea per line. No paragraphs.
- **CTA line** — one specific action, prefixed with a relevant emoji (🍴 for restaurants, 💍 for jewellery, ✨ for service businesses, etc.). Not "visit us today" — try "Walk in 7–10pm, Katargam" or "DM us to pre-order, limited per day."
- **Contact block** — placed between CTA and hashtags, matches the format Kalaa clients already use on their feeds:
  ```
  📍 ADDRESS:
  {full street address from client file}

  📞 CALL:
  {phone number from client file}
  ```
  This is the **default for every IG caption** — Kalaa's clients are mostly local SMBs and the address + phone are what convert a scroll into a walk-in or a call. Source order: (1) pull from the client file's Business Info section (`Full Address` and `Phone Number` fields); (2) if missing or too short to be useful (e.g., file only says "Katargam, Surat" with no street name), ask the user in Step 2 before generating; (3) only skip the block entirely if the client has no physical storefront (e.g., a digital marketing or remote-only client) **or** the user explicitly tells you to skip it for this post.
- *(blank line)*
- **Hashtags** — **exactly 5**, on one line, separated by spaces. Composition:
  - 2 niche-specific (`#MaxAndMoreDhosa`, `#DosaLovers`)
  - 1 location (`#SuratFood`)
  - 1 trending (`#MonsoonFood`)
  - 1 brand or campaign (`#BurjKhalifaDhosa`)
  Never `#food`, `#instagood`, or other ultra-generic tags. They add noise, not reach.
- **Mentions** — `@<client_handle>` from the file. Add location/partner tags only if relevant.

Emojis: 2 to 4 max in the **caption body** (the contact block's 📍 and 📞 are separate and don't count against this budget). Place them where they earn their spot — typically next to the hook or CTA. Don't pepper them through every line.

---

**FACEBOOK CAPTION**

Facebook is a different beast. The audience reads more, the algorithm rewards storytelling, and hashtags barely move the needle. Adjust accordingly:

- **Hook line** — same scroll-stopping rule, but you have more permission to set a scene.
- **Body** — 4 to 8 lines, slightly more storytelling than Instagram. The same idea, expanded with one more beat of context or emotion.
- **CTA line** — same specificity rule, with the same emoji prefix convention.
- **Contact block** — same `📍 ADDRESS:` / `📞 CALL:` format as Instagram, between CTA and hashtags. Same source rules (file → ask → skip only if no storefront or user says skip).
- *(blank line)*
- **Hashtags** — **exactly 3**, just the most relevant. Facebook users don't search hashtags much; these are for the brand's discovery surface, not reach.
- **Mentions** — usually unnecessary unless tagging a partner or location page.

---

**STORY TEXT (bonus)**

The same content sliced for a 24-hour story:

- **1 or 2 punchy lines** — much shorter than the post caption. Built around a tap or a sticker, not a read.
- **Sticker suggestion** — one of: poll · question · emoji slider · quiz · countdown · location · DM-me · link sticker. Pick the one that fits the post's intent (engagement vs. action vs. announcement).

---

### Step 4 — Offer variants

After delivering the three blocks, end with a single line:

> *"Want a different version? I can make it more funny / emotional / formal / casual / festive — or change the language."*

Don't pre-emptively generate variants. The user picks if they want one.

---

## Self-check before delivering

Run through this silently before sending:

- [ ] You explicitly asked the user for caption language and hashtag language before generating — you did not default silently from the client file or from prior context.
- [ ] Hook line earns the tap — no "Check out", "We are happy to announce", or generic greeting.
- [ ] Body is 3–6 lines for IG, 4–8 for FB. Not a wall of text.
- [ ] CTA is a *specific* action (named place, named time, named offer, named DM).
- [ ] **Instagram hashtags = exactly 5.** Facebook hashtags = exactly 3. Count them.
- [ ] Contact block (📍 ADDRESS + 📞 CALL) is present in both Instagram and Facebook captions, with values pulled from the client file or supplied by the user — unless the client has no physical storefront or the user explicitly asked to skip it.
- [ ] Hashtag mix is right (niche / location / trending / brand) — no `#food`-level generic.
- [ ] Language matches what the user selected (or the client file default if they didn't).
- [ ] Emojis ≤ 4, all earned.
- [ ] No conflict with the client's `Topics to avoid`.
- [ ] Brand vibe matches — premium-jewellery doesn't get meme tone; family-restaurant doesn't get cold corporate.
- [ ] If this caption is for a reel, it *complements* the video — doesn't repeat the script word-for-word.
- [ ] Story version is 1–2 lines, not a mini-caption.
- [ ] You ended with the variants offer.

If any check fails, fix it before sending.

---

## Things to actively avoid

- **Corporate openings.** "We are excited to announce", "Introducing our newest", "Check out", "Don't miss out on" — all dead.
- **Generic CTAs.** "Visit us today", "Contact us for more info", "Stay tuned" — give the reader a real next step.
- **Hashtag stuffing.** 20 hashtags isn't more reach in 2026; it looks spammy. Five well-chosen tags out-perform.
- **Generic hashtags.** `#food`, `#love`, `#instagood`, `#beautiful` — dead weight, sometimes algorithmically penalized.
- **English-by-default.** If the client speaks Gujarati to their audience, the caption is in Gujarati or Gujlish — not English with a token Gujarati word thrown in.
- **Repeating the reel.** If the reel script already says everything, the caption shouldn't repeat it. Caption gives the *context*, hook, and CTA — not a transcript.
- **Emoji clutter.** A line with 4 emojis in 8 words looks like a bot.
- **AI tells.** "In today's fast-paced world", "When it comes to", "Let's dive in" — these are AI tells. Read your draft once and cut anything that sounds generated.

---

## References

### Client file template

When creating a new client file (Step 1 fresh-start path, no-match scenario), use this structure — matches existing files in the repo:

```markdown
# {Business Name}

## 1. Business Info
- **Business Name:**
- **Business Type:**
- **Location:** *(neighborhood-level, e.g., "Katargam, Surat")*
- **Full Address:** *(street + landmark + city + state + PIN — used in IG/FB caption contact block)*
- **Phone Number:** *(in +91 XXXXX XXXXX format — used in IG/FB caption contact block)*
- **Website/Google Maps link:**

## 2. Target Audience
- **Age Range:**
- **Gender:**
- **Audience Type:**
- **Language Preference:**

## 3. Brand Identity
- **Brand Vibe:**
- **Tagline:**
- **Colors/Fonts preference:**

## 4. Products / Services
- **Key Products/Services:**
- **USP / What makes them different:**
- **Best Sellers / Most Popular:**

## 5. Content Details
- **Content Types:**
- **Posting Frequency:**
- **Hashtags they always use:**
- **Mentions to include:**

## 6. Social Handles
- **Instagram:**
- **Facebook:**
- **YouTube:**
- **Other:**

## 7. Special Instructions
- **Topics to avoid:**
- **Client preferences:**
- **Festival/seasonal relevance:**
```

### Fetching files from the repo

The repo `Ghanshyam-Kanani-T/kalaa_client_details` is private. Two paths:

1. **`gh` CLI** if installed: `gh api repos/Ghanshyam-Kanani-T/kalaa_client_details/contents/clients` to list, then `gh api .../clients/<file>.md --jq .content | base64 -d` to fetch.
2. **GitHub REST API** with the user's PAT — fetch `https://api.github.com/repos/Ghanshyam-Kanani-T/kalaa_client_details/contents/clients/<file>.md` with `Accept: application/vnd.github.v3.raw` to get the file body directly.

If the PAT isn't configured locally, tell the user plainly — don't proceed with guesses.

### Hashtag rationale (5 + 3)

The 5-tag Instagram rule comes from current 2026 best practice — Instagram's algorithm in 2026 favors *relevance* over volume, and accounts using 5–10 highly-targeted hashtags consistently out-perform accounts stuffing 20–30. Five is the lower-bound sweet spot: enough signal for discovery, zero clutter in the caption visual.

The 3-tag Facebook rule is even tighter — Facebook users don't search hashtags meaningfully, so 3 is for brand surface only, not reach.

Both numbers are **exact**, not "around." A consistent caption pattern is itself a brand asset.
