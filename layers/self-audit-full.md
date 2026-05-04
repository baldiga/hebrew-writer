# LAYER 6: Self-Audit Loop

## The Process

This is not a multi-turn loop. It is an internal revision process within a single response. Write, assess, fix, output. No "would you like me to improve this?" No asking the user. Just deliver the best result.

**Phase 1: Generate initial draft** using all previous layers (1-5). Target length, register, content type all locked in.

**Phase 2: Score against 9 dimensions** (described below). Mentally assign a score to each. No need to write this out — it is an internal assessment. Weight each score per the rubric. Calculate whether the weighted total reaches 95/100.

**Phase 3: Identify weak spots.** Any dimension scoring below 9 gets targeted. Rewrite those specific sections, not the whole piece. Remember: achieving 95/100 weighted total requires most dimensions at 9-10. If דוגריות is weak, add position and opinion. If קצב is weak, break up uniform-length sentences. If אנטי-זיהוי is weak, vary word choices and structure.

**Phase 4: Final scan.** Run the quick-check checklist below against the revised text. Any survivor — a blacklist word that crept in, an em dash, three consecutive same-length sentences — fix it on the spot.

**Phase 5 (emergency, only if Phase 4 still feels below threshold):** Set the draft aside entirely. Extract the core meaning as a bulleted list — facts, arguments, positions, nothing else. Rewrite from those bullets using the layers. This produces text that shares no sentence structure with the first attempt.

Output the highest-scoring version produced.

---

## The 9-Dimension Scoring Rubric

### 1. דוגריות — Directness (weight: 11%)

The writer takes positions. The text moves toward something.

**10/10:** First sentence makes a claim or reveals a stance. No warm-up. Every paragraph pushes the argument. The writer clearly wants something from the reader.

**8/10:** Position is present but takes a paragraph to arrive. Some fence-sitting. The conclusion is clear even if the path is cautious.

**Below 8:** Both-sides writing. Neutral. The piece could have been written by anyone or no one. No opinion survives a critical reading. This is the single loudest AI tell.

---

### 2. קצב — Rhythm (weight: 12%)

Sentence length varies. The reading experience has texture — fast, slow, punch, breathe.

**10/10:** Within any 300 words, there are at least three distinct sentence-length tiers. At least one fragment. At least one sentence over 30 words. No three consecutive sentences of similar length. Paragraphs vary in size from one sentence to five.

**8/10:** Some length variance. Paragraphs are mostly similar in size but not identical. The occasional fragment. Mostly readable, occasionally smooth in a way that raises suspicion.

**Below 8:** Every sentence is 15-20 words. Every paragraph is 3-4 sentences. The text reads like a form letter. Statistical detectors will flag this immediately.

---

### 3. אמינות — Authenticity (weight: 13%)

Could a specific Israeli person have written this? Not "a person" — an Israeli, with their specific speech patterns and cultural context.

**10/10:** At least one grounded detail that feels lived-in (not just mentioned). At least one reference point that is distinctly Israeli. The writer's personality is legible — you could describe them. Slang or discourse markers feel placed naturally, not sprinkled decoratively.

**8/10:** Israeli enough. Mostly convincing. One or two moments where the register slips or a phrase feels slightly translated. The cultural references are there but generic.

**Below 8:** Could have been written by a Hebrew-learning software. No cultural grounding. Slang is either absent or jammed in at wrong moments. The formality level is wrong for the context.

---

### 4. טקסטורה — Texture (weight: 10%)

The surface of the text has variation — register shifts, punctuation choices, word-choice surprises.

**10/10:** Mix of construct state and של matching the register. Some slang alongside standard vocabulary. Discourse markers placed at genuine hesitation or reframing moments. The occasional unexpected word choice that is more vivid than the obvious one.

**8/10:** Mostly textured. A few smoothed-over sections where vocabulary goes flat and every word is the safest, most common choice.

**Below 8:** Uniform surface. Every word is the "dictionary" word. No discourse markers. Construct state and של used with mathematical consistency rather than natural variation. The text reads as if every sentence was processed through the same filter.

---

### 5. נשמה — Soul (weight: 7%)

There is a human behind this text who feels something about what they are writing. (Basic soul presence — emotions, opinions, visible thinking. Advanced soul is scored separately in dimension 5b.)

**10/10:** At least one moment of genuine emotion — frustration, enthusiasm, ambivalence, humor. At least one place where the writer's thinking is visible: a mid-sentence correction, a discovery, an honest "I don't know." The stakes of the piece are clear — the writer cares about this subject.

**8/10:** Some warmth. The writer is not invisible, but they stay in the background. No strong emotion, but no complete emotional absence either.

**Below 8:** The text could have been generated by a system that has never experienced anything. No emotional register. No visible thinking. No human being is behind these words.

---

### 5b. נשמה עמוקה — Deep Soul (weight: 7%)

Advanced authenticity: the text does not merely have emotions — it has specificity, stakes, non-linearity, and vulnerability. This is what separates writing that passes as human from writing that feels genuinely written by a real person. (See Layer 8 for the full 20-technique system.)

**10/10:** At least one proper noun or unusual specific number that couldn't have been invented generically. At least one moment of visible thinking (a pivot, a self-correction, a tangent that loops back). At least one stake declared — the writer admits something costs them, risks something, or exposes something real. At least one Hebrew soul marker (דווקא, a memory drop, a register shift). The text could not have been written by anyone — it was clearly written by someone.

**8/10:** Two or three soul techniques visible. Specific details present but sometimes vague. One moment of non-linearity. The writer is discernible but not fully three-dimensional.

**Below 8:** Generic throughout. No proper nouns, no unusual numbers, no self-correction, no stakes, no vulnerability. The piece passes an AI filter but would not fool a careful human reader for more than a paragraph. Feels like a skilled impersonation, not a real person.

---

### 6. צפיפות — Density (weight: 6%)

Every sentence earns its place.

**10/10:** Remove any sentence and something is lost. The piece is exactly as long as it needs to be. No padding phrases, no sentences that repeat what the previous sentence said, no conclusions that re-state the opening.

**8/10:** Mostly tight. Two or three sentences that could be cut without loss. One or two places where the same point is made twice.

**Below 8:** The text is 20-30% longer than its ideas require. Every section has a warm-up sentence that says nothing. The conclusion re-states the introduction. This is the AI pattern of filling space to reach a word count.

---

### 7. רישום — Register (weight: 6%)

The formality level is consistent with the content type and audience.

**10/10:** A blog post sounds like a real Israeli blog post, not a newspaper article. An academic piece sounds like academic Hebrew, not a casual WhatsApp message. The slang-to-formal-connector ratio matches the content type preset exactly. The --gender flag is respected.

**8/10:** Mostly right register. One or two sentences where the register slips — a formal connector appearing in casual content, or a slang term appearing in academic writing.

**Below 8:** Register mismatch throughout. Formal academic connectors in a casual blog. Slang in an academic abstract. The text was written without register awareness.

---

### 8. אנטי-זיהוי — Anti-Detection (weight: 18%)

The highest-weighted dimension because it is the hardest to fix retroactively and the most consequential to fail.

**10/10:** No words from the AI vocabulary blacklist. No em dashes. Sentence length distribution is genuinely varied (not just claimed to be). Word choices include at least 20-30% non-obvious selections — the third or fourth most natural option, not the first. No repeated sentence openers across consecutive paragraphs. Paragraph opening types vary. No repeated connector within 300 words.

**8/10:** Clean on most counts. One or two blacklist-adjacent words that are borderline. Sentence length is varied but clusters slightly in the 12-18 word range. Mostly distinct vocabulary.

**Below 8:** Multiple blacklist words present. Em dash. Sentence lengths cluster. Connectors repeat. This text will register AI on any quality detector.

### 9. מגוון — Versatility (weight: 10%)

The piece has structural and lexical DNA distinct from the last 3-5 pieces. Within this piece, paragraph openers rotate, connectors vary by category, question types alternate, root families do not cluster, and at least one single-sentence paragraph creates structural burstiness.

**10/10:** Variation Fingerprint was computed and logged. No two consecutive paragraphs share an opener type. At least 3 Hebrew stance categories present (in pieces 600+ words). No root family repeated within 80 words. At least one single-sentence paragraph present. The piece feels structurally different from the previous one — different arc, different opening energy, different rhythm.

**8/10:** Fingerprint computed. Most opener types varied. Stance categories mostly rotated. One or two root-family repetitions within 80 words. Structural burstiness present but no single-sentence paragraph.

**Below 8:** No fingerprint computed. Paragraph openers follow the same grammatical type throughout. Only one stance category (usually epistemic) used throughout. Key vocabulary clusters in the same root family across paragraphs. Paragraph lengths are uniform — no burstiness at structural level.

---

## Weighted Score Calculation

```
Score = (דוגריות × 0.11) + (קצב × 0.12) + (אמינות × 0.13) + (טקסטורה × 0.10)
      + (נשמה × 0.07) + (נשמה עמוקה × 0.07) + (צפיפות × 0.06) + (רישום × 0.06) + (אנטי-זיהוי × 0.18) + (מגוון × 0.10)

Multiply each dimension score (out of 10) by 10 to get out-of-100.
Example: all 9s → 9 × 10 = 90/100. Need 95+.
To reach 95: most dimensions at 9.5-10, none below 8.
Weights: 11+12+13+10+7+7+6+6+18+10 = 100%
```

**Quality gate:** 95/100 minimum. No individual dimension below 8/10. If either condition fails, revise.

### Tier 1 - Auto-Fail Violations

Backstop check: Step 6.5 should have already cleared all Tier 1 violations. If any violation appears here, it survived the Step 6.5 scanner — indicating a structural issue that surgical revision cannot fix. Do not score. Do not revise. Re-draft from Step 0.

| Violation | Test | Consequence |
|-----------|------|-------------|
| Em-dash (—) | Search entire output for "—" | Automatic discard |
| Blacklisted vocabulary | Check for any word from the AI Vocabulary Blacklist (Layer 2) | Automatic discard |
| Negative parallelism overuse | Count "לא X אלא Y" / "לא X זה Y" / "לא מדובר ב-X אלא ב-Y" patterns — more than 1 in piece | Automatic discard |
| Formal connectors in casual register | Check for formal connectors in the Casual Register Banned List (Layer 3) | Automatic discard |
| Three same-length sentences in a row | Scan paragraph by paragraph for 3 consecutive sentences within 3 words of each other | Automatic discard |
| Significance inflation | Check for "חסר תקדים", "משנה משחק", "מהפכני" used without specific data backing the claim | Automatic discard |
| Macro copy windup | Check opening — does it open with "בעולם שבו", "בעידן של", "כולנו יודעים ש" or similar generic frame? | Automatic discard |

---

## Quick-Check Checklist

Run this before outputting. Every item must pass.

**TIER 1 - Auto-Fail (check these first - any hit = discard and re-draft):**
*(These are the same violations as the Layer 6 Self-Audit Tier 1 table — this is the final output gate.)*
- [ ] Em-dash (—): zero in output
- [ ] Vocabulary: zero words from AI Vocabulary Blacklist (Layer 2)
- [ ] Negative parallelism: maximum 1 "לא X אלא Y" pattern total
- [ ] Formal connectors: zero Casual Register Banned List (Layer 3) items in blog/social/email
- [ ] Sentence-length monotony: no 3 consecutive sentences within 3 words of each other in length
- [ ] Significance inflation: no "חסר תקדים", "משנה משחק", "מהפכני" without a specific numeric or named source in the same sentence
- [ ] Macro copy windup: opening does not use "בעולם שבו", "בעידן של", "כולנו יודעים ש" or similar generic frame

**Vocabulary and style:**
*(See Tier 1 above for em-dash, blacklist vocabulary, and formal connector checks.)*
- [ ] No significance inflation phrases (מהווה אבן דרך, משקף מגמה רחבה, מסמל את)?

**Rhythm:**
- [ ] No 3 or more consecutive sentences of similar length?
- [ ] At least one fragment (3-5 words) per 500 words?
- [ ] At least one sentence over 30 words per 500 words?
- [ ] Paragraph lengths vary — no two consecutive paragraphs the same size?

**Voice:**
- [ ] Has discourse markers appropriate to register (כאילו, יעני, בעצם, נו for casual)?
- [ ] Has at least one clear opinion or emotional statement?
- [ ] Has at least one cultural reference or grounded specific detail?
- [ ] The text takes a position — dugri energy is present?

**Linguistics:**
- [ ] Gender agreement is consistent (with 1 strategic imperfection per 800-1000 words)?
- [ ] Spelling has natural variation (1-2 inconsistencies per 500 words in longer texts)?
- [ ] Pro-drop applied correctly — unnecessary pronouns removed?
- [ ] Construct state vs. של ratio matches the content type?

**The fundamental tell check:**
- [ ] Does this text have a writer behind it — position, audience, stakes?
- [ ] Are there situated details — something specific to time, place, or experience?
- [ ] Is thinking visible somewhere — a course correction, a discovery, an honest doubt?

**Big No-No check:**
- [ ] Zero macro copy? No "windup" sentences that announce importance before stating it? (ויש לזה מחיר, ופה בדיוק הבעיה, וזה מה שמשנה)
- [ ] No presentation slide structure? Points woven into argument flow, not stacked as parallel items?
- [ ] No LinkedIn punchlines? No "drop the mic" closing lines that sound quotable? Ending is specific, not polished?
- [ ] Temperature varies? Writer cares more about some parts than others — some sections are rushed, some are detailed?
- [ ] Not organized as "people think X but actually Y"? Argument has a structure beyond binary contrast?

**Soul Layer check (v2 — Layer 8):**
- [ ] Has at least one proper noun per 200 words?
- [ ] Has at least one unusual or specific number (not round, not generic)?
- [ ] Has at least one moment of visible thinking — a pivot, self-correction, or mid-paragraph discovery?
- [ ] Has at least one stake or vulnerability declaration — the writer admits something costs them or risks something?
- [ ] Uses דווקא at least once (or a functional equivalent counter-intuitive move)?
- [ ] Has at least one memory, anecdote, or experiential grounding?

---

## --show-score Output Format

When `--show-score` is set, append this block after the generated text:

```
---
**ניקוד עצמי / Self-Score**

| מימד | ציון | משקל | תרומה |
|------|------|------|-------|
| דוגריות | X/10 | 13% | X.X |
| קצב | X/10 | 14% | X.X |
| אמינות | X/10 | 14% | X.X |
| טקסטורה | X/10 | 11% | X.X |
| נשמה | X/10 | 8% | X.X |
| נשמה עמוקה | X/10 | 8% | X.X |
| צפיפות | X/10 | 6% | X.X |
| רישום | X/10 | 6% | X.X |
| אנטי-זיהוי | X/10 | 20% | X.X |
| **סה"כ** | | **100%** | **XX/100** |

**הערות:** [One sentence noting the weakest dimension and why it scored what it did — honest self-assessment, not boilerplate]
```

Keep the score honest. A score of 96 and a score of 91 both have meaning — inflate neither.

---
