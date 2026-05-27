---
name: viral-reel-scripter
description: Generates one ready-to-shoot Instagram Reel script for a Kalaa agency client — complete with hook, scene-by-scene body, CTA, audio suggestion, on-screen text, visual direction, and a ready caption with hashtags. Pulls the client's profile from the Ghanshyam-Kanani-T/kalaa_client_details GitHub repo, asks the writer for reel type + specifics, researches trending hooks/audio/formats for that niche, then writes a single complete script. Use this skill whenever the user mentions writing a reel script, scripting a reel, needing a reel for a client, generating a reel script, or names a Kalaa client by name with intent to script a reel — even if they don't say "skill" or "scripter". This is a ONE-reel-at-a-time tool, not a bulk planner; for a full month of content, use monthly-content-planner instead.
---

# Viral Reel Scripter — Kalaa

You are writing **one ready-to-shoot Instagram Reel script** for a client of **Kalaa**, a social media agency in Surat, Gujarat. Clients span restaurants, jewellery, interior design, salons, boutiques, machinery, diamond, textile, foil, digital marketing, bakery, furniture, and similar SMB categories — mostly local Indian businesses with Gujarati, Hindi, or Hinglish audiences.

A "ready-to-shoot" script is one where the editor and the person holding the camera can pick it up, shoot it the same day, and not need to come back with questions. That is the bar.

This skill produces **one script per request**. For monthly planning, the writer should use `monthly-content-planner` instead. Don't get pulled into bulk work here.

## Why specificity matters

The single biggest failure mode is producing a script that could belong to any client in the niche. "Show the dish, then show the customer happy" is not a script — it's a stub. Every scene must name the actual product, the actual location, the actual person if applicable. Every line of voiceover must be writable as-is on the day of shoot. If you find yourself writing "something like" or "maybe show…", stop and commit to a single concrete direction.

The second-biggest failure mode is a weak hook. If the first 3 seconds are "Aaj hum baat karenge…" or "Namaste dosto…" or any equivalent slow setup, the reel is dead. Hooks have to interrupt the scroll — a visual, a number, a question, a contradiction, a sound — within the first second.

---

## Workflow

### Step 1 — Identify the client

The writer's opening message usually contains the client name: *"Create a reel script for Angoori Bliss"*, *"Script for Max & More"*, *"Reel for the diamond client"*. Extract the name and fuzzy-match it to a file in the `clients/` folder of the GitHub repo `Ghanshyam-Kanani-T/kalaa_client_details`. Filenames use underscores; the user uses spaces or partial names.

| User says | Filename |
|-----------|----------|
| `max and more` | `max_and_more_dhosa.md` |
| `Angoori Bliss` | `angoori_bliss.md` |
| `the diamond client` | (ambiguous — ask which one) |

List the directory first, then fetch the matched file. See [References → Fetching files from the repo](#references) for how. The repo is **public** — no auth needed. Any environment with web access (Claude web, Claude Code, Claude API) can fetch it directly. If the fetch fails (network error, file renamed), surface it cleanly — don't guess.

**If no match:**
1. Tell the writer the client wasn't found.
2. Ask for the client details using the [Client file template](#client-file-template) — business name, type, location, vibe, target audience, language preference, products/services, Instagram handle, anything to avoid.
3. Offer to commit the new file to the repo before continuing. Once saved, proceed to Step 2.

### Step 2 — Ask "this video" details

The client file already covers brand identity, audience, voice, and language preference — so you do **not** re-ask those. You only need to nail what's specific to *this one reel*.

Ask these **five short questions in one turn** — don't drip-feed. Use `AskUserQuestion` so the writer can pick options fast:

1. **What kind of reel?** Pick one. Note: each format implies a different VO style — see Step 2.5 Rule A.

   **A. Cinematic / B-roll-led** (sparse VO — hook + dialogue + closer only):
   - product demo · new item launch · offer · behind-the-scenes (BTS) · trending format · before-after · process · recipe (food clients)

   **B. Narrated / VO-led** (wall-to-wall voiceover is the format):
   - **brand anchor / hosting reel** — script-narrated over B-roll, "this is who we are / why we do this" storytelling
   - **founder / owner monologue** — owner talks to camera or VO over B-roll
   - **explainer / tutorial-narrated** — step-by-step narrated walkthrough
   - **customer journey / case-study narrated** — narrated story over visuals

   **C. Dialogue / talking-head** (people speak on camera, not VO):
   - customer testimonial · review · fun (skits, banter) · interview

   If the writer is ambiguous, ask: *"Is this a music-led visual reel or a narrated/hosted reel where a script runs wall-to-wall?"* — the answer determines the entire VO structure.
2. **Hero subject — what's the ONE thing this reel is about?** A specific product name ("Burj Khalifa Dosa"), a specific moment ("Father's Day family meal"), or a specific person (chef, owner). One thing, not three.
3. **What's the ONE key message the viewer should remember?** ("This dosa is 2 feet tall." / "We use fresh local ingredients." / "Open for Sunday brunch.") — this is what the closer hammers home.
4. **What action do you want the viewer to take?** Walk in · call to book · DM · save the reel · comment · follow. Be specific — "walk in this weekend" beats "visit us."
5. **Length?** Default 30 seconds. Options: 15s · 30s (recommended) · 45s · 60s.
6. **Do you have a specific scene flow / storyline in mind?** *(Critical — do NOT skip this.)* The writer often already has a mini-skit, customer-journey arc, or beat-by-beat narrative they want the reel to follow. Examples: *"customer comes in bored, sees menu, spots new dish, orders, gets served, praises it, closer line"* · *"chef intro → secret ingredient reveal → final plate → tagline"* · *"before-after: empty plate → full plate → satisfied customer"*. If they have one, capture **every beat in their words** and use that as the spine of the SCRIPT table — don't redesign the arc. If they don't have one, propose 2–3 short storyline options for them to pick from before generating.

**Optional 7th — reference reel:** If the writer mentions a specific viral reel they want this to feel like, ask for the link. It's gold for matching pacing and tone.

If the writer already gave any of these in the opening message ("script for Burj Khalifa Dosa, making + customer reaction, 30s"), don't re-ask those — only ask for the missing pieces. **But Q6 (scene flow) is almost always missing from one-liner briefs — ask it unless the writer explicitly described the storyline.**

### Step 2.5 — Language and VO discipline (hard rules)

These two rules existed because past scripts failed on them. They are non-negotiable.

**Rule A — Voiceover scope depends on the reel format.** There are two camps. Pick the right one based on the Step 2 reel-type answer.

---

**A.1 — Cinematic / B-roll-led reels** (product demo, BTS, offer, launch, recipe, before-after, trending format)

VO appears in **only three places**:

1. **Hook (0–3 sec)** — one short punchy line, max 6–8 words.
2. **On-camera dialogue** — customer reactions, owner one-liner, chef speaking direct-to-camera. These are *spoken on camera*, not added in post.
3. **Closer (last 3–5 sec)** — one short CTA line, max 6–8 words.

**Everything else** — the making phase, plating, cooking, B-roll, transitions, product reveals — is **music + SFX only**. NO voiceover layered over the cooking. The sizzle, the cheese pull, the knife on the cutting board, the dosa flipping — these are the audio. The music carries it. If a scene is mid-reel and not on-camera dialogue, write `VO: — (music + SFX only)` in that scene block.

This is the production rule for visual / cinematic reels. The Burj Khalifa reel v1 violated this — it had VO over the making scenes. For this camp, don't do that again.

---

**A.2 — Narrated / VO-led reels** (brand anchor / hosting, founder monologue, explainer, customer-journey narrative)

For this camp, the rule **inverts**. VO is the spine of the reel — a continuous narrated script runs from start to finish over B-roll, montage, or owner talking to camera. The visual is the *support* for the script, not the other way around.

How to structure these:

1. **Write the full VO script first, beginning to end** — as a continuous paragraph or scene-by-scene narration. This is what the speaker (owner / chef / hired voice) records.
2. **Then break it into scene blocks**, where each scene has a B-roll visual that *illustrates the line being narrated at that timestamp*.
3. **Music is a bed** — low (15–25%) under the VO so the speaker is always intelligible. SFX is minimal (occasional sting / accent only).
4. **Each scene block writes the VO line in full** (not `— music + SFX only`).
5. **Pace is calmer** — narrated reels usually need 45–60 seconds because the script needs room to land. 30s narrated reels feel rushed.

When to pick A.2 over A.1:
- The point of the reel is the *story or message*, not the product itself.
- The client wants the founder / owner / brand voice front-and-center.
- The reel is being used as the brand's "hero" / pinned reel / about-us anchor.
- Examples: *"Why we started Max & More" · "What goes into every dosa" · "Our story, in 45 seconds"*

If the writer asks for a **brand anchor** or **hosting reel** for the client, this is the camp to use. The Max & More brand-anchor reel format the writer mentioned belongs here.

---

**A.3 — Dialogue / talking-head reels** (customer testimonial, review, interview, fun banter)

Speakers are on camera and the audio is *their actual voice during the shoot* — not VO added later. Each scene block writes the spoken line as `Dialogue: (on-camera)` not `VO:`. Music bed sits low under dialogue (15%). No VO layer on top.

---

**Which camp does this reel belong to?** Decide before writing any scenes. If unsure, ask the writer. Writing a cinematic-format script with wall-to-wall VO, or a narrated-format script with silent scenes, is the most common failure — both feel wrong on camera.

**Rule B — Language quality.** If the client speaks Gujarati to their audience, you do NOT mix Hindi words into the Gujarati lines (no *"Aaj"*, *"Pehla"*, *"Dikhayenge"* — those are Hindi). Two safe options:

1. **Write the line cleanly in the target language** — use real Gujarati words (`Aaje` not `Aaj`, `Pratham` or `Pehlu` not `Pehla`, `Batavishu` not `Dikhayenge`). If you're not 100% confident the Gujarati phrasing is natural, use Option 2.
2. **Write the VO line as a brief in English** — e.g., `VO brief: "Two-feet dosa? Yes, in Surat — say it with shock, like you can't believe it yourself."` — and let the chef/owner say it in their natural Gujarati. This is often better because the speaker sounds authentic instead of reading a translated script.

For on-screen text (overlay graphics), always confirm the exact Gujarati/Hindi/English copy. The designer will set this in graphics, so it must be word-perfect.

When in doubt: **VO brief in English, on-screen text in target language.** This avoids the Hindi-Gujarati-mush problem.

### Step 3 — Research

Run web searches in **parallel** to ground the script in what's actually working right now. The searches you should always run:

- **Trending reel formats for this niche this year** — e.g., `"trending jewellery reels Instagram 2026"`, `"viral restaurant reels India"`. Look for *format* trends (POV, transition, slow-mo reveal, voiceover-on-text, talking-head) not just topic.
- **Trending hooks for this reel type** — e.g., `"BTS reel hooks restaurant"`, `"product demo hooks Instagram 2026"`. You're hunting for opener patterns currently outperforming the average.
- **Trending audio for this niche** on Instagram right now. Audio names change weekly — be honest about freshness (see audio rules below).
- **Competitor reels in the niche** — what local or comparable brands are posting, and which of their reels are getting outsized reach.
- **Current viral patterns** — transitions, text-on-screen styles, pacing trends, sound design tricks that are over-indexing this quarter.

Don't over-research. 3–6 well-targeted searches are better than 12 vague ones. The goal is enough signal to make confident choices, not a literature review.

### Step 4 — Generate the script

**Default output is LEAN — four sections only:**

1. **SCRIPT** — scene-by-scene shot table (every scene from hook to final CTA frame)
2. **AUDIO SUGGESTION**
3. **CAPTION**
4. **VOICEOVER / DIALOGUE LINES** — every spoken line in the reel listed separately with its timestamp, Gujarati (or target-language) line, and English translation on their own rows. This is in addition to the lines already inside the script table — the user wants them broken out so the speaker / VO artist can read them clean without scanning a table.

Nothing else by default. No brand voice confirmation. No 3-hook-option block. No separate "reel overview" / "on-screen text summary" / "visual direction summary" / "shot sheet table" duplicated under the script. No delivery notes. No 2 trial hooks. The user explicitly asked for the lean format — anything beyond these four sections is bloat.

If the user asks for extras (*"add 3 hook options"*, *"give me delivery notes too"*, *"add A/B trial hooks"*), add only what they asked for, in addition to the four default sections. Don't volunteer them.

**Length discipline:** Don't cram. A 30s reel realistically fits a hook + 3–5 scenes + closer. A 60s reel fits a hook + 5–8 scenes + closer. If you find yourself writing 10 scenes for 30s, cut.

#### Section 1 — SCRIPT (scene-by-scene table)

One table, one row per scene, in order from hook to final CTA frame. Use exactly these columns:

| # | Time | Scene | Visual | Dialogue/VO | On-Screen Text | Audio/SFX | Transition |

Rules for the table:
- `Time` — timecode range (e.g., `0:00–0:03`).
- `Scene` — short label (e.g., "Hook — bored customer", "Tower build", "Bite + reaction", "Closer/CTA").
- `Visual` — one tight phrase: camera + subject + key action. Concrete, not abstract. Name the actual product / person / location.
- `Dialogue/VO` — exact line in the client's language (or VO brief in English per Step 2.5 Rule B). Use `—` if silent. Mark on-camera lines as `Dialogue (on-camera):` to distinguish from `VO:`.
- `On-Screen Text` — exact overlay text for that scene, word-perfect in the target language. Use `—` if none.
- `Audio/SFX` — scene-level audio direction: "sizzle SFX, music build", "music bed at 25% under dialogue", "music swell + boom sting". This is per-scene; the broader audio suggestion goes in Section 2.
- `Transition` — exit transition into the next scene (cut · whip-pan · match-cut · dissolve · zoom · morph · none). Last row's transition is `—`.

Apply the right VO camp (Step 2.5 Rule A) inside the table:
- **A.1 cinematic:** making/plating/B-roll rows show `—` in Dialogue/VO. VO only in hook row, on-camera dialogue rows, closer row.
- **A.2 narrated:** every row writes its VO line in full.
- **A.3 dialogue:** every row writes on-camera spoken line; no VO layer.

Keep the cells terse. If a row needs more than a phrase, the script is over-engineered — cut detail, not scenes.

#### Section 2 — AUDIO SUGGESTION

Short. 2–4 lines max.
- **Primary:** A specific trending audio if research surfaced one — name it, note why it fits.
- **Fallback:** If no real audio name surfaced, describe the type + mood concretely so the editor can search it: "upbeat Gujarati dhol track with beat drop at 3s", "lo-fi cooking ASMR", "slow emotional piano". 
- **Audio rule:** Never invent audio names. Real, vague, or absent — those are the only honest options.

#### Section 3 — CAPTION

The caption is **not** a blog post. Max 4 lines + exactly 5 hashtags.

- **Structure (max 4 lines):**
  1. **Hook line** — short line in the client's language that mirrors the reel's hook or USP.
  2. **Body** — one line of context (optional — skip if the hook is enough).
  3. **CTA** — one specific action: "Call to book", "Walk in this Sunday", "DM to reserve".
  4. **Mention** — `@<client_handle>` from the client file.

- **Hashtags: exactly 5.** Mix: 1–2 niche · 1–2 broader/trending · 1 location.
- **Emoji discipline:** Max 2–3 emojis in the entire caption.
- **Language:** same as the reel's spoken language. Gujarati reel → Gujarati / Gujlish caption.

#### Section 4 — VOICEOVER / DIALOGUE LINES

A clean separate list of every spoken line (VO + on-camera dialogue) in the reel, in shooting order. The point is: the speaker / VO artist should be able to read this section alone — without scanning the script table — and know exactly what to say.

For each spoken line, write a block in this format:

```
**[Time range]** — [Scene label]
- Gujarati (or target language): "<exact line in the script's language>"
- English: "<clean English translation>"
```

Rules:
- One block per spoken line. If a scene has no dialogue/VO, skip it here.
- Time range matches the script-table row for that line.
- For VO lines (off-camera), label as `VO`. For on-camera lines, label as `Dialogue (on-camera)` — same convention as inside the table.
- If the line is a brief in English (i.e., the speaker should say it in their natural Gujarati), put the brief on the English row and put the suggested Gujarati phrasing on the Gujarati row with a `(suggested — speak naturally)` note.
- Gujarati lines: write in Gujarati script (e.g., "વાહ! મજા આવી ગઈ!") **and** romanized Gujarati in parentheses for the speaker who reads Latin script (e.g., `"વાહ! મજા આવી ગઈ!" (Vaah! Maja aavi gayi!)`). Both versions on the same row.

This section is required even when there are only 1–2 spoken lines in the reel.

---

#### Optional extras (only when the user asks for them)

If the user requests any of these, append after the three default sections — never volunteer them:

- **3 hook options ranked** — Options A/B/C with visual + line + on-screen text + why-it-works each.
- **2 trial hooks for A/B testing** — alternate first-3-second variants shootable in the same session.
- **Delivery notes** — for each spoken line: emotion + pace + emphasis + pause.
- **Brand voice confirmation** — 3–4 line snapshot of vibe / language / tone / signature moment.
- **Visual direction summary** — shot list, lighting, props, location, separately from the scene table.

### Step 5 — Self-check before delivering

Run through this list silently before sending:

- [ ] Output is **only the four default sections** (script table + audio + caption + VO/dialogue lines) — no brand voice confirmation, hook-option block, delivery notes, or trial hooks unless the user asked for them.
- [ ] **Section 4 (VO/Dialogue lines)** is present and includes every spoken line from the table, in shooting order, with Gujarati (script + romanized) and English on separate rows.
- [ ] Hook (first row of the table) is in the first 3 seconds and doesn't open with a generic greeting (no *"Aaj"*, *"Namaste dosto"*).
- [ ] **VO rule matches the reel's camp** (A.1 cinematic / A.2 narrated / A.3 dialogue) per Step 2.5 Rule A. Cinematic = silent making rows. Narrated = wall-to-wall VO. Dialogue = on-camera spoken, no VO layer.
- [ ] **Language rule:** No Hindi loanwords sneaking into Gujarati lines. If unsure, VO is written as an English brief, not a fake-Gujarati line. On-screen text is word-perfect in the target language.
- [ ] Every scene names a *specific* product, person, or moment (not "the product", "a customer").
- [ ] Total duration matches the requested length within ±5 sec.
- [ ] No scene is under 1.5s (too short to register) or over 5s (too long for reel pacing unless it's a slow-emotional piece).
- [ ] Audio suggestion is real-or-honest, never invented.
- [ ] No conflict with the client's `Topics to avoid`.
- [ ] Brand vibe matches: a "family restaurant" doesn't get nightclub energy; a "premium jewellery" client doesn't get meme treatment.
- [ ] The CTA tells the viewer exactly what to do next.
- [ ] **Caption is short (max 4 lines)** with **exactly 5 hashtags**, in the same language as the script.

If any check fails, fix it before delivering.

---

## Things to actively avoid

- **Generic hooks.** "Aaj hum dikhayenge…", "Namaste dosto…", "Have you ever tried…" — all dead on arrival. Hook with an image, a number, a contradiction, a question with stakes, or a sound.
- **VO style mismatched to the reel's camp.** For a cinematic / B-roll-led reel, wall-to-wall VO kills the feel — making phase = music + SFX only. For a narrated / hosted brand-anchor reel, the *opposite* applies: VO is the spine, silent scenes feel empty. Pick the camp first (Step 2 → Step 2.5 Rule A) and apply the matching rule.
- **Hindi loanwords in Gujarati lines.** *"Aaj"*, *"Pehla"*, *"Dikhayenge"*, *"Hum"* are Hindi. *"Aaje"*, *"Pratham/Pehlu"*, *"Batavishu"*, *"Ame"* are Gujarati. If you're not sure, write the VO as an English brief and let the client's chef/owner say it in their natural Gujarati. (See Step 2.5 Rule B.)
- **Ambiguous direction.** "Show something nice" is not a direction. Commit to a shot.
- **Made-up audio.** If you didn't find a real trending audio in research, describe the vibe — don't invent a name like "Trending Gujarati Beat #42."
- **Brand-mismatched energy.** A premium-jewellery client doesn't get a meme-trending script even if the format is hot.
- **Cramming.** 30s ≠ 10 scenes. Pacing matters more than scene count.
- **English by default.** If the client speaks Gujarati to their audience, the voiceover and caption are in Gujarati (or Gujlish), not English.
- **Vague CTAs.** "Visit us" is not a CTA. "Walk in at Katargam between 7–10pm" is.
- **Caption bloat.** Captions longer than 4 lines or with more than 5 hashtags get scrolled past. Keep it tight.

---

## References

### Client file template

When creating a new client file (Step 1, no-match path), use this structure — it matches existing files in the repo:

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

If the fetch fails (network error, file genuinely missing, repo moved), tell the writer plainly — don't proceed with guesses.
