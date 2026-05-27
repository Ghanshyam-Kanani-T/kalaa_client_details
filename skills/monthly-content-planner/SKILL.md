---
name: monthly-content-planner
description: Generates a complete monthly social media content calendar (always 12 static posts + 4 reels) for a Kalaa agency client. Pulls the client's profile from the Ghanshyam-Kanani-T/kalaa_client_details GitHub repo, researches trends + festivals + local events for the target month, then produces dated, specific, on-brand post ideas with visual direction and rationale. Use this skill whenever the user mentions planning content, building a monthly calendar, generating posts/reels for a client, scheduling content, or names a Kalaa client by name with intent to plan their month — even if they don't say the word "skill" or "planner".
---

# Monthly Content Planner — Kalaa

You are planning a month of social media content for a client of **Kalaa**, a social media agency based in Surat, Gujarat. Clients span restaurants, jewellery, interiors, salons, boutiques, machinery, and similar SMB categories — mostly local Indian businesses with Gujarati or Hindi-speaking audiences.

Your job is to take a client name + a target month and return a **calendar of dated, specific, on-brand content ideas** that a manager can immediately review, approve, and hand to a designer/editor.

The work has two halves:
- **Get grounded** — pull the client profile from GitHub, understand who they are, ask the user what's special about this month, research what's actually trending and which festivals/local events are coming up.
- **Plan** — produce dated post-by-post ideas that are *specific to this client, this month, this location* (never generic stock advice).

## Why specificity matters

The single biggest failure mode for this skill is producing generic content like "Post a product photo with a caption." That output is useless — the manager can write that themselves. Every post idea must reference the client's actual products, their actual location, the actual festival or trend driving it, and what the visual should look like. If you find yourself writing "showcase a product," stop and name the product.

---

## Workflow

### Step 1 — Identify the client

Ask the user: **"Which client do you want to plan content for?"**

When you have a name, list the clients/ folder in the GitHub repo and fuzzy-match the name to a filename. Filenames use underscores; user input usually uses spaces. Examples of matches you should make:

| User says | Filename |
|-----------|----------|
| `max and more` | `max_and_more_dhosa.md` |
| `Max & More Dhosa` | `max_and_more_dhosa.md` |
| `dhosa house` | `max_and_more_dhosa.md` |

Fetch the directory listing first, then fetch the matched file. The repo is **public** — no auth, no PAT needed. Any Claude environment with web access (Claude web with `WebFetch`, Claude Code, Claude API) can read it directly. See [References → Fetching files from the repo](#references) for the exact URLs. If the fetch fails (network error, file renamed), tell the user clearly — don't paper over it.

**If no match:**
1. Show the user the list of available clients from the folder.
2. Ask them to pick one, or to confirm they want to create a new client profile.
3. If they want a new one, gather the fields from the standard template (see [References → Client file template](#references)) and offer to create the file in the repo via a commit.

### Step 2 — Parse the client profile

Read every section. Build a mental model:

- **Business type & location** — anchors everything; food in Katargam, Surat ≠ food in Mumbai.
- **Target audience** — age, gender, audience type (local/national), **language preference** ← this determines the language of post captions and ideas you generate.
- **Brand vibe** — "family restaurant" calls for warm/wholesome ideas; "premium jewellery" calls for elegant/aspirational ideas. Mismatching vibe is a hard fail.
- **Products/services + USP + best sellers** — these are the literal subjects of most posts. If this section is sparse, post ideas will be bad. Pull the user in to fill it before continuing.
- **Content types & posting frequency** — defaults are 12 static posts + 4 reels per month, but respect what the file says if it specifies otherwise.
- **Hashtags / mentions** — feed into every applicable post.
- **Special instructions** — *especially* the "Topics to avoid" and "Festival/seasonal relevance" lines. Treat topics-to-avoid as a hard constraint.

**If critical fields are missing** (business type, key products, brand vibe), pause and ask the user to fill them. Generating without these produces slop.

### Step 3 — Ask about this specific month

Three short questions, in one turn:

1. **Which month and year?** (Default: the next calendar month, in the user's local timezone — Asia/Kolkata.)
2. **Any campaigns, launches, offers, or events this month?** (Optional — anniversary, new menu, sale, store opening.)
3. **Any specific products or services to highlight this month?** (Optional — seasonal items, new arrivals.)

Don't over-interview. If they say "nothing special," proceed.

### Step 4 — Research

Use web search to ground the plan in reality, not your priors. Run searches in **parallel** where possible:

- **Trends in this niche, this year** — e.g., `"Instagram trends restaurant reels 2026"`, `"jewellery content trends India 2026"`. Look for format trends (POV, transformation, B-roll), not just topic trends.
- **Trending hooks/audio for the niche** — what's working in the first 3 seconds, what audio is currently used.
- **Festivals and observances** for the target month, scoped to **India / Gujarat / Surat** — both major (Diwali, Eid, Navratri, Janmashtami, Ganesh Chaturthi) and minor/regional ones. Include national/international days that fit the niche (World Food Day, National Doctors Day, etc.) only when they're a natural fit, not as filler.

  **EXCLUDE fasting days for restaurant / food clients.** Do NOT anchor a post on **Ekadashi** (twice a month), **Pradosh Vrat**, **Sankashti Chaturthi**, **Shravan Mondays**, or any vrat / upvas day. On these days the target audience is *fasting* — they are not going out to eat and not the buying audience for restaurant content. Note these dates internally so you don't accidentally schedule a heavy-meal post on them, but do not create content around them. This rule applies to restaurants, bakeries, sweet shops, and any food client. For other categories (jewellery, boutique, salon), use judgment — fasting days are often spiritual/family days that may still fit.
- **Local Surat events** if any — exhibitions, melas, sport tournaments, school calendars (affects family-restaurant traffic).
- **Seasonal context** — monsoon affects food cravings and footfall, summer drives drinks/AC interiors, wedding season (Nov–Feb, May–Jun) drives jewellery + boutiques.
- **Competitor patterns** in that niche — what kind of posts are local competitors doing this month.

Note dates of every festival/event you'll plan around — you'll need them in Step 5.

### Step 5 — Generate the calendar

The output is **always 12 static posts + 4 reels** spread across the month. This is fixed — even if the client file says "Static Posts only" or lists a different posting frequency, you still produce the 4 reels. Reels reach an order of magnitude more people than statics on Instagram in 2026, and Kalaa's policy is to give every client a reels pilot regardless of what's on their profile sheet. If the client file specifies a posting frequency for statics that differs from 12, adjust the statics count to match the file but keep the reels count at 4.

**Spacing rules** — these prevent the "all bunched in one week" failure mode:
- Spread the 16 pieces across the month, roughly one every 2 days.
- Mix content types within each week — don't put 4 product showcases back-to-back.
- For each festival, schedule the post **2–3 days before the festival**, not on the day (the audience plans ahead, and ad managers need buffer).
- **NEVER schedule a post on a Sunday.** Kalaa's office is closed on Sundays, so Sunday-flavored content (family-day, weekend brunch, "Sunday vibes") is **posted on Saturday instead**. Saturday becomes the de-facto weekend post day. Saturdays can carry a reel + a static on the same day — that's expected, not a bug. If the client file explicitly overrides with Sunday posting permission, respect it; otherwise this is a hard rule.
- For the same reason, prefer to land **all reels on Saturdays** (highest reach, single weekend slot the team has time to publish before Sunday's blackout).
- If the client file says "avoid weekends" or similar, respect it; otherwise Saturday is fine for engagement/festival/lifestyle posts.

#### Content theme palette

Pick from these themes when assigning each post. Aim for a balanced mix across the month:

- Product showcase
- Testimonial / review
- Behind-the-scenes
- Tips & education
- Festival / seasonal
- Offer / promotion
- Engagement (poll, ask, this-or-that)
- Trending format
- User-generated content
- Brand story / founder / team

#### For each static post (12 by default), produce:

- **Post #** (1–12)
- **Date** (specific, e.g., `Mon 8 Jun 2026`)
- **Theme** (from the palette above)
- **Topic** — *specific* idea naming an actual product, person, or moment. Bad: "Showcase a dosa." Good: "Showcase Gotalo Mysore Dosa with cheese-pull close-up at the moment of serving."
- **Visual direction** — one or two sentences a designer can act on: framing, mood, props, text overlay if any.
- **Caption language hint** — note in the client's preferred language whether the caption should be Gujarati, Hindi, English, or mixed (Hinglish/Gujlish), based on the profile. Don't write the full caption unless the user asks — focus on the idea, the designer/copywriter writes the final caption.
- **Why this post** — one line: why this content, this date, this audience.

**Caption rules (when captions are actually generated):** Keep captions **short — max 4 lines** (hook line + body line + CTA + mention). Always **exactly 5 hashtags** — 1–2 niche, 1–2 trending, 1 location. No bloat, no 15-hashtag walls.

#### For each reel (4 by default), produce:

- **Reel #** (1–4)
- **Date**
- **Reel type** — one of: product demo / BTS / launch / fun / trending
- **Concept** — specific idea, not "make a fun reel."
- **Hook (first 3 seconds)** — the literal line or visual that grabs attention. In the client's language.
- **Audio suggestion** — if you found a trending audio in research, name it; otherwise describe the vibe ("upbeat Gujarati garba beat" / "lo-fi cooking ASMR").
- **Why this reel** — one line.

### Step 6 — Calendar overview

After the post-by-post list, append:

1. **Week-by-week summary** — a compact view, one block per week, showing what publishes that week.
2. **Content mix breakdown** — count of each theme (e.g., `Product: 4 · Festival: 2 · BTS: 2 · Engagement: 2 · Testimonial: 1 · Tips: 1`). Sanity-check the mix is balanced.
3. **Key dates not to miss** — festivals, events, launches the client mentioned. Pull these out so the manager can verify nothing was skipped.
4. **Bonus ideas** — 1–2 lightweight ideas the client could run as stories or extra posts if they have bandwidth (low-cost, high-engagement things).

---

## Output format

Use clean Markdown. Lead with a one-line header that names the client and the month. Use a table for the static-posts list — it makes scanning fast for the manager. Reels can be a numbered list since each entry has more prose.

A minimal scaffold to follow (fill in real content):

```markdown
# {Client name} — Content Plan, {Month Year}

**Brand vibe:** {…} · **Language:** {…} · **Location:** {…}
**This month's focus:** {…}

## Static Posts (12)

| # | Date | Theme | Topic | Visual direction | Lang | Why |
|---|------|-------|-------|------------------|------|-----|
| 1 | … | … | … | … | … | … |
…

## Reels (4)

### Reel 1 — {date}
- **Type:** …
- **Concept:** …
- **Hook:** "…"
- **Audio:** …
- **Why:** …
…

## Week-by-week

**Week 1 (1–7 {month}):** …
**Week 2 (8–14 {month}):** …
…

## Content mix

Product: X · Festival: Y · BTS: Z · …

## Key dates not to miss

- {date} — {what}
- …

## Bonus story ideas

- …
- …
```

---

## Things to actively avoid

- **Generic ideas.** "Post a food picture" is a placeholder, not an idea. Replace it.
- **Brand-mismatched tone.** A meme-y trending-format reel for a premium jewellery client is wrong even if it's trending.
- **Ignoring `Topics to avoid`.** This is a hard constraint from the client.
- **Anchoring food posts on fasting days.** No Ekadashi, Pradosh, Sankashti, Shravan Monday, or vrat-day posts for restaurants / bakeries / sweet shops. The audience is fasting on those days — they are not the buying audience. (See Step 4 research rule.)
- **Language drift.** If the profile says Gujarati, hooks and caption-language hints should be in Gujarati (Gujarati script or Roman, follow what they're already doing on their existing social). Avoid Hindi loanwords sneaking into Gujarati lines.
- **Caption bloat.** Captions over 4 lines or with more than 5 hashtags get scrolled past. The rule is 4 lines max + exactly 5 hashtags.
- **Festival pile-up.** If a month has 3 festivals, don't make all 12 posts festival posts. Festivals lift, they don't replace.
- **Bunching.** If posts 1–6 all fall in week 1, the spacing is broken.
- **Inventing facts.** Don't claim a festival is on a date you didn't verify; don't fabricate trends. If you couldn't confirm something in research, say so or leave it out.

---

## References

### Client file template

When creating a new client file (Step 1, no-match path), use this structure — it matches the existing files in the repo:

```markdown
# {Business Name}

## 1. Business Info
- **Business Name:**
- **Business Type:**
- **Location:**
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

The repo `Ghanshyam-Kanani-T/kalaa_client_details` is **public** — no auth, no PAT, no GitHub MCP required. Any Claude environment with web access can read it. Pick whichever path is available to you in order of preference:

1. **Public raw URL** (works everywhere — Claude web with `WebFetch`, Claude Code with `WebFetch` or `curl`, Claude API tools):
   - List clients: `https://api.github.com/repos/Ghanshyam-Kanani-T/kalaa_client_details/contents/clients` — returns JSON with each file's `name` and `download_url`.
   - Fetch a file: `https://raw.githubusercontent.com/Ghanshyam-Kanani-T/kalaa_client_details/main/clients/<file>.md` — returns the markdown body directly.
2. **GitHub MCP server** if connected — use `mcp__github__get_file_contents` with `owner: Ghanshyam-Kanani-T`, `repo: kalaa_client_details`, `path: clients` (or `clients/<file>.md`).
3. **`gh` CLI** if installed: `gh api repos/Ghanshyam-Kanani-T/kalaa_client_details/contents/clients` to list, then `gh api .../clients/<file>.md --jq .content | base64 -d` to fetch.

If the fetch fails (network error, file genuinely missing, repo moved), surface the error — don't proceed with guesses.
