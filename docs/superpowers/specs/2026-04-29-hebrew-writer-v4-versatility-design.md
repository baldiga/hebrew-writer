# Hebrew Writer v4 — Versatility Engine Design Spec

**Date:** 2026-04-29
**Status:** Approved
**Scope:** Additive — Layer 9 added to existing 8-layer system. Layers 1-8 unchanged.

---

## Problem Statement

After extended use of v3.1, the skill produces content that feels repetitive across pieces and within single pieces. Specifically:

- Opening structures follow the same pattern every time
- The same buzzwords and phrases recur across posts
- Paragraph rhythm is monotonous (same length distribution)
- Questions cluster into the same functional type
- Quote integration and stance markers lack variety

This is a known LLM failure mode: instruction-tuned models have a single attractor basin they return to regardless of prompt (PNAS 2025, Reinhart et al.; CMU arxiv 2604.14111). v4 destabilizes this basin by prescribing structurally incompatible schemas across pieces and enforcing within-piece pattern rotation.

**Research basis:** PNAS 2025 "Do LLMs Write Like Humans?", Antislop ICLR 2026 (8,000+ pattern taxonomy), MATTR lexical diversity research (Bestgen 2024), Hebrew argumentative discourse stance (Frontiers 2025), IsraParlTweet corpus (LREC-COLING 2024), avoid-ai-writing trigram suppression system (GitHub), lechmazur/writing_styles benchmark.

---

## Design Goals

1. **Automatic** — no user flags required. Variation is computed and enforced without user intervention.
2. **Context-aware** — fingerprint selection is driven by topic emotional weight, content type, and length.
3. **Memory-aware** — session state ensures cross-piece variety. Each new piece is guaranteed to differ meaningfully from the last 3-5.
4. **Non-destructive** — Key Tells (Layer 7) are never overridden. The fingerprint operates within the user's voice constraints.
5. **Measurable** — added to self-audit scoring (Layer 6).

---

## Architecture: Two-Phase Operation

### Phase 1 — Pre-Writing Declaration

Runs before any content is generated.

1. Assess three context signals from the input
2. Compute the Variation Fingerprint (5 dimensions)
3. Check session memory log; adjust fingerprint if constraints violated
4. Note the fingerprint (internal scaffolding — not output to user unless `--show-score` is active)
5. Log the fingerprint to `.claude/voices/{profile}/variation-log.json`

### Phase 2 — In-Writing Enforcement

Runs throughout generation.

Six active rules enforce within-piece variation. A Spent Phrase Protocol tracks used patterns per piece.

---

## The Variation Fingerprint

A 5-dimension profile assigned once per piece before writing begins.

### Dimension 1 — Schema

The structural argument arc of the whole piece. One of:

| Schema | Mechanism | Best for |
|--------|-----------|----------|
| `PAS` | Problem → Agitate → Solution | Empathy-driven persuasion; reader feels pain before solution |
| `PSB` | Problem → Story → Bridge | Problem illustrated through narrative, not abstraction |
| `HSO` | Hook → Story → Offer | Pattern interrupt, personal narrative, invitation — native Israeli social media |
| `BAB` | Before → After → Bridge | Transformation narratives, case studies |
| `4Ps` | Problem → Promise → Proof → Proposal | Skeptical audiences, evidence-heavy content |
| `AIDA` | Attention → Interest → Desire → Action | Classic persuasion arc |
| `QuestionCascade` | Opens with unanswered questions, builds toward resolution | Thought leadership |

**Israeli note:** PSB and HSO are the most culturally native schemas for Israeli readers — Israeli discourse is narrative-first. Pure AIDA (claim-first) reads as American copywriting. BAB is natural in Israeli startup culture.

### Dimension 2 — Opener Shape

How the first paragraph begins. One of:

| Value | Description |
|-------|-------------|
| `intimate` | Personal memory or observation; pulls reader into a private moment |
| `in-medias-res` | Mid-thought reaction; reader walks in on the writer already processing something |
| `provocative` | A claim most readers would instinctively resist |
| `evidence-first` | A surprising data point or external fact that reframes scale |
| `contextual` | Slow burn; sets situation before making a claim |

### Dimension 3 — Body Rhythm

Paragraph-level structural pattern. One of:

| Value | Description |
|-------|-------------|
| `staccato` | Many short paragraphs, punchy, frequent breaks |
| `flowing` | Longer paragraphs that build before pivoting |
| `alternating` | Deliberate contrast: short/punchy then long/developed, repeat |
| `spiral` | Returns to same idea from new angles, deepening each pass |
| `mosaic` | Several small seemingly disconnected sections that cohere at the end |

### Dimension 4 — Vocabulary Register

Lexical flavor of this piece. One of:

| Value | Description |
|-------|-------------|
| `street` | Heavy slang, contractions, casual markers, code-switching |
| `elevated-casual` | Clean but informal, precise word choices, no jargon |
| `technical-light` | Domain vocabulary where relevant, accessible framing |
| `hybrid` | Deliberate register mixing mid-piece; starts one way, shifts |

**Hebrew-specific:** The `hybrid` register — placing a high-register word in a low-register sentence or vice versa — is a documented signature of Israeli rhetorical style (Israeli sports journalism research, Academia.edu). This is what PNAS 2025 found LLMs systematically cannot do. The register-contrast sentence (at least once per 400 words) is a specific manifestation of this.

### Dimension 5 — Closing Type

How the piece ends. One of:

| Value | Description |
|-------|-------------|
| `abrupt` | Stops mid-thought; leaves something unsaid |
| `reflective` | Returns to the opening image or idea with new meaning |
| `challenge` | Ends by asking something of the reader, not wrapping up |
| `earned-insight` | Writer shares something figured out while writing |
| `open` | Ends with an unresolved question or tension |

---

## Context-to-Fingerprint Mapping

Before assigning fingerprint values, assess three signals:

**Signal A — Emotional weight:**
- Personal/emotional: the topic involves personal experience, relationships, feelings, personal failure/success
- Analytical/informational: the topic involves data, process, explanation, business logic

**Signal B — Content type:** social | blog | business | email | creative | academic

**Signal C — Length:** short (<400w) | medium (400-800w) | long (800w+)

**Mapping table:**

| Context combination | Schema | Opener | Rhythm | Vocab | Closing |
|--------------------|--------|--------|--------|-------|---------|
| Emotional/personal + social/blog, any length | HSO or PSB | intimate or in-medias-res | staccato or alternating | street or hybrid | open or abrupt |
| Analytical/data + blog/business, medium/long | 4Ps or AIDA | evidence-first or contextual | flowing or alternating | elevated-casual or technical-light | earned-insight |
| Controversial opinion + any type, any length | PAS or QuestionCascade | provocative or in-medias-res | alternating or mosaic | hybrid | abrupt or challenge |
| Transformation/case study + blog/business | BAB | intimate or contextual | flowing or spiral | elevated-casual | reflective or earned-insight |
| Thought leadership + long | QuestionCascade | provocative or evidence-first | mosaic or spiral | elevated-casual or hybrid | open or reflective |
| Short pieces (any context) | PAS or HSO | intimate or provocative | staccato | street or elevated-casual | abrupt or open |

**Tie-breaking rule:** When the mapping offers two options, session memory decides — whichever was used less recently wins.

---

## Session Memory

**Storage location:** `.claude/voices/{profile}/variation-log.json`
If no profile is active: `.claude/voices/default/variation-log.json`

**Log format:**
```json
{
  "last_5": [
    {"schema": "HSO", "opener": "intimate", "rhythm": "staccato", "vocab": "street", "closing": "open"},
    {"schema": "4Ps", "opener": "evidence-first", "rhythm": "flowing", "vocab": "elevated-casual", "closing": "earned-insight"},
    {"schema": "PAS", "opener": "provocative", "rhythm": "alternating", "vocab": "hybrid", "closing": "abrupt"},
    {"schema": "BAB", "opener": "contextual", "rhythm": "spiral", "vocab": "elevated-casual", "closing": "reflective"},
    {"schema": "QuestionCascade", "opener": "in-medias-res", "rhythm": "mosaic", "vocab": "hybrid", "closing": "open"}
  ]
}
```

**Constraints enforced before finalizing fingerprint:**

| Dimension | Constraint |
|-----------|------------|
| Schema | Not the same as any of last 3 pieces |
| Opener | Not the same as any of last 3 pieces |
| Rhythm | Not the same as last 2 pieces |
| Vocab | No more than 2 of last 5 share the same register |
| Closing | Not the same as any of last 3 pieces |

**Conflict resolution:** If context mapping and memory constraints conflict, memory wins — use the next-best option from the same mapping category.

**`--fresh` flag:** Clears the variation log entirely. Use when starting a new content project where variety relative to prior pieces doesn't matter.

---

## Within-Piece Enforcement Rules

Six rules run throughout generation.

### Rule 1 — Paragraph Opener Rotation

No two consecutive paragraphs may open with the same grammatical type. Seven Hebrew opener types to rotate through:

1. **Verb-first past:** "גיליתי משהו..." / "קראתי את זה ו..."
2. **Question:** "אז למה זה קורה?"
3. **Number-first:** "שלושה דברים..."
4. **Reaction word:** "אז." / "נו." / "רגע."
5. **Mid-thought:** "כשחשבתי על זה אחר כך..."
6. **Scene drop:** "יושב בפגישה. המנכ"ל אומר..."
7. **Contrarian claim:** "כולם אומרים X. זה לא נכון."

### Rule 2 — Connector Category Rotation

Six functional connector categories. Never use two consecutive paragraphs from the same category.

| Category | Hebrew examples |
|----------|----------------|
| Additive | אז גם, וגם, חוץ מזה, ויש עוד משהו |
| Contrastive | אבל, למרות זאת, בכל זאת, אלא ש |
| Causal | כי, לכן, בגלל זה, זה הביא ל |
| Temporal | אחרי זה, בינתיים, שנייה לפני |
| Exemplifying | למשל, תחשוב על, ניקח את |
| Intensifying | ובכלל, בעיקר, דווקא, ממש |

### Rule 3 — Question Type Rotation

Three question types — never the same type twice in a row:

| Type | Function | Example |
|------|----------|---------|
| Rhetorical | Reader agrees immediately | "מי רוצה לבזבז שלוש שעות על זה?" |
| Genuine | Reader doesn't have the answer | "אז למה בדיוק זה קורה?" |
| Challenging | Disrupts the reader's assumption | "אבל מה אם הבעיה היא לא מה שחשבנו?" |

### Rule 4 — Root-Family Lexical Diversity (Hebrew-specific)

Before writing, pre-map at least 3 alternative Hebrew expressions for each central concept of the piece — from different root families, not just inflections of the same root. Rotate through these alternatives during generation. No root family may repeat within 80 words.

**Example:** Concept "growth" → צמיחה (צמ-ח), התפתחות (פ-ת-ח), קידום (ק-ד-מ), עלייה (ע-ל-ה), התרחבות (ר-ח-ב)

**Hebrew morphology note:** Using שמרתי and שמירה in the same paragraph counts as repetition of the root שמ-ר even though they are different surface forms. Lexical diversity operates at the root level in Hebrew.

### Rule 5 — Stance Category Rotation

Hebrew argumentative writing uses four discourse stance types (Frontiers 2025 research). Rotate through them across paragraphs in any argumentative or persuasive piece:

| Stance | Function | Hebrew markers |
|--------|----------|---------------|
| Epistemic | What I know/believe | לדעתי, נראה לי, בטוח ש, אני חושב |
| Deontic | What must/should happen | צריך, חייב, אסור, כדאי |
| Evaluative | Judgment of value | זה מגוחך, יפה, כואב לראות, מרגש ש |
| Dialogic | Engaging the opposing view | אז יגידו לי ש... אבל; מי שחושב ש... טועה |

A piece that uses only epistemic stance reads as opinion-light. A piece using all four feels like a real, engaged mind. In pieces of 600+ words, at least 3 of the 4 categories must appear.

### Rule 6 — Paragraph-Level Structural Burstiness

Paragraph lengths must vary actively — not just sentences. Requirements:
- Every piece must include at least one single-sentence paragraph
- Never three consecutive paragraphs of the same length band
- Ideal medium-piece sequence: Long (5-7 sentences) → Short (1-2) → Medium (3-4) → Single sentence → Medium → Long

The single-sentence paragraph is the most powerful structural burstiness tool. It creates emphasis through isolation. In Hebrew, it works best as a declarative statement after a long build-up, or as a question that opens a new angle.

---

## Spent Phrase Protocol

Per-piece internal tracker that runs from the first word of generation.

**What gets tracked as "spent":**
- Any expression of 3+ words
- Any connector used (by category, not just exact word)
- Any question type used
- Any quote integration style used (direct with attribution / partial quote embedded / paraphrase / summary attribution)
- Any paragraph opener type used in the last 2 paragraphs

**The rule:** Before generating each new paragraph, mentally review what is spent. Do not reuse any spent item. If you catch yourself reaching for the same three-word sequence, stop and find a different grammatical path to the same idea.

**Trigram rule:** No three-word sequence may appear more than once in any piece. This is the prose-instruction equivalent of the trigram suppression system in avoid-ai-writing (conorbronsdon).

---

## Integration with Existing Layers

### Layer 5 (Rhythm Engineering)
Extends from sentence-level to paragraph-level burstiness. Rule 6 above is additive to existing sentence-length variance rules. No conflict possible.

### Layer 6 (Self-Audit)
The 95/100 threshold stays unchanged. v4 adds a **Versatility sub-score (10 points)**:
- 5 existing dimensions each reduced by 1 point → releases 5 points (קול ישראלי, אנטי-AI, פרוזודיה, נשמה, נשמה עמוקה — each -1)
- 5 new points added to the scoring system
- Total: 10 points for the new מגוון (Versatility) dimension; overall max rises from 95 to 100

**Versatility sub-score checklist (10 pts):**
- [ ] Fingerprint computed and logged (2pts)
- [ ] No two consecutive paragraphs share opener type (2pts)
- [ ] At least 3 stance categories present in pieces 600+ words (2pts)
- [ ] No root family repeated within 80 words (2pts)
- [ ] Paragraph-level burstiness achieved (single-sentence paragraph present) (2pts)

### Layer 7 (Voice Cloning / Key Tells)
Key Tells remain absolute Priority 1. The fingerprint operates *within* Key Tell constraints.

**Conflict resolution:** If a Key Tell dictates a particular behavior that conflicts with a fingerprint dimension, Key Tell wins. The fingerprint dimension is overridden with the closest compatible value. Example: if the user's Key Tell is "always opens with a question," the Opener dimension is locked to `provocative` or `in-medias-res` (question variants) — not a free override of the Tell.

### Layer 8 (Soul Layer)
All 20 Soul techniques remain available. v4 adds a soft rotation signal: across pieces, shift which Soul techniques are foregrounded. If the last piece leaned on Specificity Injection and Memory Drop, the next piece should foreground Vulnerability and Dialogic Stance techniques. No formal tracking required — this is a generation intention, not a logged constraint.

---

## New Flag

`--fresh`
- No value required
- Effect: clears `.claude/voices/{profile}/variation-log.json` (or default log)
- Use case: starting a new content project where variety relative to prior pieces is irrelevant
- Added to argument parsing section alongside existing flags

---

## Self-Audit Scoring Adjustment (Full Updated Breakdown)

| Dimension | v3.1 | v4 |
|-----------|------|----|
| עברית נכונה (Correct Hebrew) | 20% | 20% |
| קול ישראלי (Israeli voice) | 15% | 14% |
| אנטי-AI (Anti-detection) | 20% | 19% |
| פרוזודיה (Rhythm) | 12% | 11% |
| נשמה (Soul presence) | 8% | 7% |
| נשמה עמוקה (Deep soul) | 8% | 7% |
| קלון קולי (Voice matching) | 12% | 12% |
| מגוון (Versatility) — NEW | 0% | 10% |
| **Total** | **95%** | **100%** |

Note: 5% redistributed from existing dimensions to fund Versatility. The 95/100 threshold is unchanged in absolute points.

---

## File Changes Required for Implementation

| File | Change |
|------|--------|
| `SKILL-v3.1.md` (rename to `SKILL-v4.md`) | Add Layer 9 section; update What's New header; update argument parsing (add `--fresh`); update self-audit scoring table |
| `voice-profile-template-v3.1.md` (new v4 version) | Add variation-log reference; update version field to 4 |
| `.claude/voices/{profile}/variation-log.json` | New file; created on first use |

---

## What v4 Does NOT Change

- Layers 1-8 instructions (unchanged)
- The 42-feature extraction system
- The Key Tells extraction methodology
- The --setup / --setup-deep / --calibrate flows
- The Big No-Nos (macro copy, slide structure, LinkedIn punchline syndrome)
- The AI vocabulary blacklist (16 words)
- The em dash ban
- The 95/100 self-audit threshold (expressed in absolute points)
- GitHub public repo content (v4 is additive; prior versions remain)
