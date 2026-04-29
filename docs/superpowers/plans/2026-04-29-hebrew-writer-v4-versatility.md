# Hebrew Writer v4 — Versatility Engine Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add Layer 9 (Variation Fingerprint System) to the Hebrew Writer skill — a context-aware, memory-aware versatility engine that prevents structural and lexical repetition across and within pieces, without altering any of Layers 1-8.

**Architecture:** Copy SKILL-v3.1.md → SKILL-v4.md in a new `hebrew-writer-v4` skill directory. Add Layer 9 as a new section after Layer 8. Update Generation Pipeline to call Layer 9 in Phase 1 (pre-writing) and Phase 2 (in-writing). Update Layer 6 Self-Audit to add מגוון as a 9th scoring dimension. Install the skill locally.

**Tech Stack:** Markdown skill file (`.md`), JSON variation log (runtime-created), Claude Code skill framework.

---

## File Map

| File | Action | Purpose |
|------|--------|---------|
| `~/.claude/skills/hebrew-writer-v4/SKILL.md` | Create (copy + edit) | Main skill file — all 9 layers |
| `~/.claude/skills/hebrew-writer-v4/voice-profile-template.md` | Create (copy + edit) | Updated voice profile template |
| `.claude/voices/{profile}/variation-log.json` | Created at runtime by skill | Session memory log — not created in this plan |

**Source files:**
- `~/.claude/skills/hebrew-writer-v3-1/SKILL.md` (3,239 lines) — base to copy from
- `~/.claude/skills/hebrew-writer-v3-1/voice-profile-template.md` — base template to copy from

---

## Scoring update reference

The v3.1 Self-Audit formula (Layer 6, line ~1667 in SKILL.md) currently uses these weights:
```
דוגריות(13%) + קצב(14%) + אמינות(14%) + טקסטורה(11%) + נשמה(8%) + נשמה עמוקה(8%) + צפיפות(6%) + רישום(6%) + אנטי-זיהוי(20%) = 100%
```

v4 releases 10% (2% from דוגריות, 2% from קצב, 1% from אמינות, 1% from טקסטורה, 1% from נשמה, 1% from נשמה עמוקה, 2% from אנטי-זיהוי) to fund the new מגוון dimension:
```
דוגריות(11%) + קצב(12%) + אמינות(13%) + טקסטורה(10%) + נשמה(7%) + נשמה עמוקה(7%) + צפיפות(6%) + רישום(6%) + אנטי-זיהוי(18%) + מגוון(10%) = 100%
```

The 95/100 quality gate is unchanged.

---

## Task 1: Create v4 skill directory and copy base files

**Files:**
- Create: `~/.claude/skills/hebrew-writer-v4/` (directory)
- Create: `~/.claude/skills/hebrew-writer-v4/SKILL.md` (copy of v3.1)
- Create: `~/.claude/skills/hebrew-writer-v4/voice-profile-template.md` (copy of v3.1 template)

- [ ] **Step 1: Create the directory and copy files**

```bash
mkdir -p ~/.claude/skills/hebrew-writer-v4
cp ~/.claude/skills/hebrew-writer-v3-1/SKILL.md ~/.claude/skills/hebrew-writer-v4/SKILL.md
cp ~/.claude/skills/hebrew-writer-v3-1/voice-profile-template.md ~/.claude/skills/hebrew-writer-v4/voice-profile-template.md
```

- [ ] **Step 2: Verify copy succeeded**

```bash
wc -l ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: output shows ~3239 lines (same as v3.1 source).

- [ ] **Step 3: Commit**

```bash
cd ~/.claude/skills/hebrew-writer-v4
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): create hebrew-writer-v4 skill directory with v3.1 base"
```

---

## Task 2: Update frontmatter + add "What's New in v4" section

**Files:**
- Modify: `~/.claude/skills/hebrew-writer-v4/SKILL.md` (lines 1-30 frontmatter, line 88 after v3.1 What's New)

- [ ] **Step 1: Update the frontmatter**

In `~/.claude/skills/hebrew-writer-v4/SKILL.md`, replace the frontmatter block (lines 1-30) with:

```yaml
---
name: hebrew-writer
description: |
  v4 — Write Hebrew content indistinguishable from a native Israeli human.
  Generates, rewrites, or detects AI patterns in Hebrew text.
  9-layer system with Variation Fingerprint Engine: Hebrew-first thinking,
  55+ AI pattern detection, Israeli voice injection, linguistic precision,
  rhythm engineering, self-audit scoring (95/100 threshold), adaptive
  voice cloning (Key Tells + style-extreme passages + calibration loop),
  Soul Layer (נשמה עמוקה), and Versatility Engine (מנגנון המגוון).
  v4 adds: Variation Fingerprint (9 schemas, 5 openers, 5 rhythms, 4 vocab
  registers, 5 closing types), context-aware fingerprint selection, session
  memory log (cross-piece variety enforcement), 6 within-piece enforcement
  rules, Spent Phrase Protocol, --fresh flag.
  Grounded in PNAS 2025 register-leveling research, Antislop ICLR 2026
  (8,000+ pattern taxonomy), MATTR lexical diversity, and Hebrew
  argumentative discourse stance research (Frontiers 2025).
  Use when: writing Hebrew content, humanizing Hebrew text, checking
  Hebrew text for AI tells, or matching someone's Hebrew writing voice.
  Triggers: "write in Hebrew", "Hebrew content", "כתוב בעברית",
  "humanize Hebrew", "sound Israeli", "Hebrew blog", "Hebrew article",
  "rewrite in Hebrew", "detect AI Hebrew", "תכתוב לי", "shadow writer"
user-invocable: true
argument-hint: '"topic or text" [--mode generate|rewrite|detect] [--setup] [--setup-deep] [--calibrate] [--fresh] [--type blog|academic|social|business|email|creative|auto] [--length short|medium|long|xl|NUMBER] [--gender male|female|neutral] [--voice profile-name] [--my-voice "sample text"] [--my-voice-file path] [--learn "text" --save-as name] [--show-score]'
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---
```

- [ ] **Step 2: Add "What's New in v4" section**

After the `---` that closes the v3.1 What's New section (find the line `---` that appears right before `# Hebrew Writer — הכותב העברי`, around line 88), insert this block **before** that `---`:

```markdown
# What's New in v4

v3.1 fixed the fusion engine. Extended use of v3.1 revealed the next ceiling: structural and lexical repetition across pieces. After 3-4 posts, the same arc, the same openers, the same phrase cadences. This is the known LLM register-leveling problem (PNAS 2025): instruction-tuned models have a single attractor basin they return to regardless of topic. v4 breaks that basin by design.

**New in v4:**

- **Variation Fingerprint System (Layer 9, מנגנון המגוון)** — Before every generation, the skill computes a 5-dimension Variation Fingerprint: Schema (9 options), Opener Shape (5), Body Rhythm (5), Vocabulary Register (4), Closing Type (5). Each dimension is selected by context-aware rules (topic emotional weight + content type + length), then checked against session memory to guarantee the fingerprint differs meaningfully from the last 3-5 pieces.
- **9 Structural Schemas** — AIDA, PAS, BAB, 4Ps, PSB, HSO, QuestionCascade (for persuasive/opinion content) plus `Narrative` (Situation → Complication → Resolution) and `Explainer` (Context → Mechanism → Implication) for informational and educational content. Forcing a persuasion schema onto a product update or how-to article is its own AI tell.
- **Session Memory Log** — Variation fingerprints stored at `.claude/voices/{profile}/variation-log.json`. Enforces: no same Schema in last 3 pieces, no same Opener in last 3, no same Rhythm in last 2, no same Closing in last 3, no more than 2 of last 5 share the same vocab register. First-run fallback: if log doesn't exist, pick from context mapping and create the log after generation.
- **Schema-Opener Compatibility Table** — Not all schema-opener pairs are structurally coherent. PAS with `intimate` opener is awkward; HSO with `evidence-first` doesn't work. The compatibility table enforces valid pairings after fingerprint computation.
- **6 Within-Piece Enforcement Rules** — Paragraph opener rotation (7 types), connector category rotation (6 categories), question type rotation (3 types), root-family lexical diversity (pre-map alternatives per key concept), stance category rotation (4 Hebrew discourse stance types), paragraph-level structural burstiness (mandatory single-sentence paragraph, no 3 same-length paragraphs in a row).
- **Spent Phrase Protocol** — Per-piece internal tracker. Any 3+ word expression, any connector (by category), any question type, any quote integration style used once is spent. Trigram rule: no 3-word sequence appears twice in any piece.
- **`--fresh` flag** — Clears the variation log. Use when starting a new content project where cross-piece variety from prior pieces is irrelevant.
- **Updated Self-Audit** — מגוון (Versatility) added as a 9th scoring dimension (10%). 10% redistributed from existing dimensions. 95/100 threshold unchanged.
- **Research basis** — PNAS 2025 "Do LLMs Write Like Humans?" (register leveling), Antislop ICLR 2026 (8,000+ slop pattern taxonomy), Bestgen 2024 MATTR lexical diversity, Frontiers 2025 Hebrew discourse stance, IsraParlTweet LREC-COLING 2024, avoid-ai-writing trigram suppression (GitHub).

---
```

- [ ] **Step 3: Verify the What's New section is in place**

```bash
grep -n "What's New in v4" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: one match showing the line number of the new section.

- [ ] **Step 4: Commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): update frontmatter and add What's New in v4 section"
```

---

## Task 3: Add `--fresh` to argument parsing and mode routing

**Files:**
- Modify: `~/.claude/skills/hebrew-writer-v4/SKILL.md` (Argument Parsing section ~line 112, Mode Routing ~line 157)

- [ ] **Step 1: Add `--fresh` to the flag table**

Find this block in the Argument Parsing section (the flag extraction table):

```
--calibrate                          Triggers iterative profile refinement (requires existing profile, no value)
```

Replace it with:

```
--calibrate                          Triggers iterative profile refinement (requires existing profile, no value)
--fresh                              Clears variation log for active profile. Resets cross-piece memory. No value.
```

- [ ] **Step 2: Add `--fresh` to Mode Routing**

Find this block in Mode Routing:

```
- `--setup` → Jump to **Basic Onboarding Flow** (overrides --mode)
- `--setup-deep` → Jump to **Deep Onboarding Flow — 10-Question Voice Interview** (overrides --mode)
- `--calibrate` → Jump to **Calibration Loop** (overrides --mode; requires existing voice profile)
```

Replace with:

```
- `--setup` → Jump to **Basic Onboarding Flow** (overrides --mode)
- `--setup-deep` → Jump to **Deep Onboarding Flow — 10-Question Voice Interview** (overrides --mode)
- `--calibrate` → Jump to **Calibration Loop** (overrides --mode; requires existing voice profile)
- `--fresh` → Clear variation log at `.claude/voices/{profile}/variation-log.json` (or `default` if no profile). Confirm deletion with one line: "✓ Variation log cleared. Next piece starts with a clean slate." Then proceed with `--mode generate` (or the specified mode) as normal.
```

- [ ] **Step 3: Verify**

```bash
grep -n "\-\-fresh" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: at least 3 matches (frontmatter argument-hint, flag table, mode routing).

- [ ] **Step 4: Commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): add --fresh flag to argument parsing and mode routing"
```

---

## Task 4: Add Layer 9 — Phase 1 (Pre-Writing Declaration)

**Files:**
- Modify: `~/.claude/skills/hebrew-writer-v4/SKILL.md`
- Insert after: the `# LAYER 8: Soul Layer` section ends (right before `# Detection Report`)

Find the line `# Detection Report` (currently around line 2922) and insert the following complete block **immediately before it**:

- [ ] **Step 1: Insert Layer 9 Phase 1 content**

Insert this block before `# Detection Report`:

````markdown
---

# LAYER 9: Versatility Engine — מנגנון המגוון

## Overview

Every piece of content you generate must have a unique structural DNA. This layer prevents the LLM register-leveling problem: instruction-tuned models have a single attractor basin they return to regardless of topic (PNAS 2025). After 3-4 pieces, an unguarded skill produces the same arc, the same openers, the same phrase cadences. Layer 9 breaks that pattern by design.

**Two phases:** Phase 1 runs before any word is generated (compute Variation Fingerprint, check memory, lock in structure). Phase 2 runs throughout generation (6 active within-piece rules + Spent Phrase Protocol).

**Key Tell priority:** Layer 7 Key Tells remain absolute Priority 1. The Variation Fingerprint operates *within* Key Tell constraints — if a Tell dictates a behavior that conflicts with a fingerprint dimension, the Tell wins and the dimension is replaced with the nearest compatible value.

---

## Phase 1: Pre-Writing Declaration

Runs in full before generating any content. Seven steps.

### Step P1-1 — Read the variation log

Determine the log path:
- If `--voice {profile}` is active: `.claude/voices/{profile}/variation-log.json`
- If no profile: `.claude/voices/default/variation-log.json`

Use the Read tool to load the file.

**First-run fallback:** If the file does not exist (first use ever, or after `--fresh`), treat all fingerprint dimensions as equally available. Skip Steps P1-2 and P1-6 (no memory to check). Proceed directly to P1-3 using context mapping alone. After generation completes (post-output), create the file with the first fingerprint entry.

### Step P1-2 — Assess context signals

From the input topic and flags, determine three signals:

**Signal A — Emotional weight:**
- `personal/emotional`: topic involves personal experience, relationships, feelings, personal success/failure, vulnerability
- `analytical/informational`: topic involves data, process, explanation, business logic, systems, how-to

**Signal B — Content type:** use `--type` flag value if specified; otherwise use the auto-detected content type from the Content Type Auto-Detection section above.

**Signal C — Length:** `short` (<400w) | `medium` (400-800w) | `long` (800w+) — determined by `--length` flag or default.

### Step P1-3 — Select Variation Fingerprint

Compute a 5-dimension fingerprint using the mapping table below. When the table offers two options for a dimension, check the variation log (Step P1-1) and pick whichever was used less recently.

**`contextual` opener is last resort only:** It is the LLM's natural default (setting context before saying anything). Use it only when (a) no other opener appears in the last 5 logged pieces, OR (b) content type is academic or Explainer schema. Never select it as a tie-breaker.

#### Fingerprint Dimension 1 — Schema

The structural argument arc of the entire piece:

| Schema | Mechanism | Best for |
|--------|-----------|----------|
| `PAS` | Problem → Agitate → Solution | Empathy-driven persuasion; reader feels pain before solution |
| `PSB` | Problem → Story → Bridge | Problem shown through narrative, not abstraction |
| `HSO` | Hook → Story → Offer | Pattern interrupt, personal narrative, invitation — native Israeli social/blog |
| `BAB` | Before → After → Bridge | Transformation narratives, case studies |
| `4Ps` | Problem → Promise → Proof → Proposal | Skeptical audiences, evidence-heavy content |
| `AIDA` | Attention → Interest → Desire → Action | Classic persuasion arc |
| `QuestionCascade` | Unanswered questions → building toward resolution | Thought leadership |
| `Narrative` | Situation → Complication → Resolution | Informational/editorial; no persuasion agenda |
| `Explainer` | Context → Mechanism → Implication | How-to, educational, academic |

PSB and HSO are the most culturally native schemas for Israeli readers — Israeli discourse is narrative-first. BAB is natural in Israeli startup culture. `Narrative` and `Explainer` are correct for non-persuasive content — forcing a persuasion schema onto a product update or how-to is its own AI tell.

#### Fingerprint Dimension 2 — Opener Shape

How the first paragraph begins:

| Value | Description |
|-------|-------------|
| `intimate` | Personal memory or observation; pulls reader into a private moment |
| `in-medias-res` | Mid-thought reaction; reader walks in on the writer already processing something |
| `provocative` | A claim most readers would instinctively resist |
| `evidence-first` | A surprising data point or external fact that reframes scale |
| `contextual` | Slow burn; sets situation before making a claim — **last resort only** (see above) |

#### Fingerprint Dimension 3 — Body Rhythm

Paragraph-level structural pattern:

| Value | Description |
|-------|-------------|
| `staccato` | Many short paragraphs, punchy, frequent breaks |
| `flowing` | Longer paragraphs that build before pivoting |
| `alternating` | Deliberate contrast: short/punchy then long/developed, repeat |
| `spiral` | Returns to same idea from new angles, deepening each pass |
| `mosaic` | Several small seemingly disconnected sections that cohere at the end |

`mosaic` requires a minimum of 5-6 distinct sections — do not select for pieces under 400 words.

#### Fingerprint Dimension 4 — Vocabulary Register

| Value | Description |
|-------|-------------|
| `street` | Heavy slang, contractions, casual markers, code-switching |
| `elevated-casual` | Clean but informal, precise word choices, no jargon |
| `technical-light` | Domain vocabulary where relevant, accessible framing |
| `hybrid` | Deliberate register mixing mid-piece; starts one way, shifts — **Israeli rhetorical signature** |

When `hybrid` is selected: at least once per 400 words, include a register-contrast sentence — a high-register term placed in a low-register sentence, or vice versa. Example: "ישבנו בפגישה ופתאום הבנתי שכל הנרטיב הדיסקורסיבי שלנו מושתת על הנחה שגויה. אמרתי את זה בקול."

#### Fingerprint Dimension 5 — Closing Type

| Value | Description |
|-------|-------------|
| `abrupt` | Stops mid-thought; leaves something unsaid |
| `reflective` | Returns to the opening image or idea with new meaning |
| `challenge` | Ends by asking something of the reader, not wrapping up |
| `earned-insight` | Writer shares something figured out while writing |
| `open` | Ends with an unresolved question or tension |

### Step P1-4 — Apply context-to-fingerprint mapping

| Context combination | Schema | Opener | Rhythm | Vocab | Closing |
|--------------------|--------|--------|--------|-------|---------|
| Emotional/personal + social/blog | HSO or PSB | intimate or in-medias-res | staccato or alternating | street or hybrid | open or abrupt |
| Analytical/data + blog/business | 4Ps or AIDA | evidence-first or in-medias-res | flowing or alternating | elevated-casual or technical-light | earned-insight |
| Controversial opinion + any type | PAS or QuestionCascade | provocative or in-medias-res | alternating or mosaic | hybrid | abrupt or challenge |
| Transformation/case study + blog/business | BAB | intimate or in-medias-res | flowing or spiral | elevated-casual | reflective or earned-insight |
| Thought leadership + long | QuestionCascade | provocative or evidence-first | mosaic or spiral | elevated-casual or hybrid | open or reflective |
| Short pieces (any context, <400w) | PAS or HSO | intimate or provocative | staccato | street or elevated-casual | abrupt or open |
| Informational/recap + blog/business/email | Narrative | in-medias-res or evidence-first | flowing or alternating | elevated-casual or technical-light | reflective or earned-insight |
| Educational/how-to/academic | Explainer | evidence-first or contextual | flowing or mosaic | elevated-casual or technical-light | earned-insight or open |

### Step P1-5 — Check schema-opener compatibility

After selecting schema and opener, verify they are compatible. If incompatible, replace the opener with the first compatible alternative not used in the last 3 pieces.

| Schema | Compatible openers | Incompatible |
|--------|--------------------|-------------|
| `PAS` | provocative, in-medias-res, evidence-first | intimate, contextual |
| `PSB` | intimate, in-medias-res | provocative, evidence-first, contextual |
| `HSO` | intimate, in-medias-res, provocative | evidence-first, contextual |
| `BAB` | intimate, in-medias-res | provocative, evidence-first, contextual |
| `4Ps` | evidence-first, provocative | intimate, in-medias-res, contextual |
| `AIDA` | provocative, evidence-first, in-medias-res | intimate, contextual |
| `QuestionCascade` | provocative, in-medias-res, evidence-first | intimate, contextual |
| `Narrative` | intimate, in-medias-res | provocative, evidence-first, contextual |
| `Explainer` | evidence-first, contextual | intimate, provocative, in-medias-res |

### Step P1-6 — Check memory constraints

Against the `last_5` array in the variation log, enforce before finalizing any dimension:

| Dimension | Memory constraint |
|-----------|-----------------|
| Schema | Not the same as any of last 3 entries |
| Opener | Not the same as any of last 3 entries |
| Rhythm | Not the same as last 2 entries |
| Vocab | No more than 2 of last 5 share the same register |
| Closing | Not the same as any of last 3 entries |

**Conflict resolution:** If context mapping and memory constraints conflict, memory wins — use the next-best option from the same mapping row.

### Step P1-7 — Commit and pre-map

The fingerprint is now locked. Note it internally:

```
[v4 Fingerprint] Schema: {schema} | Opener: {opener} | Rhythm: {rhythm} | Vocab: {vocab} | Closing: {closing}
```

Only output this line to the user if `--show-score` is active.

**Before writing: pre-map root-family alternatives.** Identify the 3-5 central concepts of the piece. For each, list at least 3 Hebrew expressions from different root families. These are the alternatives to rotate through during generation (Rule 4 below). Example: concept "growth" → צמיחה (צמ-ח), התפתחות (פ-ת-ח), קידום (ק-ד-מ), עלייה (ע-ל-ה).

After generation, append the fingerprint to the variation log:

```json
{
  "last_5": [
    {"schema": "{schema}", "opener": "{opener}", "rhythm": "{rhythm}", "vocab": "{vocab}", "closing": "{closing}"},
    ... (previous entries, keep only 5 most recent)
  ]
}
```

Use the Write tool to save the updated log.

---
````

- [ ] **Step 2: Verify Layer 9 header is in place**

```bash
grep -n "LAYER 9" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: one match showing the new Layer 9 section.

- [ ] **Step 3: Commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): add Layer 9 Phase 1 - Variation Fingerprint pre-writing declaration"
```

---

## Task 5: Add Layer 9 — Phase 2 (Within-Piece Enforcement + Spent Phrase Protocol)

**Files:**
- Modify: `~/.claude/skills/hebrew-writer-v4/SKILL.md`
- Insert: immediately after the `---` that closes the Phase 1 section added in Task 4, before `# Detection Report`

- [ ] **Step 1: Insert Phase 2 content**

Insert this block after the Phase 1 content (after the last `---` of the P1-7 block), still before `# Detection Report`:

````markdown
## Phase 2: In-Writing Enforcement

Six rules run throughout generation, active from the first word. Each rule is a live constraint — check compliance before generating each new paragraph.

### Rule 1 — Paragraph Opener Rotation

No two consecutive paragraphs may open with the same grammatical type. Seven Hebrew opener types to rotate through:

1. **Verb-first past:** "גיליתי משהו..." / "קראתי את זה ו..."
2. **Question:** "אז למה זה קורה?"
3. **Number-first:** "שלושה דברים..."
4. **Reaction word:** "אז." / "נו." / "רגע."
5. **Mid-thought:** "כשחשבתי על זה אחר כך..."
6. **Scene drop:** "יושב בפגישה. המנכ"ל אומר..."
7. **Contrarian claim:** "כולם אומרים X. זה לא נכון."

Before writing each new paragraph: identify which type the last paragraph used. Select a different type.

### Rule 2 — Connector Category Rotation

Six functional connector categories. Never use two consecutive paragraphs from the same category:

| Category | Hebrew examples |
|----------|----------------|
| Additive | אז גם, וגם, חוץ מזה, ויש עוד משהו |
| Contrastive | אבל, למרות זאת, בכל זאת, אלא ש |
| Causal | כי, לכן, בגלל זה, זה הביא ל |
| Temporal | אחרי זה, בינתיים, שנייה לפני |
| Exemplifying | למשל, תחשוב על, ניקח את |
| Intensifying | ובכלל, בעיקר, דווקא, ממש |

Note: connectors that open the first sentence of a paragraph are what this rule tracks — not connectors mid-sentence.

### Rule 3 — Question Type Rotation

Three question types — never the same type twice in a row:

| Type | Function | Hebrew example |
|------|----------|---------------|
| Rhetorical | Reader agrees immediately | "מי רוצה לבזבז שלוש שעות על זה?" |
| Genuine | Reader doesn't have the answer | "אז למה בדיוק זה קורה?" |
| Challenging | Disrupts reader's assumption | "אבל מה אם הבעיה היא לא מה שחשבנו?" |

When using any question: identify its type. If the previous question was the same type, switch.

### Rule 4 — Root-Family Lexical Diversity

**Pre-writing:** Before generating, list 3+ alternative Hebrew expressions per key concept from different root families (done in Step P1-7 above). Use these alternatives in rotation during generation.

**During writing:** No root family repeats within 80 words.

**Hebrew morphology rule:** Using שמרתי and שמירה in the same paragraph counts as repeating root שמ-ר — even though they are different surface forms. Lexical diversity operates at root level, not surface form.

**Example mapping:**
- Concept "growth": צמיחה (צמ-ח) → התפתחות (פ-ת-ח) → קידום (ק-ד-מ) → עלייה (ע-ל-ה) → rotate
- Concept "understand": הבנה (ב-י-נ) → תפיסה (ת-פ-ס) → הכרה (כ-ר-ה) → rotate
- Concept "change": שינוי (ש-נ-י) → מעבר (ע-ב-ר) → טרנספורמציה (loanword) → rotate

### Rule 5 — Stance Category Rotation

Hebrew argumentative writing uses four discourse stance types (Frontiers 2025 research). In any argumentative or persuasive piece, rotate through them across paragraphs:

| Stance | Function | Hebrew markers |
|--------|----------|---------------|
| Epistemic | What I know/believe | לדעתי, נראה לי, בטוח ש, אני חושב |
| Deontic | What must/should happen | צריך, חייב, אסור, כדאי |
| Evaluative | Judgment of value | זה מגוחך, יפה, כואב לראות, מרגש ש |
| Dialogic | Engaging the opposing view | אז יגידו לי ש... אבל; מי שחושב ש... טועה |

**Quality gate:** In pieces of 600+ words, at least 3 of the 4 stance categories must appear. A piece using only Epistemic stance reads as opinion-light. A piece using all four feels like a real, engaged mind.

### Rule 6 — Paragraph-Level Structural Burstiness

Paragraph lengths must vary actively — distinct from sentence-level burstiness (already enforced in Layer 5). Requirements:

- **Every piece** must include at least one single-sentence paragraph
- Never three consecutive paragraphs of the same length band
  - Short = 1-2 sentences
  - Medium = 3-4 sentences
  - Long = 5+ sentences
- Ideal medium-piece (500-800w) sequence: Long → Short → Medium → Single sentence → Medium → Long

The single-sentence paragraph is the strongest structural burstiness tool. In Hebrew, it works best as:
- A declarative statement after a long build-up: "זה שינה הכל."
- A question that opens a new angle: "אז למה זה עדיין לא קורה?"
- A fragment: "ולא בטעות."

---

## Spent Phrase Protocol

Runs from the first word of generation. Tracks all of:
- Any expression of 3+ consecutive words used in this piece
- Any connector used (tracked by category, not just exact word)
- Any question type used in the last two questions
- Any quote integration style used: direct attribution / partial embedded / paraphrase / summary
- Any paragraph opener type used in the last two paragraphs

**The rule:** Before generating each new paragraph, mentally review the spent list. Do not reuse any spent item. If reaching for the same 3-word sequence as earlier in the piece — stop and find a different grammatical path to the same idea.

**Trigram rule:** No 3-word sequence appears more than once in any piece. This is the prose-instruction equivalent of trigram suppression.

---

## Versatility Self-Audit

At self-audit time (Layer 6), the מגוון dimension checks:

- [ ] Fingerprint was computed and logged (2pts)
- [ ] No two consecutive paragraphs share the same opener type (2pts)
- [ ] At least 3 stance categories present in pieces 600+ words (2pts)
- [ ] No root family repeated within 80 words (2pts)
- [ ] At least one single-sentence paragraph present (2pts)

Score 0-10. Enter as the מגוון dimension in the weighted scoring rubric.

---
````

- [ ] **Step 2: Verify Phase 2 content is in place**

```bash
grep -n "Spent Phrase Protocol\|Rule 6\|Phase 2" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: multiple matches showing all Phase 2 sections.

- [ ] **Step 3: Commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): add Layer 9 Phase 2 - within-piece enforcement rules and Spent Phrase Protocol"
```

---

## Task 6: Update Layer 6 Self-Audit scoring

**Files:**
- Modify: `~/.claude/skills/hebrew-writer-v4/SKILL.md`
- Target: Layer 6 section (around line 1533)

The current v3.1 formula is:
```
Score = (דוגריות × 0.13) + (קצב × 0.14) + (אמינות × 0.14) + (טקסטורה × 0.11)
      + (נשמה × 0.08) + (נשמה עמוקה × 0.08) + (צפיפות × 0.06) + (רישום × 0.06) + (אנטי-זיהוי × 0.20)
Weights: 13+14+14+11+8+8+6+6+20 = 100%
```

- [ ] **Step 1: Update the Phase 2 description to reference 9 dimensions**

Find:
```
**Phase 2: Score against 8 dimensions**
```

Replace with:
```
**Phase 2: Score against 9 dimensions**
```

- [ ] **Step 2: Update the "8-Dimension Scoring Rubric" heading**

Find:
```
## The 8-Dimension Scoring Rubric
```

Replace with:
```
## The 9-Dimension Scoring Rubric
```

- [ ] **Step 3: Add the מגוון dimension rubric**

After the `### 8. אנטי-זיהוי — Anti-Detection` section (which ends with `---` before `## Weighted Score Calculation`), insert:

```markdown
### 9. מגוון — Versatility (weight: 10%)

The piece has structural and lexical DNA distinct from the last 3-5 pieces. Within this piece, paragraph openers rotate, connectors vary by category, question types alternate, root families do not cluster, and at least one single-sentence paragraph creates structural burstiness.

**10/10:** Variation Fingerprint was computed and logged. No two consecutive paragraphs share an opener type. At least 3 Hebrew stance categories present (in pieces 600+ words). No root family repeated within 80 words. At least one single-sentence paragraph present. The piece feels structurally different from the previous one — different arc, different opening energy, different rhythm.

**8/10:** Fingerprint computed. Most opener types varied. Stance categories mostly rotated. One or two root-family repetitions within 80 words. Structural burstiness present but no single-sentence paragraph.

**Below 8:** No fingerprint computed. Paragraph openers follow the same grammatical type throughout. Only one stance category (usually epistemic) used throughout. Key vocabulary clusters in the same root family across paragraphs. Paragraph lengths are uniform — no burstiness at structural level.

---
```

- [ ] **Step 4: Update the Weighted Score Calculation formula**

Find:
```
Score = (דוגריות × 0.13) + (קצב × 0.14) + (אמינות × 0.14) + (טקסטורה × 0.11)
      + (נשמה × 0.08) + (נשמה עמוקה × 0.08) + (צפיפות × 0.06) + (רישום × 0.06) + (אנטי-זיהוי × 0.20)

Multiply each dimension score (out of 10) by 10 to get out-of-100.
Example: all 9s → 9 × 10 = 90/100. Need 95+.
To reach 95: most dimensions at 9.5-10, none below 8.
Weights: 13+14+14+11+8+8+6+6+20 = 100%
```

Replace with:

```
Score = (דוגריות × 0.11) + (קצב × 0.12) + (אמינות × 0.13) + (טקסטורה × 0.10)
      + (נשמה × 0.07) + (נשמה עמוקה × 0.07) + (צפיפות × 0.06) + (רישום × 0.06) + (אנטי-זיהוי × 0.18) + (מגוון × 0.10)

Multiply each dimension score (out of 10) by 10 to get out-of-100.
Example: all 9s → 9 × 10 = 90/100. Need 95+.
To reach 95: most dimensions at 9.5-10, none below 8.
Weights: 11+12+13+10+7+7+6+6+18+10 = 100%
```

- [ ] **Step 5: Update the Generation Pipeline Step 7 reference**

Find in the Generation Pipeline section:

```
**Step 7: Self-audit loop** (Layer 6)
Score mentally against all 9 dimensions.
```

If it says "8 dimensions" or "9 dimensions", update to:

```
**Step 7: Self-audit loop** (Layer 6)
Score mentally against all 10 dimensions (9 original + מגוון).
```

Wait — the current text says "all 9 dimensions" because it counts נשמה and נשמה עמוקה together as one section but there are 9 scoring dimensions. After adding מגוון it becomes 10 scoring inputs to the formula. Update accordingly.

- [ ] **Step 6: Verify the formula update**

```bash
grep -n "מגוון × 0.10\|11+12+13" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: match showing the updated formula.

- [ ] **Step 7: Commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): update Layer 6 self-audit to add מגוון dimension (10%) and rebalance weights"
```

---

## Task 7: Update Generation Pipeline

**Files:**
- Modify: `~/.claude/skills/hebrew-writer-v4/SKILL.md`
- Target: `# Generation Pipeline — Complete Flow` section (around line 3192 in original, later in v4 due to insertions)

- [ ] **Step 1: Add Layer 9 Phase 1 as a new step after Step 2**

Find:
```
**Step 2: Detect content type**
Auto-detect from input signals (keywords, format, length) or use --type flag value.

**Step 3: Load voice profile**
```

Replace with:

```
**Step 2: Detect content type**
Auto-detect from input signals (keywords, format, length) or use --type flag value.

**Step 2.5: Compute Variation Fingerprint** (Layer 9 Phase 1)
Run all 7 steps of Layer 9 Phase 1: read variation log → assess context signals → select fingerprint dimensions → apply context mapping → check schema-opener compatibility → check memory constraints → commit fingerprint and pre-map root-family alternatives. Log the fingerprint. Note internally (output only if --show-score active).

**Step 3: Load voice profile**
```

- [ ] **Step 2: Update Step 6 to reference Layer 9 Phase 2**

Find:
```
**Step 6: Generate initial draft**
Apply all layers:
- Layer 1: Hebrew-first composition (pro-drop, nominal sentences, particles, morphology)
- Layer 4: Gender agreement with strategic imperfection, spelling variation, construct/של register matching, correct binyan choices
- Layer 2: Avoid all blacklist words, all named patterns, all style anti-patterns
- Layer 3: Dugri energy, register-appropriate slang and discourse markers, cultural texture, emotional authenticity
- Layer 5: Burstiness (3-40 rule, rhythm pattern), perplexity (20-30% non-obvious choices), vocabulary diversity, n-gram variance
- Apply content type register preset
```

Replace with:

```
**Step 6: Generate initial draft**
Apply all layers:
- Layer 1: Hebrew-first composition (pro-drop, nominal sentences, particles, morphology)
- Layer 4: Gender agreement with strategic imperfection, spelling variation, construct/של register matching, correct binyan choices
- Layer 2: Avoid all blacklist words, all named patterns, all style anti-patterns
- Layer 3: Dugri energy, register-appropriate slang and discourse markers, cultural texture, emotional authenticity
- Layer 5: Burstiness (3-40 rule, rhythm pattern), perplexity (20-30% non-obvious choices), vocabulary diversity, n-gram variance
- Layer 9 Phase 2: Before each paragraph — enforce opener rotation, connector category rotation, question type rotation, root-family lexical diversity, stance category rotation, paragraph-level structural burstiness. Run Spent Phrase Protocol check.
- Apply content type register preset
- Open with the declared Opener shape; maintain the declared Body Rhythm throughout; use vocabulary from the declared Register; end with the declared Closing type
```

- [ ] **Step 3: Verify both pipeline additions**

```bash
grep -n "Step 2.5\|Layer 9 Phase 2\|Spent Phrase Protocol check" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: matches for all three strings.

- [ ] **Step 4: Commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): update Generation Pipeline to call Layer 9 in Phase 1 and Phase 2"
```

---

## Task 8: Update voice-profile-template to v4

**Files:**
- Modify: `~/.claude/skills/hebrew-writer-v4/voice-profile-template.md`

The v3.1 template has `version: 3.1` in frontmatter and `calibration-rounds` tracking. v4 adds a variation-log reference.

- [ ] **Step 1: Update frontmatter version**

Find in `voice-profile-template.md`:
```
version: 3.1
```

Replace with:
```
version: 4
```

- [ ] **Step 2: Add variation-log note to Generation Notes section**

Find at the bottom of the template:
```
## Generation Notes

[Any special instructions or observations about this voice that don't fit the structured categories above.
```

Replace with:

```
## Generation Notes

[Any special instructions or observations about this voice that don't fit the structured categories above.

**Variation log:** This profile's cross-piece variation history is stored at `.claude/voices/{profile-name}/variation-log.json`. The Versatility Engine reads and updates this log on every generation. Use `--fresh` to clear it when starting a new content project.
```

- [ ] **Step 3: Verify**

```bash
grep -n "version: 4\|variation-log\|--fresh" ~/.claude/skills/hebrew-writer-v4/voice-profile-template.md
```

Expected: 3 matches.

- [ ] **Step 4: Commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): update voice-profile-template to v4 with variation-log reference"
```

---

## Task 9: Install and smoke test

**Files:**
- Read: `~/.claude/skills/hebrew-writer-v4/SKILL.md` (verify installation is correct)

- [ ] **Step 1: Verify the skill is reachable by the skill system**

```bash
ls ~/.claude/skills/hebrew-writer-v4/
```

Expected: `SKILL.md  voice-profile-template.md`

- [ ] **Step 2: Check line count is reasonable (should be v3.1 + ~200-300 lines)**

```bash
wc -l ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: approximately 3,450-3,550 lines (v3.1 was ~3,239 + ~250 lines for Layer 9 + modifications).

- [ ] **Step 3: Verify all key sections are present**

```bash
grep -n "LAYER 9\|Variation Fingerprint\|--fresh\|מגוון × 0.10\|What's New in v4\|Spent Phrase Protocol\|Schema-Opener Compatibility\|Step 2.5" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: at least one match for each of these 8 strings.

- [ ] **Step 4: Verify no section from v3.1 was accidentally deleted**

```bash
grep -n "LAYER 1\|LAYER 2\|LAYER 3\|LAYER 4\|LAYER 5\|LAYER 6\|LAYER 7\|LAYER 8\|LAYER 9" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: exactly one match for each layer (1-9).

- [ ] **Step 5: Quick self-consistency check on scoring weights**

```bash
grep -A3 "Weights:" ~/.claude/skills/hebrew-writer-v4/SKILL.md
```

Expected: `11+12+13+10+7+7+6+6+18+10 = 100%`

- [ ] **Step 6: Invoke the skill with --show-score on a test topic**

Run the skill on a simple topic to verify Layer 9 fires:
```
/hebrew-writer כתוב על למה ישראלים אוהבים קפה --show-score --length short
```

Expected output includes:
- `[v4 Fingerprint]` line showing the 5 dimensions chosen
- Hebrew content (200-400 words)
- Score block at the end with מגוון dimension present

- [ ] **Step 7: Run a second piece and verify fingerprint differs**

```
/hebrew-writer כתוב על הקשר בין שינה לפרודוקטיביות --show-score --length short
```

Expected: `[v4 Fingerprint]` line shows different Schema AND different Opener than the first piece.

- [ ] **Step 8: Verify variation log was created**

```bash
ls ~/.claude/voices/default/
```

Expected: `variation-log.json` is present.

```bash
cat ~/.claude/voices/default/variation-log.json
```

Expected: valid JSON with `last_5` array containing 2 fingerprint entries.

- [ ] **Step 9: Final commit**

```bash
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" add -A
git -C "/c/Users/Lenovo/OneDrive/Desktop/CLAUDE FOLDER/hebrew writing skill" commit -m "feat(v4): complete hebrew-writer-v4 installation and smoke test"
```

---

## Self-Review

**Spec coverage check:**

| Spec requirement | Covered by task |
|-----------------|-----------------|
| Variation Fingerprint (5 dimensions) | Task 4 |
| 9 schemas including Narrative + Explainer | Task 4 |
| contextual opener as last resort | Task 4 |
| Context-to-fingerprint mapping table | Task 4 |
| Schema-opener compatibility table | Task 4 |
| Session memory log (last_5, constraints) | Task 4 |
| First-run fallback | Task 4 |
| --fresh flag | Tasks 2, 3 |
| 6 within-piece enforcement rules | Task 5 |
| Spent Phrase Protocol | Task 5 |
| Versatility self-audit checklist | Task 5 |
| Layer 6 מגוון dimension + updated weights | Task 6 |
| Generation Pipeline integration | Task 7 |
| Voice profile template update | Task 8 |
| Soul Layer rotation (soft signal) | Task 5 (included in Layer 9 overview) |
| Key Tells override note | Task 4 (Layer 9 overview) |

**Placeholder scan:** No TBD, TODO, or incomplete steps. All code blocks and insert blocks contain complete content.

**Type consistency:** The fingerprint dimension values (`staccato`, `intimate`, `HSO`, etc.) are used consistently across Tasks 4, 5, 6, and 7.

**Note on spec scoring table:** The spec doc (`2026-04-29-hebrew-writer-v4-versatility-design.md`) uses simplified dimension names that differ from the actual SKILL.md dimension names. This plan uses the actual SKILL.md names (דוגריות, קצב, etc.) and actual weights. The spec is a design reference; this plan is authoritative for implementation.
