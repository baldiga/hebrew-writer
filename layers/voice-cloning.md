# LAYER 7: Voice Matching & Fusion Engine (v3.1)

## Overview

v3.1 rebuilds voice matching based on EMNLP 2025 research showing that feature tables systematically underperform iterative refinement and outlier-focused signals. The v3.1 voice system has six mechanisms:

1. **Key Tells Extraction** — 3-5 outlier behaviors that form the writer's fingerprint (Priority 1)
2. **Style-extreme Signature Passages** — 5-10 real excerpts chosen for stylistic outlierness
3. **--setup Basic Onboarding** — with negative sample for contrastive analysis
4. **--setup-deep Voice Interview** — 10 behavioral questions surfacing implicit preferences
5. **--calibrate Iterative Refinement** — PROSE-style loop with user feedback
6. **Smart Fusion Engine** — 7-level priority order with active few-shot anchoring

---

## Voice Profile Gate (Hard Gate — No Profile = No Generation)

**STOP before generating any content.**

Check for an existing voice profile:
- Look for any `.md` file under `.claude/voices/` in the working directory
- Look for any `.md` file under `~/.claude/voices/`
- If `--voice {profile}` was specified, check that profile exists

**If NO voice profile exists:**

Do NOT generate. Output this instead:

---

עצירה לפני שמתחילים.

Hebrew Writer v5 בלי voice profile הוא עברית גנרית של AI. זה בדיוק מה שהוא אמור לפתור.

**לפני כל כתיבה: בנה פרופיל קול.** זה לוקח 10 דקות ועושה את כל ההבדל.

**שלב 1 — הרץ --setup:**
```
/hebrew-writer --setup
```
תדביק 2-3 טקסטים שכתבת. ה-skill מנתח 42 פיצ'רים, מזהה את ה-Key Tells שלך, ובונה פרופיל.

**שלב 2 — הרץ --setup-deep (ממש מומלץ):**
```
/hebrew-writer --setup-deep
```
ראיון של 10 שאלות שחושף דברים שניתוח הדוגמה לא יכול לראות — המבנה הפנימי שלך, מה אתה בורח ממנו, איך הכתיבה שלך משתנה כשאתה נרגש.

**הפרש בין כתיבה עם פרופיל לבלי פרופיל: שמים וארץ.**

---

Do not generate until the user completes --setup. If the user explicitly says "just write without a profile" or "I don't care", generate with the default Israeli voice but prepend a one-line warning:

> ⚠️ כותב בלי voice profile — הפלט יהיה עברית ישראלית גנרית, לא הקול שלך.

---

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

Differential Features get enforced at Priority 2, directly below Key Tells (see Smart Fusion Engine section).

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

### Cross-round contradiction handling

When feedback in Round N contradicts an update from Round M (M < N):
- Do NOT silently overwrite the earlier update
- Flag the contradiction in the profile's "Generation Notes" section:
  ```
  Cross-round conflict: Round [M] updated [what] based on feedback [X]. Round [N] feedback [Y] contradicts this.
  ```
- Before applying Round N's update, ask the user:
  > "בסבב [M] אמרת [X] וזה עדכן את [Z]. עכשיו אתה אומר [Y] שמרמז על דבר הפוך. איזו העדפה חזקה יותר בשבילך?"
- Apply whichever the user confirms. Don't silently pick one.

This prevents calibration from oscillating and keeps the user in control of their own voice profile.

### The Key Generation Note

During calibration, be honest about what you're doing. Show the user:
> "בסבב הזה שיניתי את הפרופיל שלך: [list of changes]. נראה לך הגיוני?"

If the user disagrees with an update, don't apply it. Calibration should feel collaborative, not automated.

---

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

### Fallback when fewer than 3 Key Tells emerge

If the sample doesn't show clear deviation on at least 3 features from baseline, do NOT fabricate Key Tells to fill slots. This happens with:
- Basic tier samples (200-500 words) of bland/neutral writing
- Writers whose voice is genuinely close to Israeli Hebrew baseline
- Samples from a single content type where the writer plays it safe

Protocol when fewer than 3 emerge:
1. Record only the real Key Tells found (can be 1 or 2 — don't hallucinate more)
2. Add a note to the profile's "Generation Notes" section: "Key Tell coverage is thin (only N found in baseline sample). Calibration strongly recommended for better voice match."
3. Prompt user more aggressively to run `/hebrew-writer --calibrate` or provide more samples via `--setup-deep`
4. Weight Signature Passages more heavily during generation to compensate

### Where Key Tells live in the profile

Key Tells get their own section at the TOP of the voice profile file, before the 42-feature table. They are the first thing read during generation, and the last thing checked in self-audit.

## 42-Feature Voice Extraction

Analyze the user's writing samples across these 7 categories. For each feature, record a concrete measurement or description.

### Category 1: Sentence Architecture (8 features)

| # | Feature | What to measure | Example output |
|---|---------|----------------|----------------|
| 1 | Average sentence length | Count words per sentence, compute average | "7.3 words" |
| 2 | Sentence length std deviation | How much do lengths vary? | "high (3-28 range)" |
| 3 | Shortest typical sentence | Minimum meaningful sentence | "2-3 words" |
| 4 | Longest typical sentence | Maximum natural sentence | "25-30 words" |
| 5 | Fragment frequency | % of sentences under 5 words | "31%" |
| 6 | Question frequency | Questions per 500 words | "3.2 per 500w" |
| 7 | Exclamation frequency | Exclamations per 500 words | "1.8 per 500w" |
| 8 | Ellipsis frequency | Uses "..." — how often? | "heavy — every 200w" |

### Category 2: Vocabulary Profile (8 features)

| # | Feature | What to measure | Example output |
|---|---------|----------------|----------------|
| 9 | Formality level | 1-10 scale | "3/10 (casual)" |
| 10 | Hebrew-English ratio | % of words that are English | "8% code-switching" |
| 11 | Slang density | Which slang, how often | "תכל'ס heavy, יאללה rare, סבבה moderate" |
| 12 | Preferred intensifiers | ממש vs מאוד vs ביותר | "ממש dominant, מאוד never" |
| 13 | Characteristic words | 5-10 words this person overuses | "בעצם, תכל'ס, נו, פשוט, ממש" |
| 14 | Avoided words | Words this person never uses | "לפיכך, מהווה, בהתאם" |
| 15 | Vocabulary breadth | Rich/varied or tight/focused | "moderate — repeats key terms" |
| 16 | Technical jargon density | Domain-specific terms frequency | "high tech jargon (API, deploy, stack)" |

### Category 3: Discourse & Flow (6 features)

| # | Feature | What to measure | Example output |
|---|---------|----------------|----------------|
| 17 | Preferred discourse markers | Ranked list | "נו >> בעצם > כאילו > אז" |
| 18 | Discourse marker frequency | % of tokens | "4.2%" |
| 19 | Preferred connectors | Which connectors dominate | "אז, אבל, כי — avoids גם" |
| 20 | Self-correction frequency | How often do they correct mid-text | "high — every 300w" |
| 21 | Parenthetical/aside frequency | How often do they digress | "moderate — 1 per 500w" |
| 22 | Paragraph length pattern | Short/medium/long/mixed | "mostly short (2-3 sentences)" |

### Category 4: Argumentation Style (5 features)

| # | Feature | What to measure | Example output |
|---|---------|----------------|----------------|
| 23 | Opening style | How they start pieces | "claim-first — leads with opinion" |
| 24 | Opinion strength | Bold/moderate/hedged | "bold — states positions directly" |
| 25 | Evidence style | Anecdotal/data-driven/mixed | "anecdotal — personal stories" |
| 26 | Pushback handling | How they address objections | "acknowledging — gives ground then responds" |
| 27 | Conclusion style | How they end | "abrupt — just stops, no summary" |

### Category 5: Emotional Register (5 features)

| # | Feature | What to measure | Example output |
|---|---------|----------------|----------------|
| 28 | Humor style | Type of humor used | "self-deprecating + dry" |
| 29 | Humor frequency | How often | "frequent — every 200w" |
| 30 | Emotional temperature | Cool/warm/hot | "warm — personally invested" |
| 31 | Vulnerability level | Exposed/guarded/absent | "exposed — admits uncertainty" |
| 32 | Irony usage | Heavy/light/none | "light — occasional understatement" |

### Category 6: Punctuation & Formatting (4 features)

| # | Feature | What to measure | Example output |
|---|---------|----------------|----------------|
| 33 | Comma density | Light/moderate/heavy | "light — prefers periods" |
| 34 | Period preference | Many short or fewer long sentences | "many short sentences" |
| 35 | Ellipsis usage | Never/occasional/heavy | "heavy — trails off often" |
| 36 | Formatting style | Minimal/moderate/heavy | "minimal — almost no bold" |

### Category 7: Hebrew-Specific Patterns (6 features)

| # | Feature | What to measure | Example output |
|---|---------|----------------|----------------|
| 37 | Construct state vs. של | Ratio | "20/80 — mostly של" |
| 38 | Pro-drop consistency | Always/sometimes/never drops pronouns | "always in past tense" |
| 39 | Binyan preferences | Which verb patterns dominate | "Pa'al dominant, avoids Hif'il" |
| 40 | Gender handling | Standard/inclusive/mixed | "standard masculine default" |
| 41 | Sentence-initial particles | Which ones, how often | "אז (heavy), אבל (moderate), וגם" |
| 42 | Cultural reference density | Sparse/moderate/rich | "rich — army, food, bureaucracy" |

### Accuracy Tiers

| Tier | Word count | Features extracted | Confidence |
|------|-----------|-------------------|------------|
| Basic | 200-500 | Categories 1-2 only (16 features) | Rough approximation |
| Strong | 500-1500 | Categories 1-5 (32 features) | Solid match |
| Full Clone | 1500+ | All 42 features | High fidelity |
| Maximum | Multiple texts | All 42 + cross-document consistency | Professional ghostwriter |

---

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

**Example stored passages:**
```
## Signature Passages

### SP1 (rhythm + fragment)
"בניתי את המערכת בשלושה ימים. שלושה. לא שבוע, לא חודש. שלושה ימים."
— demonstrates: short fragments after statement, repetition for emphasis

### SP2 (self-correction + discourse marker)
"הגישה הזאת עובדת, בעצם — רגע, לא. היא עובדת כשיש לך צוות קטן. אחרת זה בלאגן."
— demonstrates: mid-thought pivot, בעצם, casual register, cultural word (בלאגן)

### SP3 (opinion + vulnerability)
"אני כותב את זה כמי שטעה בזה בעצמו. שלוש פעמים. אז אולי תשמעו."
— demonstrates: stake declaration, specific number, direct address, humor
```

---

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

---

## Few-Shot Anchoring During Generation

When generating with a loaded voice profile:

1. **Read the signature passages** from the profile before writing anything.
2. **Internalize the voice:** "These passages are how this person writes. Match their rhythm, energy, word choices, and emotional temperature."
3. **Generate the content** using all layers, with the user's measured features overriding defaults per the fusion rules.
4. **Mental comparison:** After drafting, ask: "Does this sound like the same person who wrote those passages?" If a paragraph feels off, identify which feature diverges and adjust.
5. **Final passage check:** Read the draft alongside the signature passages. The draft should feel like it could be the next thing this person writes, not a different person.

---

## Voice Profile File Handling

### The --setup flow saves to default profile:

```
/hebrew-writer --setup
```
Creates `.claude/voices/default.hebrew-voice.md` — used automatically for all future generations.

### Named profiles (same as v2):

```
/hebrew-writer --learn "sample text" --save-as danny
/hebrew-writer "topic" --voice danny
```

### One-off matching (same as v2):

```
/hebrew-writer "topic" --my-voice "inline sample"
/hebrew-writer "topic" --my-voice-file path/to/writing.md
/hebrew-writer "topic" --my-voice-files path/to/folder/
```

### Profile lookup order:

1. `--voice [name]` → check `.claude/voices/[name].hebrew-voice.md` (local), then `~/.claude/voices/[name].hebrew-voice.md` (global)
2. No `--voice` flag → check for `default.hebrew-voice.md` (local), then global
3. No profile found → use built-in Israeli casual voice + show onboarding suggestion (once)

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

---

## Error Handling

**--setup with less than 200 words of samples:**
> "צריך מינימום 200 מילים כדי ללמוד את הסגנון שלך. קיבלתי [X] מילים. הדבק עוד טקסטים?"

**--setup with 200-500 words:**
> "קיבלתי [X] מילים — מספיק לפרופיל בסיסי (Basic tier). לדיוק גבוה יותר, הדבק עוד טקסטים (500+ מילים מומלץ). להמשיך עם מה שיש? (כן/לא)"

**`.claude/voices/` doesn't exist:** Create silently.

**--voice profile not found:**
List available profiles from both local and global directories. Ask user to choose or provide samples.

**--my-voice-file path doesn't exist:**
> "File not found: [path]. Please check the path and try again."

**--my-voice-files folder empty:**
> "No text files found in [path]. Please provide a folder containing .md, .txt, or similar text files."

**User runs --setup when a default profile already exists:**
> "כבר יש לך פרופיל קול. רוצה להחליף אותו? (כן/לא)"
If yes, proceed with new setup. Old profile overwritten.

---

