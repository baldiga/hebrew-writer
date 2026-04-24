# Hebrew Writer v3.1 — Fusion Engine Rebuild Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild the voice fusion system in v3.1 so it actually captures a user's tone, structure, and key tells — moving from static feature extraction (which hits a ceiling) to iterative refinement, contrastive learning, and outlier-focused signature extraction.

**Architecture:** Three high-leverage mechanisms replace v3's single-pass profile extraction: (1) a Key Tells extractor that identifies the 3-5 most statistically unusual writer behaviors and treats them as priority generation constraints, (2) a style-extreme passage selector that picks signature passages for stylistic outlierness rather than content similarity, (3) an iterative `--calibrate` loop (PROSE-style) where the skill generates test samples and updates the profile based on user feedback. Two supporting mechanisms add contrastive richness: a negative example field in `--setup` (what feels wrong to the user) and a `--setup-deep` 10-question voice interview for users who want maximum fidelity. Everything else in v3 stays unchanged — Layers 1-6 and 8, the Big No-Nos, content presets, and generation pipeline are preserved.

**Tech Stack:** Markdown (SKILL.md), Claude Code skill framework, standard allowed tools (Read, Write, Edit, Grep, Glob, AskUserQuestion). Prompt engineering only — no fine-tuning, no external APIs.

**Spec basis:** Research synthesis from EMNLP 2025 "Catch Me If You Can?" (Wang et al.), Apple ICML 2025 PROSE, RG-Contrastive 2025, PerFine 2025, AlphaWrite 2025, Writer.com dual-LLM architecture, and stylometric-transfer fingerprinting.

**Research document:** Referenced from the fusion research agent output (see conversation history).

---

## File Structure

| File | Responsibility |
|------|---------------|
| `SKILL-v3.1.md` | v3 base + 5 new fusion mechanisms integrated into Layer 7 |
| `voice-profile-template-v3.1.md` | Updated profile template with Key Tells, negative examples, calibration history |
| `~/.claude/skills/hebrew-writer-v3-1/SKILL.md` | Installed copy (copied from SKILL-v3.1.md) |
| `~/.claude/skills/hebrew-writer-v3-1/voice-profile-template.md` | Installed copy |

**Important:** v3.1 replaces v3 as the active version. v3 stays in the repo as `SKILL-v3.md` for reference but is no longer the primary version. Neither v2 nor v3 files are deleted — only v3.1 becomes the new primary.

---

## Chunk 1: Foundation — Create v3.1 Base and Update Frontmatter

### Task 1: Copy v3 as the starting point for v3.1

**Files:**
- Create: `SKILL-v3.1.md` (copy of `SKILL-v3.md`)

- [ ] **Step 1: Copy v3 to v3.1**

Run:
```bash
cp "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.md" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

Expected: File copied. Verify with `wc -l SKILL-v3.1.md` — should show 2,896 lines.

- [ ] **Step 2: Update the frontmatter description**

Use Edit to replace lines 4-15 in `SKILL-v3.1.md`. Replace:
```
  v3 — Write Hebrew content indistinguishable from a native Israeli human.
  Generates, rewrites, or detects AI patterns in Hebrew text.
  8-layer system with enhanced voice profiling: Hebrew-first thinking,
  55+ AI pattern detection, Israeli voice injection, linguistic precision,
  rhythm engineering, self-audit scoring (95/100 threshold), enhanced
  voice cloning (42-feature extraction + signature passage anchoring),
  and Soul Layer (נשמה עמוקה).
  v3 adds: passive onboarding (--setup), 42 stylometric features,
  few-shot signature passages, smart fusion engine.
  Data-grounded in 742K words of Israeli podcast transcripts (ivrit.ai),
  40+ academic sources, and stylometric-transfer research (EMNLP 2024-2025).
```

With:
```
  v3.1 — Write Hebrew content indistinguishable from a native Israeli human.
  Generates, rewrites, or detects AI patterns in Hebrew text.
  8-layer system with iterative voice fusion: Hebrew-first thinking,
  55+ AI pattern detection, Israeli voice injection, linguistic precision,
  rhythm engineering, self-audit scoring (95/100 threshold), adaptive
  voice cloning (Key Tells + style-extreme passages + calibration loop),
  and Soul Layer (נשמה עמוקה).
  v3.1 adds: Key Tells extraction (3-5 outlier behaviors), style-extreme
  passage selection, --calibrate iterative refinement, negative examples,
  --setup-deep 10-question voice interview.
  Grounded in EMNLP 2025 research proving feature tables hit a ceiling
  but iterative refinement + outlier focus breaks through.
```

- [ ] **Step 3: Update the argument-hint to add new flags**

Find the argument-hint line (line 19). Replace the current value with:
```
argument-hint: '"topic or text" [--mode generate|rewrite|detect] [--setup] [--setup-deep] [--calibrate] [--type blog|academic|social|business|email|creative|auto] [--length short|medium|long|xl|NUMBER] [--gender male|female|neutral] [--voice profile-name] [--my-voice "sample text"] [--my-voice-file path] [--learn "text" --save-as name] [--show-score]'
```

Added: `--setup-deep` and `--calibrate`.

- [ ] **Step 4: Commit**

```bash
cd "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill"
git add SKILL-v3.1.md
git commit -m "feat: v3.1 base — copy v3 and update frontmatter for fusion rebuild"
```

---

### Task 2: Add "What's New in v3.1" section

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the insertion point**

Use Grep to find the end of the "What's New in v3" section (look for the line `---` that precedes `# Hebrew Writer — הכותב העברי`).

Run:
```bash
grep -n "^# Hebrew Writer" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

Expected: One line number output. The line immediately before should be `---`.

- [ ] **Step 2: Insert the v3.1 section**

Use Edit to replace the `---` separator before `# Hebrew Writer — הכותב העברי` with the following block (which includes the separator, the new section, and another separator):

```
---

# What's New in v3.1

v3 built the 42-feature voice system. Real-world use proved it hit a ceiling — the profile captured what could be described but missed the implicit, unconscious patterns that make a writer feel like themselves. v3.1 is the fusion rebuild based on EMNLP 2025 research showing that feature tables systematically underperform iterative refinement and outlier-focused signals.

**New in v3.1:**

- **Key Tells extraction** — Instead of averaging 42 features, the skill identifies the 3-5 most statistically unusual behaviors this writer exhibits (things that deviate most from Israeli Hebrew baseline norms). These become priority generation constraints, enforced before anything else.
- **Style-extreme passage selection** — Signature passages are now selected for stylistic outlierness, not content representativeness. The passages most different from generic Israeli Hebrew are chosen, because they carry the strongest voice signal.
- **--calibrate iterative refinement** — Apple's PROSE loop adapted for Hebrew. After the initial profile, the skill generates 2 sample paragraphs, the user picks which sounds more like them and says what's wrong with the other, and the profile is updated based on the delta. Converges in 2-3 rounds.
- **Negative examples in --setup** — The --setup flow now asks for a "this feels wrong / flat / not me" sample alongside the positive sample. Contrastive analysis between the two extracts differential features invisible in single-sample analysis.
- **--setup-deep 10-question voice interview** — Optional deeper onboarding that surfaces implicit preferences via behavioral questions (e.g., "when you're excited about an idea, how does your punctuation change?") that extract information no passive sample analysis can reveal.
- **Research basis** — EMNLP 2025 "Catch Me If You Can?" (Wang et al.), Apple ICML 2025 PROSE, RG-Contrastive 2025, PerFine 2025 knockout strategy, Writer.com dual-LLM architecture, Stanford 20-questions personalization.

---
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — add 'What's New' section explaining fusion rebuild"
```

---

## Chunk 2: Argument Parsing and Mode Routing Updates

### Task 3: Add new flags to argument parsing

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Locate the flag extraction section**

Run:
```bash
grep -n "^--setup" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

Expected: One line showing the existing `--setup` entry in the code block.

- [ ] **Step 2: Update the flag extraction block**

Use Edit to find this text:
```
--setup                              Triggers onboarding flow (no value)
```

Replace with:
```
--setup                              Triggers basic onboarding flow (no value)
--setup-deep                         Triggers deep onboarding with 10-question interview (no value)
--calibrate                          Triggers iterative profile refinement (requires existing profile, no value)
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — add --setup-deep and --calibrate flags"
```

---

### Task 4: Update Mode Routing for new modes

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the Mode Routing section**

Use Grep to locate the line:
```
- `--setup` → Jump to **Onboarding Flow** (overrides --mode)
```

- [ ] **Step 2: Replace that single line with three routing lines**

Replace:
```
- `--setup` → Jump to **Onboarding Flow** (overrides --mode)
```

With:
```
- `--setup` → Jump to **Basic Onboarding Flow** (overrides --mode)
- `--setup-deep` → Jump to **Deep Onboarding Flow — 10-Question Voice Interview** (overrides --mode)
- `--calibrate` → Jump to **Calibration Loop** (overrides --mode; requires existing voice profile)
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — add mode routing for --setup-deep and --calibrate"
```

---

## Chunk 3: Layer 7 Rewrite — The Fusion Engine Rebuild

This is the biggest chunk. Layer 7 is being substantially restructured to integrate the 5 new mechanisms. The old v3 Layer 7 structure becomes the foundation, but Key Tells, style-extreme selection, negative examples, calibration, and deep setup are woven in.

### Task 5: Add Key Tells extraction methodology

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the "42-Feature Voice Extraction" section**

Run:
```bash
grep -n "^## 42-Feature Voice Extraction" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

- [ ] **Step 2: Insert Key Tells section BEFORE the 42-feature section**

Use Edit to find the text:
```
## 42-Feature Voice Extraction
```

Replace with:
```
## Key Tells — The 3-5 Outlier Behaviors

Before extracting 42 features, the skill identifies the **Key Tells**: the 3-5 behaviors that deviate MOST from Israeli Hebrew baseline norms. These are the fingerprint — the things a reader would use to pick this writer's text out of a lineup.

### Why Key Tells come first

Research on authorship attribution (CMU 2024, EMNLP 2025) consistently shows that style signal is concentrated in outlier behaviors, not averages. A writer who uses 11-word sentences on average has a weak signal. A writer who never uses formal connectors (לפיכך, על כן) — that absence is a strong signal. A writer who opens every argument with a question they immediately dismiss — that pattern is a fingerprint.

The 42-feature table describes. The Key Tells constrain. During generation, Key Tells get priority enforcement — they are non-negotiable.

### Baseline reference points (Israeli Hebrew norms from ivrit.ai + HeBERT data)

When analyzing a sample, compare these metrics to baseline:

| Feature | Israeli Hebrew baseline | "Deviates" means |
|---------|-------------------------|------------------|
| Avg sentence length | 8-13 words (8.9 written, 13.2 spoken) | <6 or >18 |
| נו usage | 3.77% of tokens in casual | 0% (never) or >6% (heavy) |
| אז as opener | ~4.5% of segment openers | 0% or >12% |
| Ellipsis (...) | 21-41% of chunks | <5% or >60% |
| English code-switching | 5-10% of segments | 0% or >20% |
| Discourse markers total | 4-7% of tokens | <2% or >12% |
| Formal connectors (לפיכך etc.) | ~1% in casual | 0% or >3% |
| Construct state ratio | 30-50% in mixed | <15% or >70% |
| Self-correction frequency | ~10% of segments | 0% or >25% |
| Questions per 500 words | 0.5-2 | 0 or >5 |

### Key Tells extraction protocol

After reading the user's samples, ask: "For each of the 42 features, how different is this writer from baseline?" Rank features by deviation magnitude. Select the top 3-5 MOST deviating features. For each, write a specific Key Tell statement.

**Key Tell statement format:** `[Specific behavior]. [Baseline comparison]. [How to enforce in generation].`

**Example Key Tells:**

**KT1:** This writer NEVER uses formal connectors (לפיכך, על כן, אי לכך, בשל כך). Baseline: ~1% casual. Writer: 0/47 occurrences. Enforce: Replace all formal connector suggestions with אז, כי, or bare sentence break.

**KT2:** This writer uses ellipsis (...) at 3x baseline — every 80-100 words. Baseline: every 300-400 words. Enforce: Insert at least 2 "..." per 200 words, specifically at trailing thoughts or implied continuations.

**KT3:** This writer opens arguments with a question she immediately dismisses. Pattern: "[Question]? [Dismissal]." appears 5 times in the sample. Enforce: Use this pattern at least once per piece over 300 words.

**KT4:** This writer never uses "חשוב" (important) — instead uses "שווה" (worth it) or just states the claim. 0 occurrences of חשוב in the sample. Enforce: Blacklist חשוב for this voice; substitute שווה or direct assertion.

**KT5:** This writer ends pieces without a conclusion — just stops. Baseline: ~30% of pieces have explicit conclusions. Writer: 0 of 8 samples. Enforce: Do not write a conclusion paragraph. End on the last substantive point.

### Where Key Tells live in the profile

Key Tells get their own section at the TOP of the voice profile file, before the 42-feature table. They are the first thing read during generation, and the last thing checked in self-audit.
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — add Key Tells extraction methodology (3-5 outlier behaviors)"
```

---

### Task 6: Update signature passage selection to style-extreme criteria

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the "Signature Passage Extraction" section**

Run:
```bash
grep -n "^## Signature Passage Extraction" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

- [ ] **Step 2: Replace the entire section with the style-extreme version**

Use Edit to find:
```
## Signature Passage Extraction

After analyzing the 42 features, extract 5-10 passages from the user's writing samples that BEST represent their distinctive voice.

**Selection criteria (in order of priority):**

1. **Voice-dense passages** — Sentences that contain multiple characteristic features at once (their typical slang + their sentence rhythm + their humor style in one passage)
2. **Pattern-breaking moments** — Passages where the writer does something AI would never do: a self-correction, a tangent, a fragment after a long sentence, an abrupt opinion
3. **Emotionally loaded passages** — Where the writer shows their emotional temperature most clearly
4. **Distinctive openings** — How they start paragraphs or sections (their natural opener style)
5. **Hebrew-specific markers** — Passages with their characteristic use of דווקא, construct state, discourse markers, cultural references

**Passage format:** Each passage is 15-40 words, extracted verbatim from the user's samples with a one-line label describing what it demonstrates.
```

Replace with:
```
## Signature Passage Extraction (Style-Extreme Selection)

**The v3.1 change:** v3 selected passages that "best represent" the writer's voice — which in practice meant choosing representative, average-voice passages. v3.1 selects for stylistic outlierness. The passages chosen are the ones where this writer's voice is MOST different from generic Israeli Hebrew. Research basis: EMNLP 2025 showed that content-similar exemplars reduce style imitation accuracy; style-extreme exemplars increase it.

### Style-extreme selection criteria (ranked)

1. **Maximum Key Tell concentration** — Passages that demonstrate 2+ Key Tells at once. A single passage showing both "never uses formal connectors" and "ends abruptly without conclusion" is worth more than two passages each showing one tell.

2. **Most different from baseline** — For each candidate passage, compare to generic Israeli Hebrew. The passages where the writer's choices diverge most from baseline get priority. "Most them" = "most different from average."

3. **Outlier emotional moments** — Passages where the writer's emotional temperature spikes (excitement, frustration, confession). These reveal the writer's voice under load, which is when implicit patterns are most visible.

4. **Pattern-breaking choices** — Passages with a sudden register shift, a mid-sentence correction, an abrupt fragment, a parenthetical that goes somewhere unexpected. These show the writer doing what AI cannot.

5. **Self-reference passages** — Passages where the writer refers to themselves, their experience, or their doubt. These carry vulnerability signals that anchor "who is this person."

### Passage format

Each passage is 15-40 words, extracted verbatim. Each passage gets three labels:

```
### SP1: [rhythm label] + [content label] + [Key Tell demonstrated]
> "[verbatim passage]"
— demonstrates: [what this shows about the writer's voice, specifically]
— enforces: [Key Tell number(s) this passage anchors]
```

### Selection quantity

- Basic tier (200-500 words sample): 3-5 passages
- Strong tier (500-1500 words): 5-7 passages
- Full Clone (1500+): 7-10 passages
- Maximum (multi-file): 10-15 passages

Beyond 10 passages, returns diminish. EMNLP 2025 data confirms: 5 well-selected passages beat 20 averagely-selected ones.

### What NOT to select

- Passages that are average in voice (they teach the model nothing specific)
- Passages that are topically similar to what the user will be writing about (they become content references, not style references)
- Passages where the writer is quoting someone else or being intentionally formal
- Passages that any competent Hebrew writer could have written (not distinctive)
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — replace signature passage selection with style-extreme criteria"
```

---

### Task 7: Replace the Basic Onboarding flow with the v3.1 version (adds negative example)

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find "The --setup Onboarding Flow" section**

Run:
```bash
grep -n "^## The --setup Onboarding Flow" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

- [ ] **Step 2: Replace the entire section**

Use Edit to find:
```
## The --setup Onboarding Flow

When user runs `/hebrew-writer --setup`:

**Step 1:** Ask using AskUserQuestion:
> "הדבק 2-3 טקסטים שכתבת (פוסטים, אימיילים, מאמרים, הודעות — כל דבר שנשמע כמוך). מינימום 500 מילים סה״כ. יותר = יותר מדויק."

**Step 2:** After receiving samples, ask:
> "מה המגדר שלך לנטיית פעלים? (male / female / neutral)"

**Step 3:** Ask:
> "איזה סוג תוכן אתה בעיקר כותב? (blog / social / business / mixed)"

**Step 4:** Run the 42-Feature Extraction on the samples (see below).

**Step 5:** Extract 5-10 Signature Passages from the samples (see below).

**Step 6:** Save everything as `.claude/voices/default.hebrew-voice.md`. Create the directory if needed.

**Step 7:** Confirm:
> "פרופיל הקול שלך נשמר! מעכשיו כל מה שאכתוב ישמע כמוך.
> Accuracy tier: [tier] ([word count] words analyzed)
> לשינוי: /hebrew-writer --setup"
```

Replace with:
```
## The --setup Basic Onboarding Flow

When user runs `/hebrew-writer --setup`:

**Step 1 — Positive sample:** Ask using AskUserQuestion:
> "הדבק 2-3 טקסטים שכתבת (פוסטים, אימיילים, מאמרים, הודעות — כל דבר שנשמע כמוך). מינימום 500 מילים סה״כ. יותר = יותר מדויק."

**Step 2 — Negative sample (NEW in v3.1):** Ask:
> "(אופציונלי) יש לך דוגמה של כתיבה שלך שהרגישה לא ממך — שטוחה, פורמלית מדי, או סתם לא מרגישה כמוך? הדבק פה. דלג עם 'דלג'."

**Why this question matters:** Research (RG-Contrastive 2025) shows that contrastive examples — knowing what's wrong — extracts implicit preferences better than positive examples alone. If the user provides a negative sample, we learn what voice this writer is NOT.

**Step 3 — Gender:** Ask:
> "מה המגדר שלך לנטיית פעלים? (male / female / neutral)"

**Step 4 — Primary content type:** Ask:
> "איזה סוג תוכן אתה בעיקר כותב? (blog / social / business / mixed)"

**Step 5 — Analysis:**
- Run the 42-Feature Extraction on the positive samples
- Extract the Key Tells (top 3-5 most outlier behaviors)
- Select Signature Passages using style-extreme criteria (3-7 passages depending on sample size)
- If negative sample was provided: run Contrastive Analysis (see below) and add Differential Features section to profile

**Step 6 — Save:** Write everything to `.claude/voices/default.hebrew-voice.md`. Create the directory if needed. Use the v3.1 profile structure (Key Tells at top, then 42 features, then passages, then differentials).

**Step 7 — Confirm:**
> "פרופיל הקול שלך נשמר! מעכשיו כל מה שאכתוב ישמע כמוך.
> Accuracy tier: [tier] ([word count] words analyzed)
> Key Tells found: [N]
> Signature passages: [N]
> [If negative sample provided:] Differential features extracted: [N]
>
> רוצה לשפר את ההתאמה? הרץ /hebrew-writer --calibrate כדי שאייצר דוגמאות ותגיד לי אילו הכי נשמעות כמוך.
> לשינוי: /hebrew-writer --setup"

## Contrastive Analysis (from negative sample)

When a negative sample is provided, run this analysis:

**Step A:** For each of the 42 features, measure the feature value in BOTH the positive and negative samples.

**Step B:** Identify features where positive and negative differ most — these are "Differential Features" — the things that are present in the writer's real voice but missing from their "wrong" voice.

**Step C:** Record 3-5 Differential Features in the profile with this format:
```
### DF1: [feature name]
- Positive sample: [value]
- Negative sample: [value]
- Delta: [what makes the positive "more them"]
- Enforcement: [how to apply in generation]
```

**Example:**
```
### DF1: Self-correction frequency
- Positive sample: 3 self-corrections per 500 words
- Negative sample: 0 self-corrections
- Delta: When this writer sounds like themselves, they correct themselves mid-thought. The flat version skips this.
- Enforcement: Insert at least 1 visible self-correction per piece of 300+ words. "רגע" / "בעצם" / "לא, זה לא מדויק".
```

Differential Features get enforced with the same priority as Key Tells.
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — basic onboarding now asks for negative sample (contrastive analysis)"
```

---

### Task 8: Add the Deep Onboarding flow (10-question interview)

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the end of the Basic Onboarding section**

After the Contrastive Analysis subsection you just added, find the next `---` separator.

- [ ] **Step 2: Insert the Deep Onboarding section before that separator**

Use Edit to add BEFORE the `---` that ends the basic onboarding area:

```

---

## The --setup-deep Flow — 10-Question Voice Interview

For users who want maximum fidelity. Takes 5-10 minutes. Surfaces information that passive sample analysis can't extract.

Research basis: Stanford's 20-Questions personalization (2024) showed that targeted behavioral questions capture preference data far more effectively than self-description. Users cannot describe their own writing style abstractly but can accurately answer behavioral questions.

### The 10 questions

When user runs `/hebrew-writer --setup-deep`, ask each question in sequence using AskUserQuestion. After all 10, run the standard sample collection (steps 1-5 from Basic Onboarding) and synthesize both sources into the profile.

**Q1 — Peak voice anchor:**
> "הראה לי את הדבר האהוב עליך שכתבת — משפט או פסקה שהרגיש לך הכי 'אתה'. (Paste here)"

**Q2 — Negative anchor:**
> "הראה לי משהו שכתבת שהרגיש לא נכון / פורמלי מדי / שטוח. (Paste here, or type 'דלג')"

**Q3 — Arousal punctuation:**
> "כשאתה מתלהב מרעיון — איך משתנה הפיסוק שלך? (more exclamations? more dashes? fragments? longer sentences? other?)"

**Q4 — Argument direction:**
> "אתה נוטה להתחיל עם המסקנה ולהסביר אחריה, או לבנות אליה בהדרגה? (conclusion-first / build-up / story-first / question-first)"

**Q5 — Disagreement handling:**
> "כשאתה לא מסכים עם משהו — אתה מתמודד חזיתית, הולך סביב, או פשוט אומר את דעתך בלי להתייחס לצד השני? (head-on / sideways / bypass)"

**Q6 — Discourse marker preference:**
> "מאלה, איזה אתה הכי משתמש: אז / אבל / בעצם / נו / כאילו / תכל'ס? בחר את 3 הראשונים בסדר שימוש."

**Q7 — Ending style:**
> "איך אתה מסיים כתיבה? (explicit conclusion / call to action / reflective thought / just stop / question)"

**Q8 — Parenthetical habit:**
> "כמה אתה משתמש בסוגריים או מקפים להוספת הערות בצד? (never / rare / moderate / heavy — part of my rhythm)"

**Q9 — Uncertainty handling:**
> "כשאתה לא בטוח במשהו — אתה אומר שאתה לא בטוח, או פשוט מצהיר ועובר הלאה? (I hedge openly / I state it and move / mix)"

**Q10 — External reference:**
> "יש מישהו שאמרו לך שהכתיבה שלך נשמעת כמוהו? (specific person, TV character, podcast host, author, 'כמו אני מדבר עם חברים', etc. — free text)"

### Synthesizing the answers into the profile

After Q1-Q10 + the standard sample collection, combine both sources:

1. **Build the 42-feature table** from the samples (as in Basic Onboarding)
2. **Extract Key Tells** from the samples
3. **Select style-extreme passages** from the samples
4. **ADD: Behavioral Profile section** — synthesize the 10 Q&A responses into a behavioral narrative:

```
## Behavioral Profile (from --setup-deep interview)

### Peak voice
Writer's self-identified "most them" moment: [quote from Q1]
This is the target voice — everything should feel like it could be from this peak.

### Anti-voice (what they're fleeing from)
[From Q2, if provided]
Writer's self-identified "not me" zone. Avoid these patterns.

### Arousal rhythm
When emotionally engaged, this writer's style shifts: [from Q3]
Enforcement: [specific adjustments in emotionally-loaded content]

### Argument architecture
Default direction: [from Q4]
Enforcement: When writing an opinion piece or argument, structure accordingly.

### Disagreement pattern
[From Q5]
Enforcement: When content involves disagreement/opposing view, use this exact pattern.

### Preferred discourse markers (behavioral, not measured)
Top 3: [from Q6, in order]
Enforcement: These take priority over whatever the sample analysis found. Self-report is valid here.

### Ending behavior
[From Q7]
Enforcement: Match this ending style explicitly.

### Parenthetical/aside density
Self-reported level: [from Q8]
Enforcement: Calibrate aside frequency to match.

### Uncertainty expression
[From Q9]
Enforcement: Hedge or commit based on this preference.

### Voice reference
[From Q10]
Use this as an external anchor when generating. Ask: "Would this sound like [reference]?"
```

5. **Resolve conflicts between sources:** When the sample analysis and the interview answers disagree (e.g., sample shows 15% ellipsis, user says "I never use ellipsis"), trust the sample for frequency and the interview for conscious preferences. Note conflicts in the profile's "Generation Notes" section.

---
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — add --setup-deep 10-question voice interview"
```

---

### Task 9: Add the Calibration Loop (PROSE-style iterative refinement)

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find a good insertion point — after the Deep Onboarding section**

The Deep Onboarding section ends with `---`. Insert the Calibration Loop immediately after.

- [ ] **Step 2: Insert the Calibration Loop section**

Use Edit to find the `---` that ends the Deep Onboarding section. Add AFTER it:

```

## The --calibrate Loop — Iterative Profile Refinement

The most important new mechanism in v3.1. Research basis: Apple ICML 2025 PROSE (33% improvement over static profiles), PerFine 2025 knockout strategy, Self-Refine NeurIPS.

**Core insight:** No matter how well the profile is extracted from samples, it will have gaps. The writer's implicit preferences only become visible when they see what the model produces WITHOUT knowing those preferences. The calibration loop closes the gap by generating samples, getting corrective feedback, and updating the profile — running 2-3 rounds until the profile converges.

### When to run --calibrate

After `--setup` or `--setup-deep`, suggest:
> "רוצה לשפר את ההתאמה? הרץ /hebrew-writer --calibrate"

Users can also run it manually at any time to refine an existing profile.

### The loop

When user runs `/hebrew-writer --calibrate`:

**Round 1 — Prime the loop**

**Step 1.1:** Verify a default profile exists. If not:
> "אין פרופיל קול. הרץ /hebrew-writer --setup קודם."

**Step 1.2:** Pick a calibration topic. Use a neutral topic the user hasn't written about, appropriate to their primary content type. Examples by content type:
- Blog: "למה סטארטאפים נכשלים ברגע שהם מגיעים לשלב ההסקיילאפ"
- Social: "מה שלמדתי אתמול ממישהו ברכבת"
- Business: "עדכון לצוות על שינוי בתכנון הרבעוני"
- Email: "מייל תגובה ללקוח שמתלונן על עיכוב"

**Step 1.3:** Generate TWO different versions of a short piece (200-400 words each) on the topic. Both use the current voice profile, but each emphasizes DIFFERENT aspects of the profile:
- Version A: Emphasizes Key Tells and the most distinctive signature passages
- Version B: Emphasizes the 42-feature averages and the more representative passages

Present both labeled simply as "גרסה א" and "גרסה ב".

**Step 1.4:** Ask using AskUserQuestion:
> "איזו גרסה נשמעת יותר כמוך? (א / ב / שתיהן דומות / אף אחת לא מתאימה)"

**Step 1.5:** Follow-up based on answer:

If user picks A or B:
> "מה בגרסה השנייה הרגיש לא ממך? (1-2 משפטים מספיקים)"

If user says "שתיהן דומות":
> "אז מה חסר בשתיהן? מה היית מוסיף או משנה כדי שזה ירגיש יותר ממך?"

If user says "אף אחת לא מתאימה":
> "ספר לי מה בדיוק לא מתאים. אפשר להצביע על משפטים ספציפיים: 'הקטע X לא נשמע כמוני כי Y'."

**Step 1.6:** Update the profile based on the feedback:
- If user picked a version: The winning version becomes the "ground truth." Update Key Tells to strengthen what it does well. Add new Key Tells for what the losing version did wrong (the user's description becomes the enforcement rule).
- If both were similar: The feedback ("what's missing") becomes a new Key Tell or Differential Feature.
- If neither worked: The feedback becomes priority-level corrections that override existing profile components.

**Step 1.7:** Save the updated profile. Log the calibration round in the profile's "Calibration History" section:
```
### Calibration Round 1 ([date])
- Topic: [topic]
- User preference: [A/B/both similar/neither]
- User feedback: [quote of their correction]
- Profile changes: [list of what was updated]
```

**Round 2 — Verify and refine**

**Step 2.1:** Generate TWO new versions on a DIFFERENT topic, now using the updated profile.

**Step 2.2:** Repeat Steps 1.4-1.7.

**Round 3 — Final validation (optional)**

**Step 3.1:** Generate ONE version on a third topic.

**Step 3.2:** Ask:
> "זה נשמע כמוך? (כן, מושלם / כן, בעיקר / משהו קטן חסר / עדיין לא)"

**Step 3.3:**
- "כן, מושלם" → Profile locked. Exit loop.
- "כן, בעיקר" → Ask what's missing. Update profile. Exit loop.
- "משהו קטן חסר" → Ask what. Update. Offer round 4.
- "עדיין לא" → Offer to run `/hebrew-writer --setup-deep` for the 10-question interview, or restart setup with fresh samples.

### Maximum 4 rounds

If the loop hits 4 rounds without the user saying "כן, מושלם" or "כן, בעיקר", something is fundamentally off — suggest re-running `--setup` with different samples, or trying `--setup-deep`.

### How the profile updates work

Each round's feedback generates ONE of these update types:

**Type 1 — Strengthen existing Key Tell:**
User said "גרסה א is more me — גרסה ב sounds too polished." The profile's "avoids polishing" Key Tell gets a stronger enforcement rule and moves to position #1.

**Type 2 — Add new Key Tell:**
User said "version B felt off because it used long sentences — I write short." New Key Tell: "Hard cap on sentence length — max 15 words in 90% of sentences."

**Type 3 — Remove/weaken a Key Tell:**
User said "both had too many 'נו' — I use it rarely." The existing "heavy נו usage" Key Tell gets demoted or removed.

**Type 4 — Add a Differential Feature:**
User said "version A was missing the self-correction moments I always have." New DF: "Self-correction required every 200 words minimum."

**Type 5 — Rewrite a signature passage:**
If user identifies a signature passage as unrepresentative, mark it for removal or ask for a replacement excerpt.

### The Key Generation Note

During calibration, be honest about what you're doing. Show the user:
> "בסבב הזה שיניתי את הפרופיל שלך: [list of changes]. נראה לך הגיוני?"

If the user disagrees with an update, don't apply it. Calibration should feel collaborative, not automated.

---
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — add --calibrate iterative refinement loop (PROSE-style)"
```

---

### Task 10: Update the Smart Fusion Engine with new priority rules

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the "Smart Fusion Engine" section**

Run:
```bash
grep -n "^## Smart Fusion Engine" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

- [ ] **Step 2: Replace the Smart Fusion Engine section**

Use Edit to find the full existing Smart Fusion Engine section (from `## Smart Fusion Engine` to the next `---` or `##` heading). Replace with:

```
## Smart Fusion Engine (v3.1)

When generating with a loaded voice profile, these priority rules determine what controls what. v3.1 reorders priorities to put Key Tells at the top, above the 42-feature table.

### Priority Order (v3.1)

**Priority 1 — Key Tells (NEW top priority):** The 3-5 outlier behaviors are enforced absolutely. If a generation choice conflicts with a Key Tell, the Key Tell wins. No exceptions.

**Priority 2 — Differential Features (from negative sample or calibration):** Enforced with near-equal weight to Key Tells.

**Priority 3 — Signature Passages (style-extreme selection):** Used as active few-shot anchors. Before generating each section, mentally reference the passages. Ask: "Could this paragraph fit naturally among the signature passages?"

**Priority 4 — Behavioral Profile (from --setup-deep, if present):** Self-reported preferences override sample-measured features where they conflict.

**Priority 5 — 42-Feature measurements:** Used for baseline constraints — sentence length target, formality level, slang density. These now function as a supporting scaffold, not the primary instruction.

**Priority 6 — Safety net (skill rules):** AI vocabulary blacklist, em dash ban, Big No-Nos, anti-detection minimums. These ALWAYS apply regardless of voice profile.

**Priority 7 — Soul Layer requirements:** Proper noun density, דווקא usage, memory drops, stakes. These ALWAYS add on top.

### Conflict Resolution (updated for v3.1)

**When user's voice conflicts with skill safety rules:**
- Safety always wins. Even if the user's samples show "מהווה" usage, we ban it.

**When Key Tells conflict with 42-feature averages:**
- Key Tells always win. If Key Tell says "never uses formal connectors" but the 42-feature table shows "0.3% formal connectors detected," the Key Tell rule (zero) overrides the measured average.

**When calibration feedback conflicts with original profile:**
- Calibration always wins. Feedback is newer evidence and comes directly from the user's explicit preference.

**When Signature Passages suggest something different from features:**
- Passages win for rhythm, tone, and emotional temperature. Features win for frequency counts and averages.

**When Behavioral Profile (Q&A) conflicts with sample measurements:**
- Trust samples for measurable frequencies. Trust Q&A for conscious preferences and identity markers (like "my writing sounds like X").

### Active Few-Shot Anchoring (v3.1 enhancement)

Before generating each paragraph, do this mentally:

1. Read the top 3 Signature Passages
2. Identify the rhythm pattern, emotional temperature, and word choices
3. Ask: "What would this writer do with this next paragraph?"
4. Generate, matching the rhythm first, content second
5. After each paragraph, check: "Does this fit in a collection with the signature passages? If I showed this paragraph alongside SP1 and SP2, would they feel like the same person?"

If the answer is no, revise. This "signature passage fit check" is the v3.1 equivalent of pattern matching against the writer's actual voice.

### When NO profile is loaded

If no voice profile is loaded (first-time user, --voice not specified, no default profile):
- Use default Israeli casual voice (Layers 1-5)
- At end of output, append ONE line (once per session): "💡 רוצה שזה ישמע יותר כמוך? הרץ: /hebrew-writer --setup"
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — Smart Fusion Engine updated with Key Tells priority and active passage anchoring"
```

---

## Chunk 4: Voice Profile Template Update

### Task 11: Create the v3.1 voice profile template

**Files:**
- Create: `voice-profile-template-v3.1.md`

- [ ] **Step 1: Read the v3 template**

Run:
```bash
cat "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/voice-profile-template-v3.md"
```

- [ ] **Step 2: Create the v3.1 template**

Write this content to `voice-profile-template-v3.1.md`:

```markdown
---
name: [profile-name]
created: [date]
sample-size: [word count]
accuracy-tier: [basic|strong|full-clone|maximum]
gender: [male|female|neutral]
primary-content-type: [blog|social|business|mixed]
version: 3.1
calibration-rounds: [0|1|2|3|4]
---

## Key Tells (Priority 1 — Top of profile)

The 3-5 most statistically unusual behaviors this writer exhibits. These are enforced absolutely during generation. Any conflict with the 42-feature table is resolved in favor of the Key Tell.

### KT1: [Short descriptive name]
- **What:** [Specific behavior, in one sentence]
- **Baseline:** [Israeli Hebrew norm for this feature]
- **This writer:** [Measured value, showing deviation]
- **Enforcement:** [Concrete rule for generation]

### KT2: [Short descriptive name]
[Same structure]

### KT3: [Short descriptive name]
[Same structure]

[KT4 and KT5 optional — only if they rise to the "most outlier" level]

---

## Differential Features (Priority 2 — From negative sample, if provided)

Features that are present in the writer's real voice but absent from their "wrong" / flat voice. These emerge from contrastive analysis of positive vs. negative samples.

### DF1: [Feature name]
- **Positive sample:** [value/pattern]
- **Negative sample:** [value/pattern]
- **Delta:** [what makes the positive "more them"]
- **Enforcement:** [how to apply]

[DF2, DF3 optional]

---

## Signature Passages (Priority 3 — Style-extreme selection)

5-10 verbatim excerpts selected for maximum stylistic outlierness — the passages most different from generic Israeli Hebrew. These serve as few-shot style anchors during generation.

### SP1: [rhythm label] + [Key Tell(s) demonstrated]
> "[verbatim passage]"
— demonstrates: [what this shows about the writer's distinctive voice]
— enforces: [KT numbers this passage anchors]

### SP2: [rhythm label] + [Key Tell(s) demonstrated]
> "[verbatim passage]"
— demonstrates: [description]
— enforces: [KT numbers]

[SP3-SP10 as needed]

---

## Behavioral Profile (Priority 4 — From --setup-deep interview, if run)

Self-reported preferences from the 10-question voice interview. Complements sample-measured features.

### Peak voice anchor
[Quote from Q1 — writer's self-identified "most them" moment]

### Anti-voice (from Q2)
[What the writer flees from, if they provided a negative anchor]

### Arousal rhythm (from Q3)
[How style shifts when emotionally engaged]

### Argument architecture (from Q4)
[claim-first / build-up / story-first / question-first]

### Disagreement pattern (from Q5)
[head-on / sideways / bypass]

### Preferred discourse markers (from Q6)
[Top 3 in order]

### Ending behavior (from Q7)
[explicit / CTA / reflective / just stop / question]

### Aside density (from Q8)
[never / rare / moderate / heavy]

### Uncertainty expression (from Q9)
[hedge openly / state and move / mix]

### Voice reference (from Q10)
[External anchor — person, show, vibe]

---

## 42-Feature Measurements (Priority 5 — Supporting scaffold)

### Category 1: Sentence Architecture (8 features)
[Same as v3 — see 42-feature extraction section of SKILL.md]

### Category 2: Vocabulary Profile (8 features)
[Same as v3]

### Category 3: Discourse & Flow (6 features)
[Same as v3]

### Category 4: Argumentation Style (5 features)
[Same as v3]

### Category 5: Emotional Register (5 features)
[Same as v3]

### Category 6: Punctuation & Formatting (4 features)
[Same as v3]

### Category 7: Hebrew-Specific Patterns (6 features)
[Same as v3]

---

## Fusion Rules (Generation-time priority order)

1. Key Tells (absolute)
2. Differential Features (absolute)
3. Signature Passages (active few-shot anchors)
4. Behavioral Profile (preference overrides)
5. 42-Feature measurements (baseline scaffold)
6. Skill safety net (AI blacklist, em dash ban, Big No-Nos)
7. Soul Layer requirements (proper nouns, דווקא, memory drops)

---

## Calibration History

### Round 1 ([date])
- Topic: [topic]
- User preference: [A/B/both similar/neither]
- User feedback: [quote]
- Profile changes: [list of updates]

[Additional rounds as they happen]

---

## Generation Notes

[Any special instructions, observations, or resolved conflicts that don't fit the structured categories above. E.g., "Sample measured 2% ellipsis but user said 'I use ... constantly' — trusting user, overriding sample" or "KT4 and DF1 both point to short sentences — reinforced enforcement."]
```

- [ ] **Step 3: Commit**

```bash
git add voice-profile-template-v3.1.md
git commit -m "feat: v3.1 — new voice profile template with Key Tells, Differential Features, Calibration History"
```

---

### Task 12: Update Layer 7's file structure section to reference new template

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the "Profile file structure (v3 format)" section**

Run:
```bash
grep -n "Profile file structure" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

- [ ] **Step 2: Replace with v3.1 structure reference**

Use Edit to find:
```
### Profile file structure (v3 format):
```

And everything through the end of that code block (the ``` that closes it). Replace with:

```
### Profile file structure (v3.1 format):

See `voice-profile-template-v3.1.md` for the complete template. The structure (in priority order):

1. **Frontmatter** — metadata including version: 3.1 and calibration-rounds count
2. **Key Tells** — top 3-5 outlier behaviors (Priority 1, at the TOP of the file)
3. **Differential Features** — from negative sample or calibration (Priority 2)
4. **Signature Passages** — style-extreme selection (Priority 3)
5. **Behavioral Profile** — from --setup-deep interview, if run (Priority 4)
6. **42-Feature Measurements** — supporting scaffold (Priority 5)
7. **Fusion Rules** — priority order reference
8. **Calibration History** — log of all --calibrate rounds
9. **Generation Notes** — resolved conflicts and special instructions

Read the template file during `--setup`, `--setup-deep`, or `--calibrate` using the Read tool to ensure you save in the exact expected format.
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — reference new template file structure in Layer 7"
```

---

## Chunk 5: Generation Pipeline Updates

### Task 13: Update Generation Pipeline Step 8 to use new fusion priority

**Files:**
- Modify: `SKILL-v3.1.md`

- [ ] **Step 1: Find the Generation Pipeline section**

Run:
```bash
grep -n "^# Generation Pipeline" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

- [ ] **Step 2: Find and replace Step 8 (Voice adjustments)**

Use Edit to find:
```
**Step 8: Voice adjustments** (Layer 7, if profile loaded)
Apply the internal voice model: sentence length preferences, discourse marker patterns, characteristic vocabulary, opinion expression style, paragraph opening habits.
```

Replace with:
```
**Step 8: Voice adjustments** (Layer 7, if profile loaded)
Apply the Smart Fusion Engine priority order:
1. First, read and internalize Key Tells (absolute rules)
2. Then Differential Features (if present)
3. Then read the top 3 Signature Passages — mentally feel their rhythm and voice
4. Apply Behavioral Profile preferences (if --setup-deep was run)
5. Calibrate frequencies to 42-Feature measurements as supporting scaffold
6. Generate, constantly checking: "Does this fit alongside the Signature Passages?"
7. If any generated paragraph feels off from the Key Tells or Passages, revise immediately
```

- [ ] **Step 3: Commit**

```bash
git add SKILL-v3.1.md
git commit -m "feat: v3.1 — Generation Pipeline Step 8 uses new fusion priority order"
```

---

## Chunk 6: Testing and Installation

### Task 14: Verify SKILL-v3.1.md is complete and well-formed

**Files:**
- Read: `SKILL-v3.1.md`

- [ ] **Step 1: Check line count**

Run:
```bash
wc -l "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

Expected: Approximately 3,200-3,400 lines (up from v3's 2,896 — we added substantial new content across Key Tells, negative examples, Deep Onboarding, Calibration Loop, and updated Fusion Engine).

- [ ] **Step 2: Verify all key sections are present**

Run these greps and confirm each returns a match:

```bash
grep -c "^# What's New in v3.1$" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
grep -c "^## Key Tells" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
grep -c "^## The --setup Basic Onboarding Flow$" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
grep -c "^## Contrastive Analysis" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
grep -c "^## The --setup-deep Flow" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
grep -c "^## The --calibrate Loop" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
grep -c "^## Smart Fusion Engine (v3.1)$" "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

Each command should return `1` (one match per section heading). If any returns `0`, the section is missing — go back to the relevant task and fix.

- [ ] **Step 3: Verify frontmatter YAML is valid**

Read the first 30 lines of the file:
```bash
head -30 "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md"
```

Check:
- Opens with `---`
- Closes frontmatter with `---` before the first `#` heading
- `name: hebrew-writer` (not hebrew-writer-v3.1 — the skill itself is still named hebrew-writer, only the directory changes)
- `user-invocable: true`
- `allowed-tools` list includes Read, Write, Edit, Grep, Glob, AskUserQuestion

### Task 15: Install v3.1 as a skill

**Files:**
- Create: `~/.claude/skills/hebrew-writer-v3-1/SKILL.md`
- Create: `~/.claude/skills/hebrew-writer-v3-1/voice-profile-template.md`

- [ ] **Step 1: Create the skill directory**

Run:
```bash
mkdir -p ~/.claude/skills/hebrew-writer-v3-1
```

- [ ] **Step 2: Copy SKILL.md and template**

Run:
```bash
cp "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/SKILL-v3.1.md" ~/.claude/skills/hebrew-writer-v3-1/SKILL.md
cp "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill/voice-profile-template-v3.1.md" ~/.claude/skills/hebrew-writer-v3-1/voice-profile-template.md
```

- [ ] **Step 3: Update frontmatter in the installed copy**

The installed SKILL.md needs its `name:` field to reflect the skill invocation. Edit `~/.claude/skills/hebrew-writer-v3-1/SKILL.md` and change:

```
name: hebrew-writer
```

To:
```
name: hebrew-writer-v3-1
```

Do NOT change the `name:` field in the source file `SKILL-v3.1.md` — that's the one we'll eventually rename to the main version.

- [ ] **Step 4: Verify installation**

Run:
```bash
ls -la ~/.claude/skills/hebrew-writer-v3-1/
```

Expected: Two files (SKILL.md and voice-profile-template.md).

### Task 16: Manual test — test that --setup flow works

**Files:**
- No file changes — this is a verification task

- [ ] **Step 1: Test --setup with sample text**

In a fresh Claude Code session, invoke:
```
/hebrew-writer-v3-1 --setup
```

Paste a sample of 300-500 Hebrew words when prompted. Provide a negative sample (or type 'דלג'). Answer the gender and content type questions.

Expected behavior:
- The skill asks for positive sample
- Then asks for negative sample (NEW in v3.1)
- Then asks gender
- Then asks content type
- Extracts Key Tells (should report "Key Tells found: [N]")
- Saves profile to `.claude/voices/default.hebrew-voice.md`
- Reports accuracy tier
- Suggests `/hebrew-writer-v3-1 --calibrate`

- [ ] **Step 2: Verify profile file was created correctly**

Run:
```bash
cat ~/.claude/voices/default.hebrew-voice.md 2>/dev/null || cat .claude/voices/default.hebrew-voice.md
```

Expected: File exists with v3.1 structure — Key Tells at top, then 42 features, then Signature Passages.

### Task 17: Manual test — test that --calibrate flow works

- [ ] **Step 1: Run calibration**

```
/hebrew-writer-v3-1 --calibrate
```

Expected behavior:
- Skill picks a calibration topic matching your content type
- Generates two versions (א and ב)
- Asks which sounds more like you
- Asks what's wrong with the other
- Updates the profile
- Logs the round in Calibration History
- Offers round 2

- [ ] **Step 2: Verify calibration history appears**

```bash
grep -A 5 "Calibration History" ~/.claude/voices/default.hebrew-voice.md
```

Expected: "Round 1 ([date])" entry with topic, preference, feedback, and changes.

### Task 18: Manual test — generate content with calibrated profile

- [ ] **Step 1: Generate content**

```
/hebrew-writer-v3-1 "שקף את חשיבות האיזון בין עבודה לחיים" --type blog --length medium
```

- [ ] **Step 2: Verify output quality**

Check the generated text:
- Does it sound like your voice? (Ask the user who did the setup)
- Are the Key Tells visible? (e.g., if KT was "never uses formal connectors," verify no formal connectors appear)
- Is the rhythm similar to your signature passages?
- Does it avoid the "wrong" patterns from your negative sample?

If any of these fail, note what's off and run another round of `/hebrew-writer-v3-1 --calibrate`.

---

## Chunk 7: GitHub Push and Deprecate v3

### Task 19: Push v3.1 to GitHub

**Files:**
- Modify: Git repository

- [ ] **Step 1: Stage all v3.1 files**

```bash
cd "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill"
git add SKILL-v3.1.md voice-profile-template-v3.1.md docs/superpowers/plans/2026-04-20-hebrew-writer-v3.1-fusion.md
```

- [ ] **Step 2: Verify staged changes**

```bash
git status
```

Expected: Three new files staged. Any commits from earlier tasks should already be committed (not in staging).

- [ ] **Step 3: Final commit**

```bash
git commit -m "$(cat <<'EOF'
feat: hebrew-writer v3.1 — fusion rebuild with Key Tells + calibration

v3 hit a ceiling because feature-based profiles miss implicit style.
v3.1 rebuilds the fusion engine with 5 research-backed mechanisms:

- Key Tells extraction: 3-5 most outlier behaviors (CMU authorship research)
- Style-extreme passage selection: outlier passages, not representative ones (EMNLP 2025)
- --calibrate iterative refinement: PROSE loop, 33% improvement over static profiles (Apple ICML 2025)
- Negative examples in --setup: contrastive learning (RG-Contrastive 2025)
- --setup-deep 10-question voice interview: behavioral elicitation (Stanford 2024)

Research basis: EMNLP 2025 "Catch Me If You Can?" proved 42-feature tables
systematically underperform iterative refinement and outlier-focused signals.

v2 and v3 remain in repo for reference. v3.1 is the active version.

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>
EOF
)"
```

- [ ] **Step 4: Push to remote**

```bash
git push
```

Expected: Push succeeds. New commit visible at https://github.com/baldiga/hebrew-writer.

### Task 20: Optionally rename v3.1 as the primary SKILL.md on GitHub

This is a FUTURE task — only run after v3.1 has been validated through real-world use (at least 1 week of testing).

**Files:**
- Rename: `SKILL-v3.1.md` → `SKILL.md`
- Update: `README.md`

- [ ] **Step 1: Rename file**

```bash
cd "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill"
git mv SKILL-v3.1.md SKILL.md
git mv voice-profile-template-v3.1.md voice-profile-template.md
```

- [ ] **Step 2: Update README.md to reference new files**

Search README.md for any references to `SKILL-v3.1.md` or `voice-profile-template-v3.1.md` and update to `SKILL.md` / `voice-profile-template.md`.

- [ ] **Step 3: Commit and push**

```bash
git commit -m "chore: promote v3.1 to primary SKILL.md after validation"
git push
```

---

## Execution Notes

### Key risks and mitigations

1. **Key Tells extraction may over-fit to noise:** A writer with a small sample might have "deviations" that are actually just sample variance, not true stylistic signals. Mitigation: require minimum 300 words before running Key Tells extraction. If sample is smaller, warn the user and only extract 1-2 Key Tells.

2. **Calibration feedback may contradict the original profile aggressively:** A user might pick version A, then in round 2 pick version B, then round 3 pick neither. Mitigation: the profile update logic should require at least 2 consistent signals before removing a Key Tell. Single-round contradictions are logged but don't trigger removal.

3. **Style-extreme passages may include passages that feel embarrassing to the user:** A writer's most distinctive moment might be an angry rant they'd rather not be represented by. Mitigation: after passage selection, present the 5-10 candidates to the user and ask "are any of these ones you don't want as part of your voice profile?" Let them remove up to 2.

4. **The calibrate loop could run forever if the user is hard to please:** Mitigation: hard cap at 4 rounds. After that, suggest `/hebrew-writer-v3-1 --setup-deep` or fresh samples.

5. **Negative samples may be rare or unclear:** Most users won't have a saved example of their own "wrong" writing. Mitigation: the negative sample is optional in --setup. If skipped, the profile still works, just without Differential Features.

### Implementation order rationale

The chunks are ordered for TDD and incremental verification:
1. Foundation first (copy v3, update metadata) — guarantees we have a clean base
2. Argument parsing updates — enables invoking new modes
3. Layer 7 rewrite — the heart of the fusion rebuild, in logical dependency order (Key Tells → passages → onboarding → deep onboarding → calibration → fusion engine)
4. Template update — reflects all the new profile components
5. Pipeline updates — connects generation to the new fusion priority
6. Testing and installation — verifies before deployment
7. GitHub push — deploys
8. Eventual rename — promotes v3.1 to primary after validation

Each chunk ends with a working, testable state of the skill. If work stops at the end of any chunk, the skill is not broken — it just has fewer new features.

### Success criteria

v3.1 is successful when:
- A user with a solid voice profile (500+ words, 1-2 calibration rounds) generates text they would accept as "sounding like me" at least 80% of the time
- The Key Tells are clearly visible in generated output
- The skill never violates the user's explicit preferences from the Behavioral Profile
- Anti-detection and Soul Layer requirements are still met (95/100 threshold)
- A user can improve their profile iteratively without needing to re-paste samples
