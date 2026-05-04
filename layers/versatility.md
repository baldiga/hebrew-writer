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

**First-run fallback:** If the file does not exist (first use ever, or after `--fresh`), treat all fingerprint dimensions as equally available. Skip Step P1-6 only (no memory to check). Proceed to P1-2 normally, then P1-3 using context mapping alone. After generation completes (post-output), create the file with the first fingerprint entry.

### Step P1-2 — Assess context signals

From the input topic and flags, determine three signals:

**Signal A — Emotional weight:**
- `personal/emotional`: topic involves personal experience, relationships, feelings, personal success/failure, vulnerability
- `analytical/informational`: topic involves data, process, explanation, business logic, systems, how-to

**Signal B — Content type:** use `--type` flag value if specified; otherwise use the auto-detected content type from the Content Type Auto-Detection section above.

**Signal C — Length:** `short` (<400w) | `medium` (400-800w) | `long` (800w+) — determined by `--length` flag or default.

### Step P1-3 — Select Variation Fingerprint

Compute a 5-dimension fingerprint using the mapping table below. When the table offers two options for a dimension, check the variation log (Step P1-1) and pick whichever was used less recently. If both options were used equally recently (or neither appears in the log), choose the option listed first in the table.

**`contextual` opener is last resort only:** It is the LLM's natural default (setting context before saying anything). Use it only when (a) no other opener appears in the last 5 logged pieces, OR (b) content type is `academic`, OR the selected schema is `Explainer`. Never select it as a tie-breaker.

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
| Controversial opinion + any type | PAS or QuestionCascade | provocative or in-medias-res | alternating or mosaic (mosaic only if medium or long) | hybrid | abrupt or challenge |
| Transformation/case study + blog/business | BAB | intimate or in-medias-res | flowing or spiral | elevated-casual | reflective or earned-insight |
| Thought leadership + long | QuestionCascade | provocative or evidence-first | mosaic or spiral | elevated-casual or hybrid | open or reflective |
| Short pieces (any context, <400w) | PAS or HSO | intimate or provocative | staccato | street or elevated-casual | abrupt or open |
| Informational/recap + blog/business/email | Narrative | in-medias-res or evidence-first | flowing or alternating | elevated-casual or technical-light | reflective or earned-insight |
| Educational/how-to/academic | Explainer | evidence-first or contextual | flowing or mosaic (mosaic only if medium or long) | elevated-casual or technical-light | earned-insight or open |

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

**Array indexing:** When checking "last 3 entries", use only the 3 most recent entries in the `last_5` array (index 0 through 2, where index 0 is the most recent).

**Conflict resolution:** If context mapping and memory constraints conflict, memory wins — use the next-best option from the same mapping row. If the topic matches multiple mapping rows, use the first matching row for fallback alternatives.

### Step P1-7 — Commit and pre-map

The fingerprint is now locked. Note it internally:

```
[v4 Fingerprint] Schema: {schema} | Opener: {opener} | Rhythm: {rhythm} | Vocab: {vocab} | Closing: {closing}
```

Only output this line to the user if `--show-score` is active.

**Before writing: pre-map root-family alternatives.** Identify the 3-5 central concepts of the piece. For each, list at least 3 Hebrew expressions from different root families. These are the alternatives to rotate through during generation (Rule 4 in Phase 2 below — root-family lexical diversity enforcement). Example: concept "growth" → צמיחה (צמ-ח), התפתחות (פ-ת-ח), קידום (ק-ד-מ), עלייה (ע-ל-ה).

**Post-generation (run after output):** Append the fingerprint to the variation log:

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

**Rule 1 priority:** When Rule 1 has already determined the opener type for a paragraph (e.g., "Reaction word" — אז), Rule 2 accepts whatever connector category that implies. Rule 2 tracking then restarts from the *next* paragraph. Rule 1 always takes priority on opener selection; Rule 2 enforces variety across paragraphs where Rule 1 hasn't already constrained the choice.

### Rule 3 — Question Type Rotation

Three question types — never the same type twice in a row:

| Type | Function | Hebrew example |
|------|----------|---------------|
| Rhetorical | Reader agrees immediately | "מי רוצה לבזבז שלוש שעות על זה?" |
| Genuine | Reader doesn't have the answer | "אז למה בדיוק זה קורה?" |
| Challenging | Disrupts reader's assumption | "אבל מה אם הבעיה היא לא מה שחשבנו?" |

When using any question: identify its type. If the previous question was the same type, switch.

**Activation threshold:** This rule only applies when 2 or more questions appear in the piece. If the piece uses only one question (or none), Rule 3 is inactive.

### Rule 4 — Root-Family Lexical Diversity

**Pre-writing:** Before generating, list 3+ alternative Hebrew expressions per key concept from different root families (done in Step P1-7 above). Use these alternatives in rotation during generation.

**During writing:** No root family repeats within 80 words. The window slides continuously — it does not reset at paragraph breaks.

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

**Short pieces (<600 words):** Rotation still applies paragraph-to-paragraph. The formal 3-of-4 gate does not. Aim for at least 2 different stance categories across the piece.

### Rule 6 — Paragraph-Level Structural Burstiness

Paragraph lengths must vary actively — distinct from sentence-level burstiness (already enforced in Layer 5). Requirements:

- **Every piece** must include at least one single-sentence paragraph
- Never three consecutive paragraphs of the same length band
  - Short = 1-2 sentences
  - Medium = 3-4 sentences
  - Long = 5+ sentences
- Ideal short-piece (<400w) sequence: Medium → Single sentence → Long
- Ideal medium-piece (500-800w) sequence: Long → Short → Medium → Single sentence → Medium → Long
- Ideal long-piece (800w+) sequence: Medium → Long → Single sentence → Long → Short → Medium → Long → Single sentence

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

**Relationship to Rules 1-3:** The Spent Phrase Protocol supplements, not replaces, Rules 1-3. Rules 1-3 are the primary enforcers (applied actively during generation). The Protocol is a final sweep before each paragraph — a catch-all for anything the explicit rules may miss.

---

## Versatility Self-Audit

At self-audit time (Layer 6), the מגוון dimension checks:

- [ ] Fingerprint was computed and logged (2pts)
- [ ] No two consecutive paragraphs share the same opener type (2pts)
- [ ] At least 3 stance categories present in pieces 600+ words (2pts)
- [ ] No root family repeated within 80 words (2pts)
- [ ] At least one single-sentence paragraph present (2pts)

*Rules 2 (connector rotation), Rule 3 (question type rotation), and the trigram rule are enforced during writing. They are not audited post-hoc because they require re-reading the full piece mid-stream — enforce them in real-time per their rule definitions above.*

Score 0-10. Enter as the מגוון dimension in the Layer 6 weighted scoring rubric (see Layer 6 below — the rubric and formula are updated to include מגוון).

---

